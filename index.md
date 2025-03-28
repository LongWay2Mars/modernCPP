---
title: Welcome to my blog!
---

[TOC]

# QT C++ 培训

## Day1 环境安装和入门（2025.03.05）

### Qt 自带的编译器
Qt 使用 MinGW 作为其默认的编译器。MinGW 是基于 GCC 的编译器，而在 Java 和 Python 中，分别可以使用 Maven 和 `pip install` 等工具安装依赖或第三方库。

C++ 由标准委员会指定标准，不同厂商会实现自己的编译器和标准库。Qt 致力于跨平台开发，封装了一套跨平台的库，但并不是“一次编译，到处运行”。

C++ 参考手册指出，从 C++11 开始，C++ 进入了一个较快的发展阶段，每三年就会有一个新版本发布。

### Qt 的编译脚本：qmake / CMake
`qmake` 是 Qt 官方维护的成熟构建工具，但从 Qt 5.15 版本开始，官方推荐使用 `CMake` 作为默认的构建工具。

#### **示例：Test.pro 文件**
```ini
TEMPLATE = app
CONFIG += console c++11
CONFIG -= app_bundle
CONFIG -= qt

SOURCES += \
        main.cpp
```

### Qt 的版本控制系统
推荐使用 **Git** 进行版本控制。在实际开发中，每个开发人员通常有各自的分工，合理的 Git 流程可以减少代码冲突，提高协作效率。

### C++ 中的头文件
- 头文件主要用于**声明**（declaration）。
- 源文件（.cpp）主要用于**定义和实现**（definition & implementation）。

### C++ 中的命名空间
```cpp
using namespace std;
```

这行代码会将整个 `std` 标准库暴露在源代码中。一般推荐使用具体引用，例如：
```cpp
using std::cout;
using std::cin;
```
这样可以避免命名冲突，提高代码的可读性。

### C++ 中的编译、链接、运行
在 C++ 开发过程中，程序的构建主要包括以下几个阶段：

1. **构建（Build）**
   - **预处理**：处理 `#include` 头文件，展开宏定义。
   - **编译**：将每个 `.cpp` 或 `.c` 文件编译成 `.o`（目标文件），这些目标文件是二进制格式的编译单元。
   - **链接**：将所有 `.o` 目标文件链接在一起，生成 `.exe` 可执行文件。

2. **运行（Run）**
   - 运行编译好的可执行文件，执行程序逻辑。


## Day2 C++语法和工程实践（2025.03.06）
---
### C++中的内置类型

C++ 提供了一系列内置数据类型，用于存储不同类型的值。常见的内置类型包括：

#### 1. 基本数据类型
- **整数类型**：
  - `int`：用于存储整数，大小通常为 4 字节（具体大小依赖于系统）。
  - `short`：较小的整数类型，通常为 2 字节。
  - `long`：较大的整数类型，通常为 4 或 8 字节。
  - `long long`：更大的整数类型，通常为 8 字节。
- **字符类型**：
  - `char`：用于存储字符，通常为 1 字节。
  - `wchar_t`：用于存储宽字符（Unicode 字符），通常为 2 或 4 字节。
- **浮点类型**：
  - `float`：用于存储单精度浮点数，通常为 4 字节。
  - `double`：用于存储双精度浮点数，通常为 8 字节。
  - `long double`：用于存储更高精度的浮点数，大小依赖于系统，通常为 10 或 12 字节。

#### 2. 布尔类型
- **`bool`**：用于存储布尔值，取值为 `true` 或 `false`，通常为 1 字节。

#### 3. 空类型
- **`void`**：表示没有类型，常用于函数返回类型或指针声明。

#### 4. 类型限定符
- **`const`**：常量类型，表示变量的值不可修改。
- **`volatile`**：表示变量可能会被外部因素修改，告诉编译器避免优化。
- **`mutable`**：允许即使在 `const` 成员函数中也可以修改类的某些成员变量。

```cpp
//C++内置类型
//int char float double
void builtin_type()
{
	bool a = true;//1个字节 布尔型

	char b; sizeof(b);//1个字节 字符型
	unsigned char c; sizeof(c);//1个字节

	short d = 0; sizeof(c);//2个字节 短整型
	unsigned short d1; sizeof(d);//2个字节	

	int e; sizeof(e);//4个字节 整型
	unsigned int f; sizeof(f);//4个字节

	long g; sizeof(g);	//4个字节 长整型
	unsigned long h; sizeof(h);//4个字节	

	long long i; sizeof(i);//8个字节 长长整型
	unsigned long long j; sizeof(j);//8个字节

	const char* str = "hello";//str指向的内容不能改变
	sizeof(str);//4个字节 指针的大小 32位系统4个字节 64位系统8个字节 C语言字符串以'\0'结尾 1个字节
	std::cout << strlen(str) << std::endl;//5个字节
	std::cout << sizeof(str) << std::endl;

	const char* str1 = "xxx";
	std::cout << sizeof(str1) << std::endl;
	std::cout << sizeof("xxx") << std::endl;//"xxx"是一个字符串常量，占用4个字节 因为C语言字符串以'\0'结尾 1个字节


  //数组
	//定义：类型 数组名[元素个数] = {元素1, 元素2, ...};
	int arr[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//40个字节
	std::cout << sizeof(arr) << std::endl;//40个字节
	//不用new[]分配的数组是栈上的数组，编译时就确定了数组的大小，也就是要求个数必须是常量（已知的）
	const int aa = 10;
	int a_stack_arr[aa] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//40个字节
	//访问数组元素（第一个值）
	std::cout << arr[0] << std::endl; //1
	std::cout << *arr << std::endl; //1
	//访问第二个值
	std::cout << a_stack_arr[1] << std::endl;		//2
	std::cout << *(a_stack_arr + 1) << std::endl; //2
}

int main() {
    builtin_type();
    return 0;
}
```

---

### C++中的自定义类型

C++ 允许开发者定义自定义类型，以便构建更复杂的程序结构。常见的自定义类型包括：

#### 1. 类（Class）
类是面向对象编程（OOP）中最常见的自定义类型，允许开发者定义对象的属性和方法。

```cpp
class Person {
public:
    std::string name;
    int age;

    // 构造函数
    Person(std::string n, int a) : name(n), age(a) {}

    // 方法
    void introduce() {
        std::cout << "Hello, my name is " << name << " and I am " << age << " years old." << std::endl;
    }
};
```

#### 2. 结构体（Struct）
结构体与类相似，但结构体的成员默认是公共的，且通常用于存储数据。

```cpp
struct Point {
    int x, y;
    Point(int a, int b) : x(a), y(b) {}
};
```

#### 3. 枚举（Enum）
枚举用于表示一组常量值，通常用于有限的离散值。

```cpp
enum Color { RED, GREEN, BLUE };
Color color = GREEN;
```

#### 4. 类型别名（Type Aliases）
C++ 允许使用 `typedef` 或 `using` 为现有类型创建别名，使代码更易读。

```cpp
typedef unsigned long ulong;
using uint = unsigned int;
```

#### 5. 模板（Template）
模板允许定义类型参数化的函数或类，使代码更加通用。

```cpp
template <typename T>
T add(T a, T b) {
    return a + b;
}
```

---

### 左值/右值

在 C++ 中，**左值（Lvalue）** 和 **右值（Rvalue）** 是两个重要的概念，它们与表达式的求值位置和类型紧密相关。

#### 1. 左值（Lvalue）
左值表示可以出现在赋值语句左侧的对象，它表示一个可以被修改的内存位置。

- **左值的特点**：
  - 有持久的内存地址，可以取地址（`&`）。
  - 可以作为赋值的目标。
  - 示例：变量、数组元素、解引用指针等。

```cpp
int a = 10;
a = 20;  // a 是一个左值
```

#### 2. 右值（Rvalue）
右值表示没有持久内存地址的对象，通常是表达式的临时值，不能作为赋值语句的左侧。

- **右值的特点**：
  - 没有明确的内存地址，通常是临时值。
  - 右值可以被用来初始化左值。
  - 示例：常量、临时对象、返回值等。

```cpp
int a = 10;
a = 20 + 30;  // 20 + 30 是右值
```

#### 3. 左值引用与右值引用
- **左值引用（Lvalue Reference）**：绑定到左值，使用 `&`。
  ```cpp
  int a = 10;
  int& ref = a;  // 左值引用
  ```
- **右值引用（Rvalue Reference）**：绑定到右值，使用 `&&`。右值引用允许移动语义，避免不必要的复制。
  ```cpp
  int&& rref = 20;  // 右值引用
  ```

- **普通引用/非常量引用/左值引用**： 变量引用，只能引用左值
  ```cpp
  int c = 0;
  int& ref = c;  // 左值引用
  ```

- **常量引用** ：可以引用左值和右值
  ```cpp
  int c = 0 ;
  const int& crc = c;  // 左值引用
  const int& crc2 = 1;   // 右值引用
  ```

#### 4. 重要概念
- **完美转发（Perfect Forwarding）**：通过右值引用和 `std::forward` 可以将传递给函数的参数“完美”转发给另一个函数，保持其值类别（左值或右值）。
- **移动语义（Move Semantics）**：通过右值引用，C++ 可以“移动”资源，而不是复制，减少不必要的内存分配和提高性能。

---


### 指针/引用

在 C++ 中，**指针（Pointer）** 和 **引用（Reference）** 是两个用于间接访问变量的强大工具。它们在内存管理、函数参数传递、动态分配和面向对象编程中起着重要作用。下面是它们的详尽比较和使用方法：

---

#### 📌 1. 指针（Pointer）

##### 1.1 什么是指针？
- 指针是一个变量，存储的是另一个变量的内存地址。
- 通过指针可以间接访问和修改该地址上的数据。

##### 1.2 指针的语法
```cpp
int a = 10;
int* p = &a; // p 是一个指向 a 的指针，& 取 a 的地址

std::cout << "a 的值: " << a << std::endl;       // 10
std::cout << "a 的地址: " << &a << std::endl;    // 0x7ffeed5c (示例地址)
std::cout << "p 指针存储的地址: " << p << std::endl; // 0x7ffeed5c
std::cout << "通过 p 访问 a 的值: " << *p << std::endl; // 10
```

##### 1.3 常见操作
- **取地址符 `&`**：获取变量的地址。
- **解引用符 `*`**：通过指针访问（读取/修改）变量的值。
```cpp
*p = 20; // 修改 a 的值为 20
std::cout << "a 的新值: " << a << std::endl; // 20
```

##### 1.4 指针的种类
- **空指针（nullptr）**：
```cpp
int* p = nullptr; // 指针不指向任何有效地址
```
- **野指针（Dangling Pointer）**：
```cpp
int* p; // 未初始化的指针，指向未知内存，可能会引发未定义行为
```
- **常量指针（指针常量）**：
```cpp
const int a = 10;
const int* p1 = &a; // p1 指向的值不能修改
int* const p2 = &a;  // p2 本身的指向不能修改
const int* const p3 = &a; // p3 指向和值都不能修改
```
- **指针数组** 和 **数组指针**：
```cpp
int arr[3] = {1, 2, 3};
int* pArr = arr; // 指针指向数组首元素
int (*pArray)[3] = &arr; // 数组指针，指向整个数组
```

##### 1.5 动态内存分配
```cpp
int* p = new int(10); // 动态分配内存并初始化为 10
delete p;             // 释放内存，避免内存泄漏

int* arr = new int[5]; // 动态数组
delete[] arr;          // 释放数组内存
```

---

#### 📌 2. 引用（Reference）

##### 2.1 什么是引用？
- 引用是一个变量的别名（Alias），创建后始终指向初始化的对象，无法更改。
- 使用 `&` 符号定义，但与取地址操作不同。
```cpp
int a = 10;
int& ref = a; // ref 是 a 的引用

ref = 20; // 相当于 a = 20
std::cout << "a 的值: " << a << std::endl; // 20
```

##### 2.2 引用的特点
- **必须初始化**：引用在创建时必须绑定到一个有效变量。
- **不可更改绑定**：引用绑定后无法更改指向其他变量。
- **没有 `null` 引用**：不存在空引用，必须总是绑定有效对象。
- **不占用额外内存**：引用本质上是变量的一个别名，不分配新内存。

##### 2.3 常量引用
- **保护数据不被修改**，常用于函数参数传递中以避免复制大对象。
```cpp
void showValue(const int& ref) {
    std::cout << "值: " << ref << std::endl;
}

int a = 50;
showValue(a); // 安全，不会修改 a 的值
```

---

#### 📌 3. 指针与引用的区别

| 特性                | 指针（Pointer）                 | 引用（Reference）               |
|-------------------|-----------------------------|-----------------------------|
| 初始化               | 可以不初始化（野指针）               | 必须初始化                     |
| 重新绑定             | 可以指向不同地址                   | 绑定后不可改变引用对象               |
| 是否为空             | 可以为 `nullptr`                | 不能为 `null`                 |
| 内存占用             | 需要内存存储地址                   | 不占用内存，作为别名存在              |
| 指针运算             | 支持算术运算 (如 `++`, `--`)        | 不支持指针运算                   |
| 用途                | 动态分配、数组、函数指针、复杂数据结构       | 主要用于安全、简单的别名和参数传递        |
| 安全性               | 容易产生野指针、空指针问题               | 更安全，避免指针操作的风险             |

---

#### 📌 4. 指针和引用在函数中的应用

##### 4.1 通过指针修改参数
```cpp
void modify(int* p) {
    *p = 20;
}

int a = 10;
modify(&a); 
std::cout << "a = " << a << std::endl; // 20
```

##### 4.2 通过引用修改参数
```cpp
void modify(int& ref) {
    ref = 30;
}

int a = 10;
modify(a);
std::cout << "a = " << a << std::endl; // 30
```

##### 4.3 指针和引用的混合使用
```cpp
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

---

#### 📌 5. 什么时候用指针？什么时候用引用？

- **使用指针的场景**：
  - 需要动态内存分配（例如 `new/delete`）。
  - 需要修改指针的指向（例如链表、树结构）。
  - 用于实现复杂数据结构（例如数组、函数指针、回调函数）。
  - 指针数组或数组指针的操作。

- **使用引用的场景**：
  - 用作函数参数，避免对象拷贝，提高效率（尤其是大对象）。
  - 常量引用用于只读访问对象（例如 `const T&`）。
  - 简化语法（例如范围 `for` 循环、运算符重载）。

---

#### 🎯 6. 小结

- **指针** 提供了灵活的内存操作能力，但容易出错，需要小心避免野指针、空指针、内存泄漏。
- **引用** 更安全、语法简洁，但缺乏指针的灵活性。
- 在实际项目中，优先使用引用，只有在需要指针特性时才使用指针，尤其是涉及动态分配和复杂数据结构时。

---
### 优秀实践

```
class MyClass;

