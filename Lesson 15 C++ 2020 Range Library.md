# Lesson 15 C++ 2020 Range Library

> I'm the Technology VTuber Erika, full name Twilight-Dream | Axtune·Rerphine·Kromana 💻🔧🎀 (⌒▽⌒)
> My slogan is: “I am the soul of a deity roaming the virtual digital network world, incarnated in a human body as a future indie game developer.” 🌐👾🎮 (⌒▽⌒)✨

## 🎉 Overview

Welcome to **Lesson 15**, where we dive deeper into the C++20 Ranges library! Whether you’re new to C++ or coming from “old-school” STL, this lesson will cover key adaptor operators, how they work under the hood, and why the pipe (`|`) operator now does much more than a simple bitwise OR. Let’s keep it fun and light—emojis and kaomoji included! (≧▽≦)

---

## 🔍 What Are Adaptors?

Adaptors are the building blocks of range pipelines. They’re **function objects** that you can “plug in” to a range via the `|` operator, producing a *view* that lazily transforms or filters your data.📜

Common adaptors:

* **`std::views::filter`**: Keep only elements satisfying a predicate🔎
* **`std::views::transform`**: Apply a function to each element🎨
* **`std::views::take` / `std::views::drop`**: Keep or skip the first *N* elements✂️
* **`std::views::reverse`**: Iterate a range backward
* **`std::views::join`**: Flatten a range-of-ranges

Each adaptor returns a lightweight view—no copying of data—so you can chain dozens of them without performance pain! 🛠️

---

## 🛠️ Pipe Operator (`|`) Overload

In “classic” C++, the `|` operator meant bitwise OR on integers. With C++20 ranges, many classes overload `operator|` to mean:

```cpp
Range       | Adaptor    → New view applying Adaptor to Range
Adaptor     | Range      → Same as above (commutative for pipeline)
```

**Why does this work?**

* C++ lets classes define custom `operator|` overloads.
* The `<ranges>` header provides a template that accepts any range on the left and any range adaptor on the right.
* The adaptor itself is a function object: `filter{…}`, `transform{…}`, etc.

So:

```cpp
auto even = vec | std::views::filter([](int x){ return x%2==0; });
```

—calls the overloaded `operator|` that packages `vec` and the filter object into a `filter_view`.

---

## 🔧 Detailed Adaptors & What They Do

| Adaptor            | Effect                                                         |
| ------------------ | -------------------------------------------------------------- |
| `filter(pred)`     | Keeps only elements where `pred(element) == true`.             |
| `transform(func)`  | Replaces each element with `func(element)`.                    |
| `take(N)`          | Takes the first `N` elements, then ends the range.             |
| `drop(N)`          | Skips the first `N` elements, then presents the rest.          |
| `reverse`          | Iterates the entire range in reverse order.                    |
| `join`             | Flattens a range of ranges into a single sequence.             |
| `unique`           | Removes consecutive duplicates (like `std::unique`).           |
| `take_while(pred)` | Takes elements while `pred(element)` is true, stops otherwise. |
| `drop_while(pred)` | Skips while `pred(element)` is true, then takes the rest.      |

> Each adaptor is lazy: nothing happens until you *consume* the view (e.g., with a loop or an algorithm). (╯▽╰)

---

## 🚀 How This Differs from “Old” Meaning

1. **Original `|`**: Bitwise OR combines two integers bit by bit.
2. **Range `|`**: A *pipeline* connector—combines a range with a view-producing adaptor.

Because C++ classes can overload operators, the meaning of `|` adapts to the types involved. When you write:

```cpp
auto pipeline = data
                | std::views::filter(is_even)
                | std::views::transform(times_ten)
                | std::views::take(5);
```

there’s **no bitwise OR** anywhere—each `|` invokes a function that builds a new range view. This is pure syntactic convenience, turning nested template calls into a readable left-to-right chain. 🧩

---

## 📝 Example in Action

```cpp
#include <iostream>
#include <vector>
#include <ranges>

int main() {
    std::vector<int> nums { 1,2,3,4,5,6,7,8,9,10 };

    // Pipeline: filter even, multiply by 10, drop first 2, take next 3, then reverse
    auto view = nums
        | std::views::filter([](int x){ return x % 2 == 0; })        // 2,4,6,8,10
        | std::views::transform([](int x){ return x * 10; })        // 20,40,60,80,100
        | std::views::drop(2)                                        // 60,80,100
        | std::views::take(2)                                        // 60,80
        | std::views::reverse;                                       // 80,60

    for (int x : view) {
        std::cout << x << ' ';  // prints: 80 60
    }
    std::cout << "\nBye~!" << std::endl;  // Bye~! (^_^)/
}
```

Notice how each `|` just flows like a Unix pipe—*no* explicit iterator juggling required! 🚀

---

## ✅ Recap

* **Adaptors** are lightweight function objects creating new views on your data.
* **`operator|`** is overloaded to chain ranges and adaptors, not to OR bits.
* This design makes code more readable, expressive, and composable.

