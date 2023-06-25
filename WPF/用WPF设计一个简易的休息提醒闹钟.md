@[TOC](目录)
`最近利用工作之余，写了一个WPF程序玩玩，用来提醒自己在长时间学习后要休息一会儿哈哈，功能很简单，没啥难点`
## 一.视频演示地址
可以设定间隔提醒时长和休息时长，点击开始之后会开始计时，当计时达到设定的间隔时常后，会进入休息页面会播放音乐，同时也会开始计时，当计时达到休息时长后，会关闭音乐并返回主页。

[video(video-knpakPRT-1681276464604)(type-bilibili)(url-https://player.bilibili.com/player.html?aid=654765414)(image-https://img-blog.csdnimg.cn/img_convert/7d1db73a8233b1b86c52ffdabb66d6d8.jpeg)(title-用WPF设计一个简易的休息提醒闹钟)]


## 二.代码展示
思路很简单，直接展示一些核心代码吧：

1. 项目截图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/91ca1a2752fa480ab1984398c90af9f2.png)

2. App.xaml.cs代码，用于配置Prism框架一些服务以及初始化工作：

```csharp
using AlarmWPF.ViewModels;
using AlarmWPF.Views;
using Prism.Ioc;
using System.Windows;

namespace AlarmWPF
{
	/// <summary>
	/// Interaction logic for App.xaml
	/// </summary>
	public partial class App
	{
		protected override Window CreateShell()
		{
			return Container.Resolve<MainWindow>();
		}

		protected override void RegisterTypes(IContainerRegistry containerRegistry)
		{
			containerRegistry.RegisterDialog<RestView,RestViewModel>();
		}
	}
}

```
3. MainWindow.xaml代码

```xml
<Window x:Class="AlarmWPF.Views.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:prism="http://prismlibrary.com/"
        xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
        prism:ViewModelLocator.AutoWireViewModel="True"
        xmlns:hc="https://handyorg.github.io/handycontrol" ResizeMode="NoResize"
        Title="{Binding Title}" Height="350" Width="525" WindowStartupLocation="CenterScreen" Icon="/Images/alarmIcon.png" Unloaded="Window_Unloaded">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <StackPanel Grid.Row="0" Orientation="Horizontal">
            <TextBlock Style="{StaticResource MaterialDesignHeadline6TextBlock}"  VerticalAlignment="Center" Margin="16 0 0 0" Text="间隔多少分钟提醒：" FontSize="20" FontWeight="Bold"/>
            <hc:NumericUpDown Value="{Binding InternalMinutes}" HorizontalAlignment="Center"  VerticalAlignment="Center" Style="{StaticResource NumericUpDownBaseStyle}" Width="150" FontSize="20" FontWeight="Bold"/>
        </StackPanel>
        <StackPanel Grid.Row="1" Orientation="Horizontal">
            <TextBlock Style="{StaticResource MaterialDesignHeadline6TextBlock}"  VerticalAlignment="Center" Margin="16 0 0 0" Text="休息多少分钟：" FontSize="20" FontWeight="Bold"/>
            <hc:NumericUpDown Value="{Binding RestMinutes}" HorizontalAlignment="Center"  VerticalAlignment="Center" Style="{StaticResource NumericUpDownBaseStyle}" Width="150" FontSize="20" FontWeight="Bold"/>
        </StackPanel>
        <Grid Grid.Row="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <Button Command="{Binding BtnStart}" CommandParameter="{Binding RelativeSource={RelativeSource FindAncestor, AncestorType={x:Type Window}}}"  IsEnabled="{Binding BtnStartEnable}" Grid.Column="0" Style="{StaticResource MaterialDesignFlatDarkBgButton}" Content="开始" HorizontalAlignment="Center" VerticalAlignment="Center"/>
            <Button Command="{Binding BtnStop}" CommandParameter="{Binding RelativeSource={RelativeSource FindAncestor, AncestorType={x:Type Window}}}" IsEnabled="{Binding BtnStopEnable}"  Grid.Column="1" Style="{StaticResource MaterialDesignFlatDarkBgButton}" Content="停止" HorizontalAlignment="Center" VerticalAlignment="Center"/>
        </Grid>
    </Grid>
</Window>


```
4. MainWindowViewModel.cs代码：

```csharp
using AlarmWPF.Views;
using HandyControl.Tools.Extension;
using Microsoft.Win32;
using Prism.Commands;
using Prism.Mvvm;
using Prism.Services.Dialogs;
using System;
using System.Diagnostics;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using System.Timers;
using System.Windows;
using System.Windows.Controls;
using Timer = System.Timers.Timer;

namespace AlarmWPF.ViewModels
{
	public class MainWindowViewModel : BindableBase
	{
		#region 属性
		private string _title = "自律闹钟";
		public string Title
		{
			get { return _title; }
			set { SetProperty(ref _title, value); }
		}

		private int internalMinutes;
		public int InternalMinutes
		{
			get { return internalMinutes; }
			set { SetProperty(ref internalMinutes, value); }
		}

		private int restMinutes;
		public int RestMinutes
		{
			get { return restMinutes; }
			set { SetProperty(ref restMinutes, value); }
		}


		private bool btnStartEnable=true;
		public bool BtnStartEnable
		{
			get { return btnStartEnable; }
			set { SetProperty(ref btnStartEnable, value); }
		}

		private bool btnStopEnable=true;
		public bool BtnStopEnable
		{
			get { return btnStopEnable; }
			set { SetProperty(ref btnStopEnable, value); }
		}

		public DelegateCommand<object> BtnStart { get; set; }
		public DelegateCommand<object> BtnStop { get; set; }
		private int endSeconds = 0;
		Timer timer;
		private IDialogService dialog;
		private Window window1;

		#endregion


		public MainWindowViewModel(IDialogService dialog)
		{
			window1 = new Window();
			this.dialog = dialog;
			BtnStart = new DelegateCommand<object>(Start);
			BtnStop = new DelegateCommand<object>(Stop);
			timer = new Timer(1000);
			timer.Enabled = false;
			timer.Elapsed += Timer_Elapsed;
		}

		private void Stop(object obj)
		{
		
			BtnStartEnable = true;
			BtnStopEnable = false;

			timer.Stop();
			timer.Enabled = false;
		}

		private void Start(object obj)
		{
			endSeconds = InternalMinutes * 60;
			BtnStartEnable = false;
			BtnStopEnable = true;


			timer.Enabled = true;
			timer.Start();
			window1 = obj as Window;
			window1.WindowStartupLocation = WindowStartupLocation.CenterScreen;
		}


		private void Timer_Elapsed(object sender, ElapsedEventArgs e)
		{
			endSeconds--;
			if (endSeconds==0)
			{
				timer.Stop();
				timer.Enabled=false;

				///parameters，添加向对话框传递的参数
				IDialogParameters parameters = new DialogParameters();
				parameters.Add("InternalMinutes", $"{InternalMinutes}");
				parameters.Add("RestMinutes", $"{RestMinutes}");

				Application.Current.Dispatcher.InvokeAsync(new Action(() =>
				{
					window1.Hide();
					dialog.ShowDialog("RestView",parameters,callback);
				}));
			}
		}

		/// <summary>
		/// 对话框关闭的时候调用此回调方法
		/// </summary>
		/// <param name="obj"></param>
		private void callback(IDialogResult obj)
		{
			window1.WindowStartupLocation = WindowStartupLocation.CenterScreen;
			window1.Show();
			BtnStartEnable = true;
			BtnStopEnable = false;
		}
	}
}

```
5. RestView.xaml

```xml
<UserControl x:Class="AlarmWPF.Views.RestView"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:AlarmWPF.Views"
             xmlns:prism="http://prismlibrary.com/"
             xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
             prism:ViewModelLocator.AutoWireViewModel="True"
             xmlns:hc="https://handyorg.github.io/handycontrol"
             mc:Ignorable="d" 
             Background="White"
             d:DesignHeight="600" d:DesignWidth="800" >
    <prism:Dialog.WindowStyle>
        <Style TargetType="Window">
            <Setter Property="prism:Dialog.WindowStartupLocation" Value="CenterScreen" />
            <Setter Property="ShowInTaskbar" Value="False"/>
            <Setter Property="WindowStyle" Value="None"/>
            <Setter Property="AllowsTransparency" Value="True"/>
        </Style>
    </prism:Dialog.WindowStyle>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="5*"/>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Border Grid.Row="0">
            <Image Source="/Images/rest.jpg" Stretch="Fill" />
        </Border>
        <StackPanel Grid.Row="1">
            <TextBlock Text="{Binding RestText}" FontSize="30" VerticalAlignment="Center" HorizontalAlignment="Center" FontWeight="Bold"/>
        </StackPanel>
        <StackPanel  Grid.Row="2">
            <TextBlock Text="{Binding EndTimeText}" FontSize="15" VerticalAlignment="Center" HorizontalAlignment="Center" FontWeight="Bold"/>
        </StackPanel>
    </Grid>
</UserControl>

```
6. RestViewModel.cs代码：

```csharp
using HandyControl.Tools.Extension;
using Prism.Commands;
using Prism.Mvvm;
using Prism.Services.Dialogs;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Media;
using System.Numerics;
using System.Text;
using System.Threading.Tasks;
using System.Timers;
using System.Windows;

namespace AlarmWPF.ViewModels
{
	public class RestViewModel: BindableBase,IDialogAware
	{
		#region 属性

		public DelegateCommand CloseDialog { get;set; }
		public event Action<IDialogResult> RequestClose;

		private string restText="";
		public string RestText
		{
			get { return restText; }
			set { SetProperty(ref restText, value); }
		}
		private string endTimeText = "";
		public string EndTimeText
		{
			get { return endTimeText; }
			set { SetProperty(ref endTimeText, value); }
		}

		private int endTimeSeconds = 0;
		private  int totalTime=0;

		public string Title => "该休息了";
		private Timer timer;
		SoundPlayer player;

		#endregion

		public RestViewModel()
        {
			player=new SoundPlayer();
			timer = new Timer();
			timer.Enabled= false;
			timer.Interval = 1000;
			timer.Elapsed += Timer_Elapsed;
		}


		private void Timer_Elapsed(object sender, ElapsedEventArgs e)
		{
			if (endTimeSeconds==0)
			{
				timer.Enabled = false;
				timer.Stop();
				player.Stop();
				player.Dispose();
				Application.Current.Dispatcher.InvokeAsync(() =>
				{
					RequestClose?.Invoke(new DialogResult(ButtonResult.OK));
				});
			}
			endTimeSeconds--;
			EndTimeText = $"{endTimeSeconds}秒后自动关闭";
		}

		public bool CanCloseDialog()
		{
			//允许关闭对话框
			return true;
		}

		public void OnDialogClosed()
		{
			//当对话框关闭的时候
			timer.Enabled = false;
			timer.Stop();
			player.Stop();
			player.Dispose();
		}

		public void OnDialogOpened(IDialogParameters parameters)
		{
			timer.Enabled = true;
			timer.Start();
			var internalMinutes = parameters.GetValue<string>("InternalMinutes");
			var restMinutes = parameters.GetValue<string>("RestMinutes");
			try
			{
				totalTime = int.Parse(restMinutes);
				endTimeSeconds = int.Parse(restMinutes)*60;
			}catch (Exception ex)
			{

			}
			RestText = $"亲,您又工作了{internalMinutes}分钟,该休息一下了！";
			EndTimeText = $"{endTimeSeconds}秒后自动关闭";

			string wavFile = @"music.wav";
			if (File.Exists(wavFile))
			{
				player.SoundLocation= wavFile;
				player.Load();
				player.Play();
				player.Dispose();
			}

		}
	}
}

```
## 三.源代码：
[仓库链接](https://gitee.com/hezexi/alarm-wpf/tree/develop/)
