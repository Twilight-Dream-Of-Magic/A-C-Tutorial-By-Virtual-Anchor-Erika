# Lesson 3: Variables and Type casting system; Type modifiers - const, constexpr, static

**Note: Don't be afraid of the complex examples in this lesson, especially the ones that will involve classes, functions and pointer with memory address or arrays, templates, etc. 
We are just going to use these complex examples to illustrate the type system and the type modifiers/specifier we mentioned. They will not be covered in detail here.
You will understand their meaning better when you get to the more advanced chapters.**

## Section 1: Why do you need types?
Types are a way of categorizing the values that variables can hold in a program. Types determine how much memory is allocated for a variable, how the value is stored and interpreted, and what operations can be performed on it. Types also help the compiler to check for errors and optimize the code.
Some examples of types in C++ are int, double, char, bool, std::string, std::vector<int>, etc. Each type has a name, a size (in bytes), a range of possible values, and a set of operations.

## Section 2: For conversions between range representations when they do not fit with existing types (with C and C++ form).
Sometimes, you may need to convert a value of one type to another type, either because the original type cannot represent the value you want, or because you want to use a different set of operations on it. For example, you may want to convert an int to a double to perform floating-point arithmetic, or convert a char to an int to get its ASCII code.
There are two ways of converting types in C++: implicit conversions and explicit conversions.

### Implicit conversions
Implicit conversions are automatic conversions that the compiler performs when it encounters an expression that involves different types.

For example, if you write:
```cpp
int x = 10;
double y = x + 3.14;
```

The compiler will implicitly convert x from int to double before adding it to 3.14, so that both operands have the same type. The result will be a double value stored in y.
Implicit conversions follow certain rules that depend on the types involved. Some common rules are:
 - Numeric types can be converted to other numeric types, following the usual arithmetic conversions. For example, an int can be converted to a double, but not vice versa (unless explicitly casted).
 - A pointer to any object type can be converted to a pointer to void, which is a generic pointer type that can point to any object. A pointer to void can also be converted back to any pointer to object type.
 - A pointer to any function type can be converted to a pointer to another function type, as long as they have the same number and types of parameters and return type.
 - A null pointer constant (such as NULL or nullptr) can be converted to any pointer type.
 - A boolean value (true or false) can be converted to any numeric type, with true becoming 1 and false becoming 0.
 - An enumeration value can be converted to an integer type.

Implicit conversions are convenient, but they can also introduce errors or unexpected behavior if you are not careful.

For example, if you write:
```cpp
int x = 3;
int y = 2;
double z = x / y;
```

You may expect z to be 1.5, but it will actually be 1.0. This is because the division of two integers is an integer division, which discards the fractional part. The result is then implicitly converted to a double, but it is too late to recover the lost precision.
To avoid this problem, you should either use explicit conversions (see below), or make sure that at least one operand is already a floating-point type. 

For example:
```cpp
int x = 3;
int y = 2;
double z = static_cast<double>(x) / y; // explicit conversion
double w = x / 2.0; // implicit conversion
```

Both z and w will now be 1.5.

### Explicit conversions
Explicit conversions are conversions that you explicitly request by using a cast operator. A cast operator has the form:

```cpp
(type) expression
```
Where (type) is the name of the type you want to convert to, and expression is the value you want to convert.

For example, if you write:
```cpp
char c = 'A';
int i = (int) c;
```

You are explicitly converting the character 'A' to an integer, which will give you its ASCII code (65).
Explicit conversions give you more control over how types are converted, but they also require more care and attention. Some explicit conversions may lose information or change the meaning of the value.

For example, if you write:
```cpp
int i = 300;
char c = (char) i;
```

You are explicitly converting the integer 300 to a character, but since a char can only hold values from -128 to 127, the result will be a different character (with ASCII code 44, which is ',').
Explicit conversions can also be dangerous if you cast a pointer to an incompatible type. 

For example, if you write:
```cpp
int x = 10;
double* p = (double*) &x;
```

