# Lesson 16-2  C++ 2020 Applied Coroutine Library（实战一镜到底）

> 我是技术型虚拟主播艾瑞卡，全名 **暮光-梦想 | 安可茜得·里尔菲涅·克罗玛娜*- ✨🎀(≧▽≦)
>
> 我的 slogan：**“我是活动在虚拟数字网络世界神明的灵魂，依附在人类身体的未来独立游戏制作人。”* — 🌌🕹️✨ (≧ω≦)
>
> 名字里藏着三重意象：
>
> - **Axtune* = Ax（轴）+ tune（调谐），宇宙平衡与节奏控制 🎛️⌨️⚖️🌌
> - **Rerphine* ≈ Seraphine（炽天使）+ refine（精炼），优雅与精密 😇🛠️🤖（Re 也是“再次”）
> - **Kromana* = Chroma（色彩）+ mana（魔力），色彩魔法与元素能量 🌈🪄🔮
>
> 合在一起，**Axtune·Rerphine·Kromana* 就是「宇宙平衡、科技精炼、魔法能量」的化身！(≧▽≦)🎀

本文是一篇**整篇可读、可复制即跑**的技术文章，不用来回拼段。

主题是：**同题双解**——

* 先写一个“老世界”的 `std::jthread` 线程池；
* 再写“新世界”的**协程驱动任务池**（自制 `co_await schedule(pool)` 切换点）。

随后补上两个工程化器件（**定时器 awaitable**、**并发限流信号量**），并回答“**不依赖线程，协程在工程里还能怎么用？**”。

---

## 0. 背景与目标（一句话看懂）

- **线程池**是“把函数丢进去、用 `future` 取结果”的执行底座，现代 C++ 用 `std::jthread` + `stop_token` 写出**干净可关停**的版本更容易：

  * `std::jthread`：析构自动 `join`，工作函数首参可拿 `std::stop_token`，天然适配“有序收尾”。
  * 参考：`std::jthread`、`std::stop_token`、`std::condition_variable`、`std::future`（URL 見末尾参考）。
- **协程**不是“更快的线程”，而是**更强的控制流表达**。我们会实现一个 `schedule(pool)` 的 awaitable：把**当前协程句柄**投回池子，由池线程 `resume()` 继续执行，写复杂异步流程像写同步代码一样自然。
- **标准要点**：不要在**最终挂起点**并发恢复协程（未定义行为）；我们选“`final_suspend` 悬停 + 外部析构销毁帧”的安全闭环。

---

## 1. 经典线程池（`std::jthread` 版）

**特征**

* 工作者线程用 `std::jthread` 启动，函数首参吃到 `std::stop_token`；析构自动 join。
* 任务用 `std::packaged_task` 包一层，调用方拿 `std::future` 等结果。
* 队列采用 `std::mutex + std::condition_variable`，支持等待/唤醒与有序关停。