MyClass test(const MyClass& a,MyClass &b)
{

}
```
好的做法是加const关键字的参数，通常是作为传入参数（只读），其他参数作为传出参数。

在 C++ 中，**指针（Pointer）** 和 **引用（Reference）** 是两个用于间接访问变量的强大工具。它们在内存管理、函数参数传递、动态分配和面向对象编程中起着重要作用。下面是它们的详尽比较和使用方法：

---
## Day3 移动语义和构造/析构/拷贝构造（2025.03.07）
---
### 进程的内存映像
![alt text](进程的内存映像-1.jpeg)

### 栈和堆
- 栈：传递给函数的参数，是函数里面定义的，不通过malloc/new 分配的变量，都在栈上，对这些变量做拷贝是不计代价的。栈的内容和大小是已知的，编译时才确定。
  
```cpp
void readFile(const std::string& filename)
{
	//读文件的大小
  int size = getSizeofFile();

  //分配堆内存
	char* buf = new char[size];
  /** buf本身存放在栈上，buf指向的是堆内存*/

	//把文件内容读到buf中
	readFileToBuf(buf, size);
	//处理buf中的内容
	process(buf, size);
	//释放堆内存
	delete[] buf;
}
```

- 堆：编译时不知道，只有运行时才确定。堆是计量代价的，如果要使用堆内存，必须手动分配和手动回收，否则会造成内存泄漏。
- ***内存泄漏***： 一直分配内存，但从不手动释放，就会造成操作系统无可用内存，new 分配的内存直到通过delete还给操作系统才会失效，而且使用delete后的内存是非法的。
  - 内存泄露典型案例：在函数分配的堆内存赋给函数的局部变量，而忘记将该局部变量传出来，内存泄露发生在程序运行时，程序结束后，操作系统会回收所有程序的内存。所以内存泄漏往往在长期运行的服务器上发生。
```C++
char* flle = new int[100];
//把堆内存还给操作系统
delete file;
file  = nullptr;
```


### new和malloc
- new 一般是用malloc实现的
```bash
//C++风格
int* pInt = new int[100];//100×4个字节
//C风格
int* pInt2 = (int*)malloc(100 * sizeof(int));//100×4个字节
```
### 类和对象

#### 1. 类（Class）
类是面向对象编程（OOP）中最常见的自定义类型，允许开发者定义对象的属性和方法。

```cpp
class Person {
public:
    std::string name;
    int age;

    // 构造函数
    Person(std::string n, int a) : name(n), age(a) {}

    // 方法
    void introduce() {
        std::cout << "Hello, my name is " << name << " and I am " << age << " years old." << std::endl;
    }
};
```
**面试常见**
```cpp
//MSVC
//指针： 8字节
//总长度 4字节/8字节
//使用长度 4字节/8字节

//GCC
//指针： 8字节
//指向末尾的指针 8字节
//指向最后一个元素的指针 8字节
```

#### 2. 结构体（Struct）
结构体与类相似，但结构体的成员默认是公共的，且通常用于存储数据。



## Day4-1 智能指针（2025.03.18）

### 智能指针的用法

#### MyClass类头文件

```cpp
//MyClass.h
#ifndef MYCLASS_H
#define MYCLASS_H


class MyClass
{
public:
  MyClass(int val);
  ~MyClass();
  void show();

private:
  int value;
};

#endif // MYCLASS_H

```

```

```

#### MyClass类实现文件

```cpp
//MyClass.cpp
#include "MyClass.h"
#include <iostream>

MyClass::MyClass(int val)
  :value(val)
{
  std::cout << "Constructor: " << value << std::endl;
}

MyClass::~MyClass()
{
  std::cout << "Destructor: " << value << std::endl;
}

void MyClass::show()
{
  std::cout << "Value: " << value << std::endl;
}
```

#### 主程序main.cpp

```cpp
#include "myclass.h"
#include <memory>
#include <iostream>
#include <vector>

//#include <QApplication>

// 1️.尝试返回一个 std::unique_ptr<MyClass> 的函数
std::unique_ptr<MyClass> createInstance(int val) {
  return std::make_unique<MyClass>(val); // ✅ 这样是安全的，返回临时 unique_ptr
}

// ❌ 错误示范（不能返回局部变量的 unique_ptr，否则会导致悬空指针）
// std::unique_ptr<MyClass> createInvalidInstance() {
//     std::unique_ptr<MyClass> ptr = std::make_unique<MyClass>(20);
//     return ptr;  // ❌ 这样会尝试拷贝 unique_ptr，而 unique_ptr 不允许拷贝
// }

void testuniqueptr()
{
  std::unique_ptr<MyClass> ptr1 = std::make_unique<MyClass>(10);  // 创建 unique_ptr
  ptr1->show(); // 访问成员函数

  // std::unique_ptr<MyClass> ptr2 = ptr1; // ❌ 错误！unique_ptr 不支持拷贝

  //std::unique_ptr<MyClass> ptr2 = std::move(ptr1);  // ✅ 使用 std::move 转移所有权

  if (!ptr1)
    std::cout << "ptr1 is now nullptr" << std::endl; //ptr1失去所有权

  // 3️⃣ 使用 createInstance() 返回 unique_ptr
  std::unique_ptr<MyClass> ptr3 = createInstance(30);
  ptr3->show();

  // 4️⃣ 使用 std::vector<std::unique_ptr<MyClass>> 存储多个 unique_ptr
  std::vector<std::unique_ptr<MyClass>>   myVector;
  myVector.push_back(std::make_unique<MyClass>(100));
  myVector.push_back(std::make_unique<MyClass>(200));
  myVector.push_back(std::make_unique<MyClass>(300));

  std::cout << "Vector Contentd" << std::endl;
  for (const auto& item : myVector)
  {
    item->show();
  }
}

void testsharedptr()
{
  std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>(10);  // 创建 shared_ptr
  {
    std::shared_ptr<MyClass> ptr2 = ptr1;  // 共享 ptr1 的对象，引用计数+1

    std::cout << "ptr2 use count: " << ptr2.use_count() << std::endl;
    ptr2->show();
  }  // 退出作用域，ptr2 释放，引用计数-1

  std::shared_ptr<MyClass> ptr3 = ptr1;
  std::cout << "ptr3 use count: " << ptr3.use_count() << std::endl;
  ptr3->show();


  std::shared_ptr<MyClass> ptr4 = ptr1;
  std::cout << "ptr4 use count: " << ptr4.use_count() << std::endl;


  std::vector<std::shared_ptr<MyClass>> myVector2;
  myVector2.push_back(std::make_shared<MyClass>(500));
  myVector2.push_back(std::make_shared<MyClass>(600));
  myVector2.push_back(std::make_shared<MyClass>(700));

  std::cout << "Vector Contentd" << std::endl;
  for (const auto& item : myVector2)
  {
    item->show();
  }


  ptr3.reset();  // 释放 ptr3，引用计数 -1
  ptr4.reset();  // 释放 ptr4，引用计数 -1
  ptr1.reset();  // 释放 ptr1，引用计数 -1，此时应该调用析构函数

  std::cout << "ptr1 use count: " << ptr1.use_count() << std::endl;

  // 程序结束，ptr1 释放，引用计数归 0，析构对象
}

class A;

class B {
public:
  std::weak_ptr<A> a_ptr;  // ❗改成 weak_ptr
  ~B() { std::cout << "B destroyed" << std::endl; }
};

class A {
public:
  std::shared_ptr<B> b_ptr;
  ~A() { std::cout << "A destroyed" << std::endl; }
};

void testweakptr()
{
  std::shared_ptr<A> a = std::make_shared<A>();
  std::shared_ptr<B> b = std::make_shared<B>();

  a->b_ptr = b;  // A 持有 B
  b->a_ptr = a;  // B **弱引用** A（不会增加计数）

  //return 0之后 A 和 B 都会被正确释放
}

int main(int argc, char* argv[])
{

	//testuniqueptr();

	//testsharedptr();
  
	testweakptr();
  return 0;  
  //return a.exec();
}

```



## Day4-2移动构造函数和移动赋值运算符 (C++)（2025.03.19）

本主题介绍如何为 C++ 类编写*移动构造函数*和移动赋值运算符。 移动构造函数使右值对象拥有的资源无需复制即可移动到左值中。 有关移动语义的详细信息，请参阅 [Rvalue 引用声明符：&&](https://learn.microsoft.com/zh-cn/cpp/cpp/rvalue-reference-declarator-amp-amp?view=msvc-170)。

此主题基于用于管理内存缓冲区的 C++ 类 `MemoryBlock`。

```cpp
// MemoryBlock.h
#pragma once
#include <iostream>
#include <algorithm>

class MemoryBlock
{
public:

   // Simple constructor that initializes the resource.
   explicit MemoryBlock(size_t length)
      : _length(length)
      , _data(new int[length])
   {
      std::cout << "In MemoryBlock(size_t). length = "
                << _length << "." << std::endl;
   }

   // Destructor.
   ~MemoryBlock()
   {
      std::cout << "In ~MemoryBlock(). length = "
                << _length << ".";

      if (_data != nullptr)
      {
         std::cout << " Deleting resource.";
         // Delete the resource.
         delete[] _data;
      }

      std::cout << std::endl;
   }

   // Copy constructor.
   MemoryBlock(const MemoryBlock& other)
      : _length(other._length)
      , _data(new int[other._length])
   {
      std::cout << "In MemoryBlock(const MemoryBlock&). length = "
                << other._length << ". Copying resource." << std::endl;

      std::copy(other._data, other._data + _length, _data);
   }

   // Copy assignment operator.
   MemoryBlock& operator=(const MemoryBlock& other)
   {
      std::cout << "In operator=(const MemoryBlock&). length = "
                << other._length << ". Copying resource." << std::endl;

      if (this != &other)
      {
         // Free the existing resource.
         delete[] _data;

         _length = other._length;
         _data = new int[_length];
         std::copy(other._data, other._data + _length, _data);
      }
      return *this;
   }

   // Retrieves the length of the data resource.
   size_t Length() const
   {
      return _length;
   }

private:
   size_t _length; // The length of the resource.
   int* _data; // The resource.
};
```

以下过程介绍如何为示例 C++ 类编写移动构造函数和移动赋值运算符。



### 为 C++ 创建移动构造函数

1. 定义一个空的构造函数方法，该方法采用一个对类类型的右值引用作为参数，如以下示例所示：

   ```cpp
   MemoryBlock(MemoryBlock&& other)
      : _data(nullptr)
      , _length(0)
   {
   }
   ```

2. 在移动构造函数中，将源对象中的类数据成员添加到要构造的对象：

   ```cpp
   _data = other._data;
   _length = other._length;
   ```

3. 将源对象的数据成员分配给默认值。 这可以防止析构函数多次释放资源（如内存）:

   ```cpp
   other._data = nullptr;
   other._length = 0;
   ```



### 为 C++ 类创建移动赋值运算符

1. 定义一个空的赋值运算符，该运算符采用一个对类类型的右值引用作为参数并返回一个对类类型的引用，如以下示例所示：

   C++复制

   ```cpp
   MemoryBlock& operator=(MemoryBlock&& other)
   {
   }
   ```

2. 在移动赋值运算符中，如果尝试将对象赋给自身，则添加不执行运算的条件语句。

   C++复制

   ```cpp
   if (this != &other)
   {
   }
   ```

3. 在条件语句中，从要将其赋值的对象中释放所有资源（如内存）。

   以下示例从要将其赋值的对象中释放 `_data` 成员：

   ```cpp
   // Free the existing resource.
   delete[] _data;
   ```

   执行第一个过程中的步骤 2 和步骤 3 以将数据成员从源对象转移到要构造的对象：

   ```cpp
   // Copy the data pointer and its length from the
   // source object.
   _data = other._data;
   _length = other._length;
   
   // Release the data pointer from the source object so that
   // the destructor does not free the memory multiple times.
   other._data = nullptr;
   other._length = 0;
   ```

4. 返回对当前对象的引用，如以下示例所示：

   ```cpp
   return *this;
   ```



### 示例：完成移动构造函数和赋值运算符

以下示例显示了 `MemoryBlock` 类的完整移动构造函数和移动赋值运算符：

```cpp
// Move constructor.
MemoryBlock(MemoryBlock&& other) noexcept
   : _data(nullptr)
   , _length(0)
{
   std::cout << "In MemoryBlock(MemoryBlock&&). length = "
             << other._length << ". Moving resource." << std::endl;

   // Copy the data pointer and its length from the
   // source object.
   _data = other._data;
   _length = other._length;

   // Release the data pointer from the source object so that
   // the destructor does not free the memory multiple times.
   other._data = nullptr;
   other._length = 0;
}

// Move assignment operator.
MemoryBlock& operator=(MemoryBlock&& other) noexcept
{
   std::cout << "In operator=(MemoryBlock&&). length = "
             << other._length << "." << std::endl;

   if (this != &other)
   {
      // Free the existing resource.
      delete[] _data;

      // Copy the data pointer and its length from the
      // source object.
      _data = other._data;
      _length = other._length;

      // Release the data pointer from the source object so that
      // the destructor does not free the memory multiple times.
      other._data = nullptr;
      other._length = 0;
   }
   return *this;
}
```



### 示例 使用移动语义来提高性能

以下示例演示移动语义如何能提高应用程序的性能。 此示例将两个元素添加到一个矢量对象，然后在两个现有元素之间插入一个新元素。 `vector` 类使用移动语义，通过移动矢量元素（而非复制它们）来高效地执行插入操作。

```cpp
// rvalue-references-move-semantics.cpp
// compile with: /EHsc
#include "MemoryBlock.h"
#include <vector>

using namespace std;

int main()
{
   // Create a vector object and add a few elements to it.
   vector<MemoryBlock> v;
   v.push_back(MemoryBlock(25));
   v.push_back(MemoryBlock(75));

   // Insert a new element into the second position of the vector.
   v.insert(v.begin() + 1, MemoryBlock(50));
}
```

该示例产生下面的输出：

```Output
In MemoryBlock(size_t). length = 25.
In MemoryBlock(MemoryBlock&&). length = 25. Moving resource.
In ~MemoryBlock(). length = 0.
In MemoryBlock(size_t). length = 75.
In MemoryBlock(MemoryBlock&&). length = 75. Moving resource.
In MemoryBlock(MemoryBlock&&). length = 25. Moving resource.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 0.
In MemoryBlock(size_t). length = 50.
In MemoryBlock(MemoryBlock&&). length = 50. Moving resource.
In MemoryBlock(MemoryBlock&&). length = 25. Moving resource.
In MemoryBlock(MemoryBlock&&). length = 75. Moving resource.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 25. Deleting resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 75. Deleting resource.
```

在 Visual Studio 2010 之前，此示例生成了以下输出：

```Output
In MemoryBlock(size_t). length = 25.
In MemoryBlock(const MemoryBlock&). length = 25. Copying resource.
In ~MemoryBlock(). length = 25. Deleting resource.
In MemoryBlock(size_t). length = 75.
In MemoryBlock(const MemoryBlock&). length = 25. Copying resource.
In ~MemoryBlock(). length = 25. Deleting resource.
In MemoryBlock(const MemoryBlock&). length = 75. Copying resource.
In ~MemoryBlock(). length = 75. Deleting resource.
In MemoryBlock(size_t). length = 50.
In MemoryBlock(const MemoryBlock&). length = 50. Copying resource.
In MemoryBlock(const MemoryBlock&). length = 50. Copying resource.
In operator=(const MemoryBlock&). length = 75. Copying resource.
In operator=(const MemoryBlock&). length = 50. Copying resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 25. Deleting resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 75. Deleting resource.
```

使用移动语义的此示例版本比不使用移动语义的版本更高效，因为前者执行的复制、内存分配和内存释放操作更少。



### 可靠编程

若要防止资源泄漏，请始终释放移动赋值运算符中的资源（如内存、文件句柄和套接字）。

若要防止不可恢复的资源损坏，请正确处理移动赋值运算符中的自我赋值。

如果为你的类同时提供了移动构造函数和移动赋值运算符，则可以编写移动构造函数来调用移动赋值运算符，从而消除冗余代码。 以下示例显示了调用移动赋值运算符的移动构造函数的修改后的版本：

```cpp
// Move constructor.
MemoryBlock(MemoryBlock&& other) noexcept
   : _data(nullptr)
   , _length(0)
{
   *this = std::move(other);
}
```

[std::move](https://learn.microsoft.com/zh-cn/cpp/standard-library/utility-functions?view=msvc-170#move) 函数将左值 `other` 转换为右值。

## Day4-3 C++四大函数（构造/拷贝构造/析构/赋值运算符函数）（2025.03.19）

### Point类

```cpp
#include <iostream>

using std::cout;
using std::endl;

class Point
{
public:
	Point(int ix = 0, int iy = 0)
		: _ix(ix)
		, _iy(iy)
	{
		cout << "Point(int,int)" << endl;
	}
	~Point() {
		cout << "~Point()" << endl;
	}