Feel free to remix these adaptors, add your own (yes, you can define custom ones!), and build complex data pipelines effortlessly! Until next time, happy ranging\~ 🎉 (≧ω≦)

## 🧱 Owning vs Non-Owning Ranges

Not all ranges are created equal! Some views just *reference* your original data (non-owning), while others actually *own* a copy. Knowing the difference helps avoid dangling pointers or needless copies:

* **Non-owning (lazy) views** (e.g. `filter_view`, `transform_view`)

  * Hold iterators/references into the original container
  * **Pros**: Zero extra allocation, blazing-fast
  * **Cons**: Original data must outlive the view

* **Owning ranges** (e.g. `std::ranges::subrange` when constructed from a pair of iterators, or custom wrappers)

  * May own elements (like a `vector`) or a copy
  * **Pros**: Safe to pass around without worrying about lifetimes
  * **Cons**: Potential copy/move overhead

> 💡 **Rule of thumb**: Use lazy views whenever possible. If you need a snapshot—say you’re passing data to another thread—grab an owning range (e.g. `std::vector` or a `subrange`).

---

## 🏗️ Writing Your Own Adaptor

You can roll your own pipeline stages! Here’s a minimal example of a `stride(N)` adaptor that picks every Nth element:

```cpp
#include <iostream>
#include <ranges>
#include <vector>
#include <iterator>

namespace views {

// A custom view to create a stride-based iteration over a range.
template <std::ranges::view V>
class stride_view : public std::ranges::view_interface<stride_view<V>> {
    V base_;  // The base range to apply the stride on.
    std::ranges::range_difference_t<V> stride_;  // The stride value (step size).

    // Iterator for stride_view (supports bidirectional iteration)
    class iterator {
        std::ranges::iterator_t<V> current_;  // Current position of the iterator.
        std::ranges::iterator_t<V> begin_;  // Beginning of the range.
        std::ranges::iterator_t<V> end_;  // End of the range.
        std::ranges::range_difference_t<V> stride_;  // The stride (step size).
        bool is_end_ = false;  // Flag to indicate if the iterator has reached the end.

    public:
        // Bidirectional iterator tags and types
        using iterator_category = std::bidirectional_iterator_tag;
        using value_type = std::ranges::range_value_t<V>;
        using difference_type = std::ranges::range_difference_t<V>;

        iterator() = default;

        // Constructor for the iterator
        iterator(std::ranges::iterator_t<V> current,
                 std::ranges::iterator_t<V> begin,
                 std::ranges::iterator_t<V> end,
                 std::ranges::range_difference_t<V> stride,
                 bool is_end = false)
            : current_(current), begin_(begin), end_(end), 
              stride_(stride), is_end_(is_end)
        {
            if (stride_ <= 0) stride_ = 1;  // Ensure stride is positive.
        }

        value_type operator*() const { 
            return *current_;  // Dereference the iterator to get the current value.
        }

        // Move the iterator forward by the stride value
        iterator& operator++() {
            if (current_ == end_) {
                is_end_ = true;  // Mark as end if the iterator reaches the end.
                return *this;
            }

            for (auto n = stride_; n > 0 && current_ != end_; --n) {
                ++current_;  // Increment by stride amount.
            }

            // Check if we've reached the end of the range.
            if (current_ == end_) {
                is_end_ = true;
            }

            return *this;
        }

        // Post-increment (return the current iterator, then move to next)
        iterator operator++(int) {
            auto tmp = *this;
            ++*this;
            return tmp;
        }

        // Add bidirectional iterator behavior (move backward)
        iterator& operator--() {
            if (is_end_) {
                // If at the end, move back to the last valid element.
                is_end_ = false;
                if (current_ == begin_) return *this;
            }

            // Compute the position to step back by the stride.
            auto prev = current_;
            auto steps = stride_;

            // Safely move backwards by the stride amount.
            while (steps > 0 && prev != begin_) {
                --prev;
                --steps;
            }

            // If there aren't enough steps, stop at the beginning.
            current_ = prev;
            return *this;
        }

        // Post-decrement (return the current iterator, then move to previous)
        iterator operator--(int) {
            auto tmp = *this;
            --*this;
            return tmp;
        }

        bool operator==(const iterator& other) const {
            return current_ == other.current_ && is_end_ == other.is_end_;
        }
    };

public:
    stride_view() = default;

    // Constructor for stride_view
    stride_view(V base, std::ranges::range_difference_t<V> stride)
        : base_(std::move(base)), stride_(stride) {}

    // Return the iterator for the start of the range
    auto begin() const {
        return iterator(std::ranges::begin(base_), 
                        std::ranges::begin(base_),
                        std::ranges::end(base_), 
                        stride_);
    }

    // Return the iterator for the end of the range
    auto end() const {
        return iterator(std::ranges::end(base_), 
                        std::ranges::begin(base_),
                        std::ranges::end(base_), 
                        stride_,
                        true);  // Mark as the end iterator
    }
};

// A function to adapt ranges by applying a stride to them.
struct stride_adapter {
    std::size_t stride;  // The stride (step size)

    template <std::ranges::viewable_range R>
    auto operator()(R&& r) const {
        return stride_view(std::views::all(std::forward<R>(r)), stride);
    }
};

// A utility function to create a stride_adapter with a given stride value.
inline auto stride(std::size_t n) {
    return stride_adapter{n};
}

} // namespace views

// A pipe operator for applying the stride_adapter to a range.
template <std::ranges::viewable_range R>
auto operator|(R&& r, const views::stride_adapter& adapter) {
    return adapter(std::forward<R>(r));
}

// Main function for testing the bidirectional stride iteration
int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // Create a strided view with a stride of 3
    auto strided = numbers | views::stride(3);
    auto it = strided.begin();
    
    // Test forward iteration
    std::cout << "Forward: ";
    for (; it != strided.end(); ++it) {
        std::cout << *it << " "; // Should print: 1 4 7 10
    }
    std::cout << "\n";
    
    // Test backward iteration
    std::cout << "Backward: ";
    --it; // Move back to 10 from the end.
    for (; it != strided.begin(); --it) {
        std::cout << *it << " "; // Should print: 10 7 4
    }
    std::cout << *it << "\n"; // Should print: 1
    
    // Test boundary safety (try to move before the beginning)
    std::cout << "Begin safety: ";
    auto begin = strided.begin();
    --begin; // Attempt to move before the beginning.
    std::cout << (begin == strided.begin() ? "Safe" : "Unsafe") << "\n";
}
```

