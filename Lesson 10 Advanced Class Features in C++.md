# Lesson 10: Advanced Class Features in C++

We will delve into the advanced class features in C++ that are necessary in object-oriented programming to enable us to design more robust and easy-to-maintain programs. 

Here are the main topics covered in this lesson:

## I. Friend Functions and Friend Classes

Understanding Friend Functions: Non-member functions that can access the private members of a class.
Friend Classes: Classes that can access the private members of another class, enabling more flexible designs.
Example Implementation: Demonstrates how to declare and use friend functions and friend classes within class definitions.

## II. Inheritance and Polymorphism

Inheritance: The mechanism by which one class can inherit attributes and methods from another class.
Polymorphism: Allows objects of different classes to be treated as if they were of the same class, enabling more flexible and reusable code.
Virtual Functions and Abstract Classes: Discusses the use of virtual functions and abstract classes to enforce common interfaces and implement polymorphism.
Example Implementation: Uses inheritance to create a simple class hierarchy and demonstrates polymorphic behavior.

### Sample Implementation: Use inheritance to create a simple class hierarchy and then demonstrate polymorphic behavior.
```cpp
#include <iostream>
#include <vector>

// Base class representing a generic shape
class Shape
{
protected:
    double area = 0.0;

    // Constructor with area parameter
    Shape(double a) : area(a)
    {
        std::cout << "Shape created with area: " << area << std::endl;
    }

    // Copy constructor
    Shape(const Shape& other) : area(other.area)
    {
        std::cout << "Shape copied with area: " << area << std::endl;
    }

public:
    // Virtual function to get the area of the shape
    virtual double getArea() const
    {
        return area;
    }

    // Virtual function to print the area of the shape
    virtual void printArea() const
    {
        std::cout << "Area of the shape: " << area << std::endl;
        return;
    }

    // Destructor
    virtual ~Shape()
    {
        area = 0;
        std::cout << "Shape destroyed" << std::endl;
    }
};

// Derived class representing a rectangle
class Rectangle : public Shape
{
private:
    double width, height;

    // Override base class function to calculate rectangle area
    double getArea() const override
    {
        return width * height;
    }

    // Friend function to set width and height of the rectangle
    friend void setArgument(Rectangle& object, double width, double height);

public:
    // Constructor with width and height parameters
    Rectangle(double width, double height) : width(width), height(height), Shape(0.0)
    {
        std::cout << "Rectangle created with width: " << width << " and height: " << height << std::endl;
        this->area = this->getArea();
        printArea();
    }

    // Copy constructor
    Rectangle(const Rectangle& other) : Shape(other.area)
    {
        std::cout << "Rectangle copied with area: " << area << std::endl;
        printArea();
    }

    // Destructor
    ~Rectangle()
    {
        width = height = 0;
        std::cout << "Rectangle destroyed" << std::endl;
    }

    // Override base class function to print rectangle area
    void printArea() const override
    {
        std::cout << "Area of the rectangle: " << this->area << std::endl;
        return;
    }
};

// Derived class representing a circle
class Circle : public Shape
{
private:
    const double PI = 3.14159265358979323846;
    double radius;

    // Override base class function to calculate circle area
    double getArea() const override
    {
        return PI * radius * radius;
    }

    // Friend function to set radius of the circle
    friend void setArgument(Circle& object, double radius);

public:
    // Constructor with radius parameter
    Circle(double radius) : radius(radius), Shape(0.0)
    {
        std::cout << "Circle created with radius: " << radius << std::endl;
        this->area = this->getArea();
    }

    // Copy constructor
    Circle(const Circle& other) : Shape(other.area)
    {
        std::cout << "Circle copied with area: " << area << std::endl;
        printArea();
    }

    // Destructor
    ~Circle()
    {
        radius = 0;
        std::cout << "Circle destroyed" << std::endl;
    }

    // Override base class function to print circle area
    void printArea() const override
    {
        std::cout << "Area of the circle: " << this->area << std::endl;
    }
};

// Function to set width and height of the rectangle
void setArgument(Rectangle& object, double width, double height)
{
    object.width = width;
    object.height = height;
    std::cout << "Rectangle modified with width: " << object.width << " and height: " << object.height << std::endl;
}

// Function to set radius of the circle
void setArgument(Circle& object, double radius)
{
    object.radius = radius;
    std::cout << "Circle modified with radius: " << object.radius << std::endl;
}

int main()
{
    // Vector to store pointers to shapes
    std::vector<Shape*> shapes = {new Rectangle(5, 10), new Circle(7)};

    // Print areas of shapes
    for (Shape* shape : shapes)
    {
        shape->printArea();
    }

    std::cout << "---------------------------" << std::endl;

    // Use dynamic_cast for dynamic type conversion
    if (Rectangle* rect = dynamic_cast<Rectangle*>(shapes[0]))
    {
        setArgument(*rect, 10, 20);
    }

    if (Circle* circle = dynamic_cast<Circle*>(shapes[1]))
    {
        setArgument(*circle, 10);
    }

    // Print areas of modified shapes
    for (Shape* shape : shapes)
    {
        shape->printArea();
    }

    // Delete allocated shapes and set pointers to nullptr
    for (Shape* shape : shapes)
    {
        delete shape;
        shape = nullptr;
    }

    return 0;
}
```

