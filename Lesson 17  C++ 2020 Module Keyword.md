# Lesson 17  C++ 2020 Module Keyword

> 我是技术型虚拟主播艾瑞卡 全名是 暮光-梦想 | 安可茜得·里尔菲涅·克罗玛娜 ✨🎀(≧▽≦)
> 我的slogan是：“我是活动在虚拟数字网络世界神明的灵魂，依附在人类身体的未来独立游戏制作人。” 🌌🕹️✨ (≧ω≦)

> 这些名字将「宇宙平衡器」、「科技精炼」和「魔法能量」三大意象融为一体：

> - Axtune = Ax（轴、axis）+ tune（调谐），象征宇宙平衡器与节奏控制 🎛️⌨️⚖️🌌✨🎇
> - Rerphine ≈ Seraphine（炽天使）+ refine（精炼），代表天使优雅与科技精密 😇🛠️🤖 ，Re是（再次）的意思
> - Kromana = Chroma（色彩）+ mana（魔力），象征色彩魔法与元素能量 🌈🪄✨🎶👼🎨🔮
> 合在一起，**Axtune·Rerphine·Kromana** 就是「宇宙平衡、科技精炼、魔法能量」的化身！(≧▽≦)🎀


哈喽～这里是技术型虚拟主播艾瑞卡 📺
Lesson 18，我们直接一镜到底：上真工程级“**模块 + 命名空间**”骨架（只写层级不写实现），把“**头文件为什么会地域/污染、为什么需要 include guard 宏**”讲透，最后给出一个**最复杂但干净**的项目模板，拷走就能当骨架。

---

# 模块进度条（2020 → 2023 → 2026）

* **C++20** 把语言机关落地：`module / import / export`，**全局模块片段**（`module;`：只给预处理用，比如 `#include`），**私有模块片段**（`module :private;`），**分区**（`module M:part;` / `export module M:part;`），以及**头单元**（把传统头按实现映射为可 `import` 的“header unit”，例：`import <vector>;`）。`import` 面向**已编译接口**而非文本拼接。([C++ Reference][1])
* **C++23** 标准库提供**命名模块**：`import std;` 与 `import std.compat;`。`std` 导出 `std::` 内的名字，`std.compat` 额外再导出对应**全局命名空间**名（如 `::fopen`）以兼容旧式接口。([Microsoft Learn][2])
* **现状与工具链**（截至 2025）：MSVC 提供完整教程与开关；Clang/ libc++ 文档完善；GCC/ libstdc++ 在 15.x 代实现了 P2465R3 的 `std`/`std.compat`（留意你的发行版版本与打包）。([Microsoft Learn][2])

---

# 模块 vs 命名空间（正交而互补）

* **模块**决定**可见性/可达性**与**模块链接**（声明“归属哪个模块”、是否 `export`）。
* **命名空间**只是**名字分组**。同一命名空间可以横跨多个模块；一个模块也能包含多个命名空间。`import std;` 之后，用的仍是 `std::` 家族，只是“拿法”从 `#include` 变为 `import`。([C++ Reference][1])

---

# 为什么头文件会“地域/污染”？为什么要写 include guard 宏？

* 传统 `#include` 是**文本级拼接**：被包含文件里的宏、`using`、全局名会“扩散”到包含者的作用域，层层嵌套更严重——这就是常说的“地域性副作用/命名污染”。([C++ Reference][3])
* **多重包含**会引发重复声明/定义或 ODR 问题，所以必须用 **include guard**（或 `#pragma once`）保证**一个翻译单元只编译一次**：

  ```c
  #ifndef LIB_FOO_H
  #define LIB_FOO_H
  // declarations...
  #endif
  ```

  这是预处理阶段的**防御式编程**。([Wikipedia][4])
* 模块的改良点：`import` 获取的是**已编译接口**，宏**不会**沿 `import` 渗透；确有宏配置需求时，把 `#include` 放进**全局模块片段**（`module;` 区域）。([C++ Reference][1])

---

# 工程里“最复杂但干净”的模块骨架（只给结构、不写实现）

