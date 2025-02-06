### Lesson 13: Advanced C++ Template Techniques

---

### Introduction

In the previous lessons, we introduced function templates as a way to write generic, reusable code that works with any data type. We saw basic examples, like adding two numbers or multiplying values, using function templates. However, templates offer much more than that. Today, we will explore some advanced features of function templates in C++, which will help you write more powerful, efficient, and flexible code.

We'll begin by reviewing the core concept of function templates, then we will focus on key advanced features such as:

- **Template Specialization**: Customizing templates for specific types.
- **Default Template Arguments**: Providing default values for template parameters.
- **Function Template Overloading**: Using multiple templates with the same function name.
- **Explicit Template Instantiation**: Manually specifying template parameters.
- **Return Type Deduction**: Simplifying return type with `auto`.

---

### 1. Review: Basic Function Templates

In case you need a quick recap, a function template allows you to write a function that works with any data type. Here's a simple example of a function template for addition:

```cpp
#include <iostream>

// Function template for addition
template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    int intResult = add(3, 4);  // Uses int type
    double doubleResult = add(3.5, 4.5);  // Uses double type

    std::cout << "Int result: " << intResult << std::endl;
    std::cout << "Double result: " << doubleResult << std::endl;
    return 0;
}
```

This is a basic function template where the `add` function works with any type, as long as both arguments are of the same type (`T`).

### 2. Template Specialization: Customizing Templates for Specific Types

While function templates work for a wide range of types, sometimes you might want to customize the behavior for specific types. This is where **template specialization** comes in.

#### Example: Specializing for `std::string`

Suppose you want the `add` function to behave differently when adding two strings. You can specialize the template for `std::string`:

```cpp
#include <iostream>
#include <string>

// Primary template
template <typename T>
T add(T a, T b) {
    return a + b;
}

// Specialization for std::string
template <>
std::string add<std::string>(std::string a, std::string b) {
    return a + " " + b;  // Concatenate strings with a space
}

int main() {
    std::string strResult = add<std::string>("Hello", "World");
    std::cout << "String result: " << strResult << std::endl;  // Outputs "Hello World"
    return 0;
}
```

In this example, for any non-`std::string` type, the generic `add` function is used, but for `std::string`, the specialized version concatenates the strings with a space.

### 3. Default Template Arguments: Providing Defaults for Parameters

Sometimes, you may want to provide default values for your template parameters. This is useful for simplifying function calls when you don't need to specify all the template parameters.

#### Example: Default Template Argument

```cpp
#include <iostream>

// Template with default parameter
template <typename T = int>
T multiply(T a, T b) {
    return a * b;
}

int main() {
    std::cout << multiply(3, 4) << std::endl;  // Uses int as default
    std::cout << multiply(3.5, 4.5) << std::endl;  // Uses double
    return 0;
}
```

In this example, the `multiply` function template defaults to `int` if no template argument is specified. You can still explicitly specify a different type, such as `double`, when needed.

### 4. Function Template Overloading: Handling Different Signatures

You can overload function templates just like you overload regular functions. This allows you to define multiple versions of a function template with different parameter types or numbers of parameters.

#### Example: Overloading Templates for Different Argument Types

```cpp
#include <iostream>

// Generic template
template <typename T>
T add(T a, T b) {
    return a + b;
}

// Overloaded template for different types
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;  // Handles mixed types
}

int main() {
    std::cout << add(3, 4) << std::endl;          // Uses generic template (int + int)
    std::cout << add(3.5, 4) << std::endl;        // Uses overloaded template (double + int)
    return 0;
}
```

In this example, the first `add` function works for the same types, while the second one can handle different types (e.g., `int` and `double`), and it uses `decltype` to deduce the return type.

### 5. Explicit Template Instantiation: Manually Specifying Template Types

Sometimes, you may want to force the compiler to instantiate a template with a specific type. This is done using **explicit template instantiation**.

#### Example: Explicit Instantiation

```cpp
#include <iostream>

// Template for multiplication
template <typename T>
T multiply(T a, T b) {
    return a * b;
}

// Explicit instantiation
template int multiply<int>(int, int);

int main() {
    int result = multiply(3, 4);  // Uses explicitly instantiated version
    std::cout << result << std::endl;
    return 0;
}
```

Here, we explicitly instantiate the `multiply` template for `int` to avoid automatic instantiation or to control where the template is used.

In this example, if the user does not provide a type, it defaults to `int`. However, if the user specifies a type (like `double`), it will use that type instead.

#### 6. Non-Type Template Parameters

Templates in C++ are not limited to types; they can also accept **non-type parameters**, such as integers, pointers, or references. This is useful when you want the template to be aware of specific constants or sizes at compile time.

**Example: Template with Non-Type Parameters**

```cpp
#include <iostream>

template <typename T, int size>
class Array {
private:
    T arr[size];
public:
    void set(int index, T value) {
        arr[index] = value;
    }

    T get(int index) const {
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
    Array<int, 5> intArray;  // Array of 5 integers
    for (int i = 0; i < 5; ++i) {
        intArray.set(i, i * 10);  // Set values 0, 10, 20, ...
    }
    intArray.print();  // Prints the array

    return 0;
}
```

Here, `Array` is a template that takes an integer parameter `size` and a type `T`. This allows the template to create an array of any type and size. The size is known at compile time, and it helps optimize the array’s memory usage.

#### 7. Function Template Specialization

Template specialization allows you to define a function template's behavior for specific types. This can be useful when the generic template doesn't behave as expected for certain data types.

