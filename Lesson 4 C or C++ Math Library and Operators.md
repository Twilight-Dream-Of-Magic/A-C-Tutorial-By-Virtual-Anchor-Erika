# Lesson 4: C/C++ Math Library and Operators

## Section 1: Mathematical Operations and Arithmetic Operators

In this section, we will learn how to perform basic mathematical operations using arithmetic operators in C/C++. We will also review the type range restrictions we covered in lesson 2 and how they affect the results of our calculations.

### Section 1-1: Mathematical Operations and the Type Range Restrictions We Covered in Lesson 2

We can use arithmetic operators to perform addition, subtraction, multiplication, division and modulo (remainder) operations on numeric values. The arithmetic operators are:

| Operator | Description | Example |
| :---: | :--- | :--- |
| + | Adds two operands | x + y |
| - | Subtracts the second operand from the first | x - y |
| * | Multiplies two operands | x * y |
| / | Divides the first operand by the second | x / y |
| % | Computes the remainder of dividing the first operand by the second | x % y |

The result of an arithmetic operation depends on the types of the operands. For example, if both operands are integers, the result will be an integer as well. However, if one or both operands are floating-point numbers, the result will be a floating-point number as well. This can affect the precision and accuracy of our calculations.

For example, consider the following code:

```cpp
int a = 5;
int b = 2;
double c = 5.0;
double d = 2.0;

std::cout << a / b << std::endl; // prints 2
std::cout << c / d << std::endl; // prints 2.5
```
In the first expression, both operands are integers, so the result is an integer as well. The fractional part of the division is discarded, and only the quotient is returned. In the second expression, both operands are doubles, so the result is a double as well. The fractional part of the division is preserved, and the exact value is returned.
We also need to be aware of the range restrictions of different types. For example, an int can store values from -2147483648 to 2147483647 (assuming 4 bytes), while a double can store values from approximately -1.7E308 to 1.7E308 (assuming 8 bytes). If we perform an arithmetic operation that results in a value outside of these ranges, we may encounter overflow or underflow errors.

For example, consider the following code:
```cpp
int x = 2147483647;
int y = x + 1;
std::cout << y << std::endl; // prints -2147483648
```
  In this case, adding one to the maximum value of an int causes an overflow error. The result cannot be stored in an int variable, so it wraps around to the minimum value of an int instead.
  To avoid these errors, we need to choose the appropriate type for our variables and expressions based on their expected values and precision requirements.

### Section 1-2: Using Arithmetic Operators Correctly
To use arithmetic operators correctly, we need to follow some rules and conventions:
 - We need to use parentheses to group expressions and control the order of evaluation. The order of precedence for arithmetic operators is:
 - Parentheses ()
 - Multiplication *, division / and modulo %
 - Addition + and subtraction -

  For example, consider the following expression:

```cpp
std::cout << 2 + 3 * 4 << std::endl; // prints 14
```
In this case, the multiplication is performed before the addition, because it has higher precedence. If we want to change the order of evaluation, we need to use parentheses:


```cpp
std::cout << (2 + 3) * 4 << std::endl; // prints 20
```

 - We need to be careful when using integer division and modulo operations. Integer division always returns an integer quotient, even if the operands are not evenly divisible. Modulo operation always returns an integer remainder, even if the operands are negative. For example:

```cpp
std::cout << 7 / 2 << std::endl; // prints 3
std::cout << -7 / 2 << std::endl; // prints -3
std::cout << 7 % 2 << std::endl; // prints 1
std::cout << -7 % 2 << std::endl; // prints -1
```

 - We need to use type casting to convert values from one type to another when necessary. Type casting can be done using either C-style syntax or C++-style syntax. 
 
For example:
```cpp
int a = 5;
double b = (double) a; // C-style type casting
double c = static_cast<double>(a); // C++-style type casting
```
- Type casting can be useful when we want to perform floating-point division with integer operands, or when we want to store the result of an arithmetic operation in a different type.

### Section 1-3: Using Arithmetic Operators Incorrectly
There are some common mistakes and pitfalls when using arithmetic operators that we need to avoid:
 - We should not use floating-point numbers to compare equality, because they may not be exact due to rounding errors. 
 
For example, consider the following code:
```cpp
double x = 0.1;
double y = x * 10;

if (y == 1.0) {
    std::cout << "Equal" << std::endl;
} else {
    std::cout << "Not equal" << std::endl;
}
```
  In this case, we may expect the output to be "Equal", because 0.1 times 10 should be equal to 1.0. However, the output may be "Not equal", because the value of x may not be exactly 0.1, but something like 0.10000000000000001, due to the limitations of binary representation of floating-point numbers. To compare floating-point numbers, we should use a small tolerance value instead of exact equality. 
  
For example:
```cpp
double x = 0.1;
double y = x * 10;
double epsilon = 0.00001; // a small tolerance value

if (abs(y - 1.0) < epsilon) {
    std::cout << "Equal" << std::endl;
} else {
    std::cout << "Not equal" << std::endl;
}
```

 - We should not use integer division when we want to perform floating-point division, because it may result in loss of precision and accuracy. For example, consider the following code:

```cpp
int a = 5;
int b = 2;

std::cout << a / b << std::endl; // prints 2
```
  In this case, we may want to get the result of 2.5, but we get 2 instead, because integer division discards the fractional part. To perform floating-point division, we need to either use floating-point operands or type cast one or both operands to a floating-point type. 
  
For example:
```
int a = 5;
int b = 2;

std::cout << static_cast<double>(a) / b << std::endl; // prints 2.5
```

 - We should not use modulo operation with negative operands, because it may result in unexpected or undefined behavior. For example, consider the following code:


```cpp
std::cout << -7 % 2 << std::endl; // prints -1
```
  In this case, we may expect the output to be 1, because the remainder of dividing -7 by 2 should be positive. However, the output is -1, because the sign of the remainder is determined by the sign of the dividend in C/C++. This behavior may vary depending on the compiler or platform, so it is better to avoid using modulo operation with negative operands.

## Section 2: Logical and Bitwise Operators
In this section, we will learn how to perform logical and bitwise operations on boolean and integer values using logical and bitwise operators in C/C++. We will also briefly introduce the role of logical operations in flow control and loops, which will be covered in more detail in lesson 4.

### Section 2-1: Logical Operators
Logical operators are used to perform logical operations on boolean values or expressions that evaluate to boolean values. The logical operators are:
Operator  	Description  	Example  	
&&  	Logical AND: returns true if both operands are true  	x && y  	
||  	Logical OR: returns true if either operand is true  	x || y  	
!  	Logical NOT: returns true if the operand is false  	!x  	

The result of a logical operation is always a boolean value (true or false). For example, consider the following code:

