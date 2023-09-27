# Hướng đối tượng trong C#

## Ôn tập hướng đối tượng

### 4 tính chất cơ bản
- **Tính đóng gói (Encapsulation)**: 
    - Đóng gói dữ liệu (data hiding): 
        - private, protected, public, internal
        - property
    - Đóng gói hành vi (behavior hiding): 
        - private, protected, public, internal
        - method
- **Tính kế thừa (Inheritance)**: 
    - class, interface
- **Tính đa hình (Polymorphism)**: 
    - virtual, override, abstract, interface
- **Tính trừu tượng (Abstraction)**: 
    - abstract, interface

### Class - Lớp
- Là một cấu trúc dữ liệu cho phép lưu trữ dữ liệu và hành vi (behavior) của một đối tượng

```cs
class Student
{
    // fields
    private string _firstName { get; set; };
    private string _lastName { get; set; };
    private string _phone;
    private int _age  { get; set; };
    private string _address;



    // properties
    public string Phone 
    { 
        get => _phone;
        set => _phone = value; 
    }

    
    public string FullName => $"{_firstName} {_lastName}";

    // methods
    public void Study()
    {
        Console.WriteLine("I'm studying");
    }
}

```

## Mở rộng kiến thức về lớp
- Extension method: một số lớp sẽ có thêm các method mà không cần phải thay đổi code của lớp đó. Ví dụ thông thường lớp `string` không có method `IsEmpty` nhưng ta có thể thêm method này bằng cách tạo một lớp mới và thêm method vào lớp đó.
```cs
namespace Example {
    public static class StringExtension {
        public static bool IsEmpty(this string str) {
            return string.IsNullOrEmpty(str);
        }
    }
}

// sử dụng
using Example;
// ... 
    string str1 = "Hello";
    string str2 = "";
    str.IsEmpty(); // false
    str2.IsEmpty(); // true

```

