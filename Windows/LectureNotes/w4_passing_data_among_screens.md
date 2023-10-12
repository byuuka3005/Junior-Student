# Truyền dữ liệu giữa các màn hình 
(Đọc [w4_widget](w4_widget.md) trước)

## Public Static Property

### Truyền bằng hàm tạo Constructor

- Tạo một class mới có tên là `Color`, kế thừa Interface `ICloneable` để thực hiện Deep Copy

```csharp
// Lưu ý chuyển class Color sang public
public class Color : INotifyPropertyChanged, ICloneable
{
    public string Name { get; set; }
    public event PropertyChangedEventHandler PropertyChanged;
    public object Clone()
    {
        return this.MemberwiseClone();
    }
}

```
- Tạo một `ListView` hiển thị các `Color` trong file `MainWindow.xaml`


- Trong `EditWindow` chứa một nút `Save` để lưu lại dữ liệu và đóng cửa sổ và một nút `Cancel` để đóng cửa sổ mà không lưu lại dữ liệu. Thêm một TextBox để chỉnh sửa tên của `Color`
```xml
<StackPanel>
    <TextBox Text="{Binding Name}"/>
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Click="Save_Click"/>
        <Button Content="Cancel" Click="Cancel_Click"/>
    </StackPanel>
</StackPanel>
```

- Khi bấm chuột phải chọn `Edit` trong context menu, Bên phía gửi `MainWindow` (hàm handle thao tác `Edit`) gọi hàm tạo của `EditWindow` và truyền dữ liệu vào

```csharp
private void Edit_Click(object sender, RoutedEventArgs e)
{
    Color color = (Color)lvColors.SelectedItem;
    EditWindow editWindow = new EditWindow(color);
    if(editWindow.ShowDialog().Value == true)
    {
        editWindow.ReturnedColor.CopyPropertiesTo(color);
        // Xem định nghĩa của hàm CopyPropertiesTo ở phần sau
    }
}
```


- Bên phía nhận `EditWindow` nhận dữ liệu trong hàm tạo

```csharp
public partial class EditWindow : Window
{
    public Color ReturnedColor { get; set; }
    
    public EditWindow(Color color)
    {
        InitializeComponent();
        editingColor = color.Clone() as Color;
        this.DataContext = editingColor;
    }
}
```
- Bên phía nhận `EditWindow` trả dữ liệu
về cho `MainWindow` thông qua một thuộc tính `ReturnedColor`

```csharp
public partial class EditWindow : Window
{
    public Color ReturnedColor { get; set; }
    
    public EditWindow(Color color)
    {
        InitializeComponent();
        editingColor = (Color)color.Clone();
        this.DataContext = editingColor;
    }
    
    private void Save_Click(object sender, RoutedEventArgs e)
    {
        this.DialogResult = true;
        Close();
    }
    
    private void Cancel_Click(object sender, RoutedEventArgs e)
    {
        this.DialogResult = false;
        Close();
    }
}
```

## Delegate và Event
### Delegate - Con trỏ hàm
- Ví dụ
```csharp
    delegate int Calculate(int a, int b);

    static int Sum(int a, int b)
    {
        return a + b;
    }
    // Multiply
    static int Multiply(int a, int b)
    {
        return a * b;
    }

    static void Main(string[] args)
    {
        Calculate calculate = Sum;
        Console.WriteLine(calculate(1, 2));

        calculate = Multiply;
        Console.WriteLine(calculate(1, 2));
    }
```

### Event - Sự kiện
- Ví dụ
```csharp
    delegate void ClickFunctionType(Object sender, EventArgs e)
    static event ClickFunctionType OnClick;

    static void Main(string[] args)
    {
        OnClick += btnClick;
    }

    private void btnClick(Object sender, EventArgs e)
    {
        // do some thing
    }

```

### Deep copy
- Tạo một class Reflection để thực hiện Deep Copy mà không tạo thêm clone của đối tượng
```cs

    public static class Reflection {
        public static void CopyProperties(this object source, object target) {
            Type sourceType = source.GetType();
            Type targetType = target.GetType();

            var sourceProps = source.GetType().GetProperties();

            foreach (var sourceProp in sourceProps) {
                if (!sourceProp.CanRead) continue;
                var targetProp = targetType.GetProperty(sourceProp.Name);
                if (targetProp == null) continue;
                if (!targetProp.CanWrite) continue;
                if ((targetProp.GetSetMethod(true) != null)
                    && (targetProp.GetSetMethod(true).IsPrivate)) {
                    continue;
                }

                if ((targetProp.GetSetMethod()?.Attributes
                    & System.Reflection.MethodAttributes.Static) != 0) 
                    continue;
                if (!targetProp.PropertyType.IsAssignableFrom(sourceProp.PropertyType)) 
                    continue;

                // Pass all tests, let's assign
                targetProp.SetValue(target, sourceProp.GetValue(source, null), null);
            }
        }
    }
```

### Truyền dữ liệu bằng Delegate và Event
- Tạo một thuộc tính `OnColorChanged` kiểu `EventHandler` trong class `EditWindow`
- Giả sử cần edit Opacity của `Color` thì thêm một `Slider` vào `EditWindow`
```xml
<StackPanel>
    <TextBox Text="{Binding Name}"/>
    <Slider Value="{Binding Opacity}" Minimum="0" Maximum="1" TickFrequency="0.1"/>
    <StackPanel Orientation="Horizontal">
        <Button Content="Save" Click="Save_Click"/>
        <Button Content="Cancel" Click="Cancel_Click"/>
    </StackPanel>
</StackPanel>
```

```csharp
public partial class EditWindow : Window
{
    public event EventHandler OnColorChanged;
    //...

    // gán sự kiện của slider vào hàm handle trong constructor
    public EditWindow(Color color)
    {
        InitializeComponent();
        editingColor = (Color)color.Clone();
        this.DataContext = editingColor;
    }


    private void Save_Click(object sender, RoutedEventArgs e)
    {
        ReturnedColor = editingColor;
        OnColorChanged?.Invoke(this, EventArgs.Empty);
        this.DialogResult = true;
    }

    private void Cancel_Click(object sender, RoutedEventArgs e)
    {
        this.DialogResult = false;
    }

}
```
- Bên truyền dữ liệu `MainWindow` sẽ gán hàm handle cho sự kiện `OnColorChanged` trong `EditWindow`
```csharp
private void Edit_Click(object sender, RoutedEventArgs e)
{
    Color color = (Color)lvColors.SelectedItem;
    EditWindow EditWindow = new EditWindow(color);
    // bắt sự kiện của silider và chuyển nó cho EditWindow
    EditWindow.OnColorChanged += (int sender, EventArgs e) => {
        color.Opacity = EditWindow.Opacity;
    };
    if(EditWindow.ShowDialog().Value== true)
    {
        color = EditWindow.Color;
    }
}
```

# Bài tập về nhà
- Do data binding for **a list** of books: Book’s name, Cover’s image, Author, Published Year
- Do data binding for **a list** of mobile phones: Phone’s name, Image, Manufacturer, Price
- Do data binding for **a list** of employees: Fullname, Email, Address, Telephone number, Avatar’s image

- Sử dụng ComboBox hoặc ListView
<br> Prepare at least 10 items, Add, Delete, Update (hard code data)