You are explicitly converting the address of an int variable to a pointer to a double, but since an int and a double have different sizes and representations, dereferencing p will give you a meaningless value.
In C++, there are four types of cast operators that perform different kinds of explicit conversions: `static_cast`, `dynamic_cast`, `const_cast`, and `reinterpret_cast`. Each one has a specific purpose and syntax, and you should use them carefully and appropriately. You can find more information about them on cppreference.com.

## Brief Explanation of Some Type Converters
`static_cast<type>(expression)` performs a compile-time conversion of the expression to the specified type. It can be used for conversions between related types, such as numeric types, pointers or references to base and derived classes, etc. It cannot be used for conversions that require runtime information, such as downcasting or casting away constness.
`dynamic_cast<type>(expression)` performs a runtime conversion of the expression to the specified type. It can be used for safe downcasting of pointers or references to polymorphic classes, i.e., classes with virtual functions. It returns a null pointer or throws an exception if the conversion fails.
`const_cast<type>(expression)` performs a conversion that removes or adds constness or volatility to the expression. It can be used for casting away the constness of pointers or references to const or volatile types, or for adding constness or volatility to pointers or references to non-const or non-volatile types.
`reinterpret_cast<type>(expression)` performs a low-level conversion of the expression to the specified type. It can be used for conversions between unrelated types, such as pointers to different types, integers and pointers, function pointers, etc. It does not guarantee any portability or safety and should be used with caution.

## Simplified Explanation for Beginners
Sometimes you need to change the type of a variable to make it work with another variable or function. For example, if you have an int variable and a double variable, and you want to divide them and get a decimal result, you need to change the int variable to a double variable first.
There are two ways to change the type of a variable: implicit and explicit. Implicit means the compiler does it for you automatically, and explicit means you do it yourself by writing some code.
Implicit conversions happen when you assign a value of one type to a variable of another type, and the compiler can figure out how to change it without losing any information. For example, if you assign an int value to a double variable, the compiler will add a decimal point and some zeros after it.
Explicit conversions happen when you assign a value of one type to a variable of another type, and the compiler cannot figure out how to change it without losing some information. For example, if you assign a double value to an int variable, the compiler will remove the decimal part and round it down.
To do an explicit conversion, you need to use a type converter. A type converter is a special word that tells the compiler how to change the type of a value. There are different types of converters for different situations.
`static_cast<type>(expression)` is a type converter that you can use for most situations. 
It changes the type of the expression to the type inside the angle brackets < >. 
For example,`static_cast<double>(10)` changes the int value 10 to the double value 10.0.
`dynamic_cast<type>(expression)` is a type converter that you can use when you have classes that inherit from each other. 
It changes the type of the expression from a base class pointer or reference

## Section 3: Implicit conversion types and explicit conversion types.
As mentioned above, implicit conversions are automatic conversions that the compiler performs when it encounters an expression that involves different types, while explicit conversions are conversions that you explicitly request by using a cast operator.
Implicit conversions can be classified into two types: standard conversions and user-defined conversions.

### Standard conversions
Standard conversions are implicit conversions that are defined by the language and apply to built-in types and some user-defined types. They include:
 - Numeric promotions: These are conversions of smaller numeric types to larger numeric types, such as char to int, or float to double. They preserve the value and do not lose information.
 - Numeric conversions: These are conversions of numeric types that may change the value or lose information, such as int to char, or double to float. They may cause overflow, underflow, truncation, or rounding errors.
 - Pointer conversions: These are conversions of pointer types that may change the type or level of indirection of the pointer, such as int* to void*, or int** to int*. They may cause alignment issues, type safety violations, or undefined behavior.
 - Boolean conversions: These are conversions of any type to a boolean type (bool). They convert zero values to false, and non-zero values to true.
 - Enumeration conversions: These are conversions of enumeration types (enum) to integer types. They convert the underlying value of the enumerator to the corresponding integer value.
 - Function pointer conversions: These are conversions of pointer to function types that may change the signature of the function, such as adding or removing parameters, or changing the return type. They may cause compatibility issues, type safety violations, or undefined behavior.
 - Array-to-pointer conversions: These are conversions of array types to pointer types. They convert the name of an array to a pointer to its first element.
 - Function-to-pointer conversions: These are conversions of function types to pointer types. They convert the name of a function to a pointer to that function.
 - Null pointer conversions: These are conversions of null pointer constants (such as NULL or nullptr) to any pointer type. They convert the null value to a null pointer of the target type.
 - Lvalue-to-rvalue conversions: These are conversions of lvalues (expressions that refer to objects) to rvalues (expressions that have values). They obtain the value of the object referred by the lvalue.