```cpp
bool a = true;
bool b = false;

std::cout << a && b << std::endl; // prints false
std::cout << a || b << std::endl; // prints true
std::cout << !a << std::endl; // prints false
```
Logical operators can also be used with expressions that evaluate to boolean values, such as comparison operators or conditional expressions. Comparison operators are used to compare two values and return a boolean value based on the result of the comparison. The comparison operators are:
| Operator | Description                                          | Example |
|----------|------------------------------------------------------|---------|
| ==       | Equal to: returns true if the operands are equal      | x == y  |
| !=       | Not equal to: returns true if the operands are not equal | x != y  |
| >        | Greater than: returns true if the left operand is greater than the right operand | x > y   |
| <        | Less than: returns true if the left operand is less than the right operand | x < y   |
| >=       | Greater than or equal to: returns true if the left operand is greater than or equal to the right operand | x >= y  |
| <=       | Less than or equal to: returns true if the left operand is less than or equal to the right operand | x <= y  |
	

For example, consider the following code:

```cpp
int x = 10;
int y = 20;

cout << (x == y) << endl; // prints false
cout << (x != y) << endl; // prints true
cout << (x > y) << endl; // prints false
cout << (x < y) << endl; // prints true
cout << (x >= y) << endl; // prints false
cout << (x <= y) << endl; // prints true
```
We can use logical operators to combine multiple comparison operators or expressions to form more complex logical expressions. 

For example, we can use logical operators to check if a number is in a certain range:
```cpp
int x = 15;

cout << (x > 10) && (x < 20) << endl; // prints true
```
In this case, the expression (x > 10) && (x < 20) evaluates to true if and only if x is greater than 10 and less than 20.
Logical operators follow the order of precedence:
 - Parentheses ()
 - Logical NOT !
 - Logical AND &&
 - Logical OR ||

For example, consider the following expression:
```cpp
cout << !a && b || c << endl;
```
In this case, the logical NOT is performed first, then the logical AND, then the logical OR. If we want to change the order of evaluation, we need to use parentheses:

```cpp
cout << !(a && b) || c << endl;
```
Logical operators also have a feature called short-circuit evaluation. This means that the second operand of a logical AND or a logical OR is evaluated only if necessary. 

For example, consider the following expression:
```cpp
cout << a || b << endl;
```
If a is true, then the expression is true regardless of the value of b. Therefore, b is not evaluated at all. This can save some computation time and avoid potential errors. 

For example, consider the following code:
```cpp
int x = 10;
int y = 0;

cout << (y != 0) && (x / y > 0) << endl;
```
If y is zero, then the expression (y != 0) is false, and the logical AND returns false without evaluating the second operand. This avoids a division by zero error that would occur if we tried to evaluate (x / y > 0).
Logical operators are useful for controlling the flow of execution and creating loops in C/C++. We will learn more about these topics in lesson 5.

### Section 2-2: Bitwise Operators
Bitwise operators are used to perform bitwise operations on integer values or expressions that evaluate to integer values.
The bitwise operators are:
| Operator | Description | Example |
| :---: | :--- | :--- |
| & | Bitwise AND: performs a logical AND on each pair of bits | x & y |
| \| | Bitwise OR: performs a logical OR on each pair of bits | x \| y |
| ^ | Bitwise XOR: performs a logical XOR on each pair of bits | x ^ y |
| ~ | Bitwise NOT: performs a logical NOT on each bit | ~x |
| << | Bitwise left shift: shifts the bits to the left by a specified number of positions | x << n |
| >> | Bitwise right shift: shifts the bits to the right by a specified number of positions | x >> n |

The result of a bitwise operation is always an integer value. To understand how bitwise operators work, we need to know how integers are represented in binary format. Binary format is a way of writing numbers using only two symbols: 0 and 1. Each symbol is called a bit, and each group of eight bits is called a byte. 

For example, the decimal number 42 can be written in binary as:
```
00101010
```
This means that 42 is equal to:

```
0 * 2^7 + 0 * 2^6 + 1 * 2^5 + 0 * 2^4 + 1 * 2^3 + 0 * 2^2 + 1 * 2^1 + 0 * 2^0
= 0 + 0 + 32 + 0 + 8 + 0 + 2 + 0
= 42
```
We can use bitwise operators to manipulate the individual bits of an integer value. For example, consider the following code:

```cpp
int x = 42; // binary: 00101010
int y = 15; // binary: 00001111

cout << (x & y) << endl; // prints 10
cout << (x | y) << endl; // prints 47
cout << (x ^ y) << endl; // prints 37
cout << (~x) << endl; // prints -43
cout << (x << 1) << endl; // prints 84
cout << (x >> 1) << endl; // prints 21
```

The bitwise AND operator (&) performs a logical AND on each pair of bits from x and y. The result is a new integer value whose bits are set to one if and only if both corresponding bits from x and y are one. For example:

```
  x: 00101010
& y: 00001111
-------------
  z: 00001010
```
The bitwise OR operator (|) performs a logical OR on each pair of bits from x and y. The result is a new integer value whose bits are set to one if either or both corresponding bits from x and y are one. For example:

```
  x: 00101010
| y: 00001111
-------------
  z: 00101111
```
The bitwise XOR operator (^) performs a logical XOR on each pair of bits from x and y. The result is a new integer value whose bits are set to one if exactly one of the corresponding bits from x and y is one. For example:

```
  x: 00101010
^ y: 00001111
-------------
  z: 00100101
```
The bitwise NOT operator (~) performs a logical NOT on each bit of x. The result is a new integer value whose bits are inverted from x. For example:

```
  x: 00101010
~ x: 11010101
```
The bitwise left shift operator (<<) shifts the bits of x to the left by a specified number of positions, n. The result is a new integer value whose bits are shifted left by n positions, and the vacated bits on the right are filled with zeros. For example:

```
x: 00101010
x << 1: 01010100
x << 2: 10101000
x << 3: 01010000
```
The bitwise right shift operator (>>) shifts the bits of x to the right by a specified number of positions, n. The result is a new integer value whose bits are shifted right by n positions, and the vacated bits on the left are filled with zeros or ones depending on the sign of x. For example:

```
x: 00101010
x >> 1: 00010101
x >> 2: 00001010
x >> 3: 00000101
```
Bitwise operators can be useful for performing low-level operations on data, such as encryption, compression, masking, etc. However, they can also be tricky and error-prone, so we need to use them with caution and care.

## Section 3: How to use the numeric limit range in c and the numeric limit range in c++?

The numeric limit range is the range of values that a numeric type can represent. For example, the numeric limit range of an int type is from -2147483648 to 2147483647 on most platforms.
To query the numeric limit range of a numeric type in C and C++, we can use the <limits.h> header file in C or the <climits> header file in C++.
These header files define various constants and macros that represent the minimum and maximum values of different numeric types. 