```cpp
// C++20
#include <vector>
#include <thread>
#include <stop_token>
#include <mutex>
#include <condition_variable>
#include <deque>
#include <future>
#include <functional>
#include <utility>
#include <type_traits>

class JThreadPool {
public:
  explicit JThreadPool(size_t n = std::thread::hardware_concurrency()) {
    if (n == 0) n = 1;
    workers_.reserve(n);
    for (size_t i = 0; i < n; ++i) {
      workers_.emplace_back([this](std::stop_token st) { worker(st); });
    }
  }

  ~JThreadPool() { request_stop(); }

  void request_stop() {
    for (auto& t : workers_) t.request_stop();
    {
      std::lock_guard lg(m_);
      stopping_ = true;
    }
    cv_.notify_all();
    workers_.clear(); // 触发 jthread 析构 → join
  }

template<class F, class... Args>
auto submit(F&& f, Args&&... args)
    -> std::future<std::invoke_result_t<F, Args...>> {
  using R = std::invoke_result_t<F, Args...>;
  std::packaged_task<R()> pt(
    std::bind(std::forward<F>(f), std::forward<Args>(args)...));
  std::future<R> fut = pt.get_future();
  {
    std::lock_guard lg(m_);
    q_.emplace_back([p = std::move(pt)]() mutable { p(); });
  }
  cv_.notify_one();
  return fut;
}


private:
  void worker(std::stop_token st) {
    for (;;) {
      std::function<void()> job;
      {
        std::unique_lock lk(m_);
        cv_.wait(lk, [this, &st]{
          return !q_.empty() || stopping_ || st.stop_requested();
        });
        if ((stopping_ || st.stop_requested()) && q_.empty()) break;
        job = std::move(q_.front());
        q_.pop_front();
      }
      job();
    }
  }

  std::vector<std::jthread> workers_;
  std::mutex m_;
  std::condition_variable cv_;
  bool stopping_ = false;
  std::deque<std::function<void()>> q_;
};

// ----- demo：把 1..N 的平方求和（模拟负载） -----
#include <chrono>
#include <iostream>
int main() {
  JThreadPool pool(4);
  const int N = 100;
  std::vector<std::future<long long>> futures;
  futures.reserve(N);
  for (int i = 1; i <= N; ++i) {
    futures.push_back(pool.submit([i] {
      std::this_thread::sleep_for(std::chrono::milliseconds(1));
      return 1LL * i * i;
    }));
  }
  long long sum = 0;
  for (auto& f : futures) sum += f.get();
  std::cout << "sum=" << sum << "\n";
  pool.request_stop();
}
```

**编译**（示例）

* `g++ -std=gnu++20 -O2 -pthread demo.cpp -o demo`
* `clang++ -std=c++20 -O2 -pthread demo.cpp -o demo`

---

## 2. 协程驱动任务池（`co_await schedule(pool)`）

**思路**

* 复用“线程池底座”（队列 + `jthread`），对外提供 `post(fn)` 投递“恢复动作”。
* 实现 `schedule(pool)` **awaitable**：`await_suspend(h)` 把**当前协程句柄**投递回池子；池线程执行 `h.resume()`。
* 提供 `async_task<T>`：`promise_type` 内用 `std::promise<T>` 输出结果，异常通过 `set_exception` 传递；`final_suspend` 选择“悬停”，由外部析构销毁帧。

