# Lesson 11: Operator Overloading in C++ Classes

## Operator Overloading

Operator overloading is a powerful feature in C++ that allows programmers to define operations for custom types.
This enables objects of custom types to be used more naturally, resembling the behavior of built-in types.

### 1. Arithmetic Operator Overloading in the ComplexNumber Class
Arithmetic operator overloading is commonly used to enable custom types to perform mathematical operations such as addition, subtraction, multiplication, and division.

```cpp
#include <iostream>

class ComplexNumber {
public:
    double real, imag;

    ComplexNumber(double r = 0.0, double i = 0.0) : real(r), imag(i) {}

    // Addition
    ComplexNumber operator+(const ComplexNumber& other) const {
        return ComplexNumber(real + other.real, imag + other.imag);
    }

    // Subtraction
    ComplexNumber operator-(const ComplexNumber& other) const {
        return ComplexNumber(real - other.real, imag - other.imag);
    }

    // Multiplication
    ComplexNumber operator*(const ComplexNumber& other) const {
        return ComplexNumber(real * other.real - imag * other.imag,
                       real * other.imag + imag * other.real);
    }

    // Division
    ComplexNumber operator/(const ComplexNumber& other) const {
        double denominator = other.real * other.real + other.imag * other.imag;
        return ComplexNumber((real * other.real + imag * other.imag) / denominator,
                       (imag * other.real - real * other.imag) / denominator);
    }

    // Print function
    void print() const {
        std::cout << real << " + " << imag << "i" << std::endl;
    }
};

int main() {
    ComplexNumber c1(4.0, 3.0), c2(2.0, 1.0);
    ComplexNumber result;

    result = c1 + c2;
    result.print();

    result = c1 - c2;
    result.print();

    result = c1 * c2;
    result.print();

    result = c1 / c2;
    result.print();

    return 0;
}
```

### 2. Comparison and Logical Operator Overloading in the Point Class
Comparison operator overloading is used to compare two objects.
Logical operator overloading enables evaluation of objects based on custom conditions (||, &&, !).

```cpp
#include <iostream>

class Point {
public:
    int x, y;

    Point(int x = 0, int y = 0) : x(x), y(y) {}

    // Equal to
    bool operator==(const Point& other) const {
        return x == other.x && y == other.y;
    }

    // Not equal to
    bool operator!=(const Point& other) const {
        return !(*this == other);
    }

    // Less than
    bool operator<(const Point& other) const {
        return x < other.x || (x == other.x && y < other.y);
    }

    // Greater than
    bool operator>(const Point& other) const {
        return other < *this;
    }

    // Less than or equal to
    bool operator<=(const Point& other) const {
        return !(other < *this);
    }

    // Greater than or equal to
    bool operator>=(const Point& other) const {
        return !(*this < other);
    }

    // Logical NOT, checks if the point is at the origin
    bool operator!() const {
        return x == 0 && y == 0;
    }

    // Logical AND, checks if both points are at the origin
    bool operator&&(const Point& other) const {
        return (!*this) && (!other);
    }

    // Logical OR, checks if at least one of the points is at the origin
    bool operator||(const Point& other) const {
        return (!*this) || (!other);
    }
};

void Test()
{
    Point p1(1, 2), p2(2, 3);
    std::cout << "p1 == p2: " << (p1 == p2) << std::endl;
    std::cout << "p1 != p2: " << (p1 != p2) << std::endl;
    std::cout << "p1 < p2: " << (p1 < p2) << std::endl;
    std::cout << "p1 > p2: " << (p1 > p2) << std::endl;
    std::cout << "p1 <= p2: " << (p1 <= p2) << std::endl;
    std::cout << "p1 >= p2: " << (p1 >= p2) << std::endl;
}

void Test2()
{
    Point p1(3,4), p2(0, 0), p3(0, 0);
    std::cout << "p1 is at origin: " << (!p1 ? "No" : "Yes") << std::endl;
    std::cout << "p2 is at origin: " << (!p2 ? "No" : "Yes") << std::endl;
    std::cout << "Both p1 and p2 are at origin: " << (p1 && p2 ? "Yes" : "No") << std::endl;
    std::cout << "Either p2 or p3 is at origin: " << (p2 || p3 ? "Yes" : "No") << std::endl;
}

int main() {
    Test();
    Test2();
    return 0;
}
```

