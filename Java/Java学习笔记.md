@[TOC](目录)
# 前言
## 1.Java简介
### 1.Java概述

> 一般来说，编程语言就分为两大类：
> 
> 1. 编译型语言：需要先编译为计算机可以直接执行的命令才可以运行。优点是计算机直接运行，性能高；缺点是与平台密切相关，在一种操作系统上编译的程序，无法在其他非同类操作系统上运行，比如Windows下的exe程序在Mac上就无法运行。
> 2. 解释型语言：只需要通过解释器代为执行即可，不需要进行编译。优点是可以跨平台，因为解释是解释器的事情，只需要在各个平台上安装对应的解释器，代码不需要任何修改就可以直接运行；缺点是需要依靠解释器解释执行，效率肯定没直接编译成机器指令运行的快，并且会产生额外的资源占用。

> Write Once, Run Anywhere.
> 这是Java语言的标语，它的目标很明确：一次编写，到处运行，它旨在打破平台的限制，让Java语言可以运行在任何平台上，并且不需要重新编译，实现跨平台运行。
### 2.Java语言运行机制
> 实际上Java程序也是需要进行编译才可以运行的，这一点与C语言是一样的，Java程序编译之后会变成.class结尾的二进制文件：
> 
![在这里插入图片描述](https://img-blog.csdnimg.cn/f8a6dce42f794d9e9160e9000f0d5a8f.png)
> 不过不同的是，这种二进制文件计算机并不能直接运行，而是需要交给JVM（Java虚拟机：Java Virtual Machine ）执行。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ea789c77ec4248e899cdb133d55c7d02.png)

> JVM是个什么东西呢？简单来说，它就像我们前面介绍的解释器一样，我们可以将编译完成的.class文件直接交给JVM去运行，而程序中要做的事情，也都是由它来告诉计算机该如何去执行。
> 

> 在不同的操作系统下，都有着对应的JVM实现，我们只需要安装好就可以了，而我们程序员只需要将Java程序编译为.class文件就可以直接交给JVM运行，无论是什么操作系统，JVM都采用的同一套标准读取和执行.class文件，所以说我们编译之后，在任何平台都可以运行，实现跨平台。
> 

> 由于Java又需要编译同时还需要依靠JVM解释执行，所以说Java既是编译型语言，也是解释型语言。
### 3.JDK和JRE
> 1. JRE（Java Runtime Environment）：Java的运行环境，安装了运行环境之后，Java程序才可以运行，一般不做开发，只是需要运行Java程序直接按照JRE即可。
> 2. JDK（Java Development Kit）：包含JRE，并且还附带了大量开发者工具，我们学习Java程序开发就使用JDK即可。
## 2.包声明和导入
> 包其实就是用来区分类位置的东西，也可以用来将我们的类进行分类（类似于C++中的namespace）随着我们的程序不断变大，可能会创建各种各样的类，他们可能会做不同的事情，那么这些类如果都放在一起的话，有点混乱，我们可以通过包的形式将这些类进行分类存放。(可以把Java中的包理解为C#和C++中的命名空间)。

![在这里插入图片描述](https://img-blog.csdnimg.cn/d9f399b67cdf465e9f86a15a9aeb7870.png)

> 这里又是一个新的关键字package，这个是用于指定当前类所处的包的，注意，所处的包和对应的目录是一一对应的。

> 当我们使用同一个包中的类时，直接使用即可（之前就是直接使用的，因为都直接在一个缺省的包中）而当我们需要使用其他包中的类时，需要先使用import关键字进行导入才可以：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d9a0c01879764c36955a42803cf1fe44.png)
## 3.包封装、继承和多态

> 封装、继承和多态是面向对象编程的三大特性。
> 1. 封装，把对象的属性和方法结合成一个独立的整体，隐藏实现细节，并提供对外访问的接口。
> 2. 继承，从已知的一个类中派生出一个新的类，叫子类。子类实现了父类所有非私有化的属性和方法，并根据实际需求扩展出新的行为。
> 3. 多态，多个不同的对象对同一消息作出响应，同一消息根据不同的对象而采用各种不同的方法。
## 4.使用Java代码实现一个单例类

```java
package Test;
public class Test {
    private static  Test instance;
    private Test(){

    }
    public  static  Test GetInstance(){
        if(instance==null){
            instance=new Test();
        }
        return instance;
    }
}

```
## 5.顶层Object类

> 实际上所有类都默认继承自Object类，除非手动指定继承的类型，但是依然改变不了最顶层的父类是Object类。所有类都包含Object类中的方法，比如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2d18f854d818457990ee877eb23df196.png)

> 我们发现，除了我们自己在类中编写的方法之外，还可以调用一些其他的方法，那么这些方法不可能无缘无故地出现，肯定同样是因为继承得到的，那么这些方法是继承谁得到的呢？

```java
public class Person extends Object{   
//除非我们手动指定要继承的类是什么，实际上默认情况下所有的类都是继承自Object的，只是可以省略

}
```

> 既然所有的类都默认继承自Object，我们来看看这个类里面有哪些内容：

```java
public class Object {

    private static native void registerNatives();   
    static {
        registerNatives();   
    }

    //获取当前的类型Class对象
    public final native Class<?> getClass();

    //获取对象的哈希值
    public native int hashCode();

      //判断当前对象和给定对象是否相等，默认实现是直接用等号判断，也就是直接判断是否为同一个对象
      public boolean equals(Object obj) {
        return (this == obj);
    }
  
    //克隆当前对象，可以将复制一个完全一样的对象出来，包括对象的各个属性
    protected native Object clone() throws CloneNotSupportedException;

    //将当前对象转换为String的形式，默认情况下格式为 完整类名@十六进制哈希值
    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    //唤醒一个等待当前对象锁的线程，有关锁的内容，我们会在第六章多线程部分中讲解，目前暂时不会用到
    public final native void notify();

    //唤醒所有等待当前对象锁的线程，同上
    public final native void notifyAll();

    //使得持有当前对象锁的线程进入等待状态，同上
    public final native void wait(long timeout) throws InterruptedException;

    //同上
    public final void wait(long timeout, int nanos) throws InterruptedException {
        ...
    }

    //同上
    public final void wait() throws InterruptedException {
        ...
    }

    //当对象被判定为已经不再使用的“垃圾”时，在回收之前，会由JVM来调用一次此方法进行资源释放之类的操作
    protected void finalize() throws Throwable { }
}
```
## 6.方法的重写

> 注意，方法的重写不同于之前的方法重载，不要搞混了，方法的重载是为某个方法提供更多种类，而方法的重写是覆盖原有的方法实现，比如我们现在不希望使用Object类中提供的equals方法，那么我们就可以将其重写了：

