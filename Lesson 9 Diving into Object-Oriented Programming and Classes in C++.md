## Lesson 9: Diving into Object-Oriented Programming and Classes in C++

### Introduction

Imagine a world where everything is organized into neat little boxes, each box representing an "object" with its own set of properties and actions. This is the essence of Object-Oriented Programming (OOP). In the realm of C++, these boxes are crafted using "classes". Dive in to uncover the magic of OOP and the art of creating classes in C++.

### Understanding Object-Oriented Programming

1. **What is OOP?**:
	- Think of OOP as a city. Each building (or "object") has its own design (or "class"). The city is organized, scalable, and each building serves a purpose.
	- Objects are like individual buildings, constructed from blueprints known as classes.

2. **Why OOP?**:
	- **Modularity**: Like organizing books on a shelf, OOP organizes code into classes, making it tidy and easy to navigate.
	- **Reusability**: Build once, use everywhere. Classes can be reused, just like LEGO blocks.
	- **Scalability**: Want to add a new building to your city? With OOP, expanding your code is a breeze.

## Emulating OOP in C: A Creative Approach

C is like an artist who primarily sketches, but with some creativity, it can paint a picture resembling OOP.

1. **Structures in C**:
	- Structures in C are like basic LEGO blocks. They can group different pieces (or data types) together but lack the flair of advanced sets.
	
	```c
	struct SimpleBlock {
		 // attributes or pieces
	};
	```

2. **Global Functions: The Puppeteers**:
	- While structures in C are inanimate, global functions can bring them to life, much like puppeteers controlling puppets.
	
	```c
	void animateBlock(struct SimpleBlock *block, other_actions) {
		 // breathe life into the block
	}
	```

3. **Crafty Encapsulation in C**:
	- C structures don't have curtains (or access modifiers) like `private` or `public`. But, you can keep some things hidden behind the scenes using static variables in source files.
	- Public announcements (or interfaces) are made through header files, while secrets are whispered in source files.

### Structures vs. Classes in C++: The Evolution

1. **Default Access**:
	- C++ structures are like open parks; everything (`public`) is accessible.
	- C++ classes, on the other hand, are like private properties. You need special access (`private`) to enter.

2. **Functionality**:
	- C++ structures have evolved; they can dance (have member functions) and even pass on their legacy (inheritance).
	- C structures are traditional; they don't dance or inherit.

3. **Inheritance**:
	- In the world of C++, both structures and classes can have descendants, passing on their legacy.
	- C structures, being traditional, don't believe in inheritance.

4. **Purpose**:
	- C++ structures are like public figures, often used for public roles with open access.
	- C++ classes are like secret agents, encapsulating details and performing complex missions.

### Unraveling the Mystery of Classes

1. **Crafting a Class**:
	 - Crafting a class is like designing a blueprint for a building.

	 ```cpp
	 class DreamHouse {
		  // design and features
	 };
	```

2. **Breathing Life into Classes**:
	 - Once you have a blueprint, you can construct buildings (or objects).

	 ```cpp
	 DreamHouse myHouse;
	 ```

3. **Guarding Secrets with Access Modifiers**:
	 - Decide who gets to see or change what in your DreamHouse using `public`, `private`, and `protected` doors.
	 - Remember, a good house keeps its valuables (`private`) safe.

4. **Attributes and Actions**:
	 - Every house has features (or attributes) and functionalities (or methods).
	 - Every object has access to public members and public methods, which are also called public members, and they are accessed using a dot like structure `object.member`.
	 - If the address of this object is bound to a pointer, then you need to use an arrow like operator to implement indirect access to `object_pointer->member`, the public member inside this object using the pointer.

---

### The Birth and Demise of Objects: Constructors and Destructors

1. **The Birth: Constructors**:
	- Constructors are like the grand opening ceremonies of buildings. They set the initial state.
	
	```cpp
	DreamHouse() {
		 // grand opening
	};
	```

2. **The Farewell: Destructors**:
	- Destructors are the closing ceremonies, ensuring everything is wrapped up neatly.
	
	```cpp
	~DreamHouse() {
		 // say goodbye
	};
	```

### Special Member Functions: The Heartbeat of C++ Objects

Imagine a world where objects have lives. They're born, they might clone themselves, sometimes they move to new places, and eventually, they pass away. In C++, these life events are managed by special member functions.

#### Copy Constructor and Copy Assignment Operator: The Cloning Machines

1. **Copy Constructor**:
	- Think of it as a cloning machine. When you want to create an exact replica of an object, this constructor is called into action.
	- Beware of the difference between a shallow copy (just copying the outer shell) and a deep copy (cloning everything, inside and out).
	
	```cpp
	ClassName(const ClassName &source); // The cloning blueprint
	```

2. **Copy Assignment Operator**:
	- This is like a machine that overwrites one object with the data of another. It's not creating a new object, but changing an existing one.
	
	```cpp
	ClassName& operator=(const ClassName &source); // The overwrite blueprint
	```

#### Move Constructor and Move Assignment Operator: The Movers

1. **Move Constructor**:
	- Imagine an object that can transfer its heart (resources) to another. This constructor does that. It's efficient because it doesn't replicate; it relocates.
	
	```cpp
	ClassName(ClassName &&source); // The heart transfer blueprint
	```

2. **Move Assignment Operator**:
	- Similar to the move constructor, but instead of creating a new object, it transfers the heart of one object to overwrite another.
	
	```cpp
	ClassName& operator=(ClassName &&source); // The heart overwrite blueprint
	```

#### The Essence of Special Member Functions

These functions ensure that our objects are managed efficiently. They help in:
- Properly cloning objects.
- Efficiently moving resources without unnecessary copies.
- Avoiding pitfalls like the "Rule of Three" which states that if you have either a custom destructor, copy constructor, or copy assignment operator, you likely need all three.

---

### Deep Dive into Special Member Functions

#### Copy Constructor and Copy Assignment Operator: The Details

1. **When are they called?**:
	- **Copy Constructor**: Imagine you have a toy. Now, you want another toy just like it. This constructor is called to make that replica.
	- **Copy Assignment Operator**: You have two toys, but you decide one should look and feel just like the other. This operator makes that happen.