### 3. BitSet Class: Bitwise Operator Overloading
For the `BitSet` class, we can include overloads for bitwise operations such as AND (`&`), OR (`|`), and NOT (`~`).
The `BitSet` class manages bits using an array of unsigned integers, where each element of the array can store 32 bits.
The constructor calculates the size of the array needed to store the specified number of bits and initializes the array to zero.
This method supports a broader range of bit operations while fully utilizing C++'s RAII mechanism to ensure proper resource management.

Bitwise operator overloading is utilized for performing bit manipulations.

```cpp
#include <iostream>
#include <cstring>

class BitSet {
private:
    unsigned int* bits;
    size_t size;  // Size of the array

public:
    // Constructor to initialize the size of the bitset
    BitSet(size_t numBits) : size((numBits + 31) / 32) {
        bits = new unsigned int[size];
        std::memset(bits, 0, size * sizeof(unsigned int));
    }

    // Destructor to free resources
    ~BitSet() {
        delete[] bits;
    }

    // Copy constructor
    BitSet(const BitSet& other) : size(other.size) {
        bits = new unsigned int[size];
        std::memcpy(bits, other.bits, size * sizeof(unsigned int));
    }

    // Assignment operator
    BitSet& operator=(const BitSet& other) {
        if (this != &other) {
            BitSet tmp(other);  // Using copy constructor to create a temporary instance
            std::swap(bits, tmp.bits);
            std::swap(size, tmp.size);
        }
        return *this;
    }

    // Overload bitwise AND operator
    BitSet operator&(const BitSet& other) const {
        BitSet result(std::min(size, other.size) * 32);
        for (size_t i = 0; i < result.size; i++) {
            result.bits[i] = this->bits[i] & other.bits[i];
        }
        return result;
    }

    // Overload bitwise OR operator
    BitSet operator|(const BitSet& other) const {
        BitSet result(std::max(size, other.size) * 32);
        for (size_t i = 0; i < result.size; i++) {
            result.bits[i] = this->bits[i] | other.bits[i];
        }
        return result;
    }

    // Bitwise AND assignment operator overload
    BitSet& operator&=(const BitSet& other) {
        size_t minSize = std::min(size, other.size);
        for (size_t i = 0; i < minSize; i++) {
            bits[i] &= other.bits[i];
        }
        return *this;
    }

    // Bitwise OR assignment operator overload
    BitSet& operator|=(const BitSet& other) {
        size_t minSize = std::min(size, other.size);
        for (size_t i = 0; i < minSize; i++) {
            bits[i] |= other.bits[i];
        }
        return *this;
    }

    // Bitwise NOT operator overload
    BitSet operator~() const {
        BitSet result(size * 32);
        for (size_t i = 0; i < size; i++) {
            result.bits[i] = ~bits[i];
        }
        return result;
    }

    // Print function
    void print() const {
        for (size_t i = 0; i < size; i++) {
            std::cout << std::hex << bits[i] << " ";
        }
    }
};

void Test()
{
    BitSet b1(10), b2(12);
    BitSet result = b1 & b2;
    result.print();  // Should output: 0 (all bits cleared)
    result = b1 | b2;
    result.print();  // Should output: 0 (initial state of b1 and b2 is 0)
}

void Test2()
{
    BitSet b1(64), b2(64);
    b1 |= BitSet(64);  // BitSet is initialized to all 0s, so this doesn't actually change b1
    b1.print();
    b2 &= b1;  // Likewise, b2 remains all 0s
    b2.print();
    BitSet b3 = ~b2;  // Bitwise NOT operation
    b3.print();  // Expected to print all 1s (depending on the size and specific implementation details)
}

int main()
{
    Test();
    Test2();
    return 0;
}
```

### 4. IntArray Class: Subscript Operator Overloading
The subscript operator is often overloaded to provide array-like access.
For example, overloading the `[]` operator for a class representing a container:

```cpp
#include <iostream>
#include <cassert>

class IntArray {
private:
    int* array;
    int size;

public:
    IntArray(int size) : size(size) {
        array = new int[size];
    }

    ~IntArray() {
        delete[] array;
    }

    // Overload subscript operator
    int& operator[](int index) {
        assert(index >= 0 && index < size);
        return array[index];
    }
};

int main() {
    IntArray myArray(5);
    for (int i = 0; i < 5; ++i) {
        myArray[i] = i * i;
   

 }
    std::cout << "myArray[2] = " << myArray[2] << std::endl;
    return 0;
}
```