```java
package Test;
public class Person {

    private String name;
    private String age;
    private String sex;

    @Override   //重写方法可以添加 @Override 注解
    public boolean equals(Object obj) {   //重写方法要求与父类的定义完全一致
        if(obj == null) return false;   //如果传入的对象为null，那肯定不相等
        if(obj instanceof Person) {     //只有是当前类型的对象，才能进行比较，要是都不是这个类型还比什么
            Person person = (Person) obj;   //先转换为当前类型，接着我们对三个属性挨个进行比较
            return this.name.equals(person.name) &&    //字符串内容的比较，不能使用==，必须使用equals方法
                    this.age == person.age &&       //基本类型的比较跟之前一样，直接==
                    this.sex.equals(person.sex);
        }
        return false;
    }
}
```
## 7.抽象类
在我们学习了类的继承之后，实际上我们会发现，越是处于顶层定义的类，实际上可以进一步地进行抽象，比如我们前面编写的考试方法：

```java
protected void exam(){
    System.out.println("我是考试方法");
}
```

> 这个方法在子类中一定会被重写，所以说除非子类中调用父类的实现，否则一般情况下永远都不会被调用，就像我们说一个人会不会考试一样，实际上人怎么考试是一个抽象的概念，而学生怎么考试和工人怎么考试，才是具体的一个实现，所以说，我们可以将人类进行进一步的抽象，让某些方法完全由子类来实现，父类中不需要提供实现。
> 
> 要实现这样的操作，我们可以将人类变成抽象类，抽象类比类还要抽象：

```java
public abstract class Person {   //通过添加abstract关键字，表示这个类是一个抽象类
    protected String name;   //大体内容其实普通类差不多
    protected int age;
    protected String sex;
    protected String profession;

    protected Person(String name, int age, String sex, String profession) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.profession = profession;
    }

    public abstract void exam();   //抽象类中可以具有抽象方法，也就是说这个方法只有定义，没有方法体
}
```

> 而具体的实现，需要由子类来完成，而且如果是子类，必须要实现抽象类中所有抽象方法：

```java
package Test;

public class Worker extends Person{
    protected Worker(String name, int age, String sex, String profession) {
        super(name, age, sex, profession);
    }

    ////子类必须要实现抽象类所有的抽象方法，这是强制要求的，否则会无法通过编译
    @Override
    public void exam() {
        System.out.println("我是工人，做题我并不擅长，只能得到 D");
    }
}
```

> 抽象类由于不是具体的类定义（它是类的抽象）可能会存在某些方法没有实现，因此无法直接通过new关键字来直接创建对象：

