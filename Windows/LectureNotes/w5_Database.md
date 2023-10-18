# Hệ quản trị cơ sở dữ liệu SQL Server

## Các thao tác cơ bản trong Azure Data Studio

### Tạo cơ sở dữ liệu

- Trong Azure Data Studio, mở tab `File` &rarr; `New Query` và nhập lệnh sau:

```sql
CREATE DATABASE [QLSV]
```

hoặc dùng snippet `sqlCreateDatabase` rồi nhập tên csdl

### Backup và restore cơ sở dữ liệu
#### Backup
- Backup bằng lệnh: `BACKUP DATABASE [QLSV] TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\QLSV.bak' WITH NOFORMAT, NOINIT, NAME = N'QLSV-Full Database Backup', SKIP, NOREWIND, NOUNLOAD, STATS = 10`

- Trong đó:
    - `BACKUP DATABASE [QLSV]`: backup csdl `QLSV`
    - `TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\QLSV.bak'`: lưu vào đường dẫn `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\QLSV.bak`
    - `WITH NOFORMAT, NOINIT, NAME = N'QLSV-Full Database Backup', SKIP, NOREWIND, NOUNLOAD, STATS = 10`: các tùy chọn khác
        - `NOFORMAT`: không định dạng lại bản sao lưu
        - `NOINIT`: không ghi đè lên bản sao lưu hiện có
        - `NAME = N'QLSV-Full Database Backup'`: tên bản sao lưu
        - `SKIP`: bỏ qua các tập tin không thể đọc được
        - `NOREWIND`: không đặt lại băng sau khi hoàn thành
        - `NOUNLOAD`: không hủy bỏ băng sau khi hoàn thành
        - `STATS = 10`: hiển thị tiến trình backup sau mỗi 10%

- Backup bằng UI: Chuột phải vào csdl &rarr; `Back Up`, chọn các tùy chọn sau đó bấm `Backup`

#### Restore
- Restore bằng lệnh: `RESTORE DATABASE [QLSV] FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\QLSV.bak' WITH FILE = 1, NOUNLOAD, REPLACE, STATS = 10`
- Trong đó
    - `RESTORE DATABASE [QLSV]`: restore csdl `QLSV`
    - `FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\QLSV.bak'`: lấy từ đường dẫn `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\QLSV.bak`
    - `WITH FILE = 1, NOUNLOAD, REPLACE, STATS = 10`: các tùy chọn khác
        - `FILE = 1`: chỉ định tập tin backup
        - `NOUNLOAD`: không hủy bỏ băng sau khi hoàn thành
        - `REPLACE`: thay thế csdl hiện có
        - `STATS = 10`: hiển thị tiến trình restore sau mỗi 10%
- Restore bằng UI: Chuột phải vào csdl &rarr; `Restore` &rarr; `Database`, chọn các tùy chọn sau đó bấm `Restore`

### Xuất cơ sở dữ liệu ra file .sql script
- Chuột phải vào csdl &rarr; `Tasks` &rarr; `Generate Scripts...` &rarr; `Next` &rarr; `Select specific database objects` &rarr; chọn các đối tượng cần xuất &rarr; `Next` &rarr; `Save to file` &rarr; `Next` &rarr; `Advanced` &rarr; `Types of data to script` &rarr; `Schema and data` &rarr; `OK` &rarr; `Next` &rarr; `Finish`

### Import cơ sở dữ liệu từ file .sql script
- Chuột phải vào csdl &rarr; `Tasks` &rarr; `Import Data...` &rarr; `Next` &rarr; `Data source` &rarr; `Flat file source` &rarr; `Browse` &rarr; chọn file .sql script &rarr; `Next` &rarr; `Destination` &rarr; `SQL Server Native Client 11.0` &rarr; `Server name` &rarr; `Database` &rarr; `Next` &rarr; `Next` &rarr; `Finish`

## 5 bước chính khi thao tác

1. Mở kết nối tới csdl
2. Chuẩn bị câu truy vấn
3. Thực thi câu truy vấn
4. Xử lí kết quả trả về
5. Đóng kết nối tới csdl

