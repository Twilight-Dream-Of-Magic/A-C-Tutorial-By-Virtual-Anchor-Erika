# Lesson 5: C/C++ Flow Control and Loops

In this lesson, we will explore the flow control and loop structures in C/C++. These structures allow us to control the flow of our program, making it more dynamic and flexible. We will be referring to the official C++ documentation at `cppreference.com` throughout this lesson.

## Flow Control

Flow control in C/C++ is managed using conditional statements. These statements allow the program to make decisions based on certain conditions. The primary flow control statements in C/C++ are `if`, `if-else`, and `switch`.

### if Statement

The `if` statement is used to execute a block of code only if a specified condition is true.

```cpp
if (condition) {
    // code to be executed if condition is true
}
```

### if-else Statement (Like an `a ? b : c` ternary operator)

The `if-else` statement is used to execute one block of code if a specified condition is true, and another block of code if the condition is false.

```cpp
if (condition) {
    // code to be executed if condition is true
} else {
    // code to be executed if condition is false
}
```

### if-else if-else Statement

The `if-else if-else` statement is used to execute one block of code among many, depending on the condition that is true. If none of the conditions are true, the code in the `else` block is executed.

```cpp
if (condition1) {
    // code to be executed if condition1 is true
} else if (condition2) {
    // code to be executed if condition1 is false and condition2 is true
} else {
    // code to be executed if both condition1 and condition2 are false
}
```

### switch Statement

The `switch` statement is used to select one of many code blocks to be executed.

```cpp
switch(expression) {
    case constant1:
        // code to be executed if expression is constant1
        break;
    case constant2:
        // code to be executed if expression is constant2
        break;
    default:
        // code to be executed if expression doesn't match any constants
}
```
```cpp
switch(expression) {
    case constant1:
    {
        // code to be executed if expression is constant1
        break;
    }
    case constant2:
    {
        // code to be executed if expression is constant2
        break;
    }
    default:
    {
        // code to be executed if expression doesn't match any constants
    }
}
```

## Loops

Loops in C/C++ are used to repeatedly execute a block of code as long as a specified condition is true. The primary loop structures in C/C++ are `for`, `while`, and `do-while`.

### for Loop

The `for` loop is used when you know in advance how many times the script should run.

```cpp
for (initialization; condition; increment) {
    // code to be executed for each loop
}
```

### while Loop

The `while` loop is used when you want to loop through a block of code an unknown number of times, as long as a specified condition is true.

```cpp
while (condition) {
    // code to be executed as long as condition is true
}
```

### do-while Loop

The `do-while` loop is a variant of the `while` loop, which tests the condition at the end of the loop. This means that the loop will always be executed at least once, even if the condition is false.


```cpp
do {
    // code to be executed
} while (condition);
```

## Flow Control and Loops code example
```cpp
#include <iostream>

int main() {
    // if statement
    int x = 10;
    if (x > 0) {
        std::cout << "x is positive.\n";
    }

    // if-else statement
    if (x % 2 == 0) {
        std::cout << "x is even.\n";
    } else {
        std::cout << "x is odd.\n";
    }

    // if-else if-else statement
    if (x > 10) {
        std::cout << "x is greater than 10.\n";
    } else if (x < 10) {
        std::cout << "x is less than 10.\n";
    } else {
        std::cout << "x is equal to 10.\n";
    }

    // switch statement
    int y = 2;
    switch (y) {
        case 1:
            std::cout << "y is 1.\n";
            break;
        case 2:
            std::cout << "y is 2.\n";
            break;
        default:
            std::cout << "y is neither 1 nor 2.\n";
    }

    // for loop
    for (int i = 0; i < 5; i++) {
        std::cout << "This is loop iteration " << i << ".\n";
    }

    // while loop
    int j = 0;
    while (j < 5) {
        std::cout << "This is while loop iteration " << j << ".\n";
        j++;
    }

    // do-while loop
    int k = 0;
    do {
        std::cout << "This is do-while loop iteration " << k << ".\n";
        k++;
    } while (k < 5);

    return 0;
}
```

