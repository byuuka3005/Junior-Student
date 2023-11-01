# Artifact (Tiếp theo)


## Cú pháp Binding khác

- Thay vì thông thường binding theo cú pháp
```xml
    <TextBlock Text="{Binding Name}" />
```
- Ta có thể thay đổi cú pháp binding như sau
```xml
    <TextBlock>
        <TextBlock.Text>
            <Binding Path="Name" />
        </TextBlock.Text>
    <TextBlock />
```

## Các chế độ thay đổi thuộc tính
- LostFocus: thay đổi thuộc tính khi control mất focus
- PropertyChanged: thay đổi thuộc tính ngay khi thuộc tính thay đổi

## AdornedElementPlaceholder

- Để hiển thị thông báo lỗi của validation, ta có thể sử dụng `AdornedElementPlaceholder` như sau
```xml
    <ControlTemplate x:Key="validationTemplate">
        <DockPanel>
            <TextBlock Foreground="Red" FontSize="20">!</TextBlock>
            <AdornedElementPlaceholder />
        </DockPanel>
    </ControlTemplate>
```

## Trigger
- Trigger giống như một sự kiện, được kích hoạt khi một điều kiện nào đó được thỏa mãn (như `if` trong lập trình)
- Ví dụ: khi `TextBox` có nội dung rỗng, thì `Button` sẽ bị vô hiệu hóa
```xml
<Window.Resources>
    <ControlTemplate x:Key="validationTemplate">
        <StackPanel Orientation="Horizontal">
            <TextBlock Foreground="Red" FontSize="20" Text="! "></TextBlock>
            <AdornedElementPlaceholder/>
        </StackPanel>
    </ControlTemplate>
    <Style x:Key="textBoxInError" TargetType="{x:Type TextBox}">
        <Style.Triggers>
            <Trigger Property="Validation.HasError" Value="true">
                <Setter Property="ToolTip"
                    Value="{Binding RelativeSource={x:Static RelativeSource.Self},
                    Path=(Validation.Errors)[0].ErrorContent}"/>
            </Trigger>
        </Style.Triggers>
    </Style>
</Window.Resources>

```

- Thêm vào binding rule
```xml
    <TextBox 
        x:Name="txtAge"
        Validation.ErrorTemplate="{StaticResource validationTemplate}"
        Style="{StaticResource textBoxInError}"> (Ở trên)
        <TextBox.Text> 
            <Binding Path="Age" UpdateSourceTrigger="PropertyChanged">
                <Binding.ValidationRules>
                    <local:AgeRangeRule Min="21" Max="130" />
                </Binding.ValidationRules>
            </Binding>
        </TextBox.Text>
```

