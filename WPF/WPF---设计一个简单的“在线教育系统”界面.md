@[TOC](目录)
# 只写了简单的界面,没写后台C#逻辑代码;
# 参考链接[油管视频](https://youtu.be/9YbimKI32Wk)
## 一.运行效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/24688b5eb4be4da1add0a8dc9283b417.png#pic_center)
## 二.工程结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/fb6e50846e814d03a48b05b9978bcd48.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7530f87637fa4298b47374265bdffa54.png#pic_center)
## 三.代码展示
（1）App.xaml

```xml
<Application x:Class="WpfApp1.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
             xmlns:local="clr-namespace:WpfApp1"
             StartupUri="MainWindow.xaml">
    <!--引入MaterialDesignThemes库-->
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <materialDesign:BundledTheme BaseTheme="Light" PrimaryColor="DeepPurple" SecondaryColor="Lime" />
                <ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesignTheme.Defaults.xaml" />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>

```
（2）App.xaml.cs

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;

namespace WpfApp1
{
    /// <summary>
    /// App.xaml 的交互逻辑
    /// </summary>
    public partial class App : Application
    {
    }
}

```
（3）MainWindow.xaml

```xml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:customcontrols="clr-namespace:WpfApp1.customcontrols"
        xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d" x:Name="this"
        Title="MainWindow" Icon="/assets/icon.png" Height="735" Width="1024" Background="Transparent" FontSize="15" WindowStartupLocation="CenterScreen" WindowStyle="None" ResizeMode="NoResize" >

    <!--初始化资源字典，该字典用来存放所有的app图标-->
    <Window.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <!--MergedDictionaries获取 ResourceDictionary 字典的集合，这些字典构成了合并字典中的各种资源字典-->
                <ResourceDictionary Source="/assets/icons.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Window.Resources>

    <!--窗口背景和圆角-->
    <Border CornerRadius="20" Background="White">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="200"/>
                <ColumnDefinition/>
                <ColumnDefinition Width="300"/>
            </Grid.ColumnDefinitions>
            <!--绘制一根竖直的分隔细线-->
            <Line  Stroke="Black"  HorizontalAlignment="Right"  Y1="0" Y2="{Binding Height,ElementName=this}" StrokeThickness="0.5" /> <!--Line用于在两个点之间绘制直线;Y1是获取或设置 Line 起始点的 y 坐标；Y2是获取或设置 Line 终结点的 y 坐标；StrokeThickness是获取或设置笔划轮廓的 Shape 宽度。-->

            <!--左边区域-->
            <Grid Grid.Column="0">
                <!--左上角logo区域-->
                <Grid Grid.Column="0" HorizontalAlignment="Center" VerticalAlignment="Top" Margin="0,55">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="25"/>
                        <ColumnDefinition/>
                    </Grid.ColumnDefinitions>
                    <Path Data="{StaticResource logo}" Fill="Blue" Stretch="Uniform"/>
                    <TextBlock Grid.Column="1" Text="在线教育平台" FontWeight="SemiBold" VerticalAlignment="Center" Margin="5,0"/>
                </Grid>
                <!--左边菜单栏区域-->
                <Border Grid.Column="0" CornerRadius="10" Background="WhiteSmoke" Width="150" Margin="25,132,25,419.2">
                    <StackPanel Orientation="Vertical" VerticalAlignment="Center">
                        <customcontrols:MenuButton Icon="{StaticResource home}" IconWidth="10" IndicatorBrush="#bebebe" Text="主页" IsSelected="True" VerticalAlignment="Center" GroupName="MenuButton"/>
                        <customcontrols:MenuButton Icon="{StaticResource discover}" IconWidth="10" IndicatorBrush="#bebebe" Text="发现" IsSelected="True" VerticalAlignment="Center" GroupName="MenuButton"/>
                        <customcontrols:MenuButton Icon="{StaticResource messages}" IconWidth="10" IndicatorBrush="#bebebe" Text="信息" IsSelected="True" VerticalAlignment="Center" GroupName="MenuButton"/>
                        <customcontrols:MenuButton Icon="{StaticResource settings}" IconWidth="10" IndicatorBrush="#bebebe" Text="Home" IsSelected="True" VerticalAlignment="Center" GroupName="MenuButton"/>
                    </StackPanel>
                </Border>
                <!--更新课程按钮-->
                <Button Height="52" Margin="25,338,25,345.2">
                    <Button.Style>
                        <Style TargetType="{x:Type Button}">
                            <Setter Property="HorizontalAlignment" Value="Center"/>
                            <Setter Property="VerticalAlignment" Value="Center"/>
                            <Setter Property="Height" Value="53"/>
                            <Setter Property="Width" Value="150"/>
                            <Setter Property="Margin" Value="0,-50,0,0"/>
                            <Setter Property="Background" Value="WhiteSmoke"/>
                            <Setter Property="Template">
                                <Setter.Value>
                                    <ControlTemplate TargetType="{x:Type Button}">
                                        <Border Background="{TemplateBinding Background}" CornerRadius="10">
                                            <Grid>
                                                <Grid.ColumnDefinitions>
                                                    <ColumnDefinition Width="45"/>
                                                    <ColumnDefinition/>

                                                </Grid.ColumnDefinitions>
                                                <Border Grid.Column="0" CornerRadius="10" Height="28" Width="28" Background="White" HorizontalAlignment="Center" VerticalAlignment="Center">
                                                    <Path Data="{StaticResource light}" Width="28" Height="28" Fill="#8c14fd" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                </Border>
                                                <Grid Grid.Column="1">
                                                    <TextBlock HorizontalAlignment="Left" VerticalAlignment="Center" Margin="15,5,5,5">
                                                    <Run Text="更新课程" FontSize="16" FontWeight="Bold"/>
                                                    </TextBlock>
                                                </Grid>

                                            </Grid>

                                        </Border>
                                    </ControlTemplate>
                                </Setter.Value>
                            </Setter>
                        </Style>
                    </Button.Style>
                </Button>
                <!--课程分数-->
                <Border CornerRadius="10" Background="WhiteSmoke" HorizontalAlignment="Center" VerticalAlignment="Center" Height="130" Width="70" Margin="5,500,95,0">
                    <Grid>
                        <Path Data="{StaticResource medal}" Fill="#79cb86" HorizontalAlignment="Left" VerticalAlignment="Top" Height="16" Stretch="Uniform" Margin="12,12,0,0"/>
                        <TextBlock HorizontalAlignment="Left" VerticalAlignment="Bottom" Margin="12,0,0,16">
                        <Run Text="1800" FontWeight="Bold" FontSize="12"/>
                        <LineBreak/>
                        <Run Text="分" FontWeight="Bold" FontSize="12"/>
                        </TextBlock>
                    </Grid>
                </Border>
                <!--完成进度-->
                <Border CornerRadius="10" Background="WhiteSmoke" HorizontalAlignment="Center" VerticalAlignment="Center" Height="130" Width="70" Margin="90,500,5,0">
                    <Grid>
                        <Path Data="{StaticResource progress}" Fill="#79cb86" HorizontalAlignment="Left" VerticalAlignment="Top" Height="16" Stretch="Uniform" Margin="12,12,0,0"/>
                        <TextBlock HorizontalAlignment="Left" VerticalAlignment="Bottom" Margin="12,0,0,16">
                        <Run Text="45.3%" FontWeight="Bold" FontSize="12"/>
                        <LineBreak/>
                        <Run Text="进度" FontWeight="Bold" FontSize="12"/>
                        </TextBlock>
                    </Grid>
                </Border>
            </Grid>
            <!--中间部分-->
            <Grid Grid.Column="1">
                <Grid.RowDefinitions>
                    <RowDefinition Height="150"/>
                    <RowDefinition Height="370"/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                
                <!--中间部分区域-->
                <Grid  Grid.Row="0" Margin="40,0,0,0">
                    <TextBlock Margin="0,50,0,0">
                         <Run Text="立刻播放" FontWeight="SemiBold"/>
                    </TextBlock>
                    <UniformGrid  Columns="9" Margin="0,80,0,0" HorizontalAlignment="Stretch" VerticalAlignment="Top">
                        <customcontrols:StreamingStatusButton ImageSource="assets/s1.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s2.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s3.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s4.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s5.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s6.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s7.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/s8.png"/>
                        <customcontrols:StreamingStatusButton ImageSource="/assets/.png"/>
                    </UniformGrid>
                </Grid>
                <Grid Grid.Row="1" Margin="40,0,0,0">
                    <TextBlock  FontWeight="SemiBold" HorizontalAlignment="Left" VerticalAlignment="Top">
                        <Run Text="这周流行"  FontWeight="SemiBold"/>
                    </TextBlock>
                    <Grid Grid.Column="1" HorizontalAlignment="Stretch" VerticalAlignment="Top" Margin="0">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        
                        <Border  Grid.Column="0" CornerRadius="12" HorizontalAlignment="Left" Height="301" Width="198" Margin="0,30,20,0" VerticalAlignment="Top">
                            <Border.Background>
                                <ImageBrush ImageSource="/assets/1.jpg"/>
                            </Border.Background>
                            <TextBlock  Margin="10,20,0,0">
                        <Run  Text="C语言程序设计" Foreground="White" FontWeight="SemiBold"/>
                        <LineBreak/>
                        <Run  Text="@小明老师" FontSize="13" Foreground="White" FontWeight="SemiBold"/>
                            </TextBlock>
                        </Border>
                        
                        <Border  Grid.Column="1" CornerRadius="12" HorizontalAlignment="Left" Height="301" Width="198" Margin="-30,30,0,0" VerticalAlignment="Top">
                            <Border.Background>
                                <ImageBrush ImageSource="/assets/3.jpg"/>
                            </Border.Background>
                            <TextBlock  Margin="10,20,0,0">
                                <Run  Text="数据结构" Foreground="White" FontWeight="SemiBold"/>
                                 <LineBreak/>
                                 <Run  Text="@小王老师" FontSize="13" Foreground="White" FontWeight="SemiBold"/>
                            </TextBlock>
                        </Border>
                        
                    </Grid>
                    <TextBlock Margin="0,345,0,0">
                         <Run Text="立刻播放" FontWeight="SemiBold"/>
                    </TextBlock>
                </Grid>
                <Grid Grid.Row="2" Margin="40,0,0,0">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition/>
                            <ColumnDefinition/>
                        </Grid.ColumnDefinitions>
                        <Grid.RowDefinitions>
                            <RowDefinition/>
                            <RowDefinition/>
                        </Grid.RowDefinitions>

                        <Grid Grid.Row="0" Grid.Column="0">
                            <Button>
                                <Button.Style>
                                    <Style TargetType="{x:Type Button}">
                                        <Setter Property="HorizontalAlignment" Value="Center"/>
                                        <Setter Property="VerticalAlignment" Value="Center"/>
                                        <Setter Property="Height" Value="80"/>
                                        <Setter Property="Width" Value="200"/>
                                        <Setter Property="Margin" Value="0,0,0,0"/>
                                        <Setter Property="Background" Value="WhiteSmoke"/>
                                        <Setter Property="Template">
                                            <Setter.Value>
                                                <ControlTemplate TargetType="{x:Type Button}">
                                                    <Border Background="{TemplateBinding Background}" CornerRadius="10">
                                                        <Grid>
                                                            <Grid.ColumnDefinitions>
                                                                <ColumnDefinition Width="40"/>
                                                                <ColumnDefinition/>
                                                                <ColumnDefinition Width="35"/>
                                                            </Grid.ColumnDefinitions>
                                                            <Border Grid.Column="0" Margin="10,0,0,0" CornerRadius="10" Height="28" Width="28" Background="White" HorizontalAlignment="Center" VerticalAlignment="Center">
                                                                <Path Data="{StaticResource play}" Width="28" Height="28" Fill="#1afa29" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                            <Grid Grid.Column="1">
                                                                <TextBlock  Margin="1,20,1,10">
                                                                    <Run  Text="面向对象程序设计"  FontWeight="SemiBold"/>
                                                                    <LineBreak/>
                                                                    <Run  Text="@小王老师" FontSize="13"  FontWeight="SemiBold"/>
                                                                </TextBlock>
                                                            </Grid>
                                                            <Border Grid.Column="2" Margin="-30,0,0,0">
                                                                <Path Data="{StaticResource add}" Width="28" Height="28" Fill="#ffbebebe" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                        </Grid>

                                                    </Border>
                                                </ControlTemplate>
                                            </Setter.Value>
                                        </Setter>
                                    </Style>
                                </Button.Style>
                            </Button>
                        </Grid>
                        <Grid Grid.Row="0" Grid.Column="1">
                            <Button>
                                <Button.Style>
                                    <Style TargetType="{x:Type Button}">
                                        <Setter Property="HorizontalAlignment" Value="Center"/>
                                        <Setter Property="VerticalAlignment" Value="Center"/>
                                        <Setter Property="Height" Value="80"/>
                                        <Setter Property="Width" Value="200"/>
                                        <Setter Property="Margin" Value="0,0,0,0"/>
                                        <Setter Property="Background" Value="WhiteSmoke"/>
                                        <Setter Property="Template">
                                            <Setter.Value>
                                                <ControlTemplate TargetType="{x:Type Button}">
                                                    <Border Background="{TemplateBinding Background}" CornerRadius="10">
                                                        <Grid>
                                                            <Grid.ColumnDefinitions>
                                                                <ColumnDefinition Width="40"/>
                                                                <ColumnDefinition/>
                                                                <ColumnDefinition Width="35"/>
                                                            </Grid.ColumnDefinitions>
                                                            <Border Grid.Column="0" Margin="10,0,0,0" CornerRadius="10" Height="28" Width="28" Background="White" HorizontalAlignment="Center" VerticalAlignment="Center">
                                                                <Path Data="{StaticResource play}" Width="28" Height="28" Fill="#1afa29" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                            <Grid Grid.Column="1">
                                                                <TextBlock  Margin="30,20,30,10">
                                                                    <Run  Text="操作系统"  FontWeight="SemiBold"/>
                                                                    <LineBreak/>
                                                                    <Run  Text="@小赵老师" FontSize="13"  FontWeight="SemiBold"/>
                                                                </TextBlock>
                                                            </Grid>
                                                            <Border Grid.Column="2" Margin="-30,0,0,0">
                                                                <Path Data="{StaticResource add}" Width="28" Height="28" Fill="#ffbebebe" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                        </Grid>

                                                    </Border>
                                                </ControlTemplate>
                                            </Setter.Value>
                                        </Setter>
                                    </Style>
                                </Button.Style>
                            </Button>
                        </Grid>
                        <Grid Grid.Row="1" Grid.Column="0">
                            <Button>
                                <Button.Style>
                                    <Style TargetType="{x:Type Button}">
                                        <Setter Property="HorizontalAlignment" Value="Center"/>
                                        <Setter Property="VerticalAlignment" Value="Center"/>
                                        <Setter Property="Height" Value="80"/>
                                        <Setter Property="Width" Value="200"/>
                                        <Setter Property="Margin" Value="0,0,0,0"/>
                                        <Setter Property="Background" Value="WhiteSmoke"/>
                                        <Setter Property="Template">
                                            <Setter.Value>
                                                <ControlTemplate TargetType="{x:Type Button}">
                                                    <Border Background="{TemplateBinding Background}" CornerRadius="10">
                                                        <Grid>
                                                            <Grid.ColumnDefinitions>
                                                                <ColumnDefinition Width="40"/>
                                                                <ColumnDefinition/>
                                                                <ColumnDefinition Width="35"/>
                                                            </Grid.ColumnDefinitions>
                                                            <Border Grid.Column="0" Margin="10,0,0,0" CornerRadius="10" Height="28" Width="28" Background="White" HorizontalAlignment="Center" VerticalAlignment="Center">
                                                                <Path Data="{StaticResource play}" Width="28" Height="28" Fill="#1afa29" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                            <Grid Grid.Column="1">
                                                                <TextBlock  Margin="20,20,20,10">
                                                                    <Run  Text="计算机网络"  FontWeight="SemiBold"/>
                                                                    <LineBreak/>
                                                                    <Run  Text="@小孔老师" FontSize="13"  FontWeight="SemiBold"/>
                                                                </TextBlock>
                                                            </Grid>
                                                            <Border Grid.Column="2" Margin="-30,0,0,0">
                                                                <Path Data="{StaticResource add}" Width="28" Height="28" Fill="#ffbebebe" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                        </Grid>

                                                    </Border>
                                                </ControlTemplate>
                                            </Setter.Value>
                                        </Setter>
                                    </Style>
                                </Button.Style>
                            </Button>
                        </Grid>
                        <Grid Grid.Row="1" Grid.Column="1">
                            <Button>
                                <Button.Style>
                                    <Style TargetType="{x:Type Button}">
                                        <Setter Property="HorizontalAlignment" Value="Center"/>
                                        <Setter Property="VerticalAlignment" Value="Center"/>
                                        <Setter Property="Height" Value="80"/>
                                        <Setter Property="Width" Value="200"/>
                                        <Setter Property="Margin" Value="0,0,0,0"/>
                                        <Setter Property="Background" Value="WhiteSmoke"/>
                                        <Setter Property="Template">
                                            <Setter.Value>
                                                <ControlTemplate TargetType="{x:Type Button}">
                                                    <Border Background="{TemplateBinding Background}" CornerRadius="10">
                                                        <Grid>
                                                            <Grid.ColumnDefinitions>
                                                                <ColumnDefinition Width="40"/>
                                                                <ColumnDefinition/>
                                                                <ColumnDefinition Width="35"/>
                                                            </Grid.ColumnDefinitions>
                                                            <Border Grid.Column="0" Margin="10,0,0,0" CornerRadius="10" Height="28" Width="28" Background="White" HorizontalAlignment="Center" VerticalAlignment="Center">
                                                                <Path Data="{StaticResource play}" Width="28" Height="28" Fill="#1afa29" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                            <Grid Grid.Column="1">
                                                                <TextBlock  Margin="10,20,10,10">
                                                                    <Run  Text="计算机组成原理"  FontWeight="SemiBold"/>
                                                                    <LineBreak/>
                                                                    <Run  Text="@小马老师" FontSize="13"  FontWeight="SemiBold"/>
                                                                </TextBlock>
                                                            </Grid>
                                                            <Border Grid.Column="2" Margin="-30,0,0,0">
                                                                <Path Data="{StaticResource add}" Width="28" Height="28" Fill="#ffbebebe" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                            </Border>
                                                        </Grid>

                                                    </Border>
                                                </ControlTemplate>
                                            </Setter.Value>
                                        </Setter>
                                    </Style>
                                </Button.Style>
                            </Button>
                        </Grid>
                    </Grid>
                </Grid>

            </Grid>

            <!--绘制一根竖直的分隔细线-->
            <Line Grid.Column="1" Stroke="Black"  HorizontalAlignment="Right"  Y1="0" Y2="{Binding Height,ElementName=this}" StrokeThickness="0.5" /><!--Line用于在两个点之间绘制直线;Y1是获取或设置 Line 起始点的 y 坐标；Y2是获取或设置 Line 终结点的 y 坐标；StrokeThickness是获取或设置笔划轮廓的 Shape 宽度。-->
            <!--右边区域-->
            <Grid Grid.Column="2" Margin="40,0,0,0">
                <Grid.RowDefinitions>
                    <RowDefinition />
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Grid Grid.Row="0">
                    <TextBlock Margin="0,50,0,0">
                         <Run Text="探索更多" FontWeight="SemiBold"/>
                    </TextBlock>
                    <Path Margin="100,-45,0,85" Data="{StaticResource more}" Width="28" Height="28" Fill="#ffbebebe" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                    <StackPanel  Margin="-30,0,0,0" Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
                        <TextBox Height="50" Width="150" Style="{StaticResource MaterialDesignFilledTextBox}" />
                        <Button Height="50" Style="{StaticResource MaterialDesignRaisedButton}" Content="搜索" />
                    </StackPanel>
                     
                </Grid>
                <Grid Grid.Row="1" Margin="0,-50,0,0">
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition />
                            <ColumnDefinition />
                        </Grid.ColumnDefinitions>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto"/>
                            <RowDefinition/>
                        </Grid.RowDefinitions>

                        <Border  CornerRadius="30" Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="150" Width="230" Margin="10" VerticalAlignment="Top">
                            <Border.Background>
                                <ImageBrush ImageSource="/assets/4.jpg"/>
                            </Border.Background>
                            <TextBlock  Margin="10,20,0,0">
                        <Run  Text="Web程序设计" Foreground="White" FontWeight="SemiBold"/>
                        <LineBreak/>
                        <Run  Text="@小李老师" FontSize="13" Foreground="White" FontWeight="SemiBold"/>
                            </TextBlock>
                        </Border>
                        <Border  CornerRadius="20" Margin="15,10,0,-50" Height="150" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Left"  VerticalAlignment="Top">
                            <Border.Background>
                                <ImageBrush ImageSource="/assets/5.jpg"/>
                            </Border.Background>
                            <TextBlock  Margin="0,20,0,0" Width="105">
                                <Run  Text="编译原理" Foreground="White" FontSize="13" FontWeight="SemiBold"/>
                                <LineBreak/>
                                <Run  Text="@小明老师" FontSize="10" Foreground="White" FontWeight="SemiBold"/>
                            </TextBlock>
                        </Border>
                        <Border  CornerRadius="20" Margin="10,10,-20,-50" Height="150" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Left"  VerticalAlignment="Top">
                            <Border.Background>
                                <ImageBrush ImageSource="/assets/6.jpg"/>
                            </Border.Background>
                            <TextBlock  Margin="0,20,0,0">
                                <Run  Text="Android程序设计" Foreground="White" FontWeight="SemiBold" FontSize="13"/>
                                <LineBreak/>
                                <Run  Text="@小王老师" FontSize="10" Foreground="White" FontWeight="SemiBold"/>
                            </TextBlock>
                        </Border>
                    </Grid>

                </Grid>
                <Grid Grid.Row="2">
                    <!--联系我们按钮-->
                    <Button >
                        <Button.Style>
                            <Style TargetType="{x:Type Button}">
                                <Setter Property="HorizontalAlignment" Value="Center"/>
                                <Setter Property="VerticalAlignment" Value="Center"/>
                                <Setter Property="Height" Value="100"/>
                                <Setter Property="Width" Value="200"/>
                                <Setter Property="Margin" Value="0,0,0,-80"/>
                                <Setter Property="Background" Value="WhiteSmoke"/>
                                <Setter Property="Template">
                                    <Setter.Value>
                                        <ControlTemplate TargetType="{x:Type Button}">
                                            <Border Background="{TemplateBinding Background}" CornerRadius="10">
                                                <Grid>
                                                    <Grid.ColumnDefinitions>
                                                        <ColumnDefinition Width="28"/>
                                                        <ColumnDefinition/>
                                                        <ColumnDefinition Width="35"/>
                                                    </Grid.ColumnDefinitions>
                                                    <Border Grid.Column="0" CornerRadius="10" Height="28" Width="28" Background="White" HorizontalAlignment="Center" VerticalAlignment="Center">
                                                        <Path Data="{StaticResource contact}" Width="28" Height="28" Fill="#1afa29" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                    </Border>
                                                    <Grid Grid.Column="1">
                                                        <TextBlock HorizontalAlignment="Left" VerticalAlignment="Center" Margin="15,5,5,5">
                                                    <Run Text="联系我们" FontSize="16" FontWeight="Bold"/>
                                                        </TextBlock>
                                                    </Grid>
                                                    <Border Grid.Column="2" >
                                                        <Path Data="{StaticResource detail}" Width="28" Height="28" Fill="#ffbebebe" HorizontalAlignment="Center" VerticalAlignment="Center" Stretch="Uniform"/>
                                                    </Border>
                                                </Grid>

                                            </Border>
                                        </ControlTemplate>
                                    </Setter.Value>
                                </Setter>
                            </Style>
                        </Button.Style>
                    </Button>
                </Grid>
            </Grid>
        </Grid>
    </Border>
