@[TOC](目录)
# 书山有路勤为径，学海无涯苦作舟
最近在看《深入浅出WPF》一书，简单记录下学习笔记
## 1.Binding
在WPF中，Binding更注重于表达它是一种桥梁一样的关联关系。如果把Binding比作数据的桥梁，那么他的两端分别是Binding的源（Source）和目标（Target）,数据从哪里来哪里就是源，Binding是架在中间的桥梁，Binding目标就是数据要往哪里去（即把桥架向哪里）。因此，一般情况下，Binding源是逻辑层的对象，Binding目标是UI层的控件对象，这样，数据源就会源源不断地通过Binding送往UI层、被UI层展现，也就完成了数据驱动UI的过程。
### 1.1 Binding的源和路径
Binding的源也就是数据的源头，Binding对源的要求并不苛刻——只要它是一个对象，并且通过属性公开自己的数据，它就可以作为Binding的源。
#### 1.1.1 把控件作为Binding源与Binding标记扩展
大多数情况下Binding的源是逻辑层的对象，但是有时候为了让UI元素产生一些关联也会使用Binding在控件间建立关联。如下代码是把一个TextBox的Text属性关联在了Slider的Value属性上，
```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <StackPanel>
            <TextBox x:Name="textBox1" Text="{Binding ElementName=Slider1,Path=Value}" BorderBrush="Black" Margin="5"/>
            <Slider x:Name="Slider1" Maximum="100" Minimum="0" Margin="5"/>
        </StackPanel>
    </Grid>
</Window>

```
运行效果如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f2477f119e9f4a5595e6c08620ac8aaf.png)
#### 1.1.2  控制Binding的方向及数据更新
Binding在源和目标之间架起了沟通的桥梁，默认情况下数据既能通过Binding送达目标，也能够从目标返回源（收集用户对数据的修改）。有时候只需要将数据展示给用户、不允许用户修改，这个时候可以把Binding模式更改为从源向目标的单向沟通。Binding还可以支持从目标向源的单向沟通以及在Binding关系确立时读取一次数据，这个还需要根据实际情况去选择。
1. 控制Binding数据流向的属性是**Mode**，它的类型是BindingMode枚举，它的值有：
* TwoWay
* OneWay
* OnTime
* OneWayToSource
* Defalut
注意：Default的值是指Binding的模式会根据目标的实际情况来确定，比如若是可编辑的（如TextBox的Text属性），Defalut就会采用双向模式；若是只读的（如TextBox的Text属性）就会采用单向模式。
2. **UpdateSourceTrigger**属性，即改变源的数据所采用的触发方式，它有以下几种值：
* Default
* Explicit
* LostFocus（当目标的值发生改变并且失去焦点后，就立刻触发源的值也改变）
* PropertyChanged（当目标的值发生改变时，就立刻触发源的值也改变）
例子1：

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <StackPanel>
            <TextBox x:Name="textBox1" Text="{Binding ElementName=Slider1,Path=Value,UpdateSourceTrigger=PropertyChanged}" BorderBrush="Black" Margin="5"/>
            <Slider x:Name="Slider1" Maximum="100" Minimum="0" Margin="5"/>
        </StackPanel>
    </Grid>
</Window>