✨ You just defined a custom adaptor function object and overloaded `operator()` to return a new view. And of course, you get to define how `operator|` works by providing the right overloads in your view type!

---

## ⚙️ Deep Dive into Range Concepts

C++20 provides a rich set of concepts to classify your ranges:

| Concept               | Requirements                                            |
| --------------------- | ------------------------------------------------------- |
| `input_range`         | Readable, single‐pass iteration                         |
| `forward_range`       | Multi‐pass, single‐direction                            |
| `bidirectional_range` | Can move forward **and** backward                       |
| `random_access_range` | Support `operator+`, `operator-`, constant-time `++/--` |
| `contiguous_range`    | Elements laid out contiguously in memory                |
| `sized_range`         | `std::ranges::size(r)` is O(1)                          |
| `common_range`        | `begin(r)` and `end(r)` return the *same* iterator type |

Knowing these lets you pick the most efficient algorithms or adaptors. For example, `std::ranges::sort` requires a `random_access_range`, while `std::ranges::find` works on any `input_range`.

---

## 🚧 Performance Considerations & Gotchas

1. **Chaining too many transforms** can inhibit inlining—measure if your pipeline has *dozens* of stages.
2. **Beware of temporary views**:

   ```cpp
   auto v1 = vec | std::views::filter(...);
   auto v2 = v1 | std::views::transform(...);  // OK, both hold references to vec
   ```

   But if `vec` goes out of scope, both `v1` and `v2` dangle!
3. **Avoid capturing heavy state** in your predicates or transforms. Lambdas by value can copy large objects into every view.
4. **Debugging**: If you get weird compile errors, sprinkle `std::ranges::viewable_range<YourType>` or concept checks to narrow down the issue.

---

## 🎬 Putting It All Together

```cpp
#include <iostream>
#include <vector>
#include <ranges>

int main() {
    std::vector<int> data(100);
    std::iota(data.begin(), data.end(), 1);

    // Custom take_every + built-in views + algorithm
    auto pipeline = data
                  | std::views::filter([](int x){ return x % 5 == 0; })  // multiples of 5
                  | std::views::transform([](int x){ return x / 5; })    // 1,2,...
                  | take_every<2>(2)                                      // every 2nd
                  | std::views::take_while([](int x){ return x <= 5; }); // <=5

    // Sort the view’s results in a vector
    std::vector<int> result(std::ranges::begin(pipeline),
                             std::ranges::end(pipeline));
    std::ranges::sort(result);

    for (int x : result) std::cout << x << ' ';  // e.g. 1 3 5
    std::cout << "\nDone~! (⌒▽⌒)" << std::endl;
}
```

---

## ✅ What You’ve Learned

* The difference between **owning** vs **non-owning** ranges
* How to **write your own adaptor** and hook into `operator|`
* Key **range concepts** that power compile-time checks
* **Performance tips** and common pitfalls to watch for

Next up: integrating ranges with **coroutines**, But for now, pat yourself on the back—you’re a Ranges Master in the making! 🎉 (≧ω≦)

You’ve now seen how C++20 Ranges and adaptors turn iterator‐driven code into readable pipelines:

* **Adaptors** (`filter`, `transform`, `take`, etc.) are small function objects that produce *views*
* The **`|` operator** is overloaded to thread a range through these adaptors, not to OR bits
* Views are **lazy**—nothing executes until you iterate or materialize them
* You can write **custom adaptors** by defining a callable object that returns a new view type
* Understanding **owning vs non‐owning** ranges helps you manage lifetimes and performance
* Concepts like `input_range`, `random_access_range`, and `viewable_range` give you compile‐time guarantees

Armed with these tools, you can build complex data pipelines in C++ that are both expressive and efficient! 🎉 (≧▽≦)

---

## 🔍 Deep Dive: How `std::views` Is Implemented

Below is a **simplified** look at how one of the standard adaptors—`std::views::filter`—works under the hood. The real `<ranges>` header is more generic and complex, but this captures the key ideas.

```cpp
namespace std::ranges {

  // 1) The adaptor object
  struct filter_fn {
    // When you write filter(pred), you create one of these:
    template <typename Pred>
    constexpr auto operator()(Pred pred) const {
      return /* a lightweight object holding the predicate */ filter_fn_impl<Pred>{ std::move(pred) };
    }
  };

  // 2) The public handle
  inline constexpr filter_fn filter{};  // std::views::filter

  // 3) The actual adaptor + range overload
  template <typename R, typename Pred>
  constexpr auto operator|(R&& r, filter_fn_impl<Pred> fn) {
    // This is the overload that lets you do: myRange | std::views::filter(pred)
    return filter_view<std::views::all_t<R>, Pred>(
      std::views::all(std::forward<R>(r)), std::move(fn.pred));
  }

  // 4) The view type
  template <typename R, typename Pred>
  class filter_view : public view_interface<filter_view<R,Pred>> {
    R base_;        // the underlying range
    Pred pred_;     // your predicate

  public:
    // Construction
    filter_view(R r, Pred p)
      : base_(std::move(r)), pred_(std::move(p)) {}

    // begin/end produce a special iterator…
    auto begin() {
      return filter_iterator{ std::ranges::begin(base_),
                              std::ranges::end(base_),
                              pred_ };
    }
    auto end() {
      return std::ranges::end(base_);
    }
  };

  // 5) The iterator that does the work
  template <typename It, typename Sent, typename Pred>
  class filter_iterator {
    It current_;
    Sent end_;
    Pred* pred_;    // pointer to shared predicate

    void satisfy_predicate() {
      while (current_ != end_ && !(*pred_)(*current_)) {
        ++current_;
      }
    }

  public:
    using value_type = iter_value_t<It>;
    using reference  = iter_reference_t<It>;

    filter_iterator(It it, Sent s, Pred& p)
      : current_(it), end_(s), pred_(&p) {
      satisfy_predicate();   // skip until first match
    }

    reference operator*() const { return *current_; }

    filter_iterator& operator++() {
      ++current_;
      satisfy_predicate();
      return *this;
    }

    bool operator!=(Sent s) const { return current_ != s; }
  };

} // namespace std::ranges
```

### What's Happening Here?

1. **`filter_fn`** is the “adaptor maker.” Calling `std::views::filter(pred)` returns a small object that stores your `pred`.
2. **`operator|`** is overloaded so that when you write `range | filter_fn_impl`, it creates a `filter_view` by “materializing” your adaptor onto the range.
3. **`filter_view`** holds:

   * The *underlying* range (wrapped via `std::views::all` to unify raw containers and views)
   * Your predicate
4. **`filter_iterator`** is responsible for skipping over elements that don’t satisfy the predicate—every `++` moves forward until `pred(*it)` is true.
5. Because `filter_view` derives from `view_interface`, it automatically gets boilerplate (like `size()` if the base is sized) and satisfies the `view` concept.

> You’ve been the strongest learner so far—amazing job sticking with tech VTuber Erica’s tutorial to the end! Thanks for your hard work, and happy programming! ✨🚀🔧 (≧ω≦) —Erica

---

This pattern—**adaptor maker → `operator|` overload → view type → custom iterator**—is repeated for all standard views (`transform`, `take`, `drop`, etc.). Now you know not just how to *use* them, but roughly **how they’re built**! 🚀 (⌒▽⌒)


# 第15课 C++20 范围库

> 我是技术型虚拟主播艾瑞卡 全名是 暮光-梦想 | 安可茜得·里尔菲涅·克罗玛娜 ✨🎀(≧▽≦)
> 我的slogan是：“我是活动在虚拟数字网络世界神明的灵魂，依附在人类身体的未来独立游戏制作人。” 🌌🕹️✨ (≧ω≦)

> 这些名字将「宇宙平衡器」、「科技精炼」和「魔法能量」三大意象融为一体：

