1.实现效果(相关超详细的注释已写在代码中)：
（1）效果截图：![在这里插入图片描述](https://img-blog.csdnimg.cn/0feddfce9ed347199f8fcb01362f1bab.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/6012b3de9cbf4842819c33ac01654f8b.png#pic_center)
（2）视频展示该界面：
[视频链接](https://www.bilibili.com/video/BV1N34y1h7Z5/)
2.
（1）工程目录：![在这里插入图片描述](https://img-blog.csdnimg.cn/4a59d1dc57d04790b56d32516d6691e3.png#pic_center)
（2）数据库部署：
![在这里插入图片描述](https://img-blog.csdnimg.cn/d522f26f5af8412eaaa8715da26721d9.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5678851ad9834670b942a7a1f20ac414.png#pic_center)
（3）
LoginPage.xaml代码：

```xml
<Window x:Class="Login.LoginPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
        xmlns:local="clr-namespace:Login"
        mc:Ignorable="d"
        Title="登录" Height="760" Width="450"
        TextElement.Foreground="{DynamicResource MaterialDesignBody}"
        Background="{x:Null}"
        AllowsTransparency="True"
        WindowStyle="None"
        WindowStartupLocation="CenterScreen"
        Icon="/Image/loginPage.png">
    <!--TextElement.Foreground="{DynamicResource MaterialDesignBody}"的意思是获取或设置要应用于元素内容的画笔为MaterialDesignBody；
    Background="{x:Null}"的意思是背景设置为空，就是无背景;
    AllowsTransparency="True"的意思是允许窗口透明；
    WindowStyle="None"的意思是设置窗口无边框；
    WindowStartupLocation="CenterScreen"的意思是该窗口在启动时将出现在屏幕中心位置
    -->
    <materialDesign:Card UniformCornerRadius="15" Margin="25" materialDesign:ShadowAssist.ShadowDepth="Depth4" >
        <!--materialDesign:Card是materialDesign中一种卡片式的布局；
        UniformCornerRadius=15表示设置圆角属性为15；
        Margin="25"设置间距为25，顺序是上、右、下、左的边距 ；
        materialDesign:ShadowAssist.ShadowDepth="Depth4"表示设置阴影大小，从Depth到Depth5-->
        <materialDesign:DialogHost x:Name="DialogHost" CloseOnClickAway="True">
            <!--materialDesign:DialogHost表示引用materialDesign库中对话框；
            x:Name="DialogHost"表示给该对话框起一个名字叫DialogHost；
            CloseOnClickAway="True"-->
            <StackPanel>

                <materialDesign:PopupBox HorizontalAlignment="Right" Margin="0 20 20 0" PlacementMode="BottomAndAlignRightEdges"  Height="25">
                    <!--materialDesign:PopupBox表示弹窗；PlacementMode="BottomAndAlignRightEdges"描述 Popup 控件在屏幕上显示的位置为底部和右边缘(即右下角)-->
                    <StackPanel>

                        <StackPanel Margin="16 10 0 6" Orientation="Horizontal" HorizontalAlignment="Center">
                            <TextBlock VerticalAlignment="Center" Text="暗黑模式"/>
                            <ToggleButton Cursor="Hand" ToolTip="切换至暗黑模式" Margin="12 0 8 0" x:Name="themeToggle" IsChecked="{Binding IsDarkTheme}" Click="toggleTheme"/>
                            <!--ToggleButton为状态开关按钮；Cursor="Hand"表示鼠标放上去时鼠标形状为hand；ToolTip表示鼠标放上去时，会弹出一些提示信息；Click="toggleTheme"表示自定义一些按钮点击事件-->
                        </StackPanel>
                        <Button ToolTip="登录遇到问题？" Margin="0 8 0 0" Content="帮助"/>
                        <Button x:Name="btn_exit" ToolTip="退出程序" Content="退出" Click="exitApp"/>
                    </StackPanel>
                </materialDesign:PopupBox>

                <Image Margin="0 60 0 5" Source="Image/head.jpg" Height="100"/>
                <TextBlock Margin="0 25 0 5" HorizontalAlignment="Center" FontSize="28" FontWeight="Bold" Text="欢迎回来!"/>
                <TextBlock FontSize="17" FontWeight="SemiBold" HorizontalAlignment="Center" Text="请登录你的账号密码"/>
                <TextBox Margin="0 50 0 0" x:Name="txtUserName" Width="300" FontSize="18" materialDesign:HintAssist.Hint="请输入用户名" BorderThickness="4" BorderBrush="{DynamicResource MaterialDesignDivider}" Style="{StaticResource MaterialDesignOutlinedTextBox}"/>
                <!--materialDesign:HintAssist.Hint="Enter UserNmae"表示设置提示文本为Enter UserNmae
                BorderThickness="4"表示设置边框宽度为4
                 BorderBrush="{DynamicResource MaterialDesignDivider}"表示设置边框笔刷为materialDesign库中的MaterialDesignDivider
                Style="{StaticResource MaterialDesignOutlinedTextBox}"表示设置样式为materialDesign库中的MaterialDesignOutlinedTextBox
                -->
                <PasswordBox Margin="0 20 0 0" x:Name="txtPassword" Width="300" FontSize="18" materialDesign:HintAssist.Hint="请输入密码" BorderThickness="2" BorderBrush="{DynamicResource MaterialDesignDivider}" Style="{StaticResource MaterialDesignOutlinedPasswordBox}"/>
                <Button Margin="0 20 0 0" x:Name="loginBtn" Style="{StaticResource MaterialDesignFlatMidBgButton}" materialDesign:ShadowAssist.ShadowDepth="Depth0" Height="53" Width="300" materialDesign:ButtonAssist.CornerRadius="10" FontSize="18" Content="登录" Click="loginButtonClick"/>
                <Button Margin="0 20 0 0" x:Name="signupBtn" Style="{StaticResource MaterialDesignFlatButton}" materialDesign:ShadowAssist.ShadowDepth="Depth0" Height="53" Width="300" materialDesign:ButtonAssist.CornerRadius="10" FontSize="18" Content="创建账号" Cursor="Hand" Click="regisButtonClick"/>
            </StackPanel>
        </materialDesign:DialogHost>
    </materialDesign:Card>
</Window>

```
LoginPage.xaml.cs代码：

```csharp
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
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
using MaterialDesignThemes.Wpf;

namespace Login
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class LoginPage : Window
    {
        public LoginPage()
        {
            InitializeComponent();
           
        }
        public bool IsDarkTheme { get; set; }//定义一个属性
        private readonly PaletteHelper paletteHelper = new PaletteHelper();
        private void toggleTheme(object sender, RoutedEventArgs e)//切换主题的按钮点击事件
        {
            ITheme theme = paletteHelper.GetTheme();
            if (IsDarkTheme = theme.GetBaseTheme() == BaseTheme.Dark)//如果当前主题颜色为暗黑色
            {
                IsDarkTheme = false;
                theme.SetBaseTheme(Theme.Light);//设置主题颜色为明亮
            }
            else
            {
                IsDarkTheme = true;
                theme.SetBaseTheme(Theme.Dark);//设置主题颜色为黑色
            }
            paletteHelper.SetTheme(theme);//成功设置界面主题颜色
        }

        private void exitApp(object sender, RoutedEventArgs e)//退出按钮点击事件
        {
            Application.Current.Shutdown();//退出该应用程序（即关掉该窗口）
        }
        protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e)//设置窗口可拖拽
        {
            base.OnMouseLeftButtonDown(e);
            DragMove();
        }

        private void loginButtonClick(object sender, RoutedEventArgs e)//登录按钮点击事件
        {
         //与数据库进行连接；
            //Server=小何表示服务器名称是小何；
            //Database = accountList表示数据库名称为accountList；
            //integrated security = true表示可以在不知道数据库用户名和密码的情况下时，依然可以连接数据库，
                //如果integrated security = false, 或者不写，表示一定要输入正确的数据库登录名和密码。
            SqlConnection sqlConnection = new SqlConnection(@"Server=小何;Database=accountList;Integrated Security=True");
            sqlConnection.Open();
            string add_data = "select * from [dbo].[User] where 用户名=@用户名 and 密码=@密码";
            SqlCommand cmd = new SqlCommand(add_data, sqlConnection);
            cmd.Parameters.AddWithValue("@用户名", txtUserName.Text);
            cmd.Parameters.AddWithValue("@密码", txtPassword.Password);
            cmd.ExecuteNonQuery();
            int count=Convert.ToInt32(cmd.ExecuteScalar());
            sqlConnection.Close();
            txtUserName.Text = "";
            txtPassword.Password = "";
            if (count > 0)//数据库匹配成功
            {
                mainWindow mainWindow=new mainWindow();
                this.Close();
                mainWindow.Show();
            }else//若返回-1,即匹配失败
            {
                MessageBox.Show("账号或密码输入错误！");
            }  
        }
        private void regisButtonClick(object sender, RoutedEventArgs e)//注册按钮点击事件
        {
            registPage reg1 = new registPage();
            this.Close();
            reg1.Show();
        }
    }
}

```
registPage.xaml代码：

```xml
<Window x:Class="Login.registPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
        mc:Ignorable="d"
        Title="Login" Height="760" Width="450"
        TextElement.Foreground="{DynamicResource MaterialDesignBody}"
        Background="{x:Null}"
        AllowsTransparency="True"
        WindowStyle="None"
        WindowStartupLocation="CenterScreen"
        Icon="/Image/registPage.png">
    <materialDesign:Card UniformCornerRadius="15" Margin="25" materialDesign:ShadowAssist.ShadowDepth="Depth4" >
        <!--materialDesign:Card是materialDesign中一种卡片式的布局；
        UniformCornerRadius=15表示设置圆角属性为15；
        Margin="25"设置间距为25，顺序是上、右、下、左的边距 ；
        materialDesign:ShadowAssist.ShadowDepth="Depth4"表示设置阴影大小，从Depth到Depth5-->
        <materialDesign:DialogHost x:Name="DialogHost" CloseOnClickAway="True">
            <!--materialDesign:DialogHost表示引用materialDesign库中对话框；
            x:Name="DialogHost"表示给该对话框起一个名字叫DialogHost；
            CloseOnClickAway="True"-->
            <StackPanel>

                <materialDesign:PopupBox HorizontalAlignment="Right" Margin="0 20 20 0" PlacementMode="BottomAndAlignRightEdges"  Height="25">
                    <!--materialDesign:PopupBox表示弹窗；PlacementMode="BottomAndAlignRightEdges"描述 Popup 控件在屏幕上显示的位置为底部和右边缘(即右下角)-->
                    <StackPanel>

                        <StackPanel Margin="16 10 0 6" Orientation="Horizontal" HorizontalAlignment="Center">
                            <TextBlock VerticalAlignment="Center" Text="暗黑模式"/>
                            <ToggleButton Cursor="Hand" ToolTip="切换至暗黑模式" Margin="12 0 8 0" x:Name="themeToggle" IsChecked="{Binding IsDarkTheme}" Click="toggleTheme"/>
                            <!--ToggleButton为状态开关按钮；Cursor="Hand"表示鼠标放上去时鼠标形状为hand；ToolTip表示鼠标放上去时，会弹出一些提示信息；Click="toggleTheme"表示自定义一些按钮点击事件-->
                        </StackPanel>
                        <Button ToolTip="Having Trouble Logging In?" Margin="0 8 0 0" Content="帮助"/>
                        <Button x:Name="btn_exit" ToolTip="退出程序" Content="退出" Click="exitApp"/>

                    </StackPanel>
                </materialDesign:PopupBox>

                <Image Margin="0 60 0 5" Source="Image/head.jpg" Height="100"/>
                <TextBlock Margin="0 25 0 5" HorizontalAlignment="Center" FontSize="28" FontWeight="Bold" Text="你好!"/>
                <TextBlock FontSize="17" FontWeight="SemiBold" HorizontalAlignment="Center" Text="请注册你的账号"/>
                <TextBox Margin="0 50 0 0" x:Name="registUserName" Width="300" FontSize="18" materialDesign:HintAssist.Hint="输入用户名" BorderThickness="4" BorderBrush="{DynamicResource MaterialDesignDivider}" Style="{StaticResource MaterialDesignOutlinedTextBox}"/>
                
                <!--materialDesign:HintAssist.Hint="Enter UserNmae"表示设置提示文本为Enter UserNmae
                BorderThickness="4"表示设置边框宽度为4
                 BorderBrush="{DynamicResource MaterialDesignDivider}"表示设置边框笔刷为materialDesign库中的MaterialDesignDivider
                Style="{StaticResource MaterialDesignOutlinedTextBox}"表示设置样式为materialDesign库中的MaterialDesignOutlinedTextBox
                -->
                <PasswordBox Margin="0 20 0 0" x:Name="registPassword" Width="300" FontSize="18" materialDesign:HintAssist.Hint="输入密码" BorderThickness="2" BorderBrush="{DynamicResource MaterialDesignDivider}" Style="{StaticResource MaterialDesignOutlinedPasswordBox}"/>
                <PasswordBox Margin="0 20 0 0" x:Name="registPassword1" Width="300" FontSize="18" materialDesign:HintAssist.Hint="请再次输入密码" BorderThickness="2" BorderBrush="{DynamicResource MaterialDesignDivider}" Style="{StaticResource MaterialDesignOutlinedPasswordBox}"/>
                <Button Margin="0 20 0 0" x:Name="registButton" Style="{StaticResource MaterialDesignFlatMidBgButton}" materialDesign:ShadowAssist.ShadowDepth="Depth0" Height="40" Width="300" materialDesign:ButtonAssist.CornerRadius="10" FontSize="18" Content="确认注册账号" Click="registButtonClick"/>
                <Button Margin="0 20 0 0" x:Name="backBtn" Style="{StaticResource MaterialDesignFlatMidBgButton}" materialDesign:ShadowAssist.ShadowDepth="Depth0" Height="40" Width="300" materialDesign:ButtonAssist.CornerRadius="10" FontSize="18" Content="返回登录界面" Click="backButtonClick"/>
            </StackPanel>
        </materialDesign:DialogHost>
    </materialDesign:Card>
</Window>

```
registPage.xaml.cs代码：

```csharp
using MaterialDesignThemes.Wpf;
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
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
using System.Windows.Shapes;

namespace Login
{
    /// <summary>
    /// registPage.xaml 的交互逻辑
    /// </summary>
    public partial class registPage : Window
    {
        public registPage()
        {
            InitializeComponent();
        }
        public bool IsDarkTheme { get; set; }//定义一个属性
        private readonly PaletteHelper paletteHelper = new PaletteHelper();
        private void toggleTheme(object sender, RoutedEventArgs e)//切换主题的按钮点击事件
        {
            ITheme theme = paletteHelper.GetTheme();
            if (IsDarkTheme = theme.GetBaseTheme() == BaseTheme.Dark)//如果当前主题颜色为暗黑色
            {
                IsDarkTheme = false;
                theme.SetBaseTheme(Theme.Light);//设置主题颜色为明亮
            }
            else
            {
                IsDarkTheme = true;
                theme.SetBaseTheme(Theme.Dark);//设置主题颜色为黑色
            }
            paletteHelper.SetTheme(theme);//成功设置界面主题颜色
        }
        private void exitApp(object sender, RoutedEventArgs e)//退出按钮点击事件
        {
            Application.Current.Shutdown();//退出该应用程序（即关掉该窗口）
        }
        protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e)//设置窗口可拖拽
        {
            base.OnMouseLeftButtonDown(e);
            DragMove();
        }
        private void backButtonClick(object sender, RoutedEventArgs e)
        {
            LoginPage mainWindow = new LoginPage();
            this.Close();
            mainWindow.Show();
        }
        private void registButtonClick(object sender, RoutedEventArgs e)//注册按钮点击事件
        {
            if (registPassword1.Password!=registPassword.Password)
            {
                MessageBox.Show("两次输入的密码不一致，请重新输入！");
                registPassword.Password = "";
                registPassword1.Password = "";
            }
            else
            {
                SqlConnection sqlConnection = new SqlConnection(@"Server=小何;Database=accountList;Integrated Security=True");
                sqlConnection.Open();
                string add_data = "insert into [dbo].[User] values(@用户名,@密码)";
                SqlCommand cmd = new SqlCommand(add_data, sqlConnection);
                cmd.Parameters.AddWithValue("@用户名", registUserName.Text);
                cmd.Parameters.AddWithValue("@密码", registPassword.Password);
                cmd.ExecuteNonQuery();
                sqlConnection.Close();
                registUserName.Text = "";
                registPassword.Password = "";
                registPassword1.Password = "";
                MessageBox.Show("注册成功");
                LoginPage mainWindow = new LoginPage();
                this.Close();
                mainWindow.Show();
            }

        }
    }
    }

```
App.xaml代码：

```xml
<Application x:Class="Login.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:Login" xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
             xmlns:viewModel="clr-namespace:Login.MVVM.ViewModel"
             xmlns:view="clr-namespace:Login.MVVM.View"
             StartupUri="LoginPage.xaml">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <materialDesign:BundledTheme BaseTheme="Light" PrimaryColor="Blue" SecondaryColor="Yellow" />
                <!--BaseTheme="Light"表示设置主题为light样式；PrimaryColor="Blue"表示设置界面中各个控件外观为蓝色； SecondaryColor="Lime"表示设置次要首选颜色-->
                <ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesignTheme.Defaults.xaml" />
                <ResourceDictionary Source="Theme/MenuButtonTheme.xaml"/>
                <ResourceDictionary Source="Theme/TextBoxTheme.xaml"/>
            </ResourceDictionary.MergedDictionaries>
            <DataTemplate DataType="{x:Type viewModel:HomeViewModel}">
                <view:HomeView/>
            </DataTemplate>
            <DataTemplate DataType="{x:Type viewModel:MusicViewModel}">
                <view:MusicView/>
            </DataTemplate>
            <DataTemplate DataType="{x:Type viewModel:FeaturedViewModel}">
                <view:FeaturedView/>
            </DataTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```
App.xaml.cs代码：

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;

namespace Login
{
    /// <summary>
    /// App.xaml 的交互逻辑
    /// </summary>
    public partial class App : Application
    {
    }
}

```
而mainWindow则是一个新建的空窗口，没啥代码；