	//拷贝构造函数
	Point(const Point& rhs)
		: _ix(rhs._ix)
		, _iy(rhs._iy)
	{
		cout << "Point(const Point&)" << endl;
	}
	//问题1：拷贝构造函数中的引用符号能不能去掉？
	//问题2：拷贝构造函数中的const关键字能不能去掉？

	//解答1：去掉引用符号，会导致无穷递归调用，直到栈溢出（满足拷贝构造函数的调用时机1：形参与实参结合）
	//解答2：去掉const关键字，会导致无法传递右值，即无法传递临时对象

	//左值和右值
	int number = 10;
	int& ref = number;//左值引用

	//int& ref1 = 10;//错误，不能将右值赋值给左值引用
	int&& ref2 = 10;//右值引用
	const int& ref3 = 10;//常量左值引用


	//赋值运算符函数
	Point& operator=(const Point& rhs)
	{
		cout << "Point& operator=(const Point&)" << endl;
		if (this != &rhs)
		{
			_ix = rhs._ix;
			_iy = rhs._iy;
		}
	}

	void print() const
	{
		cout << "(" << _ix
			<< "," << _iy
			<< ")" << endl;
	}

private:
	int _ix;
	int _iy;

};


class Line
{
public:
	Line(int x1, int y1, int x2, int y2)
		:_pt1(x1, y1)//如果不显示调用构造函数，会调用默认构造函数，所以最好可以显示初始化子对象
		, _pt2(x2, y2)
	{
		//cout << "Line(int,int,int,int)" << endl;
	}
	//问题：为什么没有定义析构函数，也没有调用析构函数，就能释放资源？
	// 定义析构函数
	~Line()
	{
		cout << "~Line()" << endl;
	}

public:
	void print() const
	{
		_pt1.print();
		cout << "---->";
		_pt2.print();
	}

private:
	Point _pt1;
	Point _pt2;

};

void testLine()
{
	Line line(1, 2, 3, 4);
	line.print();
}


void test() {
	Point pt;//栈对象
	cout << "pt = ";
	pt.print();

	//Point* p = new Point();//堆对象

	Point pt2(3, 4);//栈对象
	cout << "pt2 = ";
	pt2.print();

	Point pt3 = pt2;//拷贝构造函数
	cout << "pt3 = ";
	pt3.print();
}

Point func()
{
	Point pt3(1, 2);
	cout << "pt3 = ";
	pt3.print();
	return pt3;
}

void test1()
{
	Point pt4 = func();
	cout << "pt4 = ";
	pt4.print();
}

int main(int argc, char* argv[])
{
	cout << "begin test..." << endl;
	//test();

	//test1();
	cout << "end test..." << endl;

	cout << "sizeof(Point)= " << sizeof(Point) << endl;

	testLine();
	return 0;
}
```

### Computer类

```cpp
//Computer.h
#pragma once
#include <iostream>
using std::cout;
using std::endl;
class Computer
{
public:
	Computer(const char* brand,float price);
	Computer(const Computer& rhs);
	Computer operator=(const Computer& rhs);
	~Computer();
	void setBrand(const char* brand);
	void setPrice(int price);
	void print();
	void print() const;
	static void printTotalPrice();//静态成员函数void printTotalPrice();
private:
	char* _brand;
	float _price;
	static float _totalPrice;//静态数据成员
};


```

实现文件

```cpp
#include "Computer.h"
#define _CRT_SECURE_NO_WARNINGS

float Computer::_totalPrice = 0.0f;

/*
1.	构造函数：
•	Computer(const char* brand, float price)：分配内存并复制品牌字符串。
2.	拷贝构造函数：
•	Computer(const Computer& rhs)：分配新内存并复制 rhs 的品牌字符串，而不是简单地复制指针。
3.	析构函数：
•	~Computer()：释放分配的内存。
*/
Computer::Computer(const char* brand, float price)
  : _brand(new char[strlen(brand) + 1]())
  , _price(price)
{
  std::cout << "Computer(const char*,float)" << std::endl;
  strcpy_s(_brand, strlen(brand) + 1, brand);

	_totalPrice += _price;
}


//Computer::Computer(const Computer& rhs) 
//  : _brand(rhs._brand)  // 浅拷贝
//  , _price(rhs._price)

Computer::Computer(const Computer& rhs)
  : _brand(new char[strlen(rhs._brand) + 1]()) // 深拷贝
  , _price(rhs._price)
{
  std::cout << "Computer(const Computer&)" << std::endl;
  strcpy_s(_brand, strlen(rhs._brand) + 1, rhs._brand); // 复制内容
}

/*赋值运算符函数中需要关注的几个问题*/
//赋值运算符函数参数中的引用符号能不能去掉？
//赋值运算符函数参数中的const关键字能不能去掉？
//赋值运算符函数返回类型中的引用能不能去掉？
//赋值运算符函数的返回类型可以不是对象吗？

//解答1：在形参与实参结合的时候，会多执行一次拷贝构造函数
//解答2：去掉const关键字，会导致无法传递右值，即无法传递临时对象
//解答3：函数的返回类型是类类型的时候，会满足拷贝构造函数的调用条件
//解答4：返回类型可以不是对象，但是会导致无法连续赋值 pc3 = pc2 = pc1
Computer Computer::operator=(const Computer& rhs)
{
	// TODO: 在此处插入 return 语句
  std::cout << "Computer& operator=(const Computer&)" << std::endl;
	// 防止自复制 pc = pc
  if (this != &rhs) {
		delete[] _brand;
		_brand = new char[strlen(rhs._brand) + 1]();
		strcpy_s(_brand, strlen(rhs._brand) + 1, rhs._brand);
		_price = rhs._price;
  }
	//_brand = rhs._brand;// 浅拷贝
	//_price = rhs._price;// 浅拷贝
	/*
  * 问题1：两个指针指向同一块内存，当其中一个对象释放内存后，另一个对象的指针就变成了野指针（double free）
	* 例如：pc2 = pc；pc2析构释放内存后，pc的析构函数再次释放内存，导致double free
  */
  
	//double free 的解决办法：使用深拷贝

	/*
  * 问题2：两个指针指向同一块内存，当其中一个对象释放内存后，另一个对象的指针就变成了悬空指针（dangling pointer）
  * pc2改变指针指向之后，原来pc2指向的那块堆内存就找不到了
  */

	//内存泄漏的解决办法：delete[] _brand; _brand = new char[strlen(rhs._brand) + 1](); strcpy(_brand, rhs._brand);
	return *this;
}

Computer::~Computer()
{
  std::cout << "~Computer()" << std::endl;
	_totalPrice -= _price;
  if (_brand) {
		delete[] _brand;
		_brand = nullptr;
  }
}

void Computer::setBrand(const char* brand)
{
  // 这里可以实现设置品牌的逻辑
  // 释放旧的品牌内存
  delete[] _brand;
  // 分配新内存并复制品牌
  _brand = new char[strlen(brand) + 1]();
  strcpy_s(_brand, strlen(brand) + 1, brand);
}



void Computer::setPrice(int price)
{
  // 这里可以实现设置价格的逻辑
	_price = price;
}

void Computer::print()
{
	cout << "print()" << endl;
  std::cout << "brand:" << _brand << std::endl;
  std::cout << "price:" << _price << std::endl;
}

void Computer::print() const
{
	cout << "print() const" << endl;
	std::cout << "brand:" << _brand << std::endl;
	std::cout << "price:" << _price << std::endl;
}

void Computer::printTotalPrice()
{
	std::cout << "total price:" << _totalPrice << std::endl;
}

void testStaticMember() {
	cout << "购买第一台电脑前的总价：" << endl;
	Computer::printTotalPrice();
	Computer pc("lenovo", 5555);
	pc.print();
	cout << "购买第一台电脑后的总价：" << endl;
	pc.printTotalPrice();
	Computer pc2("Mac", 8888);
	pc2.print();
	cout << "购买第二台电脑后的总价：" << endl;
	pc2.printTotalPrice();
	cout << endl;
	pc2.setBrand("Mac");
	pc.print();
	pc2.print();
}

void testCopyConstructor() {
  Computer pc("lenovo", 5555);
  /*pc.setBrand("ThinkPad");
  pc.setPrice(8888);*/
  pc.print();

  Computer pc2 = pc;
  pc2.print();

  cout << endl;
	pc2.setBrand("Mac");
	pc.print();
	pc2.print();
}


void testAssignmentOperator() {
	Computer pc("lenovo", 5555);
	cout << "pc: " << endl;
	pc.print();
	Computer pc2("Mac", 8888);
	cout << "pc2: " << endl;
	pc2.print();
	pc2 = pc;
  cout << "pc2: " << endl;
	pc2.print();

  pc2 = Computer("lenovo", 6666);
  pc2.print();
}

void testConst()
{
	//const对象只能调用const成员函数，非const对象可以调用任意成员函数
	const Computer pc("lenovo", 5555);
	pc.print();
	cout << endl << endl;

	Computer pc2("Mac", 8888);
	pc2.print();
}

int main(int argc, char* argv[])
{
  //testCopyConstructor();

	//testAssignmentOperator();

	//testStaticMember();

	testConst();
  return 0;
}

```
---

## Day4-4 New 与 delete 表达式（2025.03.20）

在 C++ 中，`new` 和 `delete` 是用于动态内存管理的关键字。它们分别用于在堆上分配和释放对象或数组。

### 1. `new` 表达式的三个步骤

1. **调用 `operator new` 标准库函数**
   - `operator new` 负责在堆上申请一块原始的、未初始化的内存空间，以便存储对象。
   - 如果申请失败，会抛出 `std::bad_alloc` 异常（除非使用 `nothrow` 版本的 `new`）。

2. **调用构造函数**
   - 在申请到的内存空间上调用构造函数，初始化对象的数据成员。

3. **返回指针**
   - 返回指向新分配对象的指针，以供程序使用。

示例代码：
```cpp
class A {
public:
    A() { cout << "A() constructor" << endl; }
    ~A() { cout << "~A() destructor" << endl; }
};

int main() {
    A* p = new A;  // 调用 operator new 申请内存 -> 调用构造函数 -> 返回指针
    delete p;      // 调用析构函数 -> 调用 operator delete 释放内存
    return 0;
}
```

### 2. `delete` 表达式的两个步骤

1. **调用析构函数**
   - 在释放对象所占的内存前，先执行析构函数，以回收对象的数据成员所占用的资源（如动态分配的内存、文件句柄等）。

2. **调用 `operator delete` 库函数**
   - 释放对象本身所占用的内存空间，使其可被重新分配。

示例代码：
```cpp
A* p = new A;
delete p; // 先调用析构函数，再释放内存
```

### 3. `new[]` 与 `delete[]`

- `new[]` 用于动态分配对象数组，必须使用 `delete[]` 释放。
- `delete` 只能释放单个对象，而 `delete[]` 释放的是整个数组。

```cpp
A* arr = new A[5];  // 申请 5 个对象的数组

delete[] arr;       // 释放数组
```

如果用 `delete` 释放 `new[]` 申请的数组，可能会导致未完全析构的对象泄漏。

---

## Day5 类的定义和关键字再探（2025.03.24）

### 1. C++ 关键字 `const`、`static`、`extern`

```cpp
const int global_a = 10;  // 只读常量
static int s_b = 10;      // 静态全局变量（仅限当前文件可见）
extern int SIZE = 10;     // 外部变量（多个文件可共享）
```

- `const`：用于定义只读变量，防止意外修改。
- `static`：修饰局部变量时，延长变量生命周期，修饰全局变量时，限制其作用域。
- `extern`：用于声明外部变量，使其可以在多个文件中访问。

示例：
```cpp
void test() {
    static int fun_c = 0;  // 仅初始化一次，生命周期贯穿整个程序
    fun_c++;
    cout << "fun_c = " << fun_c << endl;
}
```

### 2. 类的定义：Circle 和 Rect

```cpp
class Circle {
public:
    Circle(double r = 0) : _r(r) {
        cout << "Circle()" << endl;
    }
    
    ~Circle() {
        cout << "~Circle()" << endl;
    }

    double getArea() const {
        return M_PI * _r * _r;
    }

private:
    double _r;
};
```

**要点：**
- **构造函数（Constructor）：** 负责初始化对象，可以带默认参数。
- **析构函数（Destructor）：** 负责清理资源，名称前带 `~`。
- **成员函数（Member Function）：** `const` 关键字保证不会修改对象数据。
  
### 3. 完整代码

```cpp
#define _USE_MATH_DEFINES
#include <iostream>
#include <cmath>

using namespace std;
//关键字
//const
const int global_a = 10;
//static
static int s_b = 10;
//extern
extern int SIZE  = 10;
//三者的用途：const 修饰常量，static 修饰全局变量，extern 修饰外部变量

void test()
{
	int local_v = 0;
	static int fun_c;//静态局部变量 可以延长变量的生命周期
	fun_c++;
	cout << "local_v = " << " " << local_v << " ," << "fun_c = " << " " << fun_c << endl;
}

//定义一个圆
class Circle
{
public:
	Circle(double r = 0)
		: _r(r)
	{
		cout << "Circle(double r = 0)" << endl;
	}

	Circle(double r,char* name)
		: _r(0)
		, _name(new char[strlen(name) + 1])
	{
		strcpy_s(_name, strlen(name) + 1,name);
		cout << "Circle(double r = 0)" << endl;
	}
	~Circle()
	{
		cout << "~Circle()" << endl;
	}

	double getRadius() const
	{
		return _r;
	}

	//setRadius
	void setRadius(double r)
	{
		_r = r;
	}

	double getArea() const
	{
		return M_PI * _r * _r;
	}

private:
	double _r;
	char* _name;
};

//定义一个矩形
class Rect
{
public:
	Rect(double l = 0, double w = 0)
		: _l(l)
		, _w(w)
	{
		cout << "Rect(double l = 0, double w = 0)" << endl;
	}
	~Rect()
	{
		cout << "~Rect()" << endl;
	}

private:
	double _l;
	double _w;
};

void testCircle()
{
	Circle c1(10);
	cout << "c1.getRadius() = " << c1.getRadius() << endl;
	cout << "c1.getArea() = " << c1.getArea() << endl;
	cout << endl << endl;
	Circle c2;
	c2.setRadius(20);
	cout << "c2.getRadius() = " << c2.getRadius() << endl;
	cout << "c2.getArea() = " << c2.getArea() << endl;
}

int main()
{
	test();
	test();
	test();
	cout << endl << endl;

	testCircle();
	return 0;
}
```

---

## Day5-1 运算符重载（2025.03.24）

### 1. 复数类 `Complex` 及其运算符重载

```cpp
class Complex {
    friend Complex operator*(const Complex& lhs, const Complex& rhs);
public:
    Complex(double r = 0, double i = 0) : _real(r), _imag(i) {}
    
    Complex operator-(const Complex& rhs) const {
        return Complex(_real - rhs._real, _imag - rhs._imag);
    }

    Complex& operator++() {  // 前置++
        ++_real;
        ++_imag;
        return *this;
    }

    Complex operator++(int) {  // 后置++
        Complex tmp(*this);
        _real++;
        _imag++;
        return tmp;
    }
    
    Complex& operator+=(const Complex& rhs) {
        _real += rhs._real;
        _imag += rhs._imag;
        return *this;
    }