## Tiến hành trong C#
### 1. Mở kết nối tới csdl
- Thêm thư viện `System.Data.SqlClient` bằng cách `Tools` &rarr; `NuGet Package Manager` &rarr; `Manage NuGet Packages for Solution...` &rarr; `Browse` &rarr; `System.Data.SqlClient` &rarr; `Install`
- Tạo chuỗi kết nối, VD:
```cs
    string connectionString = """
        Server=.\SQLEXPRESS;
        Database=QLSV;
        Trusted_Connection=True;
        User ID=sa;
        Password=12345
    """;

    SqlConnection connection = new SqlConnection(connectionString);
    // var connection = new SqlConnection(connectionString);

    connection.Open();
```
hoặc sử dụng `StringBuilder`
```cs
    var builder = new SqlConnectionStringBuilder();
    builder.DataSource = @".\SQLEXPRESS";
    builder.InitialCatalog = "QLSV";
    builder.IntegratedSecurity = true;
    builder.UserID = "sa";
    builder.Password = "12345";
    builder.TrustServerCertificate = true;

    SqlConnection connection = new SqlConnection(builder.ConnectionString);
    // var connection = new SqlConnection(builder.ConnectionString);

    connection.Open();
```


- Đưa kết nối vào tiến trình khác (Để có thể thao tác khi các tiến trình khác đang thực thi)
```cs
    connection = await Task.Run(() => {
        var connection = new SqlConnection(connectionString);
        connection.Open();
        // sleep để chờ kết nối
        // Thread.Sleep(1000);
        return connection;
    });    
```

### 2. Chuẩn bị câu truy vấn
- Tạo câu truy vấn
```cs
    var query = "SELECT * FROM SinhVien";
```

- Tuy nhiên, trong thực tế, khi ta nhập tên đăng nhập và mật khẩu, ta không muốn lộ thông tin đăng nhập ra ngoài hoặc tránh lỗi `sql injection`. Do đó, ta sẽ sử dụng tham số để thay thế cho tên đăng nhập và mật khẩu
```cs
sql = "select * from NhanVien where Username=@User";

var command = new SqlCommand(sql, connection);

command.Parameters.Add("@User", OleDbType.NVarChar); 
command.Parameters["@User"].Value = username;
```

hoặc viết tắt
```cs
command.Parametes.Add("@User", OleDbType.NVarChar).Value = "";
```

### 3. Thực thi câu truy vấn
- Các kiểu lệnh có thể thực thi:
    - `ExecuteNonQuery`: thực thi các câu lệnh `INSERT`, `UPDATE`, `DELETE`, không có giá trị trả về
    - `ExecuteScalar`: thực thi các câu lệnh `SELECT` trả về **một** giá trị duy nhất
    - `ExecuteReader`: thực thi các câu lệnh `SELECT` trả về **nhiều** giá trị

- Command cho phép thực thi các câu lệnh trên
```cs
    var command = new SqlCommand(sql, connection);
    command.ExecuteNonQuery();
```

- Reader cho phép đọc các giá trị trả về
```cs
    var reader = command.ExecuteReader();
```

### 4. Xử lí kết quả trả về
- Đọc dữ liệu từ Reader

    - Đọc từng dòng:
    ```cs
        while(reader.Read()){
            // đọc dữ liệu từ reader
            var id = reader[0] as int;
            var name = reader[1] as string;
        }
    ```
    - Hoặc cách dễ hiểu và dễ bảo trì
    ```cs
        while(reader.Read()){
            // đọc dữ liệu từ reader
            var id = reader["id"] as int;
            var name = reader["name"] as string;
        }
    ```
- Cập nhật dữ liệu, ví dụ:
    ```cs
        var sql = "UPDATE SinhVien SET Name = @Name WHERE Id = @Id";
        var command = new SqlCommand(sql, connection);
        int rowsAffected = command.ExecuteNonQuery();
    ```

- Cập nhật trigger: khi thêm dữ liệu mà Id được cấp tự động, ta cần lấy Id vừa được cấp để thực hiện các thao tác khác. Thay vì thực hiện `chèn` &rarr; `fetch` &rarr; `lấy`, ta có thể thực hiện `chèn` &rarr; `lấy` bằng cách thêm trực tiếp vào `BindingList` chứa dữ liệu. 
    ```cs
        var sql = "INSERT INTO SinhVien(Name) VALUES(@Name); SELECT SCOPE_IDENTITY()";
        var command = new SqlCommand(sql, connection);
        command.Parameters.Add("@Name", OleDbType.NVarChar).Value = name;
        var id = command.ExecuteScalar() as int;
        var sv = new SinhVien(id, name);
        listSinhVien.Add(sv);
    ```

# Bài tập về nhà
- Cải tiến bài tập về nhà tuần 4 với dữ liệu được lấy từ database thay vì code cứng

# >>> Tuần sau >>>
- Phân trang 
- Trigger
- validation