For example:
```c
#include <limits.h>
#include <stdio.h>

int main() {
    printf("The minimum value of int is %d\n", INT_MIN);
    printf("The maximum value of int is %d\n", INT_MAX);
    printf("The minimum value of long is %ld\n", LONG_MIN);
    printf("The maximum value of long is %ld\n", LONG_MAX);
    return 0;
}
```
This program prints:
```
The minimum value of int is -2147483648
The maximum value of int is 2147483647
The minimum value of long is -2147483648
The maximum value of long is 2147483647
```
Some of the constants and macros defined in <limits.h> or <climits>
 are:
| Constant/Macro | Description                           | Example                                 |
|----------------|---------------------------------------|-----------------------------------------|
| CHAR_BIT       | Number of bits in a char              | 8                                       |
| SCHAR_MIN      | Minimum value of a signed char         | -128                                    |
| SCHAR_MAX      | Maximum value of a signed char         | 127                                     |
| UCHAR_MAX      | Maximum value of an unsigned char      | 255                                     |
| CHAR_MIN       | Minimum value of a char                | -128 or 0                               |
| CHAR_MAX       | Maximum value of a char                | 127 or 255                              |
| SHRT_MIN       | Minimum value of a short               | -32768                                  |
| SHRT_MAX       | Maximum value of a short               | 32767                                   |
| USHRT_MAX      | Maximum value of an unsigned short     | 65535                                   |
| INT_MIN        | Minimum value of an int                | -2147483648                             |
| INT_MAX        | Maximum value of an int                | 2147483647                              |
| UINT_MAX       | Maximum value of an unsigned int       | 4294967295                              |
| LONG_MIN       | Minimum value of a long                | -2147483648 or -9223372036854775808     |
| LONG_MAX       | Maximum value of a long                | 2147483647 or 9223372036854775807      |
| ULONG_MAX      | Maximum value of an unsigned long      | 4294967295 or 18446744073709551615     |
 	

Note that some of these constants and macros may vary depending on the platform and the compiler. For example, the size and range of a long type may be different on a 32-bit system and a 64-bit system.
To query the numeric limit range of a numeric type in C++, we can also use the <limits> header file, which defines a template class called std::numeric_limits. This class provides various static member functions and constants that represent the properties and limits of different numeric types. 

For example:
```cpp
#include <iostream>
#include <limits>

int main() {
    std::cout << "The minimum value of int is " << std::numeric_limits<int>::min() << "\n";
    std::cout << "The maximum value of int is " << std::numeric_limits<int>::max() << "\n";
    std::cout << "The minimum value of double is " << std::numeric_limits<double>::min() << "\n";
    std::cout << "The maximum value of double is " << std::numeric_limits<double>::max() << "\n";
    return 0;
}
```

This program prints:
```
The minimum value of int is -2147483648
The maximum value of int is 2147483647
The minimum value of double is 2.22507e-308
The maximum value of double is 1.79769e+308
```
Some of the static member functions and constants defined in std::numeric_limits are:
Function/Constant  	Description  	Example  	
| Function           | Description                                                                           | Example                                      |
|--------------------|---------------------------------------------------------------------------------------|----------------------------------------------|
| min()              | Returns the minimum finite value representable by the type                           | std::numeric_limits<int>::min()              |
| max()              | Returns the maximum finite value representable by the type                           | std::numeric_limits<int>::max()              |
| lowest()           | Returns the lowest finite value representable by the type (may be different from min() for floating-point types) | std::numeric_limits<double>::lowest()    |
| epsilon()          | Returns the difference between 1 and the smallest value greater than 1 that is representable by the type (for floating-point types) | std::numeric_limits<float>::epsilon() |
| round_error()      | Returns the largest possible rounding error (for floating-point types)               | std::numeric_limits<double>::round_error()  |
| infinity()         | Returns a representation of positive infinity, if available (for floating-point types)| std::numeric_limits<float>::infinity()      |
| quiet_NaN()        | Returns a representation of a quiet (non-signaling) "Not a Number", if available (for floating-point types) | std::numeric_limits<double>::quiet_NaN() |
| signaling_NaN()    | Returns a representation of a signaling "Not a Number", if available (for floating-point types) | std::numeric_limits<long double>::signaling_NaN() |
| denorm_min()       | Returns the minimum positive normalized value, if available (for floating-point types)| std::numeric_limits<float>::denorm_min()   |
| is_signed          | A constant expression of type bool, true if the type is signed, false otherwise       | std::numeric_limits<int>::is_signed         |
| is_integer         | A constant expression of type bool, true if the type is integer, false otherwise      | std::numeric_limits<char>::is_integer       |
| is_exact           | A constant expression of type bool, true if the type uses exact representation, false otherwise (all integer types are exact, but not all exact types are integer) | std::numeric_limits<bool>::is_exact  |
| has_infinity       | A constant expression of type bool, true if the type can represent positive infinity, false otherwise | std::numeric_limits<double>::has_infinity |
| has_quiet_NaN      | A constant expression of type bool, true if the type can represent quiet (non-signaling) "Not a Number", false otherwise | std::numeric_limits<float>::has_quiet_NaN |
| has_signaling_NaN  | A constant expression of type bool, true if the type can represent signaling "Not a Number", false otherwise | std::numeric_limits<long double>::has_signaling_NaN |
| has_denorm         | A constant expression of type bool, true if the type can represent subnormal values, false otherwise (for floating-point types) | std::numeric_limits<double>::has_denorm |
  	

Note that some of these functions and constants may vary depending on the platform and the compiler. For example, some platforms may not support infinity or NaN values for floating-point types.

**statement:
About the use of type number limit range in c++, if we pursue the details, it will involve the knowledge of templates as well as functions and classes. So we don't go into too much detail here, we just use this tool to introduce the limits of the c++ type number system.**

## Section 4: Why are the mathematical operations in C++ different from the ones we usually see?

Integer overflow and underflow errors occur when an integer value is too large or too small to be represented by the data type that stores it. For example, if we use a 32-bit signed integer type, such as int, the range of values it can hold is from -2147483648 to 2147483647. If we try to store a value outside of this range, such as 2147483648 or -2147483649, we may encounter an overflow or underflow error.
Integer overflow and underflow errors are not detected or reported by C/C++, but they can cause unexpected or incorrect results in our calculations. 

For example, consider the following code:
```
int x = 2147483647;
int y = x + 1;

cout << y << endl; // prints -2147483648
```
In this case, adding one to the maximum value of an int causes an overflow error. The result cannot be stored in an int variable, so it wraps around to the minimum value of an int instead.
Integer overflow and underflow errors are related to the modulo arithmetic that C/C++ integers follow. Modulo arithmetic is a system of arithmetic where numbers wrap around after reaching a certain value, called the modulus. For example, modulo 12 arithmetic is used for clock arithmetic, where 12 is the modulus and the numbers wrap around after reaching 12. 

For example:
```
10 + 4 = 2 (mod 12)
11 + 3 = 2 (mod 12)
12 + 2 = 2 (mod 12)
```
Similarly, C/C++ integers follow modulo 2^n arithmetic, where n is the number of bits used to store the integer. For example, for a 32-bit signed integer type, n is 32 and the modulus is 2^32. Therefore, any calculation that results in a value outside of the range -2^31 ... 2^31 - 1 will wrap around after reaching the modulus. 

