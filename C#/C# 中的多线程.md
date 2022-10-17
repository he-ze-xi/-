@[TOC](目录)
# 前言
最近在看代码的过程中，发现有很多地方涉及到多线程、异步编程，这是比较重要且常用的知识点，而本人在这方面还理解尚浅，因此开始全面学习C#中的多线程，全文内容摘抄自一位前辈的网站：[网址链接](https://blog.gkarch.com/topic/threading.html)
本人在个别地方做了一些便于个人理解的修改。
![在这里插入图片描述](https://img-blog.csdnimg.cn/f3bd128b5090472c945fcb052e54296a.jpeg#pic_center)

## 1. 基础知识
### 1.简介及概念
C# 支持通过多线程并行执行代码，线程有其独立的执行路径，能够与其它线程同时执行。

一个 C# 客户端程序（Console 命令行、WPF 以及 Windows Forms）开始于一个单线程，这个线程（也称为“主线程”）是由 CLR 和操作系统自动创建的，并且也可以再创建其它线程。以下是一个简单的使用多线程的例子：

主线程创建了一个新线程t来不断打印字母 “ y “，与此同时，主线程在不停打印字母 “ x “。
```csharp
namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            Thread t = new Thread(WriteY);  // 创建新线程
            t.Start();                       // 启动新线程，执行WriteY()

            // 同时，在主线程做其它事情
            for (int i = 0; i < 1000; i++)
            {
                Console.Write("x");
            }
               
        }
        static void WriteY()
        {
            for (int i = 0; i < 1000; i++) Console.Write("y");
        }
    }
}
```
输出结果：

```csharp
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyxxyyyyyxxxxxxxxxxxxxxxyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

线程一旦启动，线程的IsAlive属性值就会为true，直到线程结束。当传递给Thread的构造方法的委托执行完成时，线程就会结束。一旦结束，该线程不能再重新启动。

CLR 为每个线程分配各自独立的栈空间，因此局部变量是独立的。在下面的例子中，我们定义一个拥有局部变量的方法，然后在主线程和新创建的线程中同时执行该方法。

```csharp

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            new Thread(Go).Start();      // 在新线程执行Go()
            Go();                         // 在主线程执行Go()
        }

        static void Go()
        {
            // 定义和使用局部变量 - 'cycles'
            for (int cycles = 0; cycles < 5; cycles++)
            {
                Console.Write("?");
            }
               
        }
    }
}
```
输出结果：

```csharp
??????????
```
变量cycles的副本是分别在各自的栈中创建的，因此才会输出 10 个问号。

线程可以通过对同一对象的引用来共享数据。例如：

```csharp

namespace App
{
    class ThreadTest
    {
        bool done;

        static void Main()
        {
            ThreadTest tt = new ThreadTest();   // 创建一个公共的实例
            new Thread(tt.Go).Start();
            tt.Go();
        }

        // 注意： Go现在是一个实例方法
        void Go()
        {
            if (!done)
            { 
                done = true;
                Console.WriteLine("Done");
            }
        }
    }
}
```
输出结果：

```csharp
Done 
```

由于两个线程是调用了同一个的ThreadTest实例上的Go()，它们共享了done字段，因此输出结果是一次 “ Done “，而不是两次。

像下面这种情况就会打印两次“Done”：

```csharp

namespace App
{
    class ThreadTest
    {
        bool done;

        static void Main()
        {
            ThreadTest tt = new ThreadTest();   // 创建一个公共的实例
            new Thread(new ThreadTest().Go).Start();
            tt.Go();
        }

        // 注意： Go现在是一个实例方法
        void Go()
        {
            if (!done)
            { 
                done = true;
                Console.WriteLine("Done");
            }
        }
    }
}
```
输出结果：

```csharp
Done
Done
```

静态字段提供了另一种在线程间共享数据的方式，以下是一个静态的done字段的例子：

```csharp

namespace App
{
    class ThreadTest
    {
        static bool done;    // 静态字段在所有线程中共享

        static void Main()
        {
            new Thread(Go).Start();
            Go();
        }

        static void Go()
        {
            if (!done)
            {
                done = true;
                Console.WriteLine("Done");
            }
         
        }
    }
}
```
输出结果：

```csharp
Done 
```

以上两个例子引出了一个关键概念线程安全（thread safety）。上述两个例子的输出实际上是不确定的：” Done “ 有可能会被打印两次。如果在Go方法里调换指令的顺序，” Done “ 被打印两次的几率会大幅提高：

```csharp

namespace App
{
    class ThreadTest
    {
        static bool done;    // 静态字段在所有线程中共享

        static void Main()
        {
            new Thread(Go).Start();
            Go();
        }

        static void Go()
        {
            if (!done)
            {
                Console.WriteLine("Done");
                done = true;       
            }
         
        }
    }
}
```
输出结果：

```csharp
Done
Done
```

这个问题是因为一个线程对if中的语句估值的时候，另一个线程正在执行WriteLine语句，这时done还没有被设置为true。

修复这个问题需要在读写公共字段时，获得一个排它锁（互斥锁，exclusive lock ）。C# 提供了lock来达到这个目的：

```csharp

namespace App
{
    class ThreadSafe
    {
        static bool done;
        static readonly object locker = new object();

        static void Main()
        {
            new Thread(Go).Start();
            Go();
        }

        static void Go()
        {
            lock (locker)
            {
                if (!done)
                { 
                    Console.WriteLine("Done");
                    done = true; 
                }
            }
        }
    }
}
```
输出结果：

```csharp
Done 
```
当两个线程同时争夺一个锁的时候（例子中的locker），一个线程等待，或者说阻塞，直到锁变为可用。这样就确保了在同一时刻只有一个线程能进入临界区（critical section，不允许并发执行的代码），所以 “ Done “ 只被打印了一次。像这种用来避免在多线程下的不确定性的方式被称为线程安全（thread-safe）。

**在线程间共享数据是造成多线程复杂、难以定位的错误的主要原因。尽管这通常是必须的，但应该尽可能保持简单。**

**一个线程被阻塞时，不会消耗 CPU 资源。**
#### 1.1Join 和 Sleep
可以通过调用Join方法来等待另一个线程结束，例如：

```csharp
namespace App
{
    class ThreadSafe
    {
        private static void Method()
        {
            Thread.Sleep(5000);
            Console.WriteLine("当前线程：" + Thread.CurrentThread.Name);
        }

        static void Main(string[] args)
        {
            Thread.CurrentThread.Name = "MainThread";

            Thread thread = new Thread(Method);
            thread.Name = "Thread";
            thread.Start();
            //会阻止主线程，直到thread线程终结（线程方法返回或线程遇到异常）
            //输出：当前线程：Thread
            //      主线程：MainThread
            //可以注销此句对比输出结果
            thread.Join();

            Console.WriteLine("主线程：" + Thread.CurrentThread.Name);

            Console.Read();
        }
    }
}
```
输出结果：

```csharp
当前线程：Thread
主线程：MainThread
```

程序运行后，会先等待5秒，然后打印“当前线程：Thread”，然后再打印：“主线程：MainThread”。如果把 thread.Join()注销后，会打印不一样的结果，代码如下：

```csharp

namespace App
{
    class ThreadSafe
    {
        private static void Method()
        {
            Thread.Sleep(5000);
            Console.WriteLine("当前线程：" + Thread.CurrentThread.Name);
        }

