# Thẩm định dữ liệu đầu vào (validation)

- Thẩm định dữ liệu đầu vào là một trong những vấn đề quan trọng trong lập trình ứng dụng.

# Lớp `ValidationRule`

- Lớp `ValidationRule` là một lớp trừu tượng, được sử dụng để kiểm tra tính hợp lệ của dữ liệu đầu vào.
- Khi kế thừa lớp `ValidationRule`, ta phải override phương thức `Validate` để kiểm tra tính hợp lệ của dữ liệu đầu vào.
- Ví dụ hàm kiểm tra tuổi trong khoảng `Min` đến `Max`
```cs
public class AgeRangeRule : ValidationRule
{
    public int Min { get; set; }
    public int Max { get; set; }

    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        int age = 0;

        try
        {
            if (((string)value).Length > 0)
                age = Int32.Parse((String)value);
        }
        catch (Exception e)
        {
            return new ValidationResult(false, "Illegal characters or " + e.Message);
        }

        if ((age < Min) || (age > Max))
        {
            return new ValidationResult(false,
              "Please enter an age in the range: " + Min + " - " + Max + ".");
        }
        return ValidationResult.ValidResult;
    }
}
```
