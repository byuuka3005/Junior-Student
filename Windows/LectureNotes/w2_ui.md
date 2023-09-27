# Giao giện (tiếp theo)

## Lưu ảnh

- Thay vì nhúng ảnh vào app như tuần trước
- Lần này sẽ lưu ảnh vào thư mục `Images` nhưng sẽ thay đổi trong `BuildAction` thành `None` và chuyển `DoNotCopy` thành `CopyIfNewer`
- Trong code, sẽ lấy đường dẫn tương đối của ảnh để hiển thị bằng cách đổi Urikind từ `Relative` thành `Absolute`

```cs
    string baseDirectory = AppDomain.CurrentDomain.BaseDirectory;
    var uri = new Uri($"{baseDirectory}Images/{imageFileName}", UriKind.Absolute);
    BitmapImage bitmap = new BitmapImage(uri);
    image.Source = bitmap;
```

## Tạo màn hình quiz
- Tạo màn hình mới `QuizWindow.xaml` bằng cách nhấn chuột phải vào thư mục `Windows` và chọn `Add -> Window (WPF)`
- Trong màn hình chính gọi màn hình mới bằng cách

```cs
    var quizWindow = new QuizWindow();
    quizWindow.Show();
```

## Bài tập về nhà
- Cái tiến chương trình học từ vứng: 
    - Thêm chế độ quiz bằng cách tạo thêm màn hình quiz và thêm nút vào màn hình chính
    - Hình thức quiz là trắc nghiệm 1 hình 2 từ hoặc 1 từ 2 hình, chọn phương án đúng