        static void Main(string[] args)
        {
            Thread.CurrentThread.Name = "MainThread";

            Thread thread = new Thread(Method);
            thread.Name = "Thread";
            thread.Start();
            //会阻止主线程，直到thread线程终结（线程方法返回或线程遇到异常）
            //输出：当前线程：Thread
            //      主线程：MainThread
            //可以注销此句对比输出结果
            //thread.Join();

            Console.WriteLine("主线程：" + Thread.CurrentThread.Name);

            Console.Read();
        }
    }
}
```
输出结果：

```csharp
主线程：MainThread
当前线程：Thread
```
程序运行后，会先打印：“主线程：MainThread”，然后等待5秒，然后打印“当前线程：Thread”。


Thread.Sleep会将当前的线程阻塞一段时间：

```csharp
Thread.Sleep (TimeSpan.FromHours (1));  // 阻塞 1小时
Thread.Sleep (500);                     // 阻塞 500 毫秒
```

当使用Sleep或Join等待时，线程是阻塞（blocked）状态，因此不会消耗 CPU 资源。

Thread.Sleep(0)会立即释放当前的时间片，将 CPU 资源出让给其它线程。Framework 4.0 新的Thread.Yield()方法与其相同，除了它只会出让给运行在相同处理器核心上的其它线程。

Sleep(0)和Yield在调整代码性能时偶尔有用，它也是一个很好的诊断工具，可以用于找出线程安全（thread safety）的问题。如果在你代码的任意位置插入Thread.Yield()会影响到程序，基本可以确定存在 bug。

#### 1.2线程是如何工作的
线程在内部由一个线程调度器（thread scheduler）管理，一般 CLR 会把这个任务交给操作系统完成。线程调度器确保所有活动的线程能够分配到适当的执行时间，并且保证那些处于等待或阻塞状态（例如，等待排它锁或者用户输入）的线程不消耗CPU时间。

在单核计算机上，线程调度器会进行时间切片（time-slicing），快速的在活动线程中切换执行。在 Windows 操作系统上，一个时间片通常在十几毫秒（译者注：默认 15.625ms），远大于 CPU 在线程间进行上下文切换的开销（通常在几微秒区间）。

在多核计算机上，多线程的实现是混合了时间切片和真实的并发，不同的线程同时运行在不同的 CPU 核心上。几乎可以肯定仍然会使用到时间切片，因为操作系统除了要调度其它的应用，还需要调度自身的线程。

线程的执行由于外部因素（比如时间切片）被中断称为被抢占（preempted）。在大多数情况下，线程无法控制其在何时及在什么代码处被抢占。

#### 1.3线程 vs 进程
好比多个进程并行在计算机上执行，多个线程是在一个进程中并行执行。进程是完全隔离的，而线程是在一定程度上隔离。一般的，线程与运行在相同程序中的其它线程共享堆内存。这就是线程为何有用的部分原因，一个线程可以在后台获取数据，而另一个线程可以同时显示已获取到的数据。

#### 1.4线程的使用与误用
多线程有许多用处，下面是通常的应用场景：

* 维持用户界面的响应
使用工作线程并行运行时间消耗大的任务，这样主UI线程就仍然可以响应键盘、鼠标的事件。

* 有效利用 CPU
多线程在一个线程等待其它计算机或硬件设备响应时非常有用。当一个线程在执行任务时被阻塞，其它线程就可以利用这个空闲出来的CPU核心。

* 并行计算
在多核心或多处理器的计算机上，计算密集型的代码如果通过分治策略（divide-and-conquer，见第 5 部分）将工作量分摊到多个线程，就可以提高计算速度。

* 推测执行（speculative execution）
在多核心的计算机上，有时可以通过推测之后需要被执行的工作，提前执行它们来提高性能。LINQPad就使用了这个技术来加速新查询的创建。另一种方式就是可以多线程并行运行解决相同问题的不同算法，因为预先不知道哪个算法更好，这样做就可以尽早获得结果。

* 允许同时处理请求
在服务端，客户端请求可能同时到达，因此需要并行处理（如果你使用 ASP.NET、WCF、Web Services 或者 Remoting，.NET Framework 会自动创建线程）。这在客户端同样有用，例如处理 P2P 网络连接，或是处理来自用户的多个请求。

**多线程同样也会带来缺点，最大的问题是它提高了程序的复杂度。使用多个线程本身并不复杂，复杂的是线程间的交互（一般是通过共享数据）。无论线程间的交互是否有意为之，都会带来较长的开发周期，以及带来间歇的、难以重现的 bug。因此，最好保证线程间的交互尽量少，并坚持简单和已被证明的多线程交互设计。这篇文章主要就是关于如何处理这种复杂的问题，如果能够移除线程间交互，那会轻松许多。**

**一个好的策略是把多线程逻辑使用可重用的类封装，以便于独立的检验和测试。.NET Framework 提供了许多高层的线程构造，之后会讲到。**

当频繁地调度和切换线程时（并且如果活动线程数量大于 CPU 核心数），多线程会增加资源和 CPU 的开销，线程的创建和销毁也会增加开销。多线程并不总是能提升程序的运行速度，如果使用不当，反而可能降低速度。 例如，当需要进行大量的磁盘 I/O 时，几个工作线程顺序执行可能会比 10 个线程同时执行要快。（在使用 Wait 和 Pulse 进行同步中，将会描述如何实现 生产者 / 消费者队列，它提供了上述功能。）
### 2.创建和启动线程
像我们在简介中看到的那样，使用Thread类的构造方法来创建线程，通过传递ThreadStart委托来指明线程从哪里开始运行，下面是ThreadStart委托的定义：
```csharp
public delegate void ThreadStart();
```
调用Start方法后，线程开始执行，直到它所执行的方法返回后，线程终止。下面这个例子使用完整的 C# 语法创建TheadStart委托：

```csharp
class ThreadTest
{
  static void Main()
  {
    Thread t = new Thread (new ThreadStart (Go));
    t.Start();   // 在新线程运行 GO()
    Go();        // 同时在主线程运行 GO()
  }