```cpp
#include <iostream>

int main() {
    // Outer loop
    for (int i = 1; i <= 3; i++) {
        std::cout << "Starting outer loop iteration " << i << ".\n";

        // Inner loop
        for (int j = 1; j <= 5; j++) {
            // Conditional control in the inner loop
            if (j % 2 == 0) {
                std::cout << "  Inner loop iteration " << j << " is even.\n";
            } else {
                std::cout << "  Inner loop iteration " << j << " is odd.\n";
            }
        }

        std::cout << "Ending outer loop iteration " << i << ".\n";
    }

    return 0;
}
```

# "goto" Statement

In addition to the flow control and loop structures discussed in Lesson 5, there is another statement called the "goto" statement in C/C++. The "goto" statement allows you to transfer control directly to a labeled statement within the same function.

The syntax of the "goto" statement is as follows:

```cpp
goto label;
```

Here, "label" is an identifier followed by a colon (:), which marks a specific point in your code.

```cpp
label:
    // code to be executed
```

When the "goto" statement is encountered, the control of the program jumps to the labeled statement indicated by "label".

## When to Use the "goto" Statement

In modern programming practice, the use of the "goto" statement is usually discouraged because it makes code harder to read and understand. However, there are a few situations where the "goto" statement can be useful:

1. **Error handling**: When an error occurs, you can use the "goto" statement to jump to the error handling section of the code. This helps avoid duplicate code or deeply nested if-else statements.

2. **Exiting nested loops**: Sometimes you may need to exit multiple nested loops early. You can avoid complex control structures by using the "goto" statement to jump directly to a label outside the nested loop.

3. **Specific Complex Control Programs**: When we are in three or more loops and we want to jump out of the inner loop, but not the outer loop, and we want to re-execute the intermediate level loop steps, we need to use the goto statement. This gives us more fine-grained control over program flow in complex loop structures.

```cpp
#include <iostream>

int main() {
    bool flag = true;
    for (int i = 0; i < 3; i++) {
        repeat_j:
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 3; k++) {
                if (i == 2 && j == 2 && k == 1 && flag) {
                    std::cout << "repeat j" << std::endl;
                    flag = false; 
                    goto repeat_j;
                }
                std::cout << "i: " << i << ", j: " << j << ", k: " << k << "\n";
            }
        }
    }
}
```

In this example, when `i` equals 2 and `j` equals 2 and `k` equals 1 and `flag` is true, the `goto` statement causes the program to jump to the `repeat_j` tag, effectively jumping out of the innermost loop and re-executing the intermediate loop. This demonstrates how the `goto` statement can be used in specialized complex control programs.

It's important to note that the "goto" statement should be used sparingly and with caution. Misuse of the "goto" statement can lead to spaghetti code and make your program difficult to understand and maintain. Whenever possible, consider alternative control structures or refactoring your code to eliminate the need for "goto".

## When Not to Use the "goto" Statement

In general, you should avoid using the "goto" statement unless there is a compelling reason to use it, such as the situations mentioned above. Here are some reasons why you should avoid the "goto" statement:

1. **Code readability**: The use of "goto" can make the control flow of your code more difficult to understand, especially when used inappropriately or excessively. It can lead to code that is hard to read, debug, and maintain.

2. **Structured programming**: The "goto" statement violates the principles of structured programming, which emphasizes the use of structured control flow constructs like loops and conditional statements. Using these constructs makes your code more organized and easier to comprehend.

3. **Alternative control structures**: In most cases, you can achieve the same results using alternative control structures, such as loops, conditional statements, and function calls. These structures are generally more expressive and can help make your code more modular and reusable.

4. **Language-specific features**: Depending on the programming language and paradigm you are using, there may be other constructs or patterns that provide better alternatives to the "goto" statement. For example, in C++, you can use exceptions for error handling or use higher-level constructs provided by libraries or frameworks.

By following good programming practices and using structured control flow constructs appropriately, you can write code that is easier to understand, maintain, and debug.

Remember, while the "goto" statement is available in C/C++, it should be used sparingly and only when there is a clear and compelling reason to do so.
In certain scenarios, it may be necessary for the inner loop to jump to the outer loop or to a specific iteration of the outer loop. This can be achieved using labeled statements and the "goto" statement.

Here's an example to demonstrate jumping from an inner loop to an outer loop using labeled statements:

```cpp
#include <iostream>

int main() {
    for (int i = 1; i <= 3; i++) {
        std::cout << "Starting outer loop iteration " << i << ".\n";

        for (int j = 1; j <= 5; j++) {
            if (j == 3) {
                std::cout << "  Jumping from inner loop to outer loop.\n";
                goto outerLoop; // Jump to the labeled statement "outerLoop"
            }
            std::cout << "  Inner loop iteration " << j << ".\n";
        }

        outerLoop:
        std::cout << "Ending outer loop iteration " << i << ".\n";
    }

    return 0;
}
```

In this example, we have an outer loop labeled as "outerLoop" and an inner loop. When the inner loop's counter variable, `j`, is equal to 3, we use the "goto" statement to jump to the labeled statement "outerLoop". This causes the program to continue execution from the next iteration of the outer loop.

However, it's important to exercise caution when using the "goto" statement for jumping between loops or nested structures. Excessive use of "goto" statements can make the code difficult to read and maintain. It's generally recommended to use more structured control flow constructs or to refactor the code if possible.

Remember, the "goto" statement can be a powerful tool in certain situations, but it should be used judiciously and with care.

Sure, here's an explanation of `break`, `continue`, and `goto` in multi-layer loops, supplemented in both English and Chinese:

## `break` in Multi-layer Loops

The `break` statement in C/C++ is used to terminate the innermost loop in which it is present. After that, the control will pass to the statements that are present after the broken loop. If the `break` statement is used in a nested loop, only the inner loop is terminated.

Here's an example of how `break` is used in multi-layer loops:

```cpp
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            break;
        }
        std::cout << "i: " << i << ", j: " << j << "\n";
    }
}
```

In this example, the inner loop will terminate when `j` equals 1, so for each iteration of `i`, only `j = 0` will be printed.

## `continue` in Multi-layer Loops

The `continue` statement in C/C++ is used to skip the rest of the current loop iteration and immediately start the next iteration of the loop. In the case of nested loops, the `continue` statement only affects the innermost loop.

Here's an example of how `continue` is used in multi-layer loops:

```cpp
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            continue;
        }
        std::cout << "i: " << i << ", j: " << j << "\n";
    }
}
```

In this example, when `j` equals 1, the `continue` statement causes the inner loop to skip the rest of the current iteration and immediately start the next one. So for each iteration of `i`, `j = 0` and `j = 2` will be printed, but not `j = 1`.

## `goto` in Multi-layer Loops

The `goto` statement in C/C++ allows for arbitrary jumps of control flow. It can be used to jump out of deeply nested loops, something that neither `break` nor `continue` can do.

Here's an example of how `goto` is used in multi-layer loops:

```cpp
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            goto end_of_loops;
        }
        std::cout << "i: " << i << ", j: " << j << "\n";
    }
}
end_of_loops:
std::cout << "End of loops.\n";
```

In this example, when `j` equals 1, the `goto` statement causes the program to jump to the `end_of_loops` label, effectively breaking out of both the inner and outer loop. So only `i = 0` and `j = 0` will be printed before "End of loops." is printed.

## Escape sequences

Escape sequences in C++ are used to represent certain special characters within string literals and character literals. 

Here are some of the escape sequences and their descriptions:
- `\'`: single quote, represented as byte 0x27 in ASCII encoding
- `\"`: double quote, represented as byte 0x22 in ASCII encoding
- `\?`: question mark, represented as byte 0x3f in ASCII encoding
- `\\\`: backslash, represented as byte 0x5c in ASCII encoding
- `\a`: audible bell, represented as byte 0x07 in ASCII encoding
- `\b`: backspace, represented as byte 0x08 in ASCII encoding
- `\f`: form feed - new page, represented as byte 0x0c in ASCII encoding
- `\n`: line feed - new line, represented as byte 0x0a in ASCII encoding
- `\r`: carriage return, represented as byte 0x0d in ASCII encoding
- `\t`: horizontal tab, represented as byte 0x09 in ASCII encoding
- `\v`: vertical tab, represented as byte 0x0b in ASCII encoding

There are also numeric escape sequences, such as `\nnn` for an arbitrary octal value, and `\xn...` for an arbitrary hexadecimal value. Universal character names can be represented using `\unnnn` for an arbitrary Unicode value, and `\Unnnnnnnn` for a code point.

Here's an example of using escape sequences in C++:
```cpp
#include <iostream>

int main()
{
    std::cout << "This\nis\na\ntest\n\n";
    std::cout << "She said, \"Sells she seashells on the seashore?\"\n";
}
```

This code will output:
```
This
is
a
test