2. **Pitfalls**:
	- **Shallow vs. Deep Copy**: The danger of just copying the outer shell and not what's inside can lead to issues, especially with dynamic memory.
	- **Rule of Three**: A golden rule in C++ that ensures if you customize one of the destructor, copy constructor, or copy assignment operator, you should likely define the other two to prevent unexpected behaviors.

#### Move Constructor and Move Assignment Operator: The Details

1. **When are they called?**:
	- **Move Constructor**: When an object decides to transfer its essence (resources) to a new object, this constructor is summoned.
	- **Move Assignment Operator**: When one object decides to adopt the essence of another, overwriting its own, this operator is invoked.

2. **Pitfalls**:
	- **Resource Stealing**: The beauty of move semantics is in resource relocation. But care is needed to ensure the source object remains in a stable state post-move.
	- **Rule of Five**: An evolution of the Rule of Three for modern C++. If you define or delete any of the special member functions, you should consider the implications for the other four.

### Pro Tips for Special Member Functions

1. **Implicitly Defined Functions**: If you don't define them, the compiler does it for you. But it might not always be in the way you want.
2. **Avoiding Pitfalls**: Modern tools like `std::unique_ptr` or `std::shared_ptr` can be lifesavers, helping you manage resources without the headaches of raw pointers.
3. **Explicit Defaults**: If you're happy with the default behavior, you can explicitly tell the compiler using `= default`.

---

### Pitfalls and Precautions with Special Member Functions

#### 1. Rule of Three: The Golden Rule

If a class defines one of the following, it likely needs to define all three:
- Destructor
- Copy Constructor
- Copy Assignment Operator

**Pitfall**: Not following this rule for classes managing resources can lead to issues like shallow copies or memory leaks.

**Precaution**: Always define all three for such classes.

#### 2. Rule of Five: The Modern Rule

With C++11 introducing move semantics, if a class defines or deletes any of the special member functions, it should consider the other four.

**Pitfall**: Not handling move operations can lead to inefficient code.

**Precaution**: Always define or delete move operations if you're defining custom copy operations or a destructor.

#### 3. Implicit Conversions: The Sneaky Ones

Single argument constructors can sometimes lead to unintended conversions.

**Pitfall**: These can introduce bugs if not careful.

**Precaution**: Use the `explicit` keyword to prevent such sneaky conversions.

---

### Rule of Three

The Rule of Three suggests that if a class defines one of the following, it likely needs to define all three:

1. Destructor
2. Copy Constructor
3. Copy Assignment Operator

#### Impact:

For classes managing dynamic memory or resources, the compiler-provided versions of these functions might lead to unintended behavior, such as shallow copies, double deletions, or memory leaks.

#### Example:

**Incorrect Implementation**:
```cpp
class String {
	 char* data;
public:
	 String(const char* str) {
		  data = new char[strlen(str) + 1];
		  strcpy(data, str);
	 }
	 // Missing custom copy constructor and copy assignment operator
	 ~String() {
		  delete[] data;
	 }
};
```

**Correct Implementation**:
```cpp
class String {
	 char* data;
public:
	 String(const char* str) {
		  data = new char[strlen(str) + 1];
		  strcpy(data, str);
	 }
	 String(const String& other) { // Copy constructor
		  data = new char[strlen(other.data) + 1];
		  strcpy(data, other.data);
	 }
	 String& operator=(const String& other) { // Copy assignment
		  if (this != &other) {
				delete[] data;
				data = new char[strlen(other.data) + 1];
				strcpy(data, other.data);
		  }
		  return *this;
	 }
	 ~String() {
		  delete[] data;
	 }
};
```

### Rule of Five (C++11 and later)

With the introduction of move semantics in C++11, the Rule of Five suggests that if a class defines or deletes any of the special member functions, it should consider the other four as well:

1. Destructor
2. Copy Constructor
3. Copy Assignment Operator
4. Move Constructor
5. Move Assignment Operator

#### Impact:

Not properly handling move operations can result in inefficient code or resource leaks.

#### Example:

**Incorrect Implementation** (missing move operations):
```cpp
class String {
	 // ... as above
};
```

**Correct Implementation**:
```cpp
class String {
	 // ... as above
	 String(String&& other) noexcept : data(other.data) { // Move constructor
		  other.data = nullptr;
	 }
	 String& operator=(String&& other) noexcept { // Move assignment
		  if (this != &other) {
				delete[] data;
				data = other.data;
				other.data = nullptr;
		  }
		  return *this;
	 }
};
```

### Additional Points:

1. **Implicitly Defined Special Functions**: If not defined by the programmer, the compiler will implicitly define the copy constructor, copy assignment operator, and destructor. 
With C++11 and onward, the move constructor and move assignment operator are also implicitly defined under certain conditions.

2. **Avoiding Pitfalls**: Using smart pointers like `std::unique_ptr` or `std::shared_ptr` can help manage resources and avoid some of the pitfalls associated with raw pointers and dynamic memory.

3. **Explicit Defaults**: In cases where the default behavior is desired, you can explicitly define the special functions using `= default`

### Rule of Three

The Rule of Three suggests that if a class defines one of the following, it likely needs to define all three:

- Destructor
- Copy Constructor
- Copy Assignment Operator

#### Incorrect `String` Class (Violating Rule of Three):

```cpp
class String {
	 char* data;
public:
	 String(const char* str) {
		  data = new char[strlen(str) + 1];
		  strcpy(data, str);
	 }
	 // Missing custom copy constructor and copy assignment operator
	 ~String() {
		  delete[] data;
	 }
};
```

In the above code, the default copy constructor and copy assignment operator will perform a shallow copy. This can lead to double-deletion errors or memory leaks.

#### Correct `String` Class (Following Rule of Three):