    void display() const {
        cout << _real << " + " << _imag << "i" << endl;
    }

private:
    double _real;
    double _imag;
};
```

### 2. 运算符重载的几种方式

- **成员函数重载**（适用于 `+=`、`++`、`-`）：
  ```cpp
  Complex operator-(const Complex& rhs) const;
  Complex& operator++();
  Complex operator++(int);
  Complex& operator+=(const Complex& rhs);
  ```

- **友元函数重载**（适用于 `+`、`*`）：
  ```cpp
  friend Complex operator+(const Complex& lhs, const Complex& rhs);
  friend Complex operator*(const Complex& lhs, const Complex& rhs);
  ```

### 3. 关键区别

| 运算符 | 适用方式 | 说明 |
|--------|---------|------|
| `+` | 普通函数 | 需要访问两个对象的数据，可用友元函数 |
| `-` | 成员函数 | 只需要访问当前对象数据 |
| `++` | 成员函数 | 影响当前对象，前置++返回引用，后置++返回值 |
| `*` | 友元函数 | 需要访问两个对象的数据 |

示例代码：
```cpp
Complex c1(1, 2), c2(3, 4);
Complex c3 = c1 + c2;
c3.display();
```

**总结：**
- 友元函数适用于二元运算（如 `+`、`*`）。
- 成员函数适用于一元运算（如 `++`）和复合赋值运算（如 `+=`）。


### 4.完整代码

```cpp
#include <iostream>
#include <limits.h>

using namespace std;

//复数
class Complex
{
	friend Complex operator*(const Complex& lhs, const Complex& rhs);
	//Complex operator/(const Complex& rhs) const;
public:
	Complex(double r = 0, double i = 0)
	: _real(r), _imag(i) 
	{
		cout << "Complex(double r = 0, double i = 0)" << endl;
	}

	~Complex()
	{
		cout << "~Complex()" << endl;
	}

	double getReal() const
	{
		return _real;
	}

	double getImag() const
	{
		return _imag;
	}

	//运算符重载之成员函数
	Complex operator-(const Complex& rhs) const
	{
		return Complex(_real - rhs._real, _imag - rhs._imag);
	}
	
	//自增运算符重载
	Complex& operator++() //前置++
	{
		++_real;
		++_imag;
		return *this;
	}

	//后置自增运算符重载
	Complex operator++(int) //后置++
	{
		Complex tmp(*this);
		_real++;
		_imag++;
		return tmp;//返回类型是一个临时对象，所以不适用引用类型
	}

	//前置++和后置++的区别？
	//解答：前置++返回是对象的引用，是左值可以取地址
	//后置++返回的是局部对象，是右值不能取地址
	
	//+=运算符重载
	//复合赋值运算符重载，推荐以成员函数的形式重载
	Complex& operator+=(const Complex& rhs)
	{
		_real += rhs._real;
		_imag += rhs._imag;
		return *this;
	}



	void display() const
	{
		
		if (_imag > 0)
		{
			cout << _real << " + " << _imag << "i" << endl;
		}
		else if (_imag == 0)
		{
			cout << _real << endl;
		}
		else
		{
			cout << _real << " - " << -_imag << "i" << endl;
		}
	}
private:
	double _real;
	double _imag;
};	

//“operator + ”必须至少有一个类类型的形参
//int operator+(int lhs, int rhs)
//{
//	//return _real + rhs;
//}

//运算符重载之普通函数
Complex operator+(const Complex& lhs, const Complex& rhs)
{
	return Complex(lhs.getReal() + rhs.getReal(), lhs.getImag() + rhs.getImag());
}


//运算符重载之友元函数（推荐使用）
Complex operator*(const Complex& lhs, const Complex& rhs)
{
	return Complex(lhs._real * rhs._real - lhs._imag * rhs._real,
		lhs._real * rhs._real + lhs._real * rhs._real);
}

void test()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;

	Complex c2(3, 4);
	cout << "c2 = ";	
	c2.display();
	cout << endl << endl;

	Complex c3 = c1 + c2;
	cout << "c3 = c1 + c2 = ";
	c3.display();
}

void test2()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;
	Complex c2(3, 4);
	cout << "c2 = ";	
	c2.display();
	cout << endl << endl;

	Complex c3 = c1 - c2;
	cout << "c3 = c1 - c2 = ";
	c3.display();

}

void test3()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;
	Complex c2(3, 4);
	cout << "c2 = ";
	c2.display();
	cout << endl << endl;
	Complex c3 = c1 * c2;
	cout << "c3 = c1 * c2 = ";
	c3.display();

	c3 += c1;
	cout << "c3 += c1 = ";
	c3.display();

	
}

void test4()
{
	Complex c3(4, 6);
	cout << "c3 = ";
	c3.display();
	cout << endl << endl;
	++c3;
	cout << "++c3 = ";
	c3.display();

	cout << endl << endl;
	c3++;
	cout << "c3++ = ";
	c3.display();

	Complex c4(4, 6);
	cout << "c4 = ";
	c4.display();
	c4++;
	cout << "c4++ = ";
	c4.display();
	cout << "前置++返回是对象的引用，是左值可以取地址，后置++返回的是局部对象，是右值不能取地址";
}

int main(int argc, char*  argv[])
{
	//test();
	//test2();
	//test3();
	test4();
	return 0;
}
```
---

## Day5-2 文件IO（2025.03.24）

### 1. 缓冲区与刷新

在C++的标准输入输出流 (`iostream`) 中，I/O 操作通常会涉及缓冲区，以提高效率。默认情况下，`cout` 采用缓冲模式，输出内容会暂存到缓冲区，只有在缓冲区满了或遇到某些特殊情况（如换行）时，数据才会被刷新到屏幕。

#### 1.1 常见的缓冲刷新方式

| 方法 | 作用 | 例子 |
|------|------|------|
| `cout << endl;` | 输出换行并刷新缓冲区 | `cout << "Hello" << endl;` |
| `cout << flush;` | 只刷新缓冲区，但不换行 | `cout << "Hello" << flush;` |
| `cout << ends;` | 在缓冲区加入一个空字符（`\0`），但不会刷新 | `cout << "Hello" << ends;` |

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void test()
{
    for (size_t i = 0; i < 1024; i++)
    {
        cout << "a";
    }

    cout << 'b' << endl; // 刷新缓冲区并换行
    cout << "c" << flush; // 只刷新，不换行
    cout << "d" << ends;  // 只插入空字符，不刷新

    this_thread::sleep_for(chrono::seconds(5));
}

int main()
{
    test();
    return 0;
}
```

### 2. 文件读写操作

C++标准库提供了 `<fstream>` 头文件，其中包含三个主要的文件流类：

| 类名 | 作用 |
|------|------|
| `ifstream` | 读文件（input file stream） |
| `ofstream` | 写文件（output file stream） |
| `fstream` | 读写文件（file stream） |

#### 2.1 读取文件

```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

void readFile()
{
    ifstream ifs("test.txt"); // 打开文件
    if (!ifs.is_open())
    {
        cerr << "文件打开失败" << endl;
        return;
    }

    string line;
    while (getline(ifs, line))
    {
        cout << line << endl;
    }
    ifs.close(); // 关闭文件
}

int main()
{
    readFile();
    return 0;
}
```

#### 2.2 写入文件

```cpp
void writeFile()
{
    ofstream ofs("output.txt");
    if (!ofs)
    {
        cerr << "文件打开失败" << endl;
        return;
    }

    ofs << "Hello, world!" << endl;
    ofs.close();
}
```

#### 2.3 追加模式写入

使用 `ios::app` 选项可以在文件末尾追加内容，而不会覆盖原有数据。

```cpp
void appendToFile()
{
    ofstream ofs("log.txt", ios::app);
    if (!ofs)
    {
        cerr << "文件打开失败" << endl;
        return;
    }
    ofs << "新日志记录" << endl;
    ofs.close();
}
```

#### 2.3 完整代码

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using namespace std;

void test()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	while (ifs >> line)//cin >> line
	{
		cout << line << endl;
	}
}

void test2()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	while (getline(ifs,line))
	{
		cout << line << endl;
	}
}

//打印前几行的内容
void test3()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	int lineNum = 0;
	while (getline(ifs, line))
	{
		cout << line << endl;
		lineNum++;
		if (lineNum == 5)
		{
			break;
		}
	}
}

//打印指定行的内容
void test4()
{
	string fileName = "test.txt";
	ifstream ifs(fileName);
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line[120];
	size_t lineNum = 0;

	vector<string> vec;
	while (getline(ifs, line[lineNum]))
	{
		++lineNum;
	}
	cout << "line[22] = " << line[22] << endl;
}

void test5()
{
	string fileName = "test.txt";
	ifstream ifs(fileName);
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	vector<string> vec;
	while (getline(ifs, line))
	{
		vec.push_back(line);
	}
	cout << "vec[22] = " << vec[22] << endl;
}

void test6()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cerr << "ifstream is not good!" << endl;
		return;
	}

	ofstream ofs("wd.txt");
	if (!ofs.good())
	{
		cerr << "ofstream is not good!" << endl;
		ifs.close();//在程序异常退出时，关闭前面的ifs
		return;
	}

	string line;
	while (getline(ifs, line))
	{
		ofs << line << endl;
	}

	ifs.close();
	ofs.close();
}

void test7()
{
	//对于文件的输出输出流而言，当文件不存在的时候就打开失败
	fstream fs("wenchang.txt");
	if (!fs.good())
	{
		cerr << "fstream is not good!" << endl;
		return;
	}
	//业务逻辑
	//通过键盘输入数据,使用fs进行读文件，随后输出到屏幕
	int number = 0;
	cout << "请输入5次数字： " << endl;
	for (size_t idx = 0; idx != 5; ++idx)
	{
		cin >> number;
		fs << number << " ";
	}

	size_t len = fs.tellp();
	cout << "len= " << len << endl;
	fs.seekp(0);
	len = fs.tellp();
	cout << "len= " << len << endl;

	for (size_t idx = 0; idx != 5; ++idx)
	{
		cout << "fs.failbit = " << fs.fail() << endl
			<< "fs.eofbit = " << fs.eof() << endl
			<< "fs.goodbit = " << fs.good() << endl;
		fs >> number;
		cout << number << " ";
	}
	cout << endl;

	fs.close();
}

//追加模式
void test8()
{
	ifstream ifs("test.txt", std::ios::in | std::ios::ate);
	if (!ifs.good())
	{
		cerr << "ifstream is not good!" << endl;
		return;
	}

	cout << "ifs.tellg() = " << ifs.tellg() << endl;

	ifs.close();
}

void test9()
{
	ofstream ofs("wenchang.txt",std::ios::out | std::ios::app);
	if (!ofs.good())
	{
		cerr << "ofstream is not good!" << endl;
		return;
	}

	cout << "ofs.tellg() = " << ofs.tellp() << endl;

	ofs.close();
}


int main(int argc, char* argv[])
{	
	//test();
	//test5(); //读取文件内容到vector容器中
	test9();
	return 0;
}

```

### 3. 文件定位操作

文件流支持 `seekg()`（get）和 `seekp()`（put）来定位文件指针。

```cpp
void filePositioning()
{
    fstream fs("test.txt", ios::in | ios::out);
    if (!fs)
    {
        cerr << "文件打开失败" << endl;
        return;
    }

    fs.seekg(0, ios::end); // 移动到文件末尾
    cout << "文件大小: " << fs.tellg() << " 字节" << endl;
    fs.close();
}
```

### 4. 字符串IO

C++ 提供了 `stringstream` 以便在字符串中进行 I/O 操作。

```cpp
#include <sstream>

void stringStreamDemo()
{
    stringstream ss;
    int num = 42;
    ss << "数字: " << num;
    string output = ss.str();
    cout << output << endl;
}
```

### 5. 配置文件解析示例

```cpp
void readConfig(const string& filename)
{
    ifstream ifs(filename);
    if (!ifs)
    {
        cerr << "打开文件失败: " << filename << endl;
        return;
    }

    string line;
    while (getline(ifs, line))
    {
        istringstream iss(line);
        string key, value;
        iss >> key >> value;
        cout << key << " -> " << value << endl;
    }
    ifs.close();
}
```

### 6. 完整代码

```cpp
#include <iostream>
#include <sstream>
#include <fstream> // 添加此行以包含 ifstream 的定义

using namespace std;
using std::ostringstream;
using std::istringstream;
using std::stringstream;

string int2string(int value)
{
	ostringstream oss;//中转
	oss << value;

	return oss.str();//获取底层的字符串
}

void  test()
{
	int number = 10;
	string s1 = int2string(number);
	cout << "s1 = " << s1 << endl;
}

void test2()
{
	int number1 = 10;
	int number2 = 20;
	stringstream ss;
	ss << "number1 = " << number1
	 	<< "number2 = " << number2;
	string s1 = ss.str();
	cout << "s1 =" <<  s1 << endl;

	string key;
	string value;
	while (ss >> key >> value)
	{
		cout << key << "--->" << value << endl;
	}

}

void readConfig(const string& filename)
{
	ifstream ifs(filename);
	if (!ifs)
	{
		cerr << "open" << filename << "error!" << endl;
		return;
	}

	string line;
	while (getline(ifs, line))
	{
		istringstream iss(line);
		string key, value;
		iss >> key >> value;

		 
		cout << key << "  " << value << endl;
	}
	ifs.close();
}

void test3()
{
	readConfig("my.conf");
}

int main()
{
	test3();
	return 0;
}
```


### 7. 二进制文件操作

```cpp
#include <iostream>
#include <fstream>
using namespace std;

struct Data {
    int id;
    double value;
};

// 写入二进制文件
void writeBinaryFile(const string &filename)
{
    ofstream ofs(filename, ios::binary);
    if (!ofs)
    {
        cerr << "文件打开失败!" << endl;
        return;
    }
    Data d = {1, 3.14};
    ofs.write(reinterpret_cast<const char *>(&d), sizeof(d));
    ofs.close();
}

// 读取二进制文件
void readBinaryFile(const string &filename)
{
    ifstream ifs(filename, ios::binary);
    if (!ifs)
    {
        cerr << "文件打开失败!" << endl;
        return;
    }
    Data d;
    ifs.read(reinterpret_cast<char *>(&d), sizeof(d));
    cout << "id: " << d.id << ", value: " << d.value << endl;
    ifs.close();
}

int main()
{
    string filename = "data.bin";
    writeBinaryFile(filename);
    readBinaryFile(filename);
    return 0;
}
```

### 总结
1. **文本文件**
   - `ifstream` 读取文件，`ofstream` 写入文件，`fstream` 读写文件。
   - `seekg()` 和 `seekp()` 控制文件指针。
   - `stringstream` 适用于字符串的 I/O 操作。
   - `ios::app` 可用于追加模式写入文件。
2. **二进制文件**
   - 读写方式：`ifstream`、`ofstream` 需使用 `ios::binary` 模式
   - 读取方式：`read()` 和 `write()`
   - 适用于存储结构化数据，如 `struct` 类型的存储和读取。


---




## **Day6-1 输入输出流运算符重载（2025.03.25）**

```plaintext
1. 拷贝构造函数的调用时机
2. 友元
   2.1 友元函数
3. 输入输出流运算符重载
   3.1 关键知识点
   3.2 代码
   3.3 关键问题
   3.4 完整代码
4. 下标访问运算符 `operator[]`
   4.1 关键知识点
   4.2 代码
5. 函数调用运算符 `operator()`
   5.1 关键知识点
   5.2 代码
   5.3 示例
   5.4 完整代码