  static void Go()
  {
    Console.WriteLine ("hello!");
  }
}
```
输出结果：

```csharp
hello!
hello!
```
在这个例子中，线程t执行Go()方法，几乎同时主线程也执行Go()方法，结果将打印两个 hello。

线程也可以使用更简洁的语法创建，使用方法组（method group），让 C# 编译器推断ThreadStart委托类型：

```csharp
Thread t = new Thread (Go);    // 无需显式使用 ThreadStart
```

另一个快捷的方式是使用 lambda 表达式或者匿名方法：

```csharp
static void Main()
{
  Thread t = new Thread ( () => Console.WriteLine ("Hello!") );
  t.Start();
}
```
#### 2.1向线程传递数据
向一个线程的目标方法传递参数最简单的方式是使用 lambda 表达式调用目标方法，在表达式内指定参数:

```csharp

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            Thread t = new Thread(() => Print("Hello from t!"));
            t.Start();
        }

        static void Print(string message)
        {
            Console.WriteLine(message);
        }
    }
}
```
输出结果：

```csharp
Hello from t!
```
使用这种方式，可以向方法传递任意数量的参数。甚至可以将整个实现封装为一个多语句的 lambda 表达式：

```csharp
new Thread (() =>
{
  Console.WriteLine ("I'm running on another thread!");
  Console.WriteLine ("This is so easy!");
}).Start();
```
另一个方法是向Thread的Start方法传递参数：

```csharp

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            Thread t = new Thread(Print);
            t.Start("Hello from t!");
        }

        static void Print(object messageObj)
        {
            string message = (string)messageObj;    // 需要强制类型转换
            Console.WriteLine(message);
        }
    }
}
```
输出结果：

```csharp
Hello from t!
```
可以这样是因为Thread的构造方法通过重载来接受两个委托中的任意一个：

```csharp
public delegate void ThreadStart();
public delegate void ParameterizedThreadStart (object obj);
```
ParameterizedThreadStart的限制是它只接受一个参数。并且由于它是object类型，通常需要类型转换。

**Lambda 表达式与被捕获变量：**   如我们所见，lambda 表达式是向线程传递数据的最强大的方法。然而必须小心，不要在启动线程之后误修改被捕获变量（captured variables）。例如，考虑下面的例子：

```csharp

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            for (int i = 0; i < 10; i++)
            {
                new Thread(() => Console.Write(i)).Start();
            }      
        }
    }
}
```
输出结果是不确定的！可能是这样0223557799。

问题在于变量i在整个循环中指向相同的内存地址。所以，每一个线程在调用Console.Write时，都在使用这个值在运行时会被改变的变量！

解决方法就是使用临时变量，如下所示：

```csharp

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            for (int i = 0; i < 10; i++)
            {
                // new Thread(() => Console.Write(i)).Start();
                var temp = i;
                new Thread(() => { Console.WriteLine(temp); }).Start();
            }      
        }
    }
}
```
输出结果：

```csharp
0
1
2
3
4
5
6
7
8
9
```

变量temp对于每一个循环迭代是局部的。所以，每一个线程会捕获一个不同的内存地址，从而不会产生问题。我们可以使用更为简单的代码来演示前面的问题：

```csharp

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            string text = "t1";
            Thread t1 = new Thread(() => Console.WriteLine(text));

            text = "t2";
            Thread t2 = new Thread(() => Console.WriteLine(text));

            t1.Start();
            t2.Start();
        }
    }
}
```
输出结果：

```csharp
t2
t2
```
#### 2.2线程命名
每一个线程都有一个 Name 属性，我们可以设置它以便于调试。这在 Visual Studio 中非常有用，因为线程的名字会显示在线程窗口（Threads Window）与调试位置（Debug Location）工具栏上。线程的名字只能设置一次，以后尝试修改会抛出异常。

静态的Thread.CurrentThread属性会返回当前执行的线程。在下面的例子中，我们设置主线程的名字：

```csharp
namespace App
{
    class ThreadNaming
    {
        static void Main()
        {
            Thread.CurrentThread.Name = "main";
            Thread worker = new Thread(Go);
            worker.Name = "worker";
            worker.Start();
            Go();
        }

        static void Go()
        {
            Console.WriteLine("Hello from " + Thread.CurrentThread.Name);
        }
    }
}
```
输出结果也不是唯一的，有两种，具体要看系统对线程的调度情况：
结果1：

```csharp
Hello from main
Hello from worker
```
结果2：

```csharp
Hello from worker
Hello from main
```
#### 2.3前台与后台线程
默认情况下，显式创建的线程都是前台线程（foreground threads）。只要有一个前台线程在运行，程序就可以保持存活，而后台线程（background threads）并不能保持程序存活。当一个程序中所有前台线程停止运行时，仍在运行的所有后台线程会被强制终止。

**线程的前台/后台状态与它的优先级和执行时间的分配无关。**

可以通过线程的IsBackground属性来查询或修改线程的前后台状态。如下面的例子：

```csharp
class PriorityTest
{
  static void Main (string[] args)
  {
    Thread worker = new Thread ( () => Console.ReadLine() );
    if (args.Length > 0) worker.IsBackground = true;//IsBackground = true表示把该线程设置为后台线程
    worker.Start();
  }
}
```
如果这个程序以无参数的形式运行，工作线程会默认为前台，并在ReadLine时等待用户输入回车。此时主线程退出，但是程序仍然在运行，因为有一个前台线程依然存活。

相反，如果给Main()传递了参数，工作线程设置为后台状态，当主线程结束时，程序几乎立即退出（终止ReadLine需要一咪咪时间）。

当进程以这种方式结束时，后台线程执行栈中所有finally块就会被避开。如果程序依赖finally（或是using）块来执行清理工作，例如释放资源或是删除临时文件，就可能会产生问题。为了避免这种问题，在退出程序时可以显式的等待这些后台线程结束。有两种方法可以实现：

* 如果是自己创建的线程，在线程上调用Join方法。
* 如果是使用线程池线程，使用事件等待句柄。

**在任一种情况下，都应指定一个超时时间，从而可以放弃由于某种原因而无法正常结束的线程。这是后备的退出策略：我们希望程序最后可以关闭，而不是让用户去开任务管理器**。

**如果用户使用任务管理器强行终止了 .NET 进程，所有线程都会被当作后台线程一般丢弃。这是通过观察得出的结论，并不是通过文档，而且可能会因为 CLR 和操作系统的版本不同而有不同的行为。**

前台线程不需要这种处理，但是必须小心避免会使线程无法结束的 bug。程序无法正常退出的一个很有可能的原因就是仍有前台线程存在。

#### 2.4线程优先级
线程的Priority属性决定了相对于操作系统中的其它活动线程，它可以获得多少执行时间。线程优先级的取值如下：

```csharp
enum ThreadPriority { Lowest, BelowNormal, Normal, AboveNormal, Highest }
```
只有当多个线程同时活动时，线程优先级才有意义。

**在提升线程优先级前请三思，这可能会导致其它线程的资源饥饿（resource starvation，译者注：指没有分配到足够的CPU时间来运行）等问题。**

提升线程的优先级是无法使其能够处理实时任务的，因为它还受到程序进程优先级的影响。要进行实时任务，必须同时使用System.Diagnostics中的Process类来提升进程的优先级（记得这不是我告诉你的）：

```csharp
using (Process p = Process.GetCurrentProcess())
  p.PriorityClass = ProcessPriorityClass.High;
```
ProcessPriorityClass.High实际上就是一个略低于最高优先级Realtime的级别。将一个进程的优先级设置为Realtime是通知操作系统，我们绝不希望该进程将 CPU 时间出让给其它进程。如果你的程序误入一个无限循环，会发现甚至是操作系统也被锁住了，就只好去按电源按钮了o(>_<)o　正是由于这一原因，High 通常是实时程序的最好选择。

**如果实时程序拥有用户界面，提升进程的优先级会导致大量的 CPU 时间被用于屏幕更新，这会降低整台机器的速度（特别是当 UI 很复杂时）。降低主线程的优先级，并提升进程的优先级可以保证需要进行实时任务的工作线程不会被屏幕重绘所抢占。但是这依然没有解决其它程序的CPU时间饥饿的问题，因为操作系统依然为这个进程分配了大量 CPU 资源。
理想的解决方案是分离 UI 线程和实时工作线程，使用两个进程分别运行。这样就可以分别设置各自的进程优先级，彼此之间通过 Remoting 或是内存映射文件进行通信。内存映射文件十分适用于这一任务，在C# 4.0 in a Nutshell的第 14 和 25 章会讲到。**

即使是提升了进程优先级，托管环境在处理强实时需求时仍然有限制。除了自动垃圾回收带来的延迟，操作系统也不能够完全满足实时任务的需要，即便是非托管的程序也是如此。最好的解决办法还是使用独立的硬件或者专门的实时平台。

#### 2.5异常处理
当线程开始运行后，其创建代码所在的try / catch / finally块与该线程不再有任何关系。考虑下面的程序：

```csharp
using System.Diagnostics;