```cpp
class String {
	 char* data;
public:
	 String(const char* str) {
		  data = new char[strlen(str) + 1];
		  strcpy(data, str);
	 }
	 String(const String& other) { // Copy constructor
		  data = new char[strlen(other.data) + 1];
		  strcpy(data, other.data);
	 }
	 String& operator=(const String& other) { // Copy assignment
		  if (this != &other) {
				delete[] data;
				data = new char[strlen(other.data) + 1];
				strcpy(data, other.data);
		  }
		  return *this;
	 }
	 ~String() {
		  delete[] data;
	 }
};
```

### Rule of Five (C++11 and later)

With the introduction of move semantics in C++11, the Rule of Five suggests that if a class defines or deletes any of the special member functions, it should consider the other four as well:

- Destructor
- Copy Constructor
- Copy Assignment Operator
- Move Constructor
- Move Assignment Operator

#### Incorrect `UniquePointerArray` Class (Violating Rule of Five):

```cpp
template<typename T>
class UniquePointerArray {
	 T* data;
	 size_t size;
public:
	 UniquePointerArray(size_t s) : size(s), data(new T[s]) {}
	 // Missing move constructor and move assignment operator
	 ~UniquePointerArray() {
		  delete[] data;
	 }
};
```

In the above code, the move constructor and move assignment operator are missing. This can lead to inefficient code or resource leaks.

#### Correct `UniquePointerArray` Class (Following Rule of Five):

```cpp
template<typename T>
class UniquePointerArray {
	 T* data;
	 size_t size;
public:
	 UniquePointerArray(size_t s) : size(s), data(new T[s]) {}
	 UniquePointerArray(UniquePointerArray&& other) noexcept : data(other.data), size(other.size) {
		  other.data = nullptr;
		  other.size = 0;
	 }
	 UniquePointerArray& operator=(UniquePointerArray&& other) noexcept {
		  if (this != &other) {
				delete[] data;
				data = other.data;
				size = other.size;
				other.data = nullptr;
				other.size = 0;
		  }
		  return *this;
	 }
	 ~UniquePointerArray() {
		  delete[] data;
	 }
};
```

**A Brief Note on Templates:**

You might have noticed the use of `template<typename T>` in our `UniquePointerArray` example. If this is new to you, don't be alarmed! Let's briefly touch upon it.

Templates in C++ allow us to write generic code. The `T` you see is a type placeholder. Think of it as a stand-in for any data type—be it `int`, `double`, `String`, or any other type. When we use the `UniquePointerArray` class, we can specify what type `T` should be, making our class versatile and reusable for different data types.

For instance:
- `UniquePointerArray<int>` would create an array of integers.
- `UniquePointerArray<String>` would create an array of our custom `String` objects.

However, the intricacies of templates are beyond the scope of this lesson. For now, just remember:
- `T` is a placeholder for a type.
- Templates allow us to write code that works with many data types, not just one.

We'll delve deeper into templates in a future lesson. For now, focus on understanding the Rule of Three and Rule of Five, and know that templates are a powerful feature of C++ that we'll explore together soon!

---

## Diving into Constructors: Implicit vs. Explicit

Constructors in C++ play a pivotal role in object creation and initialization. 
They can be broadly categorized into implicit (converting) constructors and explicit constructors. 
Understanding the distinction between these two is crucial for writing clear and bug-free code.

### Implicit (Converting) Constructors

1. **Definition**: Implicit constructors, also known as converting constructors, allow the C++ compiler to automatically convert one type into another without explicit instruction.

2. **Example**:
	```cpp
	class Box {
	public:
		 Box(int size) {
			  // The box is created with the given size
		 }
	};

	Box giftBox = 5;  // The integer 5 is implicitly converted into a Box object.
	```

3. **Detailed Example**:
	```cpp
	class Fraction {
	private:
		 int numerator;
		 int denominator;
	public:
		 // Implicit constructor
		 Fraction(int num) : numerator(num), denominator(1) {}

		 void print() {
			  std::cout << numerator << "/" << denominator << std::endl;
		 }
	};

	void displayFraction(Fraction f) {
		 f.print();
	}

	int main() {
        // 10 is implicitly converted to Fraction(10/1)
        Fraction fraction_object = 10;
        displayFraction(5);  // 5 is implicitly converted to Fraction(5/1)
        return 0;
	}
	```

4. **Potential Issues**: While convenient, implicit constructors can sometimes lead to unexpected type conversions, which might introduce bugs or unintended behavior.

### Explicit Constructors

1. **Definition**: Explicit constructors prevent automatic type conversions. 
By marking a constructor with the `explicit` keyword, you signal the compiler not to use this constructor for implicit conversions.

2. **Example**:
	```cpp
	class Box {
	public:
		 explicit Box(int size) {
			  // The box is created with the given size
		 }
	};

	Box giftBox = 5;  // Compile error due to the explicit keyword
	Box anotherBox(5);  // Correct way to create a Box with size 5.
	```

3. **Detailed Example**:
	```cpp
	class Fraction {
	private:
		 int numerator;
		 int denominator;
	public:
		 // Explicit constructor
		 explicit Fraction(int num) : numerator(num), denominator(1) {}

		 void print() {
			  std::cout << numerator << "/" << denominator << std::endl;
		 }
	};

	void displayFraction(Fraction f) {
		 f.print();
	}

	int main() {
		 // displayFraction(5);  // Compile error due to the explicit keyword
		 Fraction frac(5);
		 displayFraction(frac);  // Correct way to pass a Fraction object
		 return 0;
	}
	```


4. **Advantages**: Using the `explicit` keyword can prevent unintended conversions, especially when the conversion might be non-intuitive or error-prone.

### Key Takeaways

- **Intentionality**: Implicit constructors offer convenience but come with risks. Explicit constructors require clarity and intention, reducing potential pitfalls.
- **Best Practices**: Always review class design and potential use cases. If a constructor can lead to ambiguous results, consider marking it `explicit`.

---

## Special Directives in C++: `= default` and `= delete`

C++11 introduced two powerful specifiers: `= default` and `= delete`. These specifiers provide clear directives to the compiler about how to handle special member functions.

### Using `= default`

1. **Definition**: The `= default` specifier instructs the compiler to generate the default implementation for a special member function.