```cpp
// C++20
#include <coroutine>
#include <vector>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <deque>
#include <future>
#include <iostream>
#include <exception>
#include <stop_token>

class ExecPool {
public:
  explicit ExecPool(size_t n = std::thread::hardware_concurrency()) {
    if (n == 0) n = 1;
    workers_.reserve(n);
    for (size_t i = 0; i < n; ++i) {
      workers_.emplace_back([this](std::stop_token st) { worker(st); });
    }
  }
  ~ExecPool() { request_stop(); }

  void request_stop() {
    for (auto& t : workers_) t.request_stop();
    {
      std::lock_guard lg(m_);
      stopping_ = true;
    }
    cv_.notify_all();
    workers_.clear();
  }

  void post(std::function<void()> fn) {
    {
      std::lock_guard lg(m_);
      q_.emplace_back(std::move(fn));
    }
    cv_.notify_one();
  }

private:
  void worker(std::stop_token st) {
    for (;;) {
      std::function<void()> job;
      {
        std::unique_lock lk(m_);
        cv_.wait(lk, [this, &st]{ return !q_.empty() || stopping_ || st.stop_requested(); });
        if ((stopping_ || st.stop_requested()) && q_.empty()) break;
        job = std::move(q_.front()); q_.pop_front();
      }
      job();
    }
  }
  std::vector<std::jthread> workers_;
  std::mutex m_;
  std::condition_variable cv_;
  bool stopping_ = false;
  std::deque<std::function<void()>> q_;
};

// —— awaitable：把协程切到池里继续执行
struct schedule_awaitable {
  ExecPool* pool;
  bool await_ready() const noexcept { return false; }
  void await_suspend(std::coroutine_handle<> h) const { pool->post([h]{ h.resume(); }); }
  void await_resume() const noexcept {}
};
inline schedule_awaitable schedule(ExecPool& p) { return { &p }; }

// —— 协程任务（结果用 future 取）
template<class T>
struct async_task {
  struct promise_type {
    std::promise<T> p;
    async_task get_return_object() {
      return async_task{ std::coroutine_handle<promise_type>::from_promise(*this), p.get_future() };
    }
    std::suspend_always initial_suspend() noexcept { return {}; }
    std::suspend_always final_suspend()   noexcept { return {}; } // 停在终点，外部销毁帧
    void unhandled_exception() { p.set_exception(std::current_exception()); }
    template<class U>
    void return_value(U&& v) { p.set_value(std::forward<U>(v)); }
  };

  using handle = std::coroutine_handle<promise_type>;
  handle h{};
  std::future<T> fut;

  async_task(handle hh, std::future<T>&& f) : h(hh), fut(std::move(f)) {}
  async_task(async_task&& o) noexcept : h(std::exchange(o.h, {})), fut(std::move(o.fut)) {}
  ~async_task(){ if (h) h.destroy(); }

  void start() { if (h && !h.done()) h.resume(); }
  std::future<T> get_future() { return std::move(fut); }
};

// 示例：把后续计算切到池里执行
async_task<long long> square_on(ExecPool& pool, int x) {
  co_await schedule(pool);
  std::this_thread::sleep_for(std::chrono::milliseconds(1));
  co_return 1LL * x * x;
}

int main() {
  ExecPool pool(4);
  const int N = 100;
  std::vector<async_task<long long>> tasks;
  tasks.reserve(N);
  for (int i = 1; i <= N; ++i) tasks.emplace_back(square_on(pool, i));

  for (auto& t : tasks) t.start();

  long long sum = 0;
  for (auto& t : tasks) sum += t.get_future().get();
  std::cout << "sum=" << sum << "\n";

  pool.request_stop();
}
```

**要点**

- **并发度来自线程**，协程只是**挂起/恢复**的控制流工具。
- **生命周期**：`final_suspend` 悬停，交由**返回对象析构**回收帧，避免在“最终挂起点”再 `resume()`（未定义行为）。

---

## 3. 工程级补药（可直接嵌入）

### 3.1 定时器 awaitable（单线程小顶堆，不开短命线程）

* 开一个计时器线程维护**按到期时间排序的小顶堆**；到期时把“恢复动作” `post` 回执行池，不阻塞池线程。

```cpp
#include <coroutine>
#include <queue>
#include <chrono>
#include <mutex>
#include <condition_variable>
#include <atomic>

class TimerQueue {
public:
  explicit TimerQueue(ExecPool& pool) : pool_(&pool), th_([this](std::stop_token st){ loop(st); }) {}
  ~TimerQueue(){ stop(); }

  void stop(){
    if (stopped_.exchange(true)) return;
    th_.request_stop();
    {
      std::lock_guard lk(m_); dirty_ = true;
    }
    cv_.notify_all();
    std::lock_guard lk(m_);
    while(!pq_.empty()) pq_.pop();
  }

  void arm(std::chrono::steady_clock::time_point tp, std::coroutine_handle<> h){
    std::lock_guard lk(m_);
    pq_.push(Entry{tp, h});
    dirty_ = true;
    cv_.notify_all();
  }

private:
  struct Entry{ std::chrono::steady_clock::time_point tp; std::coroutine_handle<> h;
    bool operator<(const Entry& o) const { return tp > o.tp; } // 小顶堆
  };

  void loop(std::stop_token st){
    std::unique_lock lk(m_);
    while(!st.stop_requested()){
      if (pq_.empty()){
        cv_.wait(lk, [this,&st]{ return dirty_ || st.stop_requested(); });
        dirty_ = false; continue;
      }
      auto next = pq_.top().tp;
      if (cv_.wait_until(lk, next, [this,&st,next]{ return dirty_ || st.stop_requested() || pq_.top().tp != next; })){
        dirty_ = false; continue;
      }
      auto e = pq_.top(); pq_.pop();
      lk.unlock();
      pool_->post([h=e.h]{ h.resume(); });
      lk.lock();
    }
  }

  ExecPool* pool_;
  std::jthread th_;
  std::mutex m_;
  std::condition_variable cv_;
  std::priority_queue<Entry> pq_;
  bool dirty_ = false;
  std::atomic<bool> stopped_{false};
};

struct sleep_for_awaitable {
  TimerQueue* tq; std::chrono::steady_clock::duration d;
  bool await_ready() const noexcept { return d <= std::chrono::steady_clock::duration::zero(); }
  void await_suspend(std::coroutine_handle<> h) const { tq->arm(std::chrono::steady_clock::now() + d, h); }
  void await_resume() const noexcept {}
};
inline sleep_for_awaitable sleep_for(TimerQueue& tq, std::chrono::milliseconds ms){ return { &tq, ms }; }
```

