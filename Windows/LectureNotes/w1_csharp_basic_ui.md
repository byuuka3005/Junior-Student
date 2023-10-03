# C# cơ bản và giao diện

## .NET

## Hello World và artifacts
- Chương trình Hello World
```cs
using System;

namespace ConsoleApp1 {
    
    class Program{

        static void Main(string args[]) {
            
            Console.WriteLine("Hello World!");

        }

    }

}
```
- Artifacts
    - Build, lỗi, chạy, debug, ...
    - Comment code và độ quan trọng của nó
    - Kiểu dữ liệu giữa các kiểu viết thường và viết hoa
        - Kiểu nguyên thuỷ
        - Quá trình boxing
    - Tóm tắt hàm 
        - để điều kiện của biến vào sau mô tả tên biến.
            Vd: mô tả tên biến (>0)
        - Hạn chế **ngắt đột ngột** bằng return $\Rightarrow$ kết quả của hàm luôn nằm ở một chổ
        ```cs
            /// <summary>
            /// </summary>
        ```

## Biến và hằng
- **Biến** trong C# tương tự như trong C++
    - Ghi nhớ cách đặt tên biến:
        - Private: _camelCase
        - Public: PascalCase
    - Kiểu var: tự định nghĩa kiểu dữ liệu của vế trái bằng kiểu dữ liệu của vế phải:
        ```cs
            var arr = new int[4];
            var s = "a string";
        ```

- **Hằng** trong C# cũn tương tự trong C++, ghi nhớ cách đặt tên hằng: PascalCase

## Hàm
- Cách đặt tên hàm
    - Bắt đầu bằng động từ
    - Private: _camelCase
    - Public: PascalCase

## Array - Mảng
- Kĩ thuật String Interpolation
```cs
    int a[] = new int[3];
    a[0] = 1;
    a[1] = 2;
    a[2] = 4;

    for(int i = 0; i<a.Length; i++){
        Console.WriteLine($"Gia tri cua a[{i}] la {a[i]}");
    }
```

## Vòng lặp
- for, while, do..while, foreach
```cs
    // Ví dụ về foreach
    var a = new int[] {1, 2, 3, 4};
    var sum = 0;

    for(var num in a) {
        sum += num;
    }
```
- Kị dùng goto

## Exception

```cs
    try {
        // code
    } catch (Exception ex) {
        // handle or annouce
        Console.Write(ex.Message);
    }
```
## Random generator

```cs
    Random rng = new Random(); // random number generator
    int i = rng.Next();
    int limit = rng.Next(100);
    int range = rng.Next(20, 50);
```

## UI
- Nhắc lại XML
```xml
    <Sinhvien>
        <NamHoc BatDau="2021" KetThuc="2025"/>
    </Sinhvien>
```

- File **XAML**: Các thẻ căn bản
    - Thẻ Canvas: (hạn chế dùng)
        -   Là toạ độ tuyệt đối với 2 thuộc tính Left và Top
    - Thẻ Button: hiển thị nút nhấn
    - Thẻ Imgage
        - Content Type
    - Thẻ Lable
        - Content
    - Thẻ Textblock
        - Text, Margin

- Nhúng ảnh vào file exe (Hạn chế dùng)
    - Chuột phải vào ConBuildAction Resource ->  ảnh nằm trong exe
    - (Hông nghe kịp)

# Bài tập về nhà
* Xem trong [slide](https://docs.google.com/presentation/d/1k0JBWqt8ofjMUnD2HezRo8YII0Nwoiyk/edit#slide=id.p1)
* Hiện thị các câu nói tạo động lực kèm hình ảnh (Yêu cầu 15 câu)
* Hiển thị từ vựng ngẫu nhiên với hình ảnh (Yêu cầu tối thiểu 15 từ)

# Link demo 
* [Link demo](https://drive.google.com/file/d/1eU9ZDKjhpzxbEeKM6Jp5vobZYNAnIG1h/view?usp=drive_link)