### User-defined conversions
User-defined conversions are implicit conversions that are defined by user-defined types (such as classes or structs) using special member functions called conversion functions. 
They allow user-defined types to behave like built-in types in certain contexts, such as arithmetic operations, comparisons, assignments, etc.

A conversion function has the following syntax:
```cpp
operator type() const;
```
Where type is the name of the type you want to convert to, and const means that the function does not modify the object it is called on.

For example, if you have a class that represents a complex number, you can define a conversion function that converts it to a double:
```cpp
class Complex {
public:
  // constructor
  Complex(double real, double imag) : real(real), imag(imag) {}

  // conversion function
  operator double() const {
    return real; // return only the real part
  }

private:
  double real; // real part
  double imag; // imaginary part
};
```

Now you can use an object of type Complex as if it were a double in some expressions:
```cpp
Complex c(1.0, 2.0); // create a complex number
double d = c + 3.0; // implicitly convert c to double and add 3.0
std::cout << d << std::endl; // print 4.
```

# Constants, Constant Expressions, and Statics
## Constants
Constants are values that cannot be changed after they are initialized. They are useful for defining symbolic names for fixed values, such as mathematical constants, physical constants, or configuration parameters.

There are two ways of declaring constants in C++: using the const specifier or using the constexpr specifier.

### The const specifier (runtime constant value)
The const specifier can be applied to any variable declaration to make it a constant.

For example:
```cpp
const int x = 10; // x is a constant integer
const double pi = 3.14; // pi is a constant double
const char* s = "Hello"; // s is a constant pointer to a constant string
```
A const variable must be initialized at the point of declaration, and its value cannot be modified afterwards. Attempting to do so will cause a compile-time error.

A const variable has the same type as the variable it is initialized from, except that it is qualified with const. This means that it can only be used in contexts that accept a const value of that type. For example, you cannot pass a const int to a function that expects an int&, because that would allow the function to modify the constant value.

A const variable has the same storage duration and linkage as the variable it is initialized from, unless it is declared at namespace scope or as a class member. In that case, it has internal linkage by default (meaning that it is only visible within the translation unit where it is defined), unless it is explicitly declared as extern.

A const variable can be initialized with any expression that is valid for its type, including non-constant expressions. However, this means that its value may not be known at compile time, and it may require dynamic memory allocation or initialization. For example:

```cpp
int f(); // some non-constant function
const int y = f(); // y is a constant initialized with a non-constant expression
```
### The constexpr specifier (compile-time constant value)
The constexpr specifier can be applied to any variable declaration to make it a constant expression. A constant expression is an expression that can be evaluated at compile time, and can be used in contexts that require compile-time constants, such as template arguments, array sizes, or static initializers.

For example:
```cpp
constexpr int x = 10; // x is a constant expression of type int
constexpr double pi = 3.14; // pi is a constant expression of type double
constexpr char* s = "Hello"; // s is a constant expression of type char*
```

A constexpr variable must be initialized with a constant expression of the same type. This means that it cannot use non-constant expressions, such as function calls (unless they are constexpr functions), input/output operations, or dynamic memory allocation. Attempting to do so will cause a compile-time error.

A constexpr variable has the same type as the variable it is initialized from, except that it is qualified with const. This means that it can only be used in contexts that accept a const value of that type.

A constexpr variable has the same storage duration and linkage as the variable it is initialized from, unless it is declared at namespace scope or as a class member. In that case, it has internal linkage by default (meaning that it is only visible within the translation unit where it is defined), unless it is explicitly declared as extern.

A constexpr variable can be used in any context that requires a constant expression of its type, such as template arguments, array sizes, or static initializers.

For example:
```cpp
template<int N>
struct Array {
  int data[N]; // N must be a constant expression, data is array type
};

constexpr int n = 5; // n is a constant expression
Array<n> arr; // arr is an array of 5 integers

constexpr int f(int x) {
  return x * x; // f is a constexpr function
}

constexpr int y = f(2); // y is a constant expression initialized with f(2)
```