**Example: Template Specialization**

```cpp
#include <iostream>

template <typename T>
T print(T value) {
    return value;
}

// Specialization for type char
template <>
char print<char>(char value) {
    return std::toupper(value); // Convert char to uppercase
}

int main() {
    std::cout << print(42) << std::endl;  // Prints 42
    std::cout << print('a') << std::endl;  // Prints 'A'
    return 0;
}
```

In this case, we specialize the `print` template for the `char` type, ensuring that the function converts lowercase letters to uppercase when printing.

#### 8. Function Templates and Overloading

Function templates can be overloaded just like regular functions. Overloading allows you to create multiple function templates with the same name, but different parameter types or numbers.

**Example: Overloading Function Templates**

```cpp
#include <iostream>

// Generic template for adding two values
template <typename T>
T add(T a, T b) {
    return a + b;
}

// Overload for different number of parameters
template <typename T>
T add(T a, T b, T c) {
    return a + b + c;
}

int main() {
    std::cout << add(3, 4) << std::endl;      // Uses two arguments
    std::cout << add(1.1, 2.2, 3.3) << std::endl; // Uses three arguments
    return 0;
}
```

Here, we have overloaded the `add` template to handle two arguments and three arguments, making the template more flexible.

#### 9. Template Metaprogramming (Optional Advanced Feature)

Template metaprogramming allows you to use templates to perform computations at compile time. This advanced feature is useful for optimizing code or creating compile-time constants.

**Example: Compile-Time Factorial Calculation**

```cpp
#include <iostream>

template <int N>
struct Factorial {
    static const int value = N * Factorial<N - 1>::value;
};

template <>
struct Factorial<0> {
    static const int value = 1;  // Base case
};

int main() {
    std::cout << Factorial<5>::value << std::endl; // Outputs 120 (5! = 120)
    return 0;
}
```

This example calculates the factorial of a number at compile time using template recursion.

---

### 1. **SFINAE (Substitution Failure Is Not An Error)**

This concept is critical for understanding how C++ templates can be flexible and error-tolerant. We’ll dive into **SFINAE** by first explaining the question: **How does C++ know which template to instantiate when there are multiple candidates?**

#### Why is SFINAE Important?

In templates, if a substitution of a template argument fails (because it does not match the constraints we set), the compiler doesn't throw an error; instead, it simply "fails silently" and tries the next candidate template. This is called **Substitution Failure Is Not An Error**.

We can use SFINAE to create more robust and efficient templates that only instantiate under certain conditions.

**Example**: We will start by writing code that uses SFINAE to conditionally enable or disable template instantiation based on the type passed. This can be demonstrated with a **type trait**.

```cpp
#include <iostream>
#include <type_traits>

// SFINAE-based template to only allow arithmetic types (int, float, etc.)
template <typename T>
typename std::enable_if<std::is_arithmetic<T>::value, T>::type
add(T a, T b) {
    return a + b;
}

// This will only be enabled for non-arithmetic types
template <typename T>
typename std::enable_if<!std::is_arithmetic<T>::value, T>::type
add(T a, T b) {
    std::cout << "Non-arithmetic types are not supported for this operation." << std::endl;
    return T();
}

int main() {
    std::cout << add(1, 2) << std::endl;        // Works with arithmetic types
    std::cout << add("Hello", " World!") << std::endl; // Works with non-arithmetic types
    return 0;
}
```

**Explanation**:  
- The first `add` function is only instantiated if both `T` types are arithmetic (`std::is_arithmetic<T>::value` is `true`).
- The second `add` function is only instantiated for non-arithmetic types.  
This shows how SFINAE allows us to control which template gets instantiated based on type traits.

**SFINAE and Template Instantiation Logic**:  
- **Step 1**: The compiler looks at the function call in `main()`. For `add(1, 2)`, the compiler will attempt to match the first template.
- **Step 2**: The type traits `std::is_arithmetic<int>::value` and `std::is_arithmetic<double>::value` return true, so the first function template is instantiated.
- **Step 3**: For `add("Hello", "World!")`, since `std::is_arithmetic<const char*>::value` is false, the compiler skips the first template and instantiates the second one, printing the message for non-arithmetic types.

This example demonstrates how SFINAE is integral to C++'s template system, enabling type-safe and flexible template instantiation.

### 2. Curiously Recurring Template Pattern (CRTP)

The CRTP is a powerful technique in C++ where a class template derives from itself, using its own template argument as the base class. It's often used to enable static polymorphism, avoid runtime overhead, and enhance performance. Let’s revisit CRTP in greater depth.

**Example of CRTP**: Implementing a `Shape` class that can use compile-time polymorphism:

```cpp
#include <iostream>

// Base template class
template <typename Derived>
class Shape {
public:
    void draw() const {
        static_cast<const Derived*>(this)->drawImpl();
    }
};

// Derived class
class Circle : public Shape<Circle> {
public:
    void drawImpl() const {
        std::cout << "Drawing Circle" << std::endl;
    }
};

// Another derived class
class Square : public Shape<Square> {
public:
    void drawImpl() const {
        std::cout << "Drawing Square" << std::endl;
    }
};

int main() {
    Circle c;
    Square s;
    
    c.draw();  // Outputs "Drawing Circle"
    s.draw();  // Outputs "Drawing Square"

    return 0;
}
```