```
当在输入框中输入任意一个值时，Slider的值都会立马也跟着改变，
![在这里插入图片描述](https://img-blog.csdnimg.cn/4cbfbf5690bc42eba3b8792a6d0d2baa.png)
例子2：

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <StackPanel>
            <TextBox x:Name="textBox1" Text="{Binding ElementName=Slider1,Path=Value,UpdateSourceTrigger=LostFocus}" BorderBrush="Black" Margin="5"/>
            <Slider x:Name="Slider1" Maximum="100" Minimum="0" Margin="5"/>
        </StackPanel>
    </Grid>
</Window>

```
当在TextBox中输入一个数值，并且按下Tab键使其失去焦点时，Slider的值才会发生改变，
![在这里插入图片描述](https://img-blog.csdnimg.cn/fb2278d44fcc4f82bbc25fdeac7784f5.png)
#### 1.1.3  没有“Path”的Binding
有时候会在代码中看到一些Path是一个“.”或者干脆没有Path的Binding，着实让人摸不着头脑。这其实是一种比较特殊的情况——Binding源本身就是数据且不需要Path来指明，比如典型的string、int类型的数据，他们本身就是数据，因此我们无法指出通过它的哪个属性来访问这个数据，只需要将Path的值设置为“.”就可以了，比如以下代码举例：

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <StackPanel>
            <StackPanel.Resources>
                <sys:String x:Key="myString">
                    书山有路勤为径，学海无涯苦作舟
                </sys:String>
            </StackPanel.Resources>
            <TextBlock TextWrapping="Wrap" Text="{Binding Path=.,Source={StaticResource myString}}" FontSize="16" Margin="5"/>
            <!--<TextBlock TextWrapping="Wrap" Text="{Binding .,Source={StaticResource myString}}" FontSize="16" Margin="5"/>-->
        </StackPanel>
    </Grid>
</Window>
```
也可以简写成：

```xml
<TextBlock TextWrapping="Wrap" Text="{Binding .,Source={StaticResource myString}}" FontSize="16" Margin="5"/>
```

运行效果如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e99ec1760be044e3959a4c24022f5c9d.png)
#### 1.1.4 通过Binding的RelativeSource属性指定Source
当控件需要关注自己的、自己的容器的或者自己内部元素的某个值就需要使用这种方法。这种方法的意思是指当前元素和绑定源的位置关系。

**有时候，为了将控件自己作为数据源，我们会使用RelativeSource，这里有一个常见误区：“不明确指出Source的值Binding就会将控件自身作为数据的来源”这位句是错误的，因为不明确指出Source时Binding会把控件的DataContext属性当作数据源而非把控件自身当做数据源。**

常见的3种关系用法：
1. 第一种关系：Self
比如有一个TextBlock，如果想让TextBlock的width和height相同，通过设置属性Height="{Binding RelativeSource={RelativeSource  Mode=Self},Path=Width}" 就可以实现。
```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <StackPanel>
            <TextBlock Text="吴彦祖" Background="Red" TextWrapping="Wrap" Width="100" Height="{Binding RelativeSource={RelativeSource Mode=Self},Path=Width}"/>
        </StackPanel>
    </Grid>
</Window>


```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ff61a04fb8b84682a7b7a79b714c6384.png)

2. 第二种关系：TemplatedParent
例如为一个Button写一个样式，修改Button为椭圆型。同时需要椭圆的背景色和Button的背景色相同。

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Window.Resources>
        <Style x:Key="ButtonStyle01" TargetType="Button">
            <Setter Property="Background" Value="Green"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="Button">
                        <Grid>
                            <Ellipse>
                                <Ellipse.Fill>
                                    <SolidColorBrush Color="{Binding RelativeSource={RelativeSource Mode=TemplatedParent},Path=Background.Color}"/>
                                </Ellipse.Fill>
                            </Ellipse>
                        </Grid>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Window.Resources>
    <Grid>
        <Button Width="50" Height="50" Style="{StaticResource ButtonStyle01}"/>
    </Grid>
</Window>

```
运行效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0c54f7680e0d42b697e751e5557fd261.png)

3. 第三种关系：AncestorType
指定绑定源为某个父元素。
下面这个例子中Label的背景色将绑定至Grid的背景色。

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid Background="Purple">
        <Label Width="250" Height="50" Content="我的背景色和我的父类一样" Background = "{Binding RelativeSource={RelativeSource AncestorType={x:Type Grid}}, Path=Background.Color}"/>
    </Grid>
