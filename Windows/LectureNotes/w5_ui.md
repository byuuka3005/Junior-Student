# Một số thao tác (Tiếp)

## Thay đổi màn hình start-up
- Trong file `App.xaml`, thay đổi `StartupUri` thành tên file muốn hiển thị
```xml
<Application x:Class="WPF_Lab2.App"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            StartupUri="LoginWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```
## Tập tin .config phục vụ cho việc cấu hình ứng dụng

- Tập tin `App.config` chứa các thông tin cấu hình cho ứng dụng
- Ví dụ lưu thông tin đăng nhập gồm tên đăng nhập và mật khẩu
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
    </startup>
    <appSettings>
        <add key="username" value=""/>
        <add key="password" value=""/>
    </appSettings>
</configuration>
```

- Lấy thông tin từ tập tin cấu hình
```cs
var config = ConfigurationManager.OpenExeConfiguration(ConfigurationUserLevel.None);
config.AppSettings.Settings["username"].Value = username;
config.AppSettings.Settings["password"].Value = password;
// password không có chơi kiểu này
```
- Lưu thông tin vào tập tin cấu hình
```cs
// Lấy từ màn hình login
var username = txtUsername.Text;
var password = txtPassword.Text;
ConfigurationManager.AppSettings["username"] = username;
ConfigurationManager.AppSettings["password"] = password;
```
# Các từ khoá

## `async` và `await`
- `async` và `await` là 2 từ khoá dùng để đánh dấu một phương thức là phương thức bất đồng bộ
- Phương thức bất đồng bộ là phương thức có thể chạy song song với các phương thức khác
- Điểm khác biệt giữa `async` và `await` là `async` được đánh dấu trên phương thức, còn `await` được đánh dấu trên một tác vụ (task)

Ví dụ sử dụng `async` và `await`:
```cs
private async void btnLogin_Click(object sender, RoutedEventArgs e) {
    var username = txtUsername.Text;
    var password = txtPassword.Text;
    var user = await Task.Run(() => {
        return db.Users.SingleOrDefault(u => u.Username == username && u.Password == password);
    });
    if(user != null) {
        MessageBox.Show("Đăng nhập thành công");
    } else {
        MessageBox.Show("Đăng nhập thất bại");
    }
}
```