6. 总结
```

---

### **1. 回顾**
#### **1.1 拷贝构造函数的调用时机**
拷贝构造函数在以下情况会被调用：
1. **对象初始化**  
   - 当用一个已经存在的对象去初始化一个刚刚创建的对象时，调用拷贝构造函数。  
   - 例：
     ```cpp
     Complex c1(1, 2);
     Complex c2 = c1; // 调用拷贝构造函数
     ```
   
2. **函数参数传递**
   - 当形参与实参都是对象时，在函数调用时会调用拷贝构造函数。  
   - 例：
     ```cpp
     void func(Complex c) { }
     func(c1); // 形参与实参结合时调用拷贝构造函数
     ```

3. **函数返回对象**
   - 当函数返回一个对象时，可能调用拷贝构造函数（但现代 C++ 编译器会尝试优化此过程，如返回值优化 RVO）。
   - 例：
     ```cpp
     Complex func() {
         Complex c(3, 4);
         return c; // 可能调用拷贝构造函数
     }
     ```

---

### **2. 友元**
#### **2.1 友元函数**
- 友元函数可以访问类的私有成员。
- 友元声明可以出现在类的 `public`、`protected` 或 `private` 部分，不影响其权限。
- 友元关系是**单向的、不可传递的**：
  - **单向**：如果 `A` 是 `B` 的友元，`B` 并不会自动成为 `A` 的友元。
  - **不可传递**：如果 `A` 是 `B` 的友元，`B` 是 `C` 的友元，`A` 并不会自动成为 `C` 的友元。

---

### **3. 输入输出流运算符重载**
#### **3.1 关键知识点**
1. **`operator<<` 必须是友元函数**
   - 由于 `cout << c1;` 左操作数是 `std::ostream`，不能修改 `std::ostream`，所以 `operator<<` 不能是 `Complex` 的成员函数。
   
2. **`operator>>` 不能是 `const` 成员函数**
   - 因为 `operator>>` 需要修改对象的值，因此不能加 `const`。

#### **3.2 代码**
```cpp
std::ostream& operator<<(std::ostream& os, const Complex& rhs) {
    if (rhs._imag > 0) {
        os << rhs._real << " + " << rhs._imag << "i";
    } else if (rhs._imag == 0) {
        os << rhs._real;
    } else {
        os << rhs._real << " - " << -rhs._imag << "i";
    }
    return os;
}

std::istream& operator>>(std::istream& is, Complex& rhs) {
    std::cout << "请输入复数的实部和虚部：" << std::endl;
    is >> rhs._real >> rhs._imag;
    return is;
}
```

#### **3.3 关键问题**
- **为什么 `operator<<` 和 `operator>>` 的返回值是 `std::ostream&` 和 `std::istream&` ？**  
  - 这样可以实现**连续输入输出**：
    ```cpp
    cout << c1 << c2 << endl;  // 连续输出
    cin >> c1 >> c2;           // 连续输入
    ```

- **为什么 `operator<<` 的 `ostream&` 参数不能去掉 `&` ？**  
  - 因为 `ostream` 的拷贝构造函数已被 `delete`，不能复制 `ostream` 对象。

#### **3.4 完整代码**

```cpp
#include <iostream>
#include <limits.h>
#include <ostream>

using namespace std;

//复数
class Complex
{
	friend Complex operator*(const Complex& lhs, const Complex& rhs);
	//Complex operator/(const Complex& rhs) const;
public:
	Complex(double r = 0, double i = 0)
		: _real(r), _imag(i)
	{
		cout << "Complex(double r = 0, double i = 0)" << endl;
	}

	~Complex()
	{
		cout << "~Complex()" << endl;
	}

	

	void display() const
	{

		if (_imag > 0)
		{
			cout << _real << " + " << _imag << "i" << endl;
		}
		else if (_imag == 0)
		{
			cout << _real << endl;
		}
		else
		{
			cout << _real << " - " << -_imag << "i" << endl;
		}
	}

	//成员函数，operator输出运算符重载 
	//cout << c1 << endl; 
	//第一个参数cout ,第二个参数c1
	//std::ostream& operator<<(std::ostream& os, const Complex& rhs); //error！隐含this指针

	//对于输出流运算符函数而言，不能写成成员函数的形式，因为违背了运算符重载的原则，不能改变操作数的顺序
	//std::ostream& operator<<(std::ostream& os);//error！this指针在参数列表的第一个位置

	

		/*友元函数可以放在类的 任何位置（public / protected / private），不影响其功能。
		从代码规范角度，建议统一放在类定义的开头或结尾，以提高可读性。
		友元关系是单向的，且不具备传递性（即类 A 的友元函数不会自动成为类 B 的友元）。
		*/
	friend std::ostream& operator<<(std::ostream& os, const Complex& rhs);
	//输入流运算符重载
	friend std::istream& operator>>(std::istream& is, Complex& rhs);

private:
	double _real;
	double _imag;
};
//问题1 ：参数列表中ostrream的引用符号&能不能去掉？
//解答1 ：不能去掉因为形参"os"和实参"cout"结合的时候会满足拷贝构造函数的调用时机，但是ostream中的拷贝构造函数已被delete

//问题2 ： 函数返回类型中的引用符号&能不能删除？
//解答2 ： 不能取掉，因为return os，返回类型满足拷贝构造函数的调用时机3

//注释：basic_ifstream( const basic_ifstream& rhs ) = delete; (7)	(since C++11)
//	   basic_ofstream( const basic_ofstream& rhs ) = delete; (7)	(since C++11)
//ifstram 和 ofstream 的拷贝构造函数已经从C++11开始删除了
std::ostream& operator<<(std::ostream& os, const Complex& rhs)
{
	cout << "std::ostream& operator<<(std::ostream& os, const Complex& rhs)" << endl;

	if (rhs._imag > 0)
	{
		os << rhs._real << " + " << rhs._imag << "i" << endl;
	}
	else if (rhs._imag == 0)
	{
		os << rhs._real << endl;
	}
	else
	{
		os << rhs._real << " - " << -rhs._imag << "i" << endl;
	}

	return os;
}

void readDouble(std::istream& is, double& rhs)
{
	while (is >> rhs, !is.eof())
	{
		if (is.bad())
		{
			std::cerr << "istream is bad" << endl;
			return;
		}
		else if (is.fail())
		{
			is.clear();//重置流的状态
			is.ignore(std::numeric_limits<::std::streamsize>::max(), '\n');//清空缓冲区
			cout << "请注意：需要输入double类型的数据！";
		}
		else
		{
			cout << "rhs = " << rhs << endl;
			break;
		}
	}
}

//输入流运算符重载
std::istream& operator>>(std::istream& is, Complex& rhs)//因为要修改rhs 的值，所以不能加const
{
	cout << "std::istream& operator>>(std::istream& is, Complex& rhs)" << endl;
	cout << "请分别输入复数的实部和虚部：" << endl;
	//is >> rhs._real >> rhs._imag;
	readDouble(is, rhs._real);
	readDouble(is, rhs._imag);
	return is;
}


void testOutputOperator()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;


	//cout << "c1 = " << c1.display();
	//为什么没有做输出流运算符重载之前上面的写法 不可行呢？ 解答：二元“<<”: 没有找到接受“void”类型的右操作数的运算符(或没有可接受的转换)

	cout << "c1 = " << c1 << endl;

	cout << endl;
	Complex c2;
	cin >> c2;
	cout << "c2 = " << c2 << endl;
}

int main(int argc, char* argv[])
{
	testOutputOperator();
	return 0;
}
```
---

### **4. 下标访问运算符 `operator[]`**
#### **4.1 关键知识点**
- `operator[]` 主要用于自定义数组类型，使得 `obj[idx]` 访问数组元素。
- 返回值应为 `T&`，以保证能够修改数组内容。
- 必须进行越界检查，以防止访问非法内存。

#### **4.2 代码**
```cpp
char& CharArrar::operator[](size_t idx) {
    if (idx < _size) {
        return _data[idx];
    } else {
        static char charNull = '\0';
        return charNull;
    }
}
```

---

### **5. 函数调用运算符 `operator()`**
#### **5.1 关键知识点**
- 使对象像函数一样调用，称为**仿函数（函数对象）**。
- 可存储状态，例如调用次数。

#### **5.2 代码**
```cpp
class FunctionObject {
public:
    int operator()(int x, int y) {
        ++_cnt;
        return x + y;
    }

    int operator()(int x, int y, int z) {
        ++_cnt;
        return x * y * z;
    }

private:
    int _cnt = 0;
};
```

#### **5.3 示例**
```cpp
FunctionObject fo;
cout << "fo(3, 4) = " << fo(3, 4) << endl;
cout << "fo(3, 4, 5) = " << fo(3, 4, 5) << endl;
```

#### **5.4 完整代码**

```cpp
//bracket.h
#pragma once
#include <iostream>
#include <string.h>

using namespace std;
class CharArrar
{
public:
	CharArrar(size_t sz = 10)
		:_size(sz)
		, _data(new char[_size])
	{
		cout << " CharArrar(size_t sz = 10)" << endl;
	}

	~CharArrar()
	{
		cout << "~CharArrar()" << endl;
		if (_data)
		{
			delete _data;
			_data = nullptr;
		}
	}

	size_t size() const
	{
		return _size;
	}

	//下标访问运算符的重载
	//int arr[10] = { 1,2,3,4,5 };
	//arr.operator[](idx);
	char& operator[](size_t idx);
private:
	size_t _size;
	char* _data;
};

```
---

```cpp
//bracket.cpp
#include "bracket.h"

/*
要修复错误 C2106: “=”: 左操作数必须为左值，需要确保 CharArrar 类的
下标运算符 operator[] 返回一个可修改的左值。当前的 operator[] 声明
返回的是一个 char，这不是一个可修改的左值。我们需要将其修改为返回一个 char&。
*/
char& CharArrar::operator[](size_t idx)
{
	if (idx < _size)
	{
		return _data[idx];
	}
	else
	{
		static char charNUll = '\0';//静态变量延长生命周期
		return charNUll;   //使得返回的实体生命周期比函数的生命周期长
	}
}

//C++中优势：重载的下标访问运算符考虑了越界的问题
//引用什么时候需要加上？
//1.如果返回类型是类型的时候，可以减少拷贝构造函数的执行
//2.有可能需要一个左值（来调用右操作数），而不是拷贝后的右值、
//3.cout << "111" << c1 << endl << 10 << 1 << endl,像这种情况下连续使用的时候可以加上引用 
```
---

```cpp
//parenthese.h
#pragma once
#include <iostream>

using namespace std;

class FunctionObject
{
public:
	int operator()(int x, int y);
	

	int operator()(int x, int y, int z);
	
private:
	int _cnt;//记录被调用的次数（函数对象状态）
};

```

---
```cpp
parenthese.cpp
#include <iostream>
#include "parenthese.h"

using namespace std;



int FunctionObject::operator()(int x, int y)
{
	++_cnt;
	cout << "int operator()(int x, int y)" << endl;
	return x + y;
}

int FunctionObject::operator()(int x, int y, int z)
{
	cout << "int operator()(int x, int y,int z)" << endl;
	++_cnt;
	return x * y * z;
}

```

---
```cpp
//main.cpp
#include <iostream>
#include "bracket.h"
#include "parenthese.h"


using namespace std;

int add(int x, int y)
{
	cout << "int add(int x,int y)" << endl;
	static int cnt = 0;
	++cnt;
	return x + y;
}


void testFunctionObject()
{
	FunctionObject fo;
	int a = 3;
	int b = 4;
	int c = 5;
	//fo本质上是一个对象，但是他的使用
	cout << "fo(a,b) = " << fo(a, b) << endl;
	cout << "fo(a, b ,c) = " << fo(a, b, c) << endl;

	cout << endl;
	//正经的函数
	cout << "add(a, b) = " << add(a, b) << endl;
}

void testCharArrar()
{
	
	//把字符串中的内容拷贝到CharArray
	const  char* pstr = "hello cpp";
	CharArrar ca(strlen(pstr) + 1);

	for(size_t idx = 0; idx != ca.size(); ++idx)
	{
		//ca[idx] = pstr[idx];
		//上面和下面两条代码等价
		ca.operator[](idx) = pstr[idx];
	}

	for (size_t idx = 0; idx != ca.size(); ++idx)
	{
		cout << ca[idx] << "  ";
	}
	cout << endl;
}

int main(int argc, char** argv)
{
	testFunctionObject();

	testCharArrar();
	return  0;
}
```

---

## **Day6-2 成员访问运算符重载（2025.03.25）**

## **1. 复习**
在上一节中，我们学习了 C++ 中的 **输入输出流运算符重载（`<<` 和 `>>`）** 以及 **下标运算符 `[]` 和函数调用运算符 `()`** 的重载。本节我们将重点学习 **成员访问运算符（`->` 和 `*`）的重载**。

---

## **2. 成员访问运算符重载**
C++ 允许用户自定义类的成员访问方式，其中 `->`（箭头运算符）和 `*`（解引用运算符）是最常见的运算符之一。它们通常用于模拟智能指针或多层指针访问。

### **2.1 箭头运算符 (`->`) 重载**
#### **(1) 语法**
```cpp
class A {
public:
    B* operator->();
};
```
**作用**：  
- 允许 `A` 类的对象 **像指针一样访问 `B` 类的成员**。
- 典型用途是在 **封装指针** 的类（如智能指针）中重载 `operator->`，使得用户无需手动解引用即可访问目标对象的成员。

**注意事项：**
1. **返回值必须是一个指针或者引用**，否则无法继续访问成员变量或函数。
2. **如果返回的是对象的引用**，则可以实现多层 `->` 重载。
3. `operator->` **不能改变调用对象自身**，因此通常 **不应该声明为 `const` 成员函数**。

---

### **2.2 解引用运算符 (`*`) 重载**
#### **(1) 语法**
```cpp
class A {
public:
    B& operator*();
};
```
**作用**：
- 允许 `A` 类的对象 **像指针一样进行解引用**。
- 常见于智能指针的实现，使 `*ptr` 直接返回对象的引用，方便访问其成员。

**注意事项：**
1. **返回值一般是引用**（如 `B&`），这样不会产生额外的拷贝。
2. 适用于 **封装指针** 的类，如智能指针或代理类。

---

## **3. 代码分析**

```cpp
#include <iostream> 
using namespace std;

class Data
{
public:
	Data(int data = 0)
		:_data(data)
	{
		cout << "Data(int data = 0)" << endl;
	}

	~Data()
	{
		cout << "~Data()" << endl; 
	}

	int getData() const
	{
		return _data;
	}
private:
	int _data;
};

class SecondLayer
{
public:
	SecondLayer(Data* pData)
	:_pData(pData)
	{
		cout << "SecondLayer(Data* pData)" << endl;
	}

	//重载 -> 运算符
	Data* operator->()
	{
		return _pData;
	}

	//解引用重载运算符
	Data& operator*()
	{
		return *_pData;
	}


	~SecondLayer()
	{
		cout << "~SecondLayer()" << endl;
		if (_pData)
		{
			delete _pData;
			_pData = nullptr;
		}
	}


private:
	Data* _pData;
};

class ThirdLayer
{
public:
	ThirdLayer(SecondLayer* pSecond)
		:_pSecond(pSecond)
	{
		cout << "ThirdLayer(SecondLayer* pSecond)" << endl;
	}

	//重载 -> 运算符
	SecondLayer& operator->() 
	{
		return *_pSecond;
	}

	~ThirdLayer()
	{
		cout << "~ThirdLayer()" << endl;
		if (_pSecond)
		{
			delete _pSecond;
			_pSecond = nullptr;
		}
	}

private:
	SecondLayer* _pSecond;
};


void test()
{
	/*Data* data = new Data(1);
	SecondLayer* second = new SecondLayer(data);
	ThirdLayer* third = new ThirdLayer(second);*/

	SecondLayer second(new Data(10));//栈对象
	//A类的对象调用B类的成员函数
	/*cout << "&second : " << &second << endl;
	cout << "second.operator->() :" << second.operator->() << endl;*/

	//  重载operator->  
	cout << "second.operator->()->getData() :" << second.operator->()->getData() << endl;
	cout << "second->getData() :" << second->getData() << endl;

	//  重载operator* 
	cout << "(*second).getData()" << (*second).getData() << endl;


	ThirdLayer third(new SecondLayer(new Data(30)));//栈对象
	cout << "third->getData() : " << third->getData() << endl;
	//还原
	cout << "third.operator->().operator->()->getData()" << third.operator->().operator->()->getData();
}

