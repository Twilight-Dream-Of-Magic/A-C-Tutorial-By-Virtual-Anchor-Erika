# Lesson 16-1 C++ 2020 Coroutine Library

> 我是技术型虚拟主播艾瑞卡，全名 **暮光-梦想 | 安可茜得·里尔菲涅·克罗玛娜*- ✨🎀(≧▽≦)
>
> 我的 slogan：**“我是活动在虚拟数字网络世界神明的灵魂，依附在人类身体的未来独立游戏制作人。”*- 🌌🕹️✨ (≧ω≦)
>
> 名字里藏着三重意象：
>
> - **Axtune*- = Ax（轴）+ tune（调谐），宇宙平衡与节奏控制 🎛️⌨️⚖️🌌
> - **Rerphine*- ≈ Seraphine（炽天使）+ refine（精炼），优雅与精密 😇🛠️🤖（Re 也是“再次”）
> - **Kromana*- = Chroma（色彩）+ mana（魔力），色彩魔法与元素能量 🌈🪄🔮
>
> 合在一起，**Axtune·Rerphine·Kromana** 就是「宇宙平衡、科技精炼、魔法能量」的化身！(≧▽≦)🎀

哈喽，这一课把 C++20 协程讲成“能落地的脑内电影”。不聊天、不碎片，直接一镜到底：先把名词拍扁，再用**三个最小代码块**把 `co_await / co_yield / co_return` 各自的职责落到代码。最后，把“为什么要手动拼装一坨 struct/class”的来龙去脉讲清楚。

---

## 协程是什么（一句话硬核版）

