# Giao diện

## Giao diện đồ hoạ người dùng (Graphical User Interface - GUI)

### The Model-View-Controller (MVC) Architecture (Kiến trúc Mô hình - Xem - Điều khiển)
- Model: dữ liệu và các hàm xử lí dữ liệu, bao gồm Java code và các API dùng để biểu diễn bài toán, quản lí hành vi và dữ liệu ứng dụng
- View: giao diện người dùng, bao gồm các thành phần như button, text field, ... mà người dùng có thể nhìn thấy và tương tác
- Controller: điều khiển các hoạt động của ứng dụng, bao gồm các hàm xử lí sự kiện, các hàm xử lí dữ liệu, các hàm xử lí giao diện người dùng. Đầu vào có thể là sự kiện từ người dùng hoặc từ hệ thống như GPS, camera, cảm biến... và báo cho model và view 

### Các thành phần của giao diện
- Đầu vào: từ nhiều nguồn
- Chuyển trạng thái: từ một trạng thái sang trạng thái khác, vd như gọi onPause...
- Thông báo: thông báo cho người dùng về các sự kiện, vd như pin yếu
- View: (phần hôm nay học)

### Sử dụng XML để thiết kế giao diện
- Nesting layout: sử dụng layout bên trong layout
    - Một activity sử dụng setContentView() để thiết lập layout cho activity đó. Ví dụ như `setContentView(R.layout.activity_main);`
- Thiết lập view:
    - Set properties: thiết lập các thuộc tính của view. Ví dụ một `TextView` có các thuộc tính như `background`, `text`, `textColor`, ...
    - Set up listeners: thiết lập các sự kiện của view. Ví dụ như khi người dùng click vào một `Button`, các sự kiện có thể xảy ra như click, long-tap, mouse-over,...  
    - Set focus: thiết lập focus cho view, trong java code, sử dụng `.requestFocus()` để thiết lập focus cho view và thẻ `<requestFocus />` trong XML. Ví dụ màn hình login hiện lên thì set focus cho ô nhập tên người dùng    
    - Set visibility: thiết lập view có hiển thị hay không, sử dụng hàm `.setVisibility()` trong java code và thuộc tính `android:visibility` trong XML

- Chỉnh sửa GUI
    - Sử dụng XML
    - Sử dụng WYSIWYG (What You See Is What You Get) editor

### Widget 
- Một số widget:
    - `TextView`: hiển thị text
    - `EditText`: hiển thị text và cho phép người dùng nhập text
    - `Button`: hiển thị text và cho phép người dùng click vào
    - `ImageView`: hiển thị hình ảnh
    - `CheckBox`: hiển thị một ô vuông và cho phép người dùng check vào
    - `RadioButton`: hiển thị một nút tròn và cho phép người dùng chọn
    - `ToggleButton`: hiển thị một nút tròn và cho phép người dùng chọn, nhưng khác với `RadioButton` là nó có thể chọn nhiều lần
    - `Spinner`: hiển thị một danh sách các lựa chọn và cho phép người dùng chọn
    - `ListView`: hiển thị một danh sách các lựa chọn và cho phép người dùng chọn
    - `GridView`: hiển thị một danh sách các lựa chọn và cho phép người dùng chọn
    - `DatePicker`: hiển thị một lịch và cho phép người dùng chọn ngày
    - `TimePicker`: hiển thị một lịch và cho phép người dùng chọn thời gian
    - `AutoCompleteTextView`: hiển thị một ô nhập text và cho phép người dùng nhập text, nó cũng hiển thị một danh sách các lựa chọn dựa trên những gì người dùng đã nhập

    <img src="w2/sample_widget.png">

## Các Widget và Layout

