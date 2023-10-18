# Fragment

## Giới thiệu

- Fragment là một thành phần của một Activity, nó giúp chia nhỏ một Activity thành các phần nhỏ hơn, mỗi phần có thể được thay đổi, thêm vào, xóa đi mà không ảnh hưởng đến các phần khác. Fragment có thể được coi là một Activity nhỏ, nó có thể có các thành phần như Activity như: layout, lifecycle, event, ...

- Fragment có thể được sử dụng lại trong nhiều Activity khác nhau, nó giúp cho việc phát triển ứng dụng trở nên dễ dàng hơn.

- Fragment có thể được thêm vào hoặc xóa đi trong một Activity mà không cần phải tạo một Activity mới.

- Fragment có thể được sử dụng để hiển thị một phần của Activity trên một màn hình lớn, hoặc hiển thị nhiều Fragment trên một màn hình nhỏ.

## Chu kỳ hoạt động của Fragment (Fragment Lifecycle)

- Fragment cũng có một chu kỳ hoạt động tương tự như Activity, nó bao gồm các phương thức sau:

    - `onAttach()`: được gọi khi Fragment được gắn vào Activity.
    - `onCreate()`: được gọi khi Fragment được tạo.
    - `onCreateView()`: được gọi khi Fragment được tạo view.
    - `onActivityCreated()`: được gọi khi Activity **đã** được tạo.
    - `onStart()`: được gọi khi Fragment được khởi chạy.
    - `onResume()`: được gọi khi Fragment được hiển thị.
    - `onPause()`: được gọi khi Fragment bị tạm dừng.
    - `onStop()`: được gọi khi Fragment bị dừng.
    - `onDestroyView()`: được gọi khi Fragment bị hủy view.
    - `onDestroy()`: được gọi khi Fragment bị hủy.
    - `onDetach()`: được gọi khi Fragment bị tách khỏi Activity.

## Tạo một fragment

- Fragment phải được tạo bên trong một FragmentTransaction, nó có thể được tạo bằng cách sử dụng phương thức `add()` hoặc `replace()` của FragmentTransaction.

- Ví dụ: Tạo một fragment bằng cách sử dụng phương thức `add()` của FragmentTransaction.

```java
// Step 1: Obtain a reference to the FragmentManager
FragmentManager fragmentManager = getSupportFragmentManager();
// Step 2: Begin a new FragmentTransaction
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
// Step 3: Create and add a new Fragment, BlueFragment is extended from Fragment
FragmentBlue fragment = FragmentBlue.newInstance("Hello", "World");
// Step 4: Add the Fragment to the layout, R.id.main_holder_blue is a FrameLayout
fragmentTransaction.add(R.id.main_holder_blue, fragment);
// Step 5: Commit the FragmentTransaction
fragmentTransaction.commit();
```

## Operations on Fragments (Các thao tác trên Fragment)

- `add()`: Thêm một Fragment vào Activity. Nếu một activity được khởi động lại vì một thay đổi cấu hình, thì các Fragment được thêm vào sẽ được khôi phục lại.

- `remove()`: Xóa một Fragment khỏi Activity. Fragment sẽ bị destroy nếu nó không được thêm vào back stack.

- `replace()`: Thay thế một Fragment bằng một Fragment khác. Fragment hiện tại sẽ bị destroy nếu nó không được thêm vào back stack.

- `hide()`: Ẩn một Fragment. Fragment sẽ không bị destroy và sẽ được hiển thị lại khi được gọi phương thức `show()`. 

- `attach()`: Gắn một Fragment vào Activity. Fragment sẽ không bị destroy và sẽ được hiển thị lại khi được gọi phương thức `detach()`. 

- `addToBackStack()`: Thêm một Fragment vào back stack. Nếu một Fragment được thêm vào back stack, khi nhấn nút back trên thiết bị, Fragment sẽ được hiển thị lại.