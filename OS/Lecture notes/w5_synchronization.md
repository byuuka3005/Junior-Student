# Syncronization (Đồng bộ hoá)

## Critical section problem (Vấn đề khu vực quan trọng - Miền găng)
- Miền găng là 1 đoạn code chia sẻ tài nguyên giữa các tiểu trình, tiến trình mà không có sự quản lí hiệu quả dẫn đén sự sai lệch kết quả
&rarr; Giải pháp: Đồng bộ hoá 

## Syncronization (Đồng bộ hoá)

- Mutual exclusion (Độc quyền truy xuất): Một tiểu trình đang thực thi trong miền găng thì các tiểu trình khác không được thực thi trong miền găng đó
- Progress (Tiến trình): Một tiểu/tiến trình ngoài miền găng không thể block các tiểu/tiến trình khác đi vào miền găng
- Bounded waiting (Giới hạn chờ đợi): Các tiểu/tiến trình không được phép chờ đợi vô hạn thời gian để vào miền găng


### Busy waiting (Chờ đợi bận rộn)
- Các tiểu/tiến trình sẽ lặp đi lặp lại việc kiểm tra điều kiện để vào miền găng
```
while (no permission to enter critical section); // busy waiting
enter_critical_section;
```

- Giải pháp phần mềm:
    - **Lock variable (Biến khóa)**: Một biến được sử dụng để kiểm tra xem có tiểu/tiến trình nào đang thực thi trong miền găng hay không

    ```cpp
        int lock = 0;

        Process_A() {
            while (1) {
                while (lock); // busy waiting
                lock = 1;
                critical_section();
                lock = 0;
                // remainder section...
            }
        }

        Process_B() {
            while (1) {
                while (lock); // busy waiting
                lock = 1;
                critical_section();
                lock = 0;
                // remainder section...
            }
        }
        // Tuy nhiên giải pháp này không đảm bảo chỉ có một tiểu/tiến trình được thực thi trong miền găng
    ```
    - **Strict alternation (Luân phiên nghiêm ngặt)**: Các tiểu/tiến trình được phép vào miền găng theo thứ tự
    ```cpp
        int turn = 0;

        Process_A() {
            while (1) {
                while (turn != 0); // busy waiting
                critical_section();
                turn = 1;
                // remainder section...
            }
        }

        Process_B() {
            while (1) {
                while (turn != 1); // busy waiting
                critical_section();
                turn = 0;
                // remainder section...
            }
        }
        // Giải pháp này đảm bảo chỉ có một tiểu/tiến trình được thực thi trong miền găng
        // Tuy nhiên nó không đảm bảo rằng tiến trình A không khoá tiến trình B, tức là khi tiến trình A gặp vấn đề, nó sẽ block luôn tiến trình B
    ```
    - **Peterson's solution (Giải pháp Peterson)**: Giải pháp cho 2 tiểu/tiến trình, là sự kết hợp của 2 giải pháp trên
    ```cpp
        int turn;
        bool flag[2]{0};

        enter_critical_section(int process_id) {
            int other = 1 - process_id;
            flag[process_id] = true;
            turn = other;
            while (flag[other] && turn == other); // busy waiting, while (flag[other] && turn == other) là đoạn code kiểm tra điều kiện để vào miền găng, flag[other] nghĩa là tiến trình khác đang thực thi trong miền găng, turn == other nghĩa là đến lượt tiến trình khác thực thi trong miền găng
        }

        leave_critical_section(int process_id) {
            flag[process_id] = false;
        }

        Process_A() {
            while (1) {
                enter_critical_section(0);
                critical_section();
                leave_critical_section(0);
                // remainder section...
            }
        }

        Process_B() {
            while (1) {
                enter_critical_section(1);
                critical_section();
                leave_critical_section(1);
                // remainder section...
            }
        }
        // Giải pháp này đảm bảo chỉ có một tiểu/tiến trình được thực thi trong miền găng
        // Giải pháp này đảm bảo rằng tiến trình A không khoá tiến trình B
        // Tuy nhiên, giải pháp này làm lãng phí tài nguyên vì các tiểu/tiến trình sẽ lặp đi lặp lại việc kiểm tra điều kiện để vào miền găng, dẫn đến việc tiêu tốn CPU
        // Priority inversion (Ưu tiên đảo ngược): Khi một tiểu/tiến trình có độ ưu tiên thấp hơn tiến trình khác nhưng lại đang thực thi trong miền găng, tiến trình có độ ưu tiên cao hơn sẽ phải chờ đợi tiến trình có độ ưu tiên thấp hơn thực thi xong trong miền găng
    ```