</Window>

```
（4）MainWindow.xaml.cs

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

namespace WpfApp1
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        protected override void OnMouseLeftButtonDown(MouseButtonEventArgs e)//设置窗口可拖拽
        {
            base.OnMouseLeftButtonDown(e);
            DragMove();
        }
    }
}

```
（5）MenuButton.xaml

```xml
<UserControl x:Class="WpfApp1.customcontrols.MenuButton"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:WpfApp1.customcontrols"
             mc:Ignorable="d" x:Name="this">

    <!--绘制菜单按钮用户自定义控件-->
    <UserControl.Resources>
        <!--如果我们没有在主窗口绑定任何图标，就设置默认图标-->
        <PathGeometry x:Key="DefaultIcon" Figures="M130.983 703.413l63.491 0 0 126.957-63.491 0 0-126.957ZM829.029 4.175 196.834 4.175c-104.742 0-189.659 84.917-189.659 189.659l0 632.177c0 104.768 84.917 189.659 189.659 189.659l632.196 0c104.742 0 189.659-84.891 189.659-189.659l0-632.177C1018.688 89.092 933.771 4.175 829.029 4.175zM354.691 68.548l316.949 0 0 354.192-94.972-94.981-63.502 63.482-63.495-63.482-94.98 94.984L354.691 68.548zM955.468 826.011c0 69.839-56.607 126.452-126.439 126.452L196.834 952.463c-69.832 0-126.439-56.613-126.439-126.452l0-632.177c0-69.839 56.607-126.42 126.439-126.42l93.857 0 0 509.845 158.986-158.994 63.487 63.475 63.494-63.476 158.981 158.998 0-509.848 93.39 0c69.832 0 126.439 56.581 126.439 126.42L955.468 826.011zM640.914 703.413l63.491 0 0 126.957-63.491 0 0-126.957ZM259.972 703.413l63.485 0 0 126.957-63.485 0 0-126.957ZM385.948 703.413l63.491 0 0 126.957-63.491 0 0-126.957ZM513.931 703.413l63.498 0 0 126.957-63.498 0 0-126.957Z "/>

        <Style x:Key="IndicatorStyle" TargetType="{x:Type Border}">
            <Setter Property="HorizontalAlignment" Value="Left"/>
            <Setter Property="VerticalAlignment" Value="Stretch"/>
            <Setter Property="CornerRadius" Value="{Binding IndicatorCornerEadius,ElementName=this,FallbackValue=2,TargetNullValue=2}"/>
            <Setter Property="Background" Value="{Binding IndicatorBrush,ElementName=this}"/>
            <Setter Property="Visibility" Value="Hidden"/>
            <Setter Property="Width" Value="{Binding IndicatorWidth,ElementName=this,FallbackValue=4,TargetNullValue=4}"/>
            <Style.Triggers>
                <DataTrigger Binding="{Binding IsMouseOver,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=ToggleButton}}" Value="True">
                    <Setter Property="Visibility" Value="Visible"/>
                </DataTrigger>
                <DataTrigger Binding="{Binding IsChecked,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=ToggleButton}}" Value="True">
                    <Setter Property="Visibility" Value="Visible"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
        
        <Style x:Key="MenuIconStyle" TargetType="{x:Type Path}">
            <Setter Property="Fill" Value="#bebebe"/>
            <Style.Triggers>
                <DataTrigger Binding="{Binding IsMouseOver,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=ToggleButton}}" Value="True">
                    <Setter Property="Fill" Value="#fff7542e"/>
                </DataTrigger>

                <DataTrigger Binding="{Binding IsChecked,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=ToggleButton}}" Value="True">
                    <Setter Property="Fill" Value="#fff7542e"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>

        <Style x:Key="MenuTextStyle" TargetType="{x:Type TextBlock}">
            <Setter Property="Foreground" Value="#bebebe"/>
            <Setter Property="VerticalAlignment" Value="Center"/>
            <Setter Property="FontSize" Value="10"/>
            <Setter Property="FontWeight" Value="Normal"/>
            <Setter Property="FontFamily" Value="Segoe UI Semibold"/>
            <Setter Property="Margin" Value="13,0,0,0"/>
            <Style.Triggers>
                <DataTrigger Binding="{Binding IsMouseOver,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=ToggleButton}}" Value="True">
                    <Setter Property="Foreground" Value="Black"/>
                </DataTrigger>

                <DataTrigger Binding="{Binding IsChecked,RelativeSource={RelativeSource Mode=FindAncestor,AncestorType=ToggleButton}}" Value="True">
                    <Setter Property="Foreground" Value="Black"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
        
        <Style x:Key="MenuButtonStyle" TargetType="{x:Type ToggleButton}">
            <Setter Property="Height" Value="32"/>
            <Setter Property="Background" Value="White"/>
            <Setter Property="HorizontalAlignment" Value="Stretch"/>
            <Setter Property="HorizontalContentAlignment" Value="Center"/>
            <Setter Property="Background" Value="Transparent"/>
            <Setter Property="BorderThickness" Value="0"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="{x:Type ToggleButton}">
                        <Border Background="{TemplateBinding Background}" BorderThickness="{TemplateBinding BorderThickness}" Padding="{Binding Padding,ElementName=this}">
                            <Grid>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="Auto"/> <!--存放图标icon-->
                                    <ColumnDefinition/><!--存放文字text-->
                                </Grid.ColumnDefinitions>

                                <!--indiactor-->
                                <Border Style="{StaticResource IndicatorStyle}"/>

                                <!--图标icon-->
                                <Path Data="{Binding Icon,ElementName=this,FallbackValue={StaticResource DefaultIcon}, TargetNullValue={StaticResource DefaultIcon}}" Margin="{Binding IconMargin,FallbackValue='20,0,0,0',TargetNullValue='20,0,0,0'}"
                                      Stretch="Uniform" Width="{Binding IconWidth,ElementName=this,FallbackValue=10,TargetNullValue=10}"
                                      Style="{StaticResource MenuIconStyle}"/>
                                
                                <!--text文字-->
                                <TextBlock Style="{StaticResource MenuTextStyle}" Grid.Column="1" Text="{Binding Text,ElementName=this,FallbackValue=MenuText,TargetNullValue=MenuText}"/>
                            </Grid>
                        </Border>
                        <ControlTemplate.Triggers>
                            <DataTrigger Binding="{Binding IsChecked,ElementName=this}" Value="True">
                                <Setter Property="IsChecked" Value="True"/>
                            </DataTrigger>
                        </ControlTemplate.Triggers>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </UserControl.Resources>
    <Grid>
        <RadioButton Style="{StaticResource MenuButtonStyle}" GroupName="{Binding GroupName,ElementName=this}"/>
    </Grid>
</UserControl>

```
（6）MenuButton.xaml.cs

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
    /// MenuButton.xaml 的交互逻辑
    /// </summary>
    public partial class MenuButton : UserControl
    {
        public MenuButton()
        {
            InitializeComponent();
        }
        //下面是注册的一些依赖属性（快捷键：输入propdp,连续按两次tab键即可快捷注册依赖属性）
       
        public PathGeometry Icon
        {
            get { return (PathGeometry)GetValue(IconProperty); }
            set { SetValue(IconProperty, value); }
        }

        // Using a DependencyProperty as the backing store for Icon.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty IconProperty =
            DependencyProperty.Register("Icon", typeof(PathGeometry), typeof(MenuButton));

        public int IconWidth
        {
            get { return (int)GetValue(IconWidthProperty); }
            set { SetValue(IconWidthProperty, value); }
        }

        // Using a DependencyProperty as the backing store for IconWidth.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty IconWidthProperty =
            DependencyProperty.Register("IconWidth", typeof(int), typeof(MenuButton));

        public SolidColorBrush IndicatorBrush
        {
            get { return (SolidColorBrush)GetValue(IndicatorBrushProperty); }
            set { SetValue(IndicatorBrushProperty, value); }
        }

        // Using a DependencyProperty as the backing store for IndicatorBrush.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty IndicatorBrushProperty =
            DependencyProperty.Register("IndicatorBrush", typeof(SolidColorBrush), typeof(MenuButton));

        public int IndicatorCornerEadius
        {
            get { return (int)GetValue(IndicatorCornerEadiusProperty); }
            set { SetValue(IndicatorCornerEadiusProperty, value); }
        }

        // Using a DependencyProperty as the backing store for IndicatorCornerEadius.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty IndicatorCornerEadiusProperty =
            DependencyProperty.Register("IndicatorCornerEadius", typeof(int), typeof(MenuButton));

        public string Text
        {
            get { return (string)GetValue(TextProperty); }
            set { SetValue(TextProperty, value); }
        }

        // Using a DependencyProperty as the backing store for Text.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty TextProperty =
            DependencyProperty.Register("Text", typeof(string), typeof(MenuButton));


        public new Thickness Padding
        {
            get { return (Thickness)GetValue( PaddingProperty); }
            set { SetValue( PaddingProperty, value); }
        }

        // Using a DependencyProperty as the backing store for Thickness Padding.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PaddingProperty =
            DependencyProperty.Register(" Padding", typeof(Thickness), typeof(MenuButton));

        public bool IsSelected
        {
            get { return (bool)GetValue(IsSelectedProperty); }
            set { SetValue(IsSelectedProperty, value); }
        }

        // Using a DependencyProperty as the backing store for IsSelected.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty IsSelectedProperty =
            DependencyProperty.Register("IsSelected", typeof(bool), typeof(MenuButton));

        public string GroupName
        {
            get { return (string)GetValue(GroupNameProperty); }
            set { SetValue(GroupNameProperty, value); }
        }

        // Using a DependencyProperty as the backing store for GroupName.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty GroupNameProperty =
            DependencyProperty.Register("GroupName", typeof(string), typeof(MenuButton));

    }

    }

```
（7）StreamingStatusButton.xaml

```xml
<UserControl x:Class="WpfApp1.customcontrols.StreamingStatusButton"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:WpfApp1.customcontrols"
             mc:Ignorable="d" x:Name="this">
    <Grid>
        <Border x:Name="border" Background="Transparent" CornerRadius="10" RenderTransformOrigin="0.5,0.5" Height="35" Width="35" BorderThickness="1">
            <Rectangle StrokeThickness="2" Stretch="Uniform">
                <Rectangle.Fill>
                    <ImageBrush ImageSource="{Binding ImageSource,ElementName=this}"/>
                </Rectangle.Fill>
            </Rectangle>
        </Border>
    </Grid>
</UserControl>

```
（8）StreamingStatusButton.xaml.cs

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

        //注册依赖属性
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