For example:
```
2147483647 + 1 = -2147483648 (mod 2^32)
-2147483648 - 1 = 2147483647 (mod 2^32)
```
To avoid integer overflow and underflow errors, we need to choose the appropriate data type for our variables and expressions based on their expected values and precision requirements. We also need to check for potential overflow or underflow conditions before performing arithmetic operations that may cause them. 

For example, we can use some of the following techniques:
 - Use a larger data type, such as long or long long, if we need to store larger values than int can hold.

 - Use an unsigned data type, such as unsigned int or unsigned long, if we only need to store positive values. Unsigned data types have twice the range of signed data types with the same number of bits.

 - Use type casting to convert values from one type to another when necessary. Type casting can be done using either C-style syntax or C++-style syntax. 
 
 
For example:
```
int a = 5;
double b = (double) a; // C-style type casting
double c = static_cast<double>(a); // C++-style type casting
```

 - Use conditional statements to check for overflow or underflow conditions before performing arithmetic operations. 
 
For example:
```cpp
int x = ...; // some value
int y = ...; // some value

// check for overflow before adding x and y
if (x > INT_MAX - y) {
    // handle overflow error
} else {
    // perform addition safely
    int z = x + y;
}

// check for underflow before subtracting x and y
if (x < INT_MIN + y) {
    // handle underflow error
} else {
    // perform subtraction safely
    int z = x - y;
}
```

 - Use built-in functions or libraries that can detect or prevent overflow or underflow errors. For example:
 - GCC provides some built-in functions that can perform arithmetic operations with overflow checking, such as __builtin_add_overflow, __builtin_sub_overflow, etc.
 - C++17 provides some standard library functions that can perform arithmetic operations with overflow checking, such as std::add_overflow, std::sub_overflow, etc.
 - There are also some third-party libraries that can provide safe integer types or operations, such as SafeInt or Boost.SafeNumerics.

As for why C/C++ has two division methods, I assume you are referring to the difference between integer division and floating-point division. Integer division is performed when both operands are integers, and it returns an integer quotient, discarding the fractional part. Floating-point division is performed when one or both operands are floating-point numbers, and it returns a floating-point value, preserving the fractional part.
The reason why C/C++ has two division methods is because they have different purposes and trade-offs. Integer division is faster and simpler than floating-point division, but it may result in loss of precision and accuracy. Floating-point division is more accurate and precise than integer division, but it may be slower and more complex. Depending on the context and the requirements of the problem, we may choose one or the other.

For example, if we want to calculate how many times a number can be divided by another number without any remainder, we can use integer division. 
```cpp
int x = 10;
int y = 2;

cout << x / y << endl; // prints 5
```
In this case, we don't care about the fractional part of the division, we only want to know how many times y fits into x.
However, if we want to calculate the ratio or the percentage of two numbers, we may want to use floating-point division. 

For example:
```cpp
int x = 10;
int y = 3;

cout << static_cast<double>(x) / y << endl; // prints 3.33333
```
In this case, we care about the fractional part of the division, we want to know the exact value of x divided by y.

Actually, the behavior of division in C++ depends on the types of the operands involved. Here are some details that beginners should know about division in C++:

1. Integer Division: When dividing two integers, C++ performs integer division, which discards the fractional part and rounds towards zero. It means that the result of integer division is always truncated (rounded down) towards zero. 

For example:
```cpp
int result = 9 / 2;  // result will be 4, not 4.5
```

2. Floating-Point Division: When dividing at least one floating-point number, C++ performs floating-point division, which retains the fractional part. The result is the quotient with the full precision of the floating-point type. It does not automatically round to the nearest decimal. 

For example:
```cpp
double result = 9.0 / 2.0;  // result will be 4.5, not 4
```

3. Mixed-Type Division: When dividing an integer by a floating-point number, C++ promotes the integer to a floating-point type before performing the division. The result will be a floating-point value. 

For example:
```cpp
double result = 9 / 2.0;  // result will be 4.5, not 4
```

It's important to understand these rules because they determine the behavior of division operations in C++. Depending on the desired outcome, you may need to use explicit rounding or conversion functions if you want to round the result to a specific decimal place.

## Operator precedence
Operator precedence determines the grouping of terms in an expression, which affects how an expression is evaluated. Certain operators have higher precedence than others; for example, the multiplication operator has higher precedence than the addition operator.

Here's a list of C++ operators from highest precedence to lowest:

1. `::` - Scope resolution
2. `a++ a--` - Suffix/postfix increment and decrement
   `type() type{}` - Functional cast
   `a()` - Function call
   `a[]` - Subscript
   `. ->` - Member access
3. `++a --a` - Prefix increment and decrement
   `+a -a` - Unary plus and minus
   `! ~` - Logical NOT and bitwise NOT
   `(type)` - C-style cast
   `*a` - Indirection (dereference)
   `&a` - Address-of
   `sizeof` - Size-of
   `new new[]` - Dynamic memory allocation
   `delete delete[]` - Dynamic memory deallocation
4. `.* ->*` - Pointer-to-member
5. `a*b a/b a%b` - Multiplication, division, and remainder
6. `a+b a-b` - Addition and subtraction
7. `<< >>` - Bitwise left shift and right shift
8. `<=>` - Three-way comparison operator (since C++20)
9. `< <= > >=` - For relational operators
10. `== !=` - For equality operators
11. `a&b` - Bitwise AND
12. `^` - Bitwise XOR (exclusive or)
13. `|` - Bitwise OR (inclusive or)
14. `&&` - Logical AND
15. `||` - Logical OR
16. `a?b:c` - Ternary conditional
   `=` - Direct assignment
   `+= -=` - Compound assignment by sum and difference
   `*= /= %=` - Compound assignment by product, quotient, and remainder
   `<<= >>=` - Compound assignment by bitwise left shift and right shift
   `&= ^= |=` - Compound assignment by bitwise AND, XOR, and OR
17. `,` - Comma

Operators with the same precedence are bound to their arguments in the direction of their binding. For example, the expression `a = b = c` resolves to `a = (b = c)` instead of `(a = b) = c` because of the right-to-left binding nature of the assignment. But `a + b - c` resolves to `(a + b) - c` instead of `a + (b - c)` because of the left-to-right union of addition and subtraction.

Remember that operator precedence is not affected by operator overloading of classes.
For example, `std::cout << a ? b : c;` resolves to `(std::cout << a) ? b : c;`, because `operator << `(console output) has higher precedence than the conditional operators.

This is just a basic overview; there are more details and subtleties to consider when you write complex expressions.

---

# 第四课：C/C++ 数学库和运算符

## 第1节：数学运算和算术运算符

在这个部分，我们将学习如何使用C/C++的算术运算符进行基本的数学运算。我们也会回顾第二课中讨论过的类型范围限制以及它们如何影响我们计算的结果。

