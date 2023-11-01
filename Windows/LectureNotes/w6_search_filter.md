# Tìm kiếm và lọc


## Tìm kiếm
- Giả sử có một TextBox để nhập tên sinh viên cần tìm kiếm

- Khi người dùng nhập vào tên sinh viên, ta sẽ thực hiện câu truy vấn như sau:
```cs
    var name = textBox1.Text;
    //var sql = $"SELECT * FROM SinhVien WHERE name LIKE '%{name}%'";
    // thêm các thuộc tính số dòng hiển thị...
    var sql = """
        SELECT *, COUNT(*) OVER() AS TotalItems FROM SinhVien
        WHERE name LIKE @Name
        ORDER BY id
        OFFSET @Skip ROWS FETCH NEXT @Take ROWS ONLY
    """;
    // Name có dạng %name%
```

