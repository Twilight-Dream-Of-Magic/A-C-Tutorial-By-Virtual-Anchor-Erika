## Lesson 12: Template Programming in C++

### Introduction

In C++, template programming is a powerful feature that allows the creation of generic and reusable code. Templates enable functions and classes to operate with generic types, making them more flexible and versatile. By understanding templates, you can write code that works with any data type, enhancing the reusability and efficiency of your programs.

### What is Template Programming?

Template programming in C++ involves creating templates for functions or classes that can work with any data type. The main advantage of templates is that they allow you to write generic code without sacrificing type safety. This is achieved using type placeholders, which are substituted with actual types when the template is instantiated.

### Why Use Templates?

- **Reusability**: Templates allow you to write a single generic function or class that can work with different data types, reducing code duplication.
- **Flexibility**: Templates enable functions and classes to be more flexible and adaptable to different types of data.
- **Type Safety**: Templates maintain type safety, ensuring that operations are performed on compatible data types.

### Function Templates

Function templates allow you to create functions that operate on generic types. The syntax involves defining the template with a type placeholder.

#### Example: Function Template for Addition

```cpp
#include <iostream>

// Define a function template for addition
template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    int intResult = add(3, 4); // Using the template with int type
    double doubleResult = add(3.5, 4.5); // Using the template with double type

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

In this example, the `add` function template can be used with any data type. When we call `add(3, 4)`, the compiler instantiates the template with `int` type. Similarly, calling `add(3.5, 4.5)` instantiates the template with `double` type.

#### More Examples of Function Templates

##### Example: Function Template for Subtraction

```cpp
template <typename T>
T subtract(T a, T b) {
    return a - b;
}

int main() {
    int intResult = subtract(10, 4); // Using the template with int type
    double doubleResult = subtract(10.5, 4.5); // Using the template with double type

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

##### Example: Function Template for Multiplication

```cpp
template <typename T>
T multiply(T a, T b) {
    return a * b;
}

int main() {
    int intResult = multiply(3, 4); // Using the template with int type
    double doubleResult = multiply(3.5, 4.5); // Using the template with double type

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

### Class Templates

Class templates allow you to create classes that work with generic types. The syntax involves defining the template with a type placeholder.

#### Example: Class Template for a Box

```cpp
#include <iostream>
#include <string>

// Define a class template for a Box
template <typename T>
class Box {
private:
    T value;
public:
    Box(T v) : value(v) {}
    T getValue() const { return value; }
};

int main() {
    Box<int> intBox(123); // Using the template with int type
    Box<std::string> stringBox("Hello"); // Using the template with string type

    std::cout << "Int box: " << intBox.getValue() << std::endl;
    std::cout << "String box: " << stringBox.getValue() << std::endl;
    return 0;
}
```

In this example, the `Box` class template can be instantiated with any data type. The `intBox` object is a `Box` of type `int`, while the `stringBox` object is a `Box` of type `std::string`.

#### More Examples of Class Templates

##### Example: Class Template for a Pair

```cpp
template <typename T1, typename T2>
class Pair {
private:
    T1 first;
    T2 second;
public:
    Pair(T1 f, T2 s) : first(f), second(s) {}
    void print() const {
        std::cout << "First: " << first << ", Second: " << second << std::endl;
    }
};

int main() {
    Pair<int, double> p1(1, 2.5);
    Pair<std::string, char> p2("Hello", 'A');

    p1.print();
    p2.print();

    return 0;
}
```

### Practical Applications of Templates

Templates are widely used in the Standard Template Library (STL) for creating generic data structures and algorithms. Examples include `std::vector`, `std::list`, and `std::map`.

#### Example: Using `std::vector`

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### Template Programming Techniques

1. **Template Specialization**: Allows you to define a specific implementation of a template for a particular data type.
2. **Template Metaprogramming**: Involves using templates to perform computations at compile time.

#### Example: Template Specialization

```cpp
#include <iostream>
#include <string>

template <typename T>
class Printer {
public:
    void print(T value) {
        std::cout << value << std::endl;
    }
};

// Specialization for string type
template <>
class Printer<std::string> {
public:
    void print(std::string value) {
        std::cout << "String: " << value << std::endl;
    }
};

int main() {
    Printer<int> intPrinter;
    Printer<std::string> stringPrinter;

    intPrinter.print(42);
    stringPrinter.print("Hello, world!");

    return 0;
}
```

### Universal References

Universal references, also known as forwarding references, allow templates to accept any type of argument. This is particularly useful for perfect forwarding, where you want to forward arguments to another function exactly as they were passed.

#### Reference Collapsing Mechanism

When using universal references, C++ employs a mechanism known as reference collapsing. The rules for reference collapsing are:

- `T& &` becomes `T&`
- `T& &&` becomes `T&`
- `T&& &` becomes `T&`
- `T&& &&` becomes `T&&`

#### Example: Universal Reference in a Function Template

```cpp
#include <iostream>
#include <utility> // for std::forward

template <typename T>
void print(T&& value) {
    std::cout << value << std::endl;
}

int main() {
    int a = 10;
    print(a); // Calls print<int&>(a)
    print(20); // Calls print<int>(20)

    return 0;
}
```

### Complex Examples with Multiple Placeholders

Templates can also support multiple type placeholders, allowing more complex and flexible template definitions.

#### Example: Template with Two Type Placeholders

```cpp
#include <iostream>

template <typename T, typename U>
class Pair {
private:
    T first;
    U second;
public:
    Pair(T f, U s) : first(f), second(s) {}
    void print() const {
        std::cout << "First: " << first << ", Second: " << second << std::endl;
    }
};

int main() {
    Pair<int, double> p1(1, 2.5);
    Pair<std::string, char> p2("Hello", 'A');

    p1.print();
    p2.print();

    return 0;
}
```

### Specifying Non-Type Template Parameters

Templates can also have non-type parameters, which can change the template instantiation process. 
Non-type parameters can be integers, enumerations, pointers or references.

#### Example: Class Template with Non-Type Parameter

```cpp
#include <iostream>

template <typename T, int size>
class Array {
private:
    T arr[size];
public:
    T& operator[](int index) {
        return arr[index];
    }
    void print() const {
        for (int i = 0; i < size; ++i) {
           

 std::cout << arr[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    Array<int, 5> intArray;
    for (int i = 0; i < 5; ++i) {
        intArray[i] = i * 10;
    }
    intArray.print();

    return 0;
}
```

#### Example: Matrix Class Template with Non-Typed Parameters

```cpp
#include <iostream>

template <typename T, int rows, int cols>
class Matrix {
private:
    T data[rows][cols];
public:
    Matrix() {
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                data[i][j] = T();
            }
        }
    }

    T& operator()(int row, int col) {
        return data[row][col];
    }

    void print() const {
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                std::cout << data[i][j] << " ";
            }
            std::cout << std::endl;
        }
    }
};