She said, "Sells she seashells on the seashore?"
```

In this example, `\n` is used to create a new line, and `\"` is used to include a double quote within the string.

Remember that escape sequences are a way to include special characters in your strings that would otherwise be impossible or difficult to include. 
They can make your code more readable and flexible.

Let's consider a more complex example where we use various escape sequences in a single program. This program will demonstrate the use of escape sequences for special characters, octal and hexadecimal values, and Unicode characters.

This program will demonstrate the use of escape sequences for special characters, octal and hexadecimal values, and Unicode characters.
```cpp
#include <iostream>

int main() {
    // Using escape sequences for special characters
    std::cout << "Special characters:\n";
    std::cout << "Audible bell (\\a) - ";
    std::cout << "Hello\aWorld\n"; // You will hear a beep sound if your system supports it

    std::cout << "Backspace (\\b) - ";
    std::cout << "Hello\bWorld\n"; // The 'o' from "Hello" will be erased

    std::cout << "Form feed (\\f) - ";
    std::cout << "Hello\fWorld\n"; // Moves to the beginning of the next page

    std::cout << "New line (\\n) - ";
    std::cout << "Hello\nWorld\n"; // Moves to the next line

    std::cout << "Carriage return (\\r) - ";
    std::cout << "Hello\rWorld\n"; // Moves to the beginning of the line and overwrites "Hello" with "World"

    std::cout << "Horizontal tab (\\t) - ";
    std::cout << "Hello\tWorld\n"; // Adds a tab space between "Hello" and "World"

    std::cout << "Vertical tab (\\v) - ";
    std::cout << "Hello\vWorld\n"; // Moves to the same position on the next vertical tab stop

    // Using escape sequences for octal and hexadecimal values
    std::cout << "\nOctal and hexadecimal values:\n";
    std::cout << "Octal (\\101) - ";
    std::cout << "\101\n"; // Prints 'A' which is 101 in octal

    std::cout << "Hexadecimal (\\x41) - ";
    std::cout << "\x41\n"; // Prints 'A' which is 41 in hexadecimal

    // Using escape sequences for Unicode characters
    std::cout << "\nUnicode characters:\n";
    std::cout << "Unicode (\\u03B1) - ";
    std::cout << "\u03B1\n"; // Prints 'α' which is 03B1 in Unicode

    return 0;
}
```

This program demonstrates the use of various escape sequences in C++. 
It prints different outputs based on the escape sequences used. 
Note that the actual output may vary depending on your system and console settings, especially for escape sequences like `\a`, `\f`, `\r`, and `\v`.

---

# 第五课：C/C++ 流程控制和循环

在本课中，我们将探讨C/C++中的流程控制和循环结构。这些结构使我们能够控制程序的流程，使其更具动态性和灵活性。在本课中，我们将参考官方的C++文档 `cppreference.com`。

## 流程控制

C/C++中的流程控制是通过条件语句进行管理的。这些语句允许程序根据特定条件进行决策。C/C++中的主要流程控制语句有 `if`、`if-else` 和 `switch`。

### if 语句

`if` 语句用于仅在指定条件为真时执行一块代码。

```cpp
if (condition) {
    // 如果条件为真，则执行的代码
}
```

### if-else 语句（类似于 `a ? b : c` 三元运算符）

`if-else` 语句用于在指定条件为真时执行一块代码，并在条件为假时执行另一块代码。

```cpp
if (condition) {
    // 如果条件为真，则执行的代码
} else {
    // 如果条件为假，则执行的代码
}
```

### if-else if-else 语句

`if-else if-else` 语句用于根据真实的条件执行一块代码中的多个代码块。如果没有任何条件为真，则执行 `else` 块中的代码。

```cpp
if (condition1) {
    // 如果条件1为真，则执行的代码
} else if (condition2) {
    // 如果条件1为假且条件2为真，则执行的代码
} else {
    // 如果条件1和条件2都为假，则执行的代码
}
```

### switch 语句

`switch` 语句用于选择要执行的多个代码块中的一个。

```cpp
switch(expression) {
    case constant1:
        // 如果表达式等于 constant1，则执行的代码
        break;
    case constant2:
        // 如果表达式等于 constant2，则执行的代码
        break;
    default:
        // 如果表达式与任何常量都不匹配，则执行的代码
}
```