**Explanation**:  
- `Shape<Derived>` is the **base class** and takes a `Derived` class as its template parameter.
- In the `draw` method of `Shape`, we use **static polymorphism** by casting `this` to the derived class type and calling `drawImpl`.
- This allows the derived class to implement its own drawing logic (`drawImpl`), but the base class (`Shape`) defines the interface.

### 3. Template Metaprogramming with Type Traits

This will expand on advanced template usage to perform operations at compile-time based on types. We'll explore **type traits** like `std::enable_if`, `std::is_same`, and `std::is_integral` to conditionally enable or disable functions.

**Example: Using `std::enable_if` and `std::is_integral`**:

```cpp
#include <iostream>
#include <type_traits>

// Enable only for integral types
template <typename T>
typename std::enable_if<std::is_integral<T>::value, void>::type
printType(T val) {
    std::cout << "Integral type: " << val << std::endl;
}

int main() {
    printType(5);    // Works for integers
    // printType(5.5); // Uncommenting this will give a compile-time error (disabled for non-integral types)
}
```

Here, we use **template metaprogramming** to restrict the `printType` function to only integral types, based on the result of `std::is_integral<T>::value`.

### 4. **Variadic Templates**

Variadic templates allow a template to accept an arbitrary number of arguments. This is a common technique in C++ to implement flexible functions or class templates that can handle an unknown number of parameters.

**Example: Summing multiple arguments using variadic templates**:

```cpp
#include <iostream>

template <typename... Args>
auto sum(Args... args) {
    return (args + ...);  // Fold expression, summing all arguments
}

int main() {
    std::cout << sum(1, 2, 3, 4, 5) << std::endl;  // Outputs 15
    return 0;
}
```

This example uses **fold expressions** in C++17 to sum all the arguments passed to `sum`.

### 5. Template Aliases and `using` Syntax

The `using` keyword allows us to define template aliases, making code more readable and easier to manage. This is particularly useful for simplifying complex template declarations.

**Example: Creating template aliases**:

```cpp
#include <iostream>
#include <vector>

// Alias for a vector of integers
using IntVector = std::vector<int>;

int main() {
    IntVector v = {1, 2, 3, 4, 5};
    for (auto val : v) {
        std::cout << val << " ";
    }
    return 0;
}
```

Here, `IntVector` is a more readable alias for `std::vector<int>`.

### 6. Template Template Parameters

Template template parameters allow templates to accept other templates as parameters. This advanced technique is useful when you want to pass container types or other templates as arguments.

**Example: Using template template parameters**:

```cpp
#include <iostream>
#include <vector>

// Template template parameter (a template that accepts another template)
template <template <typename> class Container, typename T>
void printContainer(const Container<T>& container) {
    for (const auto& elem : container) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::vector<int> vec = {1, 2, 3};
    printContainer(vec);  // Works with any container type (e.g., vector, list)
}
```

In this case, `printContainer` can accept any container type, like `std::vector`, `std::list`, or others, thanks to the **template template parameter**.

---

### Summary of Lesson 13

In **Lesson 13**, we will explore advanced C++ template techniques, including:

- **SFINAE**: Understanding how templates work by controlling the instantiation process using type traits.
- **Curiously Recurring Template Pattern (CRTP)**: Enabling static polymorphism with compile-time class hierarchies.
- **Template Metaprogramming**: Using **type traits** and **enable_if** to implement compile-time logic.
- **Variadic Templates**: Creating flexible functions that can accept a variable number of arguments.
- **Template Aliases**: Simplifying complex template declarations.
- **Template Template Parameters**: Passing templates as arguments to other templates.

These techniques will help you write more flexible, efficient, and type-safe code.

---

## **Advanced C++ Template Programming Tutorial: Nested Templates, Dependent Templates, and Template Instantiation for Computations**

In C++, template programming offers powerful abstractions and flexibility, enabling developers to create generic and efficient solutions for a variety of computational tasks. In this tutorial, we will explore three advanced uses of templates: nested templates, dependent templates, and template instantiation. These examples demonstrate how to leverage templates for meaningful computational purposes such as matrix addition, sum calculations, and general arithmetic operations.

### **1. Nested Templates: `Name<Type, Namespace::template Name2<Type2>>`**

In this example, we will implement matrix addition using nested templates. The outer template `Name` will handle the matrix addition logic, while the nested template `Name2` will perform element-wise addition for the matrix.

#### **Example Code:**

```cpp
#include <iostream>
#include <vector>

namespace Namespace {
    // Template class to perform matrix addition
    template <typename T>
    struct MatrixAdd {
        void add(const std::vector<std::vector<T>>& matrix1, const std::vector<std::vector<T>>& matrix2, std::vector<std::vector<T>>& result) {
            int rows = matrix1.size();
            int cols = matrix1[0].size();

            for (int i = 0; i < rows; ++i) {
                for (int j = 0; j < cols; ++j) {
                    result[i][j] = matrix1[i][j] + matrix2[i][j];  // Element-wise addition
                }
            }
        }
    };

    // Outer template class that accepts the MatrixAdd template for matrix operations
    template <typename T, template <typename> class Operation>
    struct Matrix {
        // Use the Operation template to perform the matrix addition
        Operation<T> operation;

        void addMatrices(const std::vector<std::vector<T>>& matrix1, const std::vector<std::vector<T>>& matrix2) {
            int rows = matrix1.size();
            int cols = matrix1[0].size();
            std::vector<std::vector<T>> result(rows, std::vector<T>(cols));

            operation.add(matrix1, matrix2, result);

            // Output the result matrix
            for (const auto& row : result) {
                for (const auto& element : row) {
                    std::cout << element << " ";
                }
                std::cout << std::endl;
            }
        }
    };
}

int main() {
    // Instantiate Matrix with MatrixAdd for matrix addition
    Namespace::Matrix<int, Namespace::MatrixAdd> matrixOp;

    std::vector<std::vector<int>> matrix1 = {{1, 2}, {3, 4}};
    std::vector<std::vector<int>> matrix2 = {{5, 6}, {7, 8}};

    matrixOp.addMatrices(matrix1, matrix2);  // Perform matrix addition and output the result
}
```