### 第1-1节：数学运算和第二课中讲过的类型范围限制

我们可以使用算术运算符进行加、减、乘、除以及求模（余数）等数值运算。算术运算符有：

| 运算符 | 描述 | 示例 |
| :---: | :--- | :--- |
| + | 对两个操作数进行加法运算 | x + y |
| - | 从第一个操作数中减去第二个操作数 | x - y |
| * | 对两个操作数进行乘法运算 | x * y |
| / | 用第一个操作数除以第二个操作数 | x / y |
| % | 计算用第一个操作数除以第二个操作数的余数 | x % y |

一个算术运算的结果依赖于操作数的类型。例如，如果两个操作数都是整数，那么结果也将是一个整数。然而，如果一个或两个操作数是浮点数，那么结果也将是一个浮点数。这可能影响我们计算的精度和准确性。

例如，考虑以下的代码：

```cpp
int a = 5;
int b = 2;
double c = 5.0;
double d = 2.0;

std::cout << a / b << std::endl; // 打印 2
std::cout << c / d << std::endl; // 打印 2.5
```
在第一个表达式中，两个操作数都是整数，因此结果也是整数。除法的小数部分被丢弃，只返回商。在第二个表达式中，两个操作数都是浮点数，因此结果也是浮点数。除法的小数部分被保留，返回精确值。

我们还需要注意不同类型的范围限制。例如，一个int可以存储从-2147483648到2147483647的值（假设4字节），而一个double可以存储从大约-1.7E308到1.7E308的值（假设8字节）。如果我们进行一个算术运算，结果在这些范围之外，我们可能会遇到溢出或下溢错误。

例如，考虑以下的代码：

```cpp
int x = 2147483647;
int y = x + 1;
std::cout << y << std::endl; // 打印 -2147483648
```
在这个例子中，将一个加到int的最大值上导致了溢出错误。结果无法

存储在int变量中，所以它回滚到了int的最小值。
为了避免这些错误，我们需要根据它们预期的值和精度需求选择合适的类型来表示我们的变量和表达式。

### 第1-2节：正确使用算术运算符

为了正确使用算术运算符，我们需要遵循一些规则和约定：

- 我们需要使用括号来分组表达式并控制计算顺序。算术运算符的优先级顺序是：
   - 括号 ()
   - 乘法 *，除法 / 和求模 %
   - 加法 + 和减法 -

例如，考虑以下的表达式：

```cpp
std::cout << 2 + 3 * 4 << std::endl; // 打印 14
```
在这个例子中，因为乘法的优先级高于加法，所以乘法在加法之前执行。如果我们想改变计算顺序，我们需要使用括号：

```cpp
std::cout << (2 + 3) * 4 << std::endl; // 打印 20
```

- 我们需要小心使用整数除法和模运算。整数除法总是返回一个整数商，即使操作数不能被整除。模运算总是返回一个整数余数，即使操作数是负数。

例如：
```cpp
std::cout << 7 / 2 << std::endl; // 打印 3
std::cout << -7 / 2 << std::endl; // 打印 -3
std::cout << 7 % 2 << std::endl; // 打印 1
std::cout << -7 % 2 << std::endl; // 打印 -1
```

- 我们需要在必要的时候使用类型转换来将值从一种类型转换到另一种类型。类型转换可以使用C风格的语法或者C++风格的语法。

例如：
```cpp
int a = 5;
double b = (double) a; // C风格类型转换
double c = static_cast<double>(a); // C++风格类型转换
```
- 当我们希望进行浮点数除法运算时或者我们希望将算术运算的结果存储在另一种类型中时，类型转换可以很有用。

### 第1-3节：错误地使用算术运算符
在使用算术运算符时，有一些常见的错误和陷阱我们需要避免：

- 我们不应该使用浮点数进行相等性比较，因为由于舍入错误，它们可能不会精确。例如，考虑以下的代码：

```cpp
double x = 0.1;
double y = x * 10;

if (y == 1.0) {
    std::cout << "相等" << std::endl;
} else {
    std::cout << "不相等" << std::endl;
}
```
  在这个例子中，我们可能预期输出为 "相等"，因为0.1乘以10应该等于1.0。然而，输出可能是 "不相等"，因为x的值可能不是精确的0.1，而是类似于0.10000000000000001，这是由于浮点数的二进制表示的限制。
  对于浮点数的比较，我们应该使用一个小的容忍值，而不是精确的相等。
  
例如：
```cpp
double x = 0.1;
double y = x * 10;
double epsilon = 0.00001; // 一个小的容忍值

if (abs(y - 1.0) < epsilon) {
    std::cout << "相等" << std::endl;
} else {
    std::cout << "不相等" << std::endl;
}
```

- 当我们想要进行浮点数除法时，我们不应该使用整数除法，因为它可能导致精度和准确性的丢失。

例如，考虑以下的代码：
```cpp
int a = 5;
int b = 2;

std::cout << a / b << std::endl; // 打印 2
```
  在这个例子中，我们可能想得到的结果是2.5，但是我们得到的是2，因为整数除法会丢弃小数部分。要进行浮点数除法，我们需要使用浮点数操作数，或者将一个或两个操作数类型转换为浮点数。
  
例如：
```
int a = 5;
int b = 2;

std::cout << static_cast<double>(a) / b << std::endl; // 打印 2.5
```

- 我们不应该使用负操作数进行模运算，因为它可能导致预期之外的或未定义的行为。

例如，考虑以下的代码：
```cpp
std::cout << -7 % 2 << std::endl; // 打印 -1
```
  在这个例子中，我们可能预期输出为1，因为-7除以2的余数应该是正的。然而，输出是-1，因为在C/C++中，余数的符号由被除数的符号决定。这种行为可能会根据编译器或平台的不同而有所不同，所以最好避免使用负操作数进行模运算。

## 第2节：逻辑和位运算符
在这一部分，我们将学习如何使用C/C++中的逻辑和位运算符对布尔值和整数值进行逻辑和位运算。我们还将简要介绍逻辑运算在流程控制和循环中的作用，这些内容将在第4课详细讲解。

### 第2-1节：逻辑运算符
逻辑运算符用于对布尔值或计算结果为布尔值的表达式进行逻辑运算。逻辑运算符包括：
运算符  	描述  	例子  	
&&  	逻辑与：如果两个操作数都为真，则返回真  	x && y  	
||  	逻辑或：如果任一操作数为真，则返回真  	x || y  	
!  	逻辑非：如果操作数为假，则返回真  	!x  	

逻辑运算的结果总是一个布尔值（真或假）。例如，考虑以下代码：