int main(int argc, char** argv)
{
	test();
	test();
	return 0;
}
```

### **3.1 代码结构**
上面的代码实现了一个 **三层封装** 的指针代理类，分别是：
- `Data`：数据类，包含一个 `_data` 成员变量。
- `SecondLayer`：封装 `Data*` 指针，并重载 `operator->` 和 `operator*`。
- `ThirdLayer`：封装 `SecondLayer*` 指针，并重载 `operator->`。

---

### **3.2 代码解析**
#### **(1) `Data` 类**
```cpp
class Data
{
public:
    Data(int data = 0) : _data(data)
    {
        cout << "Data(int data = 0)" << endl;
    }

    ~Data()
    {
        cout << "~Data()" << endl;
    }

    int getData() const
    {
        return _data;
    }
private:
    int _data;
};
```
- `Data` 类封装了一个 `int` 类型的数据 `_data`，提供了 `getData()` 方法用于获取数据值。
- 构造函数、析构函数用于跟踪对象的创建和销毁。

---

#### **(2) `SecondLayer` 类**
```cpp
class SecondLayer
{
public:
    SecondLayer(Data* pData) : _pData(pData)
    {
        cout << "SecondLayer(Data* pData)" << endl;
    }

    // 重载 -> 运算符
    Data* operator->()
    {
        return _pData;
    }

    // 解引用运算符 *
    Data& operator*()
    {
        return *_pData;
    }

    ~SecondLayer()
    {
        cout << "~SecondLayer()" << endl;
        if (_pData)
        {
            delete _pData;
            _pData = nullptr;
        }
    }

private:
    Data* _pData;
};
```
- **封装 `Data*` 指针，并提供访问 `Data` 成员的方式**。
- `operator->()` 返回 `_pData` 指针，使得 `SecondLayer` 对象 **可以像指针一样使用 `->` 访问 `Data` 的方法**。
- `operator*()` 返回 `_pData` 所指向的 `Data` 对象的引用，使 `*second` 直接返回 `Data` 对象。

**示例：**
```cpp
SecondLayer second(new Data(10));
cout << second->getData() << endl;  // 等价于 second.operator->()->getData()
cout << (*second).getData() << endl; // 等价于 second.operator*().getData()
```

---

#### **(3) `ThirdLayer` 类**
```cpp
class ThirdLayer
{
public:
    ThirdLayer(SecondLayer* pSecond) : _pSecond(pSecond)
    {
        cout << "ThirdLayer(SecondLayer* pSecond)" << endl;
    }

    // 重载 -> 运算符
    SecondLayer& operator->()
    {
        return *_pSecond;
    }

    ~ThirdLayer()
    {
        cout << "~ThirdLayer()" << endl;
        if (_pSecond)
        {
            delete _pSecond;
            _pSecond = nullptr;
        }
    }

private:
    SecondLayer* _pSecond;
};
```
- `ThirdLayer` 封装了 `SecondLayer*` 指针，并提供 `operator->()` 使其 **可以像 `SecondLayer` 一样使用 `->` 访问 `Data` 的方法**。
- **实现两层 `->` 重载**，使得 `ThirdLayer` 可以连续访问 `Data` 成员。

**示例：**
```cpp
ThirdLayer third(new SecondLayer(new Data(30)));
cout << third->getData() << endl;  // 等价于 third.operator->().operator->()->getData()
```

---

### **3.3 运行 `test()` 方法**
```cpp
void test()
{
    SecondLayer second(new Data(10));
    cout << "second->getData() :" << second->getData() << endl;
    cout << "(*second).getData()" << (*second).getData() << endl;

    ThirdLayer third(new SecondLayer(new Data(30)));
    cout << "third->getData() : " << third->getData() << endl;
    cout << "third.operator->().operator->()->getData() : " << third.operator->().operator->()->getData();
}
```
**输出：**
```
Data(int data = 0)
SecondLayer(Data* pData)
second->getData() : 10
(*second).getData() : 10
Data(int data = 0)
SecondLayer(Data* pData)
ThirdLayer(SecondLayer* pSecond)
third->getData() : 30
third.operator->().operator->()->getData() : 30
~ThirdLayer()
~SecondLayer()
~Data()
~SecondLayer()
~Data()
```
- `SecondLayer` 允许访问 `Data` 对象。
- `ThirdLayer` 允许访问 `SecondLayer`，最终可访问 `Data`。
- **多层指针访问的代理模式生效**，并且析构时正确释放了内存。

---

## **4. 总结**
| 运算符 | 作用 | 适用场景 | 返回值类型 |
|--------|------|---------|------------|
| `operator->()` | 允许对象像指针一样访问成员 | 智能指针、代理类 | 指针或引用 |
| `operator*()` | 允许对象像指针一样解引用 | 智能指针、代理类 | 引用 |

**关键点：**
1. **`operator->()` 需要返回指针或引用**，可以连续调用 `->`。
2. **`operator*()` 需要返回对象的引用**，避免拷贝，提高性能。
3. **适用于封装指针的类，如智能指针和代理类**。

本节学习了 **成员访问运算符 `->` 和 `*` 的重载**，掌握它们的用法可以更好地理解 **智能指针** 和 **代理模式**。

---

## **Day6-3 类型转换函数和类域**

### **1. 类型转换函数（Type Conversion Functions）**

#### **1.1 概述**
**类型转换函数**用于将类的对象转换为其他类型，满足以下特点：
- 是**类的成员函数**，不能是友元函数或非成员函数。
- **没有参数列表**（即必须是 `()` 空参数）。
- **没有返回类型声明**，即不能显式声明 `void` 或其他返回类型。
- **使用 `return` 语句返回目标类型的变量**。

#### **1.2 代码示例**
```cpp
#include <iostream>

using namespace std;

class Complex
{
    friend std::ostream& operator<<(std::ostream& os, const Complex& rhs);

public:
    Complex(int real = 0, int imag = 0)  
        : _real(real), _imag(imag)
    {
        cout << "Complex(int, int) 构造函数" << endl;
    }

    ~Complex()
    {
        cout << "~Complex 析构函数" << endl;
    }

    void display() const
    {
        cout << _real << " + " << _imag << "i" << endl;
    }

    int getReal() const { return _real; }
    int getImag() const { return _imag; }

private:
    int _real;
    int _imag;
};

// 重载输出运算符，使 Complex 可以被 cout 直接输出
std::ostream& operator<<(std::ostream& os, const Complex& rhs)
{
    os << rhs._real << " + " << rhs._imag << "i";
    return os;
}

class Point
{
    friend std::ostream& operator<<(std::ostream& os, const Point& rhs);

public:
    Point(int x = 0, int y = 0) : _x(x), _y(y)
    {
        cout << "Point(int, int) 构造函数" << endl;
    }

    // **类型转换函数**
    operator int() const
    {
        cout << "operator int()" << endl;
        return _x + _y;
    }

    operator double() const
    {
        cout << "operator double()" << endl;
        return _y == 0 ? 0.0 : static_cast<double>(_x) / _y;
    }

    operator Complex() const
    {
        cout << "operator Complex()" << endl;
        return Complex(_x, _y);
    }

    ~Point()
    {
        cout << "~Point 析构函数" << endl;
    }

    // **类型转换构造函数**
    explicit Point(const Complex& rhs)
        : _x(rhs.getReal()), _y(rhs.getImag())
    {
        cout << "Point(const Complex&) 类型转换构造函数" << endl;
    }

    void print() const
    {
        cout << "(" << _x << ", " << _y << ")" << endl;
    }

private:
    int _x;
    int _y;
};

// 重载输出运算符，使 Point 可以被 cout 直接输出
std::ostream& operator<<(std::ostream& os, const Point& rhs)
{
    os << "(" << rhs._x << ", " << rhs._y << ")";
    return os;
}

void testTypeConversion()
{
    Point pt(3, 4);
    cout << "Point pt: " << pt << endl;

    int intValue = pt;  
    cout << "转换为 int: " << intValue << endl;

    double doubleValue = pt;
    cout << "转换为 double: " << doubleValue << endl;

    Complex comValue = pt;
    cout << "转换为 Complex: " << comValue << endl;
}

int main()
{
    testTypeConversion();
    return 0;
}
```

#### **1.3 关键优化**
✅ **类型转换构造函数**增加 `explicit`，防止隐式转换导致潜在错误。  
✅ **类型转换函数**添加 `const`，保证 `Point` 的成员变量不会被修改。  
✅ **`operator double()`** 处理 `_y == 0` 情况，避免除零异常。  
✅ **优化 `Complex` 相关代码**，消除 `friend` 冗余，提高可读性。

---

### **2. 类域（Class Scope）**

#### **2.1 作用域 vs 可见域**
- **作用域（Scope）**：指变量在代码中的**可访问范围**，影响变量的解析方式。
- **可见域（Visibility）**：变量是否在当前上下文可见，受 `private`、`protected`、`public` 修饰符影响。
- **作用域 ≥ 可见域**，如果没有**命名冲突**，二者相等。

---

#### **2.2 代码示例**
```cpp
#include <iostream>

using namespace std;

int globalNumber = 50;  // **全局作用域变量**

namespace ceshi
{
    int namespaceNumber = 20;  // **命名空间作用域**

    class Test
    {
    public:
        Test(int value = 100) : number(value) {}

        void print(int number)
        {
            cout << "局部变量 number: " << number << endl;
            cout << "成员变量 number: " << this->number << endl;
            cout << "类作用域 number: " << Test::number << endl;
            cout << "命名空间 ceshi::number: " << ceshi::namespaceNumber << endl;
            cout << "全局变量 globalNumber: " << ::globalNumber << endl;
        }

    private:
        int number;  // **类成员变量**
    };
} // namespace ceshi

void testScope()
{
    int localValue = 3000;
    ceshi::Test obj;
    obj.print(localValue);
}

int main()
{
    testScope();
    return 0;
}
```

#### **2.3 关键优化**
✅ **清晰划分不同作用域**：全局作用域 `::`、命名空间 `ceshi::`、类作用域 `this->` 和 `Test::`。  
✅ **增强 `print()` 方法输出格式**，更直观理解变量的作用域。  
✅ **消除不必要的析构函数**，提升代码简洁性。  
✅ **代码格式调整**，增强可读性。

---

### **3. 内部类（Nested Class）**

**内部类**（Nested Class）是指在一个类的定义内部再定义一个类。内部类在逻辑上属于外部类的一部分，但具有自己的作用域。

#### **3.1 内部类的定义**
```cpp
#include <iostream>
using namespace std;

class Outer {
public:
    class Inner { // 内部类
    public:
        void display() {
            cout << "This is Inner class" << endl;
        }
    };
};

void test() {
    Outer::Inner obj; // 创建内部类对象
    obj.display();
}

int main() {
    test();
    return 0;
}
```
**特点：**
1. 内部类的作用域受外部类的限制。
2. 内部类的对象可以通过 `外部类::内部类` 的方式创建。
3. 内部类可以访问外部类的 `public` 和 `protected` 成员，但不能直接访问 `private` 成员（除非使用 `friend` 关键字）。

#### **3.2 访问外部类的 `private` 成员**
如果内部类需要访问外部类的 `private` 成员，可以将其声明为 `friend`。
```cpp
class Outer {
private:
    int _data = 10;

public:
    class Inner {
    public:
        void display(Outer& obj) {
            cout << "Outer data: " << obj._data << endl;
        }
    };
};

void test2() {
    Outer outerObj;
    Outer::Inner innerObj;
    innerObj.display(outerObj);
}
```

#### **3.3 静态内部类**
静态内部类不能访问外部类的非静态成员。
```cpp
class Outer {
public:
    static class StaticInner {
    public:
        void show() {
            cout << "This is a static inner class" << endl;
        }
    };
};

void test3() {
    Outer::StaticInner obj;
    obj.show();
}
```

---

### **4. 作用域运算符（Scope Resolution Operator `::`）**
作用域运算符 `::` 用于指定变量、函数或类所属的作用域，主要应用如下：

#### **4.1 访问全局变量**
```cpp
#include <iostream>
using namespace std;

int number = 100;

void test() {
    int number = 50;
    cout << "Local number: " << number << endl;
    cout << "Global number: " << ::number << endl;
}
```

#### **4.2 访问命名空间内的变量**
```cpp
namespace A {
    int value = 10;
}

namespace B {
    int value = 20;
}

void test2() {
    cout << "A::value = " << A::value << endl;
    cout << "B::value = " << B::value << endl;
}
```

#### **4.3 访问类的静态成员**
```cpp
class Demo {
public:
    static int data;
};

int Demo::data = 50;

void test3() {
    cout << "Static data: " << Demo::data << endl;
}
```

---

### **5. 类型转换运算符（Type Casting Operators）**
C++ 提供了四种类型转换运算符：
1. `static_cast`：用于基本类型之间的转换。
2. `dynamic_cast`：用于多态类型的安全转换。
3. `const_cast`：用于去除 `const` 或 `volatile` 修饰符。
4. `reinterpret_cast`：用于不同类型指针之间的转换。

#### **5.1 `static_cast` 用法**
```cpp
void test_static_cast() {
    double num = 10.5;
    int intNum = static_cast<int>(num);
    cout << "Converted value: " << intNum << endl;
}
```

#### **5.2 `dynamic_cast` 用法**
```cpp
class Base {
public:
    virtual void show() {}
};

class Derived : public Base {
public:
    void show() {
        cout << "Derived class" << endl;
    }
};

void test_dynamic_cast() {
    Base* basePtr = new Derived();
    Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);
    if (derivedPtr) {
        derivedPtr->show();
    }
    delete basePtr;
}
```

#### **5.3 `const_cast` 用法**
```cpp
void test_const_cast() {
    const int num = 42;
    int* ptr = const_cast<int*>(&num);
    *ptr = 10;
    cout << "Modified value: " << *ptr << endl;
}
```

#### **5.4 `reinterpret_cast` 用法**
```cpp
void test_reinterpret_cast() {
    int num = 65;
    char* ch = reinterpret_cast<char*>(&num);
    cout << "Reinterpreted value: " << *ch << endl;
}
```

---

### **6. 关键字 `explicit` 与类型转换**
默认情况下，C++ 允许构造函数进行隐式类型转换，但 `explicit` 关键字可以禁止这种行为。

#### **6.1 隐式转换**
```cpp
class Demo {
public:
    Demo(int x) { cout << "Demo(int)" << endl; }
};

void test_implicit_conversion() {
    Demo obj = 10; // 隐式调用 Demo(int)
}
```

#### **6.2 使用 `explicit` 禁止隐式转换**
```cpp
class Demo {
public:
    explicit Demo(int x) { cout << "Demo(int)" << endl; }
};

