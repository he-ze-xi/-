
@[TOC](目录)

>  记录工作中遇到的WPF知识点和问题，总结在此篇博客中。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c51088946e84609b197ae00068aa614.jpeg#pic_center)
### 1.Wpf中内置的控件
* 按钮：Button和RepeatButton。
* 数据显示：DataGrid、ListView、TreeView。
* 日期显示和选择：Calendar、DataPicker。
* 对话框:OpenFileDialog、PrintDIalog、SaveFileDialog。
* 数字墨迹：Incanvas、InkPresenter。
* 文档：DocumentViewer、FlowDocumentPageViewer、FlowDocumentPageReader、FlowDocumentScrollViewer、StickNoteControl。
* 输入：TextBox、RichTextBox、PasswordBox。
* 布局：Border、BulletDecorator、Canvas、DockPanel、Expander、Grid、GridSplitter 、GroupBox、Panel、ResizeGrip、Separator、ScrollBar、ScrollViewer、StackPanel、Thumb、Viewbox、VirtualizingStackPanel、Window、WrapPanel。
* 媒体：Image、MediaElenment、SoundplayerAction。
* 菜单：ContentMenu、Menu、ToolBar。
* 导航：Frame、Hyperlink、Page、NavigationWindow、TabControl。
* 选项：CheckBox、ComboBox、ListBox、RadioButton、Slider。
* 用户信息：AccessText、Label、Popup、ProgressBar、StatusBar、TextBlock、ToolTip。
### 2.Template模板
在WPF中有三种类型的模板,分别为为数据模板**DataTemplate** 、 控件模板**ControlTemplate**、面板模板**ItemsPanelTemplate**。
#### 1.ControlTemplate
ControlTemplate它决定了控件“长成什么样子”，并让开发者有机会在控件原有的内部逻辑基础上扩展自己的逻辑,它不仅能用于来定义控件的外观、样式, 还可通过控件模板的触发器(ControlTemplate.Triggers)修改控件的行为、响应动画等。
举例：
- 效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/fda7696fd8f541d1aa9f42d077dfb5c9.gif#pic_center)
- 代码：
```xml
<Window.Resources>
        <ControlTemplate TargetType="Button" x:Key="ButtonTemplate">
            <!--定义视觉树-->
            <Grid>
                <Ellipse Name="faceEllipse" Width="{TemplateBinding Button.Width}" Height="{TemplateBinding Control.Height}"  Fill="{TemplateBinding Button.Background}"/>
                <TextBlock Name="txtBlock" Margin="{TemplateBinding Button.Padding}" VerticalAlignment="Center"  HorizontalAlignment="Center"  Text="{TemplateBinding Button.Content}" />
            </Grid>
            <!--定义触发器-->
            <ControlTemplate.Triggers>
                <Trigger Property="Button.IsMouseOver"  Value="True">
                    <Setter Property="Button.Foreground" Value="Red" />
                    <Setter Property="Button.FontSize" Value="25"/>
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </Window.Resources>
    <UniformGrid Rows="2" Columns="2" Background="GreenYellow" Width="300" Height="300">
        <Button Content="One " Margin="1" Template="{StaticResource ButtonTemplate}"/>
        <Button Content="One " Margin="1" Template="{StaticResource ButtonTemplate}"/>
        <Button Content="One " Margin="1" Template="{StaticResource ButtonTemplate}"/>
        <Button Content="One " Margin="1" Template="{StaticResource ButtonTemplate}"/>
    </UniformGrid>
```
#### （2）数据模板（CellTemplate、ItemTemplate、ContentTemplate）
 1. 简介