![在这里插入图片描述](https://img-blog.csdnimg.cn/447e823b791f4444bef4cd036b21325c.png)

> 要使用抽象类，我们只能去创建它的子类对象。
> 
> 抽象类一般只用作继承使用，当然，抽象类的子类也可以是一个抽象类：

```java
public abstract class Student extends Person{   //如果抽象类的子类也是抽象类，那么可以不用实现父类中的抽象方法
    public Student(String name, int age, String sex) {
        super(name, age, sex, "学生");
    }

    @Override   //抽象类中并不是只能有抽象方法，抽象类中也可以有正常方法的实现
    public void exam() {
        System.out.println("我是学生，我就是小镇做题家，拿个 A 轻轻松松");
    }
}
```

> 注意，抽象方法的访问权限不能为private：

![在这里插入图片描述](https://img-blog.csdnimg.cn/029051c800ae46338e7e6eaf2745dabc.png)

> 因为抽象方法一定要由子类实现，如果子类都访问不了，那么还有什么意义呢？所以说不能为私有。
## 8.接口
### 1.简介
> **使用implements关键字来实现接口。**

> 接口甚至比抽象类还抽象，他只代表某个确切的功能！也就是只包含方法的定义，甚至都不是一个类！接口一般只代表某些功能的抽象，接口包含了一些列方法的定义，类可以实现这个接口，表示类支持接口代表的功能（类似于一个插件，只能作为一个附属功能加在主体上，同时具体实现还需要由主体来实现）
> 
> 咋一看，这啥意思啊，什么叫支持接口代表的功能？实际上接口的目标就是将类所具有某些的行为抽象出来。
> 
> 比如说，对于人类的不同子类，学生和老师来说，他们都具有学习这个能力，既然都有，那么我们就可以将学习这个能力，抽象成接口来进行使用，只要是实现这个接口的类，都有学习的能力：

```java
public interface IStudy {//使用interface表示这是一个接口
    void Study();//接口中只能定义访问权限为public抽象方法，其中public和abstract关键字可以省略
}

```

我们可以让类实现这个接口：

```java
package Test;

public class Student extends Person implements IStudy {
    protected Student(String name, int age, String sex, String profession) {
        super(name, age, sex, profession);
    }

    @Override
    public void Study() {
        System.out.println("我会学习");
    }

    @Override
    public void exam() {

    }
}

```

接口不同于继承，接口可以同时实现多个：

```java
public class Student extends Person implements Study, A, B, C {  //多个接口的实现使用逗号隔开
  
}
```

> 所以说有些人说接口其实就是Java中的多继承，但是我个人认为这种说法是错的，实际上实现接口更像是一个类的功能列表，作为附加功能存在，一个类可以附加很多个功能，接口的使用和继承的概念有一定的出入，顶多说是多继承的一种替代方案。

> 接口跟抽象类一样，不能直接创建对象，但是我们也可以将接口实现类的对象以接口的形式去使用：

```java
public class Main {
    public static void main(String[] args) {
        IStudy study=new Student("小王",27,"男","A");
    }
}
```
当做接口使用时，只有接口中定义的方法和Object类的方法，无法使用类本身的方法和父类的方法。

接口同样支持向下转型：

```java
public class Main {
    public static void main(String[] args) {
        IStudy study=new Student("小王",27,"男","A");
        if(study instanceof Student){//判断引用的对象是不是Student类型
            Student student=(Student)study;//强制类型转换
            student.Study();
        }
    }
}
```
### 2.Object类中提供的Clone克隆方法

> 为啥要留到这里才来讲呢？因为它需要实现接口才可以使用：

```java
package java.lang;

public interface Cloneable {    //这个接口中什么都没定义
}
```

> 实现接口后，我们还需要将克隆方法的可见性提升一下，不然还用不了：

```java
package Test;

public class Student extends Person implements IStudy,Cloneable {//首先实现Cloneable接口，表示这个类具有克隆的功能
    public Student(String name, int age, String sex, String profession) {
        super(name, age, sex, profession);
    }

    @Override
    public void Study() {
        System.out.println("我会学习");
    }

    @Override
    public void exam() {

    }

    @Override
    public Object clone()  throws CloneNotSupportedException{
        return super.clone();//首先实现Cloneable接口，表示这个类具有克隆的功能
    }
}

```

> 接着我们来尝试一下，看看是不是会得到一个一模一样的对象：

```java
import Test.Student;

public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {//向上抛出一下异常
        Student student=new Student("小王",27,"男","A");
        Student clone=(Student)student.clone();
        System.out.println(student);
        System.out.println(clone);
        System.out.println(student==clone);
    }
}
```
输出结果：

```java
Test.Student@2f4d3709
Test.Student@4e50df2e
false

```

> 可以发现，原对象和克隆对象，是两个不同的对象，但是他们的各种属性都是完全一样的：

![在这里插入图片描述](https://img-blog.csdnimg.cn/022d26f8b4974acebcd0e0262b79efea.png)
### 3.浅拷贝和深拷贝

> 克隆操作可以完全复制一个对象的所有属性，但是像这样的拷贝操作其实也分为浅拷贝和深拷贝。
> 
> 1. 浅拷贝：对于类中基本数据类型，会直接复制值给拷贝对象；对于引用类型，只会复制对象的地址，而实际上指向的还是原来的那个对象，拷贝个基莫。
> 2. 深拷贝：无论是基本类型还是引用类型，深拷贝会将引用类型的所有内容，全部拷贝为一个新的对象，包括对象内部的所有成员变量，也会进行拷贝。

> 但clone方法出来的克隆对象是浅拷贝的结果。

## 9.常用工具类介绍
### 1.数学工具类

> Java提供的运算符实际上只能进行一些在小学数学中出现的运算，但是如果我们想要进行乘方、三角函数之类的高级运算，就没有对应的运算符能够做到，而此时我们就可以使用数学工具类来完成。

```java
public class Main {
    public static void main(String[] args) {
        //Math也是java.lang包下的类，所以说默认就可以直接使用
        System.out.println(Math.pow(5, 3));   //我们可以使用pow方法直接计算a的b次方

        Math.abs(-1);    //abs方法可以求绝对值
        Math.max(19, 20);    //快速取最大值
        Math.min(2, 4);   //快速取最小值
        Math.sqrt(9);    //求一个数的算术平方根
    }
}
```

> 三角函数：

```java
Math.sin(Math.PI / 2);     //求π/2的正弦值，这里我们可以使用预置的PI进行计算
Math.cos(Math.PI);       //求π的余弦值
Math.tan(Math.PI / 4);    //求π/4的正切值

Math.asin(1);     //三角函数的反函数也是有的，这里是求arcsin1的值
Math.acos(1);
Math.atan(0);
```
### 2.数组工具类

> 前面我们介绍了数组，但是我们发现，想要操作数组实在是有点麻烦，比如我们要打印一个数组，还得一个一个元素遍历才可以，那么有没有一个比较方便的方式去使用数组呢？我们可以使用数组工具类Arrays。

> 1. 这个类也是java.util包下类，它用于便捷操作数组，比如我们想要打印数组，可以直接通过toString方法转换字符串：

```java
public class Main {
    public static void main(String[] args) {
        int[] arr = new int[]{1, 4, 5, 8, 2, 0, 9, 7, 3, 6};
        System.out.println(Arrays.toString(arr));
    }
}
```
输出结果：

```java
[1, 4, 5, 8, 2, 0, 9, 7, 3, 6]
```

> 2. 将数组进行排序：

```java
public static void main(String[] args) {
    int[] arr = new int[]{1, 4, 5, 8, 2, 0, 9, 7, 3, 6};
    Arrays.sort(arr);    //可以对数组进行排序，将所有的元素按照从小到大的顺序排放
    System.out.println(Arrays.toString(arr));
}
```

> 3. 快速地对一个数组进行拷贝

```java
public static void main(String[] args) {
    int[] arr = new int[]{1, 2, 3, 4, 5};
    int[] target = Arrays.copyOf(arr, 5);
    System.out.println(Arrays.toString(target));   //拷贝数组的全部内容，并生成一个新的数组对象
    System.out.println(arr == target);
}
```

> 4. 将一个数组中的内容拷贝到其他数组中

```java
public static void main(String[] args) {
    int[] arr = new int[]{1, 2, 3, 4, 5};
    int[] target = new int[10];
    System.arraycopy(arr, 0, target, 0, 5);   //使用System.arraycopy进行搬运
    System.out.println(Arrays.toString(target));
}
```
## 10.练习题
### 1.冒泡排序算法

> 有一个int数组，但是数组内的数据是打乱的，现在我们需要将数组中的数据按从小到大的顺序进行排列，要求用冒泡排序算法。

```java
int[] arr = new int[]{3, 5, 7, 2, 9, 0, 6, 1, 8, 4};
```

> 解题代码：

```java
import Test.Student;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] arr = new int[]{3, 5, 7, 2, 9, 0, 6, 1, 8, 4};
        var newArr=BubbleSort(arr);
        for(int i=0;i<newArr.length;i++){
            System.out.println(newArr[i]);
        }
    }

    public static int[] BubbleSort(int[] arr){
        int[] finalData=new int[arr.length];
        int tempData=0;
        for(int i=0;i<arr.length-1;i++){
            for(int j=0;j<arr.length-1-i;j++){
                if(arr[j]>arr[j+1]){
                    tempData=arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=tempData;
                }
            }
        }
        finalData =arr;
        return finalData;
    }
}
```
### 2.二分搜索算法

> 现在有一个从小到大排序的数组，给你一个目标值target，现在我们想要找到这个值在数组中的对应下标，如果数组中没有这个数，请返回-1：

```java
import Test.Student;

import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 3, 4, 6, 7, 8, 10, 11, 13, 15};
        int target = 7;
        var res=MidSearchSort(arr,target);
        System.out.println(target+"在数组中的的下标位置是"+res);
    }
    
    public static int MidSearchSort(int[] arr,int target){
        int mid=0;
        int left=0;
        int right=arr.length-1;
        while (left<=right){
             mid=left+(right-left)/2;
            if(arr[mid]==target){
                return mid;
            } else if (target>arr[mid]) {
                left=mid+1;
            } else if (target<arr[mid]) {
                right=mid-1;
            }
        }
        return mid;
    }
}
```
### 3.青蛙跳台阶问题
> 现在一共有n个台阶，一只青蛙每次只能跳一阶或是两阶，那么一共有多少种跳到顶端的方案？
例如n=2，那么一共有两种方案，一次性跳两阶或是每次跳一阶。现在请你设计一个Java程序，计算当台阶数为n的情况下，能够有多少种方案到达顶端。

```java
import Test.Student;

import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        var res=footstep(5);
        System.out.println(res);
    }
    public static int footstep(int setps) {
        if (setps <= 3) {
            return setps;
        } else {
            return footstep(setps - 1) + footstep(setps - 2);
        }
    }
}
```
### 4.回文串判断

> “回文串”是一个正读和反读都一样的字符串，请你实现一个Java程序，判断用户输入的字符串（仅出现英文字符）是否为“回文”串。
> 
> 1. ABCBA 就是一个回文串，因为正读反读都是一样的
> 2. ABCA 就不是一个回文串，因为反着读不一样

```java
import Test.Student;
import java.util.Arrays;
import java.util.Collections;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入一个字符串");
        var str=sc.nextLine();
        var res=JudgeString(str);
        System.out.println(res);
    }
    public static boolean JudgeString(String str){
        int len = str.length();
        for (int i = 0; i < len / 2; i++) {
            if (str.charAt(i) != str.charAt(len - 1 - i)) {
                return false;
            }
        }
        return true;
    }
}
```
## 11.泛型
> 为了统计学生成绩，要求设计一个Score对象，包括课程名称、课程号、课程成绩，但是成绩分为两种，一种是以优秀、良好、合格来作为结果，还有一种就是 60.0、75.5、92.5这样的数字分数，可能高等数学这门课是以数字成绩进行结算，而计算机网络实验这门课是以等级进行结算，这两种分数类型都有可能出现，那么现在该如何去设计这样的一个Score类呢？
> 
> 现在的问题就是，成绩可能是String类型，也可能是Integer类型，如何才能很好的去存可能出现的两种类型呢？
### 1.泛型类
> 泛型其实就一个待定类型，我们可以使用一个特殊的名字表示泛型，泛型在定义时并不明确是什么类型，而是需要到使用时才会确定对应的泛型类型。可以将一个类定义为一个泛型类：
> 

```java
package Test;

public class Score<T> {//泛型类需要使用<>，我们需要在里面添加1 - N个类型变量
    String name;
    String id;
   public T value;//T会根据使用时提供的类型自动变成对应类型

    public  Score(String name,String id,T value){
        this.name=name;
        this.id=id;
        this.value=value;
    }
}
```
> 来看看这是如何使用的：
```java
public static void main(String[] args) {
    Score<String> score = new Score<String>("计算机网络", "EP074512", "优秀");
      //因为现在有了类型变量，在使用时同样需要跟上<>并在其中填写明确要使用的类型
      //这样我们就可以根据不同的类型进行选择了
    String value = score.value;   //一旦类型明确，那么泛型就变成对应的类型了
    System.out.println(value);
}
```
## 12.迭代器
集合类都是支持使用foreach语法的，
```java

import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> list= Arrays.asList("A","B","C");
        for (var s:list) {
            System.out.println(s);
        }
    }
}
```
## 13.IO
> JDK提供了一套用于IO操作的框架，为了方便我们开发者使用，就定义了一个像水流一样，根据流的传输方向和读取单位，分为字节流InputStream和OutputStream以及字符流Reader和Writer的IO框架，当然，这里的Stream并不是前面集合框架认识的Stream，这里的流指的是数据流，通过流，我们就可以一直从流中读取数据，直到读取到尽头，或是不断向其中写入数据，直到我们写入完成，而这类IO就是我们所说的BIO，字节流一次读取一个字节，也就是一个byte的大小，而字符流顾名思义，就是一次读取一个字符，也就是一个char的大小（在读取纯文本文件的时候更加适合）。

### 1.File类
> File类，它是专门用于表示一个文件或文件夹，只不过它只是代表这个文件，但并不是这个文件本身。通过File对象，可以更好地管理和操作硬盘上的文件。

```java
public static void main(String[] args) {
    File file = new File("test.txt");   //直接创建文件对象，可以是相对路径，也可以是绝对路径
    System.out.println(file.exists());   //此文件是否存在
    System.out.println(file.length());   //获取文件的大小
    System.out.println(file.isDirectory());   //是否为一个文件夹
    System.out.println(file.canRead());   //是否可读
    System.out.println(file.canWrite());   //是否可写
    System.out.println(file.canExecute());   //是否可执行
}
```
### 2.文件字节流FileInputStream和FileOutputStream 
首先介绍一下FileInputStream，我们可以通过它来获取文件的输入流：
```java
    public static void main(String[] args) {
        try {   //注意，IO相关操作会有很多影响因素，有可能出现异常，所以需要明确进行处理
            FileInputStream inputStream = new FileInputStream("路径");
            //路径支持相对路径和绝对路径
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
```
> 相对路径是在当前运行目录（就是你在哪个目录运行java命令启动Java程序的）的路径下寻找文件，而绝对路径，是从根目录开始寻找。路径分割符支持使用/或是\\，但是不能写为\因为它是转义字符！比如在Windows下：

```bash
C://User/lbw/nb    这个就是一个绝对路径，因为是从盘符开始的
test/test          这个就是一个相对路径，因为并不是从盘符开始的，而是一个直接的路径
```

> 在使用完成一个流之后，必须关闭这个流来完成对资源的释放，否则资源会被一直占用：

```java
public static void main(String[] args) {

    //注意，这种语法只支持实现了AutoCloseable接口的类(类似于C#中那些继承自ICloneable接口的流类)！
    try(FileInputStream inputStream = new FileInputStream("路径")) {   //直接在try()中定义要在完成之后释放的资源

    } catch (IOException e) {   //这里变成IOException是因为调用close()可能会出现，而FileNotFoundException是继承自IOException的
        e.printStackTrace();
    }
    //无需再编写finally语句块，因为在最后自动帮我们调用了close()
}
```

> 现在我们拿到了文件的输入流，那么怎么才能读取文件里面的内容呢？我们可以使用read方法：

```java
public static void main(String[] args) {
    //test.txt：a
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        //使用read()方法进行字符读取
        System.out.println((char) inputStream.read());  //读取一个字节的数据（英文字母只占1字节，中文占2字节）
        System.out.println(inputStream.read());   //唯一一个字节的内容已经读完了，再次读取返回-1表示没有内容了
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 使用read可以直接读取一个字节的数据，注意，流的内容是有限的，读取一个少一个。我们如果想一次性全部读取的话，可以直接使用一个while循环来完成：

```java
public static void main(String[] args) {
    //test.txt：abcd
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        int tmp;
        while ((tmp = inputStream.read()) != -1){   //通过while循环来一次性读完内容
            System.out.println((char)tmp);
        }
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 当然，一个一个读取效率太低了，那能否一次性全部读取呢？我们可以预置一个合适容量的byte[]数组来存放：

```java
public static void main(String[] args) {
    //test.txt：abcd
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        byte[] bytes = new byte[inputStream.available()];   //我们可以提前准备好合适容量的byte数组来存放
        System.out.println(inputStream.read(bytes));   //一次性读取全部内容（返回值是读取的字节数）
        System.out.println(new String(bytes));   //通过String(byte[])构造方法得到字符串
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 也可以控制要读取数量：

```java
System.out.println(inputStream.read(bytes, 1, 2));   //第二个参数是从给定数组的哪个位置开始放入内容，第三个参数是读取流中的字节数
```

> 注意：一次性读取同单个读取一样，当没有任何数据可读时，依然会返回-1。
> 
> 通过skip()方法可以跳过指定数量的字节：

```java
public static void main(String[] args) {
    //test.txt：abcd
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        System.out.println(inputStream.skip(1));
        System.out.println((char) inputStream.read());   //跳过了一个字节
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 注意：FileInputStream是不支持reset()的，虽然有这个方法，但是这里先不提及。
> 
> 既然有输入流，那么文件输出流也是必不可少的：

```java
public static void main(String[] args) {
    //输出流也需要在最后调用close()方法，并且同样支持try-with-resource
    try(FileOutputStream outputStream = new FileOutputStream("output.txt")) {
        //注意：若此文件不存在，会直接创建这个文件！
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 输出流没有read()操作而是write()操作，使用方法同输入流一样，只不过现在的方向变为我们向文件里写入内容：

```java
public static void main(String[] args) {
    try(FileOutputStream outputStream = new FileOutputStream("output.txt")) {
        outputStream.write('c');   //同read一样，可以直接写入内容
          outputStream.write("lbwnb".getBytes());   //也可以直接写入byte[]
          outputStream.write("lbwnb".getBytes(), 0, 1);  //同上输入流
          outputStream.flush();  //建议在最后执行一次刷新操作（强制写入）来保证数据正确写入到硬盘文件中
    }catch (IOException e){
        e.printStackTrace();
    }
}
```
> 那么如果是我只想在文件尾部进行追加写入数据呢？我们可以调用另一个构造方法来实现：

```java
public static void main(String[] args) {
    try(FileOutputStream outputStream = new FileOutputStream("output.txt", true)) {  //true表示开启追加模式
        outputStream.write("lb".getBytes());   //现在只会进行追加写入，而不是直接替换原文件内容
        outputStream.flush();
    }catch (IOException e){
        e.printStackTrace();
    }
}
```
> 利用输入流和输出流，就可以轻松实现文件的拷贝了：

```java
public static void main(String[] args) {
    try(FileOutputStream outputStream = new FileOutputStream("output.txt");
        FileInputStream inputStream = new FileInputStream("test.txt")) {   //可以写入多个
        byte[] bytes = new byte[10];    //使用长度为10的byte[]做传输媒介
        int tmp;   //存储本地读取字节数
        while ((tmp = inputStream.read(bytes)) != -1){   //直到读取完成为止
            outputStream.write(bytes, 0, tmp);    //写入对应长度的数据到输出流
        }
    }catch (IOException e){
        e.printStackTrace();
    }
}
```
### 3.文件字符流FileReader和FileWriter 

> 字符流不同于字节，字符流是以一个具体的字符进行读取，因此它只适合读纯文本的文件，如果是其他类型的文件不适用：

```java
public static void main(String[] args) {
    try(FileReader reader = new FileReader("test.txt")){
          reader.skip(1);   //现在跳过的是一个字符
        System.out.println((char) reader.read());   //现在是按字符进行读取，而不是字节，因此可以直接读取到中文字符
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 同理，字符流只支持char[]类型作为存储：

```java
public static void main(String[] args) {
    try(FileReader reader = new FileReader("test.txt")){
        char[] str = new char[10];
        reader.read(str);
        System.out.println(str);   //直接读取到char[]中
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 既然有了Reader肯定也有Writer：

```java
public static void main(String[] args) {
    try(FileWriter writer = new FileWriter("output.txt")){
       writer.getEncoding();   //支持获取编码（不同的文本文件可能会有不同的编码类型）
       writer.write('牛');
       writer.append('牛');   //其实功能和write一样
       writer.flush();   //刷新
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 通过File对象，我们就能快速得到文件的所有信息，如果是文件夹，还可以获取文件夹内部的文件列表等内容：

```java
File file = new File("/");
System.out.println(Arrays.toString(file.list()));   //快速获取文件夹下的文件名称列表
for (File f : file.listFiles()){   //所有子文件的File对象
    System.out.println(f.getAbsolutePath());   //获取文件的绝对路径
}
```

> 如果我们希望读取某个文件的内容，可以直接将File作为参数传入字节流或是字符流：

```java
File file = new File("test.txt");
try (FileInputStream inputStream = new FileInputStream(file)){   //直接做参数
    System.out.println(inputStream.available());
}catch (IOException e){
    e.printStackTrace();
}
```
### 4.练习：拷贝文件夹下的所有文件到另一个文件夹

```java
package CopyTest;

import java.io.*;

public class CopyHelper {

    //文件夹的拷贝
    public static void CopyDir(String sourcePathDir, String newPathDir) {
        File start = new File(sourcePathDir);
        File end = new File(newPathDir);
        String[] filePath = start.list();//获取该文件夹下的所有文件以及目录的名字
        if(!end.exists()) {
            end.mkdir();
        }
        boolean flag = false;
        for(String temp : filePath) {
            //添加满足情况的条件
            if(new File(sourcePathDir + File.separator + temp ).isFile()) {
                //为文件则进行拷贝
                flag = CopyFile(sourcePathDir + File.separator + temp, newPathDir + File.separator+temp );
            }
            if(flag){
                System.out.println("文件:" + temp + ",复制成功！");
            }else{
                System.out.println("文件:" + temp + ",复制失败！");
            }
        }
    }

    //文件的拷贝
    public static boolean CopyFile(String sourcePath, String newPath) {
        boolean flag = false;
        File readfile = new File(sourcePath);
        File newFile = new File(newPath);
        BufferedWriter bufferedWriter = null;
        Writer writer = null;
        FileOutputStream fileOutputStream = null;
        BufferedReader bufferedReader = null;
        try{
            fileOutputStream = new FileOutputStream(newFile, true);
            writer = new OutputStreamWriter(fileOutputStream,"UTF-8");
            bufferedWriter = new BufferedWriter(writer);

            bufferedReader = new BufferedReader(new FileReader(readfile));

            String line = null;
            while((line = bufferedReader.readLine()) != null){
                bufferedWriter.write(line);
                bufferedWriter.newLine();
                bufferedWriter.flush();
            }
            flag = true;
        } catch(IOException e) {
            flag = false;
            e.printStackTrace();
        } finally {
            try {
                if(bufferedWriter != null){
                    bufferedWriter.close();
                }
                if(bufferedReader != null){
                    bufferedReader.close();
                }
                if(writer != null){
                    writer.close();
                }
                if(fileOutputStream != null){
                    fileOutputStream.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return flag;
    }
}
```
```java

import CopyTest.CopyHelper;

public class Main {
    public static void main(String[] args) {
        String sourcePath = "D:\\wpf02";
        String newPath = "D:\\wpf03";
        System.out.print("From：" + sourcePath);
        System.out.print("To：" + newPath);
        CopyHelper.CopyDir(sourcePath, newPath);
    }
}
```

> 运行测试即可。
### 5.缓冲流
> 虽然普通的文件流读取文件数据非常便捷，但是每次都需要从外部I/O设备去获取数据，由于外部I/O设备的速度一般都达不到内存的读取速度，很有可能造成程序反应迟钝，因此性能还不够高，而缓冲流正如其名称一样，它能够提供一个缓冲，提前将部分内容存入内存（缓冲区）在下次读取时，如果缓冲区中存在此数据，则无需再去请求外部设备。同理，当向外部设备写入数据时，也是由缓冲区处理，而不是直接向外部设备写入。
![在这里插入图片描述](https://img-blog.csdnimg.cn/6cb3de08b9e54f0f98acf9968524e77a.png)

要创建一个缓冲字节流，只需要将原本的流作为构造参数传入BufferedInputStream即可：

```java
public static void main(String[] args) {
    try (BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("test.txt"))){   //传入FileInputStream
        System.out.println((char) bufferedInputStream.read());   //操作和原来的流是一样的
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

> 实际上进行I/O操作的并不是BufferedInputStream，而是我们传入的FileInputStream。

> BufferedOutputStream，其实和BufferedInputStream原理差不多，只是反向操作：

```java
public static void main(String[] args) {
    try (BufferedOutputStream outputStream = new BufferedOutputStream(new FileOutputStream("output.txt"))){
        outputStream.write("lbwnb".getBytes());
        outputStream.flush();
    }catch (IOException e) {
        e.printStackTrace();
    }
}
```
## 14.多线程
### 1.线程的创建和启动

> 通过创建Thread对象来创建一个新的线程,可以直接使用lambda表达式：

> 创建好后，通过调用start()方法来运行此线程：

```java
    public static void main(String[] args) {
        Thread t = new Thread(() -> {    //直接编写逻辑
            System.out.println("我是另一个线程！");
        });
        t.start();   //调用此方法来开始执行此线程
    }
```

> 可能上面的例子看起来和普通的单线程没两样，那我们先来看看下面这段代码的运行结果：

```java

public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("我是线程："+Thread.currentThread().getName());
            System.out.println("我正在计算 0-10000 之间所有数的和...");
            int sum = 0;
            for (int i = 0; i <= 10000; i++) {
                sum += i;
            }
            System.out.println("结果："+sum);
        });
        t.start();
        System.out.println("我是主线程！");
    }
}
```

我们发现，这段代码执行输出结果并不是按照从上往下的顺序了，因为他们分别位于两个线程，他们是同时进行的！如果你还是觉得很疑惑，我们接着来看下面的代码运行结果：

```java
public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 50; i++) {
            System.out.println("我是一号线程："+i);
        }
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 50; i++) {
            System.out.println("我是二号线程："+i);
        }
    });
    t1.start();
    t2.start();
}
```

> 我们可以看到打印实际上是在交替进行的，也证明了他们是在同时运行！

> 注意：我们发现还有一个run方法，也能执行线程里面定义的内容，但是run是直接在当前线程执行，并不是创建一个线程执行！
> 
![在这里插入图片描述](https://img-blog.csdnimg.cn/df14a13e4a0f456eb618b05a0260385e.png)
> 实际上，线程和进程差不多，也会等待获取CPU资源，一旦获取到，就开始按顺序执行我们给定的程序，当需要等待外部IO操作（比如Scanner获取输入的文本），就会暂时处于休眠状态，等待通知，或是调用sleep()方法来让当前线程休眠一段时间：

```java
public static void main(String[] args) throws InterruptedException {
    System.out.println("l");
    Thread.sleep(1000);    //休眠时间，以毫秒为单位，1000ms = 1s
    System.out.println("b");
    Thread.sleep(1000);
    System.out.println("w");
    Thread.sleep(1000);
    System.out.println("nb!");
}
```

> 我们也可以使用stop()方法来强行终止此线程：

```java
public static void main(String[] args) throws InterruptedException {
    Thread t = new Thread(() -> {
        Thread me = Thread.currentThread();   //获取当前线程对象
        for (int i = 0; i < 50; i++) {
            System.out.println("打印:"+i);
            if(i == 20) me.stop();  //此方法会直接终止此线程
        }
    });
    t.start();
}
```

> 虽然stop()方法能够终止此线程，但是并不是所推荐的做法，有关线程中断相关问题，我们会在后面继续了解。

> 思考：猜猜以下程序输出结果：

```java
private static int value = 0;

public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) value++;
        System.out.println("线程1完成");
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) value++;
        System.out.println("线程2完成");
    });
    t1.start();
    t2.start();
    Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
    System.out.println(value);
}
```

> 我们发现，value最后的值并不是我们理想的结果，有关为什么会出现这种问题，在我们学习到线程锁的时候，再来探讨。
### 2.线程的休眠和中断
> 一个线程处于运行状态下，线程的下一个状态会出现以下情况：
> 1. 当CPU给予的运行时间结束时，会从运行状态回到就绪（可运行）状态，等待下一次获得CPU资源。
> 2. 当线程进入休眠 / 阻塞(如等待IO请求) / 手动调用wait()方法时，会使得线程处于等待状态，当等待状态结束后会回到就绪状态。
> 3. 当线程出现异常或错误 / 被stop() 方法强行停止 / 所有代码执行结束时，会使得线程的运行终止。

这个部分我们着重了解一下线程的休眠和中断，首先了解一下如何使得线程进如休眠状态：

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        try {
            System.out.println("l");
            Thread.sleep(1000);   //sleep方法是Thread的静态方法，它只作用于当前线程（它知道当前线程是哪个）
            System.out.println("b");    //调用sleep后，线程会直接进入到等待状态，直到时间结束
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    });
    t.start();
}
```