2. **Example**:
	```cpp
	class Robot {
	public:
		 Robot() = default;  // Use the compiler's default constructor
	};
	```

3. **Detailed Other Example**:
	```cpp
	class Point {
	private:
		 int x, y;
	public:
		 Point() = default;  // Default constructor

		 Point(int xVal, int yVal) : x(xVal), y(yVal) {}

		 void print() {
			  std::cout << "(" << x << ", " << y << ")" << std::endl;
		 }
	};

	int main() {
		 Point p1;  // Calls the default constructor
		 Point p2(5, 10);  // Calls the parameterized constructor

		 p1.print();  // Outputs: (0, 0) - because of the default initialization
		 p2.print();  // Outputs: (5, 10)
		 return 0;
	}
	```

4. **Considerations**: While `= default` is handy, it's essential to understand the automatically provided implementations to ensure they align with your intentions.

### Using `= delete`

1. **Definition**: The `= delete` specifier explicitly disables certain member functions, ensuring they cannot be used.

2. **Example**:
	```cpp
	class UniqueRobot {
	public:
		 UniqueRobot(const UniqueRobot&) = delete;  // Robots can't be duplicated!
	};
	```

3. **Detailed Other Example**:
	```cpp
	class NonCopyable {
	private:
		 int data;
	public:
		 NonCopyable(int val) : data(val) {}

		 NonCopyable(const NonCopyable&) = delete;  // Deleting the copy constructor

		 void print() {
			  std::cout << "Data: " << data << std::endl;
		 }
	};

	int main() {
		 NonCopyable obj1(10);
		 // NonCopyable obj2 = obj1;  // Compile error due to deleted copy constructor

		 obj1.print();  // Outputs: Data: 10
		 return 0;
	}
	```

4. **Advantages**: `= delete` is a robust way to prevent specific operations, ensuring that certain functions or operations are not misused.

### Key Takeaways

- **Clarity**: Both `= default` and `= delete` provide clear directives to the compiler, ensuring that your class behaves as intended.
- **Best Practices**: Always document the use of `= delete` to inform users of the class about the intended restrictions.

---

#### Extended Example with Dynamic Memory:

```cpp
class String {
private:
	 char* data;

public:
	 // Implicit constructor
	 String(const char* str) {
		  data = new char[strlen(str) + 1];
		  strcpy(data, str);
	 }

	 // Copy constructor
	 String(const String& other) {
		  data = new char[strlen(other.data) + 1];
		  strcpy(data, other.data);
	 }

	 // Destructor
	 ~String() {
		  delete[] data;
	 }

	 void print() const {
		  std::cout << data << std::endl;
	 }
};

void displayString(String s) {
	 s.print();
}

int main() {
	 displayString("Hello, World!");  // Implicitly converts the C-string to a String object
	 String str1("Hello");
	 String str2 = str1;  // Calls the copy constructor
	 str2.print();
	 return 0;
}
```

### Explicit Constructors

#### Extended Example with Dynamic Memory:

```cpp
class DynamicArray {
private:
	 int* data;
	 size_t size;

public:
	 // Explicit constructor
	 explicit DynamicArray(size_t s) : size(s) {
		  data = new int[size];
		  for (size_t i = 0; i < size; ++i) {
				data[i] = 0;
		  }
	 }

	 // Copy constructor
	 DynamicArray(const DynamicArray& other) : size(other.size) {
		  data = new int[size];
		  for (size_t i = 0; i < size; ++i) {
				data[i] = other.data[i];
		  }
	 }

	 // Destructor
	 ~DynamicArray() {
		  delete[] data;
	 }

	 void set(size_t index, int value) {
		  if (index < size) {
				data[index] = value;
		  }
	 }

	 int get(size_t index) const {
		  return (index < size) ? data[index] : 0;
	 }

	 void print() const {
		  for (size_t i = 0; i < size; ++i) {
				std::cout << data[i] << " ";
		  }
		  std::cout << std::endl;
	 }
};

int main() {
	 DynamicArray arr1(5);
	 arr1.set(2, 50);
	 arr1.print();

	 DynamicArray arr2 = arr1;  // Calls the copy constructor
	 arr2.print();

	 // DynamicArray arr3 = 10;  // Compile error due to the explicit keyword
	 DynamicArray arr3(10);  // Correct way to create a DynamicArray of size 10
	 arr3.print();

	 return 0;
}
```

---

## Special Directives in C++: `= default` and `= delete`

### Using `= default`

#### Extended Example with Dynamic Memory:

```cpp
class Buffer {
private:
	 char* data;
	 size_t size;

public:
	 Buffer() = default;

	 Buffer(size_t s) : size(s) {
		  data = new char[size];
		  memset(data, 0, size);
	 }

	 // Copy constructor
	 Buffer(const Buffer& other) : size(other.size) {
		  data = new char[size];
		  memcpy(data, other.data, size);
	 }

	 // Destructor
	 ~Buffer() {
		  delete[] data;
	 }

	 void fill(char ch) {
		  memset(data, ch, size);
	 }

	 void print() const {
		  for (size_t i = 0; i < size; ++i) {
				std::cout << data[i];
		  }
		  std::cout << std::endl;
	 }
};

int main() {
	 Buffer buf1(10);
	 buf1.fill('A');
	 buf1.print();

	 Buffer buf2 = buf1;  // Calls the copy constructor
	 buf2.print();

	 return 0;
}
```

### Using `= delete`

#### Extended Example with Dynamic Memory:

```cpp
class UniqueBuffer {
private:
	 char* data;
	 size_t size;

public:
	 UniqueBuffer(size_t s) : size(s) {
		  data = new char[size];
		  memset(data, 0, size);
	 }

	 // Deleted copy constructor
	 UniqueBuffer(const UniqueBuffer&) = delete;

	 // Destructor
	 ~UniqueBuffer() {
		  delete[] data;
	 }

	 void fill(char ch) {
		  memset(data, ch, size);
	 }

	 void print() const {
		  for (size_t i = 0; i < size; ++i) {
				std::cout << data[i];
		  }
		  std::cout << std::endl;
	 }
};

int main() {
	 UniqueBuffer buf1(10);
	 buf1.fill('B');
	 buf1.print();

	 // UniqueBuffer buf2 = buf1;  // Compile error due to deleted copy constructor

	 return 0;
}
```