> 目标：在**不写任何函数/类实现**的前提下，模拟大型库的**模块切片 + 命名空间分层 + 汇总再导出**。
> 约定：模块接口文件扩展名用 `.ixx`（等价可用 `.cppm`）；`src/legacy` 专放必须通过预处理处理的旧式头或第三方宏配置。

```
/app
  └─ main.cpp
/src
  └─ modules
      ├─ acme.core.ixx                 # 顶层接口单元：含全局片段 + 接口 + 私有片段
      ├─ acme.core:logging.ixx         # 接口分区（可 export）
      ├─ acme.core:algo.ixx            # 接口分区（可 export）
      ├─ acme.core:impl.ixx            # 实现分区（不 export）
      ├─ acme.net.ixx                  # 另一个模块，依赖 acme.core
      ├─ acme.util.ixx                 # 工具模块
      ├─ acme.util:detail.ixx          # 工具模块的实现分区（不 export）
      └─ acme.ixx                      # 汇总/门面模块：对外只暴露一个入口
/src/legacy
  ├─ log_config.hpp                    # 宏配置（只能在全局片段 include）
  └─ vendor_compat.hpp                 # 旧接口适配
```

## ① 顶层接口单元 + 全局/私有片段：`acme.core.ixx`

```cpp
// file: acme.core.ixx
module;                              // 全局模块片段：仅允许预处理内容
#include "../legacy/log_config.hpp"  // 必须经宏配置的旧式头放这里

export module acme.core;             // 模块接口起点（之后方可 import）

import <string>;                     // 头单元（由实现映射，干净隔离宏）
import <vector>;

export namespace acme::core {        // 对外 API 的命名空间“门牌”
  // 这里只放声明，不写定义
}

module :private;                     // 私有模块片段：对外不可见的实现区
#include "../legacy/vendor_compat.hpp"
```

> 记忆点：**`module;` 片段内不能 `import`**，只能做预处理；`import` 要放在 `export module ...` / `module ...` 之后。([C++ Reference][1])

## ② 接口分区（可再导出）

```cpp
// file: acme.core:logging.ixx
export module acme.core:logging;
import <string>;
export namespace acme::core::logging {
  // 日志相关 API —— 只声明不实现
}
```

```cpp
// file: acme.core:algo.ixx
export module acme.core:algo;
export namespace acme::core::algo {
  // 算法层的导出声明（不写实现）
}
```

> 在**同一模块**内，可用 `import :logging;` 省略模块名前缀；外部也可直接 `import acme.core:logging;`，更多时候由**汇总模块**统一再导出。([C++ Reference][1])

## ③ 实现分区（不导出）

```cpp
// file: acme.core:impl.ixx
module acme.core:impl;               // 实现分区，不 export
namespace acme::core::detail {
  // 真正定义/细节埋在此处（本课程不写实现）
}
```

## ④ 另一个模块依赖它：`acme.net.ixx`

```cpp
// file: acme.net.ixx
export module acme.net;

import acme.core;          // 导入顶层接口单元
import acme.core:algo;     // 直接导入 core 的分区

export namespace acme::net {
  // 网络层对外 API 的声明（依赖 core 名字）
}
```

## ⑤ 工具模块与实现分区

```cpp
// file: acme.util.ixx
export module acme.util;
import <type_traits>;
export namespace acme::util {
  inline namespace v1 {     // 版本化命名空间示例
    // traits / concept 等声明
  }
}
```

```cpp
// file: acme.util:detail.ixx
module acme.util:detail;    // 不导出，仅内部实现
namespace acme::util::detail {
  // 内部细节（不对外）
}
```

## ⑥ 汇总/门面模块：`acme.ixx`

```cpp
// file: acme.ixx
export module acme;

export import acme.core;          // 再导出子模块
export import acme.core:logging;  // 精选再导出
export import acme.net;
export import acme.util;

export namespace acme {           // 统一门面（别名/入口）
  // using acme::core::Config;    // 示例：只做门面声明
}
```

## ⑦ 应用侧最小用法：`app/main.cpp`

