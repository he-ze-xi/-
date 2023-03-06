
@[TOC](文章目录)

---

# 前言
最近在空闲时间学习WPF界面设计工具Blend，之前写WPF的界面都是完全依靠手敲代码的方式，这种方式往往很低效率而且很难做到一些复杂的效果。比如动画，手敲代码实现动画的话，往往要写很多代码；而用Blend的话，只需两三分钟就可以实现一个效果很不错的动画，方便快捷，因此开始记录Blend工具学习之路，一边摸索一边记录。

### 一.初用Blend之喜
这是花了半个小时用Blend工具设计出的一个简单界面，效果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/f078d6eb603a47bf80d8de23d5b5c21b.gif#pic_center)
这是Blend自动生成的代码：

```xml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="600" Width="900" ResizeMode="NoResize" WindowStartupLocation="CenterScreen" WindowStyle="None" AllowsTransparency="True" Background="{x:Null}">
    <Window.Resources>
        <Style x:Key="FocusVisual">
            <Setter Property="Control.Template">
                <Setter.Value>
                    <ControlTemplate>
                        <Rectangle Margin="2" StrokeDashArray="1 2" Stroke="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}" SnapsToDevicePixels="true" StrokeThickness="1"/>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
        <SolidColorBrush x:Key="Button.Static.Background" Color="White"/>
        <SolidColorBrush x:Key="Button.Static.Border" Color="White"/>
        <SolidColorBrush x:Key="Button.MouseOver.Background" Color="White"/>
        <SolidColorBrush x:Key="Button.MouseOver.Border" Color="White"/>
        <SolidColorBrush x:Key="Button.MouseOver.Foreground" Color="Black"/>
        <SolidColorBrush x:Key="Button.Pressed.Background" Color="White"/>
        <SolidColorBrush x:Key="Button.Pressed.Border" Color="White"/>
        <SolidColorBrush x:Key="Button.Disabled.Background" Color="White"/>
        <SolidColorBrush x:Key="Button.Disabled.Border" Color="White"/>
        <SolidColorBrush x:Key="Button.Disabled.Foreground" Color="White"/>
        <Style x:Key="ButtonStyle1" TargetType="{x:Type Button}">
            <Setter Property="FocusVisualStyle" Value="{StaticResource FocusVisual}"/>
            <Setter Property="Background" Value="{StaticResource Button.Static.Background}"/>
            <Setter Property="BorderBrush" Value="{StaticResource Button.Static.Border}"/>
            <Setter Property="Foreground" Value="{DynamicResource {x:Static SystemColors.ControlTextBrushKey}}"/>
            <Setter Property="BorderThickness" Value="1"/>
            <Setter Property="HorizontalContentAlignment" Value="Center"/>
            <Setter Property="VerticalContentAlignment" Value="Center"/>
            <Setter Property="Padding" Value="1"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="{x:Type Button}">
                        <Border x:Name="border" Background="{TemplateBinding Background}" BorderBrush="{TemplateBinding BorderBrush}" BorderThickness="{TemplateBinding BorderThickness}" SnapsToDevicePixels="true">
                            <ContentPresenter x:Name="contentPresenter" Focusable="False" HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}" Margin="{TemplateBinding Padding}" RecognizesAccessKey="True" SnapsToDevicePixels="{TemplateBinding SnapsToDevicePixels}" VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
                        </Border>
                        <ControlTemplate.Triggers>
                            <Trigger Property="IsDefaulted" Value="true">
                                <Setter Property="BorderBrush" TargetName="border" Value="{DynamicResource {x:Static SystemColors.HighlightBrushKey}}"/>
                            </Trigger>
                            <Trigger Property="IsMouseOver" Value="true">
                                <Setter Property="Background" TargetName="border" Value="{StaticResource Button.MouseOver.Background}"/>
                                <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource Button.MouseOver.Border}"/>
                                <Setter Property="TextElement.Foreground" TargetName="contentPresenter" Value="{StaticResource Button.MouseOver.Foreground}"/>
                                <Setter Property="RenderTransform">
                                    <Setter.Value>
                                        <ScaleTransform ScaleX="1.2" ScaleY="1.2"/>
                                    </Setter.Value>
                                </Setter>
                            </Trigger>
                            <Trigger Property="IsPressed" Value="true">
                                <Setter Property="Background" TargetName="border" Value="{StaticResource Button.Pressed.Background}"/>
                                <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource Button.Pressed.Border}"/>
                            </Trigger>
                            <Trigger Property="IsEnabled" Value="false">
                                <Setter Property="Background" TargetName="border" Value="{StaticResource Button.Disabled.Background}"/>
                                <Setter Property="BorderBrush" TargetName="border" Value="{StaticResource Button.Disabled.Border}"/>
                                <Setter Property="TextElement.Foreground" TargetName="contentPresenter" Value="{StaticResource Button.Disabled.Foreground}"/>
                            </Trigger>
                        </ControlTemplate.Triggers>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
        <Storyboard x:Key="Storyboard1">
            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="rectangle" Storyboard.TargetProperty="(UIElement.Visibility)">
                <DiscreteObjectKeyFrame KeyTime="00:00:00.2000000" Value="{x:Static Visibility.Visible}"/>
            </ObjectAnimationUsingKeyFrames>
            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="image" Storyboard.TargetProperty="(UIElement.Visibility)">
                <DiscreteObjectKeyFrame KeyTime="00:00:00.4000000" Value="{x:Static Visibility.Visible}"/>
            </ObjectAnimationUsingKeyFrames>
            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="image1" Storyboard.TargetProperty="(UIElement.Visibility)">
                <DiscreteObjectKeyFrame KeyTime="00:00:00.5000000" Value="{x:Static Visibility.Visible}"/>
            </ObjectAnimationUsingKeyFrames>
            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="image2" Storyboard.TargetProperty="(UIElement.Visibility)">
                <DiscreteObjectKeyFrame KeyTime="00:00:00.6000000" Value="{x:Static Visibility.Visible}"/>
            </ObjectAnimationUsingKeyFrames>
        </Storyboard>
    </Window.Resources>
    <Window.Triggers>
        <EventTrigger RoutedEvent="FrameworkElement.Loaded">
            <BeginStoryboard Storyboard="{StaticResource Storyboard1}"/>
        </EventTrigger>
    </Window.Triggers>
    <Grid>
        <Rectangle Stroke="Black"  RadiusX="24.5" RadiusY="24.5" Fill="White"/>
        <StackPanel  HorizontalAlignment="Left" Height="550" Width="125" VerticalAlignment="Center" >
            <Grid Height="40">
                <TextBlock Margin="15,0,20,0" TextWrapping="Wrap" Text="Stripe" Height="24" VerticalAlignment="Top" TextAlignment="Center" Foreground="#FF0B4EF1" FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="16"/>
            </Grid>
            <Grid Height="60">
                <StackPanel Orientation="Horizontal">
                    <Image Height="30" Width="35" Source="认证失败.png"/>
                    <Button Style="{DynamicResource ButtonStyle1}" Margin="1,0,20,0" Content="Stripe" Height="30" VerticalAlignment="Center"  FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="14" Width="78" Background="{x:Null}" BorderBrush="{x:Null}" Foreground="#FF555151"/>
                </StackPanel>
            </Grid>
            <Grid Height="60">
                <StackPanel Orientation="Horizontal">
                    <Image Height="30" Width="35" Source="日历.png"/>
                    <Button Margin="1,0,20,0" Content="Stripe" Height="30" VerticalAlignment="Center"  FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="14" Width="78" Background="{x:Null}" BorderBrush="{x:Null}" Style="{DynamicResource ButtonStyle1}" Foreground="#FF555151"/>
                </StackPanel>
            </Grid>
            <Grid Height="60">
                <StackPanel Orientation="Horizontal">
                    <Image Height="30" Width="35" Source="设置.png"/>
                    <Button Margin="1,0,20,0" Content="Stripe" Height="30" VerticalAlignment="Center"  FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="14" Width="78" Background="{x:Null}" BorderBrush="{x:Null}" Style="{DynamicResource ButtonStyle1}" Foreground="#FF555151"/>
                </StackPanel>
            </Grid>
            <Grid Height="60">
                <StackPanel Orientation="Horizontal">
                    <Image Height="30" Width="35" Source="时间.png"/>
                    <Button Margin="1,0,20,0" Content="Stripe" Height="30" VerticalAlignment="Center"  FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="14" Width="78" Background="{x:Null}" BorderBrush="{x:Null}" Style="{DynamicResource ButtonStyle1}" Foreground="#FF555151"/>
                </StackPanel>
            </Grid>
            <Grid Height="60">
                <StackPanel Orientation="Horizontal">
                    <Image Height="30" Width="35" Source="相机.png"/>
                    <Button Margin="1,0,20,0" Content="Stripe" Height="30" VerticalAlignment="Center"  FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="14" Width="78" Background="{x:Null}" BorderBrush="{x:Null}" Style="{DynamicResource ButtonStyle1}" Foreground="#FF555151"/>
                </StackPanel>
            </Grid>
            <Grid Height="60">
                <StackPanel Orientation="Horizontal">
                    <Image Height="30" Width="35" Source="显示.png"/>
                    <Button Margin="1,0,20,0" Content="Stripe" Height="30" VerticalAlignment="Center"  FontFamily="Microsoft JhengHei" FontWeight="Bold" FontSize="14" Width="78" Background="{x:Null}" BorderBrush="{x:Null}" Style="{DynamicResource ButtonStyle1}" Foreground="#FF555151"/>
                </StackPanel>
            </Grid>
        </StackPanel>
        <Rectangle x:Name="rectangle" HorizontalAlignment="Left" Height="295" Margin="215,-30,0,0" Stroke="Black" VerticalAlignment="Top" Width="540" Fill="#FF3938B6" Visibility="Hidden"/>
        <Image x:Name="image" Margin="220,130,575,355" Source="日历.png" Stretch="Fill" Width="100" Height="100" Visibility="Hidden"/>
        <Image x:Name="image1" Margin="355,90,345,310" Source="设置.png" Stretch="Fill" Width="100" Height="100" RenderTransformOrigin="-1.05,0.45" Visibility="Hidden"/>
        <Image x:Name="image2" Margin="600,140,200,360" Source="相机.png" Stretch="Fill" Width="100" Height="100" Visibility="Hidden"/>
    </Grid>
</Window>

```

### 二.使用Blend
#### 1.Blend工具版本
当我们下载Visual Studio时，会自动下载一个同样版本的Blend。
如果界面需要实际到动画的话，这里推荐用2019版的Blend，不要用2022版的Blend，因为2019版的Blend有触发器这一视图，在这个视图中我们可以用来涉及动画，而2022版本的Blend把触发器这一视图给阉割了，如图是2019版本Blend中的触发器视图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/63b9c271a10c4fc5a1d35fab5e87a685.png)














