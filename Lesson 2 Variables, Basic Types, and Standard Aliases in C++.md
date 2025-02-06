# Lesson 2: Variables, Basic Types, and Standard Aliases in C++

## Variables in C++

A variable provides us with named storage that our programs can manipulate. Each variable in C++ has a type, which determines the size and layout of the variable's memory; the range of values that can be stored within that memory; and the set of operations that can be applied to the variable.

## Basic Types in C++

C++ provides several basic types, including integers, floating-point numbers, characters, and booleans. 

### Integer Types

C++ provides several integer types, including `short`, `int`, `long`, and `long long`. The `int` type is usually the machine word size, such as 32 bits on a typical modern machine. The `short` and `long` types are, respectively, no longer and no shorter than `int`. The `long long` type is at least as large as `long`.

### Floating-Point Types

There are three floating-point types: `float`, `double`, and `long double`. Usually `float` is 32 bits, `double` is 64 bits, and `long double` is 96 or 128 bits.

### Character Types

C++ has three character types: `char`, `wchar_t`, `char16_t`, and `char32_t`. The type `char` is exactly one byte and can be used to hold a character in the basic character set. The `wchar_t` type is a wide character type. The `char16_t` and `char32_t` types are used for Unicode characters.

### Boolean Type

The `bool` type represents the truth values. It can have one of two values: `true` or `false`.

## Standard Aliases for Basic Types

C++ provides a set of typedefs that match the definitions of types defined in the C header `<cstdint>`. These types provide a way to request integer types of a specific size. For example, `uint8_t` is an 8-bit unsigned integer, `uint16_t` is a 16-bit unsigned integer, `uint32_t` is a 32-bit unsigned integer, and `uint64_t` is a 64-bit unsigned integer.

