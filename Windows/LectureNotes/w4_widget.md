# Widget trong XAML

## ComboBox

```xml
<ComboBox
    x:Name="cboColors"
    Height="25"
    Width="200" Canvas.Left="300" Canvas.Top="362"
    FontSize="16"
    >
</ComboBox>
```

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        cboColors.Items.Add("Red");
        cboColors.Items.Add("Green");
        cboColors.Items.Add("Blue");
        cboColors.Items.Add("Yellow");
        cboColors.Items.Add("Pink");
        cboColors.Items.Add("Orange");
        cboColors.Items.Add("Purple");
        cboColors.Items.Add("Brown");
        cboColors.Items.Add("Black");
        cboColors.Items.Add("White");
    }
}
```

Hoặc thêm từ một List

```csharp
public partial class MainWindow : Window
{
    class Color
    {
        public string Name { get; set; }
    }
    public MainWindow()
    {
        InitializeComponent();
        List<Color> colors = new List<Color>();
        colors.Add(new Color() { Name = "Red" });
        colors.Add(new Color() { Name = "Green" });
        colors.Add(new Color() { Name = "Blue" });
        colors.Add(new Color() { Name = "Yellow" });
        colors.Add(new Color() { Name = "Pink" });
        colors.Add(new Color() { Name = "Orange" });
        colors.Add(new Color() { Name = "Purple" });
        colors.Add(new Color() { Name = "Brown" });
        colors.Add(new Color() { Name = "Black" });
        colors.Add(new Color() { Name = "White" });

        cboColors.ItemsSource = colors;
    }
}
```

- Tuy nhiên, tên màu sắc vẫn chưa được hiển thị khí chạy, để màu sắc hiển thị, ta cần định nghĩa lại hàm toString() của class Color

```csharp

    class Color
    {
        public string Name { get; set; }
        public override string ToString()
        {
            return Name;
        }
    }
```

Hoặc định nghĩa ItemTemplate trong XAML
    
```xml
<ComboBox
    x:Name="cboColors"
    Height="25"
    Width="200" Canvas.Left="300" Canvas.Top="362"
    FontSize="16"
    >
    <ComboBox.ItemTemplate>
        <DataTemplate>
            <TextBlock Text="{Binding Name}"/>
        </DataTemplate>
    </ComboBox.ItemTemplate>
</ComboBox>
```

- Để hiển thị hình chất lượng cao (zoom không bị bể), thêm RenderOptions.BitmapScalingMode="HighQuality" vào Image

```xml
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <Image
                Height="16"
                Width="16"
                RenderOptions.BitmapScalingMode="HighQuality"
                Source="{Binding Image}"
                />3
            <TextBlock Text=" {Binding Name}"/>
        </StackPanel>
    </DataTemplate>
```

### Thêm một phần tử vào list của ComboBox

- Thay List bằng BindingList, khi thêm phần tử thì ComboBox sẽ tự động cập nhật

```csharp   
    //List<Color> colors = new List<Color>();
    BindingList<Color> colors = new BindingList<Color>();
    // ...
    cboColors.ItemsSource = colors;
```

```csharp
    private void btnAdd_Click(object sender, RoutedEventArgs e)
    {
        Color color = new Color() { Name = txtColor.Text };
        colors.Add(color);
    }
```

### Xoá một phần tử khỏi list của ComboBox

```cs
    private void btnRemove_Click(object sender, RoutedEventArgs e)
    {
        Color color = cboColors.SelectedItem as Color;
        if (color != null)
        {
            colors.Remove(color);
        }
    }
```

- hoặc xoá dựa trên phần tử index

```cs
    private void btnRemove_Click(object sender, RoutedEventArgs e)
    {
        int index = cboColors.SelectedIndex;
        if (index >= 0)
        {
            colors.RemoveAt(index);
        }
    }
```

### Chỉnh sửa một phần tử trong list của ComboBox

```cs
    private void btnEdit_Click(object sender, RoutedEventArgs e)
    {
        Color color = cboColors.SelectedItem as Color;
        if (color != null)
        {
            color.Name = "Bck";
            colors.ResetBindings();
        }
    }
```
hoặc dựa trên index
```cs
    private void btnEdit_Click(object sender, RoutedEventArgs e)
    {
        int index = cboColors.SelectedIndex;
        if (index >= 0)
        {
            colors[index].Name = "Bck";
            colors.ResetBindings();
        }
    }
```

## ListView

```xml
<ListView
    x:Name="lvColors"
    Height="200"
    Width="200" Canvas.Left="300" Canvas.Top="192"
    >
    <ListView.Itemtemplate>
        <DataTemplate>
            <StackPanel Orientation="Vertical">
                <Image
                    Height="16"
                    Width="16"
                    RenderOptions.BitmapScalingMode="HighQuality"
                    Source="{Binding Image}"
                    />
                <TextBlock Text=" {Binding Name}"/>
            </StackPanel>
        </DataTemplate>
    </ListView.Itemtemplate>
</ListView>
```

```csharp
public partial class MainWindow : Window
{
    class Color
    {
        public string Name { get; set; }
        public string Image { get; set; }
    }
        BindingList<Color> _colors;
    public MainWindow()
    {
        InitializeComponent();
        _colors = new BindingList<Color>();
        colors.Add(new Color() { Name = "Red", Image = "Images/Red.png" });
        colors.Add(new Color() { Name = "Green", Image = "Images/Green.png" });
        colors.Add(new Color() { Name = "Blue", Image = "Images/Blue.png" });
        colors.Add(new Color() { Name = "Yellow", Image = "Images/Yellow.png" });
        colors.Add(new Color() { Name = "Pink", Image = "Images/Pink.png" });


        lvColors.ItemsSource = colors;
    }
}
```

- Để ListView hiển thị xuống dòng thay vì hiển thị trên một dòng, ta thêm thuộc tính `ScrollViewer.HorizontalScrollBarVisibility="Disabled"` vào ListView
và dùng StackPanel thay cho StackPanel


```xml
<ListView
    x:Name="lvColors"
    Height="200"
    Width="200" Canvas.Left="300" Canvas.Top="192"
    ScrollViewer.HorizontalScrollBarVisibility="Disabled"
    >
    <!-- dùng warp panel-->
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapPanel/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <!-- ... -->

</ListView>
```

### Handle ContextMenu

```xml
<ListView
    x:Name="lvColors"
    Height="200"
    Width="200" Canvas.Left="300" Canvas.Top="192"
    ScrollViewer.HorizontalScrollBarVisibility="Disabled"
    >
    <!-- dùng warp panel-->
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapPanel/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <!-- ... -->
    <ListView.ContextMenu>
        <ContextMenu>
            <MenuItem Header="Add" Click="MenuItem_Click"/>
            <MenuItem Header="Edit" Click="MenuItem_Click"/>
            <MenuItem Header="Remove" Click="MenuItem_Click"/>
        </ContextMenu>
    </ListView.ContextMenu>
</ListView>
```
```cs
    private void MenuItem_Click(object sender, RoutedEventArgs e)
    {
        // MenuItem có thể là Student, Color, hoặc bất cứ kiểu nào mà ListView đang hiển thị
        Color menuItem = lvColors.SelectedItem as Color;
        if (menuItem != null)
        {
            switch (menuItem.Header.ToString())
            {
                case "Edit":
                    // some edit code or function
                    break;
                case "Remove":
                    _colors.Remove(menuItem);
                    break;
            }
        }
    }
```
- ItemContainerStyle

```xml
<ListView
    x:Name="lvColors"
    Height="200"
    Width="200" Canvas.Left="300" Canvas.Top="192"
    ScrollViewer.HorizontalScrollBarVisibility="Disabled"
    >
    <!-- dùng warp panel-->
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapPanel/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <!-- ... -->
    <ListView.ContextMenu>
        <ContextMenu>
            <ContextMenu.ItemContainerStyle>
                <Style TargetType="x:Type ListViewItem">
                    <Setter Property="ContextMenu"
                            Value="{StaticResource ResourceKey=ColorContextMenu}">                       
                    </Setter>
                </Style>
            </ContextMenu.ItemContainerStyle>
        </ContextMenu>
    </ListView.ContextMenu>
</ListView>



- Có thể cho mỗi lựa chọn trong context menu một hàm khác nhau, như `Edit` sẽ gọi hàm `EditColor` còn `Remove` sẽ gọi hàm `RemoveColor`