```cpp
bool a = true;
bool b = false;

std::cout << a && b << std::endl; // 打印 false
std::cout << a || b << std::endl; // 打印 true
std::cout << !a << std::endl; // 打印 false
```
逻辑运算符也可以用于计算结果为布尔值的表达式，如比较运算符或条件表达式。比较运算符用于比较两个值，并根据比较的结果返回一个布尔值。比较运算符包括：
| 运算符 | 描述                                          | 例子 |
|----------|------------------------------------------------------|---------|
| ==       | 等于：如果操作数相等，则返回真      | x == y  |
| !=       | 不等于：如果操作数不相等，则返回真 | x != y  |
| >        | 大于：如果左操作数大于右操作数，则返回真 | x > y   |
| <        | 小于：如果左操作数小于右操作数，则返回真 | x < y   |
| >=       | 大于或等于：如果左操作数大于或等于右操作数，则返回真 | x >= y  |
| <=       | 小于或等于：如果左操作数小于或等于右操作数，则返回真 | x <= y  |

例如，考虑以下代码：

```cpp
int x = 10;
int y = 20;

cout << (x == y) << endl; // 打印 false
cout << (x != y) << endl; // 打印 true
cout << (x > y) << endl; // 打印 false
cout << (x < y) << endl; // 打印 true
cout << (x >= y) << endl; // 打印 false
cout << (x <= y) << endl; // 打印 true
```
我们可以使用逻辑运算符结合多个比较运算符或表达式，形成更复杂的逻辑表达式。例如，我们可以

使用逻辑运算符检查一个数是否在某个范围内：

```cpp
int x = 15;

cout << (x > 10) && (x < 20) << endl; // 打印 true
```
在这种情况下，表达式(x > 10) && (x < 20)只有当x大于10且小于20时才为真。
逻辑运算符遵循运算符优先级：
 - 圆括号 ()
 - 逻辑非 !
 - 逻辑与 &&
 - 逻辑或 ||

例如，考虑以下表达式：
```cpp
cout << !a && b || c << endl;
```
在这种情况下，首先进行逻辑非运算，然后是逻辑与运算，最后是逻辑或运算。

如果我们想改变运算顺序，需要使用圆括号：
```cpp
cout << !(a && b) || c << endl;
```
逻辑运算符还有一个被称为短路求值的特性。这意味着逻辑与或逻辑或的第二个操作数只在必要时才会被求值。

例如，考虑以下表达式：
```cpp
cout << a || b << endl;
```
如果a为真，那么无论b的值是什么，表达式都为真。因此，b根本不会被求值。这可以节省一些计算时间并避免可能的错误。

例如，考虑以下代码：
```cpp
int x = 10;
int y = 0;

cout << (y != 0) && (x / y > 0) << endl;
```
如果y为零，那么表达式(y != 0)为假，并且逻辑与在不求值第二个操作数的情况下返回假。这避免了如果我们试图求值(x / y > 0)时可能会出现的除零错误。
逻辑运算符在C/C++中用于控制执行流程和创建循环非常有用。我们将在第5课更详细地学习这些主题。

### 第2-2节：位运算符
位运算符用于对整数值或计算结果为整数值的表达式进行位运算。位运算符包括：

| 运算符 | 描述 | 示例 |
| :---: | :--- | :--- |
| & | 位与：对每一对比特进行逻辑与运算 | x & y |
| \| | 位或：对每一对比特进行逻辑或运算 | x \| y |
| ^ | 位异或：对每一对比特进行逻辑异或运算 | x ^ y |
| ~ | 位非：对每个比特进行逻辑非运算 | ~x |
| << | 位左移：将比特向左移动指定的位置数 | x << n |
| >> | 位右移：将比特向右移动指定的位置数 | x >> n |

位运算的结果总是一个整数值。要理解位运算符如何工作，我们需要知道整数如何以二进制格式表示。二进制格式是一种使用两个符号：0和1来编写数字的方式。每个符号被称为一个比特，每八个比特被称为一个字节。

例如，十进制数字42可以用二进制表示为：
```
00101010
```
这意味着42等于：

```
0 * 2^7 + 0 * 2^6 + 1 * 2^5 + 0 * 2^4 + 1 * 2^3 + 0 * 2^2 + 1 * 2^1 + 0 * 2^0
= 0 + 0 + 32 + 0 + 8 + 0 + 2 + 0
= 42
```
我们可以使用位运算符操作整数值的单个比特。

例如，考虑以下代码：
```cpp
int x = 42; // 二进制：00101010
int y = 15; // 二进制：00001111

cout << (x & y) << endl; // 打印 10
cout << (x | y) << endl; // 打印 47
cout << (x ^ y) << endl; // 打印 37
cout << (~x) << endl; // 打印 -43
cout << (x << 1) << endl; // 打印 84
cout << (x >> 1) << endl; // 打印 21
```

位与运算符（&）对x和y的每一对比特执行逻辑与运算。结果是一个新的整数值，其比特只有在x和y的相应比特都为一时才设为一。

例如：
```
  x: 00101010
& y: 00001111
-------------
  z: 00001010
```
位或运算符（|）对x和y的每一对比特执行逻辑或运算。结果是一个新的整数值，其比特只要x和y的相应比特中有一个或两个为一就设为一

例如：
```
  x: 00101010
| y: 00001111
-------------
  z: 00101111
```
位异或运算符（^）对x和y的每一对比特执行逻辑异或运算。结果是一个新的整数值，其比特只有在x和y的相应比特中恰好有一个为一时才设为一

例如：
```
  x: 00101010
^ y: 00001111
-------------
  z: 00100101
```
位非运算符（~）对x的每个比特执行逻辑非运算。结果是一个新的整数值，其比特是x的反码。

例如：
```
  x: 00101010
~ x: 11010101
```
位左移运算符（<<）将x的比特向左移动指定的位置数n。结果是一个新的整数值，其比特向左移动n个位置，右侧的空位用零填充。

例如：
```
x: 00101010
x << 1: 01010100
x << 2: 10101000
x << 3: 01010000
```
位右移运算符（>>）将x的比特向右移动指定的位置数n。结果是一个新的整数值，其比特向右移动n个位置，左侧的空位根据x的符号用零或者一填充。

例如：
```
x: 00101010
x >> 1: 00010101
x >> 2: 00001010
x >> 3: 00000101
```
位运算符可以用于对数据执行低级操作，如加密、压缩、掩码等。然而，它们也可能复杂且容易出错，因此我们需要谨慎使用。

## 第3节：如何使用C和C++中的数字限制范围？

数字限制范围是一个数值类型可以表示的值的范围。例如，int类型的数字限制范围在大多数平台上是从-2147483648到2147483647。

要在C和C++中查询数字类型的数字限制范围，我们可以使用C中的<limits.h>头文件或者C++中的<climits>头文件。这些头文件定义了各种常量和宏，它们表示不同数字类型的最小和最大值。

例如：
```c
#include <limits.h>
#include <stdio.h>

int main() {
    printf("The minimum value of int is %d\n", INT_MIN);
    printf("The maximum value of int is %d\n", INT_MAX);
    printf("The minimum value of long is %ld\n", LONG_MIN);
    printf("The maximum value of long is %ld\n", LONG_MAX);
    return 0;
}
```