### Code Explanation

This C++ code defines a base class `Shape` and two derived classes, `Rectangle` and `Circle`, representing different shapes. The code also includes some functions and the main function. Here is a detailed explanation:

### `Shape` Class

1. **Member Variables**:
   - `protected double area`: Represents the area of the shape, accessible by derived classes.

2. **Constructor and Destructor**:
   - `Shape(double a)`: Constructor that initializes the area and outputs a creation message.
   - `Shape(const Shape& other)`: Copy constructor used to output a copy message.
   - `virtual ~Shape()`: Virtual destructor to release resources and output a destruction message.

3. **Virtual Functions**:
   - `virtual double getArea() const`: Virtual function returning the area of the shape.
   - `virtual void printArea() const`: Virtual function printing the area of the shape.

### `Rectangle` Class

1. **Private Member Variables**:
   - `double width, height`: Width and height of the rectangle.

2. **Private Functions**:
   - `double getArea() const override`: Override of the base class virtual function to calculate the area of the rectangle.

3. **Friend Function**:
   - `friend void setArgument(Rectangle& object, double width, double height)`: Friend function used to set the width and height of the rectangle.

4. **Constructor and Destructor**:
   - `Rectangle(double width, double height)`: Constructor initializing the width and height of the rectangle and outputting a creation message.
   - `Rectangle(const Rectangle& other)`: Copy constructor used to output a copy message.
   - `~Rectangle()`: Destructor to release resources and output a destruction message.

5. **Virtual Function Override**:
   - `void printArea() const`: Override of the base class virtual function to print the area of the rectangle.

### `Circle` Class

1. **Private Member Variables**:
   - `double radius`: Radius of the circle.

2. **Private Functions**:
   - `double getArea() const override`: Override of the base class virtual function to calculate the area of the circle.

3. **Friend Function**:
   - `friend void setArgument(Circle& object, double radius)`: Friend function used to set the radius of the circle.

4. **Constructor and Destructor**:
   - `Circle(double radius)`: Constructor initializing the radius of the circle and outputting a creation message.
   - `Circle(const Circle& other)`: Copy constructor used to output a copy message.
   - `~Circle()`: Destructor to release resources and output a destruction message.

5. **Virtual Function Override**:
   - `void printArea() const`: Override of the base class virtual function to print the area of the circle.

### Global Functions

1. **`setArgument` Function**:
   - Used to set the width and height of a rectangle or the radius of a circle.
   - Declared as a `friend` function to have access to private members of the classes.

### `main` Function

1. **Create Shapes**:
   - Create a vector of `Shape` pointers containing a rectangle and a circle.

2. **Print Areas of Shapes**:
   - Iterate through the vector using iterators, calling `printArea` function to print the area of each shape.

3. **Dynamic Type Casting and Modification**:
   - Use `dynamic_cast` for dynamic type conversion of pointers to the base class, allowing modification of derived class members.
   - Modify the width and height of the rectangle and the radius of the circle.

4. **Print Areas of Modified Shapes**:
   - Iterate through the vector again, calling `printArea` function to print the area of each modified shape.

5. **Release Memory**:
   - Iterate through the vector, delete the memory of each shape, and set pointers to `nullptr`.

## III. Exercises

Implement a class hierarchy for a library system, where books, DVDs, and magazines inherit from a base class.
Develop a polymorphic interface for a set of shapes with a common area calculation method.

---

# Lesson 10: 高级类功能在C++中的应用