## Statics
Statics are variables that have static storage duration, meaning that they are allocated when the program starts and deallocated when the program ends. They retain their values between function calls and across different translation units.

There are two ways of declaring statics in C++: using the `static` specifier or using the `thread_local`(c++ 2011 ~ c++ 2023) specifier.

### The static specifier
The static specifier can be applied to any variable declaration to make it static.

For example:
```cpp
static int x = 10; // x is a static variable
```
A static variable can be initialized with any expression that is valid for its type, including non-constant expressions. However, if it is initialized with a non-constant expression, the initialization is performed only once, the first time the variable is used.

A static variable has the same type as the variable it is initialized from, except that it is not qualified with const (unless it is explicitly declared as const or constexpr). This means that it can be used in contexts that accept a non-const value of that type, and its value can be modified.

A static variable has static storage duration and internal linkage by default (meaning that it is only visible within the translation unit where it is defined), unless it is explicitly declared as extern.

A static variable can be used in any context that accepts a value of its type, such as assignments, comparisons, arithmetic operations, etc. For example:
```cpp
static int x = 10; // x is a static variable

void f() {
  x++; // increment x
  std::cout << x << std::endl; // print x
}

int main() {
  f(); // prints 11
  f(); // prints 12
  f(); // prints 13
}
```

The static specifier can also be used to declare static local variables inside a function. A static local variable is a variable that is local to the function, but has static storage duration and retains its value between function calls. 

For example:
```cpp
void g() {
  static int y = 0; // y is a static local variable
  y++; // increment y
  std::cout << y << std::endl; // print y
}

int main() {
  g(); // prints 1
  g(); // prints 2
  g(); // prints 3
}
```

---

# 第三课：变量和类型转换系统；类型修饰符 - const、constexpr、static

**注意：不要害怕本课中的复杂示例，特别是涉及类、函数和带有内存地址或数组、模板等指针的示例。
我们只是用这些复杂示例来说明类型系统和我们提到的类型修饰符/说明符。它们不会在此处详细讨论。
当你进入更高级的章节时，你会更好地理解它们的含义。**

## 第一节：为什么需要类型？
类型是对程序中变量可以保存的值进行分类的一种方式。类型确定了为变量分配多少内存，如何存储和解释值，以及可以对其执行哪些操作。类型还帮助编译器检查错误并优化代码。
C++中的一些类型示例包括int、double、char、bool、std::string、std::vector<int>等。每种类型都有一个名称、一个大小（以字节为单位）、一组可能的值范围和一组操作。

## 第二节：当现有类型不适合时，进行范围表示之间的转换（使用C和C++形式）。
有时，您可能需要将一种类型的值转换为另一种类型，无论是因为原始类型无法表示您想要的值，还是因为您想要在其上使用不同的操作集。例如，您可能希望将int转换为double以执行浮点运算，或将char转换为int以获得其ASCII码。
在C++中，有两种类型转换的方式：隐式转换和显式转换。

### 隐式转换

隐式转换是编译器在遇到涉及不同类型的表达式时自动执行的转换。

例如，如果你写下：

```cpp
int x = 10;
double y = x + 3.14;
```

编译器将在将x从int转换为double后再将其与3.14相加，以使两个操作数具有相同的类型。结果将是一个存储在y中的double值。

隐式转换遵循涉及类型的特定规则。一些常见规则包括：

- 数值类型可以转换为其他数值类型，遵循通常的算术转换。例如，int可以转换为double，但反过来则不行（除非显式转换）。
- 任何对象类型的指针都可以转换为void指针，void是一种通用指针类型，可以指向任何对象。void指针也可以转换回任何对象类型的指针。
- 任何函数类型的指针都可以转换为另一种函数类型的指针，只要它们具有相同的参数数量、参数类型和返回类型。
- 空指针常量（例如NULL或nullptr）可以转换为任何指针类型。
- 布尔值（true或false）可以转换为任何数值类型，true变为1，false变为0。
- 枚举值可以转换为整数类型。