```cpp
switch(expression) {
    case constant1:
    {
        // 如果表达式等于 constant1，则执行的代码
        break;
    }
    case constant2:
    {
        // 如果表达式等于 constant2，则执行的代码
        break;
    }
    default:
    {
        // 如果表达式与任何常量都不匹配，则执行的代码
    }
}
```

## 循环

C/C++中的循环用于在指定条件为真时重复执行一块代码。C/C++中的主要循环结构有 `for`、`while` 和 `do-while`。

### for 循环

`for` 循环用于当你事先知道脚本应该运行多少次时使用。

```cpp
for (initialization; condition; increment) {
    // 每次循环要执行的代码
}
```

### while 循环

`while` 循环用于在指定条件为真时，无限次地循环执行一块代码。

```cpp
while (condition) {
    // 只要条件为真，就要执行的代码
}
```



### do-while 循环

`do-while` 循环是 `while` 循环的一种变体，它在循环结束时测试条件。这意味着即使条件为假，循环也会至少执行一次。

```cpp
do {
    // 要执行的代码
} while (condition);
```

## 流程控制和循环 代码示例
```cpp
#include <iostream>

int main() {
    // if 语句
    int x = 10;
    if (x > 0) {
        std::cout << "x 是正数。\n";
    }

    // if-else 语句
    if (x % 2 == 0) {
        std::cout << "x 是偶数。\n";
    } else {
        std::cout << "x 是奇数。\n";
    }

    // if-else if-else 语句
    if (x > 10) {
        std::cout << "x 大于 10。\n";
    } else if (x < 10) {
        std::cout << "x 小于 10。\n";
    } else {
        std::cout << "x 等于 10。\n";
    }

    // switch 语句
    int y = 2;
    switch (y) {
        case 1:
            std::cout << "y 是 1。\n";
            break;
        case 2:
            std::cout << "y 是 2。\n";
            break;
        default:
            std::cout << "y 不是 1 也不是 2。\n";
    }

    // for 循环
    for (int i = 0; i < 5; i++) {
        std::cout << "这是循环迭代 " << i << "。\n";
    }

    // while 循环
    int j = 0;
    while (j < 5) {
        std::cout << "这是 while 循环迭代 " << j << "。\n";
        j++;
    }

    // do-while 循环
    int k = 0;
    do {
        std::cout << "这是 do-while 循环迭代 " << k << "。\n";
        k++;
    } while (k < 5);

    return 0;
}
```

```cpp
#include <iostream>

int main() {
    // 外部循环
    for (int i = 1; i <= 3; i++) {
        std::cout << "开始外部循环迭代 " << i << "。\n";

        // 内部循环
        for (int j = 1; j <= 5; j++) {
            // 内部循环中的条件控制
            if (j % 2 == 0) {
                std::cout << "  内部循环迭代 " << j << " 是偶数。\n";
            } else {
                std::cout << "  内部循环迭代 " << j << " 是奇数。\n";
            }
        }

        std::cout << "结束外部循环迭代 " << i << "。\n";
    }

    return 0;
}
```

# "goto" 语句

除了 Lesson 5 中讨论的流程控制和循环结构外，C/C++ 中还有另一种语句叫做 "goto" 语句。 "goto" 语句允许直接跳转到同一函数内的一个标记语句。

 "goto" 语句的语法如下：

```cpp
goto label;
```

这里的 "label" 是一个标识符，后面跟着一个冒号（:），用于标记代码中的特定位置。

```cpp
label:
    // 要执行的代码
```

当遇到 "goto" 语句时，程序的控制流将跳转到由 "label" 标记的语句处。

## 何时使用 "goto "语句

在现代编程实践中，通常不鼓励使用 "goto "语句，因为它会使代码更难阅读和理解。然而，在一些情况下，"goto "语句还是有用的：

1. **错误处理**： 当错误发生时，您可以使用 "goto "语句跳转到代码的错误处理部分。这有助于避免重复代码或深嵌套的if-else语句。

2. **退出嵌套循环**： 有时您可能需要提前退出多个嵌套循环。通过使用 "goto "语句直接跳转到嵌套循环之外的标签，可以避免复杂的控制结构。