namespace App
{
    class ThreadTest
    {
        public static void Main()
        {
            try
            {
                new Thread(Go).Start();
            }
            catch (Exception ex)
            {
                // 永远执行不到这里
                Console.WriteLine("Exception!");
            }
        }

        static void Go() 
        { 
            throw null ;// 产生 NullReferenceException 异常
        }   
    }
}
```

这个例子中的try / catch语句是无效的，而新创建的线程将会遇到一个未处理的NullReferenceException。当你考虑到每一个线程具有独立的执行路径时，这种行为就可以理解了。

修改方法是将异常处理移到Go方法中：

```csharp
using System.Diagnostics;

namespace App
{
    class ThreadTest
    {
        public static void Main()
        {
            new Thread(Go).Start();
        }

        static void Go()
        {
            try
            {
                // ...
                throw null;    // 异常会在下面被捕获
                               // ...
            }
            catch (Exception ex)
            {
                // 一般会记录异常， 和/或通知其它线程我们遇到问题了
                // ...
            }
        }
    }
}
```
在生产环境的程序中，所有线程的入口方法处都应该有一个异常处理器，就如同在主线程所做的那样（一般可能是在执行栈上靠近入口的地方）。未处理的异常会使得整个程序停止运行，弹出一个难看的对话框。

**在写异常处理块的时候，最好不要忽略错误。一般应该记录异常详细信息，然后可以弹出一个对话框让用户可以选择是否自动把这些信息提交到你的服务器。最后应该关闭程序，因为很可能错误已经破坏的程序的状态。然而这么做会导致用户丢失当前的工作，比如打开的文档。**

**WPF 和 Windows Forms 应用中的“全局”异常处理事件（Application.DispatcherUnhandledException和Application.ThreadException）只会在主UI线程有未处理的异常时触发。对于工作线程上的异常仍然需要手动处理。**

AppDomain.CurrentDomain.UnhandledException会对所有未处理的异常触发，但是它不提供阻止程序退出的办法。

然而在某些情况下，可以不必处理工作线程上的异常，因为 .NET Framework 会为你处理。这些会在接下来的内容中讲到：

* 异步委托
* BackgroundWorker
* 任务并行库（TPL）

### 3.线程池
当启动一个线程时，会有几百毫秒的时间花费在准备一些额外的资源上，例如一个新的私有局部变量栈这样的事情。每个线程会占用（默认情况下）1MB 内存。线程池（thread pool）可以通过共享与回收线程来减轻这些开销，允许多线程应用在很小的粒度上而没有性能损失。在多核心处理器以分治（divide-and-conquer）的风格执行计算密集代码时将会十分有用。

线程池会限制其同时运行的工作线程总数。太多的活动线程会加重操作系统的管理负担，也会降低 CPU 缓存的效果。一旦达到数量限制，任务就会进行排队，等待一个任务完成后才会启动另一个。这使得程序任意并发成为可能，例如 web 服务器。（异步方法模式（asynchronous method pattern）是进一步高效利用线程池的高级技术，我们在C# 4.0 in a Nutshell的 23 章来讲）。

有多种方法可以使用线程池：

* 通过任务并行库（TPL）（Framework 4.0 中加入）
* 调用ThreadPool.QueueUserWorkItem
* 通过异步委托
* 通过BackgroundWorker

以下构造会间接使用线程池：

* WCF、Remoting、ASP.NET 和 ASMX 网络服务应用
* System.Timers.Timer 和 System.Threading.Timer
* .NET Framework 中名字以 Async 结尾的方法，例如WebClient上的方法（使用* 基于事件的异步模式，EAP），和大部分BeginXXX方法（异步编程模型模式，APM）
* PLINQ

任务并行库（Task Parallel Library，TPL）与 PLINQ 足够强大并面向高层，即使使用线程池并不重要，也应该使用它们来辅助多线程。我们会在第 5 部分中进行更详细的讨论。现在，简单看一下如何使用Task类作为在线程池线程上运行委托的简单方法。

在使用线程池线程时有几点需要小心：

* **无法设置线程池线程的Name属性，这会令调试更为困难（当然，调试时也可以在 Visual Studio 的线程窗口中给线程附加备注）**。
* **线程池线程永远是后台线程（一般不是问题）**。
* **阻塞线程池线程可能会在程序早期带来额外的延迟，除非调用了ThreadPool.SetMinThreads（见优化线程池）**。
* **可以改变线程池线程的优先级，当它用完后返回线程池时会被恢复到默认状态**。

**可以通过Thread.CurrentThread.IsThreadPoolThread属性来查询当前是否运行在线程池线程上**。

#### 3.1通过 TPL 使用线程池
可以很容易的使用任务并行库（Task Parallel Library，TPL）中的Task类来使用线程池。Task类在 Framework 4.0 时被加入：如果你熟悉旧式的构造，可以将非泛型的Task类看作ThreadPool.QueueUserWorkItem的替代，而泛型的Task<TResult>看作异步委托的替代。比起旧式的构造，新式的构造会更快速，更方便，并且更灵活。

要使用非泛型的Task类，调用Task.Factory.StartNew，并传递目标方法的委托：

```csharp
using System.Diagnostics;

namespace App
{
    class ThreadTest
    {
        static void Main()    // Task 类在 System.Threading.Tasks 命名空间中
        {
            Task.Factory.StartNew(Go);
            Console.ReadKey();
        }

        static void Go()
        {
            Console.WriteLine("Hello from the thread pool!");
        }
    }
}
```
输出结果：

```csharp
Hello from the thread pool!
```
**Task.Factory.StartNew返回一个Task对象，可以用来监视任务，例如通过调用Wait方法来等待其结束**。

**当调用Task的Wait方法时，所有未处理的异常会在宿主线程被重新抛出。（如果不调用Wait而是丢弃不管，未处理的异常会像普通的线程那样结束程序。（译者注：在 .NET 4.5 中，为了支持基于async / await的异步模式，Task中这种“未观察”的异常默认会被忽略，而不会导致程序结束。））**

泛型的Task<TResult>类是非泛型Task的子类。它可以使你在其完成执行后得到一个返回值。在下面的例子中，我们使用Task<TResult>来下载一个网页：

```csharp
using System.Diagnostics;

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            // 启动 task：
            Task<string> task = Task.Factory.StartNew<string>
              (() => DownloadString("http://www.gkarch.com"));

            // 执行其它工作，它会和 task 并行执行：
            RunSomeOtherMethod();

            // 通过 Result 属性获取返回值：
            // 如果仍在执行中, 当前进程会阻塞等待直到 task 结束：
            string result = task.Result;
            Console.WriteLine(result);
        }

        static string DownloadString(string uri)
        {
            using (var wc = new System.Net.WebClient())
            {
                return wc.DownloadString(uri);
            }
           
        }

        static void RunSomeOtherMethod()
        {
            for (int i = 0; i < 3; i++)
            {
                Console.Write($"第{i}次输出,");
            }
        }
    }
}
```
输出结果：

```csharp
第0次输出,第1次输出,第2次输出,