## Overloading the New and Delete Operators Inside a Class with Custom Memory Allocation Mechanism

### Definition and Operator Overloading in the Vector3 Class
The following `Vector3` class includes three floating points representing a vector in three-dimensional space. We will overload the `new` and `delete` operators, as well as their array versions `new[]` and `delete[]` for this class.

```cpp
#include <iostream>
#include <cstdlib>  // For std::malloc and std::free

class Vector3 {
public:
    float x, y, z;

    Vector3(float _x = 0.0f, float _y = 0.0f, float _z = 0.0f) : x(_x), y(_y), z(_z) {
        std::cout << "Vector3 constructed: " << x << ", " << y << ", " << z << std::endl;
    }

    ~Vector3() {
        std::cout << "Vector3 destroyed: " << x << ", " << y << ", " << z << std::endl;
    }

    // Overloading the class's new operator
    static void* operator new(size_t size) {
        std::cout << "Custom new for Vector3 of size " << size << std::endl;
        void* p = std::malloc(size);
        if (!p) {
            throw std::bad_alloc();
        }
        return p;
    }

    // Overloading the class's delete operator
    static void operator delete(void* p) {
        std::cout << "Custom delete for Vector3" << std::endl;
        std::free(p);
    }

    // Overloading the array new operator
    static void* operator new[](size_t size) {
        std::cout << "Custom new[] for Vector3 array of size " << size << std::endl;
        void* p = std::malloc(size);
        if (!p) {
            throw std::bad_alloc();
        }
        return p;
    }

    // Overloading the array delete operator
    static void operator delete[](void* p) {
        std::cout << "Custom delete[] for Vector3 array" << std::endl;
        std::free(p);
    }
};

int main() {
    std::cout << "Creating a single Vector3 object:\n";
    Vector3* v = new Vector3(1.0f, 2.0f, 3.0f);
    delete v;

    std::cout << "\nCreating an array of Vector3 objects:\n";
    Vector3* vArray = new Vector3[2]{{1.0f, 2.0f, 3.0f}, {4.0f, 5.0f, 6.0f}};
    delete[] vArray;

    return 0;
}
```

In this example:
- We have overloaded the memory allocation (`new` and `new[]`) and deallocation (`delete` and `delete[]`) operators for individual objects and arrays of the `Vector3` class.
- Each creation and destruction of a `Vector3` object outputs information about its memory allocation and deallocation, as well as the coordinates of the object.
- This output allows for observation of the class instances' creation and destruction processes, enhancing the understanding of memory management.

This method offers great flexibility in controlling the lifecycle of object memory, making it highly suitable for scenarios requiring precise memory usage control, such as in game development, high-performance computing, and real-time systems.

## Internal Overloading of the New Operator in Classes: Difference Between Ordinary Usage and Placement New

In C++, `placement new` is a special form of the new operator that allows constructing an object on already allocated memory.
This method is mainly used in scenarios where an object needs to be created in a specific location, such as in memory pools, buffer reuse, or situations requiring special memory alignment.

Compared to the ordinary `new` operator, `placement new` does not allocate memory; it merely calls the constructor to build the object at a specified memory address.
This is fundamentally different from overloading the `new` operator inside a class, which is typically used to alter the way memory is allocated for objects or to track memory allocations.

### Differences between Ordinary New and Placement New:
1. **Ordinary Usage**: Allocates sufficient memory and calls the constructor to create the object.
2. **Placement New Usage**: Does not allocate memory but simply calls the constructor to construct the object at a given memory address (using previously allocated space on the heap to construct an instance of the class in place).

### Modifying the Previous IntArray Class:

To more clearly demonstrate the practical uses of `placement new` and memory management, we can use the previously mentioned `IntArray` class. This will help us better understand memory operations and member access.
Here is the implementation of the `IntArray` class, including the allocation and deal

location of its member array, as well as an example usage of `placement new`.