`I hope this makes the content more accessible to beginners, happy learning! `

---

## 第9课：深入探讨C++中的面向对象编程和类

### 简介

想象一个世界，一切都被组织成整齐的小盒子，每个盒子都代表一个带有其自己的属性和动作的“对象”。这就是面向对象编程（OOP）的精髓。在C++的领域中，这些盒子是使用“类”来创建的。深入了解OOP的魔法和C++中创建类的艺术。

### 理解面向对象编程

1. **什么是OOP？**:
	- 把OOP想象成一个城市。每座建筑（或“对象”）都有自己的设计（或“类”）。这座城市是有组织的、可扩展的，每座建筑都有其用途。
	- 对象就像单独的建筑，根据称为类的蓝图构建。

2. **为什么选择OOP？**:
	- **模块化**：就像在书架上整理书籍一样，OOP将代码组织成类，使其整洁且易于导航。
	- **可重用性**：构建一次，到处使用。类可以被重用，就像乐高积木块一样。
	- **可扩展性**：想在你的城市里增加一座新建筑吗？使用OOP，扩展你的代码就变得轻而易举。

## 在C中模拟OOP：一个创意方法

C就像一个主要进行素描的艺术家，但通过一些创意，它可以绘制出类似OOP的画面。

1. **C中的结构体**：
	- C中的结构体就像基础的乐高积木块。它们可以将不同的部分（或数据类型）组合在一起，但缺乏高级套装的特色。
	
	```c
	struct SimpleBlock {
		 // 属性或部分
	};
	```

2. **全局函数：操纵者**：
	- 虽然C中的结构体是无生命的，但全局函数可以使它们复活，就像操纵者控制木偶一样。
	
	```c
	void animateBlock(struct SimpleBlock *block, other_actions) {
		 // 给块注入生命
	}
	```

3. **C中的巧妙封装**：
	- C的结构体没有像`private`或`public`这样的窗帘（或访问修饰符）。但是，您可以使用源文件中的静态变量将某些内容隐藏在幕后。
	- 公开声明（或接口）通过头文件进行，而秘密在源文件中被耳语。

### C++中的结构体与类：演变

1. **默认访问**：
	- C++结构体就像公园；所有内容（`public`）都是可访问的。
	- 另一方面，C++类就像私人财产。您需要特殊的访问权限（`private`）才能进入。

2. **功能性**：
	- C++结构体已经进化；它们可以跳舞（有成员函数）甚至传递它们的遗产（继承）。
	- C结构体则传统；它们不跳舞也不继承。

3. **继承**：
	- 在C++的世界中，结构体和类都可以有后代，传递他们的遗产。
	- C结构体，作为传统的，不相信继承。

4. **目的**：
	- C++结构体就像公众人物，经常用于公开角色和开放访问。
	- C++类则像秘密特工，封装细节并执行复杂任务。

### 揭开类的神秘面纱

1. **制作一个类**：
	- 制作一个类就像为建筑设计蓝图。
	
	```cpp
	class DreamHouse {
		 // 设计和特色
	};
	```

2. **为类注入生命**：
	- 一旦你有了一个蓝图，你就可以建造建筑物（或对象）。
	
	```cpp
	DreamHouse myHouse;
	```

3. **使用访问修饰符保护秘密**：
	- 使用`public`、`private`和`protected`门决定谁可以查看或更改你的DreamHouse中的内容。
	- 记住，一个好的房子会保护它的贵重物品（`private`）。

4. **属性和行动**：
	- 每所房子都有特点（或属性）和功能（或方法）。
    - 每个对象都可以访问公有的成员和公有的方法，这也被称为公有成员，它们都使用像点一样的结构访问`object.member`。
    - 如果这个对象的地址是与指针绑定的，那就需要使用一个像箭头的运算符实现利用指针间接访问`object_pointer->member`，这个对象里面的公有成员。

---

### 对象的诞生和消亡：构造函数和析构函数

1. **诞生：构造函数**：
	- 构造函数就像建筑物的盛大开幕式。它们设定初始状态。
	
	```cpp
	DreamHouse() {
		 // 盛大开幕
	};
	```

2. **告别：析构函数**：
	- 析构函数是闭幕仪式，确保一切都被整齐地包装起来。
	
	```cpp
	~DreamHouse() {
		 // 说再见
	};
	```

### 特殊成员函数：C++对象的心跳

想象一个对象有生命的世界。它们出生，它们可能会克隆自己，有时它们会搬到新的地方，最终它们会离世。在C++

中，这些生命事件由特殊的成员函数管理。

#### 复制构造函数和复制赋值运算符：克隆机器

1. **复制构造函数**：
	- 把它想象成一个克隆机器。当你想创建一个对象的精确副本时，这个构造函数会被调用。
	- 要警惕浅复制（只复制外壳）和深复制（克隆内外所有内容）之间的区别。
	
	```cpp
	ClassName(const ClassName &source); // 克隆蓝图
	```

2. **复制赋值运算符**：
	- 这就像一个机器，用另一个对象的数据覆盖一个对象。它不是创建一个新对象，而是改变一个现有的对象。
	
	```cpp
	ClassName& operator=(const ClassName &source); // 覆盖蓝图
	```

#### 移动构造函数和移动赋值运算符：搬家人

1. **移动构造函数**：
	- 想象一个对象可以将其心脏（资源）转移到另一个对象。这个构造函数做到了这一点。它很高效，因为它不复制；它重新定位。
	
	```cpp
	ClassName(ClassName &&source); // 心脏转移蓝图
	```

2. **移动赋值运算符**：
	- 与移动构造函数相似，但不是创建一个新对象，而是转移一个对象的心脏以覆盖另一个对象。
	
	```cpp
	ClassName& operator=(ClassName &&source); // 心脏覆盖蓝图
	```