</Window>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/46cf12aa38c84d3a824aabec72b2cfc3.png)
#### 1.1.5 把ObjectDataProvider对象指定为Binding的Source
理想的情况下，上游程序员把类设计好、使用属性把数据暴露出来，下游程序员把这些类的实例作为Binding的Source、把属性作为Binding的Path来消费这些类。但是很难保证一个类的所有数据都使用属性暴露出来，比如我们需要的数据是方法的返回值，而重新设计底层类的风险和成本会比较高，况且黑盒引用类库的情况下我们也不可能更改已经编译好的类，这个时候就需要使用ObjectDataProvider来包装作为Binding源的数据对象了。

ObjectDataProvider，顾名思义就是把对象作为数据源提供给Binding。

接下来是例子：
先定义一个Calculator类，

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace BlankApp2.ViewModels
{
    public class Calculator
    {
        public double Add(double one, double two)

        {

            return one + two;

        }



        public string Add(string arg1, string arg2)

        {

            int x = 0;

            int y = 0;

            if (int.TryParse(arg1, out x) && int.TryParse(arg2, out y))

            {

                return this.Add(x, y).ToString();

            }

            else

            {

                return "Input Error!";

            }

        }

    }
}

```

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:vm="clr-namespace:BlankApp2.ViewModels"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Window.Resources>
        <ObjectDataProvider x:Key="ODP-Calculator" ObjectType="{x:Type vm:Calculator}" MethodName="Add">
            <ObjectDataProvider.MethodParameters>
                <sys:String>0</sys:String>
                <sys:String>0</sys:String>
            </ObjectDataProvider.MethodParameters>
        </ObjectDataProvider>
    </Window.Resources>
    <Grid>
        <StackPanel>　　
            <TextBox x:Name="textBox1" Margin="5" Text="{Binding Source={StaticResource ODP-Calculator}, Path=MethodParameters[0], BindsDirectlyToSource=True, UpdateSourceTrigger=PropertyChanged}"/>
            <TextBox x:Name="textBox2" Margin="5" Text="{Binding Source={StaticResource ODP-Calculator}, Path=MethodParameters[1], BindsDirectlyToSource=True, UpdateSourceTrigger=PropertyChanged}"/>
            <TextBox x:Name="textBox3" Margin="5" Text="{Binding Source={StaticResource ODP-Calculator}, Mode=OneWay}"/> 　　
        </StackPanel>
    </Grid>
</Window>
```
注：BindsDirectlyToSource=true这句话的意思是告诉bingding对象只负责把Ui元素收集到的数据写入器Source（即ObjectDataProvider对象），而不是被ObjectDataProvider封装的Calculator对象。

运行效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b903c2829ede48fa8405da8ba1e431bd.png)
### 1.2 Binding对数据的转换与校验
#### 1.2.1 校验
我们知道，Binding好比架设在Source和Target之间的桥梁，数据可以借助这个桥梁进行流通。在数据流通的过程中，我们可以在Binding这座桥梁上设置关卡，对数据的有效性进行验证。

我们利用Binding的ValidationRules（类型为Collection<ValidationRule）对数据进行验证。从它的名称和类型可以得知，我们可以为每个Binding设置多个数据校验条件，每个条件是一个ValidationRule对象，ValidationRule类是个抽象类，使用的时候，我们需要创建它的派生类并实现它的Validate方法。

Validate方法返回值是ValidationResult类型对象，如果校验通过，需要把ValidationResult对象的IsValid属性设置为true，反之，设置false并为其ErrorContent属性设置一个合适的消息内容，一般情况下是一个字符串。

下面看一个例子：
有一个Slider和一个TextBox，我们以Slider为源，TextBox为Target，Slider的取值范围为0 ~ 100，也就是说我们要需要校验TextBox中输入的值是不是在1~100这个范围内。

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using static ImTools.ImMap;
using System.Windows.Controls;