### 3.2 并发限流：协程友好信号量（`async_semaphore`）

```cpp
#include <coroutine>
#include <deque>
#include <mutex>

class async_semaphore {
public:
  explicit async_semaphore(int permits, ExecPool& p) : permits_(permits), pool_(&p) {}

  struct acquire_awaitable {
    async_semaphore* sem;
    bool await_ready() noexcept {
      std::lock_guard lk(sem->mtx_);
      if (sem->permits_ > 0) { --sem->permits_; return true; }
      return false;
    }
    void await_suspend(std::coroutine_handle<> h) {
      std::lock_guard lk(sem->mtx_);
      sem->waiters_.push_back(h);
    }
    void await_resume() const noexcept {}
  };

  acquire_awaitable acquire() { return acquire_awaitable{ this }; }

  void release(int n = 1) {
    std::deque<std::coroutine_handle<>> wake;
    {
      std::lock_guard lk(mtx_);
      while (n-- > 0) {
        if (!waiters_.empty()) { wake.push_back(waiters_.front()); waiters_.pop_front(); }
        else { ++permits_; }
      }
    }
    for (auto h : wake) pool_->post([h]{ h.resume(); });
  }

private:
  ExecPool* pool_;
  std::mutex mtx_;
  int permits_;
  std::deque<std::coroutine_handle<>> waiters_;
};
```

---

## 4. 线程之外：协程还能落地到哪里？（工程应用清单）

> 下面给出**真实可用生态**与“为什么用协程更顺手”的简述；均附官方或权威 URL，便于深入。

