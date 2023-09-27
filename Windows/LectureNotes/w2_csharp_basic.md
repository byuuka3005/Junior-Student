# C# cơ bản và giao diện (tiếp theo)

## Artifacts (tiếp tục)

- Tránh ngắt chương trình đột ngột để viết unit test
- Kết quả của hàm nên dồn vào một biến để dễ debug
- Tránh dùng `var` cho kiểu dữ liệu không rõ ràng 
- Sử dụng `#region` để nhóm các phần code liên quan (`ctrl + k, ctrl + s` để bao quanh code bằng `#region`)
- Cẩn thận khi chia cho 0 
- Nếu có quyền nói thì nói (`Write`), không có thì thì la (`throw Exception`) 
- Mẫu `Builder` thường dùng để lấy API, truy vấn lấy dữ liệu từ database,...

## Generic

```cs

    // lưu kiểu cố định hoặc các kiểu cùng kế thừa từ một kiểu cố định
    var a = new List<int>();
    var count = 10;
    var numberGen = new Random();
    var maximum = 100; // const int maximum = 100;

    for (int i = 0; i < count; i++) {
        int number = numberGen.Next(maximum);
        a.Add(number);
    }

    for(int i = 0; i < a.Count; i++) {
        Console.Write(a[i] + " ");
    }

    foreach (var number in a) {
        Console.Write(number + " ");
    }

    // có thể lưu các kiểu khác nhau
    var b = new ArrayList();
    b.Add(1);
    b.Add("Hello");
    b.Add(1.0f);
    b.Add('a');

```

## Toán tử
- Các toán tử phổ biến trong C# hầu hết tương tự trong C++
    - References: `  .    ()   []   new   ->`
    - Arithmetic: ` +   ++   -   --   *  /   %   sizeof`
    - Logical:  `&   |   ^   !`
    - Conditional: `&& (&)   ||   ! ==   !=   >   >=   <   <=`
    - Type verification:   `is    as   typeof`
    - Bitwise:  `~   >>   <<` 
    - Selection:   `?:   ?? (not null)`
    - Assignment:` =   +=   -=   *=   /=   %=   &=   |=   ^=   >>=   <<=    ??=`
    - [Lambda expression definition](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/lambda-operator):   `=>`

    ```cs
        // Ví dụ về các toán tử mới xuất hiện trong C#
        // ?? giống với toán tử 2 ngôi nhưng đối tượng sẽ được so sánh mặc định với null
        int? x = 5;     // nullable type - kiểu có thể null, xuất phát từ nhu cầu sử dụng trong database, 
        int y = x ?? 0; // Selection ?? operator
        int z = x;      // Error: nullable type
        // cannot assign to non-nullable type

        // !
        int t = x!;     // tương đương với int t = (int)x;

        // ??=
        if (variable is null) {
            variable = expression;
        }
        // tương đương với
        variable ??= expression;

        //=>
        // Lambda expression / Lambda operator – anonymous method
        int Doubl(int x) => x * 2; 
        // tương đương với
        public int Doubl(int x){return x * 2;}

        // giả sử có danh sách sinh viên _students
        private List<Student> _students;
        // tìm kiếm sinh viên tên name trong danh sách
        public Student GetStudentByName(string name) => _students.FirstOrDefault(s => s.Name == name);
        // -> FirstOrDefault() trả về null nếu ko tìm thấy
        // First() không không trả về null và throw exception khi không thấy phần tử
        // sắp xếp sinh viên trong list giảm dần theo điểm trung bình (thông thường nên phát sinh một list clone), sau đó sx tên theo alphabet
        var orderedList = _students.OrderByDescending(s => s.Gpa).ThenBy(s => s.Name);
        // mặc định của OrderBy và ThenBy là tăng dần, muốn giảm dần thì thêm Descending vào như ví dụ trên

    ```

    ## String concatenation - Nối chuỗi
    ```cs
        var sb = new StringBuilder();
        sb.Append("Hello");
        sb.Append(" ");
        sb.Append("World");
        sb.Append(times);
        Console.WriteLine(sb.ToString());

    ```

    ## String split - Tách chuỗi
    ```cs
        var s = "Hello World";
        var words = s.Split(' ');
        foreach (var word in words) {
            Console.WriteLine(word);
        }

        var fullName = "Nguyen Van A";
        const char Separator = ' ';
        var token = fullName.Split(Separator, StringSplitOptions.RemoveEmptyEntries);
        // StringSplitOptions.RemoveEmptyEntries: bỏ qua các phần tử rỗng
        // StringSplitOptions.None: giữ nguyên các phần tử rỗng
        var first = token[0];
        var middle = token[1];
        var last = token[2];
    ```