void test_explicit() {
    // Demo obj = 10; // 错误！隐式转换被禁止
    Demo obj(10);    // 正确
}
```

---

### **7. 总结**
1. **内部类** 是定义在另一个类内部的类，具有自己的作用域，并且可以是 `static`。
2. **作用域运算符 `::`** 用于访问不同作用域的变量或函数。
3. **类型转换运算符** 提供更安全、可控的转换方式。
4. **`explicit` 关键字** 可以防止构造函数的隐式转换。



## **Day7-1 智能指针雏形**

### **1. 独占语义和共享语义**

#### **1.1 概述**

### **2. std::string的实现**

#### **2.1 概述**

### **3. 移动语义**

#### **3.1 概述**
---

## **Day7-2 继承和多态**

### 一、继承

#### 1、继承的基础

面向对象的四大基本特征：抽象、封装、继承、多态。



动物：既有类。父类、基类

鸟：新类。子类、派生类

继承：从既有类产生新类的一个过程。



#### 2、继承的定义

```C++
class 子类(派生类) 
: public/protected/private 父类(基类)   //类派生列表
{
    //数据成员
    //成员函数
};
```



#### 3、派生类产生的过程

1、吸收基类的成员

2、改造基类的成员

3、添加自己新的成员

吸收、改造、新增。



#### 4、继承的局限

构造函数、析构函数、友元关系、基类的operator new/delete /=



#### 5、访问权限


#### 6、protected与private继承区别

1、protected的继承可以无限继承下去，但是private继承只能有一代

2、如果不写继承方式，默认继承方式是私有的。



### 二、派生类对象的构造



总结：**当创建派生类对象的时候**，**会调用派生类的构造函数**，为了完成从基类吸收过来的数据成员的初始化，会调用基类的构造函数，而如果基类没有显示定义构造函数，就会调用基类的无参构造函数，而如果基类有显示定义构造函数，那么也会默认调用基类的无参构造函数，完成初始化，如果不在派生类的构造函数的初始化列表中显示调用基类有参构造函数，就会报错。



总结：当创建派生类对象的时候，不管基类与派生类有没有显示写出构造函数，肯定默认会调用无参的，有时无参又没有，所以最好在派生类构造函数的初始化列表中，显示写出基类构造函数就可以。



派生类构造函数的功能：（执行顺序）

1、完成对象所占整块内存的开辟，由系统在调用构造函数时自动完成。
2、调用基类的构造函数完成基类成员的初始化。
3、若派生类中含对象成员、const成员或引用成员，则必须在初始化表中完成其初始化。
4、派生类构造函数体执行  



### 三、派生类对象的销毁

派生类对象销毁的时候，会执行派生类的析构函数，然后**基类析构函数会自动调用**。

析构函数执行顺序：

1、先调用派生类的析构函数
2、再调用派生类中成员对象的析构函数
3、最后调用普通基类的析构函数  


### 四、多继承（重要）

#### 1、多继承基础



#### 2、多继承的问题

##### 1、成员函数访问冲突  

解决方案：使用类名 + 作用域限定符

##### 2、数据成员的存储二义性

解决方案：虚拟继承。

## **Day7-3 基类与派生类深入**
## C++Day16

### 一、问题回顾

1、继承的形式？三种继承方式的特点？派生类生成过程？继承局限？

2、派生类对象的创建的特点？销毁的特征？

3、多继承的问题有哪几种？如何解决？



### 二、基类与派生类间的转换

**类型适应**：派生类对象可以适用于基类对象。一个类可以适用于另外一个类的所有场景。
![alt text](基类派生类之间的转换-1.png)

---

### **关于 `Base& ref = derived` 的推荐程度**
在C++中，`Base& ref = derived`（基类引用绑定到派生类对象）是一种 **推荐** 的写法，但具体是否适用取决于你的需求。下面详细分析它的优缺点以及适用场景。

---

## **1. `base = derived`（对象赋值，发生切片）**
```cpp
Base base = derived;  // 对象赋值，发生切片（slicing）
base.print();         // 调用 Base::print()
```
### **特点：**
- **对象切片（Object Slicing）**：`derived` 的派生类部分被丢弃，只保留 `Base` 部分，`base` 是一个全新的 `Base` 对象。
- **不推荐**：除非你明确只需要基类部分，否则通常应该避免这种写法，因为它丢失了派生类的信息。

---

## **2. `Base& ref = derived`（引用绑定，无切片）**
```cpp
Base& ref = derived;  // 引用绑定，无切片
ref.print();          // 如果 print() 是虚函数，调用 Derived::print()
```
### **特点：**
✅ **优点：**
1. **无对象切片**：`ref` 只是 `derived` 的基类部分的引用，不会复制数据，派生类信息仍然完整。
2. **支持多态**：如果 `Base::print()` 是 `virtual` 的，调用 `ref.print()` 会正确调用 `Derived::print()`（动态绑定）。
3. **更高效**：避免了不必要的拷贝（特别是当 `Base` 较大时）。

❌ **缺点：**
1. **只能访问基类成员**：通过 `ref` 无法直接访问 `Derived` 的特有方法（如 `derived.extra()`）。
2. **必须确保 `derived` 的生命周期**：如果 `derived` 被销毁，`ref` 会变成悬空引用（dangling reference），导致未定义行为（UB）。

---

## **3. 推荐程度**
| 写法 | 是否推荐 | 适用场景 |
|------|---------|---------|
| `Base base = derived;`（赋值） | ❌ 不推荐 | 除非明确只需要基类部分 |
| `Base& ref = derived;`（引用） | ✅ **推荐** | 需要多态、避免切片时 |
| `Base* ptr = &derived;`（指针） | ✅ **更推荐** | 更灵活，可以存储、传递 |

### **更推荐的替代方案：`Base*`（指针）**
```cpp
Base* ptr = &derived;  // 基类指针指向派生类对象
ptr->print();          // 多态调用 Derived::print()
```
**优点：**
- 比引用更灵活（可以设为 `nullptr`，可以修改指向的对象）。
- 常用于运行时多态（如工厂模式、容器存储不同派生类对象）。

---

## **4. 总结**
- **`Base& ref = derived` 是推荐的**，因为它避免了切片并支持多态，但必须确保 `derived` 的生命周期足够长。
- **`Base* ptr = &derived` 更推荐**，因为指针更灵活，适用于更多场景（如存储在不同容器中）。
- **`Base base = derived` 不推荐**，除非你明确只需要基类部分（但通常应该使用组合而非继承）。

### **最佳实践**
```cpp
// ✅ 推荐：指针方式（更灵活）
Base* ptr = &derived;
ptr->print();

// ✅ 也可以：引用方式（适用于局部作用域）
Base& ref = derived;
ref.print();

// ❌ 不推荐：赋值方式（切片问题）
Base base = derived;
base.print();
```

如果你需要在函数参数中传递派生类对象，通常使用 **`const Base&`**（避免拷贝）或 **`Base*`**（更灵活）。

---


### 三、派生类间的复制控制
```cpp
//DerivedCopyControl1.cpp
#include <iostream>
#include <string.h>

using namespace std;

class Base
{
	friend std::ostream& operator<<(std::ostream& os, const Base& rhs);
public:
	Base()
		:_pbase(nullptr)
	{
		cout << "Base()" << endl;
	}

	Base(const char* pbase)
		:_pbase(new char[strlen(pbase) + 1])
	{
		cout << "Base(const char* pbase)" << endl;
		strcpy_s(_pbase, strlen(pbase) + 1, pbase);
	}
	Base(const Base& rhs)
		:_pbase(new char[strlen(rhs._pbase) + 1]())
	{
		cout << "Base(const Base& rhs)" << endl;
		strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
	}
	Base& operator=(const Base& rhs)
	{
		cout << "Base& operator=(const Base& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pbase;
			_pbase = nullptr;
			_pbase = new char[strlen(rhs._pbase) + 1]();
			strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
		}
		return *this;
	}

	~Base()
	{
		cout << "~Base()" << endl;
		if (_pbase)
		{
			delete[] _pbase;
			_pbase = nullptr;
		}
	}

private:
	char* _pbase;
};

std::ostream& operator<<(std::ostream& os, const Base& rhs)
{
	if (rhs._pbase)
	{
		os << rhs._pbase;
	}
	return os;
}

class Derived : public Base
{
	friend std::ostream& operator<<(std::ostream& os, const Derived& rhs);
public:
	Derived(const char* pbase,const char* pDerived)
		:Base(pbase)
		,_pDerived(new char[strlen(pDerived) + 1])
	{
		cout << "Derived(const char* pbase,const char* pDerived)" << endl;
		strcpy_s(_pDerived, strlen(pDerived) + 1, pDerived);
	}

	Derived(const Derived& rhs)
		:_pDerived(new char[strlen(rhs._pDerived) + 1]())
	{
		cout << "Derived(const Derived& rhs)" << endl;
		strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
	}

	Derived& operator=(const Derived& rhs)
	{
		cout << "Derived& operator=(const Derived& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pDerived;
			_pDerived = nullptr;

			_pDerived = new char[strlen(rhs._pDerived) + 1]();
			strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
		}
		return *this;
	}
	~Derived()
	{
		cout << "~Derived()" << endl;
		if (_pDerived)
		{
			delete[] _pDerived;
			_pDerived = nullptr;
		}
	}

private:
	char* _pDerived;
};

std::ostream& operator<<(std::ostream& os, const Derived& rhs)
{
	//Base& ref = rhs;//error!将 "Base &" 类型的引用绑定到 "const Derived" 类型的初始值设定项时，限定符被丢弃
	const Base& ref = rhs;//正确写法
	os << ref << "," << rhs._pDerived;
	
	return os;
}

void test()
{
	Derived derived("hello","world");
	cout << "derived = " << derived << endl;

	cout << endl;
	//用一个已经存在的对象去初始化一个刚刚创建的对象
	Derived derived2(derived);
	cout << "derived = " << derived << endl;
	cout << "derived2 = " << derived2 << endl;

	cout << endl;
	Derived derived3("cpp", "difficult");
	cout << "derived3 = " << derived3 << endl;
	
	cout << endl;
	//两个派生类对象之间进行赋值
	derived3 = derived;
	cout << "derived = " << derived << endl;
	cout << "derived3 = " << derived3 << endl;
}

int main() 
{
	test();
	return 0;
}



```

---
**`DerivedCopyControl2.cpp`**版本的cpp文件是为了解决上一个版本(**`DerivedCopyControl1.cpp`**)的潜在问题，主要问题如下：
- Base的拷贝构造函数和赋值运算符未处理源对象的_pbase为nullptr的情况，导致潜在崩溃。
- Derived的拷贝构造函数未调用Base的拷贝构造函数，导致基类部分未被正确复制。
- Derived的赋值运算符未调用Base的赋值运算符，导致基类部分未被正确赋值。

```cpp
//DerivedCopyControl2.cpp
#include <iostream>
#include <string.h>

using namespace std;

class Base
{
	friend std::ostream& operator<<(std::ostream& os, const Base& rhs);
public:
	Base()
		:_pbase(nullptr)
	{
		cout << "Base()" << endl;
	}

	Base(const char* pbase)
		:_pbase(new char[strlen(pbase) + 1])
	{
		cout << "Base(const char* pbase)" << endl;
		strcpy_s(_pbase, strlen(pbase) + 1, pbase);
	}

	/*Base(const Base& rhs)
		:_pbase(new char[strlen(rhs._pbase) + 1]())
	{
		cout << "Base(const Base& rhs)" << endl;
		strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
	}*/

	// 拷贝构造函数
	Base(const Base& rhs) : _pbase(nullptr) {
		cout << "Base(const Base& rhs)" << endl;
		if (rhs._pbase) {
			_pbase = new char[strlen(rhs._pbase) + 1];
			strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
		}
	}

	/*Base& operator=(const Base& rhs)
	{
		cout << "Base& operator=(const Base& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pbase;
			_pbase = nullptr;
			_pbase = new char[strlen(rhs._pbase) + 1]();
			strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
		}
		return *this;
	}*/

	// 赋值运算符
	Base& operator=(const Base& rhs) {
		cout << "Base& operator=(const Base& rhs)" << endl;
		if (this != &rhs) {
			delete[] _pbase;
			_pbase = nullptr;
			if (rhs._pbase) {
				//base = new char[strlen(rhs._pbase) + 1]();
				//使用new char[size]()会进行零初始化，随后strcpy_s会覆盖这些零。此举虽无害但影响性能。
				_pbase = new char[strlen(rhs._pbase) + 1]; // 无需()
				strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
			}
		}
		return *this;
	}

	~Base()
	{
		cout << "~Base()" << endl;
		if (_pbase)
		{
			delete[] _pbase;
			_pbase = nullptr;
		}
	}

private:
	char* _pbase;
};

std::ostream& operator<<(std::ostream& os, const Base& rhs)
{
	if (rhs._pbase)
	{
		os << rhs._pbase;
	}
	return os;
}

class Derived : public Base
{
	friend std::ostream& operator<<(std::ostream& os, const Derived& rhs);
public:
	Derived(const char* pbase, const char* pDerived)
		:Base(pbase)
		, _pDerived(new char[strlen(pDerived) + 1])
	{
		cout << "Derived(const char* pbase,const char* pDerived)" << endl;
		strcpy_s(_pDerived, strlen(pDerived) + 1, pDerived);
	}

	
	/*Derived(const Derived& rhs)
		:_pDerived(new char[strlen(rhs._pDerived) + 1]())
	{
		cout << "Derived(const Derived& rhs)" << endl;
		strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
	}*/

	// 拷贝构造函数
	Derived(const Derived& rhs)
		: Base(rhs), // 调用基类拷贝构造
		_pDerived(new char[strlen(rhs._pDerived) + 1]) 
	{
		cout << "Derived(const Derived& rhs)" << endl;
		strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
	}

	

	/*Derived& operator=(const Derived& rhs)
	{
		cout << "Derived& operator=(const Derived& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pDerived;
			_pDerived = nullptr;

			_pDerived = new char[strlen(rhs._pDerived) + 1]();
			strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
		}
		return *this;
	}*/

	// 赋值运算符
	Derived& operator=(const Derived& rhs) {
		if (this != &rhs) {
			Base::operator=(rhs); // 调用基类赋值运算符
			delete[] _pDerived;
			_pDerived = new char[strlen(rhs._pDerived) + 1];
			strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
		}
		return *this;
	}

	~Derived()
	{
		cout << "~Derived()" << endl;
		if (_pDerived)
		{
			delete[] _pDerived;
			_pDerived = nullptr;
		}
	}

private:
	char* _pDerived;
};

std::ostream& operator<<(std::ostream& os, const Derived& rhs)
{
	//Base& ref = rhs;//error!将 "Base &" 类型的引用绑定到 "const Derived" 类型的初始值设定项时，限定符被丢弃
	const Base& ref = rhs;//正确写法
	os << ref << rhs._pDerived;

	return os;
}

void test()
{
	Derived derived("hello", "world");
	cout << "derived = " << derived << endl;

	cout << endl;
	//用一个已经存在的对象去初始化一个刚刚创建的对象
	Derived derived2(derived);
	cout << "derived = " << derived << endl;
	cout << "derived2 = " << derived2 << endl;

	cout << endl;
	Derived derived3("cpp", "difficult");
	cout << "derived3 = " << derived3 << endl;

	cout << endl;
	//两个派生类对象之间进行赋值
	derived3 = derived;
	cout << "derived = " << derived << endl;
	cout << "derived3 = " << derived3 << endl;

}

int main()
{
	test();
	return 0;
}



```
**总结：**
1、如果基类实现了拷贝构造函数或赋值运算符函数，但是派生类没有实现拷贝构造函数或赋值运算符函数，那么在将一个已经存在的派生类对象初始化一个刚刚创建的派生类对象，或者将两个派生类对象进行赋值，那么派生部分会执行缺省行为，而基类部分会执行基类的拷贝构造函数或者赋值运算符函数。

2、如果基类实现了拷贝构造函数或赋值运算符函数，并且派生类也实现拷贝构造函数或赋值运算符函数，那么在将一个已经存在的派生类对象初始化一个刚刚创建的派生类对象，或者将两个派生类对象进行赋值，那么派生部分会执行派生类自己的拷贝构造函数或者赋值运算符函数，而基类部分**不会自动**执行基类的拷贝构造函数或者赋值运算符函数，除非显示在派生类中调用基类的拷贝与赋值。


### 四、多态（重要）

#### 1、多态的基础

面向对象的四大基本特征：抽象、封装、继承、多态。

多态：对于同一种指令（警车鸣笛），针对不同对象（警察、普通人、嫌疑犯），产生不一样的行为（行动、正常行走、藏起来）。

#### 2、多态的分类

静态多态：包括：函数重载、运算符重载、模板，**发生的时机在编译的时候**。

动态多态：**发生的时机在运行的时候**，使用**虚函数**进行体现

```C++
int add(int x, int y)
{
    //
}

double add(double dx, double dy)
{
    
}

string add(string s1, string s2)
{
    
}