隐式转换很方便，但如果不小心使用，可能会引入错误或意外行为。

例如，如果你写下：

```cpp
int x = 3;
int y = 2;
double z = x / y;
```

你可能期望z的值为1.5，但实际上它将是1.0。这是因为两个整数的除法是整数除法，会丢弃小数部分。然后将结果隐式转换为double，但此时已经无法恢复丢失的精度。

为避免这个问题，你应该使用显式转换（见下文）或确保至少一个操作数已经是浮点类型。

例如：

```cpp
int x = 3;
int y = 2;
double z = static_cast<double>(x) / y; // 显式转换
double w = x / 2.0; // 隐式转换
```

现在z和w都将是1.5。

### 显式转换

显式转换是通过使用强制类型转换操作符显式请求的转换。强制类型转换操作符的形式如下：

```cpp
(type) expression
```

其中，(type) 是你想要转换到的类型的名称，expression 是你想要转换的值。

例如，如果你写下：
```cpp
char c = 'A';
int i = (int) c;
```

你正在显式将字符 'A' 转换为整数，这将给出它的 ASCII 码（65）。
显式转换让你对类型转换有更多的控制，但也需要更加小心和注意。某些显式转换可能会丢失信息或改变值的含义。

例如，如果你写下：
```cpp
int i = 300;
char c = (char) i;
```

你正在显式将整数 300 转换为字符，但由于 char 只能存储从 -128 到 127 的值，结果将是一个不同的字符（ASCII 码为 44，即 ','）。
如果将指针强制转换为不兼容的类型，显式转换也可能是危险的。

例如，如果你写下：
```cpp
int x = 10;
double* p = (double*) &x;
```

你正在显式将一个 int 变量的地址转换为指向 double 的指针，但由于 int 和 double 的大小和表示方式不同，对 p 解引用将给出一个无意义的值。
在 C++ 中，有四种类型的强制类型转换操作符，执行不同类型的显式转换：`static_cast`、`dynamic_cast`、`const_cast` 和 `reinterpret_cast`。每种转换都有特定的目的和语法，你应该谨慎而恰当地使用它们。你可以在 cppreference.com 上找到更多关于它们的信息。

## 关于某些类型转换器的简要解释
`static_cast<type>(expression)` 对表达式执行编译时的转换，将其转换为指定的类型。它可用于相关类型之间的转换，如数值类型、指针或引用类型的基类和派生类之间的转换等。它不能用于需要运行时信息的转换，如向下转换或去除const性质的转换。
`dynamic_cast<type>(expression)` 对表达式执行运行时的转换，将其转换为指定的类型。它可用于指向多态类（即具有虚函数的类）的指针或引用的安全向下转换。如果转换失败，它会返回空指针或抛出异常。
`const_cast<type>(expression)` 对表达式执行添加或移除const或volatile性质的转换。它可用于去除指向const或volatile类型的指针或引用的const性质，或者给指向非const或非volatile类型的指针或引用添加const或volatile性质。
`reinterpret_cast<type>(expression)` 对表达式执行低级别的转换，将其转换为指定的类型。它可用于不相关类型之间的转换，如指向不同类型的指针、整数和指针之间的转换、函数指针等。它不保证可移植性或安全性，应谨慎使用。

## 简化解释（初学者）
有时候你需要改变变量的类型，以便它能与另一个变量或函数一起工作。例如，如果你有一个int变量和一个double变量，你想要将它们相除并得到一个小数结果，你需要先将int变量转换为double变量。
改变变量类型的方法有两种：隐式转换和显式转换。隐式转换是编译器自动进行的，而显式转换需要你自己编写一些代码来进行转换。
隐式转换发生在将一个类型的值赋给另一个类型的变量时，编译器可以找出如何进行转换而不丢失任何信息。例如，如果将一个int值赋给一个double变量，编译器将在它后面添加一个小数点和一些零。
显式转换发生在将一个类型的值赋给另一个类型的变量时，编译器无法找出如何进行转换而不丢失一些信息。例如，如果将一个double值赋给一个int变量，编译器将去掉小数部分并向下取整。
要进行显式转换，你需要使用一个类型转换器。类型转换器是一个特殊的关键字，告诉编译器如何改变值的类型。不同的情况需要不同类型的转换器。
`static_cast<type>(expression)` 是一个通用的类型转换器。它将表达式的类型转换为尖括号<>内指定的类型。例如，`static_cast<double>(10)` 将int值10转换为double值10.0。
`dynamic_cast<type>(expression)` 是一个用于具有继承关系的类的类型转换器。它将表达式从基类指针或引用转换为派生类指针或引用。