### FrameLayout
- Các widget được đặt chồng lên nhau
- Các widget được đặt ở vị trí tương đối với parent hoặc với nhau
- [Xem thêm](https://developer.android.com/guide/topics/ui/layout/frame?hl=vi)
### LinearLayout
- Các widget được đặt (chồng chất) theo chiều ngang hoặc chiều dọc
- Nếu là dọc thì các widget được đặt theo chiều từ trên xuống dưới hoặc từ dưới lên trên
- Nếu là ngang thì các widget được đặt theo chiều từ trái sang phải hoặc từ phải sang trái 
- Fill model: các widget được đặt theo chiều ngang hoặc chiều dọc, chiếm hết không gian của parent
- `Weight`: các widget được đặt theo chiều ngang hoặc chiều dọc, chiếm hết không gian của parent, nhưng có thể thiết lập trọng số cho các widget để chia không gian của parent cho các widget
- `Gravity`: các widget được đặt theo chiều ngang hoặc chiều dọc, có thể thiết lập vị trí của các widget trong parent
- `Padding`: khoảng lề các widget
- `Margin`: khoảng cách giữa các widget và parent
- Xem thêm ở [Web](https://developer.android.com/guide/topics/ui/layout/linear?hl=vi) hoặc [Note của bck](../StudyNotes/w1_homework.md)

### RelativeLayout
- Các widget được đặt tương đối với nhau hoặc với parent
- `android:layout_alignParentTop`: đặt widget ở phía trên của parent
- `android:layout_alignParentBottom`: đặt widget ở phía dưới của parent
- `android:layout_alignParentLeft`: đặt widget ở phía trái của parent
- `android:layout_alignParentRight`: đặt widget ở phía phải của parent
- `android:layout_centerInParent`: đặt widget ở giữa của parent
- `android:layout_centerHorizontal`: đặt widget ở giữa theo chiều ngang của parent
- `android:layout_centerVertical`: đặt widget ở giữa theo chiều dọc của parent
- `android:layout_above`: đặt widget ở phía trên của widget khác
- `android:layout_below`: đặt widget ở phía dưới của widget khác

### TableLayout
- Các widget được đặt theo hàng và cột
- Số cột là flexible, tuỳ thuộc vào số cột nhiều nhất của dòng
- Số dòng là tuỳ ý, thêm dòng bằng cách thêm thẻ `<TableRow>` vào trong thẻ `<TableLayout>`, bên trong của thẻ `<TableRow>` là các widget
- Trộn cột bằng cách sử dụng thuộc tính `android:layout_span`
- Stretch columns: các cột có thể được kéo dãn để chiếm hết không gian của parent bằng cách sử dụng thuộc tính `android:stretchColumns`

### ScrollView
- Các widget được đặt theo chiều dọc
- Các widget có thể cuộn lên xuống và được xếp thành các thumbnail

### HorizontalScrollView
- Các widget được đặt theo chiều ngang
- Các widget có thể cuộn qua lại và được xếp thành các thumbnail

## Kết nối tới Java code

- Khai báo các property trong class

### TextView
- Sử dụng `findViewById()` để lấy các widget từ layout, ví dụ `TextView tv = (TextView) findViewById(R.id.textView1);` 
- Text trong TextView có thể hiểu các định dạng các kí tự đặc biệt như `\n` hoặc `<br>` để xuống dòng

### Button
- Sử dụng `findViewById()` để lấy các widget từ layout, ví dụ `Button btn = (Button) findViewById(R.id.button1);` lưu ý là phải ép kiểu
- Sử dụng các hàm xử lí sự kiện để xử lí các sự kiện của widget, ví dụ 
```java
    btn.setOnClickListener(new View.OnClickListener() {
        @Override public void onClick(View v) { 
        // TODO Auto-generated method stub 
        } 
    });
```
- Có thể Implement interface các hàm xử lí sự kiện, ví dụ ta có 2 nút `Begin` và `Exit`
```java
    public class MainActivity extends Activity implements OnClickListener {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button btnBegin = (Button) findViewById(R.id.btnBegin);
            btnBegin.setOnClickListener(this);
            Button btnExit = (Button) findViewById(R.id.btnExit);
        }
        @Override
        public void onClick(View v) {
            // xử lí sự kiện cho 2 nút Begin và Exit
            switch (v.getId()) {
                case R.id.btnBegin:
                    // xử lí sự kiện cho nút Begin
                    break;
                case R.id.btnExit:
                    // xử lí sự kiện cho nút Exit
                    break;
                default:
                    break;
            }
        }
    }
```

### EditText
- Sử dụng `findViewById()` để lấy các widget từ layout, ví dụ `EditText edt = (EditText) findViewById(R.id.editText1);` 
- Một số thuộc tính xem thêm ở [đây](https://developer.android.com/reference/android/widget/EditText?hl=vi)

### ImageView
- Sử dụng `findViewById()` để lấy các widget từ layout, ví dụ `ImageView img = (ImageView) findViewById(R.id.imageView1);`
- Một số thuộc tính xem thêm ở [đây](https://developer.android.com/reference/android/widget/ImageView?hl=vi)

### CheckBox
- Sử dụng `findViewById()` để lấy các widget từ layout, ví dụ `CheckBox chk = (CheckBox) findViewById(R.id.checkBox1);`
- Một số thuộc tính xem thêm ở [đây](https://developer.android.com/reference/android/widget/CheckBox?hl=vi)

### RadioButton
- Sử dụng `findViewById()` để lấy các widget từ layout, ví dụ `RadioButton rdo = (RadioButton) findViewById(R.id.radioButton1);`
- Một số thuộc tính xem thêm ở [đây](https://developer.android.com/reference/android/widget/RadioButton?hl=vi)

## Đọc thêm trong slide
- Miscellaneous
- Using the @string resource
- Android asset studio
- Measuring graphic elements
- Hierarchy viewer tool
- Customizing widgets
- Fixing bleeding background color

# Bài tập về nhà