<!DOCTYPE HTML>
<html lang="en-US">
<head>
  <meta charset="UTF-8">

  <title>GKarch - 致力于提高软件生产力</title>

  <meta name="keywords" content="GKarch,Glacier,BaoBros,.NET,软件框架,软件设计,组件">
  <meta name="Description" content="GKarch设计和构建软件框架及工具，为软件的设计和开发奠定基础。Glacier框架作为核心，提 供先进的低耦合、高扩展性模式基础设施，提高软件质量，降低开发成本。同时也提供常用功能框架以及企业工具、服务，致力于将更高的软件生产力带给企业和开发者。">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <link rel="alternate" type="application/rss+xml" title="GKarch新闻" href="/feed.xml">
  <link rel="stylesheet" href="//s.gkarch.com/web/css/screen.css">
  <link rel="stylesheet" href="//s.gkarch.com/web/css/font-awesome.min.css">

    <link rel="stylesheet" type="text/css" href="//s.gkarch.com/web/css/slick.css"/>
    <link rel="stylesheet" type="text/css" href="//s.gkarch.com/web/css/slick-theme.css"/>

  <link rel="icon" type="image/x-icon" href="/favicon.ico">

    <script type="application/ld+json">
      {
        "@context": "http://schema.org",
        "@type": "Organization",
        "url": "https://www.gkarch.com",
        "logo": "//s.gkarch.com/web/img/logo.png"
      }
    </script>


  <script src="//s.gkarch.com/js/jquery-1.11.1.min.js"></script>
  <!--[if lt IE 9]>
  <script src="//s.gkarch.com/web/js/html5shiv.min.js"></script>
  <script src="//s.gkarch.com/web/js/respond.min.js"></script>
  <![endif]-->
</head>


<body class="wrap">
  <div class="footer-wrap">

  <header role="banner">
  <nav class="mobile-nav show-on-mobiles">
    <ul>
  <li class="">
    <a href="/product/">产品</a>
  </li>
    <li class="">
    <a href="/download/">下载</a>
  </li>
  <li class="">
    <a href="/docs/home/">文档</a>
  </li>
  <li class="">
    <a href="/news/">新闻</a>
  </li>
  <li class="">
    <a href="/community/">社区</a>
  </li>
</ul>

  </nav>
  <div class="grid">
    <div class="unit one-third center-on-mobiles">
      <h1>
        <a href="/">
          <span class="sr-only">GKarch</span>
          <img src="//s.gkarch.com/web/img/logo-2x.png" style="margin-top:20px" alt="GKarch Logo">
        </a>
      </h1>
    </div>
    <nav class="main-nav unit two-thirds hide-on-mobiles">
      <ul>
  <li class="">
    <a href="/product/">产品</a>
  </li>
    <li class="">
    <a href="/download/">下载</a>
  </li>
  <li class="">
    <a href="/docs/home/">文档</a>
  </li>
  <li class="">
    <a href="/news/">新闻</a>
  </li>
  <li class="">
    <a href="/community/">社区</a>
  </li>
</ul>

    </nav>
  </div>
</header>
  <section class="quickstart">
  <div class="grid">
    <div class="unit align-center center-on-mobiles">
          <div class="slides">
                <div><img data-lazy="/img/b1.png" alt="轻量级核心 - 高效易用"/></div>
                <div><img data-lazy="/img/b2.png" alt="通过容器组合各种功能 - 轻松扩展"/></div>
                <div><img data-lazy="/img/b3.png" alt="模块/部件化 - 隔离、重用功能"/></div>
                <div><img data-lazy="/img/b4.png" alt="简化设计、排除干扰 - 降低开发成本"/></div>
                <div><img data-lazy="/img/b5.png" alt="企业级基础设施 + 常用功能 = 更高效的开发效率"/></div>
                <div><img data-lazy="/img/b6.png" alt="GKarch - 致力于提高软件生产力"/></div>
          </div>
    </div>
  </div>
</section>
<section class="features">
  <div class="grid">
    <div class="unit one-third">
      <h2>下载</h2>
      <p>
        下载最新版本的Glacier
      </p>
      <a href="/download/">获取 &rarr;</a>
    </div>
    <div class="unit one-third">
      <h2>文档</h2>
      <p>提供GKarch相关的使用说明</p>
      <a href="/docs/home">查看文档 &rarr;</a>
    </div>
    <div class="unit one-third">
      <h2>社区</h2>
      <p>
        无论你是一个软件设计新手，还是经验丰富的架构师，都能在GKarch社区中找到所需的知识。
      </p>
      <a href="/community/">前往 &rarr;</a>
    </div>
    <div class="clear"></div>
  </div>
</section>

  <div class="footer-push"></div>
  </div>
  <footer role="contentinfo">
  <div class="grid">
    <div class="unit one-third center-on-mobiles">
      <p><img src="//s.gkarch.com/web/../img/baobros.png" alt="BaoBros"></img></p>
    </div>
    <div class="unit two-thirds align-right center-on-mobiles">
      <p>
        <a href="/faq/">FAQ</a>
                  <span class="split"> ｜ </span>
        <a href="/license/">使用条款</a>
                  <span class="split"> ｜ </span>
                  <a href="//blog.gkarch.com" target="_blank">博客</a>
                  <span class="split"> ｜ </span>
                  <a href="/contact/">联系我们</a>
        <span class="split"> ｜ </span>
        <a href="http://www.beian.miit.gov.cn">粤ICP备15049704号</a>
        <br>
        <a href="#">深圳包子兄弟科技有限公司 版权所有</a>
          </p>
    </div>
  </div>
</footer>

  <script type="text/javascript">
$(document).ready(function() {
  $(".doc-content, .page").find(":header[id]").append(function() {
    return $("<a/>", {
      href: "#" + this.id,
      html: '<span class="sr-only">Permalink</span><i class="fa fa-link"></i>',
      title: "Permalink",
      "class": "header-link"
    });
  });

  $(".doc-content img:not([width])").wrap(function() {
    return $("<a/>", {
      href: this.src,
      target: "_blank",
      title: this.alt ? "点击查看原图 - " + this.alt : "点击查看原图",
      style: "padding:0"
    });
  });

  $(".doc-content a, .page a").filter(function(idx, elt) {
    return this.hostname != window.location.hostname && !$(elt).hasClass("btn");
  }).attr('target', '_blank').append('<i class="fa fa-external-link"></i>');
});
</script>



  <script src="//s.gkarch.com/web/js/slick.min.js"></script>
  <script type="text/javascript">
        $(document).ready(function(){
          $('.slides').slick({
                 lazyLoad: 'progressive',
                 dots: true,
                 arrows: false,
                 slidesToShow: 1,
                 autoplay: true,
                 autoplaySpeed: 5000,
          });
        });
  </script>
    <script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "//hm.baidu.com/hm.js?e52a3ee9567260a2ebbac9d7a304239c";
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(hm, s);
})();
</script>
    <script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');
  ga('create', 'UA-64119239-1', 'auto');
  ga('send', 'pageview');
</script>
  <script src="//s.gkarch.com/web/js/jquery.toc.js"></script>