namespace BlankApp2.ViewModels
{
    public class RangeValidationRule : ValidationRule
    {
        public override ValidationResult Validate(object value, CultureInfo cultureInfo)
        {
            double myValue = 0;
            if (double.TryParse(value.ToString(), out myValue))
            {
                if (myValue >= 0 && myValue <= 100)
                {
                    return new ValidationResult(true, null);
                }
            }
            return new ValidationResult(false, "输入的数值应该在0和100之间");
        }
    }
}

```

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:vm="clr-namespace:BlankApp2.ViewModels"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Window.Resources>
        <vm:RangeValidationRule x:Key="RangeValidationRule"/>
    </Window.Resources>
    <Grid>
        <StackPanel>　
            <Slider Minimum="-30" Maximum="150" Name="slider01"/>
            <TextBox Width="200" Height="30">
                <TextBox.Text>
                    <Binding UpdateSourceTrigger="PropertyChanged" ElementName="slider01" Path="Value">
                        <Binding.ValidationRules>
                            <vm:RangeValidationRule ValidatesOnTargetUpdated="True" />
                        </Binding.ValidationRules>
                    </Binding>
                </TextBox.Text>
            </TextBox>

        </StackPanel>
    </Grid>
</Window>
```
注：默认情况下，Binding校验默认来自Source的数据总是正确的，只有来自Target的数据（Target多为UI控件，等价于用户的输入）才有可能出现问题，为了不让有问题的数据污染Source，所以需要校验。换句话说，Binding只在Target被外部更新时候进行校验，而来自Binding的Source数据更新Target时是不会进行校验的。

当来自Source的数据也有可能出现问题的时候，我们需要将校验条件的ValidatesOnTargetUpdated属性设置为true。

运行效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8b848e75060c43d599908195d92a28ba.png)
#### 1.2.2 转换
可以用转换器进行转换，转换器本人比较熟悉，这里就不叙述了。以前写的文章中有。

## 2.依赖属性
依赖属性就是一种可以自己没有值，并且通过使用Binding从数据源获得值（依赖在别人身上）的属性。拥有依赖属性的对象被称为“依赖对象”，与传统的CLR属性和面向对象思想相比依赖属性有很多新颖之处，包括：
* 节省实例对内存的开销；
* 属性值可以通过Binding依赖在其他对象上。

## 3.静态资源和动态资源
在WPF中，我们通常用两种方式来使用资源，静态资源和动态资源。

* 静态资源（StaticResource）指的是程序载入内存时对内存的一次性使用，之后就不再去访问这个资源了。
* 动态资源（DynamicResource）指的是在程序运行过程中仍然会访问资源。

因此，如果确定某些资源只在程序初始化的时候使用一次、之后不再发生改变，就应该使用StaticResource；而程序运行过程中还有可能改变的资源应该以DynamicResource形式来使用。

下面是一个例子，来说明静态资源和动态资源的区别：

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:vm="clr-namespace:BlankApp2.ViewModels"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        Title="{Binding Title}" Height="350" Width="525" >
    <Window.Resources>
        <TextBlock x:Key="res1" Text="海上生明月"/>
        <TextBlock x:Key="res2" Text="海上生明月"/>
    </Window.Resources>
    <Grid>
        <StackPanel>
            <Button Margin="5,5,5,0" Content="{StaticResource res1}"/>
            <Button Margin="5,5,5,0" Content="{DynamicResource res2}"/>
            <Button Margin="5,5,5,0" Content="更新内容" Click="Button_Click"/>
        </StackPanel>
    </Grid>
</Window>
```

```csharp
using System.Windows;
using System.Windows.Controls;

namespace BlankApp2.Views
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            this.Resources["res1"]=new TextBlock(){ Text="天涯共此时"};
            this.Resources["res2"] = new TextBlock() { Text = "天涯共此时" };
        }
    }


}