> - Axtune = Ax（轴、axis）+ tune（调谐），象征宇宙平衡器与节奏控制 🎛️⌨️⚖️🌌✨🎇
> - Rerphine ≈ Seraphine（炽天使）+ refine（精炼），代表天使优雅与科技精密 😇🛠️🤖 ，Re是（再次）的意思
> - Kromana = Chroma（色彩）+ mana（魔力），象征色彩魔法与元素能量 🌈🪄✨🎶👼🎨🔮
> 合在一起，**Axtune·Rerphine·Kromana** 就是「宇宙平衡、科技精炼、魔法能量」的化身！(≧▽≦)🎀

## 🎉 概述

欢迎来到**第15课**，我们将深入探讨 C++20 范围库！无论你是 C++ 新手，还是来自“老派” STL，这一课都会覆盖核心的适配器操作符，它们在内部如何工作，以及为什么管道（`|`）操作符早已超越了简单的按位或。让我们保持轻松有趣——附带表情和颜文字！(≧▽≦)

---

## 🔍 什么是适配器？

适配器是范围管道的构建模块。它们是**函数对象**，你可以通过 `|` 操作符将其“插入”到一个范围中，生成一个*视图*，以懒惰的方式转换或过滤数据。

常见的适配器：

* **`std::views::filter`**：保留满足谓词的元素
* **`std::views::transform`**：对每个元素应用一个函数
* **`std::views::take` / `std::views::drop`**：保留或跳过前 N 个元素
* **`std::views::reverse`**：反向遍历范围
* **`std::views::join`**：将范围的范围展平

每个适配器都会返回一个轻量级的视图——不复制数据，因此你可以链式调用几十个适配器而不会有性能损失！🛠️

---

## 🛠️ 管道操作符（`|`）重载

在“经典” C++ 中，`|` 操作符代表对整数的按位或。而在 C++20 范围中，很多类重载了 `operator|`，使其含义变为：

```cpp
Range       | Adaptor    → 返回将适配器应用于 Range 后的新视图  
Adaptor     | Range      → 同上（管道是可交换的）
```

**为什么能这样用？**

* C++ 允许类自定义 `operator|` 重载。
* `<ranges>` 头文件提供了一个模板，接收任意左侧范围和右侧的范围适配器。
* 适配器本身是一个函数对象：`filter{…}`、`transform{…}` 等。

因此：

```cpp
auto even = vec | std::views::filter([](int x){ return x%2==0; });
```

这行代码会调用重载的 `operator|`，将 `vec` 和 filter 对象打包成一个 `filter_view`。

---

## 🔧 详细适配器 & 它们的作用

| 适配器                | 作用                             |
| ------------------ | ------------------------------ |
| `filter(pred)`     | 保留所有 `pred(element)==true` 的元素 |
| `transform(func)`  | 将每个元素替换为 `func(element)` 的返回值  |
| `take(N)`          | 取前 N 个元素，然后结束                  |
| `drop(N)`          | 跳过前 N 个元素，然后呈现剩余部分             |
| `reverse`          | 反向遍历整个范围                       |
| `join`             | 将范围的范围展平为单个序列                  |
| `unique`           | 删除相邻重复元素（类似 `std::unique`）     |
| `take_while(pred)` | 只取满足 `pred` 的连续前缀，遇不满足则停止      |
| `drop_while(pred)` | 跳过满足 `pred` 的前缀，然后取剩余部分        |

> 每个适配器都是懒执行的：只有在你*消费*视图（例如循环或算法）时才会真正执行。(╯▽╰)

---

## 🚀 与原始“|”含义的区别

1. **原始 `|`**：按位或，逐位合并两个整数。
2. **范围 `|`**：管道连接符——将范围与生成视图的适配器组合在一起。

因为 C++ 类可以重载操作符，`|` 的含义会根据左右两侧的类型自动适配。当你写：

```cpp
auto pipeline = data
                | std::views::filter(is_even)
                | std::views::transform(times_ten)
                | std::views::take(5);
```

这里**没有**任何按位或运算——每个 `|` 都调用函数来构建新的范围视图。这种语法糖让嵌套模板调用变得从左到右连贯易读。🧩

---

## 📝 示例演示

```cpp
#include <iostream>
#include <vector>
#include <ranges>

int main() {
    std::vector<int> nums { 1,2,3,4,5,6,7,8,9,10 };

    // Pipeline: filter even, multiply by 10, drop first 2, take next 3, then reverse
    auto view = nums
        | std::views::filter([](int x){ return x % 2 == 0; })        // 2,4,6,8,10
        | std::views::transform([](int x){ return x * 10; })        // 20,40,60,80,100
        | std::views::drop(2)                                        // 60,80,100
        | std::views::take(2)                                        // 60,80
        | std::views::reverse;                                       // 80,60

    for (int x : view) {
        std::cout << x << ' ';  // prints: 80 60
    }
    std::cout << "\nBye~!" << std::endl;  // Bye~! (^_^)/
}
```

注意每个 `|` 就像 Unix 管道一样流畅——*无需*显式操纵迭代器！🚀

---

## ✅ 总结