这个程序会输出：
```
The minimum value of int is -2147483648
The maximum value of int is 2147483647
The minimum value of long is -2147483648
The maximum value of long is 2147483647
```
在<limits.h>或<climits>中定义的一些常量和宏包括：

| 常量/宏 | 描述                           | 示例                                 |
|----------------|---------------------------------------|-----------------------------------------|
| CHAR_BIT       | char的位数              | 8                                       |
| SCHAR_MIN      | signed char的最小值         | -128                                    |
| SCHAR_MAX      | signed char的最大值         | 127                                     |
| UCHAR_MAX      | unsigned char的最大值      | 255                                     |
| CHAR_MIN       | char的最小值                | -128 或 0                               |
| CHAR_MAX       | char的最大值                | 127 或 255                              |
| SHRT_MIN       | short的最小值               | -32768                                  |
| SHRT_MAX       | short的最大值               | 32767                                   |
| USHRT_MAX      | unsigned short的最大值     | 65535                                   |
| INT_MIN        | int的最小值                | -2147483648                             |
| INT_MAX        | int的最大值                | 2147483647                              |
| UINT_MAX       | unsigned int的最大值       | 4294967295                              |
| LONG_MIN       | long的最小值                | -2147483648 或 -9223372036854775808     |
| LONG_MAX       | long的最大值                | 2147483647 或 9223372036854775807      |
| ULONG_MAX      | unsigned long的最大值      | 4294967295 或 18446744073709551615     |

注意，这些常量和宏可能因平台和编译器的不同而不同。例如，long类型在32位系统和64位系统上的大小和范围可能会不同。

要在C++中查询数字类型的数字限制范围，我们也可以使用<limits>头文件，它定义了一个叫做std::numeric_limits的模板类。这个类提供了各种静态成员函数和

常量，它们表示不同数字类型的属性和限制。

例如：
```cpp
#include <iostream>
#include <limits>

int main() {
    std::cout << "The minimum value of int is " << std::numeric_limits<int>::min() << "\n";
    std::cout << "The maximum value of int is " << std::numeric_limits<int>::max() << "\n";
    std::cout << "The minimum value of double is " << std::numeric_limits<double>::min() << "\n";
    std::cout << "The maximum value of double is " << std::numeric_limits<double>::max() << "\n";
    return 0;
}
```
这个程序会输出：
```
The minimum value of int is -2147483648
The maximum value of int is 2147483647
The minimum value of double is 2.22507e-308
The maximum value of double is 1.79769e+308
```
std::numeric_limits中定义的一些静态成员函数和常量包括：

| 函数/常量        | 描述                                                                           | 示例                                      |
|---------------------|---------------------------------------------------------------------------------------|----------------------------------------------|
| min()               | 返回该类型可以表示的最小有限值                          | std::numeric_limits<int>::min()              |
| max()               | 返回该类型可以表示的最大有限值                          | std::numeric_limits<int>::max()              |
| lowest()            | 返回该类型可以表示的最低有限值 (对于浮点类型，可能与min()不同) | std::numeric_limits<double>::lowest()    |
| epsilon()           | 返回1与该类型可以表示的大于1的最小值之间的差 (对于浮点类型) | std::numeric_limits<float>::epsilon() |
| round_error()       | 返回最大可能的舍入误差 (对于浮点类型)               | std::numeric_limits<double>::round_error()  |
| infinity()          | 如果可用，返回正无穷的表示 (对于浮点类型)| std::numeric_limits<float>::infinity()      |
| quiet_NaN()         | 如果可用，返回一个静默（非信号）的“非数字”的表示 (对于浮点类型) | std::numeric_limits<double>::quiet_NaN() |
| signaling_NaN()     | 如果可用，返回一个信号“非数字”的表示 (对于浮点类型) | std::numeric_limits<long double>::signaling_NaN() |
| denorm_min()        | 如果可用，返回最小的正规化值 (对于浮点类型)| std::numeric_limits<float>::denorm_min()   |
| is_signed           | bool类型的常量表达式，如果类型是有符号的，则为真，否则为假       | std::numeric_limits<int>::is_signed         |
| is_integer          | bool类型的常量表达式，如果类型是整数的，则为真，否则为假      | std::numeric_limits<char>::is_integer       |
| is_exact            | bool类型的常量表达式，如果类型使用精确表示，则为真，否则为假 (所有的整数类型都是精确的，但并非所有精确的类型都是整数) | std::numeric_limits<bool>::is_exact  |
| has_infinity        | bool类型的常量表达式，如果类型可以表示正无穷，则为真，否则为假 | std::numeric_limits<double>::has_infinity |
| has_quiet_NaN       | bool类型的常量表达式，如果类型可以表示静默（非信号）的“非数字”，则为真，否则为假 | std::numeric_limits<float>::has_quiet_NaN |
| has_signaling_NaN   | bool类型的常量表达式，如果类型可以表示信号“非数字”，则为真，否则为假 | std::numeric_limits<long double>::has_signaling_NaN |
| has_denorm          | bool类型的常量表达式，如果类型可以表示次正规值，则为真，否则为假 (对于浮点类型) | std::numeric_limits<double>::has_denorm |

注意，这些函数和常量可能因平台和编译器的不同而不同。例如，有些平台可能不支持浮点类型的无穷大或NaN值。

**声明：关于C++中类型数字限制范围的使用，如果我们追求细节，它将涉及到模板以及函数和类的知识。所以我们在这里不做过多的详细介绍，只是用这个工具来介绍C++类型数字系统的限制。**

## 第4节：为什么C++中的数学运算与我们通常看到的不同？

整数溢出和下溢错误是当一个整数值太大或太小以至于无法被存储的数据类型所表示时发生的。例如，如果我们使用一个32位的有符号整数类型，比如int，它可以存储的值的范围是从-2147483648到2147483647。如果我们尝试存储超出这个范围的值，比如2147483648或-2147483649，我们可能会遇到溢出或下溢错误。
C/C++不会检测或报告整数溢出和下溢错误，但它们可能会导致我们的计算结果出现意外或错误。

例如，考虑下面的代码：
```cpp
int x = 2147483647;
int y = x + 1;

cout << y << endl; // 输出 -2147483648
```
在这种情况下，给int的最大值加一会导致溢出错误。结果无法存储在int变量中，所以它回绕到int的最小值。

整数溢出和下溢错误与C/C++整数遵循的模数算术有关。模数算术是一种算术系统，其中的数字在达到一个特定的值（称为模数）后回绕。例如，模12的算术用于时钟算术，其中12是模数，数字在达到12后回绕。

例如：
```
10 + 4 = 2 (mod 12)
11 + 3 = 2 (mod 12)
12 + 2 = 2 (mod 12)
```
同样，C/C++的整数遵循模2^n算术，其中n是用来存储整数的位数。例如，对于32位有符号整数类型，n是32，模数是2^32。因此，任何导致超出范围-2^31 ... 2^31 - 1的计算结果都会在达到模数后回绕。