```
运行效果：
点击“更新内容”按钮后，第二个按钮的内容变了，而第一个按钮的内容没有变。
![在这里插入图片描述](https://img-blog.csdnimg.cn/be900978553e4943bd3184a765994c8e.png)
## 4.模板
WPF中的模板可分为两类：
* ControlTemplate，它决定了控件长成什么样子，并让程序员有机会在控件原有的内部逻辑基础上扩展自己的逻辑。
* DataTemplate，它是数据内容的表现形式，一条数据长成什么样子，是简单的文本还是直观的图形动画就由来它来决定。

简而言之，“Template”就是“外衣”，“ControlTemplate”是控件的外衣，“DataTemplate”是数据的外衣。

### 4.1 DataTemplate
DataTemplate最常用的地方有3处，分别是：
* ContentControl的ContentTemplate属性，相当于给ContentControl的内容穿衣服。
* ItemsControl的ItemTemplate属性，相当于给ItemsControl的数据条目穿衣服。
* GridViewColumn的CellTemplate属性，相当于给GridViewColumn单元格里的数据穿衣服。

例子如下：

```xml
<Window x:Class="BlankApp2.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:vm="clr-namespace:BlankApp2.ViewModels"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        xmlns:Converter="clr-namespace:BlankApp2.Converter"
        Title="{Binding Title}" Height="350" Width="525" >
    <Window.Resources>
        <Converter:AutoMakerToLogoPathConverter x:Key="AutoMakerToLogoPathConverter"/>
        <Converter:NameToPhotoPathConverter x:Key="NameToPhotoPathConverter"/>

        <DataTemplate x:Key="carDetailViewTemplate">
            <Border BorderBrush="Black" BorderThickness="1" CornerRadius="10" Margin="5">
                <StackPanel>
                    <Image Width="400" Height="250" Source="{Binding Name,Converter={StaticResource NameToPhotoPathConverter}}"/>
                    <StackPanel Orientation="Horizontal" Margin="5,0">
                        <TextBlock Text="Name:" FontWeight="Bold" FontSize="20"/>
                        <TextBlock Text="{Binding Name}" FontSize="20" Margin="5,0"/>
                    </StackPanel>
                    <StackPanel Orientation="Horizontal" Margin="5,0">
                        <TextBlock Text="Automaker:" FontWeight="Bold"/>
                        <TextBlock Text="{Binding Automaker}" Margin="5,0"/>
                        <TextBlock Text="Year:" FontWeight="Bold"/>
                        <TextBlock Text="{Binding Year}" Margin="5,0"/>
                        <TextBlock Text="TopSpeed:" FontWeight="Bold"/>
                        <TextBlock Text="{Binding TopSpeed}" Margin="5,0"/>
                    </StackPanel>
                </StackPanel>
            </Border>
        </DataTemplate>

        <DataTemplate x:Key="carListItemViewTemplate">
            <Grid Margin="2">
                <StackPanel Orientation="Horizontal">
                    <Image Source="{Binding Automaker,Converter={StaticResource AutoMakerToLogoPathConverter}}" Width="64" Height="64"/>
                    <StackPanel Orientation="Vertical">
                        <TextBlock Margin="5,0" Text="{Binding Name}" FontSize="16" FontWeight="Bold"/>
                        <TextBlock Text="{Binding Year}" FontSize="14"/>
                    </StackPanel>
                </StackPanel>
            </Grid>
        </DataTemplate>
    </Window.Resources>
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="3*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
            <UserControl Grid.Column="0" Content="{Binding ElementName=listBoxCars,Path=SelectedItem}" ContentTemplate="{StaticResource carDetailViewTemplate}"/>
            <ListBox Grid.Column="1" x:Name="listBoxCars" Width="180" Margin="5,0" ItemsSource="{Binding CarList}" ItemTemplate="{StaticResource carListItemViewTemplate}"/>
        
    </Grid>