3. **特定的复杂控制程序**： 当我们处于三个或三个以上的循环中，我们希望跳出内循环，但不希望跳出外循环，并且希望重新执行中间级循环步骤时，我们需要使用 "goto "语句。这样我们就可以在复杂的循环结构中对程序流程进行更精细的控制。

```cpp
#include <iostream>

int main() {
    bool flag = true;
    for (int i = 0; i < 3; i++) {
        repeat_j:
        for (int j = 0; j < 3; j++) {
            for (int k = 0; k < 3; k++) {
                if (i == 2 && j == 2 && k == 1 && flag) {
                    std::cout << "repeat j" << std::endl;
                    flag = false; 
                    goto repeat_j;
                }
                std::cout << "i: " << i << ", j: " << j << ", k: " << k << "\n";
            }
        }
    }
}
```

在这个例子中，当`i`等于2与`j`等于2与`k`等于1并且`flag`为true时，`goto`语句使程序跳转到`repeat_j`标签，有效地跳出了最内层循环并重新执行中间层循环。这演示了`goto`语句如何用于专门的复杂控制程序。

需要注意的是，应该谨慎地、适度地使用 "goto" 语句。滥用 "goto" 语句可能导致代码变得难以理解和维护。在可能的情况下，考虑使用替代的控制结构或重构代码，以消除对 "goto" 语句的需求。

## 不适合使用 "goto" 语句的情况

一般来说，除非有充分的理由使用 "goto" 语句（如上述情况），否则应该避免使用它。以下是一些避免使用 "goto" 语句的原因：

1. **代码可读性**：使用 "goto" 可能会使代码的控制流更难以理解，特别是在不适当或过度使用时。这可能导致代码难以阅读、调试和维护。

2. **结构化编程**： "goto" 语句违反了结构化编程的原则，结构化编程强调使用结构化的控制流结构，如循环和条件语句。使用这些结构使代码更有组织性，更易于理解。

3. **替代的控制结构**：在大多数情况下，可以使用替代的控制结构（如循环、条件语句和函数调用）来实现相同的结果。这些结构通常更具表达性，可以帮助使代码更模块化和可重用。

4. **语言特定特性**：根据所使用的编程语言和编程范式，可能有其他的结构或模式提供了比 "goto" 语句更好的替代方案。例如，在 C++ 中，可以使用异常处理来处理错误，或者使用库或框架提供的更高级的结构。

通过遵循良好的编程实践并适当地使用结构化的控制流结构，可以编写出更易于理解、维护和调试的代码。

请记住，虽然 C/C++ 中提供了 "goto" 语句，但应该谨慎使用，只在确实有明确且充分的理由时使用它。在某些情况下， "goto" 语句可以是一个强大的工具，但需要慎重使用。

## 多层循环中的 `break`

在 C/C++ 中，`break` 语句用于终止其中包含它的最内层循环。之后，控制将传递给破坏循环后面的语句。如果在嵌套循环中使用 `break` 语句，只有内循环会被终止。

以下是在多层循环中如何使用 `break` 的示例：

```cpp
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            break;
        }
        std::cout << "i: " << i << ", j: " << j << "\n";
    }
}
```

在此示例中，当 `j` 等于 1 时，内循环将终止，所以对于 `i` 的每次迭代，只有 `j = 0` 会被打印。

## 多层循环中的 `continue`

在 C/C++ 中，`continue` 语句用于跳过当前循环迭代的其余部分，并立即开始循环的下一次迭代。在嵌套循环的情况下，`continue` 语句只影响最内层的循环。

以下是在多层循环中如何使用 `continue` 的示例：

```cpp
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            continue;
        }
        std::cout << "i: " << i << ", j: " << j << "\n";
    }
}
```

在此示例中，当 `j` 等于 1 时，`continue` 语句会导致内循环跳过当前迭代的其余部分，并立即开始下一次迭代。所以对于 `i` 的每次迭代，`j = 0` 和 `j = 2` 会被打印，但 `j = 1` 不会。

## 多层循环中的 `goto`

C/C++ 中的 `goto` 语句允许进行任意的控制流跳转。它可以用于跳出深层嵌套的循环，这是 `break` 和 `continue` 都无法做到的。

以下是在多层循环中如何使用 `goto` 的示例：

```cpp
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            goto end_of_loops;
        }
        std::cout << "i: " << i << ", j: " << j << "\n";
    }
}
end_of_loops:
std::cout << "End of loops.\n";
```