## Substring - Lấy chuỗi con và tim kiếm chuỗi
```cs
    var s = "Hello World";
    // Substring(startIndex, length)
    var sub = s.Substring(6, 5); // World
    var sub2 = s.Substring(6); // World

    int index = fullName.IndexOf(" "); // 6
    var lastIndexOf = fullName.LastIndexOf(" "); // 10
    var firstName = fullName.Substring(0, index); // Nguyen
    var lastName = fullName.Substring(index + 1); // Van A
```

## Tuple 
- Tuple là một cấu trúc dữ liệu cho phép lưu trữ một tập hợp các giá trị có kiểu dữ liệu khác nhau
```cs
    // ví dụ về kiểu Tuple
    Tuple<int, string, string> person = new Tuple<int, string, string>(1, "Nguyen Van", "A");

    // truy cập các thành phần trong Tuple

    // cách 1
    Console.WriteLine(person.Item1);
    Console.WriteLine(person.Item2);
    Console.WriteLine(person.Item3);

    // cách 2 - auto destructuring
    var (id, firstName, lastName) = person;
    Console.WriteLine(id);
    Console.WriteLine(firstName);
    Console.WriteLine(lastName);

    // Ví dụ hàm trả về Tuple khi cần trả về nhiều hơn 1 giá trị
    /// <summary>
    /// Divide two integers and return the result as a Tuple
    /// Item1: the result of the division
    /// Item2: the remainder of the division
    /// </summary>
    /// <param name="dividend">The number to be divided (dividend)</param>
    /// <param name="divisor">The number to divide by (divisor)</param>
    /// <returns>A Tuple containing the quotient and remainder</returns>
    public Tuple<int, int> Divide(int dividend, int divisor) {
        int quotient = dividend / divisor;
        int remainder = dividend % divisor;
        //return Tuple.Create(quotient, remainder);
        Tuple<int, int> result = new Tuple<int, int>(quotient, remainder);
        return result;
    }

    // Truy cập thành phần trong Tuple
    var result = Divide(5, 2);
    Console.WriteLine($"Quotient = {result.Item1}, Remainder = {result.Item2}");

    string path = @"C:\Windows\Dev\data.rar";
    // chuỗi bắt đầu bằng @ là chuỗi không cần escape các ký tự đặc biệt như \ 

    // viết một hàm trả về đường dẫn chứa thư mục, tên file và phần mở rộng
    public Tuple<string, string, string> GetPathInfo(string path) {
        int index = path.LastIndexOf('\\');
        string folder = path.Substring(0, index);
        string fileName = path.Substring(index + 1);
        index = fileName.LastIndexOf('.');
        string name = fileName.Substring(0, index);
        string extension = fileName.Substring(index + 1);
        return new Tuple<string, string, string>(folder, name, extension);
    }

    // viết một hàm trả về đường dẫn chứa thư mục, tên file và phần mở rộng dùng FileInfo trong System.IO
    public Tuple<string, string, string> GetPathInfo2(string path) {
        FileInfo fileInfo = new FileInfo(path);
        string folder = fileInfo.DirectoryName;
        string name = fileInfo.Name;
        string extension = fileInfo.Extension;
        return new Tuple<string, string, string>(folder, name, extension);
    }

    // viết một hàm trả về đường dẫn chứa thư mục, tên file và phần mở rộng dùng Path
    public Tuple<string, string, string> GetPathInfo3(string path) {
        string folder = Path.GetDirectoryName(path);
        string name = Path.GetFileNameWithoutExtension(path);
        string extension = Path.GetExtension(path);
        return new Tuple<string, string, string>(folder, name, extension);
    }

```