add(1, 2);
add("hello", "world");
```

#### 3、虚函数

概念：是一个**成员函数**，并且在前面加上**virtual**关键字

性质：如果在基类中定义类虚函数，那么在派生类中该函数就是虚函数，即使在派生类中没有加virtual

重定义（重写、覆盖）：派生类要保证该虚函数的名字与基类相同，函数的返回类型也要相同，函数的参数列表也要相同（包括参数的个数、参数的类型、参数的顺序），言外之意，唯一可以不一样的，就是函数体。



#### 4、虚函数原理（重要）

当基类定义了虚函数，就会在该类创建的对象的存储布局的前面，新增一个虚函数指针，该指针指向虚函数表（简称虚表），虚表中存的是虚函数的入口地址（有多少虚函数都会入虚表）。当派生类继承基类的时候，会满足吸收的特点，那么派生类也会有该虚函数，所以派生类创建的对象的布局的前面，也会新增一个虚函数指针，该指针指向派生类自己的虚函数表（简称虚表），虚表中存的是派生类的虚函数的入口地址（有多少虚函数都会入虚表），如果此时派生类重写了从基类这里吸收过来的虚函数，那么就会用派生类自己的虚函数的入口地址覆盖从基类这里吸收过来的虚函数的入口地址。


#### 5、虚函数机制被激活的条件（重要）

或者动态多态被激活的五个条件

1、基类定义虚函数

2、派生类重写（重定义、覆盖）该虚函数

3、创建派生类的对象

4、用基类的指针指向（引用绑定）派生类的对象

5、使用基类的指针（引用）调用该虚函数



#### 6、虚函数的限制

哪些函数不能被设置为虚函数？

1、普通函数（自由函数、全局函数）：虚函数必须是成员函数，而普通函数是非成员函数。

2、内联成员函数：内联成员函数进行函数替换的时候，发生时机在编译的时候，而虚函数要体现多态，发生时机在运行的时候；如果将内联函数设置为虚函数，那此时就会失去内联的含义。

3、静态成员函数：静态成员函数发生时机在编译的时候，而虚函数要体现多态，发生时机在运行的时候；静态成员函数是共享的，被该类的所有对象共享。没有this指针

4、友元函数：如果友元函数本身是一个普通函数，那么友元函数不能被设置为虚函数。如果友元函数本身是另外一个类的成员函数，是可以被设置为虚函数的。友元关系不能被继承。

5、构造函数：构造函数不能被继承，但是虚函数是可以被继承的；构造函数发生的时机在编译的时候，而虚函数要体现动态多态，发生的时机在运行的时候；**如果构造函数被设置为虚函数，那么要体现出多态，就需要放在虚表中，需要使用虚函数指针找到虚表，而如果构造函数都不调用，那对象是没有完全创建出来的，对象都不完整，此时有没有虚函数指针都不一定。（重要）**



#### 7、虚函数的访问

1、使用指针与引用可以体现出动态多态

2、使用对象不能体现动态多态，体现的是虚函数作为普通函数的特征。

3、使用其他普通成员函数有可能体现动态多态



#### 8、抽象类

抽象类是作为接口使用的。


---

是的，`int &ref = number;` 和 `Base &ref = derived;` 这两种引用声明方式在**语法形式上是相似的**，但在**语义和底层行为上有重要区别**。下面我们详细对比它们的异同：

---

## **1. 语法形式相似**
两种写法都遵循 C++ 引用的基本语法：
```cpp
T& ref = obj;  // T 是类型，obj 是已存在的对象
```
- `int& ref = number;`：`ref` 是 `int` 的引用，绑定到 `number`。
- `Base& ref = derived;`：`ref` 是 `Base` 的引用，绑定到 `derived`（派生类对象）。

---

## **2. 核心区别**
### **(1) 引用绑定的对象类型**
| 引用声明 | 绑定对象类型 | 是否允许 |
|----------|-------------|---------|
| `int& ref = number;` | 相同类型（`int` → `int`） | ✅ 允许 |
| `Base& ref = derived;` | 派生类 → 基类（`Derived` → `Base`） | ✅ 允许（多态） |
| `Derived& ref = base;` | 基类 → 派生类（`Base` → `Derived`） | ❌ 错误（除非基类实际是派生类） |

**关键点**：
- C++ 允许 **基类引用绑定到派生类对象**（向上转型，Upcasting），这是多态的基础。
- 但反过来（派生类引用绑定基类对象）是禁止的，除非通过 `dynamic_cast` 且基类具有多态性（含虚函数）。

---

### **(2) 可访问的成员**
| 引用类型 | 能访问的成员 |
|----------|-------------|
| `int& ref` | 可访问 `int` 的所有操作（如加减乘除） |
| `Base& ref` | 只能访问 `Base` 的成员，无法直接访问 `Derived` 的特有成员 |

**示例**：
```cpp
class Base {
public:
    void base_func() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    void derived_func() { cout << "Derived\n"; }
};

int main() {
    Derived derived;
    Base& ref = derived;

    ref.base_func();    // ✅ 允许
    // ref.derived_func(); // ❌ 错误：Base 没有 derived_func
}
```

---

### **(3) 多态行为（虚函数）**
如果 `Base` 有虚函数，且 `Derived` 覆盖了它，通过 `Base& ref` 调用时会触发动态绑定：
```cpp
class Base {
public:
    virtual void print() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    void print() override { cout << "Derived\n"; }
};

int main() {
    Derived derived;
    Base& ref = derived;
    ref.print(); // 输出 "Derived"（多态调用）
}
```
而 `int&` 不涉及多态，因为内置类型（如 `int`）没有虚函数。

---

## **3. 底层机制**
- `int& ref = number;`：
  - `ref` 直接绑定到 `number` 的内存地址，操作 `ref` 等同于操作 `number`。
- `Base& ref = derived;`：
  - `ref` 绑定到 `derived` 对象中的 **基类子对象部分**（即 `Base` 的部分）。
  - 如果 `Base` 有虚函数，`ref` 会通过虚表（vtable）实现多态。

---

## **4. 总结**
| 特性                | `int& ref = number;`           | `Base& ref = derived;`               |
|---------------------|--------------------------------|--------------------------------------|
| **绑定类型**         | 相同类型（`int` → `int`）       | 派生类 → 基类（`Derived` → `Base`）   |
| **是否支持多态**     | 否（`int` 无虚函数）            | 是（如果 `Base` 有虚函数）            |
| **可访问成员**       | 全部 `int` 操作                | 仅 `Base` 的成员                     |
| **底层行为**         | 直接别名                       | 绑定到派生类对象的基类部分            |

### **关键结论**
1. **语法形式相同**，但 `Base& ref = derived;` 利用了 C++ 的多态特性。
2. **推荐使用基类引用/指针绑定派生类对象**，以实现运行时多态（比直接赋值 `Base base = derived;` 更高效且安全）。
3. 如果不需要多态，优先使用 **值类型或模板**，而非继承体系。

---

# C++ 中创建线程并传入类类型的正确写法

在 C++ 中创建线程并传入类类型有几种正确的方式，主要取决于你想如何传递类实例以及如何使用它。以下是几种常见的正确写法：

## 1. 传递成员函数（使用对象指针）

```cpp
#include <iostream>
#include <thread>

class MyClass {
public:
    void memberFunction(int x) {
        std::cout << "Member function called with " << x 
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    MyClass obj;
    std::thread t(&MyClass::memberFunction, &obj, 10);
    t.join();
    return 0;
}
```

## 2. 传递 lambda 表达式捕获对象

```cpp
#include <iostream>
#include <thread>

class MyClass {
public:
    void operator()(int x) const {
        std::cout << "Functor called with " << x 
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    MyClass obj;
    std::thread t([&obj]() { obj(20); });
    t.join();
    return 0;
}
```

## 3. 传递临时对象（移动语义）

```cpp
#include <iostream>
#include <thread>
#include <memory>

class MyClass {
public:
    void operator()() const {
        std::cout << "Temporary object called"
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    std::thread t(MyClass());  // 注意：这里可能会被解析为函数声明
    // 更好的写法：
    // std::thread t{MyClass()};
    // 或
    // std::thread t((MyClass()));
    t.join();
    return 0;
}
```

## 4. 使用 std::ref 传递引用

```cpp
#include <iostream>
#include <thread>
#include <functional>

class MyClass {
public:
    void operator()() {
        std::cout << "Object called by reference"
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    MyClass obj;
    std::thread t(std::ref(obj));
    t.join();
    return 0;
}
```

## 5. 使用智能指针

```cpp
#include <iostream>
#include <thread>
#include <memory>

class MyClass {
public:
    void operator()() {
        std::cout << "Shared object called"
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    auto obj = std::make_shared<MyClass>();
    std::thread t([obj]() { (*obj)(); });
    t.join();
    return 0;
}
```

## 注意事项

1. 线程的生命周期必须长于或等于它所引用的对象
2. 如果需要共享对象，确保线程安全性（使用互斥锁等）
3. 避免悬垂引用（不要传递局部变量的引用给可能比它生命周期长的线程）
4. 考虑使用 join() 或 detach() 明确线程的结束方式

选择哪种方式取决于你的具体需求，如对象生命周期管理、是否需要修改对象状态等。


---
### 问题描述

我们有以下C++代码：

```cpp
#include <iostream>

class B {
public:
    B() {
        test();
    }

    virtual void test() {
        std::cout << "B\n" << std::endl;
    }
private:
};

class D : public B {
public:
    D() = default;
    virtual void test() override {
        std::cout << "D\n" << std::endl;
    }
private:
};

int main() {
    D d;
    return 0;
}
```

运行这段代码时，输出的是 `B` 而不是 `D`。这是为什么？

### 初步观察

首先，我们有一个基类 `B` 和一个派生类 `D`。`B` 的构造函数中调用了虚函数 `test()`，而 `D` 重写了这个虚函数。当我们创建一个 `D` 的对象时，`B` 的构造函数会被调用，然后在 `B` 的构造函数中调用了 `test()`。然而，输出的是 `B` 的 `test()` 实现，而不是 `D` 的。

### 理解对象构造过程

在C++中，对象的构造是从基类到派生类的顺序进行的。具体到这段代码：

1. 当创建 `D` 的对象 `d` 时，首先调用 `B` 的构造函数。
2. 在 `B` 的构造函数中，调用了 `test()`。
3. 之后，`D` 的构造函数被调用（这里 `D` 的构造函数是默认的，没有额外操作）。

### 虚函数在构造函数中的行为

关键在于：**在构造函数中调用虚函数时，虚函数的动态绑定（多态）行为不会发生**。也就是说，在 `B` 的构造函数中调用 `test()` 时，它调用的是 `B` 的 `test()`，而不是 `D` 的 `test()`。

这是因为：

- 当 `B` 的构造函数正在执行时，`D` 的部分还没有被构造。此时对象的 `D` 部分还不完整，因此不能安全地调用 `D` 的重写函数。
- C++ 的设计决定了在基类的构造函数中，虚函数调用被解析为当前类（即基类）的实现，而不是派生类的实现。这是为了避免在对象未完全构造时调用未初始化的派生类成员。

### C++标准的规定

根据C++标准（如ISO/IEC 14882:2017）：

> When a virtual function is called directly or indirectly from a constructor or from a destructor, including during the construction or destruction of the class’s non-static data members, and the object to which the call applies is the object under construction or destruction, the function called is the one defined in the constructor or destructor’s own class or in one of its bases, but not a function overriding it in a class derived from the constructor or destructor’s class, or overriding it in one of the other base classes of the most derived object.

简单来说，在构造函数或析构函数中调用虚函数时，调用的版本是当前构造函数或析构函数所在类的版本，而不是派生类的重写版本。

### 示例验证

让我们稍微修改代码，以更清楚地看到构造顺序：

```cpp
#include <iostream>

class B {
public:
    B() {
        std::cout << "B constructor\n";
        test();
    }

    virtual void test() {
        std::cout << "B::test\n" << std::endl;
    }
};

class D : public B {
public:
    D() {
        std::cout << "D constructor\n";
        test();
    }
    virtual void test() override {
        std::cout << "D::test\n" << std::endl;
    }
};

int main() {
    D d;
    return 0;
}
```

输出：

```
B constructor
B::test
D constructor
D::test
```

可以看到：

1. `B` 的构造函数先被调用，此时 `test()` 调用的是 `B::test()`。
2. 然后 `D` 的构造函数被调用，此时 `test()` 调用的是 `D::test()`。

### 为什么这样设计？

这种设计是为了保证对象构造的安全性。考虑以下情况：

```cpp
class B {
public:
    B() {
        test();  // 如果这里调用 D::test()，而 D 的成员还未初始化
    }
    virtual void test() {}
};

class D : public B {
    int x;
public:
    D() : x(42) {}
    virtual void test() override {
        std::cout << x;  // 如果 B::B() 中调用 D::test()，x 还未初始化
    }
};
```

如果在 `B` 的构造函数中调用 `D::test()`，而 `D` 的成员 `x` 还未初始化（因为 `D` 的构造函数还未执行），那么访问 `x` 将是未定义行为。因此，C++ 规定在构造函数中虚函数调用不进行动态绑定，而是使用当前类的实现。

### 类似行为：析构函数

类似的情况也适用于析构函数：

```cpp
class B {
public:
    virtual ~B() {
        test();  // 调用 B::test()，即使是在 ~D() 中
    }
    virtual void test() {
        std::cout << "B::test\n";
    }
};

class D : public B {
public:
    ~D() {
        // D 的部分已经被销毁
    }
    virtual void test() override {
        std::cout << "D::test\n";
    }
};

int main() {
    B* b = new D();
    delete b;  // 输出 B::test
    return 0;
}
```

在析构函数中，派生类的部分已经被销毁，因此虚函数调用也会被解析为当前类的实现，以避免访问已销毁的派生类成员。

### 解决方案

如果需要在构造时调用派生类的虚函数实现，可以考虑以下方法：

1. **不要在构造函数中调用虚函数**：这是最直接的解决方案。将需要在构造后进行的操作移到单独的初始化函数中，由用户显式调用。

   ```cpp
   class B {
   public:
       B() {}
       void init() { test(); }  // 构造后由用户调用
       virtual void test() { std::cout << "B::test\n"; }
   };

   class D : public B {
   public:
       virtual void test() override { std::cout << "D::test\n"; }
   };

   int main() {
       D d;
       d.init();  // 输出 D::test
       return 0;
   }
   ```

2. **使用工厂模式**：通过工厂函数创建对象，并在构造后调用虚函数。

   ```cpp
   class B {
   public:
       virtual void test() { std::cout << "B::test\n"; }
   };

   class D : public B {
   public:
       virtual void test() override { std::cout << "D::test\n"; }
   };

   template <typename T>
   T* createAndTest() {
       T* obj = new T();
       obj->test();  // 安全调用派生类的实现
       return obj;
   }

   int main() {
       D* d = createAndTest<D>();  // 输出 D::test
       delete d;
       return 0;
   }
   ```

### 总结

- 在构造函数中调用虚函数时，调用的版本是当前构造函数所在类的实现，而不是派生类的重写版本。
- 这是因为派生类的部分在基类构造函数执行时还未构造完成，调用派生类的虚函数可能导致未定义行为。
- 这种设计是C++语言为了保证对象构造的安全性而做出的规定。
- 如果需要多态行为，应避免在构造函数中调用虚函数，或通过其他方式（如初始化函数或工厂模式）实现。

### 最终答案

在 `D d;` 的构造过程中，`B` 的构造函数首先被调用。在 `B` 的构造函数中调用虚函数 `test()` 时，由于 `D` 的部分尚未构造完成，C++ 的规则规定此时虚函数的调用被静态绑定到当前类（即 `B`）的实现，而不是派生类 `D` 的重写版本。因此，输出的是 `B` 而不是 `D`。这是C++为了保证对象构造的安全性而设计的特性。

---


 🚀



### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

**加粗** 或者 __加粗__
*斜体* 或者 _斜体_
***加粗+斜体*** 或者 ___加粗+斜体___

- 项目 1
- 项目 2
  - 子项 2.1
  - 子项 2.2
* 另一种写法


1. 第一项
2. 第二项
3. 第三项