例如：
```
2147483647 + 1 = -2147483648 (mod 2^32)
-2147483648 - 1 = 2147483647 (mod 2^32)
```
为了避免整数溢出和下溢错误，我们需要根据他们预期的值和精度要求，选择适当的数据类型来表示我们的变量和表达式。我们也需要在执行可能导致它们的算术运算之前，检查潜在的溢出或下溢条件。

例如，我们可以使用以下一些技巧：
 - 如果我们需要存储比int可以容纳的更大的值，可以使用更大的数据类型，如long或long long。

 - 如果我们只需要存储正值，可以使用无符号数据类型，如unsigned int或unsigned long。具有相同位数的无符号数据类型的范围是有符号数据类型的两倍。

 - 在需要时，使用类型转换将值从一种类型转换为另一种类型。类型转换可以使用C风格的语法或C++风格的语法。
 
例如：
```cpp
int a = 5;
double b = (double) a; // C风格类型转换
double c = static_cast<double>(a); // C++风格类型转换
```

 - 使用条件语句在执行算术运算之前检查溢出或下溢条件。
 
例如：
```cpp
int x = ...; // some value
int y = ...; // some value

// before adding x and y, check for overflow
if (x > INT_MAX - y) {
    // handle overflow error
} else {
    // safely perform addition
    int z = x + y;
}

// before subtracting x and y, check for underflow
if (x < INT_MIN + y) {
    // handle underflow error
} else {
    // safely perform subtraction
    int z = x - y;
}
```

 - 使用可以检测或防止溢出或下溢错误的内建函数或库。例如：
   - GCC提供了一些内建函数，可以执行带有溢出检查的算术运算，如__builtin_add_overflow、__builtin_sub_overflow等。
   - C++17提供了一些标准库函数，可以执行带有溢出检查的算术运算，如std::add_overflow、std::sub_overflow等。
   - 还有一些第三方库可以提供安全的整数类型或操作，如SafeInt或Boost.SafeNumerics。

至于为什么C/C++有两种除法方法，我猜你是在问整数除法和浮点除法的区别。当两个运算数都是整数时，执行整数除法，返回整数商，舍去小数部分。当一个或两个运算数是浮点数时，执行浮点除法，返回浮点值，保留小数部分。
C/C++有两种除法方法的原因是因为它们有不同的用途和权衡。整数除法比浮点除法更快、更简单，但可能会导致精度和准确性的损失。浮点除法比整数除法更准确、精确，但可能会慢一些，更复杂。根据上下文和问题的要求，我们可以选择其中一个。

例如，如果我们想计算一个数可以被另一个数整除多少次，我们可以使用整数除法。
```cpp
int x = 10;
int y = 2;

cout << x / y << endl; // 输出 5
```
在这种情况下，我们不关心除法的小数部分，我们只想知道y可以被x整除多少次。
然而，如果我们想计算两个数的比例或百分比，我们可能想使用浮点除法。

例如：
```cpp
int x = 10;
int y = 3;

cout << static_cast<double>(x) / y << endl; // 输出 3.33333
```
在这种情况下，我们关心除法的小数部分，我们想知道x除以y的确切值。

C++初学者需要了解的一个小细节是，在整数除法中会向下取整。对于浮点数除法，要四舍五入到最接近的小数位。

实际上，C++中的除法行为取决于所涉及操作数的类型。以下是C++中关于除法的一些初学者应该了解的细节：

1. 整数除法：当两个整数相除时，C++执行整数除法，舍弃小数部分并朝向零进行舍入。这意味着整数除法的结果总是向零截断（向下舍入）。

例如：
```cpp
int result = 9 / 2;  // 结果将是4，而不是4.5
```

2. 浮点数除法：当至少有一个浮点数参与除法时，C++执行浮点数除法，保留小数部分。结果是具有浮点类型的完整精度的商。它不会自动四舍五入到最接近的小数位。

例如：
```cpp
double result = 9.0 / 2.0;  // 结果将是4.5，而不是4
```

3. 混合类型除法：当将整数除以浮点数时，C++会将整数提升为浮点类型，然后执行除法。结果将是浮点值。

例如：
```cpp
double result = 9 / 2.0;  // 结果将是4.5，而不是4
```

理解这些规则很重要，因为它们决定了C++中除法操作的行为。根据所需的结果，如果要将结果四舍五入到特定的小数位数，可能需要使用显式的四舍五入或转换函数。

## 运算符优先级
运算符优先级决定了表达式中项的分组，这影响了表达式的求值方式。某些运算符的优先级比其他运算符高；例如，乘法运算符的优先级比加法运算符高。

以下是从最高优先级到最低优先级的C++运算符列表：

1. `::` - 作用域解析
2. `a++ a--` - 后缀/后置递增和递减
   `type() type{}` - 函数式转换
   `a()` - 函数调用
   `a[]` - 下标
   `. ->` - 成员访问
3. `++a --a` - 前缀递增和递减
   `+a -a` - 一元正和负
   `! ~` - 逻辑非和位非
   `(type)` - C风格转换
   `*a` - 间接引用（解引用）
   `&a` - 地址
   `sizeof` - 大小
   `new new[]` - 动态内存分配
   `delete delete[]` - 动态内存释放
4. `.* ->*` - 成员指针
5. `a*b a/b a%b` - 乘法、除法和取余
6. `a+b a-b` - 加法和减法
7. `<< >>` - 位左移和位右移
8. `<=>` - 三向比较运算符（自C++20起）
9. `< <= > >=` - 关系运算符
10. `== !=` - 等于运算符
11. `a&b` - 位与
12. `^` - 位异或
13. `|` - 位或
14. `&&` - 逻辑与
15. `||` - 逻辑或
16. `a?b:c` - 三元条件
   `=` - 直接赋值
   `+= -=` - 通过求和和差的复合赋值
   `*= /= %=` - 通过乘积、商和余数的复合赋值
   `<<= >>=` - 通过位左移和位右移的复合赋值
   `&= ^= |=` - 通过位与、位异或和位或的复合赋值
17. `,` - 逗号

具有相同优先级的运算符按照它们的结合性方向绑定到它们的参数。例如，表达式 `a = b = c` 解析为 `a = (b = c)`，而不是 `(a = b) = c`，因为赋值的右到左结合性。但是 `a + b - c` 解析为 `(a + b) - c` 而不是 `a + (b - c)`，因为加法和减法的左到右结合性。

请记住，运算符优先级不受类的运算符重载的影响。
例如，`std::cout << a ? b : c;` 解析为 `(std::cout << a) ? b : c;`，因为`operator<<`(console output)的优先级高于条件运算符。

这只是一个基本概述，当你编写复杂表达式时，还需要考虑更多的细节和微妙之处。