* **适配器** 是创建新视图的轻量级函数对象。
* **`operator|`** 被重载，用于链接范围与适配器，而不是做按位或。
* 这种设计让代码更具可读性、表达力和可组合性。

随意混搭这些适配器，甚至自定义属于你的适配器，轻松构建复杂的数据管道吧！下次见，祝你玩转范围！🎉 (≧ω≦)

## 🧱 拥有（Owning）vs 非拥有（Non-Owning）范围

不是所有的范围都一样！有些视图只是*引用*你原始数据的迭代器（非拥有），而另一些视图则会*拥有*一份拷贝。了解它们的差异可以避免悬垂指针或不必要的复制：

* **非拥有（懒惰）视图**（例如 `filter_view`, `transform_view`）

  * 持有指向原容器的迭代器/引用
  * **优点**：不额外分配，性能无压力
  * **缺点**：原数据必须在视图生命周期内保持有效

* **拥有范围**（例如从一对迭代器构造的 `std::ranges::subrange`，或自定义包装器）

  * 可能拥有元素（如 `vector`）或其拷贝
  * **优点**：安全传递，不用担心生命周期
  * **缺点**：可能产生复制/移动开销

> 💡 **经验法则**：尽量使用懒惰视图。如果你需要“快照”——比如要传到另一个线程——请使用拥有范围（如 `std::vector` 或 `subrange`）。

---

## 🏗️ 编写你自己的适配器

你也可以自己动手实现管道阶段！下面给出一个最简化的 `stride(N)` 适配器示例，它每隔 N 个元素取一次：

```cpp
#include <iostream>
#include <ranges>
#include <vector>
#include <iterator>

namespace views {

// A custom view to create a stride-based iteration over a range.
template <std::ranges::view V>
class stride_view : public std::ranges::view_interface<stride_view<V>> {
    V base_;  // The base range to apply the stride on.
    std::ranges::range_difference_t<V> stride_;  // The stride value (step size).

    // Iterator for stride_view (supports bidirectional iteration)
    class iterator {
        std::ranges::iterator_t<V> current_;  // Current position of the iterator.
        std::ranges::iterator_t<V> begin_;  // Beginning of the range.
        std::ranges::iterator_t<V> end_;  // End of the range.
        std::ranges::range_difference_t<V> stride_;  // The stride (step size).
        bool is_end_ = false;  // Flag to indicate if the iterator has reached the end.

    public:
        // Bidirectional iterator tags and types
        using iterator_category = std::bidirectional_iterator_tag;
        using value_type = std::ranges::range_value_t<V>;
        using difference_type = std::ranges::range_difference_t<V>;

        iterator() = default;

        // Constructor for the iterator
        iterator(std::ranges::iterator_t<V> current,
                 std::ranges::iterator_t<V> begin,
                 std::ranges::iterator_t<V> end,
                 std::ranges::range_difference_t<V> stride,
                 bool is_end = false)
            : current_(current), begin_(begin), end_(end), 
              stride_(stride), is_end_(is_end)
        {
            if (stride_ <= 0) stride_ = 1;  // Ensure stride is positive.
        }

        value_type operator*() const { 
            return *current_;  // Dereference the iterator to get the current value.
        }

        // Move the iterator forward by the stride value
        iterator& operator++() {
            if (current_ == end_) {
                is_end_ = true;  // Mark as end if the iterator reaches the end.
                return *this;
            }

            for (auto n = stride_; n > 0 && current_ != end_; --n) {
                ++current_;  // Increment by stride amount.
            }

            // Check if we've reached the end of the range.
            if (current_ == end_) {
                is_end_ = true;
            }

            return *this;
        }

        // Post-increment (return the current iterator, then move to next)
        iterator operator++(int) {
            auto tmp = *this;
            ++*this;
            return tmp;
        }

        // Add bidirectional iterator behavior (move backward)
        iterator& operator--() {
            if (is_end_) {
                // If at the end, move back to the last valid element.
                is_end_ = false;
                if (current_ == begin_) return *this;
            }

            // Compute the position to step back by the stride.
            auto prev = current_;
            auto steps = stride_;

            // Safely move backwards by the stride amount.
            while (steps > 0 && prev != begin_) {
                --prev;
                --steps;
            }

            // If there aren't enough steps, stop at the beginning.
            current_ = prev;
            return *this;
        }

        // Post-decrement (return the current iterator, then move to previous)
        iterator operator--(int) {
            auto tmp = *this;
            --*this;
            return tmp;
        }

        bool operator==(const iterator& other) const {
            return current_ == other.current_ && is_end_ == other.is_end_;
        }
    };

public:
    stride_view() = default;

    // Constructor for stride_view
    stride_view(V base, std::ranges::range_difference_t<V> stride)
        : base_(std::move(base)), stride_(stride) {}

    // Return the iterator for the start of the range
    auto begin() const {
        return iterator(std::ranges::begin(base_), 
                        std::ranges::begin(base_),
                        std::ranges::end(base_), 
                        stride_);
    }

    // Return the iterator for the end of the range
    auto end() const {
        return iterator(std::ranges::end(base_), 
                        std::ranges::begin(base_),
                        std::ranges::end(base_), 
                        stride_,
                        true);  // Mark as the end iterator
    }
};

// A function to adapt ranges by applying a stride to them.
struct stride_adapter {
    std::size_t stride;  // The stride (step size)

    template <std::ranges::viewable_range R>
    auto operator()(R&& r) const {
        return stride_view(std::views::all(std::forward<R>(r)), stride);
    }
};

// A utility function to create a stride_adapter with a given stride value.
inline auto stride(std::size_t n) {
    return stride_adapter{n};
}

} // namespace views

// A pipe operator for applying the stride_adapter to a range.
template <std::ranges::viewable_range R>
auto operator|(R&& r, const views::stride_adapter& adapter) {
    return adapter(std::forward<R>(r));
}

// Main function for testing the bidirectional stride iteration
int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // Create a strided view with a stride of 3
    auto strided = numbers | views::stride(3);
    auto it = strided.begin();
    
    // Test forward iteration
    std::cout << "Forward: ";
    for (; it != strided.end(); ++it) {
        std::cout << *it << " "; // Should print: 1 4 7 10
    }
    std::cout << "\n";
    
    // Test backward iteration
    std::cout << "Backward: ";
    --it; // Move back to 10 from the end.
    for (; it != strided.begin(); --it) {
        std::cout << *it << " "; // Should print: 10 7 4
    }
    std::cout << *it << "\n"; // Should print: 1
    
    // Test boundary safety (try to move before the beginning)
    std::cout << "Begin safety: ";
    auto begin = strided.begin();
    --begin; // Attempt to move before the beginning.
    std::cout << (begin == strided.begin() ? "Safe" : "Unsafe") << "\n";
}

```