#### **Explanation:**
- The `MatrixAdd` template class is responsible for performing element-wise matrix addition.
- The `Matrix` template class is the outer class that accepts the `MatrixAdd` template as a template parameter and uses it to add matrices.
- The method `matrixOp.addMatrices(matrix1, matrix2)` performs matrix addition and outputs the resulting matrix.

### **2. `this->template<Type> FunctionName(Type(?))`**

This example demonstrates how to use dependent templates inside a class to perform a computation, in this case, summing the elements of a sequence. The function `FunctionName` calculates the sum of all elements in the given sequence.

#### **Example Code:**

```cpp
#include <iostream>
#include <vector>

template <typename T>
struct Calculator {
    // Template member function to calculate the sum of a sequence
    template <typename U>
    U calculateSum(const std::vector<U>& values) {
        U sum = 0;
        for (const auto& value : values) {
            sum += value;
        }
        return sum;
    }

    // Calling the dependent template function
    void compute() {
        std::vector<int> values = {1, 2, 3, 4, 5};
        std::cout << "Sum of values: " << this->template calculateSum<int>(values) << std::endl;
    }
};

int main() {
    Calculator<double> calc;
    calc.compute();  // Calculate and output the sum of the sequence
}
```

#### **Explanation:**
- The `Calculator` template class includes a template member function `calculateSum`, which calculates the sum of a sequence.
- Inside the `compute()` function, we use `this->template calculateSum<int>(values)` to explicitly call the `calculateSum` function with `int` as the template argument.
- The `this->template` syntax is necessary to ensure the compiler correctly resolves dependent template names.

### **3. `Class<Type> Instance; Instance.template FunctionName<Type2>(Type2(?))`**

In this example, we will use template instantiation to create a generic calculator that performs arithmetic operations. The template class `Calculator` will be instantiated with a specific type and perform various arithmetic operations like addition and multiplication.

#### **Example Code:**

```cpp
#include <iostream>

template <typename T>
class Calculator {
public:
    // Template member function for addition
    template <typename U>
    U add(U a, U b) {
        return a + b;
    }

    // Template member function for multiplication
    template <typename U>
    U multiply(U a, U b) {
        return a * b;
    }
};

int main() {
    // Instantiate the Calculator template with 'int'
    Calculator<int> calc;

    // Call the add operation with 'double' type
    double sum = calc.template add<double>(3.14, 2.71);
    std::cout << "Sum (double): " << sum << std::endl;

    // Call the multiply operation with 'float' type
    float product = calc.template multiply<float>(1.5f, 2.0f);
    std::cout << "Product (float): " << product << std::endl;
}
```

#### **Explanation:**
- The `Calculator` template class has two template member functions: `add` and `multiply`, each performing basic arithmetic operations.
- In the `main()` function, we instantiate `Calculator<int>`, and then use `template` to explicitly specify the types for the arithmetic operations (such as `double` for addition and `float` for multiplication).
- Using `template` ensures the correct template parameters are passed to the member functions.

---

1. **Nested Template Types**: We used a `template <typename> class Operation` to implement matrix addition with nested templates. This technique allows us to perform operations on matrices of various types.
2. **Dependent Template Member Functions**: By using `this->template`, we demonstrated how to call template functions that depend on class template parameters, calculating the sum of a sequence of numbers.
3. **Template Instantiation and Function Calls**: We showed how to instantiate a template class and call template member functions with explicit template parameters for various arithmetic operations

---

### **Homework**

1. **Template Instantiation with SFINAE**:  
   Write a template function that takes a parameter of any type and returns a value based on whether the type is integral (using `std::is_integral`). The function should add two numbers if they are integral types and print an error message otherwise.

2. **Using CRTP**:  
   Create a base class template `Shape` that uses CRTP to enable compile-time polymorphism. Create derived classes `Circle` and `Rectangle`, and implement a `draw` function in each derived class. Demonstrate the use of static polymorphism by calling `draw` on objects of the derived classes in `main()`.

3. **Nested Templates**:  
   Write a class template `Wrapper` that accepts a nested template as a parameter. Use it with a vector and list as the template arguments to store integers, then iterate over both containers and print their values.

4. **Template Aliases**:  
   Create a type alias `IntList` for `std::list<int>`, and define a function that uses this alias to add values to a list. Print the contents of the list after adding values.

5. **Variadic Templates**:  
   Write a variadic template function `max` that returns the maximum value among all the arguments passed to it. The function should handle different types of arguments and print the maximum value.

6. **`Namespace::template` Usage**:  
   Demonstrate the usage of the `Namespace::template` syntax in a class template that calls a function template inside it. Use a template function `sum` that takes a list of values and returns their sum, and ensure that the function is called correctly using `Namespace::template`.



---




### 第13课：高级 C++ 模板技术

### 介绍

在之前的课程中，我们介绍了函数模板作为编写通用、可重用代码的一种方式，可以适用于任何数据类型。我们看到了基本的例子，例如使用函数模板进行加法或乘法操作。然而，模板的功能远不止于此。今天，我们将探讨C++中函数模板的一些高级特性，这些特性将帮助你编写更强大、更高效、更灵活的代码。

