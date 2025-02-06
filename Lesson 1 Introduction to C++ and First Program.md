# Lesson 1: Introduction to C++ and First Program

## What is C++?

C++ is a programming language that is used to create all sorts of applications. It's a bit like a more advanced version of the C language, with some extra features that make it easier to create things like video games, web browsers, and operating systems. You can learn more about C++ [here](https://en.cppreference.com/w/cpp).

## How is C++ different from C?

C++ is based on the C language, which means that anything you can do in C, you can also do in C++. But C++ has some extra features that C doesn't have, like the ability to create classes and objects. These features make C++ a bit more complicated than C, but also a lot more powerful.

## Let's write our first C++ program

Here's a simple program that will display the message "Hello, World!" on your screen:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

If you don't fully understand everything in this program, don't worry. For now, just know that `#include <iostream>` is a command that lets us use some basic input and output functions, `main()` is where our program starts running, `std::cout << "Hello, World!" << std::endl;` is how we display the message, and `return 0`; is how we tell the program to stop running.

This lesson is based on an olderversion of C++ called C++98. As we move forward, we'll start using features from newer versions of C++, all the way up to C++20.

`I hope this makes the content more accessible for beginners. Happy learning!`

---

# 第一课：C++介绍和第一个程序

## 什么是C++？

C++是一种编程语言，用于创建各种应用程序。它有点像C语言的更高级版本，具有一些额外的功能，使得创建像视频游戏、网络浏览器和操作系统这样的东西更容易。你可以在[这里](https://zh.cppreference.com/w/cpp)了解更多关于C++的信息。

## C++与C有什么不同？

C++基于C语言，这意味着你在C中可以做的任何事情，也可以在C++中做。但是C++有一些C没有的额外功能，比如创建类和对象的能力。这些功能使C++比C稍微复杂一些，但也更强大。

## 我们来编写第一个C++程序

这是一个简单的程序，它会在你的屏幕上显示消息"Hello, World!"：

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

如果你还不完全理解这个程序中的所有内容，不用担心。现在，只需要知道`#include <iostream>`是一个让我们使用一些基本输入输出函数的命令，`main()`是我们的程序开始运行的地方，`std::cout << "Hello, World!" << std::endl;`是我们显示消息的方式，`return 0;`是我们告诉程序停止运行的方式。

这节课基于一个叫做C++98的旧版本的C++。随着我们的进步，我们将开始使用从C++98到C++20的新版本的C++的功能。

`希望这能让内容对初学者更易理解，祝学习愉快！`