```cpp
#include <iostream>
#include <new>  // Includes support for placement new

class IntArray {
private:
    int* array;
    size_t size;  // Number of elements in the array

public:
    IntArray(size_t size) : size(size) {
        array = new int[size];  // Allocates memory
        std::cout << "IntArray constructed with size " << size << std::endl;
    }

    ~IntArray() {
        delete[] array;  // Releases memory
        std::cout << "IntArray destroyed" << std::endl;
    }

    int& operator[](size_t index) {
        return array[index];
    }

    void print() const {
        for (size_t i = 0; i < size; ++i) {
            std::cout << array[i] << " ";
        }
        std::cout << std::endl;
    }

    // Class-internal overloaded placement new
    static void* operator new(size_t size, void* place) {
        std::cout << "Placement new used\n";
        return place;  // Simply returns the passed memory address
    }

    static void* operator new(size_t size) {
        std::cout << "Normal new used\n";
        return ::operator new(size);
    }

    // Class-internal overloaded delete
    static void operator delete(void* pointer) {
        std::cout << "Memory freed\n";
        ::operator delete(pointer);
    }
};

int main() {
    // Using regular new
    IntArray* array1 = new IntArray(5);

    // Allocate enough memory to place an IntArray object, using placement new
    char* buffer = new char[sizeof(IntArray)];
    IntArray* array2 = new (buffer) IntArray(5);  // Using placement new

    // Access and modify array elements
    (*array1)[0] = 10;
    array1->print();

    (*array2)[1] = 20;
    array2->print();

    // Explicitly calling the destructor since placement new does not handle memory release
    array2->~IntArray();
    delete[] buffer;  // Manually releasing the allocated memory

    delete array1;  // Normally releasing memory and object
    return 0;
}
```

### The Pointer-to-Member Access Operator (`->`)
The pointer-to-member access operator (`->`) is a mechanism used to access members of a class through a pointer that points to an object of that class.
If you have a pointer to a class object, you can use the `->` operator to directly access its members.
For example, if `ptr` is a pointer to an `IntArray` object, then `ptr->print()` would call the `print` method of that `IntArray` object.
This usage is quite common, especially when dealing with dynamically allocated objects.
It is also equivalent to `(*class_object_pointer).member_name;`

In the code example above, we demonstrated how to use this operator to access an object's methods with `array1->print()` and `array2->print()`.
Additionally, by using `(*array1)[0] = 10;` and `(*array2)[1] = 20;`, we showed how to access and modify array elements by dereferencing the pointer and then using the subscript operator.

`I hope this makes the content more accessible to beginners, happy learning! `

---

# 第 11 课 C++ 类中的运算符重载

## 运算符重载

运算符重载是C++中一个强大的功能，它允许程序员为自定义类型定义运算符的操作。
这可以让自定义类型的对象使用起来更自然，更接近内置类型。

### 1. ComplexNumber类的算术运算符重载
算术运算符重载通常用于使自定义类型可以进行加、减、乘、除等数学运算。

```cpp
#include <iostream>

class ComplexNumber {
public:
    double real, imag;

    ComplexNumber(double r = 0.0, double i = 0.0) : real(r), imag(i) {}

    // 加法
    ComplexNumber operator+(const ComplexNumber& other) const {
        return ComplexNumber(real + other.real, imag + other.imag);
    }

    // 减法
    ComplexNumber operator-(const ComplexNumber& other) const {
        return ComplexNumber(real - other.real, imag - other.imag);
    }

    // 乘法
    ComplexNumber operator*(const ComplexNumber& other) const {
        return ComplexNumber(real * other.real - imag * other.imag,
                       real * other.imag + imag * other.real);
    }

    // 除法
    ComplexNumber operator/(const ComplexNumber& other) const {
        double denominator = other.real * other.real + other.imag * other.imag;
        return ComplexNumber((real * other.real + imag * other.imag) / denominator,
                       (imag * other.real - real * other.imag) / denominator);
    }

    // 打印函数
    void print() const {
        std::cout << real << " + " << imag << "i" << std::endl;
    }
};

int main() {
    ComplexNumber c1(4.0, 3.0), c2(2.0, 1.0);
    ComplexNumber result;

    result = c1 + c2;
    result.print();

    result = c1 - c2;
    result.print();

    result = c1 * c2;
    result.print();

    result = c1 / c2;
    result.print();

    return 0;
}
```

### 2. Point类的比较运算符和逻辑运算符重载
比较运算符重载用于比较两个对象。
逻辑运算符重载可以根据自定义条件评估对象(||, &&, !)。