## 第三节：隐式转换类型和显式转换类型

如前所述，隐式转换是编译器在遇到涉及不同类型的表达式时自动执行的转换，而显式转换是通过使用强制类型转换操作符显式请求的转换。
隐式转换可以分为两种类型：标准转换和用户定义转换。

### 标准转换
标准转换是由语言定义的、适用于内置类型和某些用户定义类型的隐式转换。它们包括：
- 数值提升：这是将较小的数值类型转换为较大的数值类型，例如将char转换为int或将float转换为double。它们保留值并且不丢失信息。
- 数值转换：这是对数值类型的转换，可能改变值或丢失信息，例如将int转换为char或将double转换为float。它们可能导致溢出、下溢、截断或舍入错误。
- 指针转换：这是对指针类型的转换，可能改变指针的类型或间接级别，例如将int*转换为void*或将int**转换为int*。它们可能导致对齐问题、类型安全性违规或未定义行为。
- 布尔转换：这是将任何类型转换为布尔类型（bool）的转换。它们将零值转换为false，非零值转换为true。
- 枚举转换：这是将枚举类型（enum）转换为整数类型的转换。它们将枚举器的底层值转换为相应的整数值。
- 函数指针转换：这是对函数指针类型的转换，可能改变函数的签名，例如添加或删除参数或更改返回类型。它们可能导致兼容性问题、类型安全性违规或未定义行为。
- 数组到指针转换：这是将数组类型转换为指针类型的转换。它们将数组的名称转换为指向其第一个元素的指针。
- 函数到指针转换：这是将函数类型转换为指针类型的转换。它们将函数的名称转换为指向该函数的指针。
- 空指针转换：这是将空指针常量（例如NULL或nullptr）转换为任何指针类型的转换。它们将空值转换为目标类型的空指针。
- 左值到右值转换：这是将左值（引用对象的表达式）转换为右值（具有值的表达式）的转换。它们获取由左值引用的对象的值。

### 用户定义转换
用户定义转换是由用户定义类型（如类或结构体）使用特殊成员函数（称为转换函数）定义的隐式转换。
它们允许用户定义类型在某些上下文中表现得像内置类型，例如算术运算、比较、赋值等。

转换函数具有以下语法：
```cpp
operator type() const;
```

其中，type 是你想要转换到的类型的名称，const 表示函数不修改调用它的对象。

例如，如果你有一个表示复数的类，你可以定义一个转换函数将其转换为double：
```cpp
class Complex {
public:
  // 构造函数
  Complex(double real, double imag) : real(real), imag(imag) {}

  // 转换函数
  operator double() const {
    return real; // 仅返回实部
  }

private:
  double real; // 实部
  double imag; // 虚部
};
```

现在你可以在某些表达式中像使用double一样使用 Complex 类型的对象：
```cpp
Complex c(1.0, 2.0); // 创建一个复数
double d = c + 3.0; // 隐式将 c 转换为 double，并加上 3.0
std::cout << d << std::endl; // 输出 4.0
```

# 常量、常量表达式和静态变量
## 常量
常量是在初始化后无法更改的值。它们用于定义固定值的符号名称，如数学常量、物理常量或配置参数。

在C++中有两种声明常量的方法：使用const修饰符或使用constexpr修饰符。

### const修饰符（运行时常量值）
const修饰符可以应用于任何变量声明，使其成为常量。

例如：
```cpp
const int x = 10; // x是一个常量整数
const double pi = 3.14; // pi是一个常量双精度浮点数
const char* s = "Hello"; // s是一个常量指向常量字符串的指针
```
const变量必须在声明点进行初始化，并且其值在初始化后不能被修改。尝试修改将导致编译时错误。