</Window>
```
运行结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/403c60cb3dc243eaaf28555914c23f74.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/db3529d2655b4fb493bbd61da94f4d82.png)
完整项目下载链接：[仓库地址](https://gitee.com/hezexi/blog-code-example/tree/%E3%80%8A%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BAWPF%E3%80%8B%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0DataTemplate%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%8101/)

### 4.2 ControlTemplate
1.可以用Blend软件来快速设计控件的外观。
#### 4.2.1 ItemsControl的PanelTemplate
ItemsControl具有一个名为ItemsPanel的属性，它的数据类型为ItemsPanelTemplate。ItemsPanelTemplate也是一种控件Template，它的作用就是让程序员有机会控制ItemsControl的条目容器。
![在这里插入图片描述](https://img-blog.csdnimg.cn/ffa86a8e3bc04b5dbb8309366536dd92.png#pic_center)


举个例子，像ListBox这种控件，它的条目一般都是自上而下排列的，如果客户要求我们制作一个条目水平排列的ListBox该怎么办呢？现在，我们只需要调整ListBox的ItemsPanel属性，请看如下代码：
这是一个ListBox，它的条目是竖直排列：

```xml
        <ListBox Grid.Column="2">
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
        </ListBox>
```
把代码改成如下：

```xml
        <ListBox Grid.Column="2">
            <ItemsControl.ItemsPanel>
                <ItemsPanelTemplate>
                    <StackPanel Orientation="Horizontal"/>
                </ItemsPanelTemplate>
            </ItemsControl.ItemsPanel>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
            <TextBlock Text="Allan"/>
        </ListBox>
```
条目就会包装在一个水平排列的StackPanel中，从而横向排列，效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/84ec402c876749ffb542f332926517a2.png)

## 5.Style
Style直译过来就是“样式、风格”，构成Style样式最重要的2种元素是Setter和Trigger，Setter类帮助我们设置控件的静态风格，Trigger类则帮助我们设置控件的行为风格。

### 5.1 Style中的Setter
Setter类的Property属性来指明你想为目标的哪个属性赋值；Setter类中的Value属性则是你提供的属性值。
### 5.2 Style中Trigger
Trigger类则包含基本Trigger、MultiTrigger、DataTrigger、MultiDatatrigger4种。

#### 5.2.1 基本Trigger
基本的Trigger就不提了，用过很多次。
#### 5.2.2 MultiTrigger：
多个条件同时成立时才会被触发，MultiTrigger比Trigger多了一个Conditions属性，需要同时成立的条件就存储在这个集合中。下面是一个简短的代码举例：
当某个CheckBox同时满足被选中、内容为“悄悄地我走了，正如我悄悄地来”2个条件时，触发器才会被触发。
![在这里插入图片描述](https://img-blog.csdnimg.cn/a1baccd1a7a14a1bb032668c353a12ba.gif#pic_center)

```csharp
<Window x:Class="BlankApp3.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525"  WindowStartupLocation="CenterScreen">
    <Window.Resources>
        <Style TargetType="CheckBox">
            <Style.Triggers>
                <MultiTrigger>
                    <MultiTrigger.Conditions>
                        <Condition Property="IsChecked" Value="true"/>
                        <Condition Property="Content" Value="悄悄地我走了，正如我悄悄地来"/>
                    </MultiTrigger.Conditions>
                    <MultiTrigger.Setters>
                        <Setter Property="FontSize" Value="20"/>
                        <Setter Property="Foreground" Value="Orange"/>
                    </MultiTrigger.Setters>
                </MultiTrigger>
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <StackPanel>
        <CheckBox Content="悄悄地我走了，正如我悄悄地来"/>
        <CheckBox Content="我挥一挥衣袖，不带走任何一片云彩"/>
    </StackPanel>
</Window>