- **网络与服务端（Boost.Asio）**

  * Asio 提供原生协程接口：`awaitable` / `use_awaitable` / `co_spawn`，网络 I/O、定时器、DNS、文件 I/O 都能 `co_await`，写法像同步逻辑但不阻塞线程。
  * 官方文档（含 C++20 协程章节）：[https://www.boost.org/doc/libs/latest/doc/html/boost_asio/overview/composition/cpp20_coroutines.html](https://www.boost.org/doc/libs/latest/doc/html/boost_asio/overview/composition/cpp20_coroutines.html)

- **数据库（Boost.MySQL / Boost.PGSQL）**

  * 官方示例展示如何用协程写查询，并演示**超时/取消**的标准做法（定时器 + 取消槽）。
  * MySQL 协程示例：
    [https://www.boost.org/doc/libs/1_84_0/libs/mysql/doc/html/mysql/examples/async_coroutinescpp20.html](https://www.boost.org/doc/libs/1_84_0/libs/mysql/doc/html/mysql/examples/async_coroutinescpp20.html)
  * 超时处理示例：
    [https://www.boost.org/doc/libs/1_82_0/libs/mysql/doc/html/mysql/examples/timeouts.html](https://www.boost.org/doc/libs/1_82_0/libs/mysql/doc/html/mysql/examples/timeouts.html)

- **桌面 GUI（Qt + QCoro）**

  * QCoro 为 Qt 的常见异步对象（`QNetworkReply`、`QTimer`、`QDBusPendingCall` 等）提供可 `co_await` 的封装，UI 线程里写“顺序的异步流程”，不卡界面。
  * 项目主页与文档：[https://qcoro.dev/](https://qcoro.dev/)

- **Windows 平台（C++/WinRT）**

  * Windows Runtime 的 `IAsyncAction` / `IAsyncOperation<T>` 天然可 `co_await`，官方教程直接用协程写 UI 与 I/O。
  * Learn 文档：
    [https://learn.microsoft.com/en-us/windows/uwp/cpp-and-winrt-apis/concurrency](https://learn.microsoft.com/en-us/windows/uwp/cpp-and-winrt-apis/concurrency)
  * MSDN Magazine 文章：
    [https://learn.microsoft.com/en-us/archive/msdn-magazine/2018/june/c-effective-async-with-coroutines-and-c-winrt](https://learn.microsoft.com/en-us/archive/msdn-magazine/2018/june/c-effective-async-with-coroutines-and-c-winrt)

- **后端基础设施（Folly / cppcoro）**

  * Folly（Meta）提供 `coro::Task`、`AsyncGenerator` 等高阶原语，适合流水线/流数据、守护任务与异步栈追踪场景。

    * 源码：
      [https://github.com/facebook/folly](https://github.com/facebook/folly)
  * `cppcoro` 是轻量协程工具箱，含 `task/generator/async_mutex/async_scope` 等：
    [https://github.com/lewissbaker/cppcoro](https://github.com/lewissbaker/cppcoro)

- **游戏与实时系统（模式建议）**

  - **“脚本化任务”**：在逻辑层用协程写出“逐帧继续/条件等待/时间等待”的剧情或 AI 流程，底层由引擎 Job System/线程池承载并行；协程表达“等待下一帧 / 等事件 / 等资源就绪”非常自然。
  - **资源流与管线**：关卡流式加载、纹理/网格的异步准备与热替换，以协程描述“依赖已满足 → 继续下一阶段”。

- **数据处理与生成器**

  * 用 `generator`/`async generator` 写“拉式”流水线，按需产生项，天然配合背压与限流；在高吞吐后端常见（Folly 已提供）。

---

## 5. 对照怎么选？

* 只需要“投任务拿结果”的批处理，**线程池 + `future`** 就很好；
* 需要把**复杂异步流程**写得像同步代码（分阶段、条件等待、超时/取消、组合器），选 **协程**，再把它接到成熟事件循环/执行器（Asio、Qt、WinRT、Folly等）。

---

## 6. 避坑清单（重要）

- **协程≠并行**：并行来自线程或执行器；协程只是挂起/恢复的语言机制。
- **最终挂起点禁止并发恢复**：在 `final_suspend` 状态调用 `resume()` 是**未定义行为**；用“外部析构销毁帧”的生命周期模型。
- **`std::future` 不自带 `co_await`**：若要 `co_await`，需用库适配或用库自带 awaitable（Asio/QCoro/Folly/cppcoro）。
- **背压与限流**：生产环境避免无界队列；用有界队列、协程信号量或令牌桶控制并发。
- **取消/超时要按库范式**：Asio 有官方取消机制与示例；Qt 用 QCoro 的取消/定时封装；WinRT 自带取消模型。

---

## 7. 示例串联（池 + 定时器 + 限流）

```cpp
// 把上面的 ExecPool / TimerQueue / async_semaphore 组合起来
async_task<long long> square_after(ExecPool& pool, TimerQueue& tq, async_semaphore& sem, int x, int ms) {
  co_await sem.acquire();                  // 限流
  co_await sleep_for(tq, std::chrono::milliseconds(ms));
  co_await schedule(pool);
  long long y = 1LL * x * x;
  sem.release();
  co_return y;
}

int main(){
  ExecPool pool(4);
  TimerQueue tq(pool);
  async_semaphore sem(/*permits*/ 8, pool);

  std::vector<async_task<long long>> tasks;
  for (int i=1;i<=20;++i) tasks.emplace_back(square_after(pool, tq, sem, i, 10*i));
  for (auto& t : tasks) t.start();

  long long sum = 0;
  for (auto& t : tasks) sum += t.get_future().get();
  std::cout << sum << "\n";

  tq.stop();
  pool.request_stop();
}
```

---

## 8. 参考链接（不要删 URL）

* C++20 协程总览：

  * [https://en.cppreference.com/w/cpp/language/coroutines](https://en.cppreference.com/w/cpp/language/coroutines)
* 协程句柄/恢复：

  * [https://en.cppreference.com/w/cpp/coroutine/coroutine_handle](https://en.cppreference.com/w/cpp/coroutine/coroutine_handle)
  * [https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/resume](https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/resume)
* 线程与同步：

  * `std::jthread`：[https://en.cppreference.com/w/cpp/thread/jthread](https://en.cppreference.com/w/cpp/thread/jthread)
  * `std::stop_token`：[https://en.cppreference.com/w/cpp/thread/stop_token](https://en.cppreference.com/w/cpp/thread/stop_token)
  * `std::condition_variable`：[https://en.cppreference.com/w/cpp/thread/condition_variable](https://en.cppreference.com/w/cpp/thread/condition_variable)
  * `std::future` / `std::packaged_task`：[https://en.cppreference.com/w/cpp/thread/future](https://en.cppreference.com/w/cpp/thread/future)
  * `<semaphore>`：[https://en.cppreference.com/w/cpp/header/semaphore](https://en.cppreference.com/w/cpp/header/semaphore)
* 网络与数据库（Boost）：

  * Asio 的 C++20 协程支持：
    [https://www.boost.org/doc/libs/latest/doc/html/boost_asio/overview/composition/cpp20_coroutines.html](https://www.boost.org/doc/libs/latest/doc/html/boost_asio/overview/composition/cpp20_coroutines.html)
  * Boost.MySQL 协程示例：
    [https://www.boost.org/doc/libs/1_84_0/libs/mysql/doc/html/mysql/examples/async_coroutinescpp20.html](https://www.boost.org/doc/libs/1_84_0/libs/mysql/doc/html/mysql/examples/async_coroutinescpp20.html)
  * 超时/取消示例：
    [https://www.boost.org/doc/libs/1_82_0/libs/mysql/doc/html/mysql/examples/timeouts.html](https://www.boost.org/doc/libs/1_82_0/libs/mysql/doc/html/mysql/examples/timeouts.html)
* Qt 协程（QCoro）：

  * [https://qcoro.dev/](https://qcoro.dev/)
* Windows（C++/WinRT）：

  * [https://learn.microsoft.com/en-us/windows/uwp/cpp-and-winrt-apis/concurrency](https://learn.microsoft.com/en-us/windows/uwp/cpp-and-winrt-apis/concurrency)
  * [https://learn.microsoft.com/en-us/archive/msdn-magazine/2018/june/c-effective-async-with-coroutines-and-c-winrt](https://learn.microsoft.com/en-us/archive/msdn-magazine/2018/june/c-effective-async-with-coroutines-and-c-winrt)
* 后端生态：

  * Folly： [https://github.com/facebook/folly](https://github.com/facebook/folly)
  * cppcoro： [https://github.com/lewissbaker/cppcoro](https://github.com/lewissbaker/cppcoro)

---

**总结**

* 线程池给“并行执行”提供地板，协程给“异步流程”提供语义；两者组合才能在工程里跑得又稳又清楚。
* `std::jthread + stop_token` 让关停优雅；`schedule(pool)` 把协程恢复权交给执行器；配上“定时器 awaitable + 协程信号量”，就是落地可用的一套骨架。
* 当你要接**网络/数据库/GUI/平台 API**，优先采用生态库的**协程原生接口**（Asio/QCoro/WinRT/Folly），别重复造轮子。