```cpp
#include <iostream>

class Point {
public:
    int x, y;

    Point(int x = 0, int y = 0) : x(x), y(y) {}

    // 等于
    bool operator==(const Point& other) const {
        return x == other.x && y == other.y;
    }

    // 不等于
    bool operator!=(const Point& other) const {
        return !(*this == other);
    }

    // 小于
    bool operator<(const Point& other) const {
        return x < other.x || (x == other.x && y < other.y);
    }

    // 大于
    bool operator>(const Point& other) const {
        return other < *this;
    }

    // 小于等于
    bool operator<=(const Point& other) const {
        return !(other < *this);
    }

    // 大于等于
    bool operator>=(const Point& other) const {
        return !(*this < other);
    }

    // 逻辑非，检查点是否位于原点
    bool operator!() const {
        return x == 0 && y == 0;
    }

    // 逻辑与，检查两个点是否同时位于原点
    bool operator&&(const Point& other) const {
        return (!*this) && (!other);
    }

    // 逻辑或，检查两个点中至少有一个位于原点
    bool operator||(const Point& other) const {
        return (!*this) || (!other);
    }
};

void Test()
{
    Point p1(1, 2), p2(2, 3);
    std::cout << "p1 == p2: " << (p1 == p2) << std::endl;
    std::cout << "p1 != p2: " << (p1 != p2) << std::endl;
    std::cout << "p1 < p2: " << (p1 < p2) << std::endl;
    std::cout << "p1 > p2: " << (p1 > p2) << std::endl;
    std::cout << "p1 <= p2: " << (p1 <= p2) << std::endl;
    std::cout << "p1 >= p2: " << (p1 >= p2) << std::endl;
}

void Test2()
{
    Point p1(3, 4), p2(0, 0), p3(0, 0);
    std::cout << "p1 is at origin: " << (!p1 ? "No" : "Yes") << std::endl;
    std::cout << "p2 is at origin: " << (!p2 ? "No" : "Yes") << std::endl;
    std::cout << "Both p1 and p2 are at origin: " << (p1 && p2 ? "Yes" : "No") << std::endl;
    std::cout << "Either p2 or p3 is at origin: " << (p2 || p3 ? "Yes" : "No") << std::endl;
}

int main() {
    Test();
    Test2();
    return 0;
}
```

### 3. BitSet类的比特运算符重载
对于`BitSet`类，我们可以包括与(`&`), 或(`|`), 和非(`~`)运算符的重载。
`BitSet`类通过一个无符号整数数组来管理比特，其中数组的每个元素可以存储32个比特。
构造函数根据需要存储的比特数来计算数组的大小，并且清零数组。
这种方法可以支持更大范围的比特操作，同时充分利用C++的RAII机制确保资源正确管理

比特运算符重载可以用于实现位操作。

```cpp
#include <iostream>
#include <cstring>

class BitSet {
private:
    unsigned int* bits;
    size_t size;  // 数组的大小

public:
    // 构造函数，初始化比特集大小
    BitSet(size_t numBits) : size((numBits + 31) / 32) {
        bits = new unsigned int[size];
        std::memset(bits, 0, size * sizeof(unsigned int));
    }

    // 析构函数，负责资源释放
    ~BitSet() {
        delete[] bits;
    }

    // 拷贝构造函数
    BitSet(const BitSet& other) : size(other.size) {
        bits = new unsigned int[size];
        std::memcpy(bits, other.bits, size * sizeof(unsigned int));
    }

    // 赋值运算符
    BitSet& operator=(const BitSet& other) {
        if (this != &other) {
            BitSet tmp(other);  // 使用拷贝构造函数创建临时实例
            std::swap(bits, tmp.bits);
            std::swap(size, tmp.size);
        }
        return *this;
    }

    // 重载与运算符
    BitSet operator&(const BitSet& other) const {
        BitSet result(std::min(size, other.size) * 32);
        for (size_t i = 0; i < result.size; i++) {
            result.bits[i] = this->bits[i] & other.bits[i];
        }
        return result;
    }

    // 重载或运算符
    BitSet operator|(const BitSet& other) const {
        BitSet result(std::max(size, other.size) * 32);
        for (size_t i = 0; i < result.size; i++) {
            result.bits[i] = this->bits[i] | other.bits[i];
        }
        return result;
    }

    // 与赋值运算符重载
    BitSet& operator&=(const BitSet& other) {
        size_t minSize = std::min(size, other.size);
        for (size_t i = 0; i < minSize; i++) {
            bits[i] &= other.bits[i];
        }
        return *this;
    }

    // 或赋值运算符重载
    BitSet& operator|=(const BitSet& other) {
        size_t minSize = std::min(size, other.size);
        for (size_t i = 0; i < minSize; i++) {
            bits[i] |= other.bits[i];
        }
        return *this;
    }

    // 非运算符重载
    BitSet operator~() const {
        BitSet result(size * 32);
        for (size_t i = 0; i < size; i++) {
            result.bits[i] = ~bits[i];
        }
        return result;
    }

    // 打印功能
    void print() const {
        for (size_t i = 0; i < size; i++) {
            std::cout << std::hex << bits[i] << " ";
        }
    }
};

void Test()
{
    BitSet b1(10), b2(12);
    BitSet result = b1 & b2;
    result.print();  // 应该输出: 0 (全部位清零)
    result = b1 | b2;
    result.print();  // 应该输出: 0 (示例中b1和b2初始状态都是0)
}

void Test2()
{
    BitSet b1(64), b2(64);
    b1 |= BitSet(64);  // BitSet初始化是全0，所以这里实际上没有改变b1
    b1.print();
    b2 &= b1;  // 同样，b2也保持全0
    b2.print();
    BitSet b3 = ~b2;  // 取反操作
    b3.print();  // 期望打印全1（视size大小和具体实现细节而定）
}

int main()
{
    Test();
    Test2();
    return 0;
}
```

