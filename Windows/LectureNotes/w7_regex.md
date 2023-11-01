# Biểu thức chính quy (Regular Expression)

## Các qui ước về kí hiệu

| Kí hiệu | Ý nghĩa | Ví dụ | 
|---------|---------|-------|
| `.` | Bất kì kí tự nào | `a.c` khớp chuỗi `abc`, `adc`, `aec`, ... |
| `*` | Lặp lại 0 hoặc nhiều lần | `a*c` khớp chuỗi `ac`, `aac`, `aaac`, ... |
| `+` | Lặp lại 1 hoặc nhiều lần | `a+c` khớp chuỗi `ac`, `aac`, `aaac`, ... |
| `?` | Lặp lại 0 hoặc 1 lần | `a?c` khớp chuỗi `ac`, `c` |
| `{n}` | Lặp lại đúng n lần | `a{3}c` khớp chuỗi `aaac` |
| `{n,}` | Lặp lại ít nhất n lần | `a{3,}c` khớp chuỗi `aaac`, `aaaac`, ... |
| `{n,m}` | Lặp lại ít nhất n lần và nhiều nhất m lần | `a{3,5}c` khớp chuỗi `aaac`, `aaaac`, `aaaaac` |
| `^` | Bắt đầu chuỗi | `^abc` khớp chuỗi `abc`, `abcd`, `abcxyz`, ... |
| `$` | Kết thúc chuỗi | `abc$` khớp chuỗi `abc`, `xabc`, `yabc`, ... |
| `[]` | Kí tự trong ngoặc vuông | `[abc]` khớp chuỗi `a`, `b`, `c` |
| `[^]` | Kí tự không nằm trong ngoặc vuông | `[^abc]` khớp chuỗi `d`, `e`, `f`, ... |
| `\` | Kí tự đặc biệt | `\*`, `\+`, `\?`, `\{`, `\}`, `\(`, `\)` |
| `\d` | Kí tự số | `\d` khớp `0`, `1`, `2`, ... |
| `\D` | Kí tự không phải số | `\D` khớp `a`, `b`, `c`, ... |
| `\w` | Kí tự chữ cái, số, gạch dưới | `\w` khớp `a`, `b`, `c`, ..., `0`, `1`, `2`, ..., `_` |
| `\W` | Kí tự không phải chữ cái, số, gạch dưới | `\W` khớp `!`, `@`, `#`, ... |
| `\s` | Kí tự khoảng trắng | `\s` khớp ` `, `\t`, `\n`, `\r` |
| `\S` | Kí tự không phải khoảng trắng | `\S` khớp `a`, `b`, `c`, ... |
| `\b` | Kí tự biên giới | `\babc\b` khớp `abc` trong chuỗi `abc xyz`, `abc-xyz`, `abc.xyz`, ... |
| `\B` | Kí tự không phải biên giới | `\Babc\B` khớp `abc` trong chuỗi `abcxyz`, `xabc`, `yabc`, ... |
| `\A` | Bắt đầu chuỗi | `\Aabc` khớp `abc` trong chuỗi `abc xyz`, `abc-xyz`, `abc.xyz`, ... |
| `\Z` | Kết thúc chuỗi | `abc\Z` khớp `abc` trong chuỗi `xyz abc`, `xyz-abc`, `xyz.abc`, ... |
| `\n` | Kí tự xuống dòng | `\n` khớp kí tự xuống dòng |
| `\t` | Kí tự tab | `\t` khớp kí tự tab |
| `\r` | Kí tự carriage return | `\r` khớp kí tự carriage return |