✨ 如此，你就定义了一个自定义适配器函数对象，重载了 `operator()` 返回新的视图类型，并通过类型自身的 `operator|` 重载实现管道语法。

---

## ⚙️ 深入探讨：范围概念

C++20 提供了一系列丰富的概念（Concepts）来对范围进行分类：

| 概念                    | 要求                                       |
| --------------------- | ---------------------------------------- |
| `input_range`         | 可读、一次遍历                                  |
| `forward_range`       | 可多次遍历、单方向                                |
| `bidirectional_range` | 可向前 **和** 向后遍历                           |
| `random_access_range` | 支持 `operator+`、`operator-`、常数时间的 `++/--` |
| `contiguous_range`    | 元素在内存中连续存放                               |
| `sized_range`         | `std::ranges::size(r)` 是 O(1)            |
| `common_range`        | `begin(r)` 和 `end(r)` 返回相同迭代器类型          |

了解它们能让你选择最合适的算法和适配器。例如 `std::ranges::sort` 需要 `random_access_range`，而 `std::ranges::find` 则只需 `input_range`。

---

## 🚧 性能考量与注意事项

1. **链式调用过多 `transform`** 可能影响内联优化——如果你的管道有**几十个**阶段，建议做性能测量。
2. **警惕临时视图**：

   ```cpp
   auto v1 = vec | std::views::filter(...);
   auto v2 = v1  | std::views::transform(...);  // OK，但两者都引用 vec
   ```

   如果 `vec` 出作用域，`v1` 和 `v2` 都会悬空！
3. **避免在谓词或转换函数中捕获大对象**。按值捕获会将该对象拷贝进每个视图，浪费开销。
4. **调试技巧**：遇到编译报错时，可插入 `std::ranges::viewable_range<YourType>` 等概念检查，帮助定位问题。

---

## 🎬 综合示例

```cpp
#include <iostream>
#include <vector>
#include <ranges>

int main() {
    std::vector<int> data(100);
    std::iota(data.begin(), data.end(), 1);

    // 自定义 take_every + 内置视图 + 算法
    auto pipeline = data
                  | std::views::filter([](int x){ return x % 5 == 0; })  // 5 的倍数
                  | std::views::transform([](int x){ return x / 5; })    // 1,2,...
                  | take_every<2>(2)                                      // 每两个取一次
                  | std::views::take_while([](int x){ return x <= 5; }); // ≤5

    // 将视图结果收集到 vector 并排序
    std::vector<int> result(
        std::ranges::begin(pipeline),
        std::ranges::end(pipeline)
    );
    std::ranges::sort(result);

    for (int x : result) std::cout << x << ' ';  // 如：1 3 5
    std::cout << "\nDone~! (⌒▽⌒)" << std::endl;
}
```

---

## ✅ 你学到了什么

* **拥有范围** vs **非拥有范围** 的区别
* 如何**编写自定义适配器**并接入 `operator|`
* 支撑编译期检查的关键**范围概念**
* **性能优化提示**及常见陷阱