int main() {
    Matrix<int, 3, 3> intMatrix;
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            intMatrix(i, j) = i * 3 + j;
        }
    }
    intMatrix.print();

    return 0;
}
```

In this example, the `Matrix` class template has untyped parameters `rows` and `cols`, which determine the size of the matrix. We define a `int` type matrix of `3x3` and fill it with integers. Finally, we print the contents of the matrix.

### Template Specialization

Template specialization allows you to define specific implementations for certain types while leaving the general template to handle other types.

#### Example: Full Template Specialization

```cpp
#include <iostream>

template <typename T>
class TypeInfo {
public:
    static void print() {
        std::cout << "Unknown type" << std::endl;
    }
};

// Specialization for int type
template <>
class TypeInfo<int> {
public:
    static void print() {
        std::cout << "Type: int" << std::endl;
    }
};

// Specialization for double type
template <>
class TypeInfo<double> {
public:
    static void print() {
        std::cout << "Type: double" << std::endl;
    }
};

int main() {
    TypeInfo<int>::print(); // Output: Type: int
    TypeInfo<double>::print(); // Output: Type: double
    TypeInfo<char>::print(); // Output: Unknown type

    return 0;
}
```

### Object-Oriented Programming vs. Template Programming

#### Advantages of Object-Oriented Programming (OOP):

- **Modularity**: Organizes code into classes, making it easy to manage and understand.
- **Reusability**: Promotes code reuse through inheritance and polymorphism.
- **Scalability**: Facilitates the addition of new features without affecting existing code.

#### Disadvantages of Object-Oriented Programming (OOP):

- **Complexity**: Can lead to complex class hierarchies that are difficult to manage.
- **Performance Overhead**: Virtual functions and dynamic binding can introduce performance overhead.

#### Advantages of Template Programming:

- **Code Reusability**: Templates allow you to write generic code that can be reused with different data types.
- **Performance**: Templates are typically resolved at compile-time, leading to more efficient code.
- **Flexibility**: Templates can be used for a wide range of generic programming tasks.

#### Disadvantages of Template Programming:

- **Complexity**: Template syntax can be complex and difficult to debug.
- **Compilation Overhead**: Templates must be instantiated for each type, which can increase compilation times.
- **Limited Separation**: In C++, templates cannot be separated into declaration and implementation files; they must be defined in the header files.

### Conclusion

Template programming in C++ is a powerful tool that enhances code reusability, flexibility, and type safety. By understanding and using templates, you can create more generic and versatile functions and classes that work with any data type. Templates are a fundamental part of C++ programming, enabling developers to write more efficient and maintainable code.

### Homework

1. **Create a function template** that performs multiplication on two values and test it with different data types.
2. **Create a class template** for a simple container that can store any type of data and provide methods to set and get the value.
3. **Practice using STL templates** like `std::vector` and `std::map` in a small program.

By exploring these exercises, you will gain hands-on experience with template programming and understand its practical applications in C++.


---

### 第12课：C++中的模板编程

#### 介绍

在C++中，模板编程是一种强大的功能，它允许创建通用和可重用的代码。模板使函数和类能够操作通用类型，使它们更加灵活和多功能。通过理解模板，你可以编写能够处理任何数据类型的代码，从而增强程序的可重用性和效率。

### 什么是模板编程？

C++中的模板编程涉及为可以处理任何数据类型的函数或类创建模板。模板的主要优点是它们允许你编写通用代码而不牺牲类型安全性。这是通过使用类型占位符来实现的，这些占位符在实例化模板时被实际类型替换。

### 为什么要使用模板？

- **可重用性**：模板允许你编写可以处理不同数据类型的单个通用函数或类，减少代码重复。
- **灵活性**：模板使函数和类能够更灵活地适应不同类型的数据。
- **类型安全性**：模板保持类型安全，确保操作在兼容的数据类型上进行。

### 函数模板

函数模板允许你创建操作通用类型的函数。其语法涉及使用类型占位符定义模板。

#### 示例：加法函数模板

```cpp
#include <iostream>

