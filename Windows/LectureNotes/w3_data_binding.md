# Data Binding

## Data Binding là gì?
- Data Binding là cơ chế cho phép các thuộc tính của một đối tượng được gán giá trị từ một nguồn dữ liệu khác
- Data Binding giúp cho việc lập trình trở nên dễ dàng hơn, giảm thiểu việc phải viết code để gán giá trị cho các thuộc tính của đối tượng
- Ví dụ về Data Binding:
    - Gán giá trị cho thuộc tính `Text` của `Label` từ một `TextBox`, `ComboBox`, `RadioButton`, `CheckBox`,  `Slider`, `ProgressBar`, ... Từ class

## Data Binding trong WPF
- Cấu trúc trong `XAML`:
```xml
   <thẻ
        ThuộcTính="{Binding ThuộcTínhCủaClass}"
        d:ThuộcTính="GiáTrịMặcĐịnhỞMứcDesign"
   >
   </thẻ>

```

- Cấu trúc trong `C#`:
```csharp
    class Class
    {
        public string ThuộcTínhCủaClass { get; set; }
    }
```

- Để match giữa `XAML` và `C#`, ta sử dụng `DataContext`, Ví dụ:
```csharp
    public partial class MainWindow : Window
    {
        Class Student : INotifyPropertyChanged
        {
            string Id { get; set; }
            string Name { get; set; }
        }
        public MainWindow()
        {
            InitializeComponent();
            Student student = new Student();
            student.Id = "001";
            student.Name = "Nguyen Van A";
            this.DataContext = student;
        }

        public event PropertyChangedEventHandler PropertyChanged;
    }
```

- INotifyPropertyChanged
    - Để `XAML` có thể nhận được sự thay đổi của thuộc tính trong `C#`, ta phải implement interface `INotifyPropertyChanged` và sử dụng event `PropertyChanged` để thông báo cho `XAML` biết rằng thuộc tính đã thay đổi
    - Ví dụ:
    ```csharp
        public partial class MainWindow : Window
        {
            class Student : INotifyPropertyChanged
            {
                string id;
                string name;
                int credit;

                public string Id
                {
                    get => id;
                    // khai báo tường minh setter để sử dụng event PropertyChanged
                    set
                    {
                        id = value;
                        PropertyChanged?.Invoke(this, 
                            new PropertyChangedEventArgs("Id"));
                    }
                }
                public string Name
                {
                    get => name;
                    set
                    {
                        name = value;
                        PropertyChanged?.Invoke(this, 
                            new PropertyChangedEventArgs("Name"));
                    }
                }
                public int Credit
                {
                    get => credit;
                    set
                    {
                        credit = value;
                        PropertyChanged?.Invoke(this, 
                            new PropertyChangedEventArgs("Credit"));
                    }
                }
            }
            public MainWindow()
            {
                InitializeComponent();
                Student student = new Student();
                student.Id = "001";
                student.Name = "Nguyen Van A";
                this.DataContext = student;
            }

            public event PropertyChangedEventHandler PropertyChanged;
        }
    ```
- Cái đó viết dài dòng vậy thôi, mình có thể nhờ trình biên dịch làm cho mình bằng cách sử dụng `Fody` và `PropertyChanged.Fody`
    - Cài đặt `PropertyChanged.Fody` cho project ở `Tools` &rarr; `NuGet Package Manager` &rarr; `Manage NuGet Packages for Solution...` &rarr; `Browse` &rarr; `PropertyChanged.Fody` &rarr; `Install`
    - Sử dụng `Fody` để tự động implement `INotifyPropertyChanged` cho các class
    - Ví dụ:
    ```csharp
        [AddINotifyPropertyChangedInterface]
        class Student
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public int Credit { get; set; }
        }
    ```

    