我们将从回顾函数模板的核心概念开始，然后重点介绍一些关键的高级特性，例如：

- **模板特化**：为特定类型定制模板。
- **默认模板参数**：为模板参数提供默认值。
- **函数模板重载**：为同一函数名称使用多个模板。
- **显式模板实例化**：手动指定模板参数。
- **返回类型推导**：使用`auto`简化返回类型。

### 1. 回顾：基本的函数模板

如果你需要快速回顾一下，函数模板允许你编写适用于任何数据类型的函数。以下是一个简单的加法函数模板示例：

```cpp
#include <iostream>

// 加法函数模板
template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    int intResult = add(3, 4);  // 使用int类型
    double doubleResult = add(3.5, 4.5);  // 使用double类型

    std::cout << "整数结果: " << intResult << std::endl;
    std::cout << "浮动结果: " << doubleResult << std::endl;
    return 0;
}
```

这是一个基本的函数模板，其中`add`函数适用于任何类型，只要两个参数的类型相同（`T`）。

### 2. 模板特化：为特定类型定制模板

虽然函数模板适用于多种类型，但有时你可能希望为特定类型定制其行为，这时就需要用到**模板特化**。

#### 示例：为`std::string`进行特化

假设你希望`add`函数在处理两个字符串时表现不同。你可以为`std::string`进行模板特化：

```cpp
#include <iostream>
#include <string>

// 主模板
template <typename T>
T add(T a, T b) {
    return a + b;
}

// 为std::string进行特化
template <>
std::string add<std::string>(std::string a, std::string b) {
    return a + " " + b;  // 用空格连接字符串
}

int main() {
    std::string strResult = add<std::string>("Hello", "World");
    std::cout << "字符串结果: " << strResult << std::endl;  // 输出 "Hello World"
    return 0;
}
```

在这个例子中，对于任何非`std::string`类型，使用通用的`add`函数；但对于`std::string`类型，特化版本会将字符串用空格连接。

### 3. 默认模板参数：为参数提供默认值

有时，你可能希望为模板参数提供默认值。当你不需要指定所有模板参数时，这非常有用，可以简化函数调用。

#### 示例：默认模板参数

```cpp
#include <iostream>

// 带默认参数的模板
template <typename T = int>
T multiply(T a, T b) {
    return a * b;
}

int main() {
    std::cout << multiply(3, 4) << std::endl;  // 默认使用int类型
    std::cout << multiply(3.5, 4.5) << std::endl;  // 使用double类型
    return 0;
}
```

在这个例子中，如果没有指定模板参数，`multiply`函数模板默认为`int`类型。你仍然可以根据需要显式指定其他类型，如`double`。

### 4. 函数模板重载：处理不同的函数签名

你可以像重载常规函数一样重载函数模板。这允许你为函数模板定义多个版本，接受不同类型或数量的参数。

#### 示例：为不同的参数类型重载模板

```cpp
#include <iostream>

// 通用模板
template <typename T>
T add(T a, T b) {
    return a + b;
}

// 为不同类型的参数重载模板
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;  // 处理混合类型
}

int main() {
    std::cout << add(3, 4) << std::endl;          // 使用通用模板 (int + int)
    std::cout << add(3.5, 4) << std::endl;        // 使用重载模板 (double + int)
    return 0;
}
```

在这个例子中，第一个`add`函数适用于相同类型的参数，而第二个`add`函数可以处理不同类型的参数（例如`int`和`double`），并且使用`decltype`推导返回类型。

### 5. 显式模板实例化：手动指定模板类型

有时，您可能希望强制编译器使用特定类型实例化模板。这可以通过 **显式模板实例化** 来实现。

#### 示例：显式实例化

```cpp
#include <iostream>

// 乘法模板
template <typename T>
T multiply(T a, T b) {
    return a * b;
}

// 显式实例化
template int multiply<int>(int, int);

int main() {
    int result = multiply(3, 4);  // 使用显式实例化版本
    std::cout << result << std::endl;
    return 0;
}
```

在此示例中，我们显式实例化了 `multiply` 模板为 `int` 类型，以避免自动实例化或控制模板使用的位置。

#### 6. 非类型模板参数

C++ 中的模板不仅限于类型；它们还可以接受 **非类型参数**，如整数、指针或引用。当您希望模板在编译时感知特定常量或大小时，这非常有用。

**示例：带有非类型参数的模板**

```cpp
#include <iostream>

template <typename T, int size>
class Array {
private:
    T arr[size];
public:
    void set(int index, T value) {
        arr[index] = value;
    }

    T get(int index) const {
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
    Array<int, 5> intArray;  // 一个大小为 5 的整数数组
    for (int i = 0; i < 5; ++i) {
        intArray.set(i, i * 10);  // 设置值为 0, 10, 20, ...
    }
    intArray.print();  // 打印数组

    return 0;
}
```

在此示例中，`Array` 是一个模板，接受一个整数参数 `size` 和一个类型 `T`。这使得模板能够创建任何类型和大小的数组。大小在编译时已知，这有助于优化数组的内存使用。

#### 7. 函数模板特化

模板特化允许您为特定类型定义函数模板的行为。当通用模板对于某些数据类型的行为不符合预期时，这非常有用。

**示例：模板特化**