- Giải pháp phần cứng:
    - *Interrupt disabling (Vô hiệu hóa ngắt)*: Vô hiệu hóa ngắt khi một tiểu/tiến trình vào miền găng
        - Interrupt (Ngắt): Là một tín hiệu được gửi bởi một thiết bị ngoại vi hoặc một tiến trình khác để thông báo với CPU rằng một sự kiện đã xảy ra, CPU sẽ ngắt tiến trình đang thực thi để xử lí sự kiện đó
            - Timer Interrupt (Ngắt định thời): Là một tín hiệu được gửi bởi bộ định thời để thông báo với CPU rằng đã hết thời gian thực thi của tiến trình đang thực thi, CPU sẽ ngắt tiến trình đang thực thi để chuyển sang tiến trình khác
            - I/O device Interrupt (Ngắt thiết bị I/O): Là một tín hiệu được gửi bởi một thiết bị I/O để thông báo với CPU rằng đã có dữ liệu được gửi đến, CPU sẽ ngắt tiến trình đang thực thi để xử lí dữ liệu đó
        - Vô hiệu hóa ngắt khi một tiểu/tiến trình vào miền găng sẽ khiến cho các ngắt không thể xảy ra, do đó các tiểu/tiến trình khác không thể vào miền găng
        ```cpp
            disable_interrupt();
            critical_section();
            enable_interrupt();
        ```


    - *TSL instruction (Lệnh TSL)*: Lệnh TSL (Test and Set Lock) được sử dụng để kiểm tra và đặt giá trị của một biến.
        ```cpp
            Test_And_Set_Lock(bool lock) {
                bool test = lock;
                lock = true;
                return test;
            }

            bool lock = false;

            Thread_A() {
                while (1) {
                    while (Test_And_Set_Lock(lock)); // busy waiting
                    critical_section();
                    lock = false;
                    // remainder section...
                }
            }
        ```


### Sleep and wakeup (Ngủ và thức dậy)

- Các tiểu/tiến trình sẽ ngủ khi không thể vào miền găng và thức dậy khi có thể vào miền găng
```cpp
if (no permission to enter critical section) sleep();
critical_section();
wakeup(anothor_thread);
```

- **Semaphore (Đếm):** Một biến nguyên được sử dụng để kiểm tra xem có bao nhiêu tiểu/tiến trình đang thực thi trong miền găng
    - Biến số nguyên được dùng để kiểm tra xem có bao nhiêu tiểu/tiến trình đang thực thi trong miền găng thông qua 2 thao tác: `down` và `up`
        - `down`: Giảm giá trị của biến semaphore đi 1, nếu giá trị của biến semaphore là 0 thì tiểu/tiến trình sẽ ngủ
        - `up`: Tăng giá trị của biến semaphore lên 1, nếu giá trị của biến semaphore là 1 thì tiểu/tiến trình sẽ thức dậy

    ```cpp
        struct Semaphore {
            int value;
            list<thread> list;
        };


        down(Semaphore s) {
            s.value--;
            if (s.value < 0) {
                add this thread to s.list;
                block();
            }
        }

        up(Semaphore s) {
            s.value++;
            if (s.value <= 0) {
                remove a thread P from s.list;
                wakeup(P);
            }
        }

        Semaphore mutex = 1; // Binary semaphore

        Process_A() {
            down(mutex);
            critical_section();
            up(mutex);
            // remainder section...
        }

        Semaphore count = 5; // Counting semaphore

        Process_X() {
            down(count);
            critical_section();
            up(count);
            // remainder section...
        }


    ```

    - *Binary semaphore (Đếm nhị phân)*: Giá trị của biến semaphore chỉ có thể là 0 hoặc 1
    - *Counting semaphore (Đếm)*: Giá trị của biến semaphore có thể là bất kì số nguyên nào không âm

- Monitor (Kiểm soát): Một khối mã được bao bọc bởi một khối mã khác, chỉ có thể có một tiểu/tiến trình thực thi trong khối mã bên trong. Monitor là một cấu trúc dữ liệu do ngôn ngữ lập trình hỗ trợ (như Java)
    ```cpp
    struct Monitor {
        variable shared_item;
        condition cond;

        procedure get_item() {
            ...
            c.wait();
            ...
        }

        procedure put_item() {
            ...
            c.signal();
            ...
        }

    }
    
    ```

    - 

## Classical problems of synchronization (Các bài toán cổ điển về đồng bộ hoá)