> 注意：sleep方法是Thread的静态方法，它只作用于当前线程（它知道当前线程是哪个）。

> 通过调用sleep()方法来将当前线程进入休眠，使得线程处于等待状态一段时间。我们发现，此方法显示声明了会抛出一个InterruptedException异常，那么这个异常在什么时候会发生呢？

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        try {
            Thread.sleep(10000);  //休眠10秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    });
    t.start();
    try {
        Thread.sleep(3000);   //休眠3秒，一定比线程t先醒来
        t.interrupt();   //调用t的interrupt方法
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

> 我们发现，每一个Thread对象中，都有一个interrupt()方法，调用此方法后，会给指定线程添加一个中断标记以告知线程需要立即停止运行或是进行其他操作，由线程来响应此中断并进行相应的处理，我们前面提到的stop()方法是强制终止线程，这样的做法虽然简单粗暴，但是很有可能导致资源不能完全释放，而类似这样的发送通知来告知线程需要中断，让线程自行处理后续，会更加合理一些，也是更加推荐的做法。我们来看看interrupt的用法：

```java
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("线程开始运行！");
            while (true){   //无限循环
                if(Thread.currentThread().isInterrupted()){   //判断是否存在中断标志
                    break;   //响应中断
                }
            }
            System.out.println("线程被中断了！");
        });
        t.start();
        try {
            Thread.sleep(3000);   //休眠3秒，一定比线程t先醒来
            t.interrupt();   //调用t的interrupt方法
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
```

> 通过isInterrupted()可以判断线程是否存在中断标志，如果存在，说明外部希望当前线程立即停止，也有可能是给当前线程发送一个其他的信号，如果我们并不是希望收到中断信号就是结束程序，而是通知程序做其他事情，我们可以在收到中断信号后，复位中断标记，然后继续做我们的事情：

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        System.out.println("线程开始运行！");
        while (true){
            if(Thread.currentThread().isInterrupted()){   //判断是否存在中断标志
                System.out.println("发现中断信号，复位，继续运行...");
                Thread.interrupted();  //复位中断标记（返回值是当前是否有中断标记，这里不用管）
            }
        }
    });
    t.start();
    try {
        Thread.sleep(3000);   //休眠3秒，一定比线程t先醒来
        t.interrupt();   //调用t的interrupt方法
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

> 复位中断标记后，会立即清除中断标记。那么，如果现在我们想暂停线程呢？我们希望线程暂时停下，比如等待其他线程执行完成后，再继续运行，那这样的操作怎么实现呢？

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        System.out.println("线程开始运行！");
        Thread.currentThread().suspend();   //暂停此线程
        System.out.println("线程继续运行！");
    });
    t.start();
    try {
        Thread.sleep(3000);   //休眠3秒，一定比线程t先醒来
        t.resume();   //恢复此线程
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

> 虽然这样很方便地控制了线程的暂停状态，但是这两个方法我们发现实际上也是不推荐的做法，它很容易导致死锁！有关为什么被弃用的原因，我们会在线程锁继续探讨。
### 3.线程的优先级
> 实际上，Java程序中的每个线程并不是平均分配CPU时间的，为了使得线程资源分配更加合理，Java采用的是抢占式调度方式，优先级越高的线程，优先使用CPU资源！我们希望CPU花费更多的时间去处理更重要的任务，而不太重要的任务，则可以先让出一部分资源。线程的优先级一般分为以下三种：
> 1. MIN_PRIORITY 最低优先级
> 2. MAX_PRIORITY 最高优先级
> 3. NOM_PRIORITY 常规优先级

```java
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("线程开始运行！");
        });
        t.start();
        t.setPriority(Thread.MIN_PRIORITY);  //通过使用setPriority方法来设定优先级
    }
```

> 优先级越高的线程，获得CPU资源的概率会越大，并不是说一定优先级越高的线程越先执行！
### 4.线程的礼让和加入
> 我们还可以在当前线程的工作不重要时，将CPU资源让位给其他线程，通过使用yield()方法来将当前资源让位给其他同优先级线程：

```java
public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        System.out.println("线程1开始运行！");
        for (int i = 0; i < 50; i++) {
            if(i % 5 == 0) {
                System.out.println("让位！");
                Thread.yield();
            }
            System.out.println("1打印："+i);
        }
        System.out.println("线程1结束！");
    });
    Thread t2 = new Thread(() -> {
        System.out.println("线程2开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println("2打印："+i);
        }
    });
    t1.start();
    t2.start();
}
```

> 观察结果，我们发现，在让位之后，尽可能多的在执行线程2的内容。

> 当我们希望一个线程等待另一个线程执行完成后再继续进行，我们可以使用join()方法来实现线程的加入：

```java
public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        System.out.println("线程1开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println("1打印："+i);
        }
        System.out.println("线程1结束！");
    });
    Thread t2 = new Thread(() -> {
        System.out.println("线程2开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println("2打印："+i);
            if(i == 10){
                try {
                    System.out.println("线程1加入到此线程！");
                    t1.join();    //在i==10时，让线程1加入，先完成线程1的内容，在继续当前内容
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    });
    t1.start();
    t2.start();
}
```

> 我们发现，线程1加入后，线程2等待线程1待执行的内容全部执行完成之后，再继续执行的线程2内容。注意，线程的加入只是等待另一个线程的完成，并不是将另一个线程和当前线程合并！我们来看看：

```java
public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        System.out.println(Thread.currentThread().getName()+"开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println(Thread.currentThread().getName()+"打印："+i);
        }
        System.out.println("线程1结束！");
    });
    Thread t2 = new Thread(() -> {
        System.out.println("线程2开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println("2打印："+i);
            if(i == 10){
                try {
                    System.out.println("线程1加入到此线程！");
                    t1.join();    //在i==10时，让线程1加入，先完成线程1的内容，在继续当前内容
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    });
    t1.start();
    t2.start();
}
```

> 实际上，t2线程只是暂时处于等待状态，当t1执行结束时，t2才开始继续执行，只是在效果上看起来好像是两个线程合并为一个线程在执行而已。

### 5.线程锁和线程同步

> 在开始讲解线程同步之前，我们需要先了解一下多线程情况下Java的内存管理：
> 
![在这里插入图片描述](https://img-blog.csdnimg.cn/ef4680c4ddad4ec395b8ac60aeb253ff.png)


> 线程之间的共享变量（比如之前悬念中的value变量）存储在主内存（main
> memory）中，每个线程都有一个私有的工作内存（本地内存），工作内存中存储了该线程以读/写共享变量的副本。它类似于我们在计算机组成原理中学习的多核心处理器高速缓存机制：

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0fd8e670c994a65aaec4bc8ceca56bb.png)

> 高速缓存通过保存内存中数据的副本来提供更加快速的数据访问，但是如果多个处理器的运算任务都涉及同一块内存区域，就可能导致各自的高速缓存数据不一致，在写回主内存时就会发生冲突，这就是引入高速缓存引发的新问题，称之为：缓存一致性。

> 实际上，Java的内存模型也是这样类似设计的，当我们同时去操作一个共享变量时，如果仅仅是读取还好，但是如果同时写入内容，就会出现问题！好比说一个银行，如果我和我的朋友同时在银行取我账户里面的钱，难道取1000还可能吐2000出来吗？我们需要一种更加安全的机制来维持秩序，保证数据的安全性！

> 比如我们可以来看看下面这个问题：

```java
private static int value = 0;

public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) value++;
        System.out.println("线程1完成");
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) value++;
        System.out.println("线程2完成");
    });
    t1.start();
    t2.start();
    Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
    System.out.println(value);
}
```

> 输出结果：

```java
线程1完成
线程2完成
15371
```

> 实际上，当两个线程同时读取value的时候，可能会同时拿到同样的值，而进行自增操作之后，也是同样的值，再写回主内存后，本来应该进行2次自增操作，实际上只执行了一次！

> 通过synchronized关键字来创造一个线程锁，首先我们来认识一下synchronized代码块，它需要在括号中填入一个内容，必须是一个对象或是一个类，我们在value自增操作外套上同步代码块：

```java
private static int value = 0;

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                synchronized (Main.class){  //使用synchronized关键字创建同步代码块
                    value++;
                }
            }
            System.out.println("线程1完成");
        });
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                synchronized (Main.class){
                    value++;
                }
            }
            System.out.println("线程2完成");
        });
        t1.start();
        t2.start();
        Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
        System.out.println(value);
    }
```

> 输出结果：

```java
线程1完成
线程2完成
20000
```

> 我们发现，现在得到的结果就是我们想要的内容了，因为在同步代码块执行过程中，拿到了我们传入对象或类的锁（传入的如果是对象，就是对象锁，不同的对象代表不同的对象锁，如果是类，就是类锁，类锁只有一个，实际上类锁也是对象锁，是Class类实例，但是Class类实例同样的类无论怎么获取都是同一个），但是注意两个线程必须使用同一把锁！

> 当一个线程进入到同步代码块时，会获取到当前的锁，而这时如果其他使用同样的锁的同步代码块也想执行内容，就必须等待当前同步代码块的内容执行完毕，在执行完毕后会自动释放这把锁，而其他的线程才能拿到这把锁并开始执行同步代码块里面的内容（实际上synchronized是一种悲观锁，随时都认为有其他线程在对数据进行修改。

> 那么下面来看看，如果使用的是不同对象的锁，那么还能顺利进行吗？

```java
private static int value = 0;

public static void main(String[] args) throws InterruptedException {
    Main main1 = new Main();
    Main main2 = new Main();
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) {
            synchronized (main1){
                value++;
            }
        }
        System.out.println("线程1完成");
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) {
            synchronized (main2){
                value++;
            }
        }
        System.out.println("线程2完成");
    });
    t1.start();
    t2.start();
    Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
    System.out.println(value);
}
```

> 输出结果：
```java
线程1完成
线程2完成
11745
```
> 当对象不同时，获取到的是不同的锁，因此并不能保证自增操作的原子性，最后也得不到我们想要的结果。

> synchronized关键字也可以作用于方法上，调用此方法时也会获取锁：

```java
private static int value = 0;

private static synchronized void add(){
    value++;
}

public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) add();
        System.out.println("线程1完成");
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) add();
        System.out.println("线程2完成");
    });
    t1.start();
    t2.start();
    Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
    System.out.println(value);
}
```
输出结果：

```java
线程1完成
线程2完成
20000
```

> 我们发现实际上效果是相同的，只不过这个锁不用你去给，如果是静态方法，就是使用的类锁，而如果是普通成员方法，就是使用的对象锁。通过灵活的使用synchronized就能很好地解决我们之前提到的问题了。
### 6.死锁
> 其实死锁的概念在操作系统中也有提及，它是指两个线程相互持有对方需要的锁，但是又迟迟不释放，导致程序卡住：

![在这里插入图片描述](https://img-blog.csdnimg.cn/57dffb89b644487f842111fd6f1ea6b9.png)

> 我们发现，线程A和线程B都需要对方的锁，但是又被对方牢牢把握，由于线程被无限期地阻塞，因此程序不可能正常终止。我们来看看以下这段代码会得到什么结果：

```java
public static void main(String[] args) throws InterruptedException {
    Object o1 = new Object();
    Object o2 = new Object();
    Thread t1 = new Thread(() -> {
        synchronized (o1){
            try {
                Thread.sleep(1000);
                synchronized (o2){
                    System.out.println("线程1");
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    });
    Thread t2 = new Thread(() -> {
        synchronized (o2){
            try {
                Thread.sleep(1000);
                synchronized (o1){
                    System.out.println("线程2");
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    });
    t1.start();
    t2.start();
}
```

> 所以，我们在编写程序时，一定要注意，不要出现这种死锁的情况。那么我们如何去检测死锁呢？我们可以利用jstack命令来检测死锁。
### 7.wait和notify方法
> 其实我们之前可能就发现了，Object类还有三个方法我们从来没有使用过，分别是wait()、notify()以及notifyAll()，他们其实是需要配合synchronized来使用的（实际上锁就是依附于对象存在的，每个对象都应该有针对于锁的一些操作，所以说就这样设计了）当然，只有在同步代码块中才能使用这些方法，正常情况下会报错，我们来看看他们的作用是什么：

```java
public static void main(String[] args) throws InterruptedException {
    Object o1 = new Object();
    Thread t1 = new Thread(() -> {
        synchronized (o1){
            try {
                System.out.println("开始等待");
                o1.wait();     //进入等待状态并释放锁
                System.out.println("等待结束！");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    });
    Thread t2 = new Thread(() -> {
        synchronized (o1){
            System.out.println("开始唤醒！");
            o1.notify();     //唤醒处于等待状态的线程
              for (int i = 0; i < 50; i++) {
                   System.out.println(i);   
            }
              //唤醒后依然需要等待这里的锁释放之前等待的线程才能继续
        }
    });
    t1.start();
    Thread.sleep(1000);
    t2.start();
}
```

> 输出结果：
```java
开始等待
开始唤醒！
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
等待结束！
```
### 8.ThreadLocal的使用
> 既然每个线程都有一个自己的工作内存，那么能否只在自己的工作内存中创建变量仅供线程自己使用呢？

> 我们可以使用ThreadLocal类，来创建工作内存中的变量，它将我们的变量值存储在内部（只能存储一个变量），不同的线程访问到ThreadLocal对象时，都只能获取到当前线程所属的变量。

```java
public static void main(String[] args) throws InterruptedException {
    ThreadLocal<String> local = new ThreadLocal<>();  //注意这是一个泛型类，存储类型为我们要存放的变量类型
    Thread t1 = new Thread(() -> {
        local.set("lbwnb");   //将变量的值给予ThreadLocal
        System.out.println("变量值已设定！");
        System.out.println(local.get());   //尝试获取ThreadLocal中存放的变量
    });
    Thread t2 = new Thread(() -> {
        System.out.println(local.get());   //尝试获取ThreadLocal中存放的变量
    });
    t1.start();
    Thread.sleep(3000);    //间隔三秒
    t2.start();
}
```
运行结果：

```java
变量值已设定！
lbwnb
null
```

> 上面的例子中，我们开启两个线程分别去访问ThreadLocal对象，我们发现，第一个线程存放的内容，第一个线程可以获取，但是第二个线程无法获取，我们再来看看第一个线程存入后，第二个线程也存放，是否会覆盖第一个线程存放的内容：

```java
 public static void main(String[] args) throws InterruptedException {
        ThreadLocal<String> local = new ThreadLocal<>();  //注意这是一个泛型类，存储类型为我们要存放的变量类型
        Thread t1 = new Thread(() -> {
            local.set("lbwnb");   //将变量的值给予ThreadLocal
            System.out.println("线程1变量值已设定！");
            try {
                Thread.sleep(2000);    //间隔2秒
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("线程1读取变量值：");
            System.out.println(local.get());   //尝试获取ThreadLocal中存放的变量
        });
        Thread t2 = new Thread(() -> {
            local.set("yyds");   //将变量的值给予ThreadLocal
            System.out.println("线程2变量值已设定！");
            System.out.println("线程2读取变量值：");
            System.out.println(local.get());
        });
        t1.start();
        Thread.sleep(1000);    //间隔1秒
        t2.start();
    }
```
运行结果：

```java
线程1变量值已设定！
线程2变量值已设定！
线程2读取变量值：
yyds
线程1读取变量值：
lbwnb
```

> 我们发现，即使线程2重新设定了值，也没有影响到线程1存放的值，所以说，不同线程向ThreadLocal存放数据，只会存放在线程自己的工作空间中，而不会直接存放到主内存中，因此各个线程直接存放的内容互不干扰。

> 我们发现在线程中创建的子线程，无法获得父线程工作内存中的变量：

```java
public static void main(String[] args) {
    ThreadLocal<String> local = new ThreadLocal<>();
    Thread t = new Thread(() -> {
       local.set("lbwnb");
        new Thread(() -> {
            System.out.println(local.get());
        }).start();
    });
    t.start();
}
```
运行结果：

```java
null
```

> 我们可以使用InheritableThreadLocal来解决：

```java
public static void main(String[] args) {
    ThreadLocal<String> local = new InheritableThreadLocal<>();
    Thread t = new Thread(() -> {
       local.set("lbwnb");
        new Thread(() -> {
            System.out.println(local.get());
        }).start();
    });
    t.start();
}
```
运行结果:

```java
lbwnb
```

> 在InheritableThreadLocal存放的内容，会自动向子线程传递。
### 9.定时器
> 我们有时候会有这样的需求，我希望定时执行任务，比如3秒后执行，其实我们可以通过使用Thread.sleep()来实现：