![在这里插入图片描述](https://img-blog.csdnimg.cn/8948e8c4114b42a7aaa0d7cbcb35a68f.png#pic_center)
- Grid这种列表表格中修改Cell的数据格式, CellTemplate可以修改单元格的展示数据的方式。
- 针对列表类型的控件, 例如树形控件，下拉列表，列表控件, 可以修改其中的ItemTemplate。
- 修改ContentTemplate, 例UserControl控件的数据展现形式。

 2. CellTemplate 模板
通过实现在DataGrid表格的列中添加按钮，来练习CellTemplate 模板的使用：
效果如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/23e63cc5a547416c803a89c6b5f4c4c0.png#pic_center)
```xml
<DataGrid  Height="260" AutoGenerateColumns="False" IsReadOnly="True"  ItemsSource="{Binding Loglists}" SelectedItem="{Binding SelectedCurrent}">
                                                <DataGrid.Columns>
                                                    <DataGridTextColumn Width="80" Header="时间" Binding="{Binding Time}"/>
                                                    <DataGridTextColumn Width="80" Header="级别" Binding="{Binding Num}"/>
                                                    <DataGridTextColumn Width="80" Header="模块" Binding="{Binding Belong}"/>
                                                    <DataGridTextColumn Width="80" Header="消息" Binding="{Binding Message}"/>
                                                    <DataGridTextColumn Width="160" Header="线程ID" Binding="{Binding ID}"/>
                                                    <DataGridTemplateColumn Header="操作" Width="100" >
                                                        <DataGridTemplateColumn.CellTemplate>
                                                            <DataTemplate>
                                                                <StackPanel Orientation="Horizontal" VerticalAlignment="Center" HorizontalAlignment="Left">
                                                                    <Button Content="编辑"/>
                                                                    <Button Margin="8 0 0 0" Content="删除" />
                                                                </StackPanel>
                                                            </DataTemplate>
                                                        </DataGridTemplateColumn.CellTemplate>
                                                    </DataGridTemplateColumn>
                                                </DataGrid.Columns>
                                            </DataGrid>
```
3. ItemTemplate模板
通过实现在ComboBox和ListBox中绑定后台并设置DataTemplate ，来练习ItemTemplate模板的使用：
效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/b3f6db80ecee4a89bb693d8b29a50c42.png#pic_center)
```xml
<Window.Resources>
        <DataTemplate x:Key="comTemplate">
            <StackPanel Orientation="Horizontal" Margin="5,0">
                <Border Width="10" Height="10" Background="{Binding Cl}"/>
                <TextBlock Text="{Binding Cl}" Margin="5,0"/>
            </StackPanel>
        </DataTemplate>
    </Window.Resources>
    <Grid>
        <Grid>
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
                <ComboBox Name="cob" Width="120" Height="30" ItemTemplate="{StaticResource comTemplate}"/>
                <ListBox Name="lib" Width="120" Height="100" Margin="5,0"  ItemTemplate="{StaticResource comTemplate}"/>
            </StackPanel>
        </Grid>
    </Grid>
```
```csharp
public class ShowColorModel
    {
        public string Cl { get; set; }
    }
```
```csharp
 List<ShowColorModel> ColorList = new List<ShowColorModel>();
            ColorList.Add(new ShowColorModel() { Cl= "#FF8C00" });
            ColorList.Add(new ShowColorModel() { Cl = "#FF7F50" });
            ColorList.Add(new ShowColorModel() { Cl = "#FF6EB4" });
            ColorList.Add(new ShowColorModel() { Cl = "#FF4500" });
            ColorList.Add(new ShowColorModel() { Cl = "#FF3030" });
            ColorList.Add(new ShowColorModel() { Cl = "#CD5B45" });

            cob.ItemsSource = ColorList;
            lib.ItemsSource = ColorList;
```
而DataTemplate是数据内容的展示方式，一条数据显示成什么样子，是简单的文本还是直观的图形就由它来决定了。
#### （3）面板模板ItemsPanelTemplate
ItemsPanelTemplate用于定义集合控件的容器外观，如ListBox,Combox 等等。

### 4.对话框
（1）**消息框MessageBox**，是一个对话框，用于快速显示信息并允许用户做出决策。
使用方法如代码所示：

```xml
    <Grid>
        <Button Width="100" Height="30" Content="Click me" Click="Button_Click"/>
    </Grid>
```

```csharp
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            string messageBoxText = "是否要保存这些信息?";
            string caption = "文字处理";
            MessageBoxButton button = MessageBoxButton.YesNoCancel;//MessageBoxButton为MessageBox的按钮类型
            MessageBoxImage icon = MessageBoxImage.Warning;//MessageBoxImage为 MessageBox的图标类型
            MessageBoxResult result;// MessageBoxResult指定用户在消息框上单击的按钮

            result = MessageBox.Show(messageBoxText, caption, button, icon, MessageBoxResult.Yes);
            if (result == MessageBoxResult.Yes)
            {
                MessageBox.Show("点击了“是“");
            }
        }
    }
```
运行如图:
![在这里插入图片描述](https://img-blog.csdnimg.cn/d38b10510e504b02986178b2ca7a9271.gif#pic_center)
MessageBoxButton的属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/421fc5f69cfd4909912248d43388d157.png#pic_center)
MessageBoxImage的属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ba631132fef4d86b65e3f1d1202242c.png#pic_center)
MessageBoxResult的属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b7607b5ffc340c09d4fad2c7feead91.png#pic_center)
（2）**通用对话框 Dialog**
示例：“打开文件”对话框:
使用方法如代码所示：
```xml
    <Grid>
        <Button Width="100" Height="30" Content="Click me" Click="Button_Click"/>
    </Grid>
```

```csharp
  public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            // 打开一个文件对话框
            var dialog = new Microsoft.Win32.OpenFileDialog();//实例化一个打开文件的对话框
            dialog.FileName = "打开文件"; // 要打开文件的名称
            dialog.DefaultExt = ".txt"; // 获取或设置默认文件扩展名
            dialog.Filter = "文本文件(*.txt)|*.txt|所有文件(*.*)|*.*"; //获取或设置当前文件名筛选器字符串

            bool?result=dialog.ShowDialog();//显示该对话框（如果不调用该方法，则不会显示该对话框）,并用bool型的result接受该函数的返回结果
            // Process open file dialog box results
            if (result == true)
            {
                // Open document
                string filename = dialog.FileName;//获取要打开文件的名称
                MessageBox.Show("打开的文件名称为:"+filename);
            }
        }
    }
```
代码运行如图:
![在这里插入图片描述](https://img-blog.csdnimg.cn/d4604c3ba778491e8c7551f5e3fda524.gif#pic_center)
示例："保存文件"对话框:
使用方法如代码所示：
```xml
    <Grid>
        <Button Width="100" Height="30" Content="Click me" Click="Button_Click"/>
    </Grid>
```

```csharp
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            
            var dialog = new Microsoft.Win32.SaveFileDialog();//实例化一个保存文件的对话框
            dialog.FileName = "Document"; // 要保存文件的名称
            dialog.DefaultExt = ".txt"; // 要保存文件的默认类型
            dialog.Filter = "Text documents (.txt)|*.txt"; //获取或设置当前文件名筛选器字符串

            // Show save file dialog box
            bool? result = dialog.ShowDialog();//显示该对话框（如果不调用该方法，则不会显示该对话框）,并用bool型的result接受该函数的返回结果

            // Process save file dialog box results
            if (result == true)
            {
                // Save document
                string filename = dialog.FileName;//获取要保存文件的名称
                MessageBox.Show("保存的文件名称为:" + filename);
            }

        }
    }
```
代码运行如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/8292ffa290624f478405e406828c5d77.gif#pic_center)
示例：“打印”对话框:
使用方法如代码所示：

```xml
    <Grid>
        <Button Width="100" Height="30" Content="Click me" Click="Button_Click"/>
    </Grid>
```
```csharp
            var dialog = new System.Windows.Controls.PrintDialog();//实例化一个打印文件的对话框
            dialog.PageRangeSelection = System.Windows.Controls.PageRangeSelection.AllPages;
            dialog.UserPageRangeEnabled = true;
            bool? result = dialog.ShowDialog();
```
代码运行如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/d152371004f04ae0bbbcaa27ef0eaa4a.gif#pic_center)
### 5.ContentPresenter
ContentPresenter是一个基础控件，其他的控件可以继承他，主要作用是实现内容的显示，可以是任何内容。
### 6.画刷
* LinearGradientBrush是线性渐变画刷。
* SolidColorBrush 是纯色画刷。
* RadialGradientBrush是径向渐变画刷。
* ImageBrush是图片画刷。
* GradientBrush是渐变画刷。
### 7.路由事件
（1）Button中有个click事件，该事件就是定义好的路由事件。
（2）路由事件和依赖属性一样，也需要注册，使用 EventManager.RegisterRoutedEvent方法来注册路由事件。
（3）自定义一个路由事件的例子：

 1. 首先添加一个用户控件，名为UserControl1，如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/4ab34eafac17483c8c1ba57060d2719d.png#pic_center)
以下代码中包含前三个步骤，
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace WpfApp2.CustomControls
{
    /// <summary>
    /// UserControl1.xaml 的交互逻辑
    /// </summary>
    public partial class UserControl1 : UserControl
    {
        public UserControl1()
        {
            InitializeComponent();
        }
        //1.声明并注册路由事件，并使用冒泡策略
            //
            // 摘要:
            //     向WPF事件系统注册新的路由事件。
            // public static RoutedEvent RegisterRoutedEvent(string name, RoutingStrategy routingStrategy, Type handlerType, Type ownerType);
            //
            // 参数:
            //   name:
            //     路由事件的名称。 该名称在所有者类型中必须是唯一的，并且不能为 null 或空字符串。
            //
            //   routingStrategy:
            //     作为枚举值的事件的路由策略。
            //
            //   handlerType:
            //     事件处理程序的类型。 该类型必须为委托类型，并且不能为 null。
            //
            //   ownerType:
            //     路由事件的所有者类类型。 该类型不能为 null。
            //
            // 返回结果:
            //     新注册的路由事件的标识符。 现在可将该标识符对象存储为类中的静态字段，然后将其用作将处理程序附加到事件的方法的参数。 路由事件标识符也用于其他事件系统 API。
        public static readonly RoutedEvent MyRoutedEvent = EventManager.RegisterRoutedEvent("MyRoutedEventHandler", RoutingStrategy.Bubble, typeof(RoutedEventHandler), typeof(UserControl1));

        //2、通过.NET事件包装路由事件
        public event RoutedEventHandler MyRoutedEventHandler
        {
            add
            {
                AddHandler(MyRoutedEvent, value);
            }
            remove
            {
                RemoveHandler(MyRoutedEvent, value);
            }
        }

        //3、使用按钮的单击事件激发路由事件
        private void Btn_Test_Click(object sender, RoutedEventArgs e)
        {
            RoutedEventArgs arg = new RoutedEventArgs();
            arg.RoutedEvent = MyRoutedEvent;
            RaiseEvent(arg);
        }
    }
}

```
到目前为止，路由事件的准备工作已经完成，接下里开始最后一个步骤：调用该用户控件，并实现路由事件即可。
```csharp
    <Grid>
        <!--在MainWindow.xaml文件中引入自定义的UserControl1用户控件-->
        <CustomControls:UserControl1 MyRoutedEventHandler="UserControl1_MyRoutedEventHandler" HorizontalAlignment="Center" Height="100"  VerticalAlignment="Center" Width="100"/>
    </Grid>
```
最后在MainWindow.xaml.cs文件中实现该路由事件的具体功能即可：

```csharp
        private void UserControl1_MyRoutedEventHandler(object sender, RoutedEventArgs e) //路由事件具体实现(当单击Test按钮后就会触发这个路由事件)
        {
            MessageBox.Show("Hello:" + e.Source.ToString());
        }
```
运行结果如图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/1aac352b9b05410fa6d185d5f01f977c.png#pic_center)
单击Test按钮后，会弹出右边的那个对话框，成功实现单击Test按钮触发自定义的路由事件。
### 8.依赖属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/4934978d68864601af43b1620078f640.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f53a3d8b424a4aadb807d273fb77aa33.png)
#### 1.先看一个例子
先举个例子，在WPF中如果我们自定义了一个叫ButtonExpand的用户控件，该控件继承自按钮Button类，这时我们想向ButtonExpand控件中添加一张背景图Image类以实现背景图效果，而Button类中没有这个Image属性，我们就可以在这时注册一个依赖项属性，此时该控件就有了这个新的属性，在前台xaml文件中我们就可以直接使用这个新的属性。
一个简短的例子来注册并使用依赖属性：

 1. 首先建立一个用户控件StreamingStatusButton，用于自定义控件外观：
![在这里插入图片描述](https://img-blog.csdnimg.cn/4989ab8b63ca476cbe141729681feffe.png#pic_center)

 2. 其次在StreamingStatusButton.xaml文件中实现代码（那个ImageSource属性是即将要注册的依赖属性）：

```xml
<UserControl x:Class="WpfApp1.customcontrols.StreamingStatusButton"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:WpfApp1.customcontrols"
             mc:Ignorable="d" x:Name="this">

    <UserControl.Resources>
        <Style x:Key="btnStyle" TargetType="Button">
            <Style.Triggers>
                <Trigger Property="IsMouseOver" Value="True">
                    <Setter Property="RenderTransform">
                        <Setter.Value>
                            <ScaleTransform ScaleX="1.2" ScaleY="1.2"/>
                            <!--ScaleTransform表示缩放;ScaleX表示X轴缩小值，正常为1;ScaleY表示Y轴缩小值，正常为1 -->
                        </Setter.Value>
                    </Setter>
                </Trigger>
            </Style.Triggers>
        </Style>
    </UserControl.Resources>
    <Grid>
        <Button Background="Transparent" BorderThickness="0" Style="{StaticResource btnStyle}">
            <Border x:Name="border" Background="Transparent" CornerRadius="10" RenderTransformOrigin="0.5,0.5" Height="27" Width="27" BorderThickness="1">
                <Rectangle StrokeThickness="2" Stretch="Uniform">
                    <Rectangle.Fill>
                        <ImageBrush ImageSource="{Binding ImageSource,ElementName=this}"/>
                    </Rectangle.Fill>
                </Rectangle>
            </Border>
        </Button>

    </Grid>
</UserControl>

```
其次在StreamingStatusButton.xaml.cs后台文件中注册ImageSource依赖属性：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace WpfApp1.customcontrols
{
    /// <summary>
    /// StreamingStatusButton.xaml 的交互逻辑
    /// </summary>
    public partial class StreamingStatusButton : UserControl
    {
        public StreamingStatusButton()
        {
            InitializeComponent();
        }

        //注册依赖属性（快捷键：输入propdp,连续按两次tab键即可快捷注册依赖属性）
        public Uri ImageSource
        {
            get { return (Uri)GetValue(ImageSourceProperty); }
            set { SetValue(ImageSourceProperty, value); }
        }
        // Using a DependencyProperty as the backing store for ImageSource.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty ImageSourceProperty =
            DependencyProperty.Register("ImageSource", typeof(Uri), typeof(StreamingStatusButton));
    }
}

```
 1. 然后在MainWindow.xaml文件中引入该自定义控件，并给刚才注册的ImageSource依赖属性赋值即可。
```xml
<customcontrols:StreamingStatusButton  ImageSource="assets/s1.png" />
```
#### 2.WPF为什么需要依赖属性
以上是一个使用依赖属性的简单例子，那么WPF为什么需要依赖属性？

 WPF的设计理念是：数据驱动，UI与逻辑松耦合。
 
1. 什么是依赖属性？
依赖属性是一种可以自已没有值，但是可以通过Binding方式，从数据源（依赖别人的数据）获得值的属性。

2. 为什么要有依赖属性
传统的CLR属性：

```csharp
public class Person
{
    private string _Name;
    public string Name
    {
        get
        {
             return _Name;
         }
         set
         {
            _Name = value;
         }
     }
 }
```
CLR属性存在的问题：

在多继承的情况下，每次继承，父类的字段都被继承，孙孙辈对象占用内存空间不可避免的膨胀。

3. 如何添加依赖属性？
在多继承，大多数字段并没有被修改的情况下，如何少对象的体积。
数据驱动指导思想下，数据如何保存简单一致，同步

```csharp
// 1. 使类型继承DependencyObject类
    public class Person : DependencyObject
    {
        // 2. 声明一个静态只读的DependencyProperty 字段
        public static readonly DependencyProperty nameProperty;
        static Person()
        {
            // 3. 注册定义的依赖属性
            nameProperty = DependencyProperty.Register("Name", typeof(string), typeof(Person), 
                new PropertyMetadata("Learning Hard",OnValueChanged)); 
        }
        // 4. 属性包装器，通过它来读取和设置我们刚才注册的依赖属性
        public string Name
        {
            get { return (string)GetValue(nameProperty); }
            set { SetValue(nameProperty, value); }
        }
        private static void OnValueChanged(DependencyObject dpobj, DependencyPropertyChangedEventArgs e)
        {
            // 当只发生改变时回调的方法
        }
    }
```
4. 依赖属性的优势：
* 解决多继承，且大多数字段值不改变的情况下，减少内存占比
将一个DependencyProperty对象存储在一个全局的Hashtable中；通过依赖对象(DependencyObject)的GetValue和SetValue存取数据；

* 以数据为中心，当数据源改变时，所以关联的UI数据改变
依赖属性值可以通过Binding依赖于其它对象上，这就使得数据源一变动；依赖于此数据源的依赖属性全部进行更新
#### 3.什么时候需要定义依赖属性？
* 希望支持动态资源引用

* 希望支持动画

* 希望支持数据绑定

* 希望支持属性值继承

* 希望该属性发生改变时触发一系列的行为

* 希望该属性有自己的元数据

* 希望在样式中使用该属性

* 希望得到WPF样式器的支持，比如在wpf窗口中直接修改该属性

### 9.命令Command
一个简单地创建一些Command命令的例子：
 2. 如下图，在Base文件中，创建一个CommandBase类，该类存放了command命令的初始化工作

![在这里插入图片描述](https://img-blog.csdnimg.cn/27720f2c6de34de98b47be7a47367a48.png#pic_center)
CommandBase.cs代码如下：

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Input;

namespace WpfApp1.Base
{
   public class CommandBase:ICommand //继承自ICommand接口
    {
       // 当出现影响是否应执行该命令的更改时发生。
        public event EventHandler CanExecuteChanged;

        //
        // 摘要:
        //     定义确定此命令是否可在其当前状态下执行的方法。
        //
        // 参数:
        //   parameter:
        //     此命令使用的数据。 如果此命令不需要传递数据，则该对象可以设置为 null。
        //
        // 返回结果:
        //     如果可执行此命令，则为 true；否则为 false。
        public bool CanExecute(object parameter)
        {
            return true;
        }

        //
        // 摘要:
        //     定义在调用此命令时要调用的方法。
        //
        // 参数:
        //   parameter:
        //     此命令使用的数据。 如果此命令不需要传递数据，则该对象可以设置为 null。
        public void Execute(object parameter)
        {
            DoExecute?.Invoke();
        }

        public Action DoExecute { get; set; }

        public CommandBase(Action doExecute)
        {
            this.DoExecute = doExecute;
        }
    }
}

```
2. 在ViewModels文件夹下，有一个MainViewModel类，该类存放了主界面MainWindow的一些需要绑定的数据和命令。该类代码如下：

```csharp

//这个类存放了MainWindow界面中一些Command命令的定义


using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using WpfApp1.Base;
using System.Windows.Input;
using System.Windows;

namespace WpfApp1.ViewModels
{
   public class MainViewModel
    {
        public ICommand btnSearchCommand { get; set; }
        public ICommand btnContactCommand { get; set; }
        public MainViewModel()
        {
           
            //“搜索按钮”的命令
            btnSearchCommand = new CommandBase(() =>
            {
                string messageBoxText = "搜索失败";
                string caption = "搜索";
                MessageBox.Show(messageBoxText, caption);
            });

            //“联系我们按钮”的命令
            btnContactCommand = new CommandBase(() =>
            {
                string messageBoxText = "我们的客服电话是：1234567890";
                string caption = "联系我们";
                MessageBox.Show(messageBoxText, caption);
            });
        }

    }
}

```
到此，command命令已经定义好，接下来直接在前台.xaml文件中使用即可：

```xml
 <Button x:Name="btnSearch" Command="{Binding btnSearchCommand}"  Height="50" Style="{StaticResource MaterialDesignRaisedButton}" Content="搜索" />
```

```xml
<Button x:Name="btnContact" Command="{Binding btnContactCommand}"/>
```
### 10.给样式赋值 {x:Null} 可以清除样式
### 11.MVVM模式
（1）MVVM:Model-View-ViewModel
（2）为什么使用MVVM模式：
 1. 团队层面 ：统一的思维方式和实现方法；
 2. 架构层面：稳定、解耦、富有禅意；
 3. 代码层面：可读、可测、可替换。

（3）什么是Model?
现实世界中对象的抽象结果。比如，现实世界中有一些学生，那么对应到Model里面就是Student类；有一些动物，那么对应到Model就是Animals。
（4）什么是View和ViewModel?
 - View=UI,VIew也就是一些前台的xaml界面；
 - ViewModel=Model for View;
 - ViewModel与View的沟通依靠两种方式：用于传递数据的数据属性、用于传递操作的命令属性。
### 12.ObservableCollection和List的比较
![在这里插入图片描述](https://img-blog.csdnimg.cn/cca0d436de48486fafd061123a97a9cf.png#pic_center)
ObservableCollection是一个动态集合，已经实现了INotifyPropertyChanged接口，不需要再实现因此PropertyChanged方法，因此能自动刷新前台页面。而List集合则没有实现INotifyPropertyChanged接口，我们需要自己实现List集合的PropertyChanged方法，才能在前台页面实现动态刷新。
### 13.MergedDictionaries 属性
该属性相当于对ResourceDictionary资源字典进行分类。
### 14.margin和padding
margin是自己与父容器的间距；
padding是自己与子控件的间距。
### 15.Convert
什么时候用到Convert？
在wpf中，常常会遇到这种情况：给定一组数据，但是我们需要在前台xaml界面中把它转化为相对应的其他的数据类型。例如，我们定义了一个学生类，如下：
```csharp
    public class Student
    {
        public string ID { get; set; }
        public string Name { get; set; }
        public string Sex { get; set; }
    }
```
而此时客户给出的是这种的数据：Sex属性并不是“男”或者“女”，而是0或1。而我们在前台界面呈现出的应该是“男”或“女”，而不是显示0或1，此时我们就需要用到convert转化，把客户输入的0转化为“男”，1转化为“女”。
```csharp
 new Student() { ID = "1", Name = "Peter", Sex = "0" },
                new Student() { ID = "2", Name = "Tom", Sex = "1" },
                new Student() { ID = "3", Name = "Ben", Sex = "0" }

```
所以此时定义一个类SexConvert，该类用于将string类型的0或1转化为string类型的男或女，

```csharp
public  class SexConvert: IValueConverter
    {
       public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            string result = "";
            string sex = (string)value;
            if (sex == "0")
            {
                result = "女";
            }
            else if (sex == "1")
            {
                result = "男";
            }
            else
            {
                result = "人妖";
            }
            return result;
        }
        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            return null;
        }
    }
```
这时只需要在前台xaml页面引入即可，在控件里这样写：把该lc:SexConvert 类以资源的形式添加进窗口资源中：

```xml
    <Window.Resources>
        <lc:SexConvert x:Key="IFC"/>
    </Window.Resources>
```
然后在控件里的使用方法如下：
```xml
<GridViewColumn Header="性别" Width="150" DisplayMemberBinding="{Binding SexList,Converter={StaticResource IFC}}"/>
```
此时就成功实现convert转化了。
### 16.INotifyPropertyChanged接口
当前台页面和后台ViewModel中的数据源通过binding绑定后，如果后台数据源发生改变，那么修改后的数据是不会自动刷新到前台xaml页面中的，这是我们需要用到INotifyPropertyChanged接口了。当类继承INotifyPropertyChanged接口并实现后，如果后台数据修改，那么修改后的数据就会自动刷新到前台页面中了。
一般来说，定义一个INotifedChangedBse 类，来继承这个INotifyPropertyChanged接口，以后其他的ViewModel类要想再使用这个接口，就可以直接继承INotifedChangedBse 类，不用再每次使用时都实现接口的方法了，减少了代码的重复性，很方便。如下图：
```csharp
    //定义一个INotifedChangedBse类，该类继承自INotifyPropertyChanged接口，当后台数据中数据发生改变时，可以通知前台也发生相应变化。
  public  class INotifedChangedBse : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
        public void RaiseProperChanged(string propertyName)
        {
            if (this.PropertyChanged != null)
            {
                this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
            }
        }
    }
```
比如有一个ShowCourseWindowViewModel 类，该类继承额了INotifedChangedBse类，那么这个ShowCourseWindowViewModel 类自然也拥有INotifedChangedBse类的RaiseProperChanged方法了，如下图：
```csharp
public class ShowCourseWindowViewModel : INotifedChangedBse
    {
        public ICommand btnTestCommand { get; set; }
        private string _btnTest;
        public string BtnTest
        {
            get { return _btnTest; }
            set { _btnTest = value; RaiseProperChanged("BtnTest"); }//当BtnTest属性发生变化时，前台也会自动刷新数据，以应对变化。
        }
        public ShowCourseWindowViewModel()
        {
            BtnTest = "未点击";
            btnTestCommand = new CommandBase(() =>
            {
                if (BtnTest == "已经点击")
                {
                    BtnTest = "未点击";
                }
                else
                {
                    BtnTest = "已经点击";
                }
            });
        }
    }
```
### 17.DataTrigger
定义：当绑定的数据满足指定的条件时,应用(指定的)属性或执行操作的触发器；
### 18.用PathGeometry绘制图形
用法如下：
```xml
    <!--这里存放一些图标的绘制情况-->
    <!--PathGeometry是用来绘制路径图形,PathGeometry类表示一个可能由弧、曲线、椭圆、直线和矩形组成的复杂形状。-->
    <PathGeometry x:Key="logo" Figures="M864.4352 385.8496l42.0992 461.7856L518.4 996.544l-388.1344-148.9152 41.472-454.8864L0 294.4 512 3.2384 1024 294.4l-159.5648 91.4496zM742.208 455.104L512 585.5616 293.952 462.0032l-27.4176 300.8128L518.4 859.456l251.8656-96.64-28.0512-307.712zM512 150.3616L257.8112 294.4 512 438.4384 766.1888 294.4 512 150.3616zM921.6 422.4l102.4-51.2v256h-83.2l-19.2-204.8z"></PathGeometry>
    <PathGeometry x:Key="home" Figures="M938.412716 480.847489 556.823881 104.126522c-1.89721-2.258437-6.142907-6.194072-12.037151-10.081613-9.920954-6.544043-21.162995-10.520611-33.496905-10.520611-13.180184 0-24.690332 3.997034-34.357506 10.623965-5.846148 4.010337-9.876951 8.102538-12.157901 11.154038L84.209914 480.894561c-14.700817 14.481829-14.700817 38.014802 0 52.529377 14.585183 14.427594 33.995255 8.343015 50.529837-7.945973l25.607214-25.314549 38.918381-37.700647c-1.359974 1.686409-2.282996 2.963495-2.284019 3.373841L196.981327 847.484797c0 50.006927 40.941458 90.427522 91.442642 90.427522l141.713582 0 10.423397 0L440.560948 927.486876 440.560948 686.440961c0-17.467837 14.411221-31.683607 32.223912-31.683607l77.051887 0c17.814738 0 32.223912 14.21577 32.223912 31.683607L582.060659 927.486876l0 10.425444 10.425444 0 141.713582 0c50.485835 0 91.442642-40.428781 91.442642-90.427522L825.642327 467.251843l62.246693 61.207014c20.142759 13.266142 37.897122 17.434068 50.523697 4.963035C953.114556 518.910387 953.114556 495.361041 938.412716 480.847489z"></PathGeometry>
    <PathGeometry x:Key="discover" Figures="M512 972.8c-253.44 0-460.8-207.36-460.8-460.8S258.56 51.2 512 51.2s460.8 207.36 460.8 460.8-207.36 460.8-460.8 460.8z m0-51.2c225.28 0 409.6-184.32 409.6-409.6S737.28 102.4 512 102.4 102.4 286.72 102.4 512s184.32 409.6 409.6 409.6z m0-204.8c-143.36 0-281.6-104.96-281.6-204.8s138.24-204.8 281.6-204.8 281.6 104.96 281.6 204.8-138.24 204.8-281.6 204.8z m0-51.2c117.76 0 230.4-87.04 230.4-153.6s-112.64-153.6-230.4-153.6-230.4 87.04-230.4 153.6 112.64 153.6 230.4 153.6z m0-51.2c-56.32 0-102.4-46.08-102.4-102.4s46.08-102.4 102.4-102.4 102.4 46.08 102.4 102.4-46.08 102.4-102.4 102.4z m0-51.2c28.16 0 51.2-23.04 51.2-51.2s-23.04-51.2-51.2-51.2-51.2 23.04-51.2 51.2 23.04 51.2 51.2 51.2z"></PathGeometry>
    <PathGeometry x:Key="messages" Figures="M512 640v42.666667H341.333333v-213.333334h170.666667v170.666667z m170.666667-256v42.666667H341.333333V341.333333h341.333334v42.666667zM213.333333 213.333333h597.333334v597.333334H213.333333V213.333333z m85.333334 85.333334v426.666666h426.666666V298.666667H298.666667z m256 298.666666h128v85.333334h-128v-85.333334z m0-128h128v85.333334h-128v-85.333334z"></PathGeometry>
    <PathGeometry x:Key="settings" Figures="M1017.358145 622.659875c-0.264999-3.879991-1.299997-7.459983-3.029993-10.519976-0.085-0.239999-0.14-0.499999-0.225-0.739998l-0.314999-0.08c-2.679994-4.24999-6.634985-7.359983-11.563974-8.299981l-126.810714-24.459945c4.42999-21.300952 6.799985-43.279902 6.799985-65.779852 0-23.539947-2.614994-46.499895-7.469983-68.719845l79.214821-30.670931c-0.08 0.19-0.154 0.379999-0.233999 0.569999l16.964961-7.049984 27.594938-10.689976c1.594996-0.609999 2.979993-1.519997 4.25499-2.539994l14.265968-5.919987c2.309995-11.739974 3.738992-23.759946 4.009991-36.069918 2.444994-111.469749-82.524814-205.479536-195.875558-224.419494-3.919991-1.339997-7.868982-1.909996-11.568974-1.669996-0.274999-0.03-0.539999-0.08-0.814998-0.11l-0.205 0.239999c-5.264988 0.559999-9.914978 2.669994-12.834971 6.489986l-75.409829 98.249778a378.612146 378.612146 0 0 0-68.764845-35.43092l35.51992-120.389728c1.334997-4.52099 0.165-9.209979-2.684994-13.36997l0.063999-0.3c-0.2-0.181-0.403999-0.339999-0.608998-0.509998-2.129995-2.829994-5.090989-5.329988-8.689981-7.310984-47.089894-37.719915-107.209758-55.648874-166.932623-52.879881-59.727865-2.769994-119.83973 15.159966-166.939623 52.879881-3.597992 1.979996-6.557985 4.47999-8.684981 7.310984-0.205 0.17-0.414999 0.328999-0.609998 0.509998l0.064999 0.3c-2.849994 4.159991-4.017991 8.84998-2.684994 13.36997l35.52192 120.389728a378.124147 378.124147 0 0 0-68.764845 35.43092l-75.409829-98.249778c-2.927993-3.819991-7.572983-5.930987-12.834971-6.489986l-0.208-0.239999c-0.271999 0.03-0.541999 0.08-0.814998 0.11-3.697992-0.239999-7.649983 0.329999-11.572974 1.669996C85.718247 146.20995 0.753438 240.220738 3.195433 351.689487c0.267999 12.310972 1.697996 24.339945 4.007991 36.069918l14.264968 5.919987c1.277997 1.020998 2.659994 1.929996 4.25499 2.539994l27.596938 10.689976 16.964961 7.049984c-0.083-0.19-0.155-0.379999-0.237999-0.569999l79.212821 30.680931c-4.849989 22.20995-7.466983 45.169898-7.466983 68.709845 0 22.499949 2.366995 44.4789 6.801985 65.779852L21.783391 603.02092c-4.927989 0.938998-8.88298 4.049991-11.562974 8.299981l-0.316999 0.08c-0.083 0.239999-0.14 0.499999-0.22 0.739998-1.731996 3.059993-2.769994 6.639985-3.034993 10.519976-27.191939 104.159765 30.87293 214.479516 141.417681 257.43942 12.211972 4.749989 24.604944 8.389981 37.071916 11.159974l11.584974-9.739978c1.511997-0.679998 2.964993-1.499997 4.211991-2.599994l21.656951-19.180956 13.782969-11.589974c-0.215-0.01-0.432999-0.029-0.654999-0.029l61.729861-54.659877c21.94995 14.559967 45.702897 26.889939 70.84984 36.769917l-35.149921 119.130731c-1.332997 4.51999-0.165 9.198979 2.684994 13.35897l-0.064999 0.3c0.195 0.181 0.404999 0.340999 0.609998 0.520998 2.126995 2.819994 5.086989 5.319988 8.684981 7.299984 47.099894 37.719915 107.211758 55.659874 166.939623 52.879881 59.722865 2.779994 119.84273-15.159966 166.932623-52.879881 3.599992-1.978996 6.560985-4.47899 8.689981-7.299984 0.205-0.18 0.408999-0.339999 0.608998-0.520998l-0.063999-0.3c2.849994-4.159991 4.019991-8.83998 2.684994-13.35897l-35.149921-119.130731c25.149943-9.869978 48.89989-22.20995 70.85084-36.769917l61.729861 54.659877c-0.22 0-0.444999 0.02-0.654999 0.029l13.779969 11.589974 21.659951 19.180956c1.244997 1.099998 2.693994 1.919996 4.209991 2.599994l11.584974 9.739978c12.464972-2.770994 24.864944-6.409986 37.069916-11.159974 110.550751-42.959903 168.61162-153.280654 141.420681-257.44042zM855.55351 834.680397c-1.344997 0.520999-2.704994 0.978998-4.059991 1.478997l-109.984752-97.389781c-8.139982-7.209984-22.909948-5.089989-32.975925 4.75999-0.929998 0.909998-1.709996 1.889996-2.493995 2.850993-25.880942 18.659958-54.920876 33.698924-86.285805 44.2399l0.15 0.239c-1.019998 0.181-2.034995 0.289999-3.063993 0.549999-14.090968 3.609992-23.085948 14.689967-20.109955 24.790944l36.834917 124.858718c-35.46492 24.419945-78.714822 35.68092-121.557726 33.220925-42.849903 2.459994-86.097806-8.80098-121.560726-33.220925L427.283476 816.200439c2.976993-10.100977-6.022986-21.180952-20.109955-24.790944-1.026998-0.259999-2.044995-0.368999-3.066993-0.549999l0.152-0.239c-31.367929-10.540976-60.412864-25.579942-86.294805-44.2399-0.784998-0.970998-1.559996-1.939996-2.489995-2.850993-10.064977-9.849978-24.827944-11.969973-32.972925-4.75999l-109.989752 97.389781c-1.352997-0.499999-2.711994-0.958998-4.059991-1.478997C86.803244 802.939469 41.988345 723.960647 55.786314 646.720821l136.229693-26.270941c11.009975-2.119995 17.53496-14.629967 14.569967-27.929937a30.139932 30.139932 0 0 0-1.539997-4.890989c-6.716985-23.908946-10.364977-48.96989-10.364976-74.849831 0-25.020944 3.382992-49.279889 9.682978-72.469836 0.22-0.439999 0.499999-0.829998 0.704998-1.290997 1.429997-3.198993 2.229995-6.448985 2.474995-9.599979 0.035-0.109 0.065-0.22 0.098-0.329999l-0.098-0.029c0.586999-9.02998-3.481992-17.079961-11.117975-20.040955l-140.159684-54.278877c-0.06-1.350997-0.163-2.689994-0.189999-4.050991-1.804996-82.329814 57.70987-152.389656 139.204685-171.858612l80.864818 105.358762c6.534985 8.509981 21.467952 9.00998 33.349925 1.109997 0.764998-0.509999 1.407997-1.100998 2.109995-1.649996l0.143 0.171c29.204934-22.14095 62.687859-39.569911 99.162776-50.979885l-0.777998-1.239997c12.322972-4.31999 19.907955-14.449967 17.149961-23.799947l-36.837917-124.849718c35.46292-24.429945 78.709822-35.67992 121.560726-33.219925 42.842903-2.459994 86.092806 8.78998 121.557726 33.219925l-36.834917 124.850718c-2.759994 9.359979 4.819989 19.478956 17.148961 23.799947l-0.778998 1.239997c36.474918 11.409974 69.959842 28.839935 99.164776 50.979885l0.145-0.16c0.699998 0.538999 1.340997 1.129997 2.104995 1.639996 11.879973 7.898982 26.80994 7.398983 33.350925-1.119997l80.863818-105.349762c81.494816 19.469956 141.010682 89.528798 139.200685 171.858612-0.025 1.359997-0.125 2.699994-0.189999 4.050991l-140.154684 54.278877c-7.634983 2.960993-11.704974 11.010975-11.119975 20.040955l-0.1 0.029c0.04 0.11 0.064 0.229999 0.1 0.329999 0.244999 3.149993 1.049998 6.399986 2.479995 9.599979 0.199 0.460999 0.478999 0.839998 0.698998 1.279997a275.914378 275.914378 0 0 1 9.685978 72.479836c0 25.869942-3.645992 50.939885-10.364976 74.839831a30.063932 30.063932 0 0 0-1.539997 4.899989c-2.964993 13.29997 3.559992 25.809942 14.569967 27.929937l136.225693 26.270941c13.798969 77.240826-31.01693 156.219648-112.666746 187.960576z  M512.006285 339.720514c-102.234769 0-185.110582 77.139826-185.110582 172.279611s82.874813 172.279611 185.110582 172.279611c102.222769 0 185.102582-77.139826 185.102582-172.279611s-82.879813-172.279611-185.102582-172.279611z m0 296.119332c-73.020835 0-132.217702-55.099876-132.217702-123.059723s59.196866-123.060722 132.217702-123.060722c73.017835 0 132.217702 55.100876 132.217702 123.060722s-59.199866 123.059722-132.217702 123.059723z"></PathGeometry>
    <PathGeometry x:Key="light" Figures="M642.160874 894.856008l0 11.1857q0 5.084409 0.508441 11.1857t0.508441 12.202582q-1.016882 19.320755-10.677259 42.200596t-37.116187 33.048659q-11.1857 4.067527-28.981132 11.694141t-53.386296 7.626614q-30.506455 0-50.33565-7.118173t-31.014896-9.151936q-14.236346-3.050645-22.879841-12.711023t-13.219464-21.862959-5.59285-24.913605-1.016882-22.879841l0-31.523337zM862.82423 229.815293q26.438928 62.029791 30.506455 116.94141t-6.101291 101.179742-29.489573 82.875869-40.166832 64.063555-39.14995 45.251241-25.422046 23.896723q-10.168818 9.151936-18.812314 13.727905t-15.253227 7.626614-12.711023 7.118173-12.202582 13.219464q-13.219464 19.320755-20.337637 37.624628t-12.202582 35.590864q-5.084409 14.236346-11.1857 23.388282t-13.219464 16.270109q-8.135055 8.135055-16.270109 13.219464l-223.714002 0q-8.135055-5.084409-15.253227-13.219464-7.118173-7.118173-14.236346-18.303873t-12.202582-28.472691q-10.168818-29.489573-24.913605-47.285005t-38.133069-35.082423q-16.270109-12.202582-37.116187-32.540218t-41.183714-47.793446-38.641509-61.521351-29.489573-74.740814-13.219464-86.943396 9.151936-97.112214q17.286991-81.350546 59.996028-136.770606t95.586892-88.97716 109.314796-48.301887 102.196624-14.744786q47.793446 0 99.654419 13.219464t100.16286 41.183714 88.468719 71.181728 65.588878 104.230387zM760.119166 437.259186q43.725919-177.95432-113.890765-271.507448-26.438928-16.270109-61.521351-25.422046t-71.690169-9.151936-72.707051 9.151936-64.571996 28.472691q-69.147964 47.793446-93.553128 103.721946t-22.3714 117.958292q1.016882 34.573982 11.694141 62.029791t25.422046 49.82721 30.506455 39.14995 27.96425 29.998014q25.422046 26.438928 44.7428 46.268123t30.506455 50.33565q11.1857 29.489573 35.082423 36.099305t44.234359 6.609732q26.438928 0 50.844091-11.694141t34.573982-36.099305q5.084409-14.236346 20.846077-34.573982t47.285005-56.945382q16.270109-19.320755 31.014896-33.5571t27.455809-28.472691 22.3714-31.014896 15.761668-41.183714z"></PathGeometry>
    <PathGeometry x:Key="medal" Figures="M435.73248 134.2464L342.97856 28.59008C456.56064 0.43008 582.0416 0.3072 695.64416 28.38528l-92.79488 105.86112a644.75136 644.75136 0 0 0-167.1168 0z m191.40608 220.93824l133.87776-152.71936c8.8064-10.0352 12.16512-23.71584 8.99072-36.67968l-30.49472-125.29664-192.83968 219.97568 80.46592 94.72zM847.0528 682.92608c0 193.06496-167.85408 347.66848-365.28128 325.57056C330.73152 991.58016 209.12128 868.78208 193.47456 717.59872c-15.1552-146.65728 67.23584-276.1728 189.93152-332.43136a324.1984 324.1984 0 0 1 40.71424-15.70816L277.66784 202.42432a40.96 40.96 0 0 1-8.99072-36.67968l30.43328-125.0304 220.2624 250.88 58.81856 69.2224c27.0336 4.93568 52.8384 13.2096 77.14816 24.35072 112.92672 51.77344 191.71328 165.62176 191.71328 297.75872z m-122.88 0c0-113.11104-91.70944-204.8-204.8-204.8-113.11104 0-204.8 91.68896-204.8 204.8s91.68896 204.8 204.8 204.8c113.09056 0 204.8-91.68896 204.8-204.8z"></PathGeometry>
    <PathGeometry x:Key="progress" Figures="   M359.850667 726.016c22.058667-23.466667 105.045333-93.141333 106.112-94.037333a46.293333 46.293333 0 0 1 60.074666 0c1.066667 0.896 84.053333 70.570667 106.112 94.037333l0.725334 0.768a24.533333 24.533333 0 0 1-18.133334 41.216H377.258667a24.533333 24.533333 0 0 1-18.133334-41.216l0.725334-0.768z m249.386666-229.248a29.013333 29.013333 0 0 0-10.325333 21.76 29.013333 29.013333 0 0 0 10.325333 21.76c7.168 6.186667 15.402667 13.056 24.149334 20.352 28.288 23.552 63.488 52.864 91.562666 84.394667a28.757333 28.757333 0 0 1-2.432 40.704 29.098667 29.098667 0 0 1-40.96-2.432c-25.173333-28.330667-57.173333-54.954667-85.376-78.421334-8.96-7.466667-17.408-14.506667-25.002666-21.077333-19.2-16.64-30.250667-40.448-30.250667-65.28 0-24.874667 11.008-48.64 30.293333-65.28 7.253333-6.314667 15.36-13.098667 23.893334-20.224 46.933333-39.296 125.568-105.045333 125.568-152.448V235.861333a28.970667 28.970667 0 0 0-29.013334-28.842666H300.288a28.970667 28.970667 0 0 0-29.013333 28.842666v44.714667c0 47.402667 78.634667 113.152 125.610666 152.448 8.533333 7.125333 16.64 13.909333 23.893334 20.181333 19.242667 16.64 30.293333 40.448 30.293333 65.28 0 24.874667-11.050667 48.682667-30.293333 65.322667-7.253333 6.314667-15.36 13.056-23.893334 20.224-46.933333 39.253333-125.568 105.002667-125.568 152.448v44.714667c0 15.872 13.013333 28.842667 29.013334 28.842666h391.338666c16 0 29.013333-12.970667 29.013334-28.842666 0-15.957333 12.970667-28.842667 29.013333-28.842667 15.957333 0 28.970667 12.885333 28.970667 28.842667a86.869333 86.869333 0 0 1-86.997334 86.528H300.288A86.869333 86.869333 0 0 1 213.333333 801.194667V756.48c0-74.325333 84.48-144.981333 146.218667-196.608 8.362667-6.997333 16.298667-13.610667 23.210667-19.584a29.013333 29.013333 0 0 0 10.325333-21.76 29.013333 29.013333 0 0 0-10.325333-21.76c-6.912-5.973333-14.848-12.629333-23.210667-19.626667C297.856 425.514667 213.333333 354.858667 213.333333 280.576V235.861333C213.333333 188.16 252.330667 149.333333 300.288 149.333333h391.381333c47.957333 0 86.997333 38.826667 86.997334 86.528v44.714667c0 74.282667-84.522667 144.981333-146.261334 196.565333-8.362667 6.997333-16.256 13.653333-23.168 19.626667z m-81.92-80.298667c-17.194667 14.933333-42.538667 14.933333-59.733333 0-0.554667-0.426667-20.778667-17.749333-43.818667-38.016-18.048-15.914667-6.954667-45.952 16.981334-45.952h113.365333c23.936 0 35.072 30.037333 17.024 45.952-23.04 20.266667-43.306667 37.546667-43.818667 37.973334z"></PathGeometry>
    <PathGeometry x:Key="detail" Figures=" M121.9 259.8h489.9c31.5 0 57.1-28.6 57.1-63.9s-25.5-63.9-57-63.9h-490c-31.5 0-57.1 28.6-57.1 63.9s25.6 63.9 57.1 63.9zM128.7 575.9h766.6c35.3 0 63.9-28.6 63.9-63.9s-28.6-63.9-63.9-63.9H128.7c-35.3 0-63.9 28.6-63.9 63.9s28.6 63.9 63.9 63.9zM895.3 764.2H128.7c-35.3 0-63.9 28.6-63.9 63.9S93.4 892 128.7 892h766.6c35.3 0 63.9-28.6 63.9-63.9s-28.6-63.9-63.9-63.9z"></PathGeometry>
    <PathGeometry x:Key="contact" Figures="M1023.245286 358.277182c-127.905661-42.399667-76.3194-122.252372-153.345461-153.34546a737.754198 737.754198 0 0 0-255.811321-51.586261 737.754198 737.754198 0 0 0-255.811322 51.586261C282.664444 236.02481 332.837382 316.584177 204.225061 358.277182S0 337.78401 0 257.224644a189.385177 189.385177 0 0 1 102.465861-154.758783C275.597833 0 614.088504 0 614.088504 0s337.78401 0 511.622643 102.465861a188.678516 188.678516 0 0 1 102.465861 153.34546c0 81.266028-70.666111 146.985511-204.931722 102.465861z  M790.04712 409.863443V322.237466H702.421142V409.863443H526.462526V322.237466H439.54321V409.863443l-353.330555 306.690922V918.659442a115.892422 115.892422 0 0 0 118.719067 105.292505h819.020225A117.305744 117.305744 0 0 0 1141.964352 918.659442v-204.931722zM614.795165 893.926303a175.958616 175.958616 0 1 1 175.251955-175.958616 175.958616 175.958616 0 0 1-175.251955 175.958616z"></PathGeometry>
    <PathGeometry x:Key="add" Figures="M512 149.333333c200.298667 0 362.666667 162.368 362.666667 362.666667s-162.368 362.666667-362.666667 362.666667S149.333333 712.298667 149.333333 512 311.701333 149.333333 512 149.333333z m0 64c-164.949333 0-298.666667 133.717333-298.666667 298.666667s133.717333 298.666667 298.666667 298.666667 298.666667-133.717333 298.666667-298.666667-133.717333-298.666667-298.666667-298.666667z m32 106.666667v160H704v64h-160V704h-64v-160.021333L320 544v-64l160-0.021333V320h64z"/>
    <PathGeometry x:Key="more" Figures=" M243.2 512m-83.2 0a1.3 1.3 0 1 0 166.4 0 1.3 1.3 0 1 0-166.4 0Z M512 512m-83.2 0a1.3 1.3 0 1 0 166.4 0 1.3 1.3 0 1 0-166.4 0Z M780.8 512m-83.2 0a1.3 1.3 0 1 0 166.4 0 1.3 1.3 0 1 0-166.4 0Z"/>
    <PathGeometry x:Key="search" Figures=" M1005.312 914.752l-198.528-198.464A448 448 0 1 0 0 448a448 448 0 0 0 716.288 358.784l198.4 198.4a64 64 0 1 0 90.624-90.432zM448 767.936A320 320 0 1 1 448 128a320 320 0 0 1 0 640z"/>
    <PathGeometry x:Key="play" Figures="  M512 0C230.4 0 0 230.4 0 512s230.4 512 512 512 512-230.4 512-512S793.6 0 512 0z m0 981.333333C253.866667 981.333333 42.666667 770.133333 42.666667 512S253.866667 42.666667 512 42.666667s469.333333 211.2 469.333333 469.333333-211.2 469.333333-469.333333 469.333333z M672 441.6l-170.666667-113.066667c-57.6-38.4-106.666667-12.8-106.666666 57.6v256c0 70.4 46.933333 96 106.666666 57.6l170.666667-113.066666c57.6-42.666667 57.6-106.666667 0-145.066667z"/>
    <PathGeometry x:Key="close" Figures="  M282.517333 213.376l-45.354666 45.162667L489.472 512 237.162667 765.461333l45.354666 45.162667L534.613333 557.354667l252.096 253.269333 45.354667-45.162667-252.288-253.44 252.288-253.482666-45.354667-45.162667L534.613333 466.624l-252.096-253.226667z"/>
    <PathGeometry x:Key="min" Figures=" M64 576h896V448H64z"/>
    <PathGeometry x:Key="max" Figures=" M917.268 132.531H106.732c-38.99 0-70.892 31.898-70.892 70.892v612.034c0 38.99 31.903 70.892 70.892 70.892h810.536c38.99 0 70.892-31.903 70.892-70.892V203.423c0-38.994-31.903-70.892-70.892-70.892z m12.335 689.813H93.809V195.948h835.788v626.396z"/>
```
### 19.触发器
#### （1）简介
顾名思义, 触发器可以理解为, 当达到了触发的条件, 那么就执行预期内的响应, 可以是样式、数据变化、动画等。
触发器通过 Style.Triggers集合连接到样式中, 每个样式都可以有任意多个触发器, 并且每个触发器都是 System.Windows.TriggerBase的派生类实例, 以下是触发器的类型：
- Trigger : 监测依赖属性的变化、触发器生效；
- MultiTrigger : 通过多个条件的设置、达到满足条件、触发器生效；
- DataTrigger : 通过数据的变化、触发器生效；
- MultiDataTrigger : 多个数据条件的触发器；
- EventTrigger : 事件触发器, 触发了某类事件时, 触发器生效；
#### （2）Trigger的使用
效果如下，当鼠标进入Brder的范围时，背景变成浅蓝色，边框变成黄色；当不在Border的范围时，景变成红色，边框变成黑色：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f0b6cf5230d740e1a094d362c0d0aa6a.gif#pic_center)
```csharp
 <Window.Resources>
        <Style x:Key="borderStyle" TargetType="Border">
            <Setter Property="BorderThickness" Value="10"/>
            <Style.Triggers>
                <Trigger Property="IsMouseOver" Value="True">
                    <Setter Property="BorderBrush" Value="Yellow"/>
                    <Setter Property="Background" Value="AliceBlue"/>
                </Trigger>
                <Trigger Property="IsMouseOver" Value="False">
                    <Setter Property="BorderBrush" Value="Black"/>
                    <Setter Property="Background" Value="Red"/>
                </Trigger>
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <Grid>
        <Grid>
            <Border Width="200" Height="100" Style="{StaticResource borderStyle}">

            </Border>
        </Grid>
    </Grid>
```
#### （3）MultiTrigger的使用
和Trigger类似, MultiTrigger可以设置多个条件满足时,触发,。下面以TextBox为例, 做一个简单的Demo
当鼠标进入文本框的范围, 并且光标设置到TextBox上, 则把TextBox的背景颜色改变成Red，效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/288da08776c74e86a38a4c816deb0418.gif#pic_center)
```xml
 <Window.Resources>
        <Style x:Key="TextBoxStyle" TargetType="TextBox">
            <Setter Property="BorderThickness" Value="5"/>
            <Style.Triggers>
                <MultiTrigger>
                    <MultiTrigger.Conditions>
                        <Condition Property="IsMouseOver" Value="True"/>
                        <Condition Property="IsFocused" Value="True"/>
                    </MultiTrigger.Conditions>
                    <MultiTrigger.Setters>
                        <Setter Property="Background" Value="Red"/>
                    </MultiTrigger.Setters>
                </MultiTrigger>
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <Grid>
        <StackPanel VerticalAlignment="Center">
            <TextBox Width="100" Height="30" Style="{StaticResource TextBoxStyle}"/>
            <Button Width="100" Height="30" Margin="0 10 0 0"/>
        </StackPanel>
    </Grid>
```
#### （4）EventTrigger的使用
触发了某类事件, 触发器执行响应。
当鼠标进入按钮的范围中, 在0.02秒内, 把按钮的字体变成18号
当鼠标离开按钮的范围时, 在0.02秒内, 把按钮的字体变成13号 。
 代码及效果如下所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/3219675060ae4d05bdeefe8a656c83e4.gif#pic_center)
```xml
<Window.Resources>
        <Style x:Key="btnStyle" TargetType="Button">
            <Setter Property="BorderThickness" Value="1"/>
            <Style.Triggers>
                <EventTrigger RoutedEvent="MouseMove">
                    <EventTrigger.Actions>
                        <BeginStoryboard>
                            <Storyboard>
                                <DoubleAnimation Duration="0:0:0.02" Storyboard.TargetProperty="FontSize" To="18"/>
                            </Storyboard>
                        </BeginStoryboard>
                    </EventTrigger.Actions>
                </EventTrigger>

                <EventTrigger RoutedEvent="MouseLeave">
                    <EventTrigger.Actions>
                        <BeginStoryboard>
                            <Storyboard>
                                <DoubleAnimation Duration="0:0:0.02" Storyboard.TargetProperty="FontSize" To="13"/>
                            </Storyboard>
                        </BeginStoryboard>
                    </EventTrigger.Actions>
                </EventTrigger>
                
            </Style.Triggers>
        </Style>
    </Window.Resources>
    <Grid>
        <StackPanel VerticalAlignment="Center">
            <Button Content="Hello彦祖" FontSize="13" Style="{StaticResource btnStyle}" Width="100" Height="30" Margin="0 10 0 0"/>
        </StackPanel>
    </Grid>
```
### 20.ItemsControl的使用
效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/fdc842903d9e43e28b3db9acac3136ad.png#pic_center)
代码：
```xml
  <Grid>
        <ItemsControl Grid.Row="1" ItemsSource="{Binding TaskBars}">
            <ItemsControl.ItemsPanel>
                <ItemsPanelTemplate>
                    <UniformGrid Columns="4"/>
                </ItemsPanelTemplate>
            </ItemsControl.ItemsPanel>

            <ItemsControl.ItemTemplate>
                <DataTemplate>
                    <Border CornerRadius="5" Background="{Binding Color}"  Margin="10">
                        <Border.Style>
                            <Style TargetType="Border">
                                <Style.Triggers>
                                    <Trigger Property="IsMouseOver" Value="True">
                                        <Setter Property="Effect">
                                            <Setter.Value>
                                                <DropShadowEffect  Color="#DDDDDD" ShadowDepth="1" BlurRadius="10"/>
                                            </Setter.Value>
                                        </Setter>
                                    </Trigger>
                                </Style.Triggers>
                            </Style>
                        </Border.Style>
                        <Grid>
                            <StackPanel Margin="20,10">
                                <TextBlock Margin="0,15" FontSize="15" Text="{Binding Title}"/>
                                <TextBlock FontSize="40" FontWeight="Bold" Text="{Binding Content}"/>
                            </StackPanel>
                            <Canvas ClipToBounds="True">
                                <Border Canvas.Top="10"  Canvas.Right="-50" Width="120"  Height="120"  CornerRadius="100" Background="#FFFFFF" Opacity="0.1"/>
                                <Border Canvas.Top="80"  Canvas.Right="-30" Width="120"  Height="120" CornerRadius="100" Background="#FFFFFF" Opacity="0.1"/>
                            </Canvas>
                        </Grid>
                    </Border>
                </DataTemplate>
            </ItemsControl.ItemTemplate>
        </ItemsControl>
    </Grid>
```
```csharp
    public class MyTasks
    {
        public string Title { get; set; }
        public string Content { get; set; }
        public string Color { get; set; }
    }
    
 public class MyTasksViewModel
    {
        private ObservableCollection<MyTasks> _tasks;
        public ObservableCollection<MyTasks> TaskBars
        {
            get { return _tasks; }
            set { _tasks = value; }
        }

        public MyTasksViewModel()
        {
             TaskBars = new ObservableCollection<MyTasks>();
            TaskBars.Add(new MyTasks() { Title = "汇总", Content = "9", Color = "#FF0CA0FF"});
            TaskBars.Add(new MyTasks() { Title = "已完成", Content = "9", Color = "#FF1ECA3A"});
            TaskBars.Add(new MyTasks() { Title = "完成", Content = "100%", Color = "#FF02C6DC" });
            TaskBars.Add(new MyTasks() { Title = "备忘录", Content = "19", Color = "#FFFFA000" });
        }
    }
```
### 21.ListBox的使用
举例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/0144113d1b01435ebd346103f3093a05.png#pic_center)

```xml
  <Grid>
        <ListBox ItemsSource="{Binding MemoDtos}"
                         ScrollViewer.VerticalScrollBarVisibility="Hidden">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <StackPanel MaxHeight="80">
                        <TextBlock 
                                    FontSize="16"
                                    FontWeight="Bold"
                                    Text="{Binding Title}"/>
                        <TextBlock 
                                    Opacity="0.5" 
                                    Margin="0,5"
                                    Text="{Binding Content}"/>
                    </StackPanel>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>
    </Grid>
```
### 22.ScrollViewer的使用
（1）留意一下这样一段代码：
```xml
				<ItemsControl.ItemsPanel>
                    <ItemsPanelTemplate>
                        <WrapPanel/>
                    </ItemsPanelTemplate>
                </ItemsControl.ItemsPanel>
```
ItemsControl.ItemsPanel是定义ItemsControl各个项的布局，ItemsPanelTemplate是用来定义集合控件的容器外观，此处设置为WrapPanel，表示各个项的布局是Wrap布局即排列完一行继续排列下一行。
（2）效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/dcf10be22a20493a96137bf88652c68a.png#pic_center)
前台代码如下：
```xml
    <Grid>
        <ScrollViewer>

            <ItemsControl Grid.Row="1" HorizontalAlignment="Center" ItemsSource="{Binding MemoDtos}">
                <ItemsControl.ItemsPanel>
                    <ItemsPanelTemplate>
                        <WrapPanel/>
                    </ItemsPanelTemplate>
                </ItemsControl.ItemsPanel>

                <ItemsControl.ItemTemplate>
                    <DataTemplate>
                        <mt:TransitioningContent OpeningEffect="{mt:TransitionEffect Kind=SlideInFromLeft}">
                            <Grid Width="220" MinHeight="180" MaxHeight="250" Margin="8">
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="auto"/>
                                    <RowDefinition/>
                                </Grid.RowDefinitions>

                                <mt:PopupBox HorizontalAlignment="Right" Panel.ZIndex="1">
                                    <Button Content="删除"/>
                                </mt:PopupBox>

                                <Border CornerRadius="3" Grid.RowSpan="2" Background="SkyBlue"/>

                                <TextBlock Padding="10,5" FontWeight="Bold" Text="{Binding Title}"/>
                                <TextBlock Padding="10,5" Text="{Binding Content}" Grid.Row="1"/>
                                <Canvas Grid.RowSpan="2" ClipToBounds="True">
                                    <Border Canvas.Top="10"  Canvas.Right="-50" Width="120"  Height="120"  CornerRadius="100" Background="#FFFFFF" Opacity="0.1"/>
                                    <Border Canvas.Top="80"  Canvas.Right="-30" Width="120" Height="120" CornerRadius="100" Background="#FFFFFF" Opacity="0.1"/>
                                </Canvas>
                            </Grid>
                        </mt:TransitioningContent>
                    </DataTemplate>
                </ItemsControl.ItemTemplate>
            </ItemsControl>
        </ScrollViewer>
    </Grid>
```
后台代码如下：
```csharp
public class MyTasksViewModel
    {
        private ObservableCollection<MemoDto> memoDtos;

        public ObservableCollection<MemoDto> MemoDtos
        {
            get { return memoDtos; }
            set { memoDtos = value;  }
        }

        public MyTasksViewModel()
        {
            MemoDtos = new ObservableCollection<MemoDto>();
            for (int i = 0; i < 10; i++)
            {
                MemoDtos.Add(new MemoDto() { Title = "备忘录" + i, Content = "我的密码...." });
            }
        }

    }
```
### 23.UpdateSourceTrigger属性
遇到了这样一行代码：
```xml
 <ToggleButton IsChecked="{Binding IsUsed, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
```
这行代码的意思是：该控件的IsChecked属性绑定到后台的IsUsed属性，当IsUsed属性值发生变化时，就会改变控件的IsChecked状态。
UpdateSourceTrigger的默认值是Default，其他值有PropertyChanged、LostFocus和Explicit。
 - PropertyChanged：当属性值发生改变时，源就会被更新；
 - LostFocus：当失去焦点的时候，源就会被更新；
 - Explicit：必须通过手动更新来推送。
### 24.wpf中的TextBox实现密码样式、水印、后台绑定功能
在写一个登录页面时，遇到了一些小问题，想把输入的密码和后台进行绑定，并且使用密码样式（即输入的密码要显示为黑色的小圆点）和水印提示,使用纯原生的TextBox或PasswordBox并不能满足上述需求。因为PasswordBox有密码样式，但是不支持数据绑定，而TextBox支持数据绑定，但是不支持密码样式。因此对TextBox设计样式，并成功实现以下两个需求：密码样式和水印提示
```xml
<Style TargetType="{x:Type TextBox}" x:Key="textBoxStyle">
                <Setter Property="TextDecorations">
                    <Setter.Value>
                        <TextDecorationCollection>
                            <TextDecoration>
                                <TextDecoration.Pen>
                                    <Pen Brush="Black" DashCap="Round" EndLineCap="Round"  StartLineCap="Round"Thickness="10">
                                        <Pen.DashStyle>
                                            <DashStyle Dashes="0.0,1.2" Offset="0.6" />
                                        </Pen.DashStyle>
                                    </Pen>
                                </TextDecoration.Pen>
                                <TextDecoration.Location>
                                    <TextDecorationLocation>Strikethrough</TextDecorationLocation>
                                </TextDecoration.Location>
                            </TextDecoration>
                        </TextDecorationCollection>
                    </Setter.Value>

                </Setter>
                <Setter Property="Height" Value="30" />
                <Setter Property="Background" Value="White" />
                <Setter Property="Foreground" Value="Transparent" />
                <Setter Property="FontSize" Value="20" />
                <Setter Property="FontFamily" Value="Courier New" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="TextBox">
                            <Border x:Name="border" BorderBrush="{TemplateBinding BorderBrush}" BorderThickness="{TemplateBinding BorderThickness}" Background="{TemplateBinding Background}" SnapsToDevicePixels="True" CornerRadius="5">
                                <Grid>
                                    <TextBlock Text="请输入密码" VerticalAlignment="Center" HorizontalAlignment="Left" Foreground="#BBB" Name="markText" Visibility="Collapsed" FontSize="12" Margin="10,0"/>
                                    <ScrollViewer x:Name="PART_ContentHost" Focusable="False" HorizontalScrollBarVisibility="Hidden" VerticalScrollBarVisibility="Hidden"  VerticalAlignment="Center" MinHeight="20"/>
                                </Grid>
                            </Border>
                            <ControlTemplate.Triggers>
                                <Trigger Property="IsEnabled" Value="false">
                                    <Setter Property="Opacity" TargetName="border" Value="0.56"/>
                                </Trigger>
                                <Trigger Property="IsMouseOver" Value="true">
                                    <Setter Property="BorderBrush" TargetName="border" Value="#FF7EB4EA"/>
                                </Trigger>
                                <Trigger Property="IsKeyboardFocused" Value="true">
                                    <Setter Property="BorderBrush" TargetName="border" Value="#FF569DE5"/>
                                </Trigger>
                                <DataTrigger Binding="{Binding Path=Text,RelativeSource={RelativeSource Mode=self}}" Value="">
                                    <Setter Property="Visibility" TargetName="markText" Value="Visible"/>
                                </DataTrigger>
                            </ControlTemplate.Triggers>
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
```

```xml
 <TextBox x:Name="passwordBox" Width="410" ToolTip="提示:密码为用户名拼音"  FontSize="35" Height="70"  Margin="50,0,0,40" Text="{Binding InputPassword}" Style="{StaticResource textBoxStyle}"/>
```
效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/bf618d2e950e4a09bf44887a930b309a.gif#pic_center)
### 25.MultiBinding
MultiBinding 允许绑定多个源，MultiBinding必须和转换器Converter一起使用。下面这个例子是将DataGrid 中的一个DataGridTextColumn 绑定到两个值上面而不是传统的一个值。
```xml
<StackPanel Grid.Row="0">
            <DataGrid  ColumnHeaderStyle="{StaticResource DataGridColumnHeaderStyle}" x:Name="Courseslist" AutoGenerateColumns="False" IsReadOnly="True" Width="780" Height="600" ItemsSource="{Binding CoursesDataList}">
                <DataGrid.Columns>
                    <DataGridTextColumn Header="课程名称" Width="140" Binding="{Binding CourseName}"/>
                    <DataGridTextColumn Header="任课教师" Width="140" Binding="{Binding CourseTeacher}"/>
                    <DataGridTextColumn Header="课程时长" Width="140" Binding="{Binding CourseTime}"/>
                    <DataGridTextColumn Header="课程评分" Width="140" Binding="{Binding CourseScore}"/>
                    <DataGridTextColumn Header="课程是否免费" Width="220">
                        <DataGridTextColumn.Binding>
                            <MultiBinding Converter="{StaticResource IFC}">
                                <Binding Path="CourseName"/>
                                <Binding Path="IsCourseFree"/>
                            </MultiBinding>
                        </DataGridTextColumn.Binding>
                    </DataGridTextColumn>
                </DataGrid.Columns>
            </DataGrid>
        </StackPanel>
```

```xml
        <lc:IsCourseFreeConvert x:Key="IFC"/>
```
课程信息的Model类：
```csharp
  public class CoursesData : INotifedChangedBse
    {
        private string _courseName;
        private string _courseTeacher;
        private string _courseTime;
        private string _courseScore;
        private string _isCourseFree;

        public string CourseName //课程名称
        {
            get { return _courseName; }
            set { _courseName = value; this.RaiseProperChanged("CourseName"); }
        }
        public string CourseTeacher//任课教师
        {
            get { return _courseTeacher; }
            set { _courseTeacher = value; this.RaiseProperChanged("CourseTeacher"); }
        }
        public string CourseTime//课程时长
        {
            get { return _courseTime; }
            set { _courseTime = value; this.RaiseProperChanged("CourseTime"); }
        }
        public string CourseScore//课程评分
        {
            get { return _courseScore; }
            set { _courseScore = value; this.RaiseProperChanged("CourseScore"); }
        }
        //课程是否免费
        public string IsCourseFree
        {
            get { return _isCourseFree; }
            set { _isCourseFree = value; this.RaiseProperChanged("IsCourseFree"); }
        }
```

定义的一个用于多值转换的类：
```csharp
   //定义一个用于多值转换的类
  public  class IsCourseFreeConvert: IMultiValueConverter //如果是单个绑定时，定义的转换器类需要继承自IValueConverter接口;如果是多个绑定，定义的转换器类需要继承自IsCourseFreeConvert接口
    {
        public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
        {
            ////将object类型的数组values转化为string类型的数组values1,否则后续会报错
            string[] values1 = new string[values.Length];
            for (int i = 0; i < values1.Length; i++)
            {
                values1[i] = values[i].ToString();
            }

            //开始进行转换
            string result;
            if (values1[1] == "0")
            {
                result = values[0] + "免费";
            }
            else if (values1[1] == "1")
            {
                result = values[0] + "收费";
            }
            else
            {
                result = "未知";
            }
            return result;
        }


        public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
        {
            throw new NotImplementedException();
        }
    }
```
最后实现转换后的效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/095c911d77c24b109f49512e95dd917c.png#pic_center)
### 26.Stylet框架中事件聚合器的使用
先看效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/79919b571c7646f4bfa0d11483aa1271.gif#pic_center)
通过点击右侧区域RunModelButtonView中的“启动”按钮和“退出”按钮发布两个信号，然后在左侧区域MainView对应的MainViewViewModel中订阅发布的两个信号。
步骤举例：
工程结构：
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f26b46974554d0488609ee190161355.png#pic_center)

1. 首先自定义一个事件类：建立一个文件夹Event,添加一个事件类ShowProcessEvent，并实现如下代码：
```csharp
public class ShowProcessEvent 
    {
        private bool _isAction;
        public bool IsAction
        {
            get => _isAction;
            set => _isAction = value;
        }

        public ShowProcessEvent(bool args)
        {
            _isAction = args;
        }
    }
```
2.把事件类加入到事件聚合器中并发布信号： 在RunModelButtonViewModel中实现点击两个按钮后发布事件功能：
```csharp
 public class RunModelButtonViewModel : Screen, IView
    {
        public RunModelButtonViewModel()
        {
        }
        /// <summary>
        /// 点击启动按钮的方法
        /// </summary>
        public void StartProcess()
        {
            IoC.Get<IEventAggregator>().Publish(new ShowProcessEvent(true));//发布事件,发送的参数为bool类型的true
        }

        /// <summary>
        /// 点击退出按钮的方法
        /// </summary>
        public void EndProcess()
        {
            IoC.Get<IEventAggregator>().Publish(new ShowProcessEvent(false));//发布事件,发送的参数为bool类型的false
        }
    }
```
3. 订阅事件：在MainViewViewModel订阅事件聚合器中加入的自定义事件爱你类,首先MainViewViewModel必须继承IHandle接口并在尖括号里加入自定义的事件类名称,其次实现接口中的Handle方法并在此方法里写出订阅事件后要执行的逻辑，最后在构造函数里订阅自定义的事件类即可：
```csharp
public class MainViewModel : Screen, IPage,IHandle<ShowProcessEvent>
 public MainViewModel()
        {
            IoC.Get<IEventAggregator>().Subscribe(this);
        }
     
  public void Handle(ShowProcessEvent message)
  {
  	//此方法里写订阅事件后具体的逻辑
  }
```
### 27.DataGrid表格
WPF中的DataGrid表格是一个很复杂而且很常用的控件，这里面有好多知识点，简单记录下对DataGrid表格的学习。
#### 1.DataGrid中一些常见的属性：
1. **CanUserReorderColumns**：是否允许用户通过使用鼠标拖拽列标题，更改列的显示顺序；

2. **AutoGenerateColumns**：是否根据数据源集合自动生成列，如果设置成false，那么就需要自己定义一些列；

3. **CanUserSortColumns**：用来判断是否允许用户按列对表中内容进行排序；

4. **CanUserDeleteRows**：是否允许删除行；

5. **SelectionMode**：设置DataGrid的选取模式，是否可以选取多行。SelectionMode="Single"表示只能选择一行，SelectionMode="Extended"表示可以选择多行；

6. **SelectionUnit**：设置每次选中的方式，是一行还是一个单元格。SelectionUnit="FullRow"表示每次选取的是一行，SelectionUnit="Cell"表示每次选取的是一个单元格；

7. **IsReadOnly**：设置单元格只读，不可编辑；
 
8. **StringFormat属性**： <TextBlock Text="{Binding Value,StringFormat={}{0:F4}}" 这一句话表示对float类型的Value显示4位小数在View中：StringFormat 表示格式化显示数值，常用于数字的小数点显示，绑定内容的前缀后缀的添加以及时间的格式化形式；

9. **DecimalPlaces**：小数要显示的位数;

#### 2. DataGrid列（重点）
目前DataGrid中可用的列有如下：
* DataGridTextColumn：用于展示普通文本的列
* DataGridCheckBoxColumn：用于在列上展示一些单选框，以表示用户是否选中；
* DataGridComboBoxColumn：用于展示一些可供用户从列表集合中选择的数据列；
* DataGridHyperlinkColumn：用于展示一些超链接的列；
* DataGridTemplateColumn：自定义单元格的列，可以在这个列里自定义一些布局，比如加个按钮；

下图是微软官方给出的关于这几种列的数据类型：
| 生成的列类型 |  数据类型|
|--|--|
| DataGridTextColumn | String类型 |
| DataGridCheckBoxColumn| Bool类型|
| DataGridComboBoxColumn| Enum类型|
| DataGridHyperlinkColumn| Uri类型|

下面直接上代码来展示效果：

1. 文件结构（这是基于Prism模板创建的工程，只需要关注DataGridExampleView和DataGridExampleViewModel这两个文件的代码即可，其他文件可以忽略）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/7a34035c76db416abcc6d4f392994963.png)
2. DataGridExampleView代码：
![在这里插入图片描述](https://img-blog.csdnimg.cn/00502aef655541aa888b1c5a4183c3cb.png)

```xml
<UserControl x:Class="BlankApp1.Views.DataGridExampleView"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:BlankApp1.Views"
             xmlns:VideModels="clr-namespace:BlankApp1.ViewModels"
             xmlns:Converter="clr-namespace:BlankApp1.Converter"
             mc:Ignorable="d" 
             xmlns:core="clr-namespace:System;assembly=mscorlib"
             xmlns:prism="http://prismlibrary.com/"
             prism:ViewModelLocator.AutoWireViewModel="True"
             d:DesignHeight="450" d:DesignWidth="800" Background="White">
    <UserControl.Resources>

        <ObjectDataProvider x:Key="SexHobbyKey" MethodName="GetValues" ObjectType="{x:Type core:Enum}">
            <ObjectDataProvider.MethodParameters>
                <x:Type Type="VideModels:HobbyType"/>
            </ObjectDataProvider.MethodParameters>
        </ObjectDataProvider>
        
        <Converter:QQConverter x:Key="QQConverter"/>
    </UserControl.Resources>
    
    <DataGrid Width="600" AutoGenerateColumns="False" CanUserSortColumns="False" Height="400" IsReadOnly="True" ItemsSource="{Binding Students}" SelectionUnit="Cell" VerticalAlignment="Center" HorizontalAlignment="Center" CanUserAddRows="False">
        <DataGrid.Columns>
            <DataGridTextColumn Header="Name" Binding="{Binding Name}"/>
            <DataGridCheckBoxColumn Header="状态" Binding="{Binding IsSelected}"/>
            <DataGridComboBoxColumn Header="爱好" SelectedItemBinding="{Binding Hobby}" ItemsSource="{Binding Source={StaticResource SexHobbyKey}}"/>
            <DataGridHyperlinkColumn Header="QQ邮箱" Binding="{Binding Email}"  Width="150"/>
            <DataGridHyperlinkColumn Header="QQ" Binding="{Binding Email,Converter={StaticResource QQConverter}}"  Width="150"/>
            <DataGridTemplateColumn Header="操作">
                <DataGridTemplateColumn.CellTemplate>
                    <DataTemplate>
                        <Button Width="60" Height="40" Content="编辑"/>
                    </DataTemplate>
                </DataGridTemplateColumn.CellTemplate>
            </DataGridTemplateColumn>
        </DataGrid.Columns>
    </DataGrid>
</UserControl>
```

3. DataGridExampleViewModel代码：
![在这里插入图片描述](https://img-blog.csdnimg.cn/76e0c48cd6574a5295e3b8b89a033fae.png)
```csharp
using Prism.Mvvm;
using Prism.Regions;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace BlankApp1.ViewModels
{
    public class Student
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
        public bool IsSelected { get; set; }
        public HobbyType Hobby { get; set; }
        public string Email { get; set; }
    }

    public enum HobbyType
    {
        Basketball,
        FootBall,
        TableTennis
    }


    public class DataGridExampleViewModel:BindableBase
    {
        public List<Student> _students = new List<Student>();
        public List<Student> Students
        {
            get { return _students; }
            set { _students = value;RaisePropertyChanged(); }
        }

        public DataGridExampleViewModel()
        {
            List<Student> studentList = new List<Student>();
            studentList.Add(new Student { Email = "1221343567@qq.com",Id = 1, IsSelected = false, Name = "John Doe", Age = 19, Hobby = HobbyType.Basketball });
            studentList.Add(new Student { Email = "124567@qq.com", Id = 2, IsSelected = false, Name = "Jane Doe", Age = 18, Hobby = HobbyType.FootBall });
            studentList.Add(new Student { Email = "1dfgh54567@qq.com", Id = 3, IsSelected = true, Name = "Marry Doe", Age = 21, Hobby = HobbyType.Basketball });
            studentList.Add(new Student { Email = "12879567@qq.com", Id = 4, IsSelected = false, Name = "Kang Doe", Age = 17, Hobby = HobbyType.Basketball });
            Students =studentList;
        }
    }
}

```
4. 转换器QQConverter类
![在这里插入图片描述](https://img-blog.csdnimg.cn/079291a0845d431a9f3528a7998e7627.png)

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Data;

namespace BlankApp1.Converter
{
    /// <summary>
    /// 该转换器类用于把QQ邮箱转化为QQ号码
    /// </summary>
    public class QQConverter : IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            string QQEmail = value.ToString();
            int index = QQEmail.IndexOf("@");
            string QQ=QQEmail.Substring(0,index);
            return QQ;
        }

        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            return null; 
        }
    }
}

```
5. 运行演示：

![](https://img-blog.csdnimg.cn/ce647000c6fc4b8db5a752ee1ce5b9de.png)
### 28.行为Behaviors
#### 1.什么是行为？
1. 行为是Blend中的自带资产。
如果要在VS中使用行为需要下载一个包Microsoft.Xaml.Behaviors.Wpf：
![在这里插入图片描述](https://img-blog.csdnimg.cn/99f3217481d04f8a9cc7768aa02af9f0.png)
2. 控件的界面逻辑大都可以被认为是行为：TextBox在被聚焦后自动全选、在Window中按下Esc会退出、Button点击后会弹出一个小窗口、Grid按照某种方式排布子控件。
3. 行为本质上是基于附加属性(AP)实现的。
4. 行为本身也继承了DependencyObject。

#### 2.怎么使用Behaviors
每个控件添加的行为是一个个单独的实例，而且可以添加多个行为(存放在Collection中)。
##### 1.使用包中自带的行为：
比如要对一个Border实现拖拽效果，可以使用MouseDragElementBehavior：

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:Behaviors="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <Border Width="50" Height="50" Background="Red">
            <Behaviors:Interaction.Behaviors>
                <i:MouseDragElementBehavior/>
            </Behaviors:Interaction.Behaviors>
        </Border>
    </Grid>
</Window>

```
运行效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/bfc41667b1d64691a0f5bff47f3b4fac.gif#pic_center)
##### 2.自定义一个行为
1. 自定义一个行为：当鼠标移入、移除该行为附加的控件时，控件的背景色会发生改变：

```csharp
using Microsoft.Xaml.Behaviors;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Input;
using System.Windows.Media;
using EventTrigger = Microsoft.Xaml.Behaviors.EventTrigger;

namespace BlankApp1.Behaviors
{
    public class BorderBehavior : Behavior<Border>
    {
        protected override void OnAttached()
        {
            ///当鼠标移入该控件时
            AssociatedObject.MouseEnter += (s, e) =>
            {
                AssociatedObject.Background = Brushes.Blue;//AssociatedObject指的是使用该行为的控件
            };

            ///当鼠标移出该控件时
            AssociatedObject.MouseLeave += (s, e) =>
            {
                AssociatedObject.Background = Brushes.Black;//AssociatedObject指的是使用该行为的控件
            };

        }

        protected override void OnDetaching()
        {

        }
    }
}

```

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:Behaviors="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:CustomBehaviors="clr-namespace:BlankApp1.Behaviors"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <Border Width="50" Height="50" Background="Red">
            <Behaviors:Interaction.Behaviors>
                <i:MouseDragElementBehavior/>
                <CustomBehaviors:BorderBehavior/>
            </Behaviors:Interaction.Behaviors>
        </Border>
    </Grid>
</Window>

```
效果展示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/348c886825c744f5a48a0c23f39af61b.gif#pic_center)
2.自定义一个行为：点击按钮会改变其他TextBox的内容:

```csharp
using Microsoft.Xaml.Behaviors;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Input;
using System.Windows.Media;
using EventTrigger = Microsoft.Xaml.Behaviors.EventTrigger;

namespace BlankApp1.Behaviors
{
    public class ButtonClearBehavior : Behavior<Button>
    {
        /// <summary>
        /// 注册一个依赖属性用于获取要清空内容的TextBox
        /// </summary>
        public TextBox Target
        {
            get { return (TextBox)GetValue(TargetProperty); }
            set { SetValue(TargetProperty, value); }
        }

        public static readonly DependencyProperty TargetProperty =
            DependencyProperty.Register("Target", typeof(TextBox), typeof(ButtonClearBehavior), new PropertyMetadata(null));

        protected override void OnAttached()
        {
            AssociatedObject.Click += AssociatedObject_Click;
        }

        private void AssociatedObject_Click(object sender, RoutedEventArgs e)
        {
            Target.Text = "变变变";
        }

        protected override void OnDetaching()
        {

        }
    }
}

```

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:Behaviors="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:CustomBehaviors="clr-namespace:BlankApp1.Behaviors"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <StackPanel>
        <TextBox x:Name="textBox1" Text="我是1号"/>
        <TextBox x:Name="textBox2" Text="我是2号"/>
        <Button Content="清空">
            <Behaviors:Interaction.Behaviors>
                <CustomBehaviors:ButtonClearBehavior Target="{Binding ElementName=textBox1}"/>
                <CustomBehaviors:ButtonClearBehavior Target="{Binding ElementName=textBox2}"/>
            </Behaviors:Interaction.Behaviors>
        </Button>
    </StackPanel>
</Window>
```
运行效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f75820a256af4bc6a8cccaeaa9f9f41a.gif#pic_center)
### 29.触发器Triggers(非WPF原生)
Microsoft.Xaml.Behaviors.Wpf包中既包含行为，也包含触发器。

1. 有有点类似WPF的原生Triggers，在触发某些条件时会发生一些变化。
2. 但是比原生Triggers更为灵活，可以做的事情也很多。
3. 通常与Action结合使用：InvokeCommandAction、CallMethodAction、ChangePropertyAction。

例子1，前台窗口的Loaded加载事件被调用后，会随之调用MVVM后台的方法：

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:CustomBehaviors="clr-namespace:BlankApp1.Behaviors"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <i:Interaction.Triggers>
        <i:EventTrigger EventName="Loaded">
            <i:InvokeCommandAction Command="{Binding ShowCommand}"/>
        </i:EventTrigger>
    </i:Interaction.Triggers>
    <StackPanel>
        <TextBox x:Name="textBox1" Text="{Binding TimeStr}"/>
        <TextBox x:Name="textBox2" Text="我是2号"/>
    </StackPanel>
</Window>

```

```csharp

using Microsoft.Xaml.Behaviors;
using Microsoft.Xaml.Behaviors.Core;
using Prism.Commands;
using Prism.Interactivity;
using Prism.Mvvm;
using System;
using System.Diagnostics;
using System.Runtime.CompilerServices;
using System.Threading;
using System.Threading.Tasks;
using System.Timers;
using System.Windows;
using System.Windows.Input;
using InvokeCommandAction = Microsoft.Xaml.Behaviors.InvokeCommandAction;

namespace BlankApp1.ViewModels
{
    public class MainWindowViewModel : BindableBase
    {
        private string timeStr = "我是1号";
        public string TimeStr
        {
            get { return timeStr; }
            set
            {
                timeStr = value; RaisePropertyChanged();
            }
        }

        public MainWindowViewModel()
        {
            ShowCommand = new DelegateCommand(Show);
        }
        public async void Show()
        {
           await Show1();
        }
        public DelegateCommand ShowCommand { get; set; }
        public async Task Show1()
        {
            await Task.Delay(3000);
            TimeStr = string.Empty;
        }
    }
}

```
运行效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/be984bb0fee7412a81a955304e40d9b7.gif#pic_center)
例子2，点击按钮，会关闭当前窗口的Close方法来关闭窗口：

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:CustomBehaviors="clr-namespace:BlankApp1.Behaviors"
        prism:ViewModelLocator.AutoWireViewModel="True"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <Button Content="关闭窗口" HorizontalAlignment="Center" VerticalAlignment="Center">
            <i:Interaction.Triggers>
                <i:EventTrigger EventName="Click">
                    <i:CallMethodAction TargetObject="{Binding RelativeSource={RelativeSource AncestorType=Window}}" MethodName="Close"/>
                </i:EventTrigger>
            </i:Interaction.Triggers>
        </Button>
    </Grid>
</Window>

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a9eaf097857f4de9ba972e9f3989c957.gif#pic_center)
### 30.长按按钮(非单击)来执行方法
1.定义一个附加属性，当长按按钮一定时间后会执行某个方法
```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;
using System.Windows.Input;
using System.Windows.Threading;

namespace BlankApp1.CustomDPs
{
    /// <summary>
    /// 长按Button触发事件。新增属性LongPressSwitch，类型为bool.
    /// </summary>
    public class ButtonHelper
    {
        public static readonly DependencyProperty LongPressSwitchProperty = DependencyProperty.RegisterAttached("LongPressSwitch",
                            typeof(bool), typeof(ButtonHelper),
                            new FrameworkPropertyMetadata((bool)false, new PropertyChangedCallback(OnLongPressSwitchChanged)));

        private static DispatcherTimer _pressDispatcherTimer;
        private static Button _curBtn;

        static ButtonHelper()
        {
            _pressDispatcherTimer = new DispatcherTimer();
            _pressDispatcherTimer.Tick += OnDispatcherTimeOut;
            _pressDispatcherTimer.Interval = new TimeSpan(0, 0, 0, 1, 0);
        }

        public static bool GetLongPressSwitch(Button d)
        {
            return (bool)d.GetValue(LongPressSwitchProperty);
        }
        public static void SetLongPressSwitch(Button d, bool value)
        {
            d.SetValue(LongPressSwitchProperty, value);
        }

        private static void OnLongPressSwitchChanged(DependencyObject dependency, DependencyPropertyChangedEventArgs e)
        {
            if (dependency is Button btn)
            {
                var enabled = (bool)e.NewValue;
                if (enabled)
                {
                    btn.PreviewMouseDown += ButtonPreviewMouseDown;
                    btn.PreviewMouseUp += ButtonPreviewMouseUp;
                }
                else
                {
                    btn.PreviewMouseDown -= ButtonPreviewMouseDown;
                    btn.PreviewMouseUp -= ButtonPreviewMouseUp;
                    MessageBox.Show("单击了");
                }
            }
        }

        private static void ButtonPreviewMouseDown(object sender, MouseButtonEventArgs e)
        {
            if (sender is Button btn)
            {
                _curBtn = btn;
                _pressDispatcherTimer?.Start();
                e.Handled = true;
            }
        }

        private static void ButtonPreviewMouseUp(object sender, MouseButtonEventArgs e)
        {
            if (sender is Button btn)
            {
                _pressDispatcherTimer?.Stop();
                e.Handled = true;
            }
        }

        private static void OnDispatcherTimeOut(object sender, EventArgs e)
        {
            _pressDispatcherTimer?.Stop();
            MessageBox.Show("长按了");
        }
    }
}

```
2.在界面引用该附加属性

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
        xmlns:CustomDP="clr-namespace:BlankApp1.CustomDPs" 
        prism:ViewModelLocator.AutoWireViewModel="True"
        Height="350" Width="525" >
    <Grid>
        <Button  Width="100" Height="50" CustomDP:ButtonHelper.LongPressSwitch="True"  HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Window>

```
### 31.背景或边框使用 Border 代替 Grid 控件
如果只是需要显示背景色或者只是为了显示边框，此时选用 Grid 控件就太重了，可以使用 Border 代替，减少内存占用以及提升对象初始化性能。
### 32.公开在 XAML 中使用对象的访问权限
在 XAML 中使用的对象，包括转换器以及自定义元素等，推荐将这部分类的定义的可访问权限，在不影响整个框架设计的情况下，设置为 public 权限，用来提升 XAML 创建对象的性能

原因是在 XAML 创建对象的时候，会通过反射的方法创建，而如果是反射创建的话，使用 public 权限，可以让类的构造函数被 WPF 框架进行缓存，可以大大提高对象创建的性能。

给 XAML 里面创建的类型应该是公开的，这样才能发挥 XAML 的创建对象性能。如果类型是 internal 的，那么 XAML 每次创建都需要反射创建

在 XAML 里面的创建的类型包括了用户自定义控件和转换器等类型，这些类型推荐是作为公开的，除非是确实不能公开的类型
### 33.尽可能使用 TextBlock 代替 Label 控件
在 WPF 中，存在一个框架设计问题是引入了 Label 这个定位不够明确的控件。在所有使用 Label 的地方，都应该尽可能使用 TextBlock 代替，用来提升性能。其实在 WPF 中 Label 也仅仅只是对 TextBlock 的封装，除了性能比 TextBlock 更差之外，几乎没有别的差别。
### 34.调用 Dispatcher.Invoke 时需要判断是否可以使用 Dispatcher.InvokeAsync 代替
在使用 WPF 的 Dispatcher.Invoke 时，如果遇到异步，是有可能出现锁的相互等待。因此更多推荐使用 Dispatcher.InvokeAsync 代替，如果可以修改为 Dispatcher.InvokeAsync 那么推荐使用 Dispatcher.InvokeAsync 代替。如果需要等待 Invoke 内容执行完成，记得使用 Dispatcher.InvokeAsync 时需要加上 await 等待。

不想思考的话，默认使用 Dispatcher.InvokeAsync 就好了，除非有特别需求，否则少用 Dispatcher.Invoke 方法或 Dispatcher.BeginInvoke 方法。
### 35.调用 Dispatcher.Invoke 里面使用 Shutdown 方法可以使用 InvokeShutdown 代替
如下面代码

```csharp
  Application.Current.Dispatcher.Invoke(() =>
  {
      Application.Current.Shutdown(0);
  });
```

可以使用 InvokeShutdown 代替：

```csharp
 Application.Current.Dispatcher.InvokeShutdown();
```
### 36.不要使用 Dispatcher.InvokeShutdown 方法退出应用
应该使用 Application.Current.Dispatcher.InvokeShutdown 或者是 Application.Current.Shutdown 进行退出，不应该使用 Dispatcher.InvokeShutdown 方法退出应用。
### 37.给 DispatcherTimer 设置 Interval 属性
如果创建一个空的 DispatcherTimer 对象，没有设置 Interval 属性，也没有加上事件，直接开始，那将会空跑 UI 线程，让 UI 线程开始拉满一个 CPU 资源。

这是因为 DispatcherTimer 对象在没有设置 Interval 属性时，此属性的值就是 0 时间，也就是不断执行。

代码审查的时候，看到 DispatcherTimer 需要看看 Interval 属性是否被设置了，和设置的时间是多少。
### 38.当鼠标滑过一个被禁用的元素时，让ToolTip 显示
在WPF中，当鼠标划过一些禁用元素时，设置了ToolTip属性后，是不会有提示的效果，比如下面代码：

```xml
 <Button Content="点击测试" Width="100" Height="40" ToolTip="oK" IsEnabled="False" />
```
但是当设置设置ToolTipService.ShowOnDisabled为 true时，被禁用的元素就可以显示ToolTip的内容了，如：

```xml
 <Button Content="点击测试" Width="100" Height="40" ToolTip="oK" IsEnabled="False" ToolTipService.ShowOnDisabled="True"/>
```
### 39.资源字典引用

```xml
<Application.Resources>
    <ResourceDictionary>
      <ResourceDictionary.MergedDictionaries>
        <ResourceDictionary Source="pack://application:,,,/JeenalerenenearWerjilakaw;component/ColorBrushResourcesDictionary.xaml"></ResourceDictionary>
      </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
  </Application.Resources>
```
设计器挖的一个坑是 component 如果写两次，如 ;component;component 那么设计器依然能帮你找到资源，但是运行就炸了。
### 40.判断 WPF 程序使用管理员权限运行
引用命名空间，复制下面代码，然后调用 IsAdministrator 方法，如果返回 true 就是使用管理员权限运行：

```csharp
        public static bool IsAdministrator()
        {
            WindowsIdentity current = WindowsIdentity.GetCurrent();
            WindowsPrincipal windowsPrincipal = new WindowsPrincipal(current);
            //WindowsBuiltInRole可以枚举出很多权限，例如系统用户、User、Guest等等
            return windowsPrincipal.IsInRole(WindowsBuiltInRole.Administrator);
        }
```
### 41.注册全局事件
如果需要注册一个类型的全局事件，如拿到 TextBox 的全局输入，那么可以使用下面代码：

```csharp
EventManager.RegisterClassHandler(typeof(TextBox), TextBox.KeyDownEvent, new RoutedEventHandler(方法));
```
### 42.高版本的 WPF 引用低版本类库导致无法启动
如果在一个 .net 4.0 的 WPF 程序引用一个 .net 2.0 的库，那么就会让程序无法运行，解决方法添加useLegacyV2RuntimeActivationPolicy。

打开 app.config 添加 useLegacyV2RuntimeActivationPolicy="true" 在 startup 元素

下面是 app.config 代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
<startup useLegacyV2RuntimeActivationPolicy="true">
  <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
</startup>
</configuration>
```
### 43.使用十进制设置颜色

xaml代码如下：

```xml
 <Button Content="点击测试" Width="100" Height="40" ToolTip="oK">
            <Button.Background>
                <SolidColorBrush>
                    <SolidColorBrush.Color>
                        <Color R="100" G="200" B="30" A="100"/>
                    </SolidColorBrush.Color>
                </SolidColorBrush>
            </Button.Background>
        </Button>
```

### 44.WPF 判断文件是否隐藏

可以设置一些文件是隐藏文件，那么 WPF 如何判断 FileInfo 是隐藏文件？

简单的代码，通过判断 Attributes 就可以得到，请看下面:

```c#
 file.Attributes.HasFlag(FileAttributes.Hidden)
```

### 45.TextBlock 换行

1. 方法1：在 xaml 可以使用 " &#x0a"加一个英文分号表示换行，所以最简单的方法是在 Text 里面输入换行，如下：

```xml
        <TextBlock Text="青青子衿，悠悠我心。&#x0a;纵我不往，子宁不嗣音？青青子佩，悠悠我思。&#x0a;纵我不往，子宁不来？挑兮达兮，在城阙兮。&#x0a;一日不见，如三月兮。">
            
        </TextBlock>
```
效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/a026ab0ebb994643af3b9b3ee51fa209.png#pic_center)


2. 方法2：使用xml:space="preserve"直接输入换行。添加了 space 就可以在换行的时候自动换行。
代码如下：

```yaml
        <TextBlock xml:space="preserve">
            <!--Text="青青子衿，悠悠我心。&#x0a;纵我不往，子宁不嗣音？青青子佩，悠悠我思。&#x0a;纵我不往，子宁不来？挑兮达兮，在城阙兮。&#x0a;一日不见，如三月兮。">-->
            <TextBlock.Text>
                青青子衿，悠悠我心。纵我不往，子宁不嗣音？
                青青子佩，悠悠我思纵我不往，子宁不来？
                挑兮达兮，在城阙兮。一日不见，
                如三月兮。
            </TextBlock.Text>
        </TextBlock>
```
效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/da04a83609434604a7db15c5a1edce6d.png)
3. 方法3：通过 LineBreak 的方法换行：
代码如下：

```xml
        <TextBlock>
                青青子衿，
                <LineBreak/>
                悠悠我心。
                纵我不往，
                <LineBreak/>
                子宁不嗣音？
        </TextBlock>
```
效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/27d5b32294ad45769499de2b041e7d89.png)

### 46.WPF 去掉最大化按钮
通过在窗口添加下面代码：

```xml
ResizeMode="NoResize"
```
### 47.WPF ListView 使用 WrapPanel 没有自动换行
把ScrollViewer.HorizontalScrollBarVisibility属性禁用即可。
```xml
<ListView ScrollViewer.HorizontalScrollBarVisibility="Disabled">
  <ListView.ItemsPanel>
    <ItemsPanelTemplate>
      <WrapPanel Orientation="Horizontal" />
    </ItemsPanelTemplate>
  </ListView.ItemsPanel>
</ListView>
```
### 48.WPF 让 TextBox 支持水平滚动
只需要设置 HorizontalScrollBarVisibility 可见就可以了。
### 49.WPF 如何给 Grid 的某一行添加背景色
其实在 WPF 里面是不存在单独设置 Grid 的某一行的配色，但是想要达到这个视觉效果，可以通过 Border 配合做到。

```xml
 <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"></RowDefinition>
            <RowDefinition Height="*"></RowDefinition>
            <RowDefinition Height="*"></RowDefinition>
        </Grid.RowDefinitions>
    </Grid>
```
如上代码，想给这三行的第一行设置背景色，那么可以加一个Border，如下：

```xml
  <Border Grid.Row="0" Background="Red"/>
```
这样，这一行就变成了红色。

### 50.WPF中使用OxyPlot包生成图表
代码示例：

1. 首先下载OxyPlot.WPF包；
2. 在View中添加上述代码：

```xml
<Window x:Class="BlankApp1.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:oxyPlot="http://oxyplot.org/wpf"
        Title="{Binding Title}" Height="350" Width="525" >
    <Grid>
        <oxyPlot:PlotView  Foreground="Black" Margin="5" Background="Transparent" Model="{Binding ChartModel}"/>
    </Grid>
</Window>

```
3.在ViewModel中代码如下：

```csharp
using OxyPlot;
using OxyPlot.Axes;
using OxyPlot.Legends;
using OxyPlot.Series;
using Prism.Mvvm;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace BlankApp1.ViewModels
{

    public class ChartData
    {
        public DateTime Date { get; set; }

        public double Total { get; set; }

        public double PassRate { get; set; }
    }

    public class MainWindowViewModel : BindableBase
    {
        private string _title = "Prism Application";
        public string Title
        {
            get { return _title; }
            set { SetProperty(ref _title, value); }
        }

        private PlotModel chartModel;

        public PlotModel ChartModel
        {
            get { return chartModel; }
            set { SetProperty(ref chartModel, value); }
        }

        /// <summary>
        /// 根据数据生成图表模型
        /// </summary>
        /// <param name="list"></param>
        /// <returns></returns>
        private PlotModel CreateChartModel(List<ChartData> list)
        {
            var model = new PlotModel() { Title = "OxyPlot测试" };

            // 添加图例说明
            model.Legends.Add(new Legend
            {
                LegendPlacement = LegendPlacement.Outside,
                LegendPosition = LegendPosition.BottomCenter,
                LegendOrientation = LegendOrientation.Horizontal,
                LegendBorderThickness = 1,
                LegendTextColor = OxyColors.Red
            });

            // 定义第一个Y轴y1，显示数量
            var ay1 = new LinearAxis()
            {
                Key = "y1",
                Position = AxisPosition.Left,
            };


            // 定义第二个Y轴y2，显示百分比
            var ay2 = new LinearAxis()
            {
                Key = "y2",
                Position = AxisPosition.Right,
                Minimum = 0.1,
                MajorStep = .1,
                Maximum = 1,
                LabelFormatter = v => $"{v:P1}"
            };
            // 在第二Y轴坐标50%和80%处显示网格线
            ay2.ExtraGridlines = new double[2] { 0.5, 0.8 };
            ay2.ExtraGridlineStyle = LineStyle.DashDashDot; // 网格线样式

            // 定义X轴为日期轴，从15天前到现在
            var minValue = DateTimeAxis.ToDouble(DateTime.Now.Date.AddDays(-15));
            var maxValue = DateTimeAxis.ToDouble(DateTime.Now.Date);
            var ax = new DateTimeAxis()
            {
                Minimum = minValue,
                Maximum = maxValue,
                StringFormat = "yyyy-MM-dd日",
                MajorStep = 2,
                Position = AxisPosition.Bottom,
                Angle = 45,
                IsZoomEnabled = false
            };

            // 定义柱形图序列，指定数据轴为Y1轴
            var totalBarSeries = new LinearBarSeries();
            totalBarSeries.YAxisKey = "y1";
            totalBarSeries.BarWidth = 10;
            //totalBarSeries.FillColor = OxyColor.FromArgb(69, 76, 175, 80);
            //totalBarSeries.StrokeThickness = 1;
            //totalBarSeries.StrokeColor = OxyColor.FromArgb(255, 76, 175, 80);
            totalBarSeries.Title = "总数";
            // 点击时弹出的label内容
            totalBarSeries.TrackerFormatString = "{0}\r\n{2:dd}日: {4:0}";
            // 设置数据绑定源和字段
            totalBarSeries.ItemsSource = list;
            totalBarSeries.DataFieldX = "Date";
            totalBarSeries.DataFieldY = "Total";
            // 下面为手动添加数据方式
            //totalBarSeries.Points.Add(new DataPoint(DateTimeAxis.ToDouble(DateTime.Now.Date.AddDays(-15)), 333));

            // 定义三色折线图序列，指定数据轴为Y2轴
            var passedRateSeries = new ThreeColorLineSeries();
            passedRateSeries.Title = "通过率";
            passedRateSeries.YAxisKey = "y2";
            // 点击时弹出的label内容
            passedRateSeries.TrackerFormatString = "{0}\r\n{2:dd}日: {4:P1}";
            // 设置颜色阈值范围
            passedRateSeries.LimitHi = .8;
            passedRateSeries.LimitLo = .5;
            // 设置数据绑定源和字段
            passedRateSeries.ItemsSource = list;
            passedRateSeries.DataFieldX = "Date";
            passedRateSeries.DataFieldY = "PassRate";
            // 下面为手动添加数据方式
            //passedRateSeries.Points.Add(new DataPoint(DateTimeAxis.ToDouble(DateTime.Now.Date.AddDays(-15)), .750));
            // 添加图标资源
            model.Series.Add(totalBarSeries);
            model.Series.Add(passedRateSeries);
            model.Axes.Add(ay1);
            model.Axes.Add(ay2);
            model.Axes.Add(ax);
            // 设置图形边框
            model.PlotAreaBorderThickness = new OxyThickness(1, 0, 1, 1);
            return model;
        }

        public MainWindowViewModel()
        {
            ChartModel=CreateChartModel(GetData());
        }

        /// <summary>
        /// 模拟后台异步查询表格数据
        /// </summary>
        /// <returns></returns>
        private List<ChartData> GetData()
        {
            var data = new List<ChartData>()
            {
                new ChartData { Date = DateTime.Now.Date.AddDays(-15), Total = 121, PassRate = .84 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-14), Total = 88, PassRate = .92 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-13), Total = 180, PassRate = .35 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-12), Total = 150, PassRate = .46 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-11), Total = 78, PassRate = .58 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-10), Total = 99, PassRate = .71 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-9), Total = 143, PassRate = .81 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-8), Total = 56, PassRate = .85 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-7), Total = 108, PassRate = .95 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-6), Total = 79, PassRate = .78 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-5), Total = 63, PassRate = .65 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-4), Total = 157, PassRate = .58 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-3), Total = 148, PassRate = .36 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-2), Total = 115, PassRate = .48 },
                new ChartData { Date = DateTime.Now.Date.AddDays(-1), Total = 89, PassRate = .63 },
                new ChartData { Date = DateTime.Now.Date, Total = 121, PassRate = .90 },
            };
            return data;
        }
    }
}

```
4.运行程序，效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/05450ac428604b0aae2e76b36cff2196.png)
### 50.WPF中使用avalonedit
AvalonEdit是基于WPF的代码显示控件，在工业领域应用较多，简单用法如下：