</body>
</html>
```
**（这里的 < string > 类型参数是为了示例的清晰，它可以被省略，让编译器推断。）**

查询task的Result属性时，未处理的异常会被封装在AggregateException中自动重新抛出。然而，如果没有查询Result属性（并且也没有调用Wait），未处理的异常会令程序结束。

TPL 具有更多的特性，非常适合于利用多核处理器。关于 TPL 的讨论我们在第 5 部分中继续。

#### 3.2优化线程池
线程池初始时其池内只有一个线程。随着任务的分配，线程池管理器就会向池内“注入”新线程来满足工作负荷的需要，直到最大数量的限制。在足够的非活动时间之后，线程池管理器在认为“回收”一些线程能够带来更好的吞吐量时进行线程回收。

可以通过调用ThreadPool.SetMaxThreads方法来设置线程池可以创建的线程上限；默认如下：

* Framework 4.0，32位环境下：1023
* Framework 4.0，64位环境下：32768
* Framework 3.5：每个核心 250
* Framework 2.0：每个核心 25
（这些数字可能根据硬件和操作系统不同而有差异。）数量这么多是因为要确定阻塞（等待一些条件，比如远程计算机的相应）的线程的条件是否被满足。

也可以通过ThreadPool.SetMinThreads设置线程数量下限。下限的作用比较奇妙：它是一种高级的优化技术，用来指示线程池管理器在达到下限之前不要延迟线程的分配。当存在阻塞线程时，提高下限可以改善程序并发性。

默认下限数量是 CPU 核心数，也就是能充分利用 CPU 的最小数值。在服务器环境下（比如 IIS 中的 ASP.NET），下限数量一般要高的多，差不多 50 或者更高。
#### 3.3最小线程数量是如何起作用的
将线程池的最小线程数设置为 x 并不是立即创建至少 x 个线程，而是线程会根据需要来创建。这个数值是指示线程池管理器当需要的时候，立即 创建 x 个线程。那么问题是为什么线程池在其它情况下会延迟创建线程？

答案是为了防止短生命周期的任务导致线程数量短暂高峰，使程序的内存足迹（memory footprint）快速膨胀。为了描述这个问题，考虑在一个 4 核的计算机上运行一个客户端程序，它一次发起了 40 个任务请求。如果每个任务都是一个 10ms 的计算，假设它们平均分配在 4 个核心上，总共的开销就是 100ms 多。理想情况下，我们希望这 40 个任务运行在 4 个线程上：

* 如果线程数量更少，就无法充分利用 4 个核心。
* 如果线程数量更多，会浪费内存和 CPU 时间去创建不必要的线程。

线程池就是以这种方式工作的。让线程数量和 CPU 核心数量匹配，就能够既保持小的内存足迹，又不损失性能。当然这也需要线程都能被充分使用（在这个例子中满足该条件）。

但是，现在来假设任务不是进行 10ms 的计算，而是请求互联网，使用半秒钟等待响应，此时本地 CPU 是空闲状态。线程池管理器的线程经济策略（译者注：指上面说的线程数量匹配核心数）这时就不灵了，应该创建更多的线程，让所有的请求同时进行。

幸运的是，线程池管理器还有一个后备方案。如果在半秒内没有能够响应请求队列，就会再创建一个新的线程，以此类推，直到线程数量上限。

半秒的等待时间是一把双刃剑。一方面它意味着一次性的短暂任务不会使程序快速消耗不必要的40MB（或者更多）的内存。另一方面，在线程池线程被阻塞时，比如在请求数据库或者调用WebClient.DownloadFile，就进行不必要的等待。因为这种原因，你可以通过调用SetMinThreads来让线程池管理器在分配最初的 x 个线程时不要等待，例如：

```csharp
ThreadPool.SetMinThreads (50, 50);
```

（第二个参数是表示多少个线程分配给 I/O 完成端口（I/O completion ports，IOCP），来被APM使用，这会在C# 4.0 in a Nutshell 的第 23 章描述。）

最小线程数量的默认值是 CPU 核心数。

## 2.并行编程
### 1.任务并行
任务并行（task parallelism）是 PFX 中最底层的并行方式。这一层次的类定义在System.Threading.Tasks命名空间中，如下所示：
|类| 作用 |
|--|--|
|Task|管理工作单元|
|Task< TResult >|管理有返回值的工作单元|
|TaskFactory|创建任务|
|TaskFactory< TResult >|创建有相同返回类型的任务和任务延续|
|TaskScheduler|管理任务调度|
|TaskCompletionSource|手动控制任务的工作流|

本质上，任务是用来管理可并行工作单元的轻量级对象。任务使用 CLR 的线程池来避免启动独立线程的开销：它和ThreadPool.QueueUserWorkItem使用的是同一个线程池，在 CLR 4.0 中这个线程池被调节过，让Task工作的更有效率（一般来说）。

需要并行执行代码时都可以使用任务。然而，它们是为了充分利用多核而调节的：事实上，Parallel类和PLINQ内部就是基于任务并行构建的。

任务并不只是提供了简单高效的使用线程池的方式。它们还提供了一些强大的功能来管理工作单元，包括：
* 调节任务调度
* 在一个任务中启动另一个任务时建立其父子关系
* 实现协作取消
* 等待一组任务，而无需使用信号构造
* 附加“延续”任务
* 基于多个前项任务调度延续任务
* 传递异常给父任务、延续任务和任务的使用方
#### 1.1创建与启动任务
如同我们在第 1 部分线程池的讨论中那样，可以调用Task.Factory.StartNew，并给它传递一个Action委托来创建并启动Task：

```csharp
Task.Factory.StartNew (() => Console.WriteLine ("Hello from a task!"));
```
泛型的版本Task<TResult>（Task的子类）可以让你在任务结束时获得返回的数据：

```csharp
Task<string> task = Task.Factory.StartNew<string> (() =>    // 开始任务
{
  using (var wc = new System.Net.WebClient())
    return wc.DownloadString ("http://www.linqpad.net");
});

RunSomeOtherMethod();         // 我们可以并行的做其它工作...

string result = task.Result;  // 等待任务结束并获取结果
```
Task.Factory.StartNew是一步创建并启动任务。你也可以分解它，先创建Task实例，再调用Start：

```csharp
var task = new Task (() => Console.Write ("Hello"));
// ...
task.Start();
```
使用这种方式创建的任务也可以同步运行（在当前线程上）：使用RunSynchronously替代Start。

**可以使用Status属性来追踪任务的执行状态。**

**指定状态对象：**
当创建任务实例或调用Task.Factory.StartNew时，可以指定一个状态对象（state object），它会被传递给目标方法。如果你希望直接调用方法而不是 lambda 表达式，则可以使用它。
```csharp
static void Main()
{
  var task = Task.Factory.StartNew (Greet, "Hello");
  task.Wait();  // 等待任务结束
}

static void Greet (object state) { Console.Write (state); }   // 打印 "Hello"
```
因为 C# 中有 lambda 表达式，我们可以更好的使用状态对象，用它来给任务赋予一个有意义的名字。然后就可以使用AsyncState属性来查询这个名字：

```csharp
static void Main()
{
  var task = Task.Factory.StartNew (state => Greet ("Hello"), "Greeting");
  Console.WriteLine (task.AsyncState);   // 打印 "Greeting"
  task.Wait();
}