#### 特殊成员函数的本质

这些函数确保我们的对象被高效地管理。它们帮助：
- 正确地克隆对象。
- 有效地移动资源，而不进行不必要的复制。
- 避免像“三法则”这样的陷阱，它规定如果你有自定义的析构函数、复制构造函数或复制赋值运算符，你可能需要所有三个。

---

### 深入探索特殊成员函数

#### 复制构造函数和复制赋值运算符：详细内容

1. **何时被调用？**：
	- **复制构造函数**：想象你有一个玩具。现在，你想要另一个和它一样的玩具。这个构造函数被调用来制作这个复制品。
	- **复制赋值运算符**：你有两个玩具，但你决定一个应该看起来和感觉就像另一个。这个运算符使之成为可能。

2. **陷阱**：
	- **浅复制与深复制**：只复制外壳而不是里面的内容可能会导致问题，特别是在动态内存中。
	- **三法则**：C++中的一个黄金法则，确保如果你自定义析构函数、复制构造函数或复制赋值运算符中的一个，你应该定义其他两个以防止意外行为。

#### 移动构造函数和移动赋值运算符：详细内容

1. **何时被调用？**：
	- **移动构造函数**：当一个对象决定将其本质（资源）转移到一个新对象时，这个构造函数被召唤。
	- **移动赋值运算符**：当一个对象决定接受另一个对象的本质，覆盖其自己的时，这个运算符被调用。

2. **陷阱**：
	- **资源窃取**：移动语义的美在于资源重新定位。但是需要小心确保源对象在移动后仍处于稳定状态。
	- **五法则**：对现代C++中的三法则的进化。如果你定义或删除任何特殊的成员函数，你应该考虑其他四个的影响。

---

### 特殊成员函数的专业建议

1. **隐式定义的函数**：如果你不定义它们，编译器会为你做。但它可能并不总是按你想要的方式。
2. **避免陷阱**：现代工具像`std::unique_ptr`或`std::shared_ptr`可以是救命的，帮助你管理资源，而不是原始指针的麻烦。
3. **明确的默认值**：如果你对默认行为感到满意，你可以使用`= default`明确地告诉编译器。

### 特殊成员函数的陷阱和注意事项

#### 1. 三法则：黄金法则

如果一个类定义了以下其中一个，它可能需要定义所有三个：
- 析构函数
- 复制构造函数
- 复制赋值运算符

**陷阱**：对于管理资源的类不遵循此规则可能导致浅拷贝或内存泄漏的问题。

**预防措施**：总是为这样的类定义所有三个。

#### 2. 五法则：现代规则

随着C++11引入移动语义，如果一个类定义或删除任何特殊成员函数，它应考虑其他四个。

**陷阱**：不处理移动操作可能导致代码效率低下。

**预防措施**：如果你定义自定义复制操作或析构函数，始终定义或删除移动操作。

#### 3. 隐式转换：隐秘的问题

单参数构造函数有时可能导致意外的转换。

**陷阱**：如果不小心，这些可能会引入错误。

**预防措施**：使用`explicit`关键字来防止这种隐秘的转换。

---

### 三法则

三法则建议，如果一个类定义了以下其中一个，它可能需要定义所有三个：

1. 析构函数
2. 复制构造函数
3. 复制赋值运算符

#### 影响：

对于管理动态内存或资源的类，编译器提供的这些函数版本可能导致意外的行为，如浅拷贝、双重删除或内存泄漏。

#### 示例：

**错误的实现**：
```cpp
class String {
    char* data;
public:
    String(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }
    // 缺少自定义复制构造函数和复制赋值运算符
    ~String() {
        delete[] data;
    }
};
```

**正确的实现**：
```cpp
class String {
    char* data;
public:
    String(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }
    String(const String& other) { // 复制构造函数
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
    }
    String& operator=(const String& other) { // 复制赋值
        if (this != &other) {
            delete[] data;
            data = new char[strlen(other.data) + 1];
            strcpy(data, other.data);
        }
        return *this;
    }
    ~String() {
        delete[] data;
    }
};
```

### 五法则（C++11及以后）

随着C++11引入移动语义，五法则建议，如果一个类定义或删除任何特殊成员函数，它应该也考虑其他四个：

1. 析构函数
2. 复制构造函数
3. 复制赋值运算符
4. 移动构造函数
5. 移动赋值运算符

#### 影响：

不正确处理移动操作可能导致代码效率低下或资源泄漏。

#### 示例：

**错误的实现**（缺少移动操作）：
```cpp
class String {
    // ... 如上
};
```

**正确的实现**：
```cpp
class String {
    // ... 如上
    String(String&& other) noexcept : data(other.data) { // 移动构造函数
        other.data = nullptr;
    }
    String& operator=(String&& other) noexcept { // 移动赋值
        if (this != &other) {
            delete[] data;
            data = other.data;
            other.data = nullptr;
        }
        return *this;
    }
};
```

### 其他要点：

1. **隐式定义的特殊函数**：如果程序员没有定义，编译器会隐式定义复制构造函数、复制赋值运算符和析构函数。
从C++11开始，移动构造函数和移动赋值运算符在某些条件下也被隐式定义。

2. **避免陷阱**：使用`std::unique_ptr`或`std::shared_ptr`等智能指针可以帮助管理资源并避免与原始指针和动态内存相关的某些陷阱。

3. **明确的默认值**：在需要默认行为的情况下，您可以使用`= default`明确定义特殊函数。

---

### 三法则

三法则建议，如果一个类定义了以下其中一个，它可能需要定义所有三个：

- 析构函数
- 复制构造函数
- 复制赋值运算符

#### 不正确的`String`类 (违反三法则):

```cpp
class String {
    char* data;
public:
    String(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }
    // 缺少自定义复制构造函数和复制赋值运算符
    ~String() {
        delete[] data;
    }
};
```

在上面的代码中，缺省的复制构造函数和复制赋值运算符将执行浅拷贝。这可能导致双重删除错误或内存泄漏。

#### 正确的`String`类 (遵循三法则):