```xml
<UserControl x:Class="BlankApp1.CustomAvalonEditControl"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:BlankApp1"
             xmlns:avalonEdit="http://icsharpcode.net/sharpdevelop/avalonedit"
             mc:Ignorable="d" 
             d:DesignHeight="450" d:DesignWidth="800">
    <Border BorderBrush="LightGray" BorderThickness="2">
        <avalonEdit:TextEditor Name="textEditor" SyntaxHighlighting="XML"  
                               ShowLineNumbers="True" FontFamily="Consolas" 
                               FontSize="15pt" Background="White" 
                               Foreground="Black" />
    </Border>
</UserControl>

```

```csharp
using ICSharpCode.AvalonEdit.CodeCompletion;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace BlankApp1
{
    /// <summary>
    /// CustomAvalonEditControl.xaml 的交互逻辑
    /// </summary>
    public partial class CustomAvalonEditControl : UserControl
    {
        public CustomAvalonEditControl()
        {
            InitializeComponent();
            textEditor.TextChanged += TextEditor_TextChanged;
            textEditor.TextArea.TextEntering += TextArea_TextEntering;
        }

        CompletionWindow completionWindow;
        private void TextArea_TextEntering(object sender, TextCompositionEventArgs e)
        {
            if (e.Text.Length > 0 && completionWindow != null)
            {
                if (!char.IsLetterOrDigit(e.Text[0]))
                {
                    completionWindow.CompletionList.RequestInsertion(e);
                }
            }
        }

        private void TextEditor_TextChanged(object sender, EventArgs e)
        {
            ConfigText = textEditor.Text;
        }

        public string ConfigText
        {
            get { return (string)GetValue(ConfigTextProperty); }
            set { SetValue(ConfigTextProperty, value); }
        }

        public static readonly DependencyProperty ConfigTextProperty =
            DependencyProperty.Register("ConfigText", typeof(string), typeof(CustomAvalonEditControl),
                new FrameworkPropertyMetadata(null, FrameworkPropertyMetadataOptions.BindsTwoWayByDefault, OnConfigTextChanged));

        private static void OnConfigTextChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
        {
            var edt = d as CustomAvalonEditControl;
            if (e.NewValue is string newStr && newStr != edt.textEditor.Text)
                edt.textEditor.Text = e.NewValue as string;
        }
    }
}

```
运行效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e388309183fb4dc799b532a490744824.png)
### 51.WPF中使用CallerMemberName简化InotifyPropertyChanged的实现
在WPF中，当我们使用MVVM的方式实现属性变更通知时，往往要实现INotifyPropertyChanged接口，如下：

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace BlankApp1.Others
{
    public class Practice01 : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
        protected void OnPropertyChanged(string propertyName = "")
        {
            PropertyChangedEventHandler handler = PropertyChanged;
            if (handler != null)
            {
                handler(this, new PropertyChangedEventArgs(propertyName));
            }
        }

        private string text;
		public string Text
		{
			get { return text; }
            set { text = value; OnPropertyChanged("Text"); }
		}
	}
}

```
这么做有一个比较大的隐患，那就是用了字符串的硬编码的方式传递了属性名称，一旦拼写错误或因为重构代码忘记去更新这个字符串时，这样就会导致界面上得不到更新。（本身硬编码的方式来保证两者的一致性就是不靠谱的行为）

虽然这本身并不是问题，但却不是很好的实践。也有人通过一些手段来解决这个问题，有的是通过表达式树，还有的通过Attribute注入的方式。

从性能上来讲，注入是一个比较好的方式，但往往引入了比较复杂的框架。实际上，在C# 5.0中就引入了一个调用方信息的语法方便我们获取调用方的函数名称和位置，通过它可以非常简单快捷的解决上面的这个问题：

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Runtime.CompilerServices;
using System.Text;
using System.Threading.Tasks;

namespace BlankApp1.Others
{
    public class Practice01 : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;
        protected void OnPropertyChanged([CallerMemberName] string propertyName = "")
        {
            PropertyChangedEventHandler handler = PropertyChanged;
            if (handler != null)
            {
                handler(this, new PropertyChangedEventArgs(propertyName));
            }
        }

        private string text;
		public string Text
		{
			get { return text; }
            set { text = value; OnPropertyChanged(); }
		}
	}
}

```