static void Greet (string message) { Console.Write (message); }
```
输出结果：

```csharp
Greeting
Hello
```

Visual Studio 会在并行任务窗口显示每个任务的AsyncState属性，所以指定有意义的名字可以很大程度的简化调试。

#### 1.2等待任务
有两种方式可以显式等待任务完成：

* 调用Wait方法（可选择指定超时时间）
* 访问Result属性（当使用Task< TResult >时）

**也可以同时等待多个任务：通过静态方法Task.WaitAll（等待所有指定任务完成）和Task.WaitAny（等待任意一个任务完成）。**

WaitAll和依次等待每个任务类似，但它更高效，因为它只需要（至多）一次上下文切换。并且，如果有一个或多个任务抛出未处理的异常，WaitAll仍然能够等待所有任务，并在之后重新抛出一个AggregateException异常，它聚合了所有出错任务的异常，功能相当于下面的代码：

```csharp
// 假设 t1、t2 和 t3 是任务：
var exceptions = new List<Exception>();
try { t1.Wait(); } catch (AggregateException ex) { exceptions.Add (ex); }
try { t2.Wait(); } catch (AggregateException ex) { exceptions.Add (ex); }
try { t3.Wait(); } catch (AggregateException ex) { exceptions.Add (ex); }
if (exceptions.Count > 0) throw new AggregateException (exceptions);
```
调用WaitAny相当于在一个ManualResetEventSlim上等待，每个任务结束时都对它发信号。

除了使用超时时间，你也可以传递一个取消标记给Wait方法：这样可以取消等待。注意这不是取消任务。

#### 1.3异常处理
当你等待一个任务结束时（通过调用Wait方法或访问其Result属性），所有未处理的异常都会用一个AggregateException对象封装，方便重新抛给调用方。一般就无需在任务代码中处理异常，而是这么做：
```csharp
int x = 0;
Task<int> calc = Task.Factory.StartNew (() => 7 / x);
try
{
  Console.WriteLine (calc.Result);
}
catch (AggregateException aex)
{
  Console.Write (aex.InnerException.Message);  // 试图以 0 为除数
}
```
#### 1.4取消任务CancellationTokenSource和CancellationToken
 CancellationTokenSource和CancellationToken这两个类共同工作：

* CancellationTokenSource定义了Cancel方法。
* CancellationToken定义了IsCancellationRequested属性和ThrowIfCancellationRequested方法。

要使用这两个类，首先实例化一个CancellationTokenSource对象：

```csharp
var cancelSource = new CancellationTokenSource();
```

然后，传递Token属性给你希望支持取消的方法：

```csharp
new Thread (() => Work (cancelSource.Token)).Start();
```

这里是Work的定义：

```csharp
void Work (CancellationToken cancelToken)
{
  cancelToken.ThrowIfCancellationRequested();
  // ...
}
```

当需要取消时，在cancelSource上调用Cancel就可以了。

用法举例，当我们在控制台输入exit时，就会调用令牌中的Cancel方法,让Work方法停下来：

```csharp
using System.Diagnostics;

namespace App
{
    class ThreadTest
    {
        static void Main()
        {
            var cancelSource = new CancellationTokenSource();
            new Thread(() => Work(cancelSource.Token)).Start();

            string s=Console.ReadLine();
            if (s == "exit")
            {
                cancelSource.Cancel();
            }
        }

       static void Work(CancellationToken cancelToken)
        {
           
            for (int i = 0; i < 10; i++)
            {
               if (cancelToken.IsCancellationRequested)
                {
                    break;
                }
                Console.WriteLine("你好");
                Thread.Sleep(1000);
            }
            //cancelToken.ThrowIfCancellationRequested();
        }
    }
}
```

**CancellationToken是一个结构体，但是你可以把它当作类来看待。当它进行隐式复制时，副本的行为是相同的，都会引用原始的CancellationTokenSource。
CancellationToken结构体提供了其它两个有用的成员。第一个是WaitHandle，返回一个等待句柄，在取消时会对它发信号。第二个是Register，使你可以注册一个在取消时调用的委托。**

启动任务时可以可选的传递一个取消标记（cancellation token）。它可以让你通过协作取消模式取消任务，像之前描述的那样：

```csharp
var cancelSource = new CancellationTokenSource();
CancellationToken token = cancelSource.Token;

Task task = Task.Factory.StartNew (() =>
{
  // 做些事情...
  token.ThrowIfCancellationRequested();  // 检查取消请求
  // 做些事情...
}, token);
// ...
cancelSource.Cancel();
```

如果要检测任务取消，可以用如下方式捕捉AggregateException，并检查它的内部异常：

```csharp
try
{
  task.Wait();
}
catch (AggregateException ex)
{
  if (ex.InnerException is OperationCanceledException)
    Console.Write ("Task canceled!");
}
```
### 2.TaskCompletionSource
Task类做了两件事情：

* 它可以调度一个委托到线程池线程上运行。
* 它提供了管理工作项的丰富功能（延续、子任务、异常封送等等）。

有趣的是，这两件事可以是分离的：可以只利用任务的管理工作项的功能而不让它调度到线程池上运行。TaskCompletionSource类开启了这个模式。

**使用TaskCompletionSource时，就创建它的实例。它暴露一个Task属性来返回一个任务，你可以对其等待或附加延续，就和对一般的任务一样。然而这个任务可以通过TaskCompletionSource对象的下列方法进行完全控制**：

```csharp
public class TaskCompletionSource<TResult>
{
  public void SetResult (TResult result);
  public void SetException (Exception exception);
  public void SetCanceled();

  public bool TrySetResult (TResult result);
  public bool TrySetException (Exception exception);
  public bool TrySetCanceled();
  // ...
}
```
如果调用多次，SetResult、SetException和SetCanceled会抛出异常，而TrySetResult 、TrySetException 、TrySetCanceled方法会返回false。

**TResult对应任务的返回类型，所以TaskCompletionSource<int>会给你一个Task<int>。如果需要不返回结果的任务，可以使用object类型来创建TaskCompletionSource，并在调用SetResult时传递null。可以把Task<object>转换为Task类型来使用。**

下面的代码在等待五秒之后打印 “ 123 “：

```csharp
var source = new TaskCompletionSource<int>();

new Thread (() => { Thread.Sleep (5000); source.SetResult (123); })
  .Start();

Task<int> task = source.Task;      // 我们的“奴隶”任务
Console.WriteLine (task.Result);   // 123
```
再举一个例子：
大多数时候，只在目标方法要调用基于事件API，又要返回Task的时候使用。比如下面的ApiWrapper方法，该方法要返回Task,又要调用EventClass对象的Do方法，并且等到Do方法触发Done事件后，Task才能得到结果并返回。
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

```csharp
EventClass:1
Foo4:1
Done
```



### 3.并发集合
Framework 4.0 在System.Collections.Concurrent命名空间中提供了一组新的集合。它们都是完全线程安全的：
|
并发集合| 对应的非并发集合 |
|--|--|
|ConcurrentStack< T >|Stack< T >|
|ConcurrentQueue< T >|Queue< T >|
|ConcurrentBag< T >|( none )|
|TaskFactory< TResult >|创建有相同返回类型的任务和任务延续|
|BlockingCollection< T >|	( none )|
|ConcurrentDictionary<TKey,TValue>|Dictionary<TKey,TValue>|

在一般的多线程场景中，需要线程安全的集合时可能会用到这些并发集合。但是，有些注意事项：

* 并发集合是为了并行编程而调整的。除了高并发场景，传统的集合都比它们更高效。
* 线程安全的集合并不能确保使用它的代码也是线程安全的。
* 如果在对并发集合进行枚举的同时有其它线程修改了集合，并不会产生异常，而是会得到一个新旧内容的混合结果。
* 没有List<T>的并发版本。
* 并发的栈、队列和包（bag）类内部都是使用链表实现的。这使得它们的空间效率不如非并发的Stack和Queue类，但是这对于并发访问更好，因为链表有助于实现无锁或更少的锁。（这是因为向链表中插入一个节点只需要更新两个引用，而对于List<T>这种结构插入一个元素可能需要移动几千个已存在的元素。）
换句话说，这些集合并不是提供了加锁使用普通集合的快捷办法。为了演示这一点，如果我们在单一线程上执行以下代码：

```csharp

var d = new ConcurrentDictionary<int,int>();
for (int i = 0; i < 1000000; i++) d[i] = 123;
```

它会比下面的代码慢三倍：

```csharp

