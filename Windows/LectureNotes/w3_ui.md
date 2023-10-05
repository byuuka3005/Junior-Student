# Converter

## Tạo một Converter trong WPF
- Trong project, chọn New &rarr; Class... &rarr;
- Kế thừa interface `IValueConverter`, đặt tên là `RelativePathToAbsolutePathConverter`
- Override 2 hàm `Convert` và `ConvertBack`
- Ví dụ:
```csharp
    class RelativePathToAbsolutePathConverter : IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            string relativePath = value as string;
            if (relativePath == null)
                return null;
            string absolutePath = Path.GetFullPath(relativePath);
            return absolutePath;
        }

        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            throw new NotImplementedException();
        }
    }
```

## Sử dụng Converter trong WPF
- Để sử dụng Converter, ta phải khai báo Converter trong `Window` hoặc `Application` bằng cách thêm dòng sau vào `Window` hoặc `Application`:
```xml
    <Window.Resources>
        <local:RelativePathToAbsolutePathConverter x:Key="RelativePathToAbsolutePathConverter" />
    </Window.Resources>
```

- Để sử dụng Converter, ta sử dụng thuộc tính `Converter` của các `Binding` trong `XAML`, ví dụ:
```xml
    <Image Source="{Binding ImagePath, Converter={StaticResource RelativePathToAbsolutePathConverter}}" />
```