```cpp
// file: app/main.cpp
import std;     // C++23 命名模块（依工具链/库版本开启）
import acme;    // 我们的汇总模块

int main() {
  // acme::core::... / acme::net::...
}
```

> **可用性提示**：启用 `import std;` 的具体开关、版本与路径，取决于编译器与标准库的实现版本（如 MSVC 的选项与教程、Clang 文档、libstdc++ 的版本状态）。上手前请先确认你的工具链版本与标准库包。([Microsoft Learn][2])

---

# 规则速记（写惯头文件的同学看这四条就够）

* **`module;` 只是“预处理缓冲区”**：只能 `#include`/`#define` 等；**不得** `import`。实际声明请写在 `export module ...`/`module ...` 之后。([C++ Reference][1])
* **分区是“切片”**：`export module M:part;` 可对外；`module M:impl;` 仅内部可见；**同模块内部**可用 `import :part;`。([C++ Reference][1])
* **模块与命名空间正交**：可在不同模块向同一命名空间继续增加声明；对外仍从该命名空间访问。([C++ Reference][1])
* **头单元 vs 全局片段**：`import <vector>;` 是头单元（干净、隔离宏）；确需宏配置，把 `#include "x.hpp"` 放到 **全局模块片段**。([Clang][5])

> 附带提醒（与本课主题无关但很常见）：协程在 **final suspend** 处恢复是 **UB**，只能销毁。工程里若你把协程/任务放进模块里，记得这条红线。([C++ Reference][6])

---

如果你照这个骨架搭项目，先有结构再谈内容：模块切片清晰、命名空间分层合理、宏只在全局片段“关起门来”处理，外部世界只见到经过 `export` 的干净 API。这就是“复杂但干净”的真正含义。

---

**参考链接（Markdown 版）**

* Modules（自 C++20 起）：[https://en.cppreference.com/w/cpp/language/modules.html](https://en.cppreference.com/w/cpp/language/modules.html)
* Import the C++ Standard Library as modules（MSVC 教程）：[https://learn.microsoft.com/en-us/cpp/cpp/tutorial-import-stl-named-module](https://learn.microsoft.com/en-us/cpp/cpp/tutorial-import-stl-named-module)
* Clang《Standard C++ Modules》：[https://clang.llvm.org/docs/StandardCPlusPlusModules.html](https://clang.llvm.org/docs/StandardCPlusPlusModules.html)
* `#include`（文本拼接与搜索路径）：[https://en.cppreference.com/w/cpp/preprocessor/include.html](https://en.cppreference.com/w/cpp/preprocessor/include.html)
* Include guard（百科）：[https://en.wikipedia.org/wiki/Include_guard](https://en.wikipedia.org/wiki/Include_guard)
* WG21 P2465R3（`std` 与 `std.compat`）：[https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2465r3.pdf](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2465r3.pdf)
* libstdc++ 状态表（含 `__cpp_lib_modules` 与版本）：[https://gcc.gnu.org/onlinedocs/libstdc++/manual/status.html](https://gcc.gnu.org/onlinedocs/libstdc++/manual/status.html)
* `coroutine_handle::resume` 的 UB 说明：[https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/resume.html](https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/resume.html)

[1]: https://en.cppreference.com/w/cpp/language/modules.html?utm_source=chatgpt.com "Modules (since C++20)"
[2]: https://learn.microsoft.com/en-us/cpp/cpp/tutorial-import-stl-named-module?view=msvc-170&utm_source=chatgpt.com "Tutorial: Import the C++ standard library using modules ..."
[3]: https://en.cppreference.com/w/cpp/preprocessor/include.html?utm_source=chatgpt.com "Source file inclusion - cppreference. ..."
[4]: https://en.wikipedia.org/wiki/Include_guard?utm_source=chatgpt.com "include guard"
[5]: https://clang.llvm.org/docs/StandardCPlusPlusModules.html?utm_source=chatgpt.com "Standard C++ Modules — Clang 22.0.0git documentation"
[6]: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/resume.html?utm_source=chatgpt.com "operator(), std::coroutine_handle<Promise>::resume"