在此示例中，当 `j` 等于 1 时，`goto` 语句会导致程序跳转到 `end_of_loops` 标签，有效地跳出了内部和外部循环。所以只有 `i = 0` 和 `j = 0` 会被打印，然后打印 "End of loops."。

## 转义序列

在C++中，转义序列用于在字符串字面量和字符字面量中表示某些特殊字符。

以下是一些转义序列及其描述：
- `\'`：单引号，以ASCII编码表示为字节0x27
- `\"`：双引号，以ASCII编码表示为字节0x22
- `\?`：问号，以ASCII编码表示为字节0x3f
- `\\`：反斜杠，以ASCII编码表示为字节0x5c
- `\a`：可听见的铃声，以ASCII编码表示为字节0x07
- `\b`：退格，以ASCII编码表示为字节0x08
- `\f`：换页符 - 新页，以ASCII编码表示为字节0x0c
- `\n`：换行符 - 新行，以ASCII编码表示为字节0x0a
- `\r`：回车，以ASCII编码表示为字节0x0d
- `\t`：水平制表符，以ASCII编码表示为字节0x09
- `\v`：垂直制表符，以ASCII编码表示为字节0x0b

还有数字转义序列，如`\nnn`表示任意八进制值，`\xn...`表示任意十六进制值。可以使用`\unnnn`表示任意Unicode值，使用`\Unnnnnnnn`表示一个代码点。

以下是在C++中使用转义序列的示例：
```cpp
#include <iostream>

int main()
{
    std::cout << "This\nis\na\ntest\n\n";
    std::cout << "She said, \"Sells she seashells on the seashore?\"\n";
}
```

这段代码的输出将是：
```
This
is
a
test

She said, "Sells she seashells on the seashore?"
```

在这个例子中，`\n`用于创建新行，`\"`用于在字符串中包含双引号。

请记住，转义序列是一种在字符串中包含特殊字符的方式，否则这些字符将无法或难以包含。它们可以使你的代码更易读和灵活。

让我们考虑一个更复杂的例子，其中我们在一个程序中使用各种转义序列。这个程序将演示如何使用转义序列表示特殊字符、八进制和十六进制值，以及Unicode字符。

```cpp
#include <iostream>

int main() {
    // 使用转义序列表示特殊字符
    std::cout << "特殊字符:\n";
    std::cout << "响铃符 (\\a) - ";
    std::cout << "Hello\aWorld\n"; // 如果你的系统支持，你会听到一声哔哔声

    std::cout << "退格符 (\\b) - ";
    std::cout << "Hello\bWorld\n"; // "Hello"中的'o'将被擦除

    std::cout << "换页符 (\\f) - ";
    std::cout << "Hello\fWorld\n"; // 移动到下一页的开始

    std::cout << "换行符 (\\n) - ";
    std::cout << "Hello\nWorld\n"; // 移动到下一行

    std::cout << "回车符 (\\r) - ";
    std::cout << "Hello\rWorld\n"; // 移动到行首并用"World"覆盖"Hello"

    std::cout << "水平制表符 (\\t) - ";
    std::cout << "Hello\tWorld\n"; // 在"Hello"和"World"之间添加一个制表符空间

    std::cout << "垂直制表符 (\\v) - ";
    std::cout << "Hello\vWorld\n"; // 移动到下一个垂直制表位的同一位置

    // 使用转义序列表示八进制和十六进制值
    std::cout << "\n八进制和十六进制值:\n";
    std::cout << "八进制 (\\101) - ";
    std::cout << "\101\n"; // 打印 'A'，它在八进制中是101

    std::cout << "十六进制 (\\x41) - ";
    std::cout << "\x41\n"; // 打印 'A'，它在十六进制中是41

    // 使用转义序列表示Unicode字符
    std::cout << "\nUnicode字符:\n";
    std::cout << "Unicode (\\u03B1) - ";
    std::cout << "\u03B1\n"; // 打印 'α'，它在Unicode中是03B1

    return 0;
}
```

这个程序演示了在C++中使用各种转义序列的用法。
它根据使用的转义序列打印不同的输出。
请注意，实际的输出可能会因你的系统和控制台设置的不同而有所不同，特别是对于像`\a`、`\f`、`\r`和`\v`这样的转义序列。