const变量的类型与其初始化的变量相同，只是在类型前加上const修饰符。这意味着它只能在接受该类型的const值的上下文中使用。例如，你不能将const int传递给期望int&的函数，因为这将允许函数修改常量的值。

const变量的存储期和链接性与其初始化的变量相同，除非它是在命名空间范围或作为类成员声明的。在这种情况下，默认情况下它具有内部链接性（仅在定义它的翻译单元中可见），除非显式声明为extern。

const变量可以使用对其类型有效的任何表达式进行初始化，包括非常量表达式。然而，这意味着它的值可能在编译时不知道，可能需要动态内存分配或初始化。例如：
```cpp
int f(); // 一些非常量函数
const int y = f(); // y是用非常量表达式初始化的常量
```

### constexpr修饰符（编译时常量值）
constexpr修饰符可以应用于任何变量声明，使其成为常量表达式。常量表达式是在编译时可以计算的表达式，并且可以在需要编译时常量的上下文中使用，如模板参数、数组大小或静态初始化器。

例如：
```cpp
constexpr int x = 10; // x是int类型的常量表达式
constexpr double pi = 3.14; // pi是double类型的常量表达式
constexpr char* s = "Hello"; // s是char*类型的常量表达式
```

constexpr变量必须使用相同类型的常量表达式进行初始化。这意味着它不能使用非常量表达式，如函数调用（除非它们是constexpr函数）、输入/输出操作或动态内存分配。尝试这样做将导致编译时错误。

constexpr变量

的类型与其初始化的变量相同，只是在类型前加上const修饰符。这意味着它只能在接受该类型的const值的上下文中使用。

constexpr变量的存储期和链接性与其初始化的变量相同，除非它是在命名空间范围或作为类成员声明的。在这种情况下，默认情况下它具有内部链接性（仅在定义它的翻译单元中可见），除非显式声明为extern。

constexpr变量可以在需要其类型的常量表达式的任何上下文中使用，例如模板参数、数组大小或静态初始化器。

例如：
```cpp
template<int N>
struct Array {
  int data[N]; // N必须是常量表达式，data是数组类型
};

constexpr int n = 5; // n是常量表达式
Array<n> arr; // arr是由5个整数组成的数组

constexpr int f(int x) {
  return x * x; // f是一个constexpr函数
}

constexpr int y = f(2); // y是用f(2)初始化的常量表达式
```

## 静态变量
静态变量是具有静态存储期的变量，意味着它们在程序启动时分配，在程序结束时释放。它们在函数调用之间和不同的翻译单元之间保持其值。

在C++中有两种声明静态变量的方法：使用 `static` 修饰符或使用 `thread_local` 修饰符（C++ 2011 ~ C++ 2023）。

### static 修饰符
static 修饰符可以应用于任何变量声明，使其成为静态变量。

例如：
```cpp
static int x = 10; // x 是一个静态变量
```
静态变量可以用其类型有效的任何表达式进行初始化，包括非常量表达式。然而，如果它使用非常量表达式进行初始化，初始化只会在第一次使用变量时执行一次。

静态变量与其初始化的变量具有相同的类型，只是没有 const 限定符（除非它显式声明为 const 或 constexpr）。这意味着它可以在接受该类型的非 const 值的上下文中使用，并且它的值可以被修改。

静态变量具有静态存储期和默认的内部链接性（只在定义它的翻译单元内可见），除非它显式声明为 extern。

静态变量可以在接受其类型的任何上下文中使用，例如赋值、比较、算术运算等。例如：
```cpp
static int x = 10; // x 是一个静态变量

void f() {
  x++; // 增加 x
  std::cout << x << std::endl; // 输出 x
}

int main() {
  f(); // 输出 11
  f(); // 输出 12
  f(); // 输出 13
}
```

static 修饰符还可以用于在函数内部声明静态局部变量。静态局部变量是函数局部的变量，但具有静态存储期，在函数调用之间保留其值。

例如：
```cpp
void g() {
  static int y = 0; // y 是一个静态局部变量
  y++; // 增加 y
  std::cout << y << std::endl; // 输出 y
}

int main() {
  g(); // 输出 1
  g(); // 输出 2
  g(); // 输出 3
}
```