var d = new Dictionary<int,int>();
for (int i = 0; i < 1000000; i++) lock (d) d[i] = 123
```

（但是对ConcurrentDictionary读取会更快，因为读是无锁的。）

并发集合与普通集合的另一个不同之处是它们暴露了一些特殊的方法，来进行原子的检查并行动（test-and-act）的操作，例如TryPop。这些方法中的大部分都是由IProducerConsumerCollection< T >接口统一的。

## 3.一些多线程相关概念
### 1.线程
线程（thread） 是操作系统能够调度的最小执行单元。不过我更喜欢把它理解为，这是操作系统对于 CPU 计算资源的抽象。

如同虚拟内存，程序看到的空间资源是操作系统抽象出来的虚拟空间。而看到的时间资源就是操作系统抽象出来的线程了。在早期 CPU 还是单核的时候，如果没有这种抽象，就不可能实现多任务。这样的抽象也能让程序的开发者不用考虑 CPU 具体的情况，而是根据程序需要自由的使用计算资源。

这样，有了虚拟内存和线程，程序在时间和空间上都有了一个硬件无关的环境。

### 2.抢占
因为有了线程这样的抽象，导致需要同时执行的代码要远大于 CPU 核心数，这种供不应求就需要一种机制保证多个线程能够共享一个核心。

操作系统并不清楚线程要做什么，也不清楚线程要执行多久，所以一种比较合理的方式就是一会儿执行这个，一会儿执行那个。就相当于把 CPU 核心的时间切分成开，然后把它们分配给线程使用，让线程与核心轮流发生关系。

线程并不清楚这次时间还剩多久，也不清楚下次轮到自己是什么时候，只要操作系统判断时间到了，线程就会被暂停执行，操作系统选择下一个线程来执行。在线程看来，就是自己的时间被 抢占（preempt） 了。

被操作系统切分开的时间叫做 时间切片（时间片，time-slice，quantum），切片的长度由操作系统控制，是系统定时器精度的整数倍（Windows 系统默认 15.625ms，可以修改）。

### 3.上下文切换
在抢占式多任务系统中，由于线程并不清楚自己在什么时候会被打断，所以必须由操作系统保存线程当时的状态，之后再次运行它的时候恢复之前的状态，这样线程才能正常继续执行。

上下文切换（context-switch） 就是指操作系统暂停线程执行，保存其状态，恢复下一个执行的线程的状态，开始其执行的过程。这个过程大概需要几微秒，算是开销比较大的操作。这里的状态主要是指 CPU 寄存器的值（包括指令指针和栈指针），还有一些操作系统需要的数据。

上面主要说的是时间片结束时的情况，还有种情况就是线程主动等待，这时仍然需要上下文切换，在后面阻塞和自旋中会再次提到。另外，系统中断也会导致一种轻量级的上下文切换，不过中断处理不能算线程，在后面的系统中断中会进行描述。

### 4.协作
多个线程共享一个核心还有另一种方式，就是 协作（cooperation）。意思是操作系统不能打断线程的执行，而是线程在其觉得合适的时机主动 出让（yield），这时操作系统才能选择下一个执行的线程。

很明显的问题是，如果正在执行的线程因为各种原因不出让，那么其它线程就无法执行。这在操作系统层面一般是无法接受的，所以绝大多数操作系统都是抢占式的。不过这种协作的机制可以无需上下文切换，或者大幅降低上下文切换的开销，系统调度起来非常容易。因此如果能够用好，性能会很高。所以在操作系统整体是抢占式的前提下，往往还有一定协作机制，可能由操作系统实现，也可能由编程语言或平台实现。往往被称为 协程（coroutine），之后会进一步说明。

### 5.计算密集 vs I/O 密集
在讨论多线程的场景下，工作分为两种类型：

计算密集（compute-intensive，或 CPU-bound）
顾名思义，代码的执行需要大量占用 CPU 时间，一般来说就是进行循环计算的操作。比如查找、排序、统计、编码解码等等这些与外设无关，只使用 CPU 和内存的算法。换句话说就是代码的执行主要受 CPU 计算能力限制（极端优化性能时，CPU 缓存也非常重要）。
I/O 密集（I/O-intensive，或 I/O-bound）
代码大量的时间用于进行 I/O。比如涉及数据库、网络、磁盘、串口、USB或者其它设备的输入输出，需要发起请求，等待结果，或者响应请求，传递数据。也就是代码的执行主要受 I/O 能力限制。
由于它们的依赖不同，所以不能使用同样的多线程方式来处理，而应该区别对待。比如对于计算密集的工作，最多使用 CPU 核心数量的线程来计算就足够了，它们会让 CPU 满负荷工作，线程再多只能增加系统负担，降低效率。而对于 I/O 密集的工作，应该尽可能使用操作系统提供的异步机制，而不是让大量线程浪费在 I/O 等待上。

### 6.线程池
线程在时间和空间上都属于昂贵的资源。时间上之前有提到，进行上下文切换需要几微秒，而且如果频繁进行阻塞，浪费剩余时间片，开销会更大；空间上，Windows 上一个线程默认占用 1MB 内存空间用于线程独立的栈，意味着如果是 32 位程序，最多同时使用 2000 多个线程。

因此，需要一种机制能够让线程相对合理的被使用，既能充分利用硬件资源，又能避免进行过多的线程创建与销毁。线程池（thread pool） 就是这种机制的实现，它可能由操作系统提供，也可能由语言或平台提供，也可能由第三方或自己实现。

线程池管理工作线程，使用者把任务（需要执行的代码）交给线程池，也就是加入线程池的任务队列，工作线程完成之前的任务后，就继续从队列中取任务执行。如果没有工作线程空闲，而队列中还有任务，线程池就可能会创建新的工作线程来处理任务。而如果工作线程空闲太久，就会被销毁。

实际的实现可能更复杂，比如每个工作线程都有自己的任务队列，降低了在队列上同步的需要；创建新的工作线程时可能会等待，避免为大量短时间任务创建大量线程。但因为线程池不清楚任务要执行多久，不清楚任务总体的规模，也不清楚任务是计算密集还是 I/O 密集的，所以并不一定能够提供非常好的效果。一般的原则是：短时间的和少量并发的任务可以交给线程池，而长时间或大量并发的任务最好自己处理，来达到更好的效果。

### 7.并行 vs 并发
并行（parallelism） 与 并发（concurrency） 是非常常见的术语，然而使用时经常会混淆，这里简单说明下。

它们的共同点是，都描述了同时执行代码的行为。然而这里的“同时”具有不同的含义。

对于并行来说，同时是指真正的同时。也就是说，单核 CPU 是无法并行的。它更强调的是对计算资源的利用，效果是让程序执行的更快。比如：修改一个算法让它支持并行执行；让多台机器并行处理请求；使用 GPU 并行计算。

对于并发来说，同时是指多个任务在处理过程中有重叠，让外界看起来是同时的。无论单核还是多核 CPU 都可以并发。它更强调的是程序的可用性，与是否加速无关。比如：操作系统的时间切片就是一种支持多任务并发的机制；为保证 UI 响应，使用后台线程并发加载数据；服务器并发处理请求。

一个更宽泛的概念就是上面已经用到的 同时（simultaneous），它不算术语，所以也没有特殊的含义，可以用来描述不需要区分并行和并发的场景。比如：同时执行任务 A 和任务 B；同时发起 10 个请求。

### 8.线程同步
### 9.同步上下文
### 10.异步
### 11.IOCP
### 12.协程Permalink
### 13.系统中断