```cpp
#include <iostream>

template <typename T>
T print(T value) {
    return value;
}

// 对 char 类型的特化
template <>
char print<char>(char value) {
    return std::toupper(value); // 将 char 转换为大写字母
}

int main() {
    std::cout << print(42) << std::endl;  // 打印 42
    std::cout << print('a') << std::endl;  // 打印 'A'
    return 0;
}
```

在这个例子中，我们为 `char` 类型特化了 `print` 模板，确保在打印时将小写字母转换为大写字母。

#### 8. 函数模板与重载

函数模板可以像普通函数一样进行重载。重载允许您创建多个具有相同名称但不同参数类型或数量的函数模板。

**示例：重载函数模板**

```cpp
#include <iostream>

// 用于加法的通用模板
template <typename T>
T add(T a, T b) {
    return a + b;
}

// 针对不同数量参数的重载
template <typename T>
T add(T a, T b, T c) {
    return a + b + c;
}

int main() {
    std::cout << add(3, 4) << std::endl;      // 使用两个参数
    std::cout << add(1.1, 2.2, 3.3) << std::endl; // 使用三个参数
    return 0;
}
```

在这里，我们对 `add` 模板进行了重载，以处理两个参数和三个参数，使模板更加灵活。

#### 9. 模板元编程（可选的高级特性）

模板元编程允许您使用模板在编译时执行计算。这一高级特性对于优化代码或创建编译时常量非常有用。

**示例：编译时阶乘计算**

```cpp
#include <iostream>

template <int N>
struct Factorial {
    static const int value = N * Factorial<N - 1>::value;
};

template <>
struct Factorial<0> {
    static const int value = 1;  // 基本情况
};

int main() {
    std::cout << Factorial<5>::value << std::endl; // 输出 120 (5! = 120)
    return 0;
}
```

这个例子使用模板递归在编译时计算阶乘。

---

### 1. **SFINAE（Substitution Failure Is Not An Error）**

SFINAE是理解C++模板灵活性和容错性的重要概念。我们将通过以下问题深入探讨：**C++如何在存在多个候选模板时知道该实例化哪个模板？**

#### 为什么SFINAE重要？

在模板中，如果模板参数的替换失败（因为它不符合我们设置的约束条件），编译器不会抛出错误，而是默默“失败”，并尝试下一个候选模板。这被称为**Substitution Failure Is Not An Error**。

我们可以利用SFINAE创建更强大和高效的模板，仅在特定条件下实例化。

**示例**：我们将编写代码，使用SFINAE根据传入的类型有条件地启用或禁用模板实例化。可以通过**类型特征**来演示这一点。

```cpp
#include <iostream>
#include <type_traits>

// 基于SFINAE的模板，仅允许算术类型（int, float等）
template <typename T>
typename std::enable_if<std::is_arithmetic<T>::value, T>::type
add(T a, T b) {
    return a + b;
}

// 仅在非算术类型下启用
template <typename T>
typename std::enable_if<!std::is_arithmetic<T>::value, T>::type
add(T a, T b) {
    std::cout << "不支持对非算术类型进行此操作。" << std::endl;
    return T();
}

int main() {
    std::cout << add(1, 2) << std::endl;        // 对算术类型有效
    std::cout << add("Hello", " World!") << std::endl; // 对非算术类型有效
    return 0;
}
```

**解释**：
- 第一个`add`函数仅在`T`类型是算术类型时实例化（`std::is_arithmetic<T>::value`为`true`）。
- 第二个`add`函数仅在`T`类型是非算术类型时实例化。
这演示了SFINAE如何通过类型特征控制哪个模板被实例化。

**SFINAE与模板实例化逻辑**：
- **第1步**：编译器查看`main()`中的函数调用，对于`add(1, 2)`，编译器会尝试匹配第一个模板。
- **第2步**：类型特征`std::is_arithmetic<int>::value`和`std::is_arithmetic<double>::value`返回`true`，因此实例化第一个模板。
- **第3步**：对于`add("Hello", "World!")`，由于`std::is_arithmetic<const char*>::value`为`false`，编译器跳过第一个模板，实例化第二个模板，并打印非算术类型的消息。

这个例子展示了SFINAE在C++模板系统中的重要性，使得模板实例化更具类型安全性和灵活性。

### 2. **Curiously Recurring Template Pattern (CRTP)**

CRTP是C++中的一个强大技术，其中一个类模板从其自身派生，并使用自己的模板参数作为基类。它通常用于实现静态多态性，避免运行时开销，并提升性能。我们将更深入地探讨CRTP。

**CRTP示例**：实现一个可以使用编译时多态性的`Shape`类：

```cpp
#include <iostream>

// 基类模板
template <typename Derived>
class Shape {
public:
    void draw() const {
        static_cast<const Derived*>(this)->drawImpl();
    }
};

// 派生类
class Circle : public Shape<Circle> {
public:
    void drawImpl() const {
        std::cout << "绘制圆形" << std::endl;
    }
};

// 另一个派生类
class Square : public Shape<Square> {
public:
    void drawImpl() const {
        std::cout << "绘制方形" << std::endl;
    }
};

int main() {
    Circle c;
    Square s;
    
    c.draw();  // 输出 "绘制圆形"
    s.draw();  // 输出 "绘制方形"

    return 0;
}
```

**解释**：
- `Shape<Derived>`是**基类**，并将`Derived`类作为模板参数。
- 在`Shape`类的`draw`方法中，我们通过将`this`强制转换为派生类类型，并调用`drawImpl`方法，实现了**静态多态性**。
- 这允许派生类实现自己的绘制逻辑（`drawImpl`），而基类（`Shape`）定义了接口。