For more details, you can refer to the [C++ reference](https://en.cppreference.com/w/cpp/language/types) on cppreference.com.

```
#include <iostream>
#include <cstdint>

int main() {
    // Integer types
    int a = 10;
    short b = 5;
    long c = 1000;
    long long d = 1000000;

    // Floating-point types
    float e = 1.23f;
    double f = 3.14159;
    long double g = 0.123456789L;

    // Character types
    char h = 'H';
    wchar_t i = L'我';

    // Boolean type
    bool j = true;

    // Standard aliases for basic types
    uint8_t k = 255;
    uint16_t l = 65535;
    uint32_t m = 4294967295;
    uint64_t n = 18446744073709551615U;

    // Print the size of each variable
    std::cout << "Size of int: " << sizeof(a) << " bytes\n";
    std::cout << "Size of short: " << sizeof(b) << " bytes\n";
    std::cout << "Size of long: " << sizeof(c) << " bytes\n";
    std::cout << "Size of long long: " << sizeof(d) << " bytes\n";
    std::cout << "Size of float: " << sizeof(e) << " bytes\n";
    std::cout << "Size of double: " << sizeof(f) << " bytes\n";
    std::cout << "Size of long double: " << sizeof(g) << " bytes\n";
    std::cout << "Size of char: " << sizeof(h) << " bytes\n";
    std::cout << "Size of wchar_t: " << sizeof(i) << " bytes\n";
    std::cout << "Size of bool: " << sizeof(j) << " bytes\n";
    std::cout << "Size of uint8_t: " << sizeof(k) << " bytes\n";
    std::cout << "Size of uint16_t: " << sizeof(l) << " bytes\n";
    std::cout << "Size of uint32_t: " << sizeof(m) << " bytes\n";
    std::cout << "Size of uint64_t: " << sizeof(n) << " bytes\n";

    return 0;
}
```

This program declares variables of different types, assigns values to them, and then uses the sizeof operator to print the size of each variable in bytes. The sizeof operator returns the size of a variable or a type in bytes.

### Range of Variables
The range of a variable in C++ depends on its type and whether it is signed or unsigned. Here are the ranges for the basic types:

- char: -128 to 127 (signed) or 0 to 255 (unsigned)
- short: -32,768 to 32,767 (signed) or 0 to 65,535 (unsigned)
- int: -2,147,483,648 to 2,147,483,647 (signed) or 0 to 4,294,967,295 (unsigned)
- long: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (signed) or 0 to 18,446,744,073,709,551,615 (unsigned)
- float: Approximately ±3.4E-38 to ±3.4E+38
- double: Approximately ±1.7E-308 to ±1.7E+308
- bool: false (0) or true (1)
- The exact range can depend on the system and compiler. For more details, you can refer to the C++ reference on cppreference.com.

### Signed and Unsigned
In C++, integer types can be either signed or unsigned.

Signed integers can represent both positive and negative numbers. If you don't specify signed or unsigned when declaring an integer variable, it will be signed by default.

Unsigned integers can only represent non-negative numbers (i.e., zero and positive numbers). They are often used when the variable should never have a negative value, such as the size of an array.

Here's an example of how signed and unsigned work:

```cpp
signed int a = -10;  // This is okay
unsigned int b = -10;  // This will not give an error, but will not result in -10
```
In the second line, we're trying to assign a negative number to an unsigned integer. This won't give a compile error, but b will actually be assigned a very large positive number due to how unsigned integers handle negative numbers.

Remember, it's important to choose the right type for your variables to prevent unexpected behavior.

`I hope this makes the content more accessible for beginners. Happy learning!`

---

# 第2课：C++的变量、基本类型和标准别名

## C++中的变量

变量为我们提供了我们的程序可以操作的命名存储。C++中的每个变量都有一个类型，它决定了变量内存的大小和布局；可以存储在该内存中的值的范围；以及可以应用于变量的操作集。

## C++的基本类型

C++提供了几种基本类型，包括整数、浮点数、字符和布尔值。 

### 整数类型

C++提供了几种整数类型，包括`short`、`int`、`long`和`long long`。`int`类型通常是机器字大小，比如在典型的现代机器上是32位。`short`和`long`类型分别不长于和不短于`int`。`long long`类型至少和`long`一样大。

### 浮点类型

有三种浮点类型：`float`、`double`和`long double`。通常`float`是32位，`double`是64位，`long double`是96或128位。

### 字符类型

C++有三种字符类型：`char`、`wchar_t`、`char16_t`和`char32_t`。`char`类型恰好是一个字节，可以用来存储基本字符集中的一个字符。`wchar_t`类型是一种宽字符类型。`char16_t`和`char32_t`类型用于Unicode字符。

### 布尔类型

`bool`类型表示真值。它可以有两个值：`true`或`false`。

## 基本类型的标准别名

C++提供了一组typedefs，与C头文件`<cstdint>`中定义的类型的定义相匹配。这些类型提供了一种请求特定大小的整数类型的方法。例如，`uint8_t`是一个8位无符号整数，`uint16_t`是一个16位无符号整数，`uint32_t`是一个32位无符号整数，`uint64_t`是一个64位无符号整数。

更多详情，您可以参考cppreference.com上的[C++参考](https://zh.cppreference.com/w/cpp/language/types)。

```cpp
#include <iostream>
#include <cstdint>

int main() {
    // 整数类型
    int a = 10;
    short b = 5;
    long c = 1000;
    long long d = 1000000;

    // 浮点类型
    float e = 1.23f;
    double f = 3.14159;
    long double g = 0.123456789L;

    // 字符类型
    char h = 'H';
    wchar_t i = L'我';

    // 布尔类型
    bool j = true;

    // 基本类型的标准别名
    uint8_t k = 255;
    uint16_t l = 65535;
    uint32_t m = 4294967295;
    uint64_t n = 18446744073709551615U;

    // 打印每个变量的大小
    std::cout << "Size of int: " << sizeof(a) << " bytes\n";
    std::cout << "Size of short: " << sizeof(b) << " bytes\n";
    std::cout << "Size of long: " << sizeof(c) << " bytes\n";
    std::cout << "Size of long long: " << sizeof(d) << " bytes\n";
    std::cout << "Size of float: " << sizeof(e) << " bytes\n";
    std::cout << "Size of double: " << sizeof(f) << " bytes\n";
    std::cout << "Size of long double: " << sizeof(g) << " bytes\n";
    std::cout << "Size of char: " << sizeof(h) << " bytes\n";
    std::cout << "Size of wchar_t: " << sizeof(i) << " bytes\n";
    std::cout << "Size of bool: " << sizeof(j) << " bytes\n";
    std::cout << "Size of uint8_t: " << sizeof(k) << " bytes\n";
    std::cout << "Size of uint16_t: " << sizeof(l) << " bytes\n";
    std::cout << "Size of uint32_t: " << sizeof(m) << " bytes\n";
    std::cout << "Size of uint64_t: " << sizeof(n) << " bytes\n";

    return 0;
}
```

这个程序声明了不同类型的变量，为它们赋值，然后使用sizeof运算符打印每个变量的大小（以字节为单位）。sizeof运算符返回变量或类型的大小（以字节为单位）。

### 变量的范围
C++中变量的范围取决于其类型以及它是有符号还是无符号的。以下是基本类型的范围：

- char：-128到127（有符号）或0到255（无符号）
- short：-32,768到32,767（有符号）或0到65,535（无符号）
- int：-2,147,483,648到2,147,483,647（有符号）或0到4,294,967,295（无符号）
- long：-9,223,372,036,854,775,808到9,223,372,036,854,775,807（有符号）或0到18,446,744,073,709,551,615（无符号）
- float：大约±3.4E-38到±3.4E+38
- double：大约±1.7E-308到±1.7E+308
- bool：false（0）或true（1）
确切的范围可能取决于系统和编译器。有关更多详情，您可以参考cppreference.com上的C++参考。

### 有符号和无符号
在C++中，整数类型可以是有符号的或无符号的。

有符号整数可以表示正数和负数。如果在声明整数变量时没有指定有符号或无符号，那么默认情况下它将是有符号的。

无符号整数只能表示非负数（即零和正数）。它们通常用于变量不应具有负值的情况，例如数组的大小。

下面是有符号和无符号的工作示例：

```cpp
signed int a = -10;  // 这是可以的
unsigned int b = -10;  // 这不会报错，但结果不会是-10
```
在第二行中，我们试图将一个负数分配给一个无符号整数。这不会引发编译错误，但由于无符号整数如何处理负数，b实际上将被分配一个非常大的正数。

请记住，选择正确的类型用于您的变量以防止出现意外行为。

`希望这能让内容对初学者更易理解，祝学习愉快！`