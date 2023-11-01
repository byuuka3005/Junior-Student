# Phân trang

## Trả ra tổng item
```sql
    SELECT *, COUNT(*) OVER() AS TotalItems FROM SinhVien
```

## Công thức hiển thị

Nếu đang ở trang i tức là 
- Bỏ qua $i-1$ trang
- Tổng cộng $(i-1) * rowPerPage$ item đã bỏ qua

## Phân trang với ComboBox

```cs
    private void comboBox1_SelectedIndexChanged(object sender, EventArgs e)
    {
        // nên đưa rowPerPage và page vào biến toàn cục của Window class
        var rowPerPage = int.Parse(comboBox1.SelectedItem.ToString());
        var page = 1;
        var skip = (page - 1) * rowPerPage;
        // Chọn tất cả các cột, bỏ qua 'skip' dòng, lấy 'rowPerPage' dòng
        var sql = $"SELECT * FROM SinhVien ORDER BY id OFFSET {skip} ROWS FETCH NEXT {rowPerPage} ROWS ONLY";
    
        var connection = new SqlConnection(connectionString);
        connection.Open();
        var command = new SqlCommand(sql, connection);
        var reader = command.ExecuteReader();
        var students = new List<Student>();
        while (reader.Read())
        {
            var id = reader["id"] as int? ?? 0;
            var name = reader["name"] as string;
            var age = reader["age"] as int? ?? 0;
            var student = new Student
            {
                Id = id,
                Name = name,
                Age = age
            };
            students.Add(student);
        }

        // Tính toán phân trang 
        var totalItems = reader["TotalItems"] as int? ?? 0;
        reader.Close();
        dataListView1.DataSource = students;
    }
```