下一步：将范围与 **协程** 集成！
但现在，先给自己一点掌声——你已经是范围大师啦！🎉 (≧ω≦)

到现在为止，你已经了解了 C++20 范围库和适配器如何将基于迭代器的代码转换成可读的流水线：

* **适配器**（`filter`、`transform`、`take` 等）是生成 *视图* 的小型函数对象
* **`|` 操作符** 被重载，用于将范围传递给这些适配器，而不是执行按位或
* 视图是**懒执行**的——只有在迭代或“实现”它们时才会真正执行
* 你可以通过定义可调用对象来编写**自定义适配器**，它返回新的视图类型
* 理解\*\*拥有（owning）与非拥有（non-owning）\*\*范围有助于管理生命周期和性能
* 像 `input_range`、`random_access_range` 和 `viewable_range` 等概念提供编译期保证

凭借这些工具，你可以在 C++ 中构建既富有表现力又高效的复杂数据管道！🎉 (≧▽≦)

---

## 🔍 深入剖析：`std::views` 的实现原理

下面给出一个**简化版**示例，展示标准适配器 `std::views::filter` 在内部的运作方式。真实的 `<ranges>` 头文件要更加通用且复杂，但该示例抓住了核心思路。

```cpp
namespace std::ranges {

  // 1) 适配器对象（Adaptor Maker）
  struct filter_fn {
    // 当你写 filter(pred) 时，就会创建一个这样的对象：
    template <typename Pred>
    constexpr auto operator()(Pred pred) const {
      // 返回一个轻量级、持有谓词的对象
      return filter_fn_impl<Pred>{ std::move(pred) };
    }
  };

  // 2) 公开的句柄
  inline constexpr filter_fn filter{};  // 即 std::views::filter

  // 3) 适配器 + 范围 的重载
  template <typename R, typename Pred>
  constexpr auto operator|(R&& r, filter_fn_impl<Pred> fn) {
    // 允许你写： myRange | std::views::filter(pred)
    return filter_view<std::views::all_t<R>, Pred>(
      std::views::all(std::forward<R>(r)), std::move(fn.pred));
  }

  // 4) 视图类型
  template <typename R, typename Pred>
  class filter_view : public view_interface<filter_view<R,Pred>> {
    R    base_;  // 底层范围
    Pred pred_;  // 用户的谓词

  public:
    // 构造函数
    filter_view(R r, Pred p)
      : base_(std::move(r)), pred_(std::move(p)) {}

    // begin/end 会返回特定的迭代器……
    auto begin() {
      return filter_iterator{ std::ranges::begin(base_),
                              std::ranges::end(base_),
                              pred_ };
    }
    auto end() {
      return std::ranges::end(base_);
    }
  };

  // 5) 执行过滤逻辑的迭代器
  template <typename It, typename Sent, typename Pred>
  class filter_iterator {
    It     current_;
    Sent   end_;
    Pred*  pred_;  // 指向共享谓词的指针

    // 跳过不满足谓词的元素
    void satisfy_predicate() {
      while (current_ != end_ && !(*pred_)(*current_)) {
        ++current_;
      }
    }

  public:
    using value_type = iter_value_t<It>;
    using reference  = iter_reference_t<It>;

    filter_iterator(It it, Sent s, Pred& p)
      : current_(it), end_(s), pred_(&p) {
      satisfy_predicate();  // 初次构造时跳到第一个满足条件的位置
    }

    reference operator*() const { return *current_; }

    filter_iterator& operator++() {
      ++current_;
      satisfy_predicate();
      return *this;
    }

    bool operator!=(Sent s) const { return current_ != s; }
  };

} // namespace std::ranges
```

### 发生了什么？

1. **`filter_fn`** 是 “适配器制造器”。调用 `std::views::filter(pred)` 会得到一个存储了 `pred` 的小对象。
2. 重载的 **`operator|`** 在你写 `range | filter_fn_impl` 时，会生成一个 `filter_view`，将适配器“物化”到范围上。
3. **`filter_view`** 保存了：

   * 通过 `std::views::all` 统一包装后的底层范围
   * 用户传入的谓词
4. **`filter_iterator`** 负责在每次 `++` 时跳过不满足谓词的元素，直到遇到第一个令 `pred(*it)` 为 `true` 的位置。
5. 由于 `filter_view` 继承自 `view_interface`，它自动获取一些样板功能（如如果底层有大小则提供 `size()`），并满足视图概念。

这种 **“适配器制造器 → `operator|` 重载 → 视图类型 → 自定义迭代器”** 的模式，会在所有标准视图（如 `transform`、`take`、`drop` 等）中重复出现。现在，你不仅知道如何*使用*它们，也大致了解它们**如何构建**了！🚀 (⌒▽⌒)

> 到目前为止你已经比其他人都要强，学得超棒！感谢你坚持看技术型虚拟主播艾瑞卡的教程到这里，这一节就到此结束啦，祝你编程愉快！🎉💻 (≧▽≦) —艾瑞卡