// 定义加法函数模板
template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    int intResult = add(3, 4); // 使用int类型的模板
    double doubleResult = add(3.5, 4.5); // 使用double类型的模板

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

在这个例子中，`add`函数模板可以与任何数据类型一起使用。当我们调用`add(3, 4)`时，编译器实例化`int`类型的模板。同样，调用`add(3.5, 4.5)`实例化`double`类型的模板。

#### 更多函数模板示例

##### 示例：减法函数模板

```cpp
template <typename T>
T subtract(T a, T b) {
    return a - b;
}

int main() {
    int intResult = subtract(10, 4); // 使用int类型的模板
    double doubleResult = subtract(10.5, 4.5); // 使用double类型的模板

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

##### 示例：乘法函数模板

```cpp
template <typename T>
T multiply(T a, T b) {
    return a * b;
}

int main() {
    int intResult = multiply(3, 4); // 使用int类型的模板
    double doubleResult = multiply(3.5, 4.5); // 使用double类型的模板

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

### 类模板

类模板允许你创建操作通用类型的类。其语法涉及使用类型占位符定义模板。

#### 示例：盒子类模板

```cpp
#include <iostream>
#include <string>

// 定义盒子类模板
template <typename T>
class Box {
private:
    T value;
public:
    Box(T v) : value(v) {}
    T getValue() const { return value; }
};

int main() {
    Box<int> intBox(123); // 使用int类型的模板
    Box<std::string> stringBox("Hello"); // 使用string类型的模板

    std::cout << "Int box: " << intBox.getValue() << std::endl;
    std::cout << "String box: " << stringBox.getValue() << std::endl;
    return 0;
}
```

在这个例子中，`Box`类模板可以实例化任何数据类型。`intBox`对象是`int`类型的盒子，而`stringBox`对象是`std::string`类型的盒子。

#### 更多类模板示例

##### 示例：成对类模板

```cpp
template <typename T1, typename T2>
class Pair {
private:
    T1 first;
    T2 second;
public:
    Pair(T1 f, T2 s) : first(f), second(s) {}
    void print() const {
        std::cout << "First: " << first << ", Second: " << second << std::endl;
    }
};

int main() {
    Pair<int, double> p1(1, 2.5);
    Pair<std::string, char> p2("Hello", 'A');

    p1.print();
    p2.print();

    return 0;
}
```

### 模板的实际应用

模板广泛应用于标准模板库（STL），用于创建通用的数据结构和算法。示例包括`std::vector`、`std::list`和`std::map`。

#### 示例：使用`std::vector`

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### 模板编程技术

1. **模板特化**：允许你为特定数据类型定义模板的具体实现。
2. **模板元编程**：涉及在编译时使用模板进行计算。

#### 示例：模板特化

```cpp
#include <iostream>
#include <string>

template <typename T>
class Printer {
public:
    void print(T value) {
        std::cout << value << std::endl;
    }
};

// 对字符串类型的特化
template <>
class Printer<std::string> {
public:
    void print(std::string value) {
        std::cout << "String: " << value << std::endl;
    }
};

int main() {
    Printer<int> intPrinter;
    Printer<std::string> stringPrinter;

    intPrinter.print(42);
    stringPrinter.print("Hello, world!");

    return 0;
}
```

### 通用引用

通用引用（也称为转发引用）允许模板接受任何类型的参数。这对于完美转发非常有用，即将参数精确地传递给另一个函数。

#### 引用折叠机制

使用通用引用时，C++采用一种称为引用折叠的机制。引用折叠的规则是：

- `T& &` 变为 `T&`
- `T& &&` 变为 `T&`
- `T&& &` 变为 `T&`
- `T&& &&` 变为 `T&&`

#### 示例：函数模板中的通用引用

```cpp
#include <iostream>
#include <utility> // for std::forward

template <typename T>
void print(T&& value) {
    std::cout << value << std::endl;
}

int main() {
    int a = 10;
    print(a); // 调用 print<int&>(a)
    print(20); // 调用 print<int>(20)

    return 0;
}
```

### 带有多个占位符的复杂示例

模板还支持多个类型占位符，允许更复杂和灵活的模板定义。

#### 示例：具有两个类型占位符的模板

```cpp
#include <iostream>

template <typename T, typename U>
class Pair {
private:
    T first;
    U second;
public:
    Pair(T f, U s) : first(f), second(s) {}
    void print() const {
        std::cout << "First: " << first << ", Second: " << second << std::endl;
    }
};

int main() {
    Pair<int, double> p1(1, 2.5);
    Pair<std::string, char> p2("Hello", 'A');

    p1.print();
    p2.print();

    return 0;
}
```

### 指定非类型模板参数

模板还可以有非类型参数，这可以改变模板实例化过程。非类型参数可以是整数、枚举、指针或引用。

#### 示例：带非类型参数的矩阵类模板

```cpp
#include <iostream>

template <typename T, int rows, int cols>
class Matrix {
private:
    T data[rows][cols];
public:
    Matrix() {
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                data[i][j] = T();
            }
        }
    }

    T& operator()(int row, int col) {
        return data[row][col];
    }

    void print() const {
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < cols; ++j) {
                std::cout << data[i][j] << " ";
            }
            std::cout << std::

endl;
        }
    }
};

int main() {
    Matrix<int, 3, 3> intMatrix;
    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            intMatrix(i, j) = i * 3 + j;
        }
    }
    intMatrix.print();

    return 0;
}
```

在这个例子中，`Matrix`类模板具有非类型参数`rows`和`cols`，它们决定矩阵的大小。我们定义了一个`3x3`的`int`类型矩阵，并用整数填充它。最后，我们打印矩阵的内容。

### 模板特化

模板特化允许你为某些类型定义特定实现，而让通用模板处理其他类型。

#### 示例：完全模板特化

```cpp
#include <iostream>

template <typename T>
class TypeInfo {
public:
    static void print() {
        std::cout << "Unknown type" << std::endl;
    }
};

// 对int类型的特化
template <>
class TypeInfo<int> {
public:
    static void print() {
        std::cout << "Type: int" << std::endl;
    }
};

// 对double类型的特化
template <>
class TypeInfo<double> {
public:
    static void print() {
        std::cout << "Type: double" << std::endl;
    }
};

int main() {
    TypeInfo<int>::print(); // 输出: Type: int
    TypeInfo<double>::print(); // 输出: Type: double
    TypeInfo<char>::print(); // 输出: Unknown type

    return 0;
}
```

### 面向对象编程与模板编程

#### 面向对象编程（OOP）的优点：

- **模块化**：将代码组织成类，易于管理和理解。
- **可重用性**：通过继承和多态性促进代码重用。
- **可扩展性**：有助于添加新功能而不影响现有代码。

#### 面向对象编程（OOP）的缺点：

- **复杂性**：可能导致难以管理的复杂类层次结构。
- **性能开销**：虚函数和动态绑定可能引入性能开销。

#### 模板编程的优点：

- **代码重用**：模板允许你编写可与不同数据类型一起使用的通用代码。
- **性能**：模板通常在编译时解析，生成更高效的代码。
- **灵活性**：模板可用于广泛的通用编程任务。

#### 模板编程的缺点：

- **复杂性**：模板语法复杂且难以调试。
- **编译开销**：模板必须为每种类型实例化，这可能增加编译时间。
- **有限的分离**：在C++中，模板不能分为声明和实现文件；它们必须在头文件中定义。

### 结论

C++中的模板编程是增强代码可重用性、灵活性和类型安全性的强大工具。通过理解和使用模板，你可以创建更通用和多功能的函数和类，能够处理任何数据类型。模板是C++编程的基本部分，使开发者能够编写更高效和可维护的代码。

### 作业

1. **创建一个函数模板**，执行两个值的乘法，并用不同的数据类型测试它。
2. **创建一个类模板**，用于存储任意类型的数据，并提供设置和获取值的方法。
3. **练习使用STL模板**，如`std::vector`和`std::map`，在一个小程序中。

通过探索这些练习，你将获得模板编程的实践经验，并理解其在C++中的实际应用。