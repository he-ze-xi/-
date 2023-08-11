﻿@[TOC](目录)

# 前言

`大学期间浅学过C++语言，最近工作之余想再细致学习下C++语言。`

---

![在这里插入图片描述](https://img-blog.csdnimg.cn/fcaa640c6b0046129a0e6818449b282e.jpeg#pic_center)

## 1.c++发展历史

> 1. 从80年代到1995年。这一阶段C++语言基本上是传统类型上的面向对象语言，并且凭借这接近c语言的效率，在工业界使用的开发语言中占据了相当大的份额。
> 2. 从1995年到2000年，这一阶段由于标准模板(STL)和后来的Boost程等程序库的出现，泛型程序设计在C++中占据了越来越多的比重。
> 3. 2000年至今，由于以Loki、MPL等程序库为代表产生式编程和模板元编程的出现，C++出现了发展历史上第一个新的高峰。
> 4. 当今主流程序设计语言中最复杂的一员。

## 2.g++介绍

> g++是一款类似于gcc的编译软件，用于编译C++的源代码，使用的方法类似于gcc。



## 3.命名空间

### 1.命名空间简介

> 1. 命名空间是用来防止大型的项目中出现重名的函数、变量或类。
> 2. 比如说，同一个项目的不同模块中出现同名函数或者全局变量，是不可避免发生的情况。而命名空间恰好能解决这个问题。

### 2.命名空间的声明

>  关键字namespace后指定空间名，大括号里进行各种声明。

```csharp
namespace 空间名
{
	//可以在此声明函数、变量、结构体
}
```

### 3.命名空间中函数的定义

> 在此命名空间中声明的函数，在定义时必须在函数名称前加上空间名::，以此来于全局或其他命名空间的函数进行区别，样式如下：

```csharp
返回值类型 空间名::函数名(参数列表)
{
	//函数体
}
```

### 4.命名空间的指定

3种方式：

> 1.  可以使用作用域运算符"::"来指定命名空间。

```csharp
iotek::func();//调用命名空间下的func()
```

> 2. 可以使用using关键字指定命名空间下部分声明

```csharp
using iotek::func;

func();//调用命名空间下的func()
```

> 3. 可以使用using关键字指定命名空间内的全部声明

```csharp
using namespace iotek;

func();//调用命名空间下的func()
```

## 4.输入输出

### 1.简介

>1. 由于C++兼容C语言，因此我们仍然可以使用scanf和printf进行输入和输出。然而，C++本身也有对标准输入输出进行封装，他们就是cin和cout。
>2. 在C++中更推荐使用cin和cout来进行标准输入和输出。
>3. 使用cin和cout的准备工作

```csharp
#include<iostream>
using namespace std;
```

### 2.使用cout进行输出

> 1. cout可以使用多种数据类型的输出，包括所有的基本数据类型，字符数组以及string类型。
> 2. 输出单个数据：

```csharp
cout<<要输出的数据;
```

> 3. 输出多个数据：

```csharp
cout<<要输出的数据<<要输出的数据;
```

>3. 换行输出：

```csharp
cout<<要输出的数据<<endl;//从此endl表示换行
```

### 3.使用cin进行输入

> 1. cin可以使用多种数据类型的输入，包括所有的基本数据类型，字符数组以及string类型。
> 2. 输入单个数据：

```csharp
cin>>要输入的数据;
```

> 3. 输入多个数据：

```csharp
cin>>要输入的对象>>要输入的对象;
```

## 5.面向过程和面向对象

> 1. 面向过程程序设计范型是使用较广泛的面向过程性语言，其主要特征是：程序由过程定义和过程调用组成（简单地说，过程就是程序执行某项操作的一段代码，函数就是最常用的过程）。
>
> 2. 面向对象程序的基本元素是对象，面向对象程序的主要结构特点是：
>
> * 程序一般由类的定义和类的使用两部分组成；
> * 程序中的一切操作都是通过向对象发送消息来实现的，对象接收到消息后，启动有关方法完成相应的操作。

> 对象的概念：
>
> 1. 从广义上讲，要在内存上一段有意义的区域就称之为对象。
> 2. 在C++中，对象一般是指在类中装载的实例，具有相关的成员变量和成员函数。类是抽象的概念，而对象是通过类实现的具体实例。
> 3. 比如说，学生是类，学生小明是对象。

## 6.构造函数和析构函数

> 1. 由于对象一定会在内存中占用一段时间，所以一定会有其生命周期。也就是说对象一定有申请内存空间和释放内存空间的步骤。
> 2. 构造函数是当对象申请内存空间之后自动调用的函数。
> 3. 析构函数是当对象的内存空间即将被销毁前自动调用大的函数。

> 说明：在以下情况中，当对象的生命周期结束时，析构函数会被自动调用：
>
> 1. 如果定义了一个全局对象，则在程序流程离开其作用域时，调用该全局对象的析构函数。
> 2. 如果一个对象定义在一个函数体内，则当这个函数被调用结束时，该对象应该被释放，析构函数被自动调用。
> 3. 若一个对象是使用new运算符创建的，在使用delete运算符释放它时，delete会自动调用析构函数。

> 代码举例：

```cpp
#include <iostream>
#include <string>

using namespace std;
#pragma warning(disable:4996)

class Student {
private:
	char* name;
	char* stu_no;
	float score;
public:
	Student(const char* name1, const char* stu_no1, float score1);
	~Student();
	void modify(float score1);
	void show();
};

Student::Student(const char* name1,const char* stu_no1, float score1)
{
	name = new char[strlen(name1) + 1];
	strcpy(name, name1);
	stu_no = new char[strlen(stu_no1) + 1];
	strcpy(stu_no, stu_no1);
	score = score1;
}

Student::~Student()
{
	delete[]name;
	delete[]stu_no;
}

void Student::modify(float score1)
{
	score = score1;
}

void Student::show()
{
	cout << "姓名: " << name << endl;
	cout << "学号: " << stu_no << endl;
	cout << "成绩：" << score << endl;
}

int main()
{
	Student stu("雪女","2020199012",99);
	stu.modify(100);
	stu.show();

	return 0;
}

```

> 输出结果：
> 姓名: 雪女 
> 学号: 2020199012
> 成绩：100

## 7.new和delete

### 1.简介

> 1. new和delete是C++中的两个关键字，主要用于在向堆中申请或者释放空间。
> 2. 和C语言中申请堆内存不同的是，new和delete在申请/释放内存的时候还会调用构造和析构函数。

### 2.new的使用方法

> 1. 使用new创建对象：
>    类名 *变量名=new 构造函数(参数列表);
> 2. 使用new创建对象数组：
>    类名 *变量名=new 类名[数组的大小];//此时会调用多次构造函数。

### 3.delete的使用方法

> 1. 使用delete销毁对象：
>    delete指向对象的指针；//此时会调用析构函数
> 2. 使用delete销毁对象数组：
>    delete []对象数组名；//此时会调用多次析构函数
> 3. 注意delete和delete[]的选择。

## 8.this指针

> 1. 当通过对象调用成员函数传递参数时，会额外将本身的地址做为参数传递进入函数。比如说，我们实际上调用成员函数如下：

```csharp
tom.Introduce();
```

> 实际上编译器认为的代码是：

```csharp
tom.Introduce(&tom);
```

> 2. 在函数体的内部可以通过this指针获取到编译器隐式传入的当前对象的地址，并访问对象的成员。例如：

```csharp
this->类成员;
```

## 9.标准库类型String

### 1.简介

> 1. 标准库类型string表示可变长的字符序列，使用string类型必须首先包含string头文件。
> 2. 建议在C++中使用string表示字符串，而不是使用C风格的字符串。

### 2.string类型的操作

>1. 使用string类型必须首先包含string头文件。作为标准库的一部分，string定义在命名空间中std中。

```csharp
#include<iostream>
using namespace std;
```

>2. 如何初始化类的对象由类本身决定的，一个类可以有很多种定义初始化的方式，只不过这些方式之间必须有所区别：

```csharp
string s1;//空的字符串
string s2(s1);//初始化说s2为s1的副本
string s3=s2;//初始化s3为s2的副本
string s4="value";//使用字符串常量初始化
```

### 3.String对象的操作

```csharp
s.empty();//s如果为空返回true，否则返回false;
s.size();//返回s中的字符个数;
s[n];//返回s中第n个字符的引用,从0开始;
s1+s2;//返回是s1和s2连接后的结果;
s1=s2;//用s2的副本代替s1中原来的字符;
s1==s2;//判断字符串是否相等;
```

## 10.类的静态成员

> 1. 在C++中，静态成员是属于整个类的属性或行为的，而不是属于某个对象的。

## 11.单例模式

### 1.简介

> 1. 单例模式是一种常用的软件设计模式，在他的核心结构中只包含一个被称为单例的特殊类。
> 2. 通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。

### 2.为什么使用单例模式

> 在应用系统开发中，我们常常有以下需求：
> 1.需要生成唯一序列的环境;
> 2.需要频繁实例化然后销毁的对象;
> 3.创建对象时耗时过多或者耗资源过多，但又经常用到的对象;
> 4.方便资源相互通信的环境;

> 实际案例:
> 多线程中网络资源初始化
> 回收站机制
> 任务管理器
> 应用程序日志管理

### 3. 实现步骤：

```csharp
1.构造函数私有化
2.提供一个全局的静态方法，访问唯一对象
3.类中定义一个静态指针，指向唯一对象
```

> 懒汉式代码：

```csharp
#include <iostream>
using namespace std;
//懒汉式
class SingleTon
{
    private:
        SingleTon();
    public:
        static SingleTon* m_singleTon;
        static SingleTon* GetInstance();
        void TestPrint();
};

//懒汉式并没有创建单例对象
SingleTon* SingleTon::m_singleTon = NULL;
int main()
{
    SingleTon* p1 = SingleTon::GetInstance();
    SingleTon* p2 = SingleTon::GetInstance();
    cout << "p1:" << hex << p1 << endl;
    cout << "p2:" << hex << p2 << endl;
    p1->TestPrint();
    p2->TestPrint();
    return 0;
}
SingleTon::SingleTon()
{
    m_singleTon = NULL;
    cout << "构造了对象....." << endl;
}
SingleTon* SingleTon::GetInstance()
{
    if (m_singleTon == NULL)
    {
        m_singleTon = new SingleTon;
    }
    return m_singleTon;
}
void SingleTon::TestPrint()
{
    cout << "测试调用....." << endl;
}
```

> 输出结果：

```csharp
构造了对象.....
p1:000002BBB1C672C0
p2:000002BBB1C672C0
测试调用.....
测试调用.....
```

> 饿汉式代码：

```csharp
#include <iostream>
using namespace std;
//懒汉式
class SingleTon
{
private:
    SingleTon();
public:
    static SingleTon* m_singleTon;
    static SingleTon* GetInstance();
    void TestPrint();
};
//饿汉式创建单例对象
SingleTon* SingleTon::m_singleTon = new SingleTon;
int main()
{
    SingleTon* p1 = SingleTon::GetInstance();
    SingleTon* p2 = SingleTon::GetInstance();
    cout << "p1:" << hex << p1 << endl;
    cout << "p2:" << hex << p2 << endl;
    p1->TestPrint();
    p2->TestPrint();
    return 0;
}
SingleTon::SingleTon()
{
    m_singleTon = NULL;
    cout << "构造了对象....." << endl;
}
SingleTon* SingleTon::GetInstance()
{
    return m_singleTon;
}
void SingleTon::TestPrint()
{
    cout << "测试调用....." << endl;
}
```

> 输出结果：

```csharp
构造了对象.....
p1:000002BBB1C672C0
p2:000002BBB1C672C0
测试调用.....
测试调用.....
```

### 4.优缺点

> 优点：
>
> 1. 在内存中只有一个对象，节省内存空间；
> 2. 避免频繁的创建销毁对象，可以提高性能；
> 3. 避免对共享资源的多重占用，简化访问；
> 4. 为整个系统提供一个全局访问点。

> 缺点：
>
> 1. 不适用于变化频繁的对象；
> 2. 如果实例化的对象长时间用，系统会认为该对象是垃圾而被回收，这可能会导致对象状态的丢失；

## 12.const关键字

### 1.const引用

> 1. 由const关键字修饰的引用，称之为对常量的引用，简称为常量引用。
> 2. 和普通引用不同的是，对于常量的引用不能被用作修改它所绑定的对象。
>    比如，如下代码中,r1无法修改ci的值：

```cpp
const int ci=1024;
const int &r1=ci;
```

> 3. 常量引用仅对于引用可参与的操作做出了限定，对于引用的对象本身是不是一个常量未作限定，即**const引用即可绑定到常量对象上，也可一绑定到非常量对象上。**
> 4. 尽可能在参数传递时使用const引用。

### 2.const成员函数

> 1. 当成员函数不会更改对象的任何成员变量时，可以将成员函数声明为const。

```cpp
```csharp
class 类名
{
public :
		返回类型 函数名(参数列表) const;
}
返回类型 类名::函数名(参数列表) const;
{
	//函数体
}
```

> 2. const修饰的对象和普通对象不同,他只能调用const修饰的成员函数。而普通对象可以调用任何成员函数。
> 3. 将const修饰成员函数的目的，是为了确认该成员函数可作用于const对象身上。

### 3.const_cast

> 1. 对于将常量对象转换成非常量对象的行为,一般称其为”去掉const性质“。一旦去掉了某个对象的const性质，编译器就不再组织我们对该对象进行写操作了。
> 2. 如果对象本身不是一个常量，使用强制类型转换获得写权限是合法的行为。然而如果对象是一个常量，再使用const_cast执行写操作就会产生未定义的后果。
> 3. 使用方法：

```cpp
const char *pc;
char *p=const_cast<char*>(pc);
char &r=const_cast<char&>(*pc);
```

### 4.const修饰符

> const可以与指针一起使用，它们的组合情况复杂，可归纳为3种：指向常量的指针、常指针和指向常量的常指针。

#### 1.指向常量的指针

> 指向常量的指针：一个指向常量的指针变量。

```bash
const char* pc = "abcd";
该方法不允许改变指针所指的变量，即
    pc[3] = ‘x';   是错误的，
但是，由于pc是一个指向常量的普通指针变量，不是常指针，因此可以改变pc所指的地址，例如
    pc = "ervfs";
该语句付给了指针另一个字符串的地址，改变了pc的值。

```

#### 2.常指针

> 常指针：将指针变量所指的地址声明为常量。

```bash
char* const pc = "abcd";
创建一个常指针，一个不能移动的固定指针，可更改内容，如
    pc[3] = 'x';
但不能改变地址，如
    pc = 'dsff';  不合法

```

#### 3.指向常量的常指针

> 指向常量的常指针：这个指针所指的地址不能改变，它所指向的地址中的内容也不能改变。

```bash
const char* const pc = "abcd";
内容和地址均不能改变

```

## 13.重载

### 1.函数重载

> 1. 编程中，重载是指函数名相同，函数的参数列表不同(包括参数个数和参数类型)，至于返回类型可同可不同。
> 2. 重载是可使相同函数、运算符等处理不同类型数据或接受不同个数的参数的一种方法。
> 3. 如果同一个作用域内的几个函数名字相同但形参列表不同，称之为重载函数。
> 4. 重载的函数接受的形参类型不一样，但是执行的操作非常类似。调用这些函数，编译器会根据传递的实参列表推断想要的函数是哪个。

### 2.运算符重载

> 1. 运算符重载是对已经有的运算符号赋予多重含义，使同一个运算符号作用于不同的数据类型会有不同的行为。

> 下面以”+“运算符号为例子，代码示例如下：

```cpp
#include<iostream>

using namespace std;

class Complex
{
private:
	double real, img;
public:
	Complex(double r = 0, double i = 0) :real(r), img(i)
	{

	}

	friend Complex operator+(Complex& a, Complex& b)
	{
		Complex temp;
		temp.real = a.real + b.real;
		temp.img = a.img + b.img;
		return temp;
	}

	void Display()
	{
		cout << real;
		if(img>0)
		{
			cout << "+";
		}

		if (img != 0)
		{
			cout << img << "i" << endl;
		}
	}
};

int main()
{
	Complex a(2.3, 4.6);
	Complex b(3.6, 2.8);
	Complex c;
	a.Display();
	b.Display();
	c = a + b;
	c.Display();

	c = operator+(a, b);
	c.Display();

	return 0;
}
```

运行结果：

```cpp
2.3+4.6i
3.6+2.8i
5.9+7.4i
5.9+7.4i

```

## 14.内联函数

> 在函数名前冠以关键字inline，该函数就被声明为内联函数。每当程序中出现对该函数的调用时，C++编译器使用函数体中的代码插入到调用该函数的语句之处，同时使用实参代替形参，以便在程序运行时不再进行函数调用。引入内联函数主要是为了消除调用函数时的系统开销，以提高运行速度。

> 说明：
>
> 1. 内联函数在第一次被调用之前必须进行完整的定义，否则编译器将无法知道应该插入什么代码。
> 2. 在内联函数体内一般不能含有复杂的控制语句，如for语句和switch语句等。
> 3. 使用内联函数是一种空间换时间的措施，若内联函数较长，较复杂且调用较为频繁时不建议使用。

```cpp
#include <iostream>
using namespace std;

inline double Circle(double r)
{
    double PI = 3.14;
    return PI * r * r;
}

int main()
{
    for (int i = 0;i <= 3;i++)
    {
        cout << "r=" << i << "area=" << Circle(i);
        cout << endl;
    }
    return 0;
}


```

## 15.带有默认参数值的函数

> 当进行函数调用时，编译器按从左到右的顺序将实参与形参结合，若未指定足够的实参，则编译器按顺序用函数原型中的默认值来补足所缺少的实参。

```cpp
void init(int x = 5, int y = 10);
init (100, 19);   // 100 ， 19
init(25);         // 25, 10
init();           // 5， 10

```

## 16.new和delete运算符

> 程序运行时，计算机的内存被分为4个区：程序代码区、全局数据区、堆和栈。
> 其中，堆可由用户分配和释放。
> C语言中使用函数malloc()和free()来进行动态内存管理。
> C++则提供了运算符new和delete来做同样的工作，而且后者比前者性能更优越，使用更灵活方便。

说明：

1. 用运算符new分配的空间，使用结束后应该用也只能用delete显式地释放，否则这部分空间将不能回收而变成死空间。
2. 在使用运算符new动态分配内存时，如果没有足够的内存满足分配要求，new将返回空指针（NULL）。
3. 使用运算符new可以为数组动态分配内存空间，这时需要在类型后面加上数组大小。

```cpp
指针变量名 = new 类型名[下标表达式];
int *p = new int[10];

```

> 释放动态分配的数组存储区时，可使用delete运算符。

```cpp
delete []指针变量名;
delete p;
```

4. new 可在为简单变量分配空间的同时，进行初始化

```cpp
指针变量名 = new 类型名(初值);
int *p;
p = new int(99);
···
delete p;
```

## 17.引用

> 变量的引用就是变量的别名，因此引用又称别名。

> 引用与其所代表的变量共享同一内存单元，系统并不为引用另外分配存储空间。实际上，编译系统使引用和其代表的变量具有相同的地址。

```cpp
#include <iostream>
using namespace std;
int main()
{
	int i = 10;
	int& j = i;
	cout << "i = " << i << " j = " << j << endl;
	cout << "i的地址为 " << &i << endl;
	cout << "j的地址为 " << &j << endl;
	return 0;
}
```

> 上面代码输出i和j的值相同，地址也相同。

## 18.向函数传递对象

### 1.使用对象作为函数参数

> 对象可以作为参数传递给函数，其方法与传递其他类型的数据相同。在向函数传递对象时，是通过“传值调用”的方法传递给函数的。因此，函数中对对象的任何修改均不影响调用该函数的对象（实参本身）。

### 2.使用对象指针作为函数参数

> 对象指针可以作为函数的参数，使用对象指针作为函数参数可以实现传值调用，即在函数调用时使实参对象和形参对象指针变量指向同一内存地址，在函数调用过程中，形参对象指针所指的对象值的改变也同样影响着实参对象的值。

### 3.使用对象引用作为函数参数

> 在实际中，使用对象引用作为函数参数非常普遍，大部分程序员喜欢使用对象引用替代对象指针作为函数参数。因为使用对象引用作为函数参数不但具有用对象指针做函数参数的优点，而且用对象引用作函数参数将更简单、更直接。

代码举例：

```cpp
#include <iostream>
using namespace std;

class Point {
public:
	int x;
	int y;
	Point(int x1, int y1) : x(x1), y(y1)  //成员初始化列表,将x1和x2的值赋给x和y
	{ 

	}
	int getDistance()
	{
		return x * x + y * y;
	}
};

void changePoint1(Point point)    //使用对象作为函数参数
{
	point.x += 1;
	point.y -= 1;
}

void changePoint2(Point* point)   //使用对象指针作为函数参数
{
	point->x += 1;
	point->y -= 1;
}

void changePoint3(Point& point)  //使用对象引用作为函数参数
{
	point.x += 1;
	point.y -= 1;
}


int main()
{
	Point point[3] = { Point(1, 1), Point(2, 2), Point(3, 3) };
	Point* p = point;
	changePoint1(*p);
	cout << "the distance is " << p[0].getDistance() << endl;


	p++;
	changePoint2(p);
	cout << "the distance is " << p->getDistance() << endl;


	changePoint3(point[2]);
	cout << "the distance is " << point[2].getDistance() << endl;

	return 0;
}

```

输出结果：

```cpp
the distance is 2
the distance is 10
the distance is 20
```

## 19.友元

> 类的主要特点之一是数据隐藏和封装，即类的私有成员（或保护成员）只能在类定义的范围内使用，也就是说私有成员只能通过它的成员函数来访问。但是，有时为了访问类的私有成员而需要在程序中多次调用成员函数，这样会因为频繁调用带来较大的时间和空间开销，从而降低程序的运行效率。为此，C++提供了友元来对私有或保护成员进行访问。友元包括友元函数和友元类。

### 1.友元函数

> 友元函数既可以是不属于任何类的非成员函数，也可以是另一个类的成员函数。友元函数不是当前类的成员函数，但它可以访问该类的所有成员，包括私有成员、保护成员和公有成员。

> 在类中声明友元函数时，需要在其函数名前加上关键字friend。此声明可以放在公有部分，也可以放在保护部分和私有部分。友元函数可以定义在类内部，也可以定义在类外部。

#### 1.将非成员函数声明为友元函数

```cpp
#include <iostream>
using namespace std;
class Score{
private:
	int mid_exam;
	int fin_exam;
public:
	Score(int m, int f);
	void showScore();
	friend int getScore(Score &ob);
};

Score::Score(int m, int f)
{
	mid_exam = m;
	fin_exam = f;
}

int getScore(Score &ob)
{
	return (int)(0.3 * ob.mid_exam + 0.7 * ob.fin_exam);
}

int main()
{
	Score score(98, 78);
	cout << "成绩为: " << getScore(score) << endl;

	return 0;
}

```

> 说明：
>
> 1.  友元函数虽然可以访问类对象的私有成员，但他毕竟不是成员函数。因此，在类的外部定义友元函数时，不必像成员函数那样，在函数名前加上“类名::”。
> 2.  因为友元函数不是类的成员，所以它不能直接访问对象的数据成员，也不能通过this指针访问对象的数据成员，它必须通过作为入口参数传递进来的对象名（或对象指针、对象引用）来访问该对象的数据成员。
> 3.  友元函数提供了不同类的成员函数之间、类的成员函数与一般函数之间进行数据共享的机制。尤其当一个函数需要访问多个类时，友元函数非常有用，普通的成员函数只能访问其所属的类，但是多个类的友元函数能够访问相关的所有类的数据。

> 例子：一个函数同时定义为两个类的友元函数：

```cpp
#include <iostream>
#include <string>
using namespace std;

class Score;    //对Score类的提前引用说明
class Student {
private:
	string name;
	int number;
public:
	Student(string na, int nu) {
		name = na;
		number = nu;
	}
	friend void show(Score& sc, Student& st);
};

class Score {
private:
	int mid_exam;
	int fin_exam;
public:
	Score(int m, int f) {
		mid_exam = m;
		fin_exam = f;
	}
	friend void show(Score& sc, Student& st);
};

void show(Score& sc, Student& st) {
	cout << "姓名：" << st.name << "  学号：" << st.number << endl;
	cout << "期中成绩：" << sc.mid_exam << "  期末成绩：" << sc.fin_exam << endl;
}

int main() {
	Score sc(89, 99);
	Student st("白", 12467);
	show(sc, st);

	return 0;
}

```

#### 2.将成员函数声明为友元函数

> 一个类的成员函数可以作为另一个类的友元，它是友元函数中的一种，称为友元成员函数。友元成员函数不仅可以访问自己所在类对象中的私有成员和公有成员，还可以访问friend声明语句所在类对象中的所有成员，这样能使两个类相互合作、协调工作，完成某一任务。

```cpp
#include <iostream>
#include <string>
using namespace std;

class Score;    //对Score类的提前引用说明
class Student{
private:
	string name;
	int number;
public:
	Student(string na, int nu) {
		name = na;
		number = nu;
	}
	void show(Score &sc);
};

class Score{
private:
	int mid_exam;
	int fin_exam;
public:
	Score(int m, int f) {
		mid_exam = m;
		fin_exam = f;
	}
	friend void Student::show(Score &sc);
};

void Student::show(Score &sc) {
	cout << "姓名：" << name << "  学号：" << number << endl;
	cout << "期中成绩：" << sc.mid_exam << "  期末成绩：" << sc.fin_exam << endl;
}

int main() {
	Score sc(89, 99);
	Student st("白", 12467);
	st.show(sc);

	return 0;
}

```

> 说明：
>
> 一个类的成员函数作为另一个类的友元函数时，必须先定义这个类。并且在声明友元函数时，需要加上成员函数所在类的类名；

### 2.友元类

可以将一个类声明为另一个类的友元。

```cpp
class Y{
    ···
};
class X{
    friend Y;    //声明类Y为类X的友元类
};

```

> 当一个类被说明为另一个类的友元类时，它所有的成员函数都成为另一个类的友元函数，这就意味着作为友元类中的所有成员函数都可以访问另一个类中的所有成员。

> 友元关系不具有交换性和传递性。

## 20.多态性与虚函数

> 多态性就是不同对象收到相同的消息时，产生不同的动作。这样，就可以用同样的接口访问不同功能的函数，从而实现“一个接口，多种方法”。

### 1.虚函数

> 虚函数的定义是在基类中进行的，它是在基类中需要定义为虚函数的成员函数的声明中冠以关键字virtual，从而提供一种接口界面。定义虚函数的方法如下：

```bash
virtual 返回类型 函数名(形参表) {
    函数体
}
```

> 在基类中的某个成员函数被声明为虚函数后，此虚函数就可以在一个或多个派生类中被重新定义。虚函数在派生类中重新定义时，其函数原型，包括返回类型、函数名、参数个数、参数类型的顺序，都必须与基类中的原型完全相同。

> 代码举例：

```cpp
#include<iostream>

using namespace std;

class Family
{
private:
	string flower;
public:
	Family(string name = "鲜花") :flower(name)
	{

	}
	string GetName()
	{
		return flower;
	}

	virtual void Like()
	{
		cout << "家人喜欢不同的花：" << endl;
	}
};

class  Mother:public Family
{
private:

public:
	Mother(string name = "月季") :Family(name) 
	{

	}
	void Like()
	{
		cout << "妈妈喜欢" << GetName() << endl;
	}
};

class  Daugther:public Family
{
public:
	Daugther(string name = "百合") :Family(name)
	{

	}
	void Like()
	{
		cout << "女儿喜欢" << GetName() << endl;
	}

private:

};

int main()
{
	Family* p;
	Family f;
	Mother mom;
	Daugther dau;
	p = &f;
	p->Like();
	p = &mom;
	p->Like();
	p = &dau;
	p->Like();
	
	return 0;
}
```

> 运行结果：

```bash
家人喜欢不同的花：
妈妈喜欢月季
女儿喜欢百合
```

### 2.虚析构函数

> 如果在主函数中用new运算符建立一个派生类的无名对象和定义一个基类的对象指针，并将无名对象的地址赋值给这个对象指针，当用delete运算符撤销无名对象时，系统只执行基类的析构函数，而不执行派生类的析构函数。

```bash
Base *p;
p = new Derived;
delete p;
-----------------
输出：调用基类Base的析构函数

```

> 原因是当撤销指针p所指的派生类的无名对象，而调用析构函数时，采用了静态连编方式，只调用了基类Base的析构函数。

> 如果希望程序执行动态连编方式，在用delete运算符撤销派生类的无名对象时，先调用派生类的析构函数，再调用基类的析构函数，可以将基类的析构函数声明为虚析构函数。一般格式为

```cpp
virtual ~类名(){
    ·····
}

```

> 虽然派生类的析构函数与基类的析构函数名字不相同，但是如果将基类的析构函数定义为虚函数，由该类所派生的所有派生类的析构函数也都自动成为虚函数。示例如下：

```cpp
#include<iostream>

using namespace std;

class Base
{
public:
	virtual ~Base()
	{
		cout << "调用基类Base的析构函数;" << endl;
	}
};


class Derived :public Base
{
public:
	~Derived()
	{
		cout << "调用派生类Derived的析构函数;" << endl;
	}
};

int main()
{
	Base* p;
	p = new Derived;
	delete p;
	return 0;
}
```

> 运行结果：

```cpp
调用派生类Derived的析构函数...
调用基类Base的析构函数...

```

### 3.纯虚函数

> 纯虚函数是在声明虚函数时被“初始化为0的函数”，声明纯虚函数的一般形式如下：

```bash
virtual 函数类型 函数名(参数表) = 0;
```

> 声明为纯虚函数后，基类中就不再给出程序的实现部分。纯虚函数的作用是在基类中为其派生类保留一个函数的名字，以便派生类根据需要重新定义。

### 4.抽象类

> 如果一个类至少有一个纯虚函数，那么就称该类为抽象类，对于抽象类的使用有以下几点规定：
>
> 1. 由于抽象类中至少包含一个没有定义功能的纯虚函数。因此，抽象类只能作为其他类的基类来使用，不能建立抽象类对象。
> 2. 不允许从具体类派生出抽象类。所谓具体类，就是不包含纯虚函数的普通类。 
> 3. 抽象类不能用作函数的参数类型、函数的返回类型或是显式转换的类型。
> 4. 可以声明指向抽象类的指针或引用，此指针可以指向它的派生类，进而实现多态性。
> 5. 如果派生类中没有定义纯虚函数的实现，而派生类中只是继承基类的纯虚函数，则这个派生类仍然是一个抽象类。如果派生类中给出了基类纯虚函数的实现，则该派生类就不再是抽象类了，它是一个可以建立对象的具体类了。

### 5.代码综合示例：利用多态计算面积

应用C++的多态性，计算三角形、矩形和圆的面积。

```cpp
#include<iostream>

using namespace std;

/// <summary>
/// 公共基类
/// </summary>
class Figure
{
protected:
	double x, y;
public:
	Figure(double a, double b) :x(a), y(b)
	{

	}
	virtual void GetArea()
	{
		cout << "没有为这个类定义面积计算" << endl;
	}
};

class Triangle :public Figure
{
public:
	Triangle(double a, double b) :Figure(a, b)
	{

	}
	/// <summary>
	/// 虚函数重定义
	/// </summary>
	void GetArea()
	{
		cout << "高度" << x << "高度" << y;
		cout << "面积是:" << x * y * 0.5 << endl;
	}
};

class Squre:public Figure
{
public:
	Squre(double a, double b) :Figure(a, b)
	{

	}
	void GetArea()
	{
		cout << "宽度" << x << "高度" << y;
		cout << "面积" << x * y << endl;
	}
};

class  Circle:public Figure
{
public:
	Circle(double a) :Figure(a, a)
	{
		
	 }
	void GetArea()
	{
		cout << "半径" << x;
		cout << "面积" << x * x * 3.14 << endl;
	}

};

int main()
{
	Figure* p;
	Triangle t(10, 6);
	Squre s(10, 6);
	Circle c(10);

	p = &t;
	p->GetArea();

	p = &s;
	p->GetArea();

	p = &c;
	p->GetArea();

	return 0;
}

```

运行结果：

```cpp
高度10高度6面积是:30
宽度10高度6面积60
半径10面积314
```

## 21.函数模板和类模板

> 利用模板机制可以显著减少冗余信息，能大幅度地节约程序代码，进一步提高面向对象程序的可重用性和可维护性。模板是实现代码重用机制的一种工具，它可以实现类型参数化，即把类型定义为参数，从而实现代码的重用，使得一段程序可以用于处理多种不同类型的对象，大幅度地提高程序设计的效率。

### 1.模板的概念

在程序设计中往往存在这样的现象：两个或多个函数的函数体完全相同，差别仅在与它们的参数类型不同。例如：

```cpp
int Max(int x, int y) {
    return x >= y ? x : y;
}

double Max(double x, double y) {
    return x >= y ? x : y;
}
```

> 能否为了方便，为上述这些函数只写出一套代码呢？可以的，使用模板就可以。

### 2.函数模板

> 所谓函数模板，实际上是建立一个通用函数，其函数返回类型和形参类型不具体指定，用一个虚拟的类型来代表，这个通用函数就称为函数模板。在调用函数时，系统会根据实参的类型（模板实参）来取代模板中的虚拟类型，从而实现不同函数的功能。

> 函数的声明格式如：

```cpp
template <typename 类型参数>
返回类型 函数名(模板形参表)
{
	函数体
}

也可以定义为以下形式:
template <class  类型参数>
返回类型 函数名(模板形参表)
{
	函数体
}
```

> 实际上，template是一个声明模板的关键字，它表示声明一个模板。类型参数（通常用C++标识符表示，如T、type等）实际上是一个虚拟的类型名，使用前并未指定它是哪一种具体的类型，但使用函数模板时，必须将类型实例化。类型参数前需加关键字typename或class，typename和class的作用相同，都是表示一个虚拟的类型名（即类型参数）。

> 代码举例1：一个与指针有关的函数模板。

```cpp
#include<iostream>
using namespace std;

template <typename T>
T Max(T *array,int size=0)
{
    T max=array[0];
    for(int i=1;i<size;i++)
    {
        if(array[i]>max)
        {
            max=array[i];
        }
    }
    return max;
}

int main()
{
    int array_int[]={783,78,234,34,90,1};
    double array_double[]={99.02,21.9,23.90,12.89,1.09,34.0};
    int imax=Max(array_int,6);
    double  dmax=Max(array_double,6);
    cout<<"整数的最大值是:"<<imax<<endl;
    cout<<"小数的最大值是:"<<dmax<<endl;
    return 0;
}

```

> 代码举例2：函数模板的重载

```cpp
#include <iostream>
using namespace std;

 template <typename Type>
 Type Max(Type x,Type y){
     return x>y?x:y;
 }

 template <typename Type>
 Type Max(Type x,Type y,Type z){
     Type t=x>y?x:y;
     t=t>z?t:z;
     return t;
 }

 int main(){
     cout<<"33,36中最大值为:"<<Max(33,66)<<endl;
     cout<<"33,66,44中最大值为:"<<Max(33,66,44)<<endl;
     return 0;
 }
```

> 说明：
>
> 1. 在函数模板中允许使用多个类型参数。但是，应当注意template定义部分的每个类型参数前必须有关键字typename或class。
> 2. 在template语句与函数模板定义语句之间不允许插入别的语句。
> 3. 同一般函数一样，函数模板也可以重载。
> 4. 函数模板与同名的非模板函数可以重载。在这种情况下，调用的顺序是：首先寻找一个参数完全匹配的非模板函数，如果找到了就调用它；若没有找到，则寻找函数模板，将其实例化，产生一个匹配的模板参数，若找到了，就调用它。

### 3.类模板

> 所谓类模板，实际上就是建立一个通用类，其数据成员、成员函数的返回类型和形参类型不具体指定，用一个虚拟的类型来代表。使用类模板定义对象时，系统会根据实参的类型来取代类模板中虚拟类型，从而实现不同类的功能。

```cpp
template <typename T>
class  Three{
private:
    T x,y,z;
public:
    Three(T a,T b,T c){
        x=a;
        y=b;
        z=c;
    }
    T sum(){
        return  x+y+z;
    }
};
```

> 栈类模板的使用：

```cpp
#include<iostream>
#include<string>
using namespace std;

const int Size=10;

template <class T>
class Stack{
private:
    T stack[Size];
    int top;
public:
    void init(){
        top=0;
    }
    void push(T t);
    T pop();
};

template <class T>
void  Stack<T>::push(T t) {
    if(top==Size){
        cout<<"Stack is full!"<<endl;
        return;
    }
    stack[top++]=t;
}

template <class T>
T Stack<T>::pop(){
    if(top==0){
        cout<<"Stack is empty!"<<endl;
        return 0;
    }
    return stack[--top];
}

int main(){
    Stack<string> st;
    st.init();
    st.push("aaa");
    st.push("bbb");
    cout<<st.pop()<<endl;
    cout<<st.pop()<<endl;

    return 0;
}

```

> 运行结果：

```cpp
bbb
aaa
```

## 22.文件操作

### 1.文件的输入或输出

> 所谓文件，一般指存放在外部介质上的数据的集合。
> 文件流是以外存文件为输入/输出对象的数据流。输出文件流是从内存流向外存文件的数据，输入文件流是从外存流向内存的数据。
> 根据文件中数据的组织形式，文件可分为两类：文本文件和二进制文件。
>
> 在C++中进行文件操作的一般步骤如下：
>
> 1. 为要进行操作的文件定义一个流对象。
> 2. 建立（或打开）文件。如果文件不存在，则建立该文件。如果磁盘上已存在该文件，则打开它。
> 3. 进行读写操作。在建立（或打开）的文件基础上执行所要求的输入/输出操作。
> 4. 关闭文件。当完成输入/输出操作时，应把已打开的文件关闭。

### 2.文件的打开与关闭

> 为了执行文件的输入/输出，C++提供了3个文件流类。


> 这3个文件流类都定义在头文件fstream中。 要执行文件的输入/输出，须完成以下几件工作：
>
> 1. 在程序中包含头文件fstream。
> 2. 建立流对象。
> 3. 使用成员函数open()打开文件。
> 4. 进行读写操作。
> 5. 使用close()函数将打开的文件关闭。

### 3.文本文件的读/写

> 1. ofstream  文件写操作，内存写入存储设备。
> 2. ifstream  文件读操作，存储设备读取到内存中。
> 3. fstream   读写操作，对打开的文件可进行读写操作。

> 文件打开模式：
>
>       1.  ios::in   只读
>               2. ios::out  只写
>               3. ios::app  从文件末尾开始写，防止丢失文本中原有的内容，追加模式
>               4. ios::binary 二进制模式
>               5. ios::nocreate 打开一个文件时，如果文件不存在，不创建
>               6. ios::noreplace 打开一个文件时，如果文件不存在，创建该文件
>               7. ios::trunc   打开一个文件时，然后清空内容
>               8. ios::ate     打开一个文件时，将位置移动到文件末尾

> 文件指针位置的C++中的用法：
>     1. ios::beg   文件开头
>     2. ios::end   文件末尾
>     3. ios::cur   文件当前位置
>     举个例子：
>         1. file.seekg(0, ios::beg)  让文件指针定位到文件开头
>        2.  file.seekg(0, ios::end)  让文件指针定位到文件末尾
>         3. file.seekg(10, ios::cur) 让文件指针从当前位置向文件末尾方向移动10个字节
>         4. file.seekg(-10, ios::cur) 让文件指针从当前位置向文件开始方向移动10个字节
>         5. file.seekg(10,ios::beg)   让文件指针定位到离文件开头10个字节的位置

> 常用的错误判断方法:
>     1. good()   如果文件打开成功
>     2. bad()    打开文件时发生错误
>     3. eof()    到达文件尾
>
>
> getline()函数的作用是从输入字节流中读入字符，存到string变量中，直到遇到下面的情况停止：
>     1. 读入了文件结束标志。
>     2. 读到一个新行。
>
>       3.  达到字符串的最大穿长度。
>
> 说明：如果getline没有读入字符，将返回false，用于判断文件是否结束。

> 代码示例：读取hello.txt文件中的字符串，写入out.txt中。

```cpp
#include "iostream"
#include "fstream"
using namespace std;

int main(){
    ifstream infile("E:\\C++\\cpp_Code\\hello.txt");  // 读操作
    ofstream outfile("E:\\C++\\cpp_Code\\out.txt");   // 写操作
    string temp;
    if(! infile.is_open()){
        cout << "打开文件失败" << endl;
    }
    while(getline(infile, temp)){
        outfile << temp;
        outfile << endl;
    }
    infile.close();
    outfile.close();
    cout<<"完成处理操作！";
    return 0;
}
```

### 4.检测文件结束

> 在文件结束的地方有一个标志位，即为EOF。采用文件流方式读取文件时，使用成员函数eof()可以检测到这个结束符。如果该函数的返回值非零，表示到达文件尾。返回值为零表示未达到文件尾。该函数的原型是：

```cpp
int eof();
函数eof()的用法示例如下：
ifstream ifs;
···
if (!ifs.eof())   //尚未到达文件尾
    ···
还有一个检测方法就是检查该流对象是否为零，为零表示文件结束。
ifstream ifs;
···
if (!ifs)
    ···
如下例子：
while (cin.get(ch))
    cut.put(ch);
这是一个很通用的方法，就是检测文件流对象的某些成员函数的返回值是否为0，为0表示该流（亦即对应的文件）到达了末尾。

```

## 23.异常处理

> 程序中常见的错位分为两大类：编译时错误和运行时错误。
> 编译时的错误主要是语法错误，如关键字拼写错误、语句末尾缺分号、括号不匹配等。
> 运行时出现的错误统称为异常，对异常的处理称为异常处理。

> C++处理异常的办法：如果在执行一个函数的过程中出现异常，可以不在本函数中立即处理，而是发出一个信息，传给它的上一级（即调用函数）来解决，如果上一级函数也不能处理，就再传给其上一级，由其上一级处理。如此逐级上传，如果到最高一级还无法处理，运行系统一般会自动调用系统函数terminate()，由它调用abort终止程序。

> 代码举例：
> 输入三角形的三条边长，求三角形的面积。当输入边的长度小于0时，或者当三条边都大于0时但不能构成三角形时，分别抛出异常，结束程序运行。

```cpp
#include "iostream"
#include "cmath"
using  namespace std;

double Triangle(double a,double b,double c){
    double  s=(a+b+c)/2;
    if(a<=0||b<=0||c<=0||a+b<=c||a+c<=b||b+c<=a){
        throw  1.0;//语句throw抛出int异常
    }
    return sqrt(s*(s-a)*(s-b)*(s-c));
};

int main(){
    double  a,b,c;
    try {
        cout<<"请输入三角形的三个边长(a,b,c)"<<endl;
        cin>>a>>b>>c;
        if(a<0||b<0||c<0){
            throw  1;//语句throw抛出int异常
        }
        if(a>0 && b>0 && c>0){
            cout<<"a="<<a<<"b="<<b<<"c="<<c<<endl;
            cout<<"三角形的面积:"<<Triangle(a,b,c)<<endl;
        }
    }catch(double ){
        cout<<"这3条边不能构成三角形"<<endl;
    }catch (int){
        cout<<"边长小于或等于0"<<endl;
    }

    return 0;
}

```

> 运行结果：

```cpp
请输入三角形的三个边长(a,b,c)
1 1 2
a=1b=1c=2
三角形的面积:这3条边不能构成三角形
```

## 24.STL标准模板库

> 标准模板库（Standard Template Library）中包含了很多实用的组件，利用这些组件，程序员编程方便而高效。

### 1.Vector

> vector容器与数组类似，包含一组地址连续的存储单元。对vector容器可以进行很多操作，包括查询、插入、删除等常见操作。

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
	vector<int> nums;
	nums.insert(nums.begin(), 99);
	nums.insert(nums.begin(), 34);
	nums.insert(nums.end(), 1000);
	nums.push_back(669);

	cout << "\n当前nums中元素为: " << endl;
	for (int i = 0; i < nums.size(); i++)
		cout << nums[i] << " " ;

	cout << nums.at(2);
	nums.erase(nums.begin());
	nums.pop_back();

	cout << "\n当前nums中元素为: " << endl;
	for (int i = 0; i < nums.size(); i++)
		cout << nums[i] << " " ;

	return 0;
}

```

### 2.list容器

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
	list<int> number;
	list<int>::iterator niter;
	number.push_back(123);
	number.push_back(234);
	number.push_back(345);

	cout << "链表内容：" << endl;
	for (niter = number.begin(); niter != number.end(); ++niter)
		cout << *niter << endl;
	number.reverse();
	cout << "逆转后的链表内容：" << endl;
	for (niter = number.begin(); niter != number.end(); ++niter)
		cout << *niter << endl;
	number.reverse();

	return 0;
}

```

### 3.stack

利用栈进行进制转换：

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
	stack<int> st;
	int num = 100;
	cout << "100的八进制表示为：";
	while (num) {
		st.push(num % 8);
		num /= 8;
	}
	int t;
	while (!st.empty()) {
		t = st.top();
		cout << t;
		st.pop();
	}
	cout << endl;

	return 0;
}

```

### 4.队列

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
	queue<int> qu;
	for (int i = 0; i < 10; i++) 
		qu.push(i * 3 + i);
	while (!qu.empty()) {
		cout << qu.front() << " ";
		qu.pop();
	}
	cout << endl;

	return 0;
}

```