```
#### 5.2.3 由数据触发的DataTrigger
DataTrigger对象的Binding属性会把数据源源不断地送过来，一旦送来的值和Value属性一致，DataTrigger即被触发。

下面例子中，当TextBoxd的Text长度低于6时，其Border会保持红色。
![在这里插入图片描述](https://img-blog.csdnimg.cn/11d22649f0764ea0a66c651554f0441e.gif#pic_center)

```xml
<Window x:Class="BlankApp3.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:converter="clr-namespace:BlankApp3.Converter"
        Title="{Binding Title}" Height="350" Width="525"  WindowStartupLocation="CenterScreen">
    <Window.Resources>
        <converter:JudegeTextLengthConverter x:Key="JudegeTextLengthConverter"/>
        <Style TargetType="TextBox">
            <Style.Triggers>
                <DataTrigger Binding="{Binding RelativeSource={RelativeSource Mode=Self}, Path=Text.Length, Converter={StaticResource JudegeTextLengthConverter}}" Value="false">
                    <Setter Property="BorderBrush" Value="Red"/>
                    <Setter Property="BorderThickness" Value="3"/>
                </DataTrigger>
                
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <StackPanel>
        <TextBox Margin="5"/>
        <TextBox Margin="5,0"/>
        <TextBox Margin="5"/>
    </StackPanel>
</Window>
```
#### 5.2.4 由数据条件触发的MultiDataTrigger
有时我们会遇到要求多个数据条件同时满足时才能触变化的需求，此时可以考虑使用MultiDataTrigger。比如有这样一个需求：用户在界面上使用ListBox显示了一列Student数据，当Student对象同时满足ID为2、Name为Tom的时候，条目就高亮显示。

```xml
<Style TargetType="ListBox">
            <Style.Triggers>
                <MultiDataTrigger>
                    <MultiDataTrigger.Conditions>
                        <Condition Binding="{Binding Path=ID}" Value="2"/>
                        <Condition Binding="{Binding Path=Name}" Value="Tom"/>
                    </MultiDataTrigger.Conditions>
                    <MultiDataTrigger.Setters>
                        <Setter Property="Background" Value="Orange"/>
                    </MultiDataTrigger.Setters>
                </MultiDataTrigger>
            </Style.Triggers>
        </Style>
```
#### 5.2.6 由事件触发的EventTrigger
EventTrigger是触发器中最特殊的一个。首先，它不是由属性值或数据的变化来触发而是由事件来触发；其次，被触发后它并非应用一组Setter，而是执行一段动画。因此，UI层的动画效果往往与EventTrigger相关联。

在下面这个例子中，创建了一个针对Button的Style，这个Style包含两个EventTrigger，一个由MouseEnter事件触发；另外一个由MouseLeave事件触发，XAML代码如下：

```xml
<Window x:Class="BlankApp3.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:converter="clr-namespace:BlankApp3.Converter"
        Title="{Binding Title}" Height="350" Width="525"  WindowStartupLocation="CenterScreen">
    <Window.Resources>
        <converter:JudegeTextLengthConverter x:Key="JudegeTextLengthConverter"/>
        <Style TargetType="Button">
            <Style.Triggers>
                <EventTrigger RoutedEvent="MouseEnter">
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation To="150" Duration="0:0:0.2" Storyboard.TargetProperty="Width"/>
                            <DoubleAnimation To="150" Duration="0:0:0.2" Storyboard.TargetProperty="Height"/>
                        </Storyboard>
                    </BeginStoryboard>
                </EventTrigger>
                <EventTrigger RoutedEvent="MouseLeave">
                    <BeginStoryboard>
                        <Storyboard>
                            <DoubleAnimation  Duration="0:0:0.2" Storyboard.TargetProperty="Width"/>
                            <DoubleAnimation  Duration="0:0:0.2" Storyboard.TargetProperty="Height"/>
                        </Storyboard>
                    </BeginStoryboard>
                </EventTrigger>
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <Canvas>
        <Button Content="测试按钮1" Width="80" Height="40"/>
    </Canvas>
</Window>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d6086357c04a4f82bafda7e600a283b3.gif#pic_center)

