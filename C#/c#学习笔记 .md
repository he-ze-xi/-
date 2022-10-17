@[TOC](目录)
# 记录在学习以及工作中遇到的C#知识点，并全部总结至此笔记中
### 语句块
![在这里插入图片描述](https://img-blog.csdnimg.cn/e11bbe262854404d97393c63166b8413.png#pic_center)
### Write和WriteLine的区别：
（1）Write没有在字符串后面添加换行符，而WriteLine则在每个字符串后面添加了换行符。如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2763bc17077e408e8c13f2f04c20f797.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2160d820b9af47008aee71f51bc79318.png#pic_center)
（2）从命令行读取和输入的例子：

```csharp
using System;
using System.Text;
namespace APP
{
    class Program
    {
        static void Main()
        {
            Console.WriteLine("请输入您的姓名：");
            string name = Console.ReadLine();
            Console.WriteLine("请输入您的年龄：");
            int age =int.Parse(Console.ReadLine());//通过int.parse方法可以将字符串类型转化为int类型
            Console.WriteLine("您好，{0}岁的{1}", age, name);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ed60b063ce84a4d8c1c760e5e28f7f7.png#pic_center)
### params
params是一个计算机函数，表示函数的参数是可变个数的，即可变的方法参数，用于表示类型相同，但参数数量不确定。
C#开发语言中 params 是关键字，**params主要的用处是在给函数传参数的时候用，就是当函数的参数不固定的时候。 在函数的参数数目可变而执行的代码差异很小的时候很有用！C#语法规定，params后边必定跟数组。作用是把不定数量的、同类型的参数装入这个数组中.**
### 托管代码和非托管代码
（1）托管代码：
运行在CLR(CLR是一个通用语言架构，它定义了一个代码运行的环境）下的代码就是托管代码，诸如C#、VB.NET 写的代码都会先编译成MSIL（MS中间代码），并运行在CLR的子集CLI(Common Language Infrastructure）中，最终根据不同的平台使用JIT（just in Time）编译成机器代码。
与Java机制不同在于Java是经过一次编译和一次解释运行，C#是经过两次编译运行,这两个阶段分别为：源代码编译为托管代码，托管代码编译为微软平台的专用语言，又称机器语言。
（2）非托管代码：
非托管的代码也叫本地代码（native），是由操作系统管理的。
高级语言编写的程序必须经过一定的步骤编译为机器语言才能被机器理解和运行。
这一系列步骤为：预处理、编译、汇编、链接。
（3）托管代码和非托管代码的区别：
1、托管代码是一种中间语言，运行在CLR上；非托管代码被编译为机器码，运行在机器上。
2、托管代码独立于平台和语言，能更好的实现不同语言平台之间的兼容；非托管代码依赖于平台和语言。
3、托管代码可享受CLR提供的服务（如安全检测、垃圾回收等），不需要自己完成这些操作；非托管代码需要自己提供安全检测、垃圾回收等操作。
### DllImport的使用：
托管代码生成的DLL文件，可以在VS中直接通过添加引用的方式使用。
非托管代码生成的DLL文件，比如使用C++编写的代码编译生成的DLL，不能在VS中直接引用，可以通过DllImport方法来使用。
### WriteLine、ReadLine和ReadKey：
console.ReadLine()是等待输入，并按回车继续。
console.WriteLine()是向控制台窗口产生输出。
Console.ReadKey()是等待键盘输入，退出程序。使调试时能看到输出结果。如果没有此句，命令窗口会一闪而过。
### C#中访问修饰符
在C#语言中，共有五种访问修饰符：public、private、protected、internal、protected internal。
（1）作用范围：
![在这里插入图片描述](https://img-blog.csdnimg.cn/5cbf00d5417640f39f6864cadf35f49a.png#pic_center)
（2）在c#中，
1. 类、结构的默认修饰符是internal。
2. 类中所有的成员默认修饰符是private。
3. 接口默认修饰符是internal。
4. 接口的成员默认修饰符是public。
5. 命名空间、枚举类型成员默认修饰符是public。
6. 委托的默认修饰符是internal。
### 类型的实例化
从某一个类型模板创建实际的对象，就称为实例化该类型。
### 成员可以分为两种：数据成员和函数成员
数据成员：保存了这个类的对象或作为一个整体的类相关的数据；
函数成员：执行代码，函数成员定义类型的行为。
### 枚举enum和结构struct的区别：
enum与struct的区别是：枚举里的变量都是整型的同类型，而struct是由不同类型
的变量所组成的，默认情况下，枚举的第一个值为0，后面每个连续的元素值递增1。
### 运行中的程序使用两个内存区域来存储数据：栈和堆。
### 栈存储几种类型的数据：
某些类型变量的值；
程序当前的执行环境；
传递给方法的参数。
### 与栈不同，堆里的内存能够以任意顺序存入和移除.
### 堆和栈：
栈是一个内存数组，是一个LIFO（Last-In First-Out，后进先出）的数据结构。
堆和栈的区别主要有五大点，分别是：
（1）申请方式的不同。栈由系统自动分配，而堆是人为申请开辟;
（2）申请大小的不同。栈获得的空间较小，而堆获得的空间较大;
（3）申请效率的不同。栈由系统自动分配，速度较快，而堆一般速度比较慢;
（4）存储内容的不同。栈在函数调用时，函数调用语句的下一条可执行语句的地址第一个进栈，然后函数的各个参数进栈，其中静态变量是不入栈的。而堆一般是在头部用一个字节存放堆的大小，堆中的具体内容是人为安排;
（5）底层不同。栈是连续的空间，而堆是不连续的空间。
### 在声明一个类时使用static关键字，具有两个方面的意义：
防止程序员写代码来实例化该静态类；
防止在类的内部声明任何实例字段或方法。
### static在内存中的地址为：程序的全局数据区。
### 值类型和引用类型
数据项的类型定义了存储数据需要的内存大小及组成该类型的数据成员。类型还决定了对象在内存中的存储位置——栈或堆。
类型被分为两种：值类型和引用类型，这两种类型的对象在内存中的存储方式不同。
值类型只需要一段单独的内存，用于存储实际的数据。
引用类型需要两段内存，第一段存储实际的数据，它总是位于堆中；第二段是一个引用，指向数据在堆中的存放位置。
对于值类型，数据存放在栈里；对于引用类型，实际数据存放在堆里而引用存放在栈里。
![在这里插入图片描述](https://img-blog.csdnimg.cn/8942b2baa46c4d22ab74fc8ac3b38bb6.png#pic_center)
### C#的4种变量
分别是本地变量、字段、参数、数组元素。
本地变量：在方法的作用域保存临时数据，不是类型的成员。
字段：保存和类型或类型实例相关的数据，是类型的成员。
参数：用于从一个方法到另一个方法传递数据的临时变量，不是类型的成员。
数组元素：（通常是）同类数据项构成的有序集合的一个成员，可以为本地变量，也可以为类型的成员。
###简单的变量声明
一个简单的变量声明至少需要一个类型和一个名称，如int a,其中int为类型，a为名称。
### 变量 、字段、属性的区别：
一句话：字段、属性都是变量，只是为了区分和数据安全设置的。
字段的使用场景：与类或者对象关系密切，建议使用private修饰。
属性的使用场景：对字段进行封装，提供get/set关键字，进行访问。
变量的使用场景：与类或者对象关系不密切，常常在方法或者语句块中使用。
字段和属性是相对于类而言的，而变量相对于方法或者语句块而言，可以在任何地方使用。
### 常量字段const表现得像静态字段，但在内存中没有存储位置。
![在这里插入图片描述](https://img-blog.csdnimg.cn/27ee9e5c6ed04080898d33520c572f7e.png#pic_center)
虽然常量成员表现得像一个静态量，但**不能将常量声明为静态static**，如：static const int a=3.14这句语法是错误的。
### 析构函数执行在类的实例被销毁之前需要的清理或释放非托管资源的行为。
### 非托管资源是指通过Win32 Api获得的文件句柄，或非托管内存块。
### const和readonly：
const是静态常量，readonly是动态常量。
静态常量是指编译器在编译时候会对常量进行解析，并将常量的值替换成初始化的那个值；而动态常量的值则是在运行的那一刻才获得的，编译器编译期间将其标示为只读常量，而不用常量的值代替，这样动态常量不必在声明的时候就初始化，而可以延迟到构造函数中初始化。
区别：可以通过静态常量与动态常量的特性来说明：
      1）const修饰的常量在声明的时候必须初始化;readonly修饰的常量则可以延迟到构造函数初始化 。
      2）const修饰的常量在编译期间就被解析，即常量值被替换成初始化的值;readonly修饰的常量则延迟到运行的时候。
      此外const常量既可以声明在类中也可以在函数体内，但是static readonly常量只能声明在类中。
###  this用法(指代当前实例、扩展方法、索引器)
#### 1.**this指代当前实例：**
 this关键字在类中使用，是对当前实例的引用，它只能被用在下列类成员的代码块中：
 （1）实例构造函数；
 （2）实例方法；
 （3）属性和索引器的实例访问器。
 值得注意的是，静态成员并不是实例的一部分，所以不能在任何静态函数成员的代码中使用this关键字，确切来说，this用于下列目的：
 用于区分类的成员和本地变量或参数；
 作为调用方法的实参。
#### 2. **this用作扩展方法：**
 扩展方法能够向现有类型“添加”方法，而无需创建新的派生类型、重新编译或以其他方式修改原始类型。扩展方法是一种特殊的静态方法，但可以像扩展类型上的实例方法一样进行调用。C#扩展方法第一个参数指定该方法作用于哪个类型，并且该参数以 this 修饰符为前缀。
    需要注意的几点：
   （1）**扩展方法（this 需要扩展的类 命名），如：public static void ExtensionEat(this Person person)；**
（2）**扩展方法必须是静态类的一个静态方法。**
（3）**调用扩展方法,必须用对象来调用 。**
如图看一段代码：

```csharp
using System;
namespace ConsoleApp10
{
    class Program
    {
        static void Main(string[] args)
        {
            Music p = new Music();
            p.ExtensionEat();
            p.ExtensionHello();
            p.ExtensionHaHa();
            ExtensionMusic.ExtensionHaHa(p);
            Console.ReadLine();
        }
    }
    public class Music
    {
        public void Eat()
        {
            Console.WriteLine("听歌曲");
        }
        public void Hello(string name)
        {
            Console.WriteLine("歌曲名：" + name);
        }
    }
    public static class ExtensionMusic //静态的扩展类，用于扩展Music类
    {
        public static void ExtensionEat(this Music  mu)//扩展方法必须是静态类的一个静态方法,this Music  mu这一句表示该ExtensionEat要扩展的类名是Music,然后在Music的实例中就可以调用该方法
        {
            mu.Eat();
            Console.WriteLine("打开播放器");
        }
        public static void ExtensionHello(this Music mu)
        {
            mu.Hello("起风了");
            Console.WriteLine("真好听");
        }
        public static void ExtensionHaHa(this Music mu)
        {
            Console.WriteLine("多听几遍");
        }
    }
}
```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/493896ff1679480c897e75fb1aefc4c1.png#pic_center)
#### 3. this用作索引器：
索引器（Indexer） 允许一个对象可以像数组一样使用下标的方式来访问。
先看一段代码：
```csharp
/// <summary>
/// 最简单的索引器
/// </summary>
public class IDXer
{
    private string[] name = new string[10];

    //索引器必须以this关键字定义，其实这个this就是类实例化之后的对象
    public string this[int index]
    {
        get
        {
            return name[index];
        }
        set
        {
            name[index] = value;
        }
    }
}

public class Program
{
    static void Main(string[] args)
    {
        //最简单索引器的使用           
        IDXer indexer = new IDXer();
        //“=”号右边对索引器赋值，其实就是调用其set方法
        indexer[0] = "张三";
        indexer[1] = "李四";
        //输出索引器的值，其实就是调用其get方法
        Console.WriteLine(indexer[0]);
        Console.WriteLine(indexer[1]);
        Console.ReadKey();
    }
}
```

```csharp
 //以字符串为下标的索引器
    public class IDXer2
    {
        private Hashtable name = new Hashtable();

        //以字符串为下标的索引器
        public string this[string index]
        {
            get
            {
                return name[index].ToString();
            }
            set
            {
                name.Add(index, value);
            }
        } 
    }

    public class Program
    {
        static void Main(string[] args)
        {
            //以字符串为下标的索引器
            IDXer2 indexer2 = new IDXer2();
            indexer2["A01"] = "张三";
            indexer2["A02"] = "李四";
            Console.WriteLine(indexer2["A01"]);
            Console.WriteLine(indexer2["A02"]);
            Console.ReadKey();
        }
  }
```
索引器的索引值（Index）类型不限定为整数，如上两个图代码中的整形和字符串型索引。
索引器与属性的区别：
（1）索引器的命名只能为this，而属性可以任意命名（开头字母大写）；
（2）索引器可以重载，属性不可以；
（3）索引器不可用static进行声明，属性可以。
### 派生类屏蔽基类的成员：
虽然派生类不能删除它继承的任何成员，但可以用与基类成员名称相同的成员来屏蔽基类成员，这也是继承的主要功能之一，很实用。**派生类屏蔽基类成员的关键字是new**，在派生类中屏蔽一些基类成员的一些要点如下：
（1）要屏蔽一个继承的数据成员，需要声明一个新的相同类型的成员，并使用相同的名称。
（2）通过在派生类中声明新的带有相同签名的函数成员，可以隐藏或屏蔽继承的基类的函数成员。注意：签名由名称和参数列表组成，不包括返回类型。
（3）**要让编译器知道你在故意屏蔽继承的成员，实用new修饰符**。否则，程序会成功编译，但会警告你隐藏了一个继承的成员。
（4）也可以屏蔽静态成员。
### virtual的使用
**当使用基类引用访问派生类对象时，得到的是基类的成员**。**虚方法可以使基类的引用访问“升至”派生类级别**。可以使用基类引用调用派生类的方法，只需要满足以下条件：
（1）派生类的方法和基类的方法拥有相同的签名和返回类型；
（2）基类的方法使用virtual标注；
（3）派生类的方法使用override标注。
可对比以下两图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/96218732bf764e3c88f1e03d1d54de9d.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f13c4719bbb4b06b413d5e30e396975.png#pic_center)
### 构造函数初始化语句：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2bd56a65141e4c55b123d68643545321.png#pic_center)
如，一个简单的继承类程序：

```csharp
using System;
namespace Application
{
    public class ShangPin
    {
        private double _price;
        private double _count;
        private string _name;
        private int _id;
        public double Price
        {
            get { return _price; }
            set { _price = value; }
        }
        public double Count
        {
            get { return _count; }
            set { _count = value; }
        }
        public string Name
        {
            get { return _name; }
            set { _name = value; }
        }
        public int ID
        {
            get { return _id; }
            set { _id = value; }
        }
        public ShangPin(string name, double price, int id, double count)
        {
            this.Name = name;
            this.Price = price;
            this.ID = id;
            this.Count = count;
            Console.WriteLine("名称：{0},价格：{1},编号：{2},数量：{3}", this.Name, this.Price, this.ID, this.Count);
        }
    }

    public class Banana : ShangPin
    {
        private double _keepDays;
        public double KeepDays
        {
            set { _keepDays = value; }
            get { return _keepDays; }
        }

        public Banana(string name, double price, int id, double count, double keepDays) : base(name, price, id, count)
        {
            this.KeepDays = keepDays;
        }
    }
    class app
    {
        static void Main()
        {
            // ShangPin shangPin = new ShangPin("可乐",2.5,1,100);
            // Console.WriteLine("名称：{0},价格：{1},编号：{2},数量：{3}",shangPin.Name,shangPin.Price,shangPin.ID,shangPin.Count );
            Banana banana = new Banana("香蕉", 2.3, 2, 50.0, 7);
            Console.WriteLine("名称：{0},价格：{1},编号：{2},数量：{3},保存天数：{4}", banana.Name, banana.Price, banana.ID, banana.Count, banana.KeepDays);//会先调用基类的构造函数，然后再调用子类的构造函数
        }
    }

}
```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d905cbd6c284baf878289a4b20b03a6.png#pic_center)

而另外一种形式的构造函数初始化语句（通过this）可以让编译器使用该类中的其他构造函数。
### readonly字段
**readonly字段只可以在构造函数中初始化**，如果在其他方法中初始化一个readonly字段（即使这个方法只被构造函数调用），会得到一个编译错误。
### 本地变量与字段的区别：
![在这里插入图片描述](https://img-blog.csdnimg.cn/c68062f503d043cdb70dc45cf58a815a.png#pic_center)
### 关键字var
编译器可以通过初始化语句提供的信息推断出数据类型。
（1）传统定义变量是已经知道变量的类型，如： int a = 1； string b = “hello”；char c='x';
而var关键字不用预先知道变量的类型，是根据给变量赋值来判定变量属于什么类型；如var a =1， 则a是整型；var b = “hello”,则b是字符型;var c='x',则c也是字符型。
（2）简言之，var可以代替任何类型，编辑器会根据上下文来判断使用者具体想用什么类型，当无法确定自己将使用什么类型时，就可以使用var。
（3）使用var关键字有一些重要的条件：
1. 必须在定义时初始化。必须是var a=“abc”的形式，不能是var a； a=“abc”的形式；
2. 只能用于本地变量，不能用于字段；
3. 只能在变量声明中包含初始化时使用；
4. 一旦编译器推断除变量的类型，它就是固定且不能更改的。
### C#中方法的参数有四种类型
      （1） 值参数  (不加任何修饰符,是默认的类型)
      （2）引用型参数  (以ref 修饰符声明)
      （3）输出参数  (以out 修饰符声明)
      （4）数组型参数  (以params 修饰符声明)
### 值参数和引用参数：
当值参数为值类型时，值被复制，产生一个独立的数据项，当值参数为引用类型时，引用被复制，实参和形参都引用堆中的同一个对象。
使用引用参数时，必须在方法的声明和调用中都使用ref修饰符。
引用类型作为值参数和引用参数：
（1）将引用类型对象作为值参数传递 ：如果在方法内创建一个新对象并赋值给形参，将切断形参与实参之间的关联，并且在方法调用结束之后，新对象也将不复存在。
（2）将引用类型作为引用参数传递 ：如果在方法内创建一个新对象并赋值给形参，在方法结束后该对象依然存在，并且是实参所引用的值。
### 输出参数out
引用参数ref修饰的参数必须对其赋初值，但是初值不能是常量（即不能用const修饰），因为按引用传递可能会改变参数的值。在函数使用out参数时，必须看做是尚未赋值（不晓得为什么），实参传递给形参的值在函数执行时会丢失，参考以下代码段：
![在这里插入图片描述](https://img-blog.csdnimg.cn/be808637c443491c81b670211011f815.png#pic_center)
### 成员访问修饰符：
![在这里插入图片描述](https://img-blog.csdnimg.cn/5cbf00d5417640f39f6864cadf35f49a.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/55740405dd3e43f681e280103110a8a2.png#pic_center)
### 抽象类
抽象类是指设计为被继承的类，抽象类只能被用作其他类的基类。
（1）不能创建抽象类的实例；
（2）抽象类必须使用abstract修饰符来声明，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8b864f6a0c3b4037b1cec5ce290a0ed9.png#pic_center)
（3）抽象类可以包含抽象成员和普通的非抽象成员，即抽象类的成员可以是抽象成员和普通成员的任意组合。
（4）抽象类自己可以派生另外一个抽象类，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ebde190e3c94f70b2985873326b6bed.png#pic_center)
（5）**任何派生自抽象类的类必须使用override关键字实现该类的所有抽象成员，除非派生类自己也是抽象类**。
看一段代码:
![在这里插入图片描述](https://img-blog.csdnimg.cn/d2aa6b92a43a49c4a48a30b7d86211d9.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/61d8773c2be04c9e964d8f892cb3043d.png#pic_center)
### 静态类
**静态类中所有的成员都是静态的**，静态类用于存放不受实例数据影响的函数或数据。**静态类的一个常见用途就是创建一个包含一组数学方法和值的数学库**。
关于静态类需要注意的几个地方：
（1）类本身必须被标记为static;
（2）**类的所有成员必须是静态的**；
（3）类可以有一个静态构造函数，但不能有实例构造函数，不能创建该类的实例；
（4）静态类是隐式封装的，也就是说，不能继承静态类。
### C#提供隐式转换与显式转换
对于隐式转换，当决定在特定上下文中使用特定类型时，如有必要，编译器会自动执行转换；
对于显式转换，编译器只在使用显示转换运算符时才执行转换。
### 运算符重载
运算符重载：通过特定的语法，使某些运算符可以具备特殊的功能。关键字operator，修饰符必须为public static。
运算符重载的例子：

```csharp
using System;

public struct Point
{
    public double x;
    public double y;
    public Point(double x,double y)
    {
        this.x = x;
        this.y = y;
    }
//运算符重载，使加号具有新的功能
//可以实现两个Point对象直接相加，得到一个新的点
    public static Point operator +(Point p1,Point p2)
    {
        return new Point(p1.x + p2.x, p1.y + p2.y);
    }
    //参数类型可以不相同
    public static Point operator + (Point p1,int a)
    {
        return new Point(p1.x + a, p1.y + a);
    }
//*号是二元运算符，因此必须有两个参数
    public static Point operator *(Point p1,Point p2)
    {
        return new Point(1, 1);
    }
//-号的身份有两种：1是加减乘除的减号，2是负号。为前者时是二元运算符，为后者是一元运算符。
    public static Point operator - (Point p1)
    {
        return new Point(1, 1);
    }
    //【重载--运算符】练习
    public static Point operator --(Point p1)
    {
        return new Point(p1.x-1,p1.y-1);
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        Point p1 = new Point(1, 2);
        Point p2 = new Point(2, 3);
        Point p = p1 + p2;
        //【重载--运算符】练习
        Point p3 = new Point(1, 3);
        p3--;
        Console.WriteLine(p3.x);
        Console.WriteLine(p3.x);
        Console.WriteLine(p3.y);
        p3 = new Point(1, 3);
        --p3;
        Console.WriteLine(p3.x);
        Console.WriteLine(p3.y);
    }
}

```
### C# 关键字explicit和implicit
implicit：代表用来声明隐式自定义类型的转换;
explicit：代表用来声明显示自定义类型的转换。
### typeof运算符
typeof用于获取类型的 System.Type 对象
例如下面代码：
```csharp
using System.Reflection;//反射
using System;
namespace APP
{
    class SomeClass
    {
        public int Field1;
        public int Field2;
        public void Method1()
        {
        	
        }
        public int Method2()
        { 
            return 1;
        }
    }
    class Program
    {
        static void Main()
        {
            var t = typeof(SomeClass);
            FieldInfo[] fi = t.GetFields(); //返回FieldInfo类型，用于取得该类的字段（成员变量）的信息
             MethodInfo[] mi = t.GetMethods();//返回MethodInfo类型，用于取得该类的方法的信息
            foreach (var f in fi)
            {
                Console.WriteLine("Field:{0}", f.Name);
            }
                
            foreach (var m in mi)
            {
                Console.WriteLine("Method:{0}", m.Name);
            }       
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/de8b1b0a8bc54ebb908be6dd97cb9888.png#pic_center)
### 结构和类的区别
结构和类非常相似，都有数据成员和函数成员，最重要的区别是：
（1）类是引用类型，而结构是值类型；
（2）结构是隐式封装的，这意味着他们不能被派生。
**结构也可以有实例构造函数和静态构造函数**，但不能有析构函数。
### 关于静态字段
**static修饰的静态字段被类的所有实例共享**，所有实例都访问同一内存位置。如果该内存位置的值被一个实例改变了，这种变化对于所有的实例都改变。
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b11cdc14fb746cf868c567e6c5aea7e.png#pic_center)
### **可以声明为static的类成员类型(重要)**
![在这里插入图片描述](https://img-blog.csdnimg.cn/251c410e0d1948fb94d4b159676a789c.png#pic_center)
### 枚举
（1）枚举是一个值类型， 包含一组命名的常量，  枚举类型用 enum 关键字定义。如，声明一个简单的枚举类：
![在这里插入图片描述](https://img-blog.csdnimg.cn/d52f6adae3f348b49583aefbca2e058e.png#pic_center)
（2）默认情况下， enum 的类型是 int。
（3）**动态获得枚举类型的信息：**
Enum.GetNames：该方法返回一个包含所有枚举名的字符串数组，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/735f55bd872b40acb1a3c982d6572652.png#pic_center)Enum.GetValues：为了获得枚举的所有值， 可以使用 Enum.GetValues 。 Enum.GetValues 返回枚举值的一个数组。 为了获得整数值， 需要把它转换为枚举的底层类型， 为此应使用 foreach语句，如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e24172713c8a49e68e737efc4142b84c.png#pic_center)
（4）Flags特性：没理解（后续学习）
### 数组
（1）C#不支持动态数组，也就是说，数组一旦被创建，大小就固定了。
（2）C#中的各种数组：
![在这里插入图片描述](https://img-blog.csdnimg.cn/9366394396ee4806a52a7e5b864fcbcb.png#pic_center)
（3）数组中一些可用的方法和属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/14c68a2fa9184f3891ca2047c35cf8f1.png#pic_center)
### 委托
[自己以前写过的委托笔记](https://blog.csdn.net/weixin_48239221/article/details/124169936)
委托就是可以用方法名调用另一方法的便捷方法，可以理解为一个”命令”。
委托是一种用户自定义的类型，**它也是引用类型**，委托持有一个或者多个方法，以及一些列预定义操作。
#### 1.delegate委托
先看一代码：
```csharp
using System;

namespace enumExample
{
    delegate void myDel(int value);//声明一个委托类型
    class Program
    {
        void PrintLow(int value)
        {
            Console.WriteLine("{0}---Low Value", value);
        }
        void PrintHigh(int value)
        {
            Console.WriteLine("{0}---High Value", value);
        }
        static void Main(string[] args)
        {
            Program program = new Program();
            myDel del;//声明一个委托变量
            Random rand = new Random();//创建随机整数生成器对象
            int randValue = rand.Next(99);//该随机数在0到99之间随机选取
            del = randValue < 50 ? new myDel(program.PrintLow) : new myDel(program.PrintHigh);//当随机数的值小于50时，创建一个包含PrintLow的委托对象并将其赋值给del；
                                                                                              //当随机数的值大于50时，创建一个包含PrintHigh的委托对象并将其赋值给del；
            del(randValue);//执行委托
        }
    }
}

```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/94b9e199e8d1490aa2c40acb1d755490.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f26b4771feb0417c8fb2bfc09a9c7a0f.png#pic_center)
因此使用委托的步骤：
（1）声明一个委托类型；
```csharp
delegate void myDel(int value);//声明一个委托类型
```
（2）使用该委托类型声明一个委托变量；
```csharp
myDel del;//声明一个委托变量
```
（3）创建委托类型的变量，把它赋值给委托变量。新的委托对象包含指向某个方法的引用，这个方法和第一步定义的签名和返回类型保持一致；
```csharp
del = randValue < 50 ? new myDel(program.PrintLow) : new myDel(program.PrintHigh);//当随机数的值小于50时，创建一个包含PrintLow的委托对象并将其赋值给del；
                                                                                       //当随机数的值大于50时，创建一个包含PrintHigh的委托对象并将其赋值给del；
```
（4）（非必要步骤）可以选择为委托对象增加其他方法（这些方法必须和第一步定义的签名和返回类型保持一致）；
（5）在代码中可以像调用方法一样调用委托，在调用委托的时候，其包含的每一个方法都会被执行。
```csharp
 del(randValue);//执行委托
```
再看一段代码，稳固下委托的使用步骤：

```csharp
using System;

namespace ConsoleApp1
{
     class Program
    {
        public delegate void MyDelete(string str);//声明一个委托，可以简单地把委托理解为一个”命令”
        static void Print1(string str)//定义一个方法Print1
        {
            Console.WriteLine("Print1:"+str);
        }
        static void Print2(string str)//定义一个方法Print2
        {
            Console.WriteLine("Print2:" + str);
        }
        static void Main()
        {
            MyDelete myDelete=Print1; //将方法追加至委托
            myDelete += Print2;//委托的多播；
            myDelete("hello");//该myDelete委托此时调用的是Print1和Print2两个函数，这就是委托的多播（一个委托可以调用多个方法）
            Console.ReadLine();
        }
    }
}


```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/50add8640a284869b3f87338406e88e9.jpeg#pic_center)
**调用带有返回值的委托：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/40b4c55fbf314b3199f181590c2530fa.png#pic_center)
如：

```csharp
using System;
namespace ConsoleApp1
{
    delegate int MyDel();//声明一个返回值为int的委托类型
    class MyClass
    {
      private int _value = 5;
      public int Add3()
        {
            _value += 3;
            return _value;
        }
       public int Add2()
        {
            _value += 2;
            return _value;
        }
    }
    class Program
    {
        static void Main()
        {
            MyClass mc = new MyClass();
            MyDel del1=mc.Add2;//给该委托加一个方法
            del1 += mc.Add3;//给该委托追加一个方法
            del1 += mc.Add2;//给该委托追加一个方法
            Console.WriteLine("Value:{0}", del1());
        }
    }
}
```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/473e1f1ca4a34c958b0f27e4178ba222.png#pic_center)
**调用带有引用参数的委托：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/263bf92f9479414a9a2a963993ae64e1.png#pic_center)
#### 2.Action委托
**Action委托签名不提供返回类型（也就是说Action委托是没有返回值的）**，它具有Action、Action<T1,T2>、Action<T1,T2,T3>……Action<T1,……T16>多达16个的重载，其中传入参数均采用泛型中的类型参数T，涵盖了几乎所有可能存在的无返回值的委托类型。
代码示例：
```csharp
 class Program
    {
        public void Print()
        {
            Console.WriteLine("我是一个普通的输出方法");
        }
        public void Add1(int a, int b)
        {
            Console.WriteLine("a+b的值为{0}", a + b);
        }

        public void Add2(string name, int age)
        {
            Console.WriteLine("我的名字叫：{0}，今年{1}岁", name, age);
        }
        public void Add3(bool isTrue,string str)
        {
            if (isTrue)
            {
                Console.WriteLine("Yes,"+str);
            }
            else
            {
                Console.WriteLine("No," + str);
            }
        }
        public static void Main(string[] args)
        {
            Program program = new Program();
            Action ac1 = program.Print; // Action是系统内置（预定义）的一个委托类型，它可以指向一个没有返回值，没有参数的方法
            ac1();
            Action<int, int> ac2 = program.Add1; // 定义了一个委托类型，这个类型可以指向一个没有返回值，有两个int参数的方法
            ac2(3, 4);
            Action<string, int> ac3 = program.Add2; // Action可以后面通过泛型去指定Action指向的方法的多个参数的类型，参数的类型跟Action后面声明的委托类型是对应的
            ac3("张三", 23);
            Action<bool,string> ac4 = program.Add3;
            bool isTrue = false;
            ac4(isTrue, "你好");
        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/0a429e3d1168499c8625e01fcf809a3c.png#pic_center)
#### 3.Func委托
Func具有Func<TResult>、Func<T,Tresult>、Func<T1,T2,T3……,Tresult>17种类型重载，T1……T16为出入参数，Tresult为返回类型。**Func委托必须有返回值**
```csharp
 class Program
    {
        private  int Test()
        {
            return 1;
        }
        private  int Mes(string name)
        {
            Console.WriteLine("我的名字叫：" + name);
            return 100;
        }
        private  int Add(int a, int b)
        {
            return a + b;
        }
        public static void Main(string[] args)
        {
            Program program = new Program();

            Func<int> fu1 = program.Test; // Func中的泛型类型指定的是方法的返回值类型
            int res1 = fu1();
            Console.WriteLine(res1);

            Func<string, int> fu2 = program.Mes; // Func后面可以跟很多类型，最后一个类型是返回值类型，前面的类型是参数类型，参数类型必须跟指向的方法的参数类型安装顺序对应
            int res2 = fu2("李四");
            Console.WriteLine(res2);

            Func<int, int, int> fu3 = program.Add; // Func后面必须指定一个返回值类型，参数类型可以有0~16个，先写参数类型，最后一个是返回值类型
            int res3 = fu3(2, 3);
            Console.WriteLine(res3);
        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e8cfc4c202304afe99e77e6945d1d379.png#pic_center)
#### 4.Predicate委托

 - Predicate是返回bool型的泛型委托；
 - Predicate<int>表示传入参数为int，返回bool的委托；
 - Predicate有且只有一个参数，返回值固定为bool；
```csharp
 class Program
    {

        private bool isEqual(bool b)
        {
            return b;
        }
        private bool Show(int number)
        {
            if (number == 1)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
        public static void Main(string[] args)
        {
            Program program = new Program();

            Predicate<int> p1 = program.Show;// Predicate是返回bool型的泛型委托，有且只有一个参数
            bool res1= p1(5);
            Console.WriteLine(res1);

            Predicate<bool> pre = program.isEqual; // Predicate是返回bool型的泛型委托，有且只有一个参数
            bool res2= pre(2 > 1);
            Console.WriteLine(res2);
        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/69c64a3988d2485193840a78b6786341.png#pic_center)
#### 5.这几种委托的区别：
 - Delegate至少0个参数，至多32个参数，可以无返回值，也可以指定返回值类型；
 -  Func可以接受0个至16个传入参数，必须具有返回值；
 - Action可以接受0个至16个传入参数，无返回值；
 -  Predicate只能接受一个传入参数，返回值为bool类型；

### 匿名方法
匿名方法是在初始化委托时内联（inline）声明的方法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/e78e1d094cce4ada9aa7c079334d18e2.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8707f11d6b334f09b1b2bb4c9c0d28ac.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc2e671e34fe4b448be88ebede0bd9e0.png#pic_center)
### Lambda表达式
Lambda表达式的出现很好地替代了匿名语法，它是匿名方法的简写形式,用来代替匿名方法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/7384da9d9767429a8f8123a0995c2f51.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1f390676a374f238906385ca0c53b7d.png#pic_center)
### 一些代码编写规范
（1）类名使用 BigCamelCase 大驼峰风格：
如：BaseClass,DerivedClass,SecondDerivedClass。
（2）方法名也用BigCamelCase大驼峰风格：
如：GetHttpMessage()，GetValue()。
（3）参数名、成员变量、局部变量都统一使用 lowerCamelCase 小驼峰风格：
如：localValue,inputUserId。
（4）常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长：
如：MAX_STOCK_COUNT,CACHE_EXPIRED_TIME。
（5）抽象类命名使用 Abstract开头 或 Base 结尾 ；异常类命名使用 Exception 结尾：
抽象类是特殊类，不能被实例化，抽象方法只能存在于抽象类中
访问修饰符 abstract class 类名
{
访问修饰符 abstract void 方法名();
}
抽象方法是一种特殊虚方法，只能声明，不能实现；
（6）测试类命名以它要测试的类的名称开始，以 Test 结尾。
（7）接口和实现类的命名：接口以I开头，实现类用 Impl 结尾：
如：XOrderImpl 实现 IOrder 接口。
（8）枚举类名带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开：
如：枚举名字为 ProcessStatusEnum 的成员名称：SUCCESS / UNKNOWN_REASON。
### 事件
（1）使用 += 运算符注册事件；使用 -= 运算符取消订阅。
（2）**C# 中使用事件机制实现线程间的通信**。
（3）事件模型的5个组成部分
事件拥有者（event source）（类对象）（有些书将其称为事件发布者）；
事件成员（event）（事件拥有者的成员）（事件成员就是事件本身，事件不会主动发生，其只会在事件拥有者的内部逻辑的触发下发生。）；
事件响应者（event subscriber）（类对象）（有些书将其称为事件订阅者）；
事件处理器（event handler）（事件的响应者的成员）（根据拿到的事件参数/信息对事件进行处理）；
事件订阅（委托类型）。
例如，“裁判员开枪，运动员开始跑步。”
在上面这个例子中，事件拥有者是裁判员，事件成员是开枪，事件响应者是运动员，事件处理是开始跑步。
（4）代码举例：
```csharp
	//事件的使用
//1.声明一个委托
//2.声明一个事件
//3.编写引发事件的函数
//4.编写引发事件后的处理程序
//5.把事件发生后的处理程序注册到事件上
using System;
namespace APP
{
    //事件发送者
    class Dog
    {
        //1.声明关于事件的委托；
        public delegate void AlarmEventHandler(object sender, EventArgs e);
        //2.声明事件；   
        public event AlarmEventHandler Alarm;
        //3.编写引发事件的函数；
        public void OnAlarm()
        {
            if (this.Alarm != null)
            {
                Console.WriteLine("\n狗报警: 有小偷进来了,汪汪~~~~~~~");
                this.Alarm(this, new EventArgs());   //发出警报
            }
        }
    }
    //事件接收者
    class Host
    {
        //４.编写事件处理程序
        void HostHandleAlarm(object sender, EventArgs e)
        {
            Console.WriteLine("主人: 抓住了小偷！");
        }
        //５.注册事件处理程序
        public Host(Dog dog)
        {
            dog.Alarm += new Dog.AlarmEventHandler(HostHandleAlarm);
        }
    }

    //６.现在来触发事件
    class Program
    {
        static void Main(string[] args)
        {
            Dog dog = new Dog();
            Host host = new Host(dog);
            //当前时间，从2008年12月31日23:59:50开始计时
            DateTime now = new DateTime(2015, 12, 31, 23, 59, 50);
            DateTime midnight = new DateTime(2016, 1, 1, 0, 0, 0);
            //等待午夜的到来
            Console.WriteLine("时间一秒一秒地流逝... ");
            while (now < midnight)
            {
                Console.WriteLine("当前时间: " + now);
                System.Threading.Thread.Sleep(1000);    //程序暂停一秒
                now = now.AddSeconds(1);                //时间增加一秒
            }
            //午夜零点小偷到达,看门狗引发Alarm事件
            Console.WriteLine("\n月黑风高的午夜: " + now);
            Console.WriteLine("小偷悄悄地摸进了主人的屋内... ");
            dog.OnAlarm();//引发事件的函数
            Console.ReadLine();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/61d5ef731be3457b845cfc044701872e.png#pic_center)
（5）再如代码举例:

```csharp
	//事件的使用
//1.声明一个委托
//2.声明一个事件
//3.编写引发事件的函数
//4.编写引发事件后的处理程序
//5.把事件发生后的处理程序注册到事件上
using System;
namespace APP
{
   public class Dog
    {
        public delegate void dele();//1.声明一个委托
        public event dele deleHandler;//2.声明一个事件
        public void cry()//3.引发事件的函数
        {
            Console.WriteLine("小偷进来，狗开始叫：");
            if (deleHandler != null)
            {
                deleHandler();//引发该事件
            }
        }
     }
    public class Program
    {
        public static void result()//4.事件发生后的处理程序
        {
            Console.WriteLine("狗叫之后，主任转身起床抓小偷!");
        }
        static void Main()
        {
            Dog dog = new Dog();
            dog.deleHandler += result;//5.把事件发生后的处理程序注册到事件上
            dog.cry(); //引发事件的发生  
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ca09eafc244545bfa02300ed5fbc766b.png#pic_center)
### 接口
（1）**接口的声明不能包含以下成员：
数据成员、静态成员。
接口的声明只能包含如下类型的非静态成员函数的声明：
方法、属性、事件、索引器。**
（2）这些函数成员的声明不能包含任何实现代码，在每一个成员声明的主体后必须使用分号；
（3）接口名称必须从大写的I开始写起；
（4）下面声明一个简单的接口：
```csharp
 public interface IMyinterFace1
    {
        int add(int low, int high);
        double reduce(double low, double high);
    }
```
（5）接口的访问性和接口成员的访问性之间有一些区别：
接口的声明可以有任何的访问修饰符（但一般都为public），如：private、protected、internal、public。而接口的成员是隐式public的，不能有任何的访问修饰符，包括public。例如下面的这段代码：
```csharp
 public interface IMyinterFace1
    {
        //接口成员不允许有访问修饰符
        int add(int low, int high);//正确
        double reduce(double low, double high);//正确
        public int add_1(int low, int high);//错误
        private double reduce_2(double low, double high);//错误
    }
```
（6）只有类和接口才能实现接口，然而要实现接口，类和结构必须：在基类列表中包含接口名称，为每一个接口的成员提供实现。
注意：**如果类实现了接口，它就必须实现类的所有成员；**
如果类从基类继承并实现了接口，则基类列表中的基类名称必须放在所有接口之前，例如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2be9b8e855b1471ea132a974327c543d.png#pic_center)
比较接口和抽象类：
抽象类可以有
（7）as运算符和接口一起配合使用是很好的选择。
如果我们尝试将类对象的引用强制转换为类未实现的接口的引用，强制转换操作会抛出一个异常，可以通过使用as运算符来避免这个问题。具体如下：
如果类实现了接口，表达式指向返回接口的引用。
如果类没有实现接口，表达式返回null而不是异常。
![在这里插入图片描述](https://img-blog.csdnimg.cn/b4fdf93c99e94ec89a72931ce9790d45.png#pic_center)
if(this !=null)判空的目的是看看接口是否拿到了类的实例。

（8）类或结构可以实现任意数量的接口。
（9）如果一个类实现了多个接口，并且其中一些接口具有相同的签名和返回类型的成员，那么类可以实现单个成员来满足所有包含重复成员的接口。例如：

```csharp
using System;
namespace APP
{
    interface IMyinterFace1
    {
        void PrintOut(string s);
    }

    interface IMyinterFace2
    {
        void PrintOut(string t);
    }

    public class AP : IMyinterFace1,IMyinterFace2
    {
        public void PrintOut(string s)
        {
            Console.WriteLine(s);
        }
    }
    public class A
    {
        static void Main()
        {
            AP aP = new AP();
            aP.PrintOut("你好世界！");
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1ca998866754b1c986be6941dc662f4.png#pic_center)
（10）接口还可以继承接口，例如：
```csharp
  interface IMyinterFace1
    {
        void PrintOut(string s);
    }

    interface IMyinterFace2
    {
        void PrintOut(string t);
    }

    interface IMyinterFace3 : IMyinterFace1, IMyinterFace2
    {

    }
```
(11)接口隔离原则：在实际项目中，可以实现一些小接口，而不是把功能都集中在一个大接口里。

### 转换
转换是接收一个类型的值并使用它作为另一个类型的等价值的过程。
使用强制转换表达式，意味着可能要承担数据丢失的后果。
#### （1）装箱和拆箱
 - **装箱：将值类型转换为引用类型的操作**。
 - **拆箱：相应地将引用类型转换成值类型。**
	   ![在这里插入图片描述](https://img-blog.csdnimg.cn/46c88735c3734d84ac8b00e837a4ba55.png#pic_center)
#### （2）is运算符(用来检测转换是否成功，不像as操作符那样直接转换)：
有时候有些转换是不成功的，需要在运行时抛出一个InvalidCastException异常，可以使用is运算符来检查转换是否会成功完成，从而避免盲目尝试转换。
is运算符的语法如下：
```csharp
源表达式 is 目标类型
```
如果源表达式可以通过以下三种方式：引用、拆箱、装箱转换成功，则运算符返回true。
代码举例：
```csharp
using System;
namespace APP
{
    public class Person
    {
        public string Name = "小明";
        public int Age = 25;
    }

    public class Employee : Person
    {

    }

    public class Program
    {
        static void Main()
        {
            Employee bill = new Employee();
            Person p;
            if(bill is Person)//检测变量bill能否转换为Person类型
            {
                p = bill;
                Console.WriteLine("Person Info:{0},{1}", p.Name, p.Age);
            }
        }
    }
}
```
**注意：** is运算符只可用于引用、装箱、拆箱三种转换，不可用于用户自定义转换。
#### （3）as运算符：
as运算符和强制转换运算符类似，只是不抛出异常。如果转换失败，它返回空而不是抛出异常。
语法如下：
源表达式 as 目标类型
**注意：** 目标类型必须是引用类型。
![在这里插入图片描述](https://img-blog.csdnimg.cn/be41243b67e341e2b2a7799dcee3a084.png#pic_center)
（6）当长类型的数据转换成短数据时，数据可能会丢失，需要我们进行显示地强制转换。
### 泛型
泛型：多种类型可以共享一组代码。泛型允许我们声明类型参数化的代码，可以用不同的类型进行实例化，也就是说，我们可以用“类型占位符”来写代码，然后在创建类的实例时指明真实的类型。
（1）**C#提供了5中泛型，包括：类、结构、接口、委托和方法。（注意：前四种是方法，而方法则是成员。）**
（2）泛型类不是实际的类，而是类的模板，也就是说，我们要先从他们构建实际的类类型，然后创建这个构建后的类类型的实例。这个过程如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/049cda551932407d9e494766c33f3514.png#pic_center)
（3）**泛型类代码**
看一段代码：
```csharp
using System;
namespace APP
{
    public class GeneriClass1<T> //泛型类GeneriClass1
    {
        private T[] arr;
        public GeneriClass1(int size)//构造函数
        {
            arr = new T[size];
        }
        public T GetItem(int index)
        {
            T t = arr[index];
            return t;
        }

        public void setItem(T t,int index)
        {
            arr[index] = t;
        }
    }

    public class Program
    {
        static void Main()
        {
            GeneriClass1<int> IntArray = new GeneriClass1<int>(5);//构建实际的类类型，然后创建这个构建后的类类型的实例
            GeneriClass1<string> StringArray = new GeneriClass1<string>(5);//构建实际的类类型，然后创建这个构建后的类类型的实例

            for (int i = 0; i < 5; i++)
            {
                IntArray.setItem(i * 5, i);
            }
            for (int i = 0; i < 5; i++)
            {
                Console.Write(IntArray.GetItem(i)+" ");
            }
            Console.WriteLine();

            StringArray.setItem("老鼠", 0);
            StringArray.setItem("小狗", 1);
            StringArray.setItem("乌龟", 2);
            StringArray.setItem("小西八", 3);
            StringArray.setItem("小八嘎", 4);
            for (int i = 0; i < 5; i++)
            {
                Console.Write(StringArray.GetItem(i)+" ");
            }
            Console.WriteLine();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/146fac05402843d095c844ee3ca5a0a5.png#pic_center)
（4）泛型方法：
泛型方法具有类型参数列表和可选的约束。
 1. 泛型方法有两个参数列表：
 封闭在圆括号内的方法参数列表和封闭在尖括号内的类型参数列表。
 2. 要声明泛型方法，需要：在方法名称之后和方法参数列表之前防止类型参数列表，并在方法参数列表后放置可选的约束子句。
 如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/71e9ba404b20440ab94e1f2e691f9c66.png#pic_center)
3. 泛型方法的调用：
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5ca9aecd69b45fbb1e13736611e1c4a.png#pic_center)
4. 泛型方法举例：
代码：
```csharp
using System;
namespace APP
{
    class Simple//非泛型类
    {
        public static void ReversePrint<T>(T[] arr)//泛型方法
        {
            Array.Reverse(arr);//翻转该数组
            foreach( T item in arr)
            {
                Console.Write(item.ToString()+" ");
            }
            Console.WriteLine();
        }
    }
    class Program
    {
        static void Main()
        {
            var intArray = new int[5] { 1,3,5,7,9};
            var stringArray = new string[5] { "s1","s2","s3","s4","s5"};
            var doubleArray = new double[5] { 1.2,2.3,3.4,4.5,5.6};

            Simple.ReversePrint<int>(intArray);//调用方法
            Simple.ReversePrint(intArray);//推断类型并调用

            Simple.ReversePrint<string>(stringArray);//调用方法
            Simple.ReversePrint(stringArray);//推断类型并调用

            Simple.ReversePrint<double>(doubleArray);//调用方法
            Simple.ReversePrint(doubleArray);//推断类型并调用
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d254270e4779434b89fa3666b7960d43.png#pic_center)
（5）扩展方法和泛型类：
扩展方法可以和泛型类结合来使用，它允许我们将类中的静态方法关联到不同的泛型类上，还允许我们像调用类构造实例的实例方法一样来调用方法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/492fcbff0ac247bd94b8f2554be66935.png#pic_center)
代码举例：

```csharp
using System;
using System.Collections.Generic;

namespace APP
{
    public class Holder<T>
    {
        List<T> list = new List<T>();
        public Holder(T t1,T t2,T t3)
        {
            list.Add(t1);
            list.Add(t2);
            list.Add(t3);
        }
        public List<T> returnList()
        {
            return list;
        }
    }

    public static class ExtendExample//泛型类的扩展方法必须是静态static的
    {
        public static void ExtendH<T>(this Holder<T> e,int index1,string s1)//扩展方法  注意：扩展方法的定义必须要有this,this后边跟的是扩展的泛型类的名字.此ExtendH方法有两个形参，分别是int型的index1，string型的s1
        {
            List<T> lists = new List<T>();
            lists = e.returnList();
            Console.WriteLine(index1+" "+s1);
            foreach(var item in lists)
            {
                Console.Write(item+" ");
            }
            Console.WriteLine();
        }
    }

    public class Program
    {
        public static void Main()
        {
            Holder<int> intHolder = new Holder<int>(1, 3, 5);
            Holder<string> stringHolder = new Holder<string>("s1", "s2", "s3");
            intHolder.ExtendH(1,"s1");
            stringHolder.ExtendH(2,"s2");
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/224b5f1d98a14d06aa17f055babe0c81.png#pic_center)
（6） 泛型结构

```csharp
using System;
using System.Collections.Generic;

namespace APP
{
   struct PieceOfData<T>
    {
        private T _data;
        public PieceOfData(T t)//结构的构造函数(注意;结构也可以有有构造函数)
        {
            _data = t;
        }

        public T Data { get => _data;}
    }

    public class Program
    {
        public static void Main()
        {
            var intData = new PieceOfData<int>(1);
            var stringData = new PieceOfData<String>("你好");
            Console.WriteLine("intData={0}", intData.Data);
            Console.WriteLine("stringData={0}", stringData.Data);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/b90e3f172faa40f6acaf8c24701fa0d5.png#pic_center)

（7） 泛型委托
![在这里插入图片描述](https://img-blog.csdnimg.cn/413723eca9514145aaf61da9919c7d3d.png#pic_center)
代码举例：

```csharp
using System;
using System.Collections.Generic;

namespace APP
{
    public delegate R MyDele<R, T>(T t);//定义一个泛型委托，返回值为R泛型类型，传入参数为T泛型类型
    public class Simple
    {
        public static string PrintInt1(int t)
        {
            string s1 = "Print1:" + t;
            Console.WriteLine(s1);
            return s1;
        }
        public static string PrintInt2(int t)
        {
            string s1 = "Print2:" + t;
            Console.WriteLine(s1);
            return s1;
        }
    }
    public class Program
    {
        public static void Main()
        {
            MyDele<string, int> myDele = new MyDele<string, int>(Simple.PrintInt1);
            myDele += Simple.PrintInt2;
           string s= myDele(3);//如果委托带有返回值，那么委托最后一个方法返回的值就是委托调用返回的值，因此在这行代码中，委托返回的是PrintInt2方法的返回值
            Console.WriteLine(s);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/dca64bfc89fe40fda2b47f034989d62a.png#pic_center)
（8）泛型接口：
代码示例1：泛型类继承泛型接口
```csharp
using System;
using System.Collections.Generic;

namespace APP
{
    public interface IBase<T>//泛型接口
    {
        T Show(T t);
    }
   public class sample<T> : IBase<T>//泛型类继承泛型接口
    {
        public T Show(T t)
        {
            return t;
        }
    }
    public class Program
    {
        public static void Main()
        {
            var intSample = new sample<int>();
            var strSample = new sample<string>();
            Console.WriteLine(intSample.Show(1));
            Console.WriteLine(strSample.Show("你好"));
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/7434617d52164253a3a7b132206f93e4.png#pic_center)
代码示例2：非泛型类继承泛型接口

```csharp
using System;
using System.Collections.Generic;

namespace APP
{
    public interface IBase<T>//泛型接口
    {
        T Show(T t);
    }
   public class sample:IBase<int>,IBase<string>//非泛型类继承泛型接口
    {
        public int Show(int t)
        {
            return t;
        }
        public string Show(string t)
        {
            return t;
        }
    }
    public class Program
    {
        public static void Main()
        {
            var Sample = new sample();
            Console.WriteLine(Sample.Show(1));
            Console.WriteLine(Sample.Show("你好"));
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2bf93eae33d3433a83270c5018db2a7a.png#pic_center)

### c#中的“\r”和“\n”问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/54e7df87da7b41b394b0cf0412256e8b.png#pic_center)
如下面一段代码：
```csharp
using System;
namespace APP
{
    class Program
    {
        static void Main()
        {
            Console.WriteLine("{0}", "12\n3678\r456");
            Console.WriteLine("{0}", "-------分隔------");
            Console.WriteLine("{0}", "12\\n\r456");
            Console.WriteLine("{0}", "12\\n456");
            Console.WriteLine("{0}", "\\");
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d5d9b174a4964e4f96f802000747ebc8.png#pic_center)
### 本地变量和字段是否会自动初始化？
本地变量是不会自动初始化的，而字段是会自动初始化的。
例如double rad是本地变量，如果没有赋值，就不会自动初始化；
字段则可以自动初始化。
### 属性相比公共字段的优点：
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ad0fb22f10c459ca54da3e18712ecb4.png#pic_center)
### 枚举器和迭代器
### LINQ
（1）LINQ(发音为link)表示语言集成查询；
（2）LINQ是.net框架的扩展，它允许我们可以使用SQL查询数据库的方式来查询数据集合；
（3）使用LINQ，可以从XML文档、数据库、程序对象的集合中查找数据；
（4）匿名类型经常用于LINQ查询的结果之中；
（5）如下是一段匿名类型的对象：
```csharp
using System;
using System.Linq;

namespace APP
{
    class Program
    {
        public static void Main()
        {
            var student = new { Name = "小明", Age = 19, Sex = "男" };//匿名类型的对象
            Console.WriteLine("{0},{1},{2}", student.Name, student.Age, student.Sex);
          }
    }
}
```
（6）form子句：
from子句指定了要作为数据源使用的数据集合。
```csharp
using System;
using System.Linq;

namespace APP
{
    class Program
    {
        public static void Main()
        {
            int[] arr= { 1, 3, 5, 7, 9 };
            var query = from item in arr //from子句指定了要作为数据源使用的数据集合
                        where item < 7
                        select item;
            foreach (var item in query)
            {
                Console.WriteLine("{0}, ", item);
            }

         }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/af05ba5963cd402b91cf43a9b9b9ae35.png#pic_center)
（7）join（联结）子句：
LINQ中的Join接受两个集合然后创建一个新的集合，每一个元素包含两个原始集合中的原始成员。
看一段代码：
```csharp
using System;
using System.Linq;

namespace APP
{
    public class Student //学生类
    {
        public int StID;//学生编号
        public string StName;//学生姓名
    }
    public class CourseStudent//学生所学习的课程类
    {
        public int StID;//学生编号
        public string CourseName;//4课程名称
    }
    class Program
    {
        static void Main()
        {
            Student[] students = new Student[3]
{
            new Student{ StID=1,StName="小王" },
            new Student{ StID=2,StName="小刘" },
            new Student{ StID=3,StName="小明" },
};
            CourseStudent[] courseStudentes = new CourseStudent[5]
            {
            new CourseStudent{ StID=1,CourseName="历史" },
            new CourseStudent{ StID=2,CourseName="哲学" },
            new CourseStudent{ StID=1,CourseName="音乐" },
            new CourseStudent{ StID=3,CourseName="历史" },
            new CourseStudent{ StID=3,CourseName="音乐" },
            };

            //查找了所有选取历史课的学生姓名
            var query = from s in students
                        join c in courseStudentes on s.StID equals c.StID
                        where c.CourseName == "历史"
                        select s.StName;
            foreach(var q in query)
            {
                Console.WriteLine("学习历史课的学生有：{0}", q);
            }

        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1ff347372ec4e5e9aa1189bc6cc847c.png#pic_center)
再看一段代码：

```csharp
using System;
using System.Linq;

namespace APP
{
    class Program
    {
        static void Main()
        {
            var GroupA = new int[] { 3, 4, 5, 6 };
            var GroupB = new int[] { 6, 7, 8, 9 };
            //查找了所有选取历史课的学生姓名
            var query = from int a in GroupA
                        from int b in GroupB
                        let sum = a + b
                        where sum >= 11//条件1
                        where a == 4//条件2
                        select new { a, b, sum };//匿名类型的对象
            foreach(var q in query)
            {
                Console.WriteLine(q);
            }

        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f3654c39257491f980a1d72b316d326.png#pic_center)
（8）orderby子句:
orderby子句接受一个表达式并根据表达式顺序返回结果项。

```csharp
using System;
using System.Linq;

namespace APP
{
    class Program
    {
        static void Main()
        {
            var students = new[]
            {
                new{Name="小王",Age=21},
                new{Name="小李",Age=25},
                new{Name="小刘",Age=11},
                new{Name="小张",Age=14},
                new{Name="小何",Age=19},
                new{Name="小陈",Age=23},
                new{Name="小罗",Age=27},
            };
            var query = from student in students
                        orderby student.Age//根据Age排序
                        select student;
            foreach(var q in query)
            {
                Console.WriteLine("姓名:{0},年龄:{1}", q.Name, q.Age);
            }
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/95f7dc84b3424c3a92d4f0809ac0556b.png#pic_center)
（9）select ...group子句:
![在这里插入图片描述](https://img-blog.csdnimg.cn/aef078a9f2c44df5add903c2ed0e19d5.png#pic_center)
(10)group子句：
group子句按照一些标准进行分组。
(11)查询延续：into子句：
into子句可以接受查询的一部分结果并赋予另外一个名字，从而可以在查询的另一部中使用。
![在这里插入图片描述](https://img-blog.csdnimg.cn/0630e516679d4da790f8e3568d99ebf7.png#pic_center)
（12）再看一段代码：

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Threading;

namespace APP
{
    class Program
    {
        static void Main()
        {
            int x = 10;
            int[] intArr = { 1, 2, 3, 4, 5, 6, 7 };

            Person person1 = new Person() { name = "One",age = 10 };
            Person person2 = new Person() { name = "Two", age = 20 };
            Person person3 = new Person() { name = "Three", age = 20 };

            List<Person> people = new List<Person>();
            people.Add(person1);
            people.Add(person2);
            people.Add(person3);

            var lst = people.AsEnumerable().Where((a) => a.age == 20);
            foreach (var item in lst)
            {
                Console.WriteLine("名称：{0}，年龄：{1}",item.name,item.age);
            }
        }

    }
   public  class Person{ public string name; public int age; }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f28534867b714379a58ea9d2d00ff741.png#pic_center)

（13）标准查询运算符：
![在这里插入图片描述](https://img-blog.csdnimg.cn/21255436dc6f48a1981b8b6aba690c89.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/899a085490b74f5383cb02d2c0279406.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/6d02fa0a2ec64978821d72767e96f427.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/60875820280e420eb00096327b34b50c.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/784eef7c020d46209c276d1e41e202c1.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/a04442a9e09445fe9fc63117413cb9b7.png#pic_center)
（14）再看一段代码：
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace APP
{
    public class Person
    {
        private string _name;
        private int _age;
        private string _sex;

        public string Name { get => _name; set => _name = value; }
        public int Age { get => _age; set => _age = value; }
        public string Sex { get => _sex; set => _sex = value; }
        //重写ToString方法
        public override string ToString()
        {
            string str = this.Name + "," + this.Age + "," + this.Sex;
            return str;
        }
    }
    public class Program
    {
        public static void Main()
        {
            List<Person> lists = new List<Person>();
            lists.Add(new Person() { Name = "s1", Age = 13, Sex = "男" });
            lists.Add(new Person() { Name = "s1", Age = 11, Sex = "男" });
            lists.Add(new Person() { Name = "s3", Age = 17, Sex = "女" });
            lists.Add(new Person() { Name = "s4", Age = 9, Sex = "男" });
            lists.Add(new Person() { Name = "s5", Age = 31, Sex = "男" });
            var lists2 = lists.AsEnumerable().OrderBy(item => item.Age);
            foreach(var item in lists2)
            {
                Console.WriteLine(item.ToString());
            }
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/80d15586b0fb4ca2bd827a6a2e14a043.png#pic_center)
### 异步编程
（1）进程是构成运行程序的资源的集合，这些资源包括虚地址空间、文件句柄和许多其他程序运行所需要的的东西。
（2）线程代表了真正的执行程序，一旦进程建立，系统会在Main方法的第一行语句处就开始线程的执行。
![在这里插入图片描述](https://img-blog.csdnimg.cn/5a39e6fc2b0c4d95b56c08f500cd6fb4.png#pic_center)
（3）同步方法：如果一个程序调用某个方法，等待其执行所有处理后才继续执行，那么就称这样的方法是同步的。
（4）C#中的async和await特性可以创建并使用异步方法，该特性由三种部分组成：
 1. 调用方法：该方法会用来调用异步方法，然会在异步方法执行其他任务的时候继续操作；
 2. 异步方法（async)：该方法异步执行其工作，然会立即返回到调用方法；
 3. await表达式：用在异步方法内部，一般和async配合来使用，await指明需要异步执行的任务。一个异步方法可以包含多个表达式，但是如果一个都不包含的话，编译器就会发出警告。
 代码举例：
```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        public static async Task Test()//异步方法
        {
            Console.WriteLine("Test方法开始执行");
                 //await指明需要异步执行的任务
            await Task.Run(() => Thread.Sleep(1000));//此处有await,因此程序执行到这里时，会跳出该Test方法，执行 Console.WriteLine("程序执行结束！");这一句代码 。
            Console.WriteLine("Test方法执行完毕");
        }
        static void Main(string[] args)
        {
            Console.WriteLine("程序开始执行！");
            Program.Test();
            Console.WriteLine("程序执行结束！");
            Console.ReadKey();
        }
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/5e3158a89a4f4e89bee19ca1b57d1b44.png#pic_center)
 （5） [异步编程](https://blog.csdn.net/zls365365/article/details/124601385?ops_request_misc=&request_id=&biz_id=102&utm_term=c?ops_request_misc=&request_id=&biz_id=102&utm_term=c&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-4-124601385.nonecase&spm=1018.2226.3001.4187#%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-4-124601385.nonecase)
### 异常
（1）try语句：
try语句用来指明因避免出现异常而被保护的代码片段，并在发生异常时提供代码处理异常。
![在这里插入图片描述](https://img-blog.csdnimg.cn/b8493429d10f4522ae380453b4e1603f.png#pic_center)
（2）try块后面必须跟catch块或finally块组合使用，不能单独使用
（3）代码举例：

```csharp
using System;
namespace Application
{
    class app
    {
        static void Main()
        {
            try
            {
                int[] nums = { 1, 2, 3, 4, 5 };
                Console.WriteLine("开始执行~");
                Console.WriteLine(nums[5]);
            }
            catch (IndexOutOfRangeException e)
            {
                Console.WriteLine("程序发生异常，数组越界了，请修正！");
                throw new Exception(); // 抛出异常
            }
            finally
            {
                Console.WriteLine("执行完毕~");
            }
        }
    }
}
```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0cec8d9a5d4340138ba7524474b32c28.png#pic_center)

### 预处理指令
（1）预处理指令指示编译器如何处理源代码。
（2）预处理指令：
![在这里插入图片描述](https://img-blog.csdnimg.cn/3148177a5d47497a8c45cd755ed1c885.png#pic_center)
（3）#define和#undef只能用在源文件的第一行，也就是任何C#代码之前使用，在C#代码开始后，#define和#undef就不能再使用。
### 反射和特性
（1）元数据：有关程序及其类型的数据称为元数据，它们保存在程序的程序集中。
（2）程序在运行时，可以查看其他程序集或其本身的元数据。一个运行的程序可以查看本身的元数据和其他程序的元数据的行为叫做反射。
（3）反射在System.Reflection命名空间中。
（4）Type类![在这里插入图片描述](https://img-blog.csdnimg.cn/a21a6be8c7a7498da3d0c2c4aea8dcf7.png#pic_center)
（5）特性：
特性是一种允许我们向程序的程序集增加元数据的语言结构。它是用于保存程序结构信息的某种特殊类型的类。
特性的目的就是告诉编译器把程序结构的某组元数据嵌入到程序集。
（6）Obsolete特性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/344d8d28e2f34584898cdaf15d7afac2.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c8717a5eff4447499fdd7d29f362376.png#pic_center)
（7）Conditional特性
（8）看一段代码，反射中SetValue和GetValue以及克隆的使用：

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Text;

namespace _8_02
{
   public class WaferBase
    {
        public string Name = "Wafer";
        public double Thickness = 0.1;
        public WaferBase clone()
        {
            WaferBase waferBase1 = new WaferBase();
            foreach (var item in this.GetType().GetFields())
            {
                item.SetValue(waferBase1, item.GetValue(this));
            }
            return waferBase1;
        }
    }
}

```

```csharp
using System;

namespace _8_02
{
        class app
        {
            static void Main()
            {
             WaferBase waferBase = new WaferBase();
             WaferBase  waferBaseClone= waferBase.clone();
            
             Console.WriteLine(waferBaseClone.Name);
             Console.WriteLine(waferBaseClone.Thickness);
            }
        }

}

```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f640cd78372d46258dab3fdc00404190.png#pic_center)
### 字符串
（1）字符串string类型的一些成员：![在这里插入图片描述](https://img-blog.csdnimg.cn/7ec2dcf9f0ca4e5092b8769dacd81286.png#pic_center)
（2）Spilt方法：
该方法很有用，该方法会将一个字符串分割成若干个子字符串，并将他们以数组的形式返回。将一组按照预定位置分隔字符串的分隔符传给Spilt方法，就可以指定如何处理输出数组中的空元素（当然，原始字符串依然不会改变）。代码举例如下：

```csharp
using System;
using System.Reflection;
namespace APP
{
    class Program
    {
        static void Main()
        {
            string str = "Hi there! this,is :a string!";
            char[] delimiters = { ',',':',' ','!'};
            string[] words = str.Split(delimiters, StringSplitOptions.RemoveEmptyEntries); //RemoveEmptyEntries表示返回值不包括含有空字符串的数组元素
            Console.WriteLine("单词个数是：{0}", words.Length);
            foreach(var item in words)
            {
                Console.WriteLine(item);
            }
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ee640b976304e43a87ceda8aa1322af.png#pic_center)
（3）StringBuilder类
StringBuilder类位于System.Text命名空间中，它可以帮助程序员动态、有效地产生字符串，并避免创建许多副本。代码举例如下：

```csharp
using System;
using System.Text;
namespace APP
{
    class Program
    {
        static void Main()
        {
            StringBuilder str = new StringBuilder("小西八！");
            Console.WriteLine(str.ToString());
            str.Replace("西八", "八嘎");//替换部分字符串
            Console.WriteLine(str);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bfa5c71f710c42cf9153b465a91c12d4.png#pic_center)
（4）把字符串解析为数据值
解析允许我们接受表示值的字符串，并转化为实际的值，通过Parse静态方法。如：

```csharp
using System;
using System.Text;
namespace APP
{
    class Program
    {
        static void Main()
        {
            string s1 = "24.3";
            string s2 = "12.4";
            double d1 = double.Parse(s1);//把字符串转化为double类型
            double d2 = double.Parse(s2);
            Console.WriteLine(d1 + d2);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c7ba34db65c94bf790adb321c11a5071.png#pic_center)
但是，Parse方法有一个缺点，当转换不成功（即不能把string类型转化为其他类型）时会抛出一个异常，而在编程中要尽量避免异常。TryParse方法可以解决这个问题。
![在这里插入图片描述](https://img-blog.csdnimg.cn/715d715ccbb34b03a72670343a3f4b3f.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/529faf86840346e0aace9455c9f01e3a.png#pic_center)
### List < T >
（1）List < T >是泛型集合，用法如下代码所示：

```csharp
using System;
using System.Collections.Generic;

namespace APP
{
    public class Person
    {
        private string _name;
        private int _age;
        private string _hobby;

        public string Name { get => _name; set => _name = value; }
        public int Age { get => _age; set => _age = value; }
        public string Hobby { get => _hobby; set => _hobby = value; }

        public string ShowMessage()
        {
            return "Name=" + Name + ",Age=" + Age + ",Hobby=" + Hobby;
        }

    }
    class Program
    {
        static void Main()
        {
            List <Person> list= new List<Person>();
            list.Add(new Person { Name = "小明", Age = 18, Hobby = "听音乐" });
            list.Add(new Person { Name="小刘",Age=14,Hobby="爬山"});
            list.Add(new Person { Name = "小王", Age = 19, Hobby = "游泳" });
            foreach(var item in list)
            {
                Console.WriteLine(item.ShowMessage());
            }
        }
     }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/49b752e4ad624b79b87892bf2b95b035.png#pic_center)
（2）List的一些常用属性和方法：
![在这里插入图片描述](https://img-blog.csdnimg.cn/53f127a4ef6f48e782bb4f0d1591de2f.png#pic_center)
（3）可以用List<T>中的AsEnumerable()方法+lambda表达式结合的语句进行一些操作，如下代码：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace practice7_25
{
    class Person
    {
        private string _name;
        private int _age;
        private int _id;

        public string Name { get => _name; set => _name = value; }
        public int Age { get => _age; set => _age = value; }
        public int ID { get => _id; set => _id = value; }
    }
  public class Program
    {
        static void Main(string[] args)
        {
            List<Person> lists = new List<Person>();
            lists.Add(new Person { Name="小明",Age=18,ID=5});
            lists.Add(new Person { Name = "小王", Age = 20, ID = 5 });
            lists.Add(new Person { Name = "小六", Age = 13, ID = 8 });
            lists.Add(new Person { Name = "小1", Age = 11, ID = 8 });
            lists.Add(new Person { Name = "小2", Age = 17, ID = 2 });
            int counts = lists.Count();
            Console.WriteLine("元素个数为：{0}", counts);
            var result = lists.AsEnumerable().OrderBy(p => p.ID);
            foreach (var item in result)
            {
                Console.WriteLine(item.Name + " " + item.Age + " " + item.ID);
            }
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a4cefc345dba40e1b900097b8b9cc2ce.png#pic_center)


### 方法重载和重写的区别
重写：当一个子类继承一父类，而子类中的方法与父类中的方法的名称，参数个数、类型都完全一致时，就称子类中的这个方法重写了父类中的方法。
重载：一个类中的方法与另一个方法同名，但是参数表不同，这种方法称之为重载方法。
### 简述c#中类(class)和结构(struct)的异同
**异**:(1)类型不同:
类是引用类型,在堆上分配地址;
结构是值类型,在栈上分配地址;
(2)继承性不同:
类:是完全可扩展的，也可以继承其他类和接口，自身也能被继承；
结构：不能从另外一个结构或者类继承，本身也不能被继承(但能够继承接口，方法和类继承接口一样)。
(3)内部结构不同：
类： 有默认的构造函数和析构函数，可以使用访问修饰符 ，必须使用new 初始化；
 结构： 没有默认的构造函数，但是可以添加构造函数，没有析构函数 ，可以不使用new 初始化。
**同**:基类型都是对象（object）
### 简述c#中接口和类的异同
**不同点**：
不能直接实例化接口。
接口不包含方法的实现。
接口可以多继承，类只能单继承。
类定义可以在不同的源文件之间进行拆分。
**相同点**：
接口、类都可以从多个接口继承。
接口类似于抽象基类：继承接口的任何非抽象类型都必须实现接口的所有成员。
接口和类都可以包含事件、索引器、属性。
### 一道C#编程测验题

```csharp
using System;
namespace APP
{
    public class student
    {
        public int age;
    }
    class Program
    {
        static void ChangeAge(student s)
        {
            s.age = 20;
        }
        static void ChangeAge_1(student s)
        {
            s = new student();
            s.age = 20;
        }

        static void ChangeAge_2(ref student s)
        {
            s = new student();
            s.age = 20;
        }
        static void ChangeAge_3(out student s)
        {
            s = new student();
            s.age = 20;
        }
        static void Main(string[] args)
        {
            student s = new student();
            s.age = 18;
            student s1 = new student();
            s1.age = 18;
            student s2 = new student();
            s2.age = 18;
            student s3 = new student();
            s3.age = 18;

            ChangeAge(s);
            ChangeAge_1(s1);
            ChangeAge_2(ref s2);
            ChangeAge_3(out s3);
            Console.WriteLine("student s age is {0}", s.age);
            Console.WriteLine("student s1 age is {0}", s1.age);
            Console.WriteLine("student s2 age is {0}", s2.age);
            Console.WriteLine("student s3 age is {0}", s3.age);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/226151d9c6ee4ea589893943f7462cbf.png#pic_center)
### C#中如何判断传入值的数据类型
（1）GetType方法：获取当前变量的类型对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/269e34b39d2d4e5591fd9bfa1eadccc7.png#pic_center)
（2）typeof运算符，在此笔记第41条已提到。
### C#中唯一的非派生类是Object类
因为C#中所有的类都直接或者间接派生自Object类，因此Object类类是C#中唯一的非派生类。
### C#转义字符的两种处理方式
当声明一个字符串变量时有一些字符是不能以平常的方式包含在变量中的。为了解决这个问题，C#提供了两种不同的C#转义字符方法:：
（1）第一种方法是使用\
![在这里插入图片描述](https://img-blog.csdnimg.cn/4663f09db2b240ec9b8c3f11eccf242c.png#pic_center)
代码举例一可参考笔记第56条；
代码举例二：
```csharp
            string str1 = "C:\\Program Files\\Microsoft\\";
            Console.WriteLine("str1的值是{0}:",str1);

            string str2 = "C:\nProgram Files\\nMicrosoft\\n";
            Console.WriteLine("str2的值是{0}:", str2);

            string str3 = "\"\n";
            Console.WriteLine("str3的值是{0}:", str3);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/c3ba3e6db41c45b581d184cc42a30361.png#pic_center)

**注意：当出现"\\\n"时，结果是显示的是\n，而不会出现换行**
（2）第二种C#转义字符方法是使用@
![在这里插入图片描述](https://img-blog.csdnimg.cn/22f4433d77d74457b548c877378315db.png#pic_center)
代码举例：
```csharp
            string str1 = "C:\\Program Files\\Microsoft\\";
            Console.WriteLine("str1的值是{0}:", str1);

            string str2 = @"C:\Program Files\Microsoft\";
            Console.WriteLine("str2的值是{0}:", str2);
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/0daf384e486f4192a3d2442127884a9d.png#pic_center)
### 抽象成员，哪些成员可以声明为抽象的，虚成员和抽象成员的区别
（1）抽象成员是抽象类中的成员，抽象成员是指设计为被覆写的函数成员，抽象成员具有以下特征：
 - 抽象成员必须是函数成员，字段和常量不能为抽象成员；
 - 抽象成员必须用Abstract标记；
 - 抽象成员不能实现代码块；
 - 抽象成员必须被子类用override关键字重写;
 例如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/04e741b1cf8e4ce5bd7a75222e416d93.png#pic_center)
（2）虚成员和抽象成员的区别：
抽象方法是只有方法名称，没有方法体（也就是没有方法具体实现），子类必须用override关键字重写父类抽象方法。
虚函数有方法体，但是子类可以覆盖，也可不覆盖。
### 构造函数重载、静态构造函数、构造函数分别什么时候执行？
先看代码展示：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace practice7_25
{
    public class A
    {
        static A()
        {
            Console.WriteLine("SA");
        }
        public A()
        {
            Console.WriteLine("baseA");
        }
        public virtual void Fun()
        {
            Console.WriteLine("A.Fun");
        }
    }
    public class B : A
    {
        static B()
        {
            Console.WriteLine("SB");
        }
        public B()
        {
            Console.WriteLine("baseB");
        }
        public override void Fun()
        {
            Console.WriteLine("B.Fun");
        }
    }
    public class Program
    {
        static void Main(string[] args)
        {
            A a = new A();
            a.Fun();
            Console.WriteLine("————————————————————————————————");
            A b = new B();
            b.Fun();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d7734c28203d49c9b95e54224f5c23be.png#pic_center)

十二字口诀：“**先静后构，静外到内，构内到外**”;
**先静后构：**
一个类同时存在静态构造函数和普通构造函数，对象实例化后，先执行静态构造函数（先静），再执行普通构造函数（后构）；
故上文对象A实例后输出的是SA baseA。

**静外到内：**
父类和子类都存在静态构造函数的时候，实例化子类后，先执行子类静态构造函数（静外），再执行父类静态构造函数（到内）；
上文B继承了A，B和A同时存在静态构造函数但是由于一个静态构造函数在一个应用程序的完整生命周期中，最多只会被自动执行一次，因此基类的静态构造函数只会在基类被初始化的时候调用且调用一次，所以实例化B后，静态构造函数输出结果是是SB ，而不是SB SA。

**构内到外：**
实例化子类后，先执行父类的普通构造函数（构内），再执行子类的构造函数（到外）；
上文B继承了A，实例化B后，普通构造函数输出结果是baseA baseB。

 - 对于构造函数重载，先执行基类的构造函数，再执行子类的构造函数;
 - 对于析构函数，先执行子类的析构函数，再执行基类的析构函数。
### 测验题：委托的创建和初始化
![在这里插入图片描述](https://img-blog.csdnimg.cn/6012af70786a47de8115e7706b2e52fb.png#pic_center)
```csharp
public class Program
    {
        public delegate int MyDelegate(int x);
        
        static void Main()
        {
            Program program = new Program();

            //以下这5种方式声明委托都是可以的：
            MyDelegate myDelegate1 = delegate (int x) { return x + 1; };
            MyDelegate myDelegate2 =(int x)=>{ return x + 1; };
            MyDelegate myDelegate3 = (x) => { return x + 1; };
            MyDelegate myDelegate4 = x=> { return x + 1; };
            MyDelegate myDelegate5 = x => x + 1;
            
            Console.WriteLine(myDelegate1(1));
            Console.WriteLine(myDelegate2(1));
            Console.WriteLine(myDelegate3(1));
            Console.WriteLine(myDelegate4(1));
            Console.WriteLine(myDelegate5(1));
        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac80b7a5374f45519ccca9071074d7e7.png#pic_center)
### 测验题：判断类型是否是隐式转换
隐式转换：类型的转换不会丢失数据或精度。
![在这里插入图片描述](https://img-blog.csdnimg.cn/0de14dac536d49b38d746bf317a3448c.png#pic_center)
```csharp
 public class Program
    {
        static void Main()
        {
            //doube转int
            double a1 = 2.3456;
            int a2 = (int)a1;
            Console.WriteLine("double转int: {0}---{1}", a1, a2);

            //float转double
            float b1 = 2.3456f;
            double b2 = (double)b1;
            Console.WriteLine("float转double: {0}---{1}", b1, b2);

            //unshort转int
            ushort c1 = 2;
            int c2 = (int)c1;
            Console.WriteLine("unshort转int: {0}---{1}", c1, c2);

            //int转long
            int d1 = 2;
            long d2 = (long)d1;
            Console.WriteLine("int转long: {0}---{1}", d1, d2);
        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/54ea40d9a932440e8bf4bac2ac06c13c.png#pic_center)
### LINQ的部分知识点考察

```csharp
   public class Program
    {
        static void Main()
        {
            int[] array1 = new[] { 3,2,5,33,56,43,4,5 };

            //Take是获取到第几个下标为止,取得的值不包括当前下标值的内容
            var array2 = array1.Take(5).Where(x => x > 20).ToList();//在本行代码中，array1.Take(5)返回的集合是：3,2,5,33,56，因此最终array2接受的只有33和56大于20
            foreach (var item in array2)
            {
                Console.WriteLine(item);
            }

        }
    }
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/683888afd2c24cdebe43f8af8da6133d.png#pic_center)
### 哪些成员可以在接口内声明，列举接口和抽象类的区别？
（1）可以在接口内声明的成员：方法、属性、事件、索引器。
（2）接口和抽象类的区别：
 1. 接口中不能声明常量和字段，抽象类中可以声明任何类成员；
 2. 在接口中只能定义成员，但不能具体实现，在抽象类中除了抽象方法外，其他成员允许有具体的实现；
 3. 接口中没有实例构造函数，也就是说没有构造函数，抽象类中有构造函数；
 4. 接口成员不能使用任何访问修饰符，抽象类中的类成员可以使用任意的访问修饰符；
 5. 继承接口的类或结构必须隐式或显式实现接口中的所有成员，否则需要将实现类定义为抽象类，并将接口中未实现的成员以抽象的方式实现，继承抽象类的类必须重写实现抽象类中的所有抽象方法，或者抽象类继承抽象类，可以重写部分抽象方法；
 6. 接口不能作为派生类继承，抽象类可以继承非抽象类或抽象类；
 7. 接口可以作为基类来多继承：接口、类和结构，抽象类可以作为基类只能实现单继承，只能让非抽象类或者抽象类继承。
### 什么叫显式转换，什么叫隐式转换？装箱和拆箱又是什么？
（1）显式转换：类型的转换可能会丢失数据或精度，语言不会自动做的转换需要用到显示转换。
（2）隐式转换：类型的转换不会丢失数据或精度，因此语言会自动给做的转换叫做隐式转换。
（3）装箱：值类型到引用类型的隐式转换。任何值类型都可以被隐式转换为object类型，System.ValueType或InterfaceT；
（4）拆箱：把装箱后的对象转换回值类型的过程。拆箱是显式转换。
### 进程和线程分别是什么？谈谈进程和线程的区别与联系。
（1）进程：构成运行程序的资源的集合。这些资源包括虚地址空间，文件句柄和许多其他程序运行所需的东西。
（2）线程：在进程的内部，系统创建了一个称为线程的内核对象，它代表了真正执行的程序。
（3）进程和线程的区别与联系：
1. 默认情况下，一个进程只包含一个线程，从程序的开始一直执行到结束；
2. 线程可以派生其他线程，因此在任意时刻，一个进程都可能包含不同状态的多个线程来执行程序的不同部分；
3. 如果一个进程拥有多个线程，他们将共享进程的资源；
系统为处理器执行所规划的单元是线程，不是进程。
### 一道c#试题
```csharp
using System;
namespace  day
{
    class day1
    {
        static void Main()
     {
        int n1=1,n2=2;
        bool re1=n1>n2 && n1++>1;
        Console.WriteLine(n1);
        //此处 false&&?,不管false与谁且，结果仍未fasle,所以，语句“n1++>1”并没有执行，所以此处n1的结果为1

        bool re2=n1<n2 || n2++<1;
        Console.WriteLine(n2);
        //此处true||?,不管true与谁或，结果仍未true,所以语句“n2++<1”并没有执行,所以此处n2结果为2
     }
    }
    
}
```
结果如图：n1的值为1，n2的值为2
![在这里插入图片描述](https://img-blog.csdnimg.cn/5dc0b3839f8b4fa3b7fd7ee7d72a99eb.png#pic_center)
### 使用CancellationTokenSource 取消多线程
CancellationTokenSource 用于取消多线程操作。
使用步骤：
（1）声明CancellationTokenSource 对象；
（2）实例化 CancellationTokenSource 对象，此对象管理取消通知并将其发送给单个取消标记。并进行注册回调事件；
 注意：想再次启动线程,必须重新再new CancellationTokenSource();因为取消了一次CancellationTokenSource.Cancel()，CancellationToken.IsCancellationRequested的标记一直为true;
（3）Cancel()方法调用会设置cancellationManage.IsCancellationRequested为True;
调用 CancellationTokenSource.Cancel 方法以提供取消通知。 这会将 CancellationToken.IsCancellationRequested 取消标记的每个副本上的属性设置为 true ；
代码举例：

```csharp
using System;
using System.Linq;
using System.Threading;

namespace practice01
{

    class Program
    {
        private static CancellationTokenSource cts;//1.声明CancellationTokenSource对象

        static void Main(string[] args)
        {
            while (true)
            {
                Console.WriteLine("1.开始运行多线程");
                Console.WriteLine("2.取消多线程");
                switch (Console.ReadLine())
                {
                    case "1":
                        BeginThread();
                        break;
                    case "2":
                        CancelThread();
                        break;
                    default:
                        break;
                }
            }

            // Console.ReadKey();
        }

        /// <summary>
        /// 回调停止方法
        /// </当多线程结束即调用Cancel方法后，将会调用此回调方法>
        private static void CallStopThread()
        {
            Console.WriteLine("回调停止方法");
        }

        /// <summary>
        /// 取消方法
        /// </summary>
        private static void CancelThread()
        {
            cts.Cancel();//4.Cancel()方法调用会设置IsCancellationRequested为True;
        }

        /// <summary>
        /// 开始运行线程方法
        /// </summary>
        private static void StartThread()
        {
            for (int i = 0; i < 1000000; i++)
            {
                if (cts.IsCancellationRequested) break;//如果为true，就跳出当前循环
                Console.WriteLine($"开始运行线程方法,线程ID：{Thread.CurrentThread.ManagedThreadId}");
                Thread.Sleep(1000);
            }
        }

        /// <summary>
        /// 开始运行线程
        /// </summary>
        private static void BeginThread()
        {
            Console.WriteLine("开始运行1方法");
            cts = new CancellationTokenSource(); //2.实例化 CancellationTokenSource 对象，此对象管理取消通知并将其发送给单个取消标记。
            cts.Token.Register(CallStopThread);//3.注册回调事件

            Thread[] ths = new Thread[3];
            for (int i = 0; i < ths.Count(); i++)
            {
                ths[i] = new Thread(StartThread);
                ths[i].Start();
            }
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/45cc100eb22f497d91787324e68c46b4.png#pic_center)
### C#中的单例模式与IOC
今天看项目代码，在IOC(控制反转)这部分知识这边卡了壳，问了下同事，终于理解了它的用法。
&emsp; 首先将一个类声明为单例模式下的类，然后将它放进IOC容器中，这样每次从IOC容器中取出的时候，就不用再重复实例化该类，减少内存的占用。
&emsp; 因为传统的实例化都是通过用new关键字实例化，当频繁地在不同文件中使用该类的时候，就可能需要不断地通过new来实例化，极大消耗了内存占用；而通过将单例模式下的类注入IOC容器后，每次用该类的时候可以通过IOC.Get方法取出该类（不管从IOC容器中取多少次，只会对该类进行一次实例化，不会重复实例化）。

### C# string.Join的用法
发现了string类中一个特别好用的方法string.Join，先直接看效果：
```csharp
int[] arrays = { 2, 8, 29, 19, 12, 13, 99, 89, 105, 108, 81 };
Console.WriteLine(string.Join(",", arrays));
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/75b9b34bf3ae4c42b6d16d9f79adae61.png#pic_center)
看出来了，string.Join(",", arrays)这部分代码将arrays数组中每个成员用","分隔开，方便快捷。
下面是该方法原型：
public static String Join<T>(String? separator, IEnumerable<T> values)方法
- 摘要:
// 串联集合的成员，其中在每个成员之间使用指定的分隔符。
-  参数:
// separator:
// 要用作分隔符的字符串。只有在 values 具有多个元素时，separator 才包括在返回的字符串中。
// values:
// 一个包含要串联的对象的集合。


### C#中using
#### 1.using指令
用法：using 命名空间
如：
```csharp
using System;
```
#### 2.using别名
用法：using 别名 = 包括详细命名空间信息的具体的类型。
如：
```csharp
using classA = namespaceA.MyClass;//给namespaceA下的MyClass起一个别名叫classA
using classB = namespaceB.MyClass;
```
#### 3.using语句
定义一个范围，在范围结束时处理（释放）对象。
注意：只有使用了IDisposible接口的对象才可以用using进行管理。
只在一定的范围内有效，出了这个范围时，自动调用IDisposable接口释放掉using语句块中的内容，当然并不是所有的类都适用，只有实现了IDisposable接口的类才可以使用。
应用场景：当在某个代码段中使用了类的实例，而希望无论因为什么原因，只要离开了这个代码段就自动调用这个类实例的Dispose。
如：
```csharp
using (Class1 cls1 = new Class1(), cls2 = new Class1())
{

}
```
### object.ReferenceEquals方法
在.net中，可以用object.ReferenceEquals比较两个对象是否是同一个对象，是同一个对象的话，返回结果为true，反之返回false。
如：
```csharp
 TestServiceImpI t= sp.GetService<TestServiceImpI>();
 TestServiceImpI t1 = sp.GetService<TestServiceImpI>();
 bool result= object.ReferenceEquals(t1, t);
 Console.WriteLine(result);
```
### 读取环境变量
用Environment.GetEnvironmentVariable方法读取环境变量，并返回一个string类型的结果。
### C#新语法：顶级语句（C#9.0）
1.使用vs2022新建一个.net core控制台应用，并选择.net6.0时会有一个是否选择使用顶级语句的按钮，如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/569f121d2fac4fea88fd78b4e60a9568.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd1512ca04024f2dac4721e97ebaa07a.png#pic_center)
那么什么才是顶级语句？
1. 直接在C#文件中直接编写入口方法的代码，不用类，不用Main。经典写法仍然支持。反编译一下了解真相。
2. 同一个项目中只能有一个文件具有顶级语句。
3. 顶级语句中可以直接使用await语法，也可以声明函数，如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/34bf1f6208b24a7989a7116e566f1905.png#pic_center)
### C#新语法：全局Using指令（C#10.0）
1.将 global 修饰符添加到 using 前，这个命名空间就应用到整个项目，不用重复using。
2.通常创建一个专门用来编写全局using代码的C#文件。
3.如果csproj中启用了<ImplicitUsings>enable</ImplicitUsings>，编译器会自动隐式增加对于System、System.Linq等常用命名空间的引入，不同各类型项目引入的命名空间也不一样。
### C#新语法:record类型（C#9.0）
1.C# 9.0 中引入了 record，record 是一个特殊类，本质上是Class类型。用它来实现 model 在有些情况下会非常的好用。
先看一段代码：
```csharp
record RecordPerson
{
    public string Name { get; init; }

    public int Age { get; init; }
}

record RecordPerson2(string Name, int Age);

public static void MainTest()
{
    var p1 = new RecordPerson()
    {
        Name = "Tom",
        Age = 12,
    };
    Console.WriteLine(p1);

    var p2 = p1 with { Age = 10 };
    Console.WriteLine(p2);

    var p3 = new RecordPerson() { Name = "Tom", Age = 12 };
    Console.WriteLine(p3);
    Console.WriteLine($"p1 Equals p3 =:{p1 == p3}");

    RecordPerson2 p4 = new("Tom", 12);
    Console.WriteLine(p4);
}
```
这里的示例，用 record 声明了两个 model，第二个 model 声明的时候使用了简化的写法record RecordPerson2(string Name, int Age); 这样的声明意味着，构造方法有两个参数，分别是 string Name 和 int Age，并对应着两个属性，属性的声明方式和 RecordPerson 一样 public string Name { get; init; } 都是一个 get 一个 init。
对于 record 支持一个 with 表达式，来修改某几个属性的值，这对于有很多属性都相同的场景来说是及其方便的，来看一下上面示例的输出结果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/c2097c45d8b84646905ea364ad3d2c0d.png#pic_center)
2.在C#9.0中增加了记录（record）类型的语法，编译器会为我们自动生成Equals、GetHashcode等方法。
```csharp
public record Person(string FirstName, string LastName);

Person p1 = new Person("Yang", "Zack");
Person p2 = new Person("Yang","Zack");
Person p3 = new Person("Gates", "Bill");
Console.WriteLine(p1);
Console.WriteLine(p1==p2);
Console.WriteLine(p1==p3);
Console.WriteLine(p1.FirstName);
```
### C#新语法:init（C#9.0）
init是C# 9.0中引入的新的访问器。
如果一个属性用get和init修饰，而不是get和set修饰，那么该属性只能在本类的构造函数中进行赋值，在本类的其他方法和其他类中均无法赋值。
### Sealed关键字
sealed的英文意思就是密封，禁止的意思，故名思义，就是由它修饰的类或方法将不能被继承或是重写。在c#中sealed关键字可以用来修饰类和方法。
#### 1.sealed关键字修饰类
当对一个类应用 sealed 修饰符时，此修饰符会阻止其他类从该类继承。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9b779b11d72543499b5e746679503054.png#pic_center)
如上图，sealed关键字修饰了类B，类B可以继承自类A，但是类C无法从类B继承。
#### 2.sealed关键字修饰方法或属性
当sealed修饰方法时,表示该方法不能被重写。
```csharp
    public class A
    {
        protected  virtual  void M()
        {
            Console.WriteLine("A.M()");
        }
        protected  virtual  void N()
        {
            Console.WriteLine("A.N()");
        }
    }
    public class B:A 
    {
        protected override void M()
        {
            Console.WriteLine("B.M()");
        }
        protected sealed override void N()
        {
            Console.WriteLine("B.N()");
        }
    }
    public sealed class C:B
    {
        protected override void M()
        {
            Console.WriteLine("C.M()");
        }
        protected override void N()  //会报错 ："C.N():"继承成员"B.N()"是密封的，无法进行重写
        {
            Console.WriteLine("C.N()"); 
        }
    }
```
### 异步编程中Task.Run 和 Task.Factory.StartNew 区别
可以认为Task.Run是简化的Task.Factory.StartNew 的使用，除了需要指定一个线程是长时间占用的，否则就使用 Task.Run。Task.Run方法是从线程池中调用一个新的线程，并不是调用Task.Run方法就一定会开辟一个新的线程；而Task.Factory.StartNew方法是开辟一个新的线程。两个方法实际上没有太大的区别，因此推荐使用Task.Run方法。
总的来讲，在需要设置线程是长时间运行的才需要使用 Task.Factory.StartNew 方法，不然就使用 Task.Run方法。
### C#中依赖注入的三种方式
#### 1.通过接口注入
**接口注入：**
相比构造函数注入和属性注入，接口注入显得有些复杂，使用也不常见。具体思路是先定义一个接口，包含一个设置依赖的方法。然后依赖类，继承并实现这个接口。

```csharp
using System;
namespace Example
{
    public interface IServiceClass
    {
        string ServiceInfo();
    }

    public class ServiceClassA : IServiceClass
    {
        public string ServiceInfo()
        {
            return "我是ServiceClassA";
        }
    }
    public class ServiceClassB : IServiceClass
    {
        public string ServiceInfo()
        {
            return "我是ServiceClassB";
        }
    }
    public interface IClinetClass
    {
        IServiceClass myServiceClass { get; set; }
    }

    public class ClientClass: IClinetClass
    {
        private IServiceClass _myServiceClass;
        public IServiceClass myServiceClass
        {
            get { return _myServiceClass; }
            set { _myServiceClass = value; }
        }
        public void Set_ServiceImpl(IServiceClass serviceClass)
        {
            this.myServiceClass = serviceClass;
        }
        public void ShowInfo()
        {
            Console.WriteLine(myServiceClass.ServiceInfo());
        }
    }

    public class Program
    {
        public static void Main()
        {
            IServiceClass serviceClassA = new ServiceClassA();
            IServiceClass serviceClassB = new ServiceClassB();
            ClientClass client = new ClientClass();

            client.Set_ServiceImpl(serviceClassA);
            client.ShowInfo();

            client.Set_ServiceImpl(serviceClassB);
            client.ShowInfo();
        }
    }
}
```

#### 2.通过属性访问器Settter注入
Setter注入（Setter Injection）是指在客户类中，设置一个服务类接口类型的数据成员，并设置一个Set方法作为注入点，这个Set方法接受一个具体的服务类实例为参数，并将它赋给服务类接口类型的数据成员。
举例：
1. UML图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/9de3f43a8f614366abea3e01f66ec604.png#pic_center)
2. 代码：

```csharp
using System;
namespace Example
{
    public interface IServiceClass
    {
        string ServiceInfo();
    }

    public class ServiceClassA : IServiceClass
    {
        public string ServiceInfo()
        {
            return "我是ServiceClassA";
        }
    }
    public class ServiceClassB : IServiceClass
    {
        public string ServiceInfo()
        {
            return "我是ServiceClassB";
        }
    }
    public class ClientClass
    {
        private IServiceClass _serviceImpl;
        public IServiceClass ServiceImpl
        {
            get { return _serviceImpl; }
            set { _serviceImpl = value; }
        }
        public void Set_ServiceImpl(IServiceClass serviceClass)
        {
            this.ServiceImpl = serviceClass;
        }
        public void ShowInfo()
        {
            Console.WriteLine(ServiceImpl.ServiceInfo());
        }
    }

    public class Program
    {
        public static void Main()
        {
            IServiceClass serviceClassA = new ServiceClassA();
            IServiceClass serviceClassB = new ServiceClassB();
            ClientClass client = new ClientClass();

            client.Set_ServiceImpl(serviceClassA);
            client.ShowInfo();

            client.Set_ServiceImpl(serviceClassB);
            client.ShowInfo();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/9d2e8969daa145d0b66e1ee8dfd5012f.png#pic_center)
#### 3.通过构造函数注入
构造函数注入：
通过客户类的构造函数，向客户类注入服务类实例。
构造注入（Constructor Injection）是指在客户类中，设置一个服务类接口类型的数据成员，并以构造函数为注入点，这个构造函数接受一个具体的服务类实例为参数，并将它赋给服务类接口类型的数据成员。
1. UML图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ad253eb36454d9dbffdbb28d0217f7c.png#pic_center)
2. 代码：
```csharp
using System;
namespace Example
{
    public interface IServiceClass
    {
        string ServiceInfo();
    }

    public class ServiceClassA : IServiceClass
    {
        public string ServiceInfo()
        {
            return "我是ServiceClassA";
        }
    }
    public class ServiceClassB : IServiceClass
    {
        public string ServiceInfo()
        {
            return "我是ServiceClassB";
        }
    }
    public class ClientClass
    {
        private IServiceClass _serviceImpl;
        public ClientClass(IServiceClass serviceClass)
        {
            this._serviceImpl = serviceClass;
        }
        public void ShowInfo()
        {
            Console.WriteLine(_serviceImpl.ServiceInfo());
        }
    }
    public class Program
    {
        public static void Main()
        {
            IServiceClass serviceClassA = new ServiceClassA();
            IServiceClass serviceClassB = new ServiceClassB();

            ClientClass client1 = new ClientClass(serviceClassA);
            client1.ShowInfo();

            ClientClass client2 = new ClientClass(serviceClassB);
            client2.ShowInfo();
        }    
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e8f2c797902842f0ab8cbd22a9010c14.png#pic_center)
### C#中yield关键字使用：yield return和yield break的作用
C#中，yield关键字的作用是将当前集合中的元素立即返回，只要没有yield break,方法还是会继续执行循环到迭代结束。
1. yield return是一次一个的返回，yield return例子：
```csharp
namespace yield关键字
{
    public class Program
    {
        public static IEnumerable<int> enumerableFuc()
        {
            yield return 1;
            yield return 2;
            yield return 3;
        }

        public static void Main()
        {
            foreach (int item in enumerableFuc())
            {
                Console.WriteLine(item);
            }
            Console.ReadKey();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bab39cdd19164669b05e7380cafc3b52.png#pic_center)
通过单步调试即可发现，enumerableFuc方法每次被调用就会返回一个数据，第一次调用enumerableFuc方法会返回1，第二次调用会返回2，第三次调用会返回3。
2. yield break用于结束返回（终止迭代），yield break例子：
```csharp
namespace yield关键字
{
    public class Program
    {
        public static IEnumerable<int> enumerableFuc()
        {
            yield return 1;
            yield return 2;
            yield break;
            yield return 3;
        }
        public static void Main()
        {
            foreach (int item in enumerableFuc())
            {
                Console.WriteLine(item);
            }
            Console.ReadKey();
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb0cb6a4f7e84c718b24156c29c4dbfb.png#pic_center)
通过此代码可以看出，此代码返回的结果是1和2，不是1、2和3，因为用了yield break，因此enumerableFuc方法中的第四行yield return 3这一行没有被执行。
### C#中的克隆
克隆比较常用，所以.NET在System命名空间中提供了ICloneable接口，其中就是唯一的一个方法Clone()；使用Colne方法可以完成浅拷贝，但有时候我们还需要完成深拷贝以拷贝类中的引用类型的属性。先来看引用拷贝、浅拷贝、深拷贝三者的区别：
区分 引用拷贝、浅拷贝和深拷贝：

* **引用拷贝**：只复制对象的地址，并不会创建一个新的对象；
* **浅拷贝**：浅拷贝会创建一个对象，并进行属性复制，但对于引用类型的属性，只会复制其对象地址；
* **深拷贝**：深拷贝会完全复制整个对象，包括引用类型的属性。

浅拷贝代码举例，代码如下：

```csharp
using System;

namespace PrototypePattern
{
    public class Person : ICloneable
    {
        private string _name="小明";
        public string Name
        {
            get { return _name; }
            set { _name = value; }
        }

        private string _sex="男";
        public string Sex
        {
            get { return _sex; }
            set { _sex = value; }
        }

        private int _age=18;
        public int Age
        {
            get { return _age; }
            set { _age = value; }
        }

        private List<string> _hobbies=new List<string>() { "唱歌","跳舞","做饭"};
        public List<string> Hobbies
        {
            get { return _hobbies; }
            set { _hobbies = value; }
        }

        public void Show()
        {
            string showHobbies = "";
            foreach (var item in Hobbies)
            {
                showHobbies += item.ToString();
            }
            Console.WriteLine("姓名:{0},性别:{1},年龄:{2},爱好:{3}", Name, Sex, Age, showHobbies);
        }
        //克隆信息
        public object Clone()
        {
            return (object)this.MemberwiseClone();//MemberwiseClone（）这个方法就是创建当前对象的浅表副本
        }
    }

    public class Client
    {
        public static void Main()
        {
            Person person1 = new Person();
            person1.Hobbies.Add("爬山");
            person1.Show();
            Person person2 = (Person)person1.Clone();//将person1的信息拷贝至person2上
            person2.Show();
        }
    }
}
```

输出结果：

```tex
姓名:小明,性别:男,年龄:18,爱好:唱歌跳舞做饭爬山
姓名:小明,性别:男,年龄:18,爱好:唱歌跳舞做饭爬山
```
上面Clone（）方法就是实现的接口的方法，用来克隆对象。
MemberwiseClone（）这个方法就是创建当前对象的浅表副本。

深拷贝例子：
针对上面代码进行修改，

```csharp
using System;

namespace PrototypePattern
{
    public class Person : ICloneable
    {
        private string _name="小明";
        public string Name
        {
            get { return _name; }
            set { _name = value; }
        }

        private string _sex="男";
        public string Sex
        {
            get { return _sex; }
            set { _sex = value; }
        }

        private int _age=18;
        public int Age
        {
            get { return _age; }
            set { _age = value; }
        }

        private List<string> _hobbies=new List<string>() { "唱歌","跳舞","做饭"};
        public List<string> Hobbies
        {
            get { return _hobbies; }
            set { _hobbies = value; }
        }

        public void Show()
        {
            string showHobbies = "";
            foreach (var item in Hobbies)
            {
                showHobbies += item.ToString();
            }
            Console.WriteLine("姓名:{0},性别:{1},年龄:{2},爱好:{3}", Name, Sex, Age, showHobbies);
        }
        //克隆信息
        public object Clone()
        {
            Person p= (Person)this.MemberwiseClone();//MemberwiseClone（）这个方法就是创建当前对象的浅表副本
            
            p.Hobbies = new List<string>();
            if (Hobbies != null)
            {
                foreach (var item in Hobbies)
                {
                    p.Hobbies.Add(item);
                }
            }
            return p;
        }
    }

    public class Client
    {
        public static void Main()
        {
            Person person1 = new Person();
            person1.Hobbies.Add("爬山");
            person1.Show();
            Person person2 = (Person)person1.Clone();//将person1的信息拷贝至person2上
            person2.Show();
        }
    }
}
```
输出结果：
```tex
姓名:小明,性别:男,年龄:18,爱好:唱歌跳舞做饭爬山
姓名:小明,性别:男,年龄:18,爱好:唱歌跳舞做饭爬山
```
**在C#中，string类型和数组类型也属于引用类型，而string类型比较特殊（具体可查资料），因此只需要手动克隆List类型的Hobbies属性即可实现深克隆。**
### 关于C#中序列化的一些小问题
关于序列化的详细知识可参考此篇文章[C# Xml进行序列化与反序列化](https://blog.csdn.net/hjssss/article/details/88718634)
1. 如果在一个类前加上[Serializable]标签，说明该类可以被序列化，比如：
```csharp
[Serializable]      // 表示该类可以被序列化
public class Person
{
.......
}
```
2. 如果在一个属性前加上 [XmlIgnore]这么一个标签，表示在使用Xml读写文件时，该属性将不会被序列化，比如：
```csharp
    [Serializable]      // 表示该类可以被序列化
    public class Person
    {
        private string _name;
        [XmlIgnore]
        public string Name//该Comment属性不会被序列化，因为在该属性前面加了[XmlIgnore]标签
        {
            get { return _name; }
            set { _name = value; }
        }

        private string _age;
        public string Age//该Age属性会被序列化
        {
            get { return _age; }
            set { _age = value; }
        }
    }
```
### Lazy（延迟初始化）的用法
#### 1.Lazy简介
通过Lazy关键字，我们可以声明某个对象为仅仅当第一次使用的时候，再初始化，如果一直没有调用，那就不初始化，省去了一部分不必要的开销，提升了效率，同时Lazy是天生线程安全的。
#### 2.应用场景
* 对象创建成本高且程序可能不会使用它;
* **对象创建成本高，且希望将其创建推迟到其他高成本操作完成后**。
  例如，假定程序在启动时加载多个对象实例，但是只需立即加载其中一部分。 可以通过推迟初始化不需要的对象，直到创建所需对象，提升程序的启动性能。
#### 3.Lazy基本用法
##### 用法1：构造时使用默认的初始化方式
在使用Lazy时，如果没有在构造函数中传入委托，则在首次访问值属性时，将会使用Activator.CreateInstance来创建类型的对象，如果此类型没有无参数的构造函数时将会引发运行时异常。

```csharp
using System;

namespace LazyUsage
{
    class LazyDemo
    {
        static void Main()
        {
            Lazy<Data> lazyData = new Lazy<Data>();
            Console.WriteLine("Main-lazyData被初始化了吗? value = " + lazyData.IsValueCreated);
            lazyData.Value.Print();//此处访问时才会将Data真正的初始化
            Console.WriteLine("Main-lazyData被初始化了吗? value = " + lazyData.IsValueCreated);

            Console.ReadKey();
        }
    }

    class Data
    {
        public Data()
        {
            Console.WriteLine("Data::.ctor->Initialized");
        }

        public void Print()
        {
            Console.WriteLine("Data::Print->println");
        }
    }
}

```
输出结果：

```bash
Main-lazyData被初始化了吗? value = False
Data::.ctor->Initialized
Data::Print->println
Main-lazyData被初始化了吗? value = True

```
##### 用法2：构造时使用指定的委托初始化

```csharp
using System;

namespace LazyUsage
{
    class LazyDemo
    {
        static void Main()
        {
            //指定委托来初始化Data
            Lazy<Data> lazyData = new Lazy<Data>(
                () =>
                {
                    Console.WriteLine("Main->lazyData将被初始化");
                    return new Data("Test");
                });
            Console.WriteLine("Main->lazyData被初始化了吗? value = " + lazyData.IsValueCreated);
            lazyData.Value.Print();
            Console.WriteLine("Main->lazyData被初始化了吗? value = " + lazyData.IsValueCreated);

            Console.ReadKey();
        }
    }

    class Data
    {
        public string Name { get; private set; }

        public Data(string name)
        {
            Name = name;
            Console.WriteLine("Data::.ctor->Initialized,name = " + name);
        }

        public void Print()
        {
            Console.WriteLine("Data::Print->name = " + Name);
        }
    }
}
```

```bash
Main->lazyData被初始化了吗? value = False
Main->lazyData将被初始化
Data::.ctor->Initialized,name = Test
Data::Print->name = Test
Main->lazyData被初始化了吗? value = True
```
### 字典：Dictionary
#### 1.何时使用Dictionary
当要存储的东西很多、列表很长时，可以使用Dictionary字典。
#### 2.关于使用Dictionary时的注意事项
* 字典Dictionary在名称空间System.Collections.Generic下；
* 字典是一组键（Key）到一组值（Value）的映射；
* 键必须是唯一的且不能为空；
* 键和值可以是任何数据类型
#### 3.Dictionary的一些简单用法
```csharp
using System.Runtime.CompilerServices;

namespace prop
{
    public class Practice
    {
        //创建一个字典
        private Dictionary<string,string> MyDict=new Dictionary<string,string>();
        public Practice()
        {
            //向字典中添加元素
            MyDict.Add("小明", "听音乐");
            MyDict.Add("小红", "跳舞");


            //检索键:方式1
            Console.WriteLine( MyDict["小明"]);
            Console.WriteLine(MyDict["小红"]);
            //检索键:方式2
            foreach (var k in MyDict.Keys)
            {
                Console.WriteLine(k);
            }

            //检索值
            foreach (var v in MyDict.Values)
            {
                Console.WriteLine(v);
            }

            //字典中的元素个数
            Console.WriteLine(MyDict.Count);

            //删除元素
            MyDict.Remove("小明");

            //判断字典中是否有这个键，返回布尔值
            bool keyResult = MyDict.ContainsKey("小明");

            //判断字典中是否有这个值，返回布尔值
            bool valueResult = MyDict.ContainsValue("小明");

            //当在字典中不能确定是否存在该键时需要使用TryGetValue，以减少一次不必要的查找，同时避免了判断Key值是否存在而引发的“给定关键字不在字典中。”的错误。（TryGetValue是根据ID返回相应的数据，如果没有ID则返回默认值）
             MyDict.TryGetValue("小明", out string tryResult);
        }
    }
}
```
### 字典：ConcurrentDictionary的使用
ConcurrentDictionary<TKey,TValue> 类表示可由多个线程同时访问的键/值对的线程安全集合。
也就是说，ConcurrentDictionary可由多个线程同时访问，并且线程安全。

对于c#中使用的List<T>,Dictionary<TKey, TValue>等常用的集合，如果需要在多线程中有写操作，会线程不安全，需要加锁（lock）。而ConcurrentDictionary无需手动加锁即可实现多线程中的写操作。

ConcurrentDictionary的用法和Dictionary类似。

### TaskCompletionSource的使用
MSDN链接：[添加链接描述](https://learn.microsoft.com/zh-cn/dotnet/standard/asynchronous-programming-patterns/)
#### 使用场景
TaskCompletionSource生成Task方法，使用TaskCompletionSource很简单，只需要实例化它即可。TaskCompletionSource有一个Task属性，可以对该属性暴露的task做操作，比如让它wait或者ContinueWith等操作。当然，这个task由TaskCompletionSource完全控制。

大多数时候，只在目标方法要调用基于事件API，又要返回Task的时候使用。比如下面的ApiWrapper方法，该方法要返回Task<string>,又要调用EventClass对象的Do方法，并且等到Do方法触发Done事件后，Task才能得到结果并返回。

```csharp

class CD_Ctor
{
   public static void Main()
    {
        var task = ApiWrapper(); 
        Console.WriteLine("Foo4:"+Thread.CurrentThread.ManagedThreadId);
        Console.WriteLine(task.Result);
    }

    public static Task<string> ApiWrapper()
    {
        var tcs=new TaskCompletionSource<string>();
        var api = new EventClass();
        api.Done += (args) => { tcs.TrySetResult(args); };
        api.Do();
        return tcs.Task;
    }

    public class EventClass
    {
        public Action<string> Done = (args) => { };
        public void Do()
        {
            Console.WriteLine("EventClass:"+Thread.CurrentThread.ManagedThreadId);
            Done("Done");
        }
    }
}

```

### 待学习：
* IList接口，IEnumerable接口，ICollection接口
* C#协程 yield return null
* 反射、特性
* 状态机
* CancellationTokenSource、CancellationToken
* (额外小知识) Git的基本概念，如：签入、签出、分支、拉取、推送等概念
* 一些常用的继承自Exception的子异常，在写代码时尽量抛出这些子异常，而不是Exception这样的所有异常的全称。
