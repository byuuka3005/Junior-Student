# Artifact

## Partial class trong C#

- Partial class là một class được chia thành nhiều file khác nhau, nhưng khi biên dịch thì nó sẽ được biên dịch thành một class duy nhất.

- Cú pháp:

```cs
partial class ClassName
{
    // code
}
```

Ví dụ:
```cs
// File 1
partial class Student
{
    public string Name { get; set; }
}

// File 2
partial class Student
{
    public int Age { get; set; }
}
```
## Kĩ thuật using

- Kĩ thuật using được sử dụng để giải phóng tài nguyên, ví dụ như giải phóng bộ nhớ, giải phóng tài nguyên mạng, giải phóng tài nguyên file, ...

- Cú pháp:

```cs
using (var resource = new Resource())
{
    // code
}
```
- Ví dụ khi mớ một reader để đọc dữ liệu từ database, ta sẽ sử dụng kĩ thuật using để đảm bảo rằng khi đọc xong, ta sẽ giải phóng tài nguyên đó.

```cs
using (var reader = command.ExecuteReader())
{
    // code
}
```

## Đóng gói và triển khai ứng dụng

1. Cài đặt Extension `Visual Studio Installer Projects` để tạo file cài đặt
1. Tạo file cài đặt:
    - `Add new project` &rarr; `Other project types` &rarr; `Visual Studio Installer` &rarr; `Setup project`
    - Thêm file exe chương trình vào nơi cài đặt
`Add` &rarr; `Project Output …` &rarr; `Primary output from …`
    - Thêm file config
1. Tạo Shotcut trên desktop
    - Chọn `Create shortcut to primary output from …` từ menu `Add`
    - Kéo thả vào `User’s Desktop`
    - Đặt tên theo ý muốn
1. Một số lưu ý
    - Phiên bản của .Net target phải giống nhau
    - Check của project setup
        - View &rarr; Editor &rarr; Launch conditions
        - Cần đảm bảo 2 thư mục Application Folder & User’s Desktop thuộc tính AlwaysCreate là true
    - Thay đổi thư mục mặc định cài từ Default Company Name theo ý thích (F4) `View` &rarr; `Property Window`
        - Vào Property của Setup Project
        - Thay đổi Manufacturer
1. Chạy file cài từ Visual Studio để kiểm tra
    - Chuột phải vào project setup &rarr; `Install`
    - Đưa file cho client

1. Đổi icon

## Log4net
- Log4net là một thư viện hỗ trợ ghi log trong ứng dụng, hữu ích trong việc gỡ lỗi ứng dụng.
- Cài đặt thư viện log4net
    - `Tools` &rarr; `Nuget Package Manager` &rarr; `Manage Nuget Packages for Solution`
    - Tìm kiếm `log4net` và cài đặt








