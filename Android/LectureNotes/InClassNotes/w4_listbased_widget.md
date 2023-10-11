# List-based Widgets

## ListView
- là một widget hiển thị danh sách các item có thể scroll

### ArrayAdapter
- là một adapter dùng để cung cấp dữ liệu cho ListView
- ArrayAdapter sử dụng một mảng các đối tượng để cung cấp dữ liệu cho ListView
- ArrayAdapter sử dụng một layout mặc định để hiển thị dữ liệu cho ListView
- ArrayAdapter sử dụng một TextView mặc định để hiển thị dữ liệu cho ListView
- Đầu vào gồm 3 tham số: context, layout, data
```java
ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, 
 android.R.layout.simple_list_item_1, 
 data);
```
### Sử dụng ArrayAdapter với Activity
```xml
<ListView
 android:id="@+id/listView"
 android:layout_width="match_parent"
 android:layout_height="match_parent" />
```
```java
public class MainActivity extends Activity {
    ListView listView;
    String[] data = {"Android", "iOS", "Windows Phone", "Blackberry"};
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        listView = (ListView) findViewById(R.id.listView);
        // step 1: set click listener for list view
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(MainActivity.this, data[position], Toast.LENGTH_SHORT).show();
            }
        });
        // step 2: create adapter
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, 
         android.R.layout.simple_list_item_1, 
         data);
        // step 3: set adapter for list view
        listView.setAdapter(adapter);
    }
}
```
- Có thể tuỳ chỉnh layout cho từng item trong ListView
```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
 android:id="@+id/textView"
 android:layout_width="match_parent"
 android:layout_height="wrap_content"
 android:padding="10dp"
 android:textSize="20sp" />
```

## Spinner (ComboBox)

- Ví dụ của Spinner tương tự như ListView nhưng khác sự kiện click

### Sử dụng Spinner
```xml
<Spinner
 android:id="@+id/spinner"
 android:layout_width="match_parent"
 android:layout_height="wrap_content" />
```
```java
public class MainActivity implements AdepterView.OnItemSelectedListener {
    Spinner spinner;
    String[] data = {"Android", "iOS", "Windows Phone", "Blackberry"};
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        spinner = (Spinner) findViewById(R.id.spinner);
        // step 1: set click listener for spinner
        spinner.setOnItemSelectedListener(this);
        // step 2: create adapter
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, 
         android.R.layout.simple_spinner_item, 
         data);
        // step 3: set adapter for spinner
        spinner.setAdapter(adapter);
    }
    @Override
    public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
        Toast.makeText(MainActivity.this, data[position], Toast.LENGTH_SHORT).show();
    }
    @Override
    public void onNothingSelected(AdapterView<?> parent) {
    }
}
```

## GridView

- GridView là một widget hiển thị danh sách các item có thể scroll theo chiều ngang và chiều dọc

### Sử dụng GridView
```xml
<GridView
 android:id="@+id/gridView"
 android:layout_width="match_parent"
 android:layout_height="match_parent"
 android:numColumns="2" />
```
```java
public class MainActivity extends Activity {
    GridView gridView;
    String[] data = {"Android", "iOS", "Windows Phone", "Blackberry"};
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        gridView = (GridView) findViewById(R.id.gridView);
        // step 1: set click listener for grid view
        gridView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Toast.makeText(MainActivity.this, data[position], Toast.LENGTH_SHORT).show();
            }
        });
        // step 2: create adapter
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, 
         android.R.layout.simple_list_item_1, 
         data);
        // step 3: set adapter for grid view
        gridView.setAdapter(adapter);
    }
}
```

## AutoCompleteTextView

- Phù hợp với các app từ điển

### Sử dụng AutoCompleteTextView
```xml
<AutoCompleteTextView
 android:id="@+id/autoCompleteTextView"
 android:layout_width="match_parent"
 android:layout_height="wrap_content" />
```
```java
public class MainActivity extends Activity implements TextWatcher {
    AutoCompleteTextView autoCompleteTextView;
    String[] data = {"Android", "iOS", "Windows Phone", "Blackberry"};
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        autoCompleteTextView = (AutoCompleteTextView) findViewById(R.id.autoCompleteTextView);
        // step 1: create adapter
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, 
         android.R.layout.simple_list_item_1, 
         data);
        // step 2: set adapter for auto complete text view
        autoCompleteTextView.setAdapter(adapter);
    }
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after) {
    }
    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
    }
    @Override
    public void afterTextChanged(Editable s) {
        Toast.makeText(MainActivity.this, s.toString(), Toast.LENGTH_SHORT).show();
    }
}
```

## HorizontalScrollView
- HorizontalScrollView là một widget hiển thị danh sách các item có thể scroll theo chiều ngang

### Sử dụng HorizontalScrollView
```xml
<HorizontalScrollView
    android:id="@+id/horizontalScrollView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
 <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button 1" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button 2" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button 3" />
 </LinearLayout>
</HorizontalScrollView>
```