### 3. **模板元编程与类型特征**

这部分将扩展模板的高级用法，通过类型进行编译时操作。我们将探讨**类型特征**，如`std::enable_if`、`std::is_same`和`std::is_integral`，以有条件地启用或禁用函数。

**示例：使用`std::enable_if`和`std::is_integral`**：

```cpp
#include <iostream>
#include <type_traits>

// 仅对整数类型启用
template <typename T>
typename std::enable_if<std::is_integral<T>::value, void>::type
printType(T val) {
    std::cout << "整数类型: " << val << std::endl;
}

int main() {
    printType(5);    // 对整数有效
    // printType(5.5); // 注释掉这行会导致编译时错误（禁用非整数类型）
}
```

在这个例子中，我们使用**模板元编程**将`printType`函数限制为仅对整数类型有效，基于`std::is_integral<T>::value`的结果。

### 4. **变长模板 (Variadic Templates)**

变长模板允许模板接受任意数量的参数。这是C++中实现灵活函数或类模板的常用技术，能够处理不确定数量的参数。

**示例：使用变长模板求多个参数的和**：

```cpp
#include <iostream>

template <typename... Args>
auto sum(Args... args) {
    return (args + ...);  // 折叠表达式，求和所有参数
}

int main() {
    std::cout << sum(1, 2, 3, 4, 5) << std::endl;  // 输出 15
    return 0;
}
```

这个示例使用C++17中的**折叠表达式**，将所有传递给`sum`的参数求和。

### 5. **模板别名与`using`语法**

`using`关键字允许我们定义模板别名，使代码更具可读性和可维护性。特别是在简化复杂模板声明时非常有用。

**示例：创建模板别名**：

```cpp
#include <iostream>
#include <vector>

// 定义一个整型向量的别名
using IntVector = std::vector<int>;

int main() {
    IntVector v = {1, 2, 3, 4, 5};
    for (auto val : v) {
        std::cout << val << " ";
    }
    return 0;
}
```

在这个例子中，`IntVector`是`std::vector<int>`的更具可读性的别名。

### 6. **模板模板参数 (Template Template Parameters)**

模板模板参数允许模板接受其他模板作为参数。这是一种高级技术，当你希望将容器类型或其他模板作为参数传递时非常有用。

**示例：使用模板模板参数**：

```cpp
#include <iostream>
#include <vector>

// 模板模板参数（接受另一个模板的模板）
template <template <typename> class Container, typename T>
void printContainer(const Container<T>& container) {
    for (const auto& elem : container) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;
}

int main() {
    std::vector<int> vec = {1, 2, 3};
    printContainer(vec);  // 可以与任何容器类型一起使用（如vector、list等）
}
```

在这个例子中，`printContainer`可以接受任何容器类型，如`std::vector`、`std::list`等，这得益于**模板模板参数**。

---

### 第13课总结

在**第13课**中，我们将探讨C++模板的高级技术，包括：

- **SFINAE**：通过使用类型特征控制模板实例化过程，理解模板如何工作。
- **Curiously Recurring Template Pattern (CRTP)**：通过编译时类层次结构实现静态多态性。
- **模板元编程**：使用**类型特征**和**enable_if**实现编译时逻辑。
- **变长模板**：创建可以接受任意数量参数的灵活函数。
- **模板别名**：简化复杂的模板声明。
- **模板模板参数**：将模板作为参数传递给其他模板。

这些技术将帮助你编写更加灵活、高效和类型安全的代码。

---

## **高级 C++ 模板编程教程：嵌套模板、依赖模板和模板实例化计算**

在 C++ 中，模板编程提供了强大的抽象和灵活性，使开发者能够为各种计算任务创建通用且高效的解决方案。在本教程中，我们将探讨三种高级模板使用方法：嵌套模板、依赖模板和模板实例化。这些例子展示了如何利用模板进行有意义的计算任务，如矩阵加法、求和计算和一般的算术运算。

### **1. 嵌套模板：`Name<Type, Namespace::template Name2<Type2>>`**

在这个例子中，我们将使用嵌套模板实现矩阵加法。外部模板 `Name` 将处理矩阵加法逻辑，而嵌套模板 `Name2` 将执行矩阵的逐元素加法。

#### **示例代码：**

```cpp
#include <iostream>
#include <vector>

namespace Namespace {
    // 用于执行矩阵加法的模板类
    template <typename T>
    struct MatrixAdd {
        void add(const std::vector<std::vector<T>>& matrix1, const std::vector<std::vector<T>>& matrix2, std::vector<std::vector<T>>& result) {
            int rows = matrix1.size();
            int cols = matrix1[0].size();

            for (int i = 0; i < rows; ++i) {
                for (int j = 0; j < cols; ++j) {
                    result[i][j] = matrix1[i][j] + matrix2[i][j];  // 逐元素加法
                }
            }
        }
    };

    // 接受 MatrixAdd 模板进行矩阵运算的外部模板类
    template <typename T, template <typename> class Operation>
    struct Matrix {
        // 使用 Operation 模板进行矩阵加法
        Operation<T> operation;

        void addMatrices(const std::vector<std::vector<T>>& matrix1, const std::vector<std::vector<T>>& matrix2) {
            int rows = matrix1.size();
            int cols = matrix1[0].size();
            std::vector<std::vector<T>> result(rows, std::vector<T>(cols));

            operation.add(matrix1, matrix2, result);

            // 输出结果矩阵
            for (const auto& row : result) {
                for (const auto& element : row) {
                    std::cout << element << " ";
                }
                std::cout << std::endl;
            }
        }
    };
}

int main() {
    // 使用 MatrixAdd 实例化 Matrix 进行矩阵加法
    Namespace::Matrix<int, Namespace::MatrixAdd> matrixOp;

    std::vector<std::vector<int>> matrix1 = {{1, 2}, {3, 4}};
    std::vector<std::vector<int>> matrix2 = {{5, 6}, {7, 8}};

    matrixOp.addMatrices(matrix1, matrix2);  // 执行矩阵加法并输出结果
}
```