只要函数体里出现 `co_await`、`co_yield`、或 `co_return` 之一，编译器就把它“升格”为**可挂起**的协程：生成一个**协程帧**（保存局部变量与暂停点），并以 **`promise_type` + `std::coroutine_handle`*- 驱动它的生命周期。协程可以在挂起点暂停，稍后从同一行继续。[C++ reference：Coroutines](https://en.cppreference.com/w/cpp/language/coroutines.html)

**三件套一句话**：

- `co_await e`：**可能挂起**；把控制权交还给调用者，等外部恢复后再从这行继续。
- `co_yield v`：**产出一个值就暂停**；语义等于调用 `promise.yield_value(v)` 后挂起。
- `co_return x`：**正常结束协程**；把结果交给 `promise.return_value(x)`（或 `return_void()`），再进入最终挂起点。

---

## ① `co_await`：最小“挂起-恢复”演示

```cpp
// C++20
#include <coroutine>
#include <iostream>

struct task {
  struct promise_type {
    task get_return_object() {
      return task{ std::coroutine_handle<promise_type>::from_promise(*this) };
    }
    std::suspend_always initial_suspend() noexcept { return {}; } // 先别跑
    std::suspend_always final_suspend()   noexcept { return {}; } // 结束时停住，等外部销毁
    void return_void() noexcept {}
    void unhandled_exception() { std::terminate(); }
  };
  using handle = std::coroutine_handle<promise_type>;
  explicit task(handle h):h(h) {}
  ~task(){ if(h) h.destroy(); }                    // RAII：销毁协程帧
  bool resume(){ if(!h || h.done()) return false; h.resume(); return !h.done(); }
private: handle h{};
};

struct always_suspend {
  bool await_ready() const noexcept { return false; }  // 一定挂起
  void await_suspend(std::coroutine_handle<>) const noexcept {
    std::cout << "  [await_suspend]\n";
  }
  void await_resume() const noexcept {
    std::cout << "  [await_resume]\n";
  }
};

task demo() {
  std::cout << "A\n";
  co_await always_suspend{};   // 第一次 resume() 会停在这里
  std::cout << "B\n";
}

int main() {
  auto t = demo();             // 还没执行（initial_suspend）
  std::cout << "start\n";
  t.resume();                  // 跑到 co_await -> await_suspend -> 挂起
  std::cout << "after first resume\n";
  t.resume();                  // 恢复：await_resume -> 打印 B -> final_suspend
  std::cout << "done\n";
}
```

**要点**：`co_await` 把控制权交还给调用者；`initial_suspend / final_suspend` 决定“创建时是否立刻跑”和“结束时是否等外部回收帧”。标准**没有**内置“调度器/线程池”。参考：[coroutine_handle](https://en.cppreference.com/w/cpp/coroutine/coroutine_handle.html)、[suspend_always](https://en.cppreference.com/w/cpp/coroutine/suspend_always.html)。

---

## ② `co_yield`：最小“逐个产出”演示（用 promise 收集）

```cpp
// C++20：演示 co_yield 的语义（yield_value + 暂停）
#include <coroutine>
#include <vector>
#include <iostream>

struct collector {
  struct promise_type {
    std::vector<int> out;
    collector get_return_object() {
      return collector{ std::coroutine_handle<promise_type>::from_promise(*this) };
    }
    std::suspend_always initial_suspend() noexcept { return {}; }
    std::suspend_always final_suspend()   noexcept { return {}; }
    // co_yield v 语义：调用 yield_value(v)
    std::suspend_always yield_value(int v) noexcept {
      out.push_back(v);
      return {}; // 产出后暂停一下
    }
    void return_void() noexcept {}
    void unhandled_exception() { std::terminate(); }
  };
  using handle = std::coroutine_handle<promise_type>;
  explicit collector(handle h):h(h){}
  ~collector(){ if(h) h.destroy(); }

  bool resume(){ if(!h || h.done()) return false; h.resume(); return !h.done(); }
  const std::vector<int>& values() const { return h.promise().out; }
private: handle h{};
};

collector seq() {
  co_yield 10;
  co_yield 20;
  co_yield 30;
}

int main() {
  auto c = seq();
  while (c.resume()) { /* 逐次产出并暂停 */ }
  for (int v : c.values()) std::cout << v << " "; // 10 20 30
}
```

**要点**：示例不实现迭代器，而是在 `promise.yield_value` 中**收集**产出。真正的**可迭代生成器**在 C++23 提供了 `std::generator`，此课聚焦语义本身。参考：[co_yield](https://en.cppreference.com/w/cpp/keyword/co_yield)、[<generator>（C++23）](https://en.cppreference.com/w/cpp/header/generator)。

---

## ③ `co_return`：最小“带返回值的任务”

```cpp
// C++20：task<int>，co_return 42
#include <coroutine>
#include <optional>
#include <iostream>

struct int_task {
  struct promise_type {
    std::optional<int> result;
    int_task get_return_object() {
      return int_task{ std::coroutine_handle<promise_type>::from_promise(*this) };
    }
    std::suspend_always initial_suspend() noexcept { return {}; }
    std::suspend_always final_suspend()   noexcept { return {}; }
    void return_value(int v) noexcept { result = v; } // co_return v -> 走这里
    void unhandled_exception() { std::terminate(); }
  };
  using handle = std::coroutine_handle<promise_type>;
  explicit int_task(handle h):h(h){}
  ~int_task(){ if(h) h.destroy(); }

  bool resume(){ if(!h || h.done()) return false; h.resume(); return !h.done(); }
  int get() const { return *h.promise().result; }
private: handle h{};
};

int_task calc() {
  co_return 42; // 等价于 promise.return_value(42)
}

int main() {
  auto t = calc();
  while (t.resume()) {}        // 跑到 final_suspend
  std::cout << t.get() << "\n"; // 42
}
```

**要点**：`co_return` 触发 `return_value()`（或 `return_void()`）并到**最终挂起点**。从协程尾直接“掉出函数体”的语义等于 `co_return;`，但若没提供 `return_void()`，就是 UB，不要这么做。参考：[co_return](https://en.cppreference.com/w/cpp/keyword/co_return)。

---

## 1 页心智图（把握住“谁做决定”）

- **返回对象**（你自定义的 `task`/`generator` 等）= 外部“把柄”，里面通常包着 `std::coroutine_handle<promise_type>`，提供 `resume()/done()/destroy()` 或更友好的接口。[coroutine_handle](https://en.cppreference.com/w/cpp/coroutine/coroutine_handle.html)
- **`promise_type` 决策中心**：
  `get_return_object()`（把柄长什么样）→ `initial_suspend/final_suspend`（启停策略）→ `return_value/return_void`（如何结束）→ `unhandled_exception`（异常如何传出）。[Coroutines 概览](https://en.cppreference.com/w/cpp/language/coroutines.html)
- **await 协议**：`co_await expr` → 找 **awaiter**（成员 `operator co_await`、ADL 非成员、或直接把 `expr` 当 awaiter）→ 走 `await_ready / await_suspend / await_resume` 三连。理解“为何挂起、如何恢复”的关键参考：[Lewis Baker：Understanding operator co_await](https://lewissbaker.github.io/2017/11/17/understanding-operator-co_await)

---

## “为啥要手动拼装一堆 struct/class？”

因为 C++20 刻意只把**语言机制**放进来——也就是“可挂起/可恢复”的语义和 `<coroutine>` 的**最低抽象**（`std::coroutine_handle`、`std::suspend_always/never` 等）。
**返回对象长什么样、结果/异常怎么交付、启动与回收策略如何选**，都交给你（或库作者）在 `promise_type` 与返回对象里定义。标准库本身**没**内置通用 I/O/任务类型供 `co_await` 使用（`std::future` 也不行）。

翻成人话：**语言只给你变速箱，车壳/方向/回收都让你自己定制**。这也是 C++ 的哲学：**零开销抽象**、**不绑定生态**、**分期交付**（C++23 才补上了 `std::generator` 这种“现成件”）。参考：

- [Coroutines 总览（cppreference）](https://en.cppreference.com/w/cpp/language/coroutines.html)
- [C++ Core Coroutines（WG21 设计稿）](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1063r0.pdf)
- [标准库 `<generator>`](https://en.cppreference.com/w/cpp/header/generator)

---

## 编译器究竟把协程“改写”为何物（把流程演成电影）

把“含协程关键字的函数”想象成被重写为这段伪代码（顺序和主流实现一致）：

```cpp
// 伪代码：协程创建与生命周期
{
  // 1) 建立“协程帧”：保存局部变量、暂停点、以及 promise
  auto& p = /- construct promise_type inside the frame */;

  // 2) 生成“返回对象”给调用者
  auto ret = p.get_return_object();

  try {
    // 3) 初始挂起：决定是否“懒启动”
    co_await p.initial_suspend(); // suspend_always => 懒；suspend_never => 立刻跑

    // 4) 执行主体；每遇到 co_await：
    //   - 按顺序找 awaiter（成员 -> ADL -> 自身）
    //   - if (!awaiter.await_ready())
    //         awaiter.await_suspend(handle) 交还控制权；
    //     else awaiter.await_resume() 直接继续
    body();

  } catch (...) {
    p.unhandled_exception();
  }

  // 5) 正常结束（或 co_return expr）
  //    => p.return_void() / p.return_value(expr)
  co_await p.final_suspend();   // 由这里决定“结束后谁来销毁帧”
  // 6) 外部在合适时机 destroy() 回收帧
}
```

配套读物：

- [Raymond Chen：initial/final suspend 的执行语义](https://devblogs.microsoft.com/oldnewthing/20210331-00/?p=105028)
- [Lewis Baker：`co_await` 的 awaiter 三连与对称转移](https://lewissbaker.github.io/2017/11/17/understanding-operator-co_await)

---

## 为何很多教程看起来要写一坨 `task/promise_type`？

这正是三件“必须你来定”的事：

1. **返回对象接口**：是否暴露 `resume()`/`done()`？是否提供 `get()`/迭代器？C++20 没有统一“用户级 API”，你需要一个 class 把“如何使用这段协程”表达清楚。
2. **启动/结束策略**：`initial_suspend()` 选择 `suspend_always` 就是**懒启动**，`suspend_never` 就是**即刻启动**；`final_suspend()` 常用 `suspend_always`，方便外部安全回收帧（析构里 `destroy()`）。
3. **结果与异常交付**：`return_value/return_void` 决定 `co_return` 的去处；`unhandled_exception` 常用 `std::exception_ptr` 存起来，等 `await_resume()` 或 `get()` 再抛回。

——换言之，那坨 class 不是啰嗦，而是把**权力和开销控制权**还给你。

---

## 一个“带注释的极简骨架”，把执行路径再过一遍

```cpp
struct task {
  struct promise_type {
    task get_return_object() {
      // ① 生成“把柄”（返回对象）— 通常把 handle 包进 task
      return task{ std::coroutine_handle<promise_type>::from_promise(*this) };
    }
    // ② 初始挂起：决定是否懒启动
    std::suspend_always initial_suspend() noexcept { return {}; }
    // ③ 结束策略：停在 final_suspend，等“把柄”析构时 destroy()
    std::suspend_always final_suspend()   noexcept { return {}; }
    void return_void() noexcept {}                    // ④ co_return
    void unhandled_exception() { std::terminate(); } // ⑤ 异常通道（教学版）
  };

  using handle = std::coroutine_handle<promise_type>;
  explicit task(handle h): h(h) {}
  ~task(){ if(h) h.destroy(); }     // ⑥ 回收协程帧（不泄漏）

  bool resume(){ if(!h || h.done()) return false; h.resume(); return !h.done(); }
private: handle h{};
};
```

对号入座：创建→`get_return_object`→（可能）懒启动→途中 `co_await`（可能挂起）→`co_return`→`final_suspend`→外部 `destroy()`。

---

## 协程里的“异常通道”怎么写才标准？（最小可复用片段）

大白话：**协程里没被 `try` 住的异常，会被送进 `promise_type::unhandled_exception()`。*- 常见做法是用 `std::exception_ptr` 存起来，外部在 `await_resume()` 或 `get()` 时**原样抛回**。

```cpp
#include <coroutine>
#include <exception>
#include <optional>

template<class T>
struct task {
  struct promise_type {
    std::optional<T> value;
    std::exception_ptr ep;

    task get_return_object() {
      return task{ std::coroutine_handle<promise_type>::from_promise(*this) };
    }
    std::suspend_always initial_suspend() noexcept { return {}; }
    std::suspend_always final_suspend()   noexcept { return {}; }

    void return_value(T v) noexcept(std::is_nothrow_move_constructible_v<T>) { value = std::move(v); }
    void unhandled_exception() noexcept { ep = std::current_exception(); } // 捕获异常
  };

  using handle = std::coroutine_handle<promise_type>;
  explicit task(handle h) : h_(h) {}
  task(task&& o) noexcept : h_(std::exchange(o.h_, {})) {}
  ~task() { if (h_) h_.destroy(); }

  bool resume() { if (!h_ || h_.done()) return false; h_.resume(); return !h_.done(); }

  // 取结果：若有异常，原样抛回
  T get() const {
    if (h_.promise().ep) std::rethrow_exception(h_.promise().ep);
    return *h_.promise().value;
  }

private:
  handle h_{};
};
```

关键 API：[`std::exception_ptr` / `std::current_exception()` / `std::rethrow_exception()`](https://en.cppreference.com/w/cpp/error/exception_ptr)。

---

## `initial_suspend` / `final_suspend` 的三种常见搭配

| 搭配                                                  | 含义             | 适用场景                                     | 风险/备注    |
| :-------------------------------------------------- | :------------- | :--------------------------------------- | :------- |
| `initial: suspend_always` + `final: suspend_always` | **懒启动，终点停一下*-  | 教学/库默认：外部拿到把柄后手动 `resume()`；结束点留给外部安全回收帧 | 最稳，少踩坑   |
| `initial: suspend_never` + `final: suspend_always`  | **创建即跑，终点停一下*- | “点火就跑”的一次性任务                             | 常用组合     |
| `initial: suspend_never` + `final: suspend_never`   | **开头/结尾都不停*-   | 极少数“自管生命周期”的场景                           | 稍不留神就 UB |

建议新手就用“**双 always**”（懒启动 + 终点停），把回收放在返回对象析构里，最不容易出错。参考：[initial/final suspend 详解](https://devblogs.microsoft.com/oldnewthing/20210331-00/?p=105028)。

---

## 为啥标准的 `std::future` 不能直接 `co_await`？

因为标准并没要求 `std::future/std::shared_future` 提供 `operator co_await()` 或 awaiter 三件套，所以**不具备 `co_await` 协议**。某些平台/框架曾做过扩展，后来也强调这不是可移植标准。想要 `co_await future`，依赖库扩展，而非 C++20 标配。参考：

- [Why did I lose the ability to co_await a std::future…](https://devblogs.microsoft.com/oldnewthing/20200916-00/?p=104227)

延伸阅读：C++20 的 `<coroutine>` 只提供**语言底座**；**没有**配套通用 I/O/任务类型。C++23 才把 `std::generator` 纳入标准库用于生成器场景。

---

## 课上即可复现的两段“流程可视化”

**片段 A：`co_await` 的三步曲（ready/suspend/resume）**

```cpp
#include <coroutine>
#include <iostream>

struct LogAwaiter {
  bool await_ready() const noexcept { std::cout << "[ready? false]\n"; return false; }
  void await_suspend(std::coroutine_handle<>) const noexcept { std::cout << "[suspend]\n"; }
  int  await_resume()  const noexcept { std::cout << "[resume]\n"; return 7; }
};

struct Task {
  struct promise_type {
    Task get_return_object() { return Task{ std::coroutine_handle<promise_type>::from_promise(*this) }; }
    std::suspend_always initial_suspend() noexcept { std::cout << "(initial suspend)\n"; return {}; }
    std::suspend_always final_suspend()   noexcept { std::cout << "(final suspend)\n";   return {}; }
    void return_void() noexcept {}
    void unhandled_exception(){ std::terminate(); }
  };
  using H = std::coroutine_handle<promise_type>;
  explicit Task(H h):h(h){} ~Task(){ if(h) h.destroy(); }
  bool resume(){ if(!h||h.done()) return false; h.resume(); return !h.done(); }
  H h;
};

Task f() {
  std::cout << "A\n";
  int x = co_await LogAwaiter{};
  std::cout << "B, x=" << x << "\n";
}

int main() {
  Task t = f();      // (initial suspend)
  t.resume();        // A -> [ready?] -> [suspend]
  t.resume();        // [resume] -> B -> (final suspend)
}
```

**片段 B：`co_return` 的去向（return_value 通道）**

```cpp
#include <coroutine>
#include <optional>
#include <iostream>

struct IntTask {
  struct promise_type {
    std::optional<int> r;
    IntTask get_return_object(){ return IntTask{ std::coroutine_handle<promise_type>::from_promise(*this) }; }
    std::suspend_always initial_suspend() noexcept { return {}; }
    std::suspend_always final_suspend()   noexcept { return {}; }
    void return_value(int v) noexcept { r = v; }   // ← co_return v 到这儿
    void unhandled_exception(){ std::terminate(); }
  };
  using H = std::coroutine_handle<promise_type>;
  explicit IntTask(H h):h(h){} ~IntTask(){ if(h) h.destroy(); }
  bool resume(){ if(!h||h.done()) return false; h.resume(); return !h.done(); }
  int  get() const { return *h.promise().r; }
  H h;
};

IntTask calc(){ co_return 42; }

int main(){
  auto t = calc();
  while(t.resume()){}                  // 跑到 final_suspend
  std::cout << t.get() << "\n";        // 42
}
```

---

## 口袋速记卡（两行就记住）

- **语言给机关，库定策略**：C++20 只提供协程语义与 `<coroutine>` 底座；返回对象长相、调度模型、结果/异常通道都由你（或库）决定。
- **从 C++23 起有现成生成器**：`std::generator` 让“逐个产出”这类场景一行就用；底层那套可定制机杼依旧保留，灵活性不打折。

---

## 参考资料（直接可点开）

- C++ reference：Coroutines（语言总览） —  [https://en.cppreference.com/w/cpp/language/coroutines.html](https://en.cppreference.com/w/cpp/language/coroutines.html)
- C++ reference：`std::coroutine_handle` — [https://en.cppreference.com/w/cpp/coroutine/coroutine_handle.html](https://en.cppreference.com/w/cpp/coroutine/coroutine_handle.html)
- C++ reference：`std::suspend_always` — [https://en.cppreference.com/w/cpp/coroutine/suspend_always.html](https://en.cppreference.com/w/cpp/coroutine/suspend_always.html)
- C++ reference：`co_await` — [https://en.cppreference.com/w/cpp/keyword/co_await.html](https://en.cppreference.com/w/cpp/keyword/co_await.html)
- C++ reference：`co_yield` — [https://en.cppreference.com/w/cpp/keyword/co_yield.html](https://en.cppreference.com/w/cpp/keyword/co_yield.html)
- C++ reference：`co_return` — [https://en.cppreference.com/w/cpp/keyword/co_return.html](https://en.cppreference.com/w/cpp/keyword/co_return.html)
- C++ reference：`<generator>`（C++23）— [https://en.cppreference.com/w/cpp/header/generator](https://en.cppreference.com/w/cpp/header/generator)
- Lewis Baker：Understanding `operator co_await` — [https://lewissbaker.github.io/2017/11/17/understanding-operator-co_await](https://lewissbaker.github.io/2017/11/17/understanding-operator-co_await)
- Raymond Chen：initial/final suspend — [https://devblogs.microsoft.com/oldnewthing/20210331-00/?p=105028](https://devblogs.microsoft.com/oldnewthing/20210331-00/?p=105028)
- Raymond Chen：`std::future` 与 `co_await` 的来龙去脉 — [https://devblogs.microsoft.com/oldnewthing/20200916-00/?p=104227](https://devblogs.microsoft.com/oldnewthing/20200916-00/?p=104227)
- C++ reference：`std::exception_ptr` — [https://en.cppreference.com/w/cpp/error/exception_ptr](https://en.cppreference.com/w/cpp/error/exception_ptr)

---

> 到目前为止你已经比其他人都要强，学得超棒！感谢你坚持看技术型虚拟主播艾瑞卡的教程到这里，这一节就到此结束啦，祝你编程愉快！🎉💻 (≧▽≦) —艾瑞卡