# Tổng quan về hệ điều hành

## 1. Định nghĩa
- Phần mềm ứng dụng: phần mềm phục vụ giải quyết bài toán cụ thể nào đó
- Phần mềm hệ thống: môi trường thực thi các phần mềm khác
- Hệ điều hành là một phần mềm hệ thống

## 2. Vai trò 
- **Ảo hoá**: Cung cấp các khái niệm trừu tượng giúp dễ dàng thao tác với phần cứng
- **Quản lí tài nguyên**: phân chia tài nguyên cho các tiến tình một các công bằng, hiệu quả và an toàn. Các hoạt động cấp phát và thu hồi, chia sẻ và bảo vệ
- **Cung cấp các lời gọi hệ thống**: Cung cấp danh sách các hàm hệ thộng để tương tác với phần cứng

## 3. Phân loại
- **Batch OS - Hệ điều hành xử lí theo lô**: thẻ đục lỗ, một tập hợp các tác vụ liên quan với nhau (1 tác vụ, 1 job), tại 1 thời điểm chỉ xủa lí một tác vụ, khi yêu cầu đọc ghi, hệ điều hảnh rảnh.
- **Multiprograming OS - Hệ điều hành đa chương**: tránh tình trạng CPU bị rảnh rỗi khi đọc ghi, đảm bảo hiệu suất của cpu được khai thác tối đa, bằng cách cho CPU nhận tác vụ khác khi bận đọc ghi.
- **Multitask OS - Timesharing OS - Hệ điều hành đa nhiệm**: là phiên bản mở rộng của hdh đa chương, luân phiên xử lí các tác vụ, sử dụng thuật toán điều phối CPU

- **Parallel OS - Multiprocessing OS - Hệ điều hành song song/đa xử lí**: Có nhiều CPU và các CPU này hoàn toàn có thể chạy song song với nhau. Một vài tác vụ có thể chạy song song trên các nhân của CPU nhưng không phải là hệ điều hành song song.

- **Distributed OS - Hệ điều hành phân tán**: Nhiều máy tính, thiết bị liên kết với nhau cùng phối hợp thực hiện một công việc nào đó, chia sẻ cùng tài nguyên.

- **Real-time OS - RTOS - Hệ điều hành xử lí thoài gian thực**: Đảm bảo độ chính xác bị ràng buộc về mặt thời gian. 
    - Hard real-time: ràng buộc thời gian là bắt buộc, delay sẽ dẫn dến vấn đề
    - Soft real-time: độ trễ thời gian không gây ra vấn đề lớn, không quá nghiêm trọng

- **Embedded OS - Hệ điều hành nhúng**: sử dụng trên robot hoặc các thiết bị không phải là máy tính hiện đại, có khả năng hoạt động trên điều kiện giới hạn.

## 4. Các thành phần chính
- **Quản lí tiến trình**:
    - Quản lí được hoặt động của tiến trình
    - Cơ chế liên lạc (Interprocess Communication - IPC)
    - Điều phối CPU
    - Đồng bộ hoá
- **Quản lí bộ nhớ**:
    - Cấp phát/thu hồi và cơ chế bảo vệ bộ nhớ
    - Bộ nhớ ảo: chọn phân vùng đặc biệt trên ROM, dùng riêng cho mở rộng bộ nhớ chính.

- **Quản lí tập tin và ổ đĩa**: 
    - Tập tin là khái niệm trừu tượng của hdh
    
- **Quản lí hệ thống nhập xuất**: quản lí việc nhập xuất của mỗi tiến trình đã xong hay chưa.

- **System Call**: Lời gọi hệ thống, hàm hệ thống: tập hợp các hàm trên hệ thống cho phép thao tác với hệ điều hành. Được cung cấp bởi hệ điều hành (API) và được cài đặt sẵn trong các ngôn ngữ lập trình. Các tập hợp các mã lệnh: 
    - Kernel: nhân hệ điều hành chứa các đoạn mã chính
    - System call Interface: Cho phép ứng dụng giao tiếp với hệ điều hành
    - Device drivers: Cho phép hệ điều hành giao tiếp với phần cứng

- **SHELL or Command Interpreter**: Hệ thống cơ chế dòng lệnh, cho phép thực hiện thao thác thông qua lệnh thay vì giao diện.

- **Protection and Security**: Bảo vệ tài nguyên cho máy tính

## 5. Kiến trúc

- **Simple Structure - Kiến trúc đơn giản**
    - Không phân chia thành các thành phần của hệ điều hành
    - Các phần mềm ứng dụng có thể truy cập thẳng vào phần cứng.
    VD: MS.DOS
- **Monolithic Structure - Kiến trúc đơn khối/một khối**
    - Các thành phần được gộp lại thành một module
    - Các phần mềm ứng dụng không còn có thể truy cập thẳng vào phần cứng
    - VD: UNIX truyền thống
- **Layered Structure - Kiến trúc phân tầng**
    - Các thành phần được chia ra làm nhiều tầng
    - Các phần mềm ứng dụng phải thông qua nhiều tầng mới có thể thao tác với phần cứng
- **Microkenel Structure - Kiến trúc vi nhân**
    - Nhân được thu hẹp lại, những module không thực sự cần đưa vào nhân, mà nó có thể chuyển thành phần mềm ứng dụng. 
- **Hybrid Structure - Kiến trúc lai**
    - Kết hợp các kiến trúc lại với nhau

- **Các kiến trúc khác**