#### **解释：**
- `MatrixAdd` 模板类负责执行逐元素的矩阵加法。
- `Matrix` 模板类是外部类，它接受 `MatrixAdd` 模板作为模板参数，并利用它进行矩阵加法。
- 方法 `matrixOp.addMatrices(matrix1, matrix2)` 执行矩阵加法并输出结果矩阵。

### **2. `this->template<Type> FunctionName(Type(?))`**

本例展示了如何在类内部使用依赖模板来执行计算，在这个例子中，我们计算序列元素的和。函数 `FunctionName` 计算给定序列中所有元素的和。

#### **示例代码：**

```cpp
#include <iostream>
#include <vector>

template <typename T>
struct Calculator {
    // 计算序列和的模板成员函数
    template <typename U>
    U calculateSum(const std::vector<U>& values) {
        U sum = 0;
        for (const auto& value : values) {
            sum += value;
        }
        return sum;
    }

    // 调用依赖模板函数
    void compute() {
        std::vector<int> values = {1, 2, 3, 4, 5};
        std::cout << "Sum of values: " << this->template calculateSum<int>(values) << std::endl;
    }
};

int main() {
    Calculator<double> calc;
    calc.compute();  // 计算并输出序列和
}
```

#### **解释：**
- `Calculator` 模板类包含一个模板成员函数 `calculateSum`，该函数计算序列中所有元素的和。
- 在 `compute()` 函数中，我们使用 `this->template calculateSum<int>(values)` 显式调用 `calculateSum` 函数，并传入 `int` 作为模板参数。
- `this->template` 语法用于确保编译器正确解析依赖模板名称。

### **3. `Class<Type> Instance; Instance.template FunctionName<Type2>(Type2(?))`**

在本例中，我们将使用模板实例化创建一个通用计算器，执行算术运算。模板类 `Calculator` 将实例化为特定类型并执行加法和乘法等算术运算。

#### **示例代码：**

```cpp
#include <iostream>

template <typename T>
class Calculator {
public:
    // 用于加法的模板成员函数
    template <typename U>
    U add(U a, U b) {
        return a + b;
    }

    // 用于乘法的模板成员函数
    template <typename U>
    U multiply(U a, U b) {
        return a * b;
    }
};

int main() {
    // 用 'int' 实例化 Calculator 模板
    Calculator<int> calc;

    // 使用 'double' 类型调用加法操作
    double sum = calc.template add<double>(3.14, 2.71);
    std::cout << "Sum (double): " << sum << std::endl;

    // 使用 'float' 类型调用乘法操作
    float product = calc.template multiply<float>(1.5f, 2.0f);
    std::cout << "Product (float): " << product << std::endl;
}
```

#### **解释：**
- `Calculator` 模板类具有两个模板成员函数：`add` 和 `multiply`，分别执行加法和乘法运算。
- 在 `main()` 函数中，我们实例化 `Calculator<int>`，然后使用 `template` 显式指定算术操作的类型（例如 `double` 用于加法，`float` 用于乘法）。
- 使用 `template` 确保正确的模板参数传递给成员函数。

1. **嵌套模板类型**：我们使用了 `template <typename> class Operation` 来实现矩阵加法，并演示了如何通过嵌套模板对不同类型的矩阵进行操作。
2. **依赖模板成员函数**：通过使用 `this->template`，我们展示了如何调用依赖于类模板参数的模板函数，计算数列的和。
3. **模板实例化和函数调用**：我们展示了如何实例化模板类，并显式指定模板参数来执行各种算术运算。

### **作业**

1. **使用 SFINAE 进行模板实例化**：  
   编写一个模板函数，接受任何类型的参数，并基于该类型是否为整数类型（使用 `std::is_integral`）返回一个值。如果参数为整数类型，则返回两数之和，否则输出错误消息。

2. **使用 CRTP**：  
   创建一个基类模板 `Shape`，使用 CRTP 实现编译时多态。创建派生类 `Circle` 和 `Rectangle`，并在每个派生类中实现 `draw` 函数。在 `main()` 函数中调用派生类对象的 `draw` 函数，演示静态多态性。

3. **嵌套模板**：  
   编写一个类模板 `Wrapper`，接受一个嵌套模板作为参数。使用向量和列表作为模板参数存储整数，然后遍历这两个容器并打印其值。

4. **模板别名**：  
   为 `std::list<int>` 创建一个类型别名 `IntList`，并定义一个函数，使用该别名将值添加到列表中。添加值后打印列表内容。

5. **变长模板**：  
   编写一个变长模板函数 `max`，返回所有传入参数中的最大值。该函数应能处理不同类型的参数，并输出最大值。

6. **`Namespace::template` 使用**：  
   演示在类模板中使用 `Namespace::template` 语法，调用其中的函数模板。使用一个模板函数 `sum`，接受一个值的列表并返回它们的和，并确保正确调用该函数。