```cpp
class String {
    char* data;
public:
    String(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }
    String(const String& other) { // 复制构造函数
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
    }
    String& operator=(const String& other) { // 复制赋值
        if (this != &other) {
            delete[] data;
            data = new char[strlen(other.data) + 1];
            strcpy(data, other.data);
        }
        return *this;
    }
    ~String() {
        delete[] data;
    }
};
```

### 五法则 (C++11及以后)

随着C++11引入移动语义，五法则建议，如果一个类定义或删除任何特殊成员函数，它应该也考虑其他四个：

- 析构函数
- 复制构造函数
- 复制赋值运算符
- 移动构造函数
- 移动赋值运算符

#### 不正确的`UniquePointerArray`类 (违反五法则):

```cpp
template<typename T>
class UniquePointerArray {
    T* data;
    size_t size;
public:
    UniquePointerArray(size_t s) : size(s), data(new T[s]) {}
    // 缺少移动构造函数和移动赋值运算符
    ~UniquePointerArray() {
        delete[] data;
    }
};
```

在上面的代码中，移动构造函数和移动赋值运算符都缺失了。这可能导致代码效率低下或资源泄漏。

#### 正确的`UniquePointerArray`类 (遵循五法则):

```cpp
template<typename T>
class UniquePointerArray {
    T* data;
    size_t size;
public:
    UniquePointerArray(size_t s) : size(s), data(new T[s]) {}
    UniquePointerArray(UniquePointerArray&& other) noexcept : data(other.data), size(other.size) {
        other.data = nullptr;
        other.size = 0;
    }
    UniquePointerArray& operator=(UniquePointerArray&& other) noexcept {
        if (this != &other) {
            delete[] data;
            data = other.data;
            size = other.size;
            other.data = nullptr;
            other.size = 0;
        }
        return *this;
    }
    ~UniquePointerArray() {
        delete[] data;
    }
};
```

**关于模板的简短说明：**

您可能已经注意到我们在`UniquePointerArray`示例中使用了`template<typename T>`。如果这对您来说是新的，请不要担心！让我们简要地了解一下。

C++的模板允许我们编写通用代码。您看到的`T`是一个类型占位符。把它想象成任何数据类型的代替品，无论是`int`、`double`、`String`还是其他任何类型。当我们使用`UniquePointerArray`类时，我们可以指定`T`应该是什么类型，使我们的类对于不同的数据类型都是通用和可重用的。

例如：
- `UniquePointerArray<int>`将创建一个整数数组。
- `UniquePointerArray<String>`将创建我们自定义的`String`对象数组。

然而，模板的复杂性超出了本课的范围。现在，只需记住：
- `T`是一个类型的占位符。
- 模板允许我们编写适用于多种数据类型的代码，而不仅仅是一种。

我们将在未来的课程中更深入地探讨模板。现在，请专注于理解三法则和五法则，并知道模板是C++的一个强大功能，我们很快就会一起探索！

---

## 深入构造函数：隐式 vs. 显式

C++中的构造函数在对象的创建和初始化中起到了关键作用。
它们可以大致分为隐式（转换）构造函数和显式构造函数。
理解这两者之间的区别对于编写清晰且无错误的代码至关重要。

### 隐式（转换）构造函数

1. **定义**：隐式构造函数，也被称为转换构造函数，允许C++编译器自动将一种类型转换为另一种类型，而无需明确指令。

2. **示例**:
   ```cpp
   class Box {
   public:
       Box(int size) {
           // 使用给定的大小创建盒子
       }
   };

   Box giftBox = 5;  // 整数5被隐式转换为Box对象。
   ```

3. **详细示例**:
    ```cpp
    class Fraction {
    private:
        int numerator;
        int denominator;
    public:
        // 隐式构造函数
        Fraction(int num) : numerator(num), denominator(1) {}

        void print() {
            std::cout << numerator << "/" << denominator << std::endl;
        }
    };

    void displayFraction(Fraction f) {
        f.print();
    }

    int main() {
    // 10被隐式转换为Fraction(10/1)
    Fraction fraction_object = 10;
    displayFraction(5);  // 5被隐式转换为Fraction(5/1)
    return 0;
    }
```

4. **潜在问题**：虽然方便，但隐式构造函数有时可能导致意外的类型转换，这可能引入错误或不预期的行为。

### 显式构造函数

1. **定义**：显式构造函数防止自动类型转换。通过使用`explicit`关键字标记构造函数，你告诉编译器不要使用此构造函数进行隐式转换。

2. **示例**:
   ```cpp
   class Box {
   public:
       explicit Box(int size) {
           // 使用给定的大小创建盒子
       }
   };

   Box giftBox = 5;  // 由于explicit关键字而导致的编译错误
   Box anotherBox(5);  // 正确的方法是创建一个大小为5的盒子。
   ```

3. **详细示例**:
   ```cpp
   class Fraction {
   private:
       int numerator;
       int denominator;
   public:
       // 显式构造函数
       explicit Fraction(int num) : numerator(num), denominator(1) {}

       void print() {
           std::cout << numerator << "/" << denominator << std::endl;
       }
   };

   void displayFraction(Fraction f) {
       f.print();
   }

   int main() {
       // displayFraction(5);  // 由于explicit关键字而导致的编译错误
       Fraction frac(5);
       displayFraction(frac);  // 正确的方法是传递一个Fraction对象
       return 0;
   }
   ```

4. **优点**：使用`explicit`关键字可以防止意外转换，尤其是当转换可能是非直观的或容易出错的时候。

### 关键点

- **有意识地选择**：隐式构造函数提供了方便，但带有风险。显式构造函数要求明确和有意识地使用，从而减少了潜在的陷阱。
- **最佳实践**：始终审查类设计和潜在的用例。如果构造函数可能导致模棱两可的结果，考虑使用`explicit`标记它。

---

## C++中的特殊指令：`= default` 和 `= delete`

C++11引入了两个强大的说明符：`= default` 和 `= delete`。这些说明符为编译器提供了如何处理特殊成员函数的明确指示。

### 使用`= default`