我们将会深入研究C++中的高级类功能，这些功能是面向对象编程中必要的，使我们能够设计出更加健壮和易于维护的程序。

以下是本课主要的内容：

## I. 友好函数和好友类

理解友元函数：非成员函数，可以访问类的私有成员。

了解 Friend 函数： 可以访问类中私有成员的非成员函数。
Friend 类： 可以访问另一个类的私有成员的类，可以实现更灵活的设计。
示例实现： 演示如何在类定义中声明和使用友函数和友类。

## II. 继承和多态性

继承： 一个类从另一个类继承属性和方法的机制。
多态性： 允许将不同类中的对象视为同类，从而使代码更加灵活和可重用。
虚拟函数和抽象类： 讨论如何使用虚拟函数和抽象类来执行通用接口和实现多态性。

### 示例实现: 使用继承来创建一个简单的类层次结构，然后演示多态行为。
```cpp
#include <iostream>
#include <vector>

// 基类，表示通用形状
class Shape
{
protected:
    double area = 0.0;

    // 构造函数，带有面积参数
    Shape(double a) : area(a)
    {
        std::cout << "创建形状，面积为：" << area << std::endl;
    }

    // 复制构造函数
    Shape(const Shape& other) : area(other.area)
    {
        std::cout << "复制形状，面积为：" << area << std::endl;
    }

public:
    // 虚函数，获取形状的面积
    virtual double getArea() const
    {
        return area;
    }

    // 虚函数，打印形状的面积
    virtual void printArea() const
    {
        std::cout << "形状的面积为：" << area << std::endl;
        return;
    }

    // 析构函数
    virtual ~Shape()
    {
        area = 0;
        std::cout << "形状销毁" << std::endl;
    }
};

// 派生类，表示矩形
class Rectangle : public Shape
{
private:
    double width, height;

    // 重写基类函数，计算矩形的面积
    double getArea() const override
    {
        return width * height;
    }

    // 友元函数，设置矩形的宽度和高度
    friend void setArgument(Rectangle& object, double width, double height);

public:
    // 构造函数，带有宽度和高度参数
    Rectangle(double width, double height) : width(width), height(height), Shape(0.0)
    {
        std::cout << "创建矩形，宽度为：" << width << "，高度为：" << height << std::endl;
        this->area = this->getArea();
        printArea();
    }

    // 复制构造函数
    Rectangle(const Rectangle& other) : Shape(other.area)
    {
        std::cout << "复制矩形，面积为：" << area << std::endl;
        printArea();
    }

    // 析构函数
    ~Rectangle()
    {
        width = height = 0;
        std::cout << "矩形销毁" << std::endl;
    }

    // 重写基类函数，打印矩形的面积
    void printArea() const override
    {
        std::cout << "矩形的面积为：" << this->area << std::endl;
        return;
    }
};

// 派生类，表示圆形
class Circle : public Shape
{
private:
    const double PI = 3.14159265358979323846;
    double radius;

    // 重写基类函数，计算圆形的面积
    double getArea() const override
    {
        return PI * radius * radius;
    }

    // 友元函数，设置圆形的半径
    friend void setArgument(Circle& object, double radius);

public:
    // 构造函数，带有半径参数
    Circle(double radius) : radius(radius), Shape(0.0)
    {
        std::cout << "创建圆形，半径为：" << radius << std::endl;
        this->area = this->getArea();
    }

    // 复制构造函数
    Circle(const Circle& other) : Shape(other.area)
    {
        std::cout << "复制圆形，面积为：" << area << std::endl;
        printArea();
    }

    // 析构函数
    ~Circle()
    {
        radius = 0;
        std::cout << "圆形销毁" << std::endl;
    }

    // 重写基类函数，打印圆形的面积
    void printArea() const override
    {
        std::cout << "圆形的面积为：" << this->area << std::endl;
    }
};

// 函数，设置矩形的宽度和高度
void setArgument(Rectangle& object, double width, double height)
{
    object.width = width;
    object.height = height;
    std::cout << "修改矩形，宽度为：" << object.width << "，高度为：" << object.height << std::endl;
}

// 函数，设置圆形的半径
void setArgument(Circle& object, double radius)
{
    object.radius = radius;
    std::cout << "修改圆形，半径为：" << object.radius << std::endl;
}

int main()
{
    // 存储指向形状的指针的向量
    std::vector<Shape*> shapes = {new Rectangle(5, 10), new Circle(7)};

    // 打印形状的面积
    for (Shape* shape : shapes)
    {
        shape->printArea();
    }

    std::cout << "---------------------------" << std::endl;

    // 使用 dynamic_cast 进行动态类型转换
    if (Rectangle* rect = dynamic_cast<Rectangle*>(shapes[0]))
    {
        setArgument(*rect, 10, 20);
    }

    if (Circle* circle = dynamic_cast<Circle*>(shapes[1]))
    {
        setArgument(*circle, 10);
    }

    // 打印修改后形状的面积
    for (Shape* shape : shapes)
    {
        shape->printArea();
    }

    // 删除分配的形状并将指针设置为 nullptr
    for (Shape* shape : shapes)
    {
        delete shape;
        shape = nullptr;
    }

    return 0;
}
```

### 代码说明

这段C++代码定义了一个基类 `Shape` 和两个派生类 `Rectangle` 和 `Circle`，用于表示不同形状的图形。代码中还包含了一些函数和主函数，以下是详细说明：

### `Shape` 类

1. **成员变量**：
   - `protected double area`：表示图形的面积，被派生类访问。

2. **构造函数和析构函数**：
   - `Shape(double a)`：构造函数，初始化面积，并输出创建的消息。
   - `Shape(const Shape& other)`：复制构造函数，用于在对象复制时输出消息。
   - `virtual ~Shape()`：虚析构函数，用于释放资源，输出销毁消息。

3. **虚函数**：
   - `virtual double getArea() const`：虚函数，返回图形的面积。
   - `virtual void printArea() const`：虚函数，打印图形的面积。

### `Rectangle` 类

1. **私有成员变量**：
   - `double width, height`：矩形的宽度和高度。

2. **私有函数**：
   - `double getArea() const override`：重写基类的虚函数，计算矩形的面积。

3. **友元函数**：
   - `friend void setArgument(Rectangle& object, double width, double height)`：友元函数，用于设置矩形的宽度和高度。

4. **构造函数和析构函数**：
   - `Rectangle(double width, double height)`：构造函数，初始化矩形的宽度和高度，并输出创建消息。
   - `Rectangle(const Rectangle& other)`：复制构造函数，用于在对象复制时输出消息。
   - `~Rectangle()`：析构函数，释放资源，输出销毁消息。

5. **虚函数重写**：
   - `void printArea() const`：重写基类虚函数，打印矩形的面积。

### `Circle` 类

1. **私有成员变量**：
   - `double radius`：圆形的半径。

2. **私有函数**：
   - `double getArea() const override`：重写基类的虚函数，计算圆形的面积。

3. **友元函数**：
   - `friend void setArgument(Circle& object, double radius)`：友元函数，用于设置圆形的半径。

4. **构造函数和析构函数**：
   - `Circle(double radius)`：构造函数，初始化圆形的半径，并输出创建消息。
   - `Circle(const Circle& other)`：复制构造函数，用于在对象复制时输出消息。
   - `~Circle()`：析构函数，释放资源，输出销毁消息。

5. **虚函数重写**：
   - `void printArea() const`：重写基类虚函数，打印圆形的面积。

### 全局函数

1. **`setArgument` 函数**：
   - 用于设置矩形的宽度和高度，或者圆形的半径。
   - `friend` 声明使其成为类的友元函数，可以访问类的私有成员。

### `main` 函数

1. **创建图形**：
   - 创建一个存储 `Shape` 指针的向量，包含一个矩形和一个圆形。

2. **打印图形的面积**：
   - 使用迭代器遍历向量，调用 `printArea` 函数打印每个图形的面积。

3. **动态类型转换和修改**：
   - 使用 `dynamic_cast` 对指向基类的指针进行动态类型转换，以便调用派生类的函数。
   - 修改矩形的宽度和高度，修改圆形的半径。

4. **再次打印修改后的图形的面积**：
   - 使用迭代器遍历向量，调用 `printArea` 函数打印修改后每个图形的面积。

5. **释放内存**：
   - 使用迭代器遍历向量，删除每个图形的内存，并将指针设置为 `nullptr`。

## III.可能的测试和练习
- 例如，使用继承来创建一个更复杂且具有多个类的类层次结构，并演示多态性。
- 例如，编写一个程序，使用继承和多态性来实现一个简单的图形编辑器，允许用户创建、编辑和保存各种图形。
- 例如，完成一个需要对象以及多态性的数学运算的游戏。

