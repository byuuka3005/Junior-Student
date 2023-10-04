# Basic UI Controls

## WPF và XAML
### WPF - Windows Presentation Foundation
- Là một framework để xây dựng các ứng dụng desktop trên Windows

<img src="https://www.win2wpf.com/Images/wpfArchitecture.PNG">

### XAML - Extensible Application Markup Language
- Là một ngôn ngữ để xây dựng giao diện người dùng (UI) cho các ứng dụng WPF
- XAML là một ngôn ngữ khai báo (declarative language) dựa trên XML
- XAML được sử dụng để tạo các UI cho các ứng dụng WPF, Silverlight, UWP, Xamarin, ...

## Window, MessageBox, Dialog

### Window, MessageBox

#### Icon 
Nhấn chuột phải vào Project &rarr; Application &rarr; Icon &rarr; Chọn file 
- [Trang icon](https://www.flaticon.com/) mà thầy dùng 
- Để thay đổi icon của ứng dụng, ta thay đổi thuộc tính `Icon` của `Window` trong file `MainWindow.xaml`
```xml
<Window x:Class="WPFAppDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        Title="MainWindow" Height="450" Width="800" Icon="Images/icon.ico">
    <Grid>
    </Grid>
</Window>
```
#### Startup Location
- Để hiển thị cửa sổ tại giữa màn hình, ta thay đổi thuộc tính `WindowStartupLocation` của `Window` trong file `MainWindow.xaml`
```xml
<Window x:Class="WPFAppDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
        Title="MainWindow" Height="450" Width="800" Icon="Images/icon.ico" 
        WindowStartupLocation="CenterScreen">
    <Grid>
    </Grid>
</Window>
```

- Để cửa sổ con hiển thị trên cửa sổ cha, ta thay đổi thuộc tính `Owner` của `Window` trong file `MainWindow.xaml.cs`
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        Window1 w1 = new Window1();
        w1.Owner = this;
        w1.Show();
    }
}
```



## Button

## Label / TextBlock

## TextBox
- Binding dữ liệu giữa `TextBox` và `Label` trong file `MainWindow.xaml`
```xml

        <TextBox
            x:Name="txtUsername"
            Height="25"
            Width="200" Canvas.Left="300" Canvas.Top="192"
            FontSize="16"
            />
        <PasswordBox
            x:Name="txtPassword"
            Height="25"
            Width="200" Canvas.Left="300" Canvas.Top="277" 
            HorizontalAlignment="Center" 
            VerticalAlignment="Top"
            FontSize="16"
            />

 
        <Label
            x:Name="lblUsername"
            Content="_Username" Canvas.Left="300" Canvas.Top="161"
            Target="{Binding ElementName=txtUsername}"
            
            />
        <Label
            x:Name="lblPassword"
            Content="_Password" Canvas.Left="300" Canvas.Top="246"
            />
        <!--
            Lưu ý không Binding với PasswordBox
        -->


```


## Image

## CheckBox / RadioButton

### CheckBox


### RadioButton
- Để các RadioButton được gom nhóm, đặt chúng trong một `StackPanel` và thiết lập thuộc tính `GroupName` của các `RadioButton` trong `StackPanel` đó

## ProgressBar / Slider

### ProgressBar
- Để ProgressBar tự động tăng giá trị, ta sử dụng `Timer` để tăng giá trị của ProgressBar
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        DispatcherTimer timer = new DispatcherTimer();
        timer.Interval = TimeSpan.FromSeconds(1);
        timer.Tick += Timer_Tick;
        timer.Start();
    }

    private void Timer_Tick(object sender, EventArgs e)
    {
        if (progressBar.Value < progressBar.Maximum)
        {
            progressBar.Value += 10;
        }
        else
        {
            progressBar.Value = progressBar.Minimum;
        }
    }
}
```

- Để thể hiện giá trị của ProgressBar dưới dạng phần trăm, ta sử dụng thuộc tính `IsIndeterminate` và `Value` của ProgressBar
```xml
<ProgressBar
    x:Name="progressBar"
    Height="25"
    Width="200" Canvas.Left="300" Canvas.Top="192"
    FontSize="16"
    IsIndeterminate="False"
    Value="0"
    />
```

- Để progressbar hiển thị loading animation, ta sử dụng thuộc tính `IsIndeterminate` của ProgressBar
```xml
<ProgressBar
    x:Name="progressBar"
    Height="25"
    Width="200" Canvas.Left="300" Canvas.Top="192"
    FontSize="16"
    IsIndeterminate="True"
    />
```


### Slider
- Để Slider tự động tăng giá trị, ta sử dụng `Timer` để tăng giá trị của Slider
```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        DispatcherTimer timer = new DispatcherTimer();
        timer.Interval = TimeSpan.FromSeconds(1);
        timer.Tick += Timer_Tick;
        timer.Start();
    }

    private void Timer_Tick(object sender, EventArgs e)
    {
        if (slider.Value < slider.Maximum)
        {
            slider.Value += 10;
        }
        else
        {
            slider.Value = slider.Minimum;
        }
    }
}
```

# Debug Artifacts

- Để đẩy thông tin Debug sang cửa sổ Immediate, ta sử dụng hàm `Debug.WriteLine()`, trước đó, vào `Debug` &rarr; `Options...` &rarr; `Debugging` &rarr; `General` &rarr; Tích chọn `Redirect all Output Window text to the Immediate Window` để đẩy thông tin Debug sang cửa sổ Immediate 