1. **定义**：`= default`说明符指示编译器为特殊成员函数生成默认实现。

2. **示例**：
   ```cpp
   class Robot {
   public:
       Robot() = default;  // 使用编译器的默认构造函数
   };
   ```

3. **详细的其他示例**：
   ```cpp
   class Point {
   private:
       int x, y;
   public:
       Point() = default;  // 默认构造函数

       Point(int xVal, int yVal) : x(xVal), y(yVal) {}

       void print() {
           std::cout << "(" << x << ", " << y << ")" << std::endl;
       }
   };

   int main() {
       Point p1;  // 调用默认构造函数
       Point p2(5, 10);  // 调用带参数的构造函数

       p1.print();  // 输出：(0, 0) - 因为默认初始化
       p2.print();  // 输出：(5, 10)
       return 0;
   }
   ```

4. **考虑因素**：虽然`= default`很方便，但了解自动提供的实现并确保它们与您的意图一致是很重要的。

### 使用`= delete`

1. **定义**：`= delete`说明符明确禁用某些成员函数，确保它们不能被使用。

2. **示例**：
   ```cpp
   class UniqueRobot {
   public:
       UniqueRobot(const UniqueRobot&) = delete;  // 机器人不能被复制！
   };
   ```

3. **详细的其他示例**：
   ```cpp
   class NonCopyable {
   private:
       int data;
   public:
       NonCopyable(int val) : data(val) {}

       NonCopyable(const NonCopyable&) = delete;  // 删除复制构造函数

       void print() {
           std::cout << "Data: " << data << std::endl;
       }
   };

   int main() {
       NonCopyable obj1(10);
       // NonCopyable obj2 = obj1;  // 由于删除了复制构造函数而导致的编译错误

       obj1.print();  // 输出：Data: 10
       return 0;
   }
   ```

4. **优点**：`= delete`是防止特定操作的强大方法，确保不会误用某些函数或操作。

### 主要收获

- **明确性**：`= default` 和 `= delete`都为编译器提供了明确的指示，确保您的类按预期行为。
- **最佳实践**：始终记录`= delete`的使用，以告知类的用户关于预期限制的信息。

---

#### 带动态内存的扩展示例：

```cpp
class String {
private:
    char* data;

public:
    // 隐式构造函数
    String(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }

    // 拷贝构造函数
    String(const String& other) {
        data = new char[strlen(other.data) + 1];
        strcpy(data, other.data);
    }

    // 析构函数
    ~String() {
        delete[] data;
    }

    void print() const {
        std::cout << data << std::endl;
    }
};

void displayString(String s) {
    s.print();
}

int main() {
    displayString("Hello, World!");  // 隐式地将C字符串转换为String对象
    String str1("Hello");
    String str2 = str1;  // 调用拷贝构造函数
    str2.print();
    return 0;
}
```

### 显式构造函数

#### 带动态内存的扩展示例：

```cpp
class DynamicArray {
private:
    int* data;
    size_t size;

public:
    // 显式构造函数
    explicit DynamicArray(size_t s) : size(s) {
        data = new int[size];
        for (size_t i = 0; i < size; ++i) {
            data[i] = 0;
        }
    }

    // 拷贝构造函数
    DynamicArray(const DynamicArray& other) : size(other.size) {
        data = new int[size];
        for (size_t i = 0; i < size; ++i) {
            data[i] = other.data[i];
        }
    }

    // 析构函数
    ~DynamicArray() {
        delete[] data;
    }

    void set(size_t index, int value) {
        if (index < size) {
            data[index] = value;
        }
    }

    int get(size_t index) const {
        return (index < size) ? data[index] : 0;
    }

    void print() const {
        for (size_t i = 0; i < size; ++i) {
            std::cout << data[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    DynamicArray arr1(5);
    arr1.set(2, 50);
    arr1.print();

    DynamicArray arr2 = arr1;  // 调用拷贝构造函数
    arr2.print();

    // DynamicArray arr3 = 10;  // 由于explicit关键字导致的编译错误
    DynamicArray arr3(10);  // 正确的创建大小为10的DynamicArray的方法
    arr3.print();

    return 0;
}
```

---

## C++中的特殊指令：`= default` 和 `= delete`

### 使用 `= default`

#### 带动态内存的扩展示例：

```cpp
class Buffer {
private:
    char* data;
    size_t size;

public:
    Buffer() = default;

    Buffer(size_t s) : size(s) {
        data = new char[size];
        memset(data, 0, size);
    }

    // 拷贝构造函数
    Buffer(const Buffer& other) : size(other.size) {
        data = new char[size];
        memcpy(data, other.data, size);
    }

    // 析构函数
    ~Buffer() {
        delete[] data;
    }

    void fill(char ch) {
        memset(data, ch, size);
    }

    void print() const {
        for (size_t i = 0; i < size; ++i) {
            std::cout << data[i];
        }
        std::cout << std::endl;
    }
};

int main() {
    Buffer buf1(10);
    buf1.fill('A');
    buf1.print();

    Buffer buf2 = buf1;  // 调用拷贝构造函数
    buf2.print();

    return 0;
}
```

### 使用 `= delete`

#### 带动态内存的扩展示例：

```cpp
class UniqueBuffer {
private:
    char* data;
    size_t size;

public:
    UniqueBuffer(size_t s) : size(s) {
        data = new char[size];
        memset(data, 0, size);
    }

    // 已删除的拷贝构造函数
    UniqueBuffer(const UniqueBuffer&) = delete;

    // 析构函数
    ~UniqueBuffer() {
        delete[] data;
    }

    void fill(char ch) {
        memset(data, ch, size);
    }

    void print() const {
        for (size_t i = 0; i < size; ++i) {
            std::cout << data[i];
        }
        std::cout << std::endl;
    }
};

int main() {
    UniqueBuffer buf1(10);
    buf1.fill('B');
    buf1.print();

    // UniqueBuffer buf2 = buf1;  // 由于已删除的拷贝构造函数而导致的编译错误

    return 0;
}
```

`希望这使内容对初学者更易于理解，祝你学得愉快！`