### 4. IntArray类的下标运算符重载
下标运算符通常用于提供类似数组的访问方式。
例如，重载一个表示容器的类的`[]`运算符：

```cpp
#include <iostream>
#include <cassert>

class IntArray {
private:
    int* array;
    int size;

public:
    IntArray(int size) : size(size) {
        array = new int[size];
    }

    ~IntArray() {
        delete[] array;
    }

    // 重载下标运算符
    int& operator[](int index) {
        assert(index >= 0 && index < size);
        return array[index];
    }
};

int main() {
    IntArray myArray(5);
    for (int i = 0; i < 5; ++i) {
        myArray[i] = i * i;
    }
    std::cout << "myArray[2] = " << myArray[2] << std::endl;
    return 0;
}
```

## 自定义内存分配机制的 类内部的 New 和 Delete 运算符重载

### Vector3 类的定义与运算符重载
下面的 `Vector3` 类包括三个浮点数，代表三维空间中的一个向量。
我们将在这个类中重载 `new` 和 `delete` 运算符，以及对应的数组版本 `new[]` 和 `delete[]`。

```cpp
#include <iostream>
#include <cstdlib>  // For std::malloc and std::free

class Vector3 {
public:
    float x, y, z;

    Vector3(float _x = 0.0f, float _y = 0.0f, float _z = 0.0f) : x(_x), y(_y), z(_z) {
        std::cout << "Vector3 constructed: " << x << ", " << y << ", " << z << std::endl;
    }

    ~Vector3() {
        std::cout << "Vector3 destroyed: " << x << ", " << y << ", " << z << std::endl;
    }

    // 重载类的 new 运算符
    static void* operator new(size_t size) {
        std::cout << "Custom new for Vector3 of size " << size << std::endl;
        void* p = std::malloc(size);
        if (!p) {
            throw std::bad_alloc();
        }
        return p;
    }

    // 重载类的 delete 运算符
    static void operator delete(void* p) {
        std::cout << "Custom delete for Vector3" << std::endl;
        std::free(p);
    }

    // 重载数组 new 运算符
    static void* operator new[](size_t size) {
        std::cout << "Custom new[] for Vector3 array of size " << size << std::endl;
        void* p = std::malloc(size);
        if (!p) {
            throw std::bad_alloc();
        }
        return p;
    }

    // 重载数组 delete 运算符
    static void operator delete[](void* p) {
        std::cout << "Custom delete[] for Vector3 array" << std::endl;
        std::free(p);
    }
};

int main() {
    std::cout << "Creating a single Vector3 object:\n";
    Vector3* v = new Vector3(1.0f, 2.0f, 3.0f);
    delete v;

    std::cout << "\nCreating an array of Vector3 objects:\n";
    Vector3* vArray = new Vector3[2]{{1.0f, 2.0f, 3.0f}, {4.0f, 5.0f, 6.0f}};
    delete[] vArray;

    return 0;
}
```

在这个示例中：
- 我们为 `Vector3` 类重载了单个对象和对象数组的内存分配(`new`和`new[]`)和释放(`delete`和`delete[]`)运算符。
- 每次创建和销毁 `Vector3` 对象时，我们都会输出有关其内存分配和释放的信息，以及对象的坐标值。
- 通过输出可以观察到类实例的创建和销毁过程，从而更好地理解内存管理。

这种方式让你可以非常灵活地控制对象的内存生命周期，非常适合需要精细控制内存使用的情况，如游戏开发、高性能计算和实时系统等领域。

## 类内部的 New 运算符重载：普通用法和Placement New用法的区别

在C++中，`placement new`是一种特殊形式的运算符new，它允许在已经分配的内存上构造一个对象。
这种方法主要用于需要在特定位置创建对象的情况，如内存池、缓冲区重用、或对内存对齐有特殊要求的情况。

与普通的`new`运算符相比，`placement new`并不分配内存，它只是调用对象的构造函数在指定的内存地址上构建对象。
这与在类内部重载`new`运算符的方式有本质的区别，后者通常是用来改变对象的内存分配方式或跟踪内存分配的。

### 普通 New 和 Placement New 的区别：
1. **普通用法**：分配足够的内存并调用构造函数来创建对象。
2. **Placement New 用法**：不分配内存，仅调用构造函数在给定的内存地址上构造对象(使用已有分配在堆上的内存空间原地构造类的实例) 。

### 修改之前的IntArray类：

为了更清楚地演示`placement new`的实际用途以及内存管理，我们可以使用之前提到的`IntArray`类，这将帮助我们更直观地理解内存操作和成员访问。
下面是`IntArray`类的实现，其中包括了成员数组的分配和释放，以及`placement new`的示例使用。

```cpp
#include <iostream>
#include <new>  // 包含对placement new的支持

class IntArray {
private:
    int* array;
    size_t size;  // 数组的元素数量

public:
    IntArray(size_t size) : size(size) {
        array = new int[size];  // 分配内存
        std::cout << "IntArray constructed with size " << size << std::endl;
    }

    ~IntArray() {
        delete[] array;  // 释放内存
        std::cout << "IntArray destroyed" << std::endl;
    }

    int& operator[](size_t index) {
        return array[index];
    }

    void print() const {
        for (size_t i = 0; i < size; ++i) {
            std::cout << array[i] << " ";
        }
        std::cout << std::endl;
    }

    // 类内部重载的 placement new
    static void* operator new(size_t size, void* place) {       
        std::cout << "Placement new used\n";
        return place;  // 只返回传入的内存地址
    }

    static void* operator new(size_t size) {
        std::cout << "Normal new used\n";
        return ::operator new(sz);
    }

    // 类内部重载的 delete
    static void operator delete(void* pointer) {
        std::cout << "Memory freed\n";
        ::operator delete(pointer);
    }
};

int main() {
    // 使用正常的new
    IntArray* array1 = new IntArray(5);

    // 分配足够的内存以放置IntArray对象，使用placement new
    char* buffer = new char[sizeof(IntArray)];
    IntArray* array2 = new (buffer) IntArray(5);  // 调用placement new

    // 访问和修改数组元素
    (*array1)[0] = 10;
    array1->print();

    (*array2)[1] = 20;
    array2->print();

    // 显示调用析构函数，placement new不负责释放内存
    array2->~IntArray();
    delete[] buffer;  // 手动释放分配的内存

    delete array1;  // 正常释放内存和对象
    return 0;
}
```

### 指针间接访问类成员运算符 (`->`)
指针间接访问类成员运算符 (`->`) 是一种用于通过指向类对象的指针来访问其成员的运算符。
如果你有一个指向类对象的指针，你可以使用`->`运算符直接访问其成员。
例如，如果`ptr`是一个指向`IntArray`对象的指针，则`ptr->print()`将调用该`IntArray`对象的`print`方法。
这是一种非常常见的用法，尤其在处理动态分配的对象时。
它也等效于`(*class_object_pointer).member_name;`

在上面的代码示例中，我们通过`array1->print()`和`array2->print()`展示了如何使用这个运算符来访问对象的方法。
同时，通过`(*array1)[0] = 10;`和`(*array2)[1] = 20;`的用法，我们展示了如何通过解引用指针然后使用下标运算符来访问和修改数组元素。

`希望这使内容对初学者更易于理解，祝你学得愉快！`