# Giao diện (tiếp theo)

## Lưu ảnh

- Thay vì nhúng ảnh vào app như tuần trước, tuần này sẽ lưu ảnh vào thư mục `Images` nhưng sẽ thay đổi trong `BuildAction` thành `None` và chuyển `DoNotCopy` thành `CopyIfNewer`
- Trong code, sẽ lấy đường dẫn tương đối của ảnh để hiển thị bằng cách đổi Urikind từ `Relative` thành `Absolute`

```cs
    string baseDirectory = AppDomain.CurrentDomain.BaseDirectory;
    var uri = new Uri($"{baseDirectory}Images/{imageFileName}", UriKind.Absolute);
    BitmapImage bitmap = new BitmapImage(uri);
    image.Source = bitmap;
```
- Mục đích là để giảm dung lượng của app khi build

- Để hiên thị ảnh khi khởi tạo cửa sổ (window) thì thêm vào trong file `xaml` thuộc tính `Loaded` của thẻ `Window` và gọi hàm `Window_Loaded` trong file `cs`

```xml
    <Window x:Class="WpfApp1.MainWindow"
        ...
        Loaded="Window_Loaded">
```

- Trong file `cs` thêm hàm `Window_Loaded` và code hiển thị ảnh

```cs
    private void Window_Loaded(object sender, RoutedEventArgs e)
    {
        //Code hiển thị ảnh
    }
```

- Phần code hiển thị ảnh có thể được Extract thành hàm riêng, để Extract trong Ms Visual Studio, bôi đen phần code muốn Extract, sau đó chọn 1 trong các cách sau:
    1. Nhấn tổ hợp phím `Ctrl + R, Ctrl + M` 
    2. Chuột phải vào đoạn code cần Extract và chọn `Quick Actions and Refactorings` &rarr; `Extract Method`
    3. Chọn tab `Edit` &rarr; `Refactor` &rarr; `Extract Method`
    4. Nhấn tổ hợp phím `Ctrl + .` và chọn `Extract Method` (hoặc nhấn `Enter`)

## Tạo màn hình mới
- Tạo màn hình mới `NewWindow.xaml` bằng cách nhấn chuột phải vào project và chọn `Add` &rarr; `Window (WPF)`
- Trong màn hình chính gọi màn hình mới bằng cách

```cs
    var newWindow = new NewWindow();
    newWindow.Show();

    //Đóng màn hình hiện tại
    this.Close();
```

## Bài tập về nhà
- Cái tiến chương trình học từ vứng: 
    - Thêm chế độ quiz bằng cách tạo thêm màn hình quiz và thêm nút vào màn hình chính để chuyển sang màn hình quiz 
    - Hình thức quiz là trắc nghiệm 1 hình 2 từ hoặc 1 từ 2 hình, chọn phương án đúng 
    - Màn hình hiển thị số câu đúng trên cùng theo format `[số câu đúng]/[tổng số câu]`
