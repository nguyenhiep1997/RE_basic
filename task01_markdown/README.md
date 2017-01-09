# bài báo cáo về Markdown ( task 1)

> nguồn tham khảo [Markdown hướng dẫn](http://bit.ly/2cxplkv)

> người thực hiên : Nguyễn Văn Hiệp

> cập nhật lần cuối : **10 - 01 - 2017**
> -------
> ###mục lục 

 >[ 1. Tìm hiểu xem Markdown là gì? Dùng để làm gì?](#1)
>
> - [1.1. Markdown là gì?](#1.1)
> - [1.2. Dùng để làm gì?](#1.2)
>
>[ 2. Các cú pháp thường gặp.](#2)
>
> - [2.1. Thẻ tiêu đề](#2.1)
> - [2.2. Ký tự in đậm, in nghiêng](#2.2)
> - [2.3. Trích dẫn, bo chữ](#2.3)
> - [2.4. Gạch đầu dòng](#2.4)
> - [2.5. Chèn link, chèn ảnh](#2.5)
> - [2.6. Tạo bảng](#2.6)
>
>[3. Cài đặt Sublime Text và tìm kiếm, cài đặt các công cụ hỗ trợ markdown (export HTML, markdown preview, ...)](#3)
>
> - [3.1. Cài đặt Sunlinme Text](#3.1)
> - [3.2. Cài đặt Plugins](#3.2)
<ul>
<li> [3.2.1. Cài đặt Package Control](#3.2.1)</li>
<li> [3.2.2. Cài đặt Plugins](#3.2.2)</li>
</ul>
> - [3.3. Cài đặt công cụ hỗ trợ Markdown](#3.3)
<ul>
<li> [3.3.1 Export HTML](#3.3.1)</li>
<li> [3.3.2 Markdown Preview](#3.3.2)</li>
</ul>
>
[4. các công cụ Editor](#4)
> 
> - [4.1 Stackedit](#4.1)
> - [4.2 Jbt](#4.2)
> - [4.3 Tablesgenerator](#4.3)
> -----------------------------

###1. Tìm hiểu xem markdown là gì? Dùng để làm gì?<a name="1"></a>
####1.1 Tìm hiểu xem markdown là gì?<a name="1.1"></a>

**_Markdown_** là ngôn ngữ đánh dấu văn bản được tạo ra bởi John Gruber. Markdown sử dụng cú pháp khá đơn giản và dễ hiểu để đánh dấu văn bản và văn bản được viết bằng Markdown sẽ có thể được chuyển đổi sang HTML. Ngược lại các văn bản được viết bằng HTML cũng có thể được chuyển đổi sang Markdown.
####1.2  Dùng để làm gì?<a name="1.2"></a>

**_Markdown_** hiện tại còn được chính thức sử dụng trên github.com, stackoverflow.com, Atlassian Stash…: trong các file README.md, trong bất kỳ chỗ nào cần phải viết.

###2. Các cú pháp thường gặp.<a name="2"></a>

 - ####2.1 Thẻ tiêu đề<a name="2.1"></a>

**_Markdown_** sử dụng kí tự `#` để bắt đầu cho các thẻ tiêu đề, có thể dùng từ 1 đến 6 ký tự `#` liên tiếp. Mức độ riêu đề giảm dần từ 1 đến 6
VD: 

`#Tiêu đề cấp 1`
#Tiêu đề cấp 1

`##Tiêu đề cấp 2`
##Tiêu đề cấp 2
...
`######Tiêu đề cấp 6`
######Tiêu đề cấp 6

####2.2. Ký tự in đậm, in nghiêng<a name="2.2"></a>

 - Để in đậm một đoạn text bạn chỉ cần làm như sau:

`**từ cần in đậm**`

**từ cần in đậm**

 - Để in nghiên một đoạn text bạn chỉ cần làm như sau:

`*từ cần in nghiêng*`

*từ cần in nghiêng*

####2.3 Trích dẫn, bo chữ <a name="2.3"></a>

 - Để bo một đoạn text thì bạn chỉ cần sử dụng cú pháp sau:
 ```
 `đoạn cần bo`
 ```


Kết quả là: `đoạn cần bo`

 - Để làm nổi bật một đoạn:

![](http://i.imgur.com/lSnHaKc.png)

Kết quả là: 

```
title pgm_1:sample program
.model small
.stack 100h
.code
main  proc
    mov     ah,2 ;ham hien thi 1 ky tu
    mov     dl,'?'  ;ky tu la "?" 
    int     21h ;hien thi ky tu
;vao 1 ky tu
    mov     ah,1  ; ham doc 1 ky tu
    int     21h ;ky tu trong al
    mov     bl,al ;cat ky tu trong bl
;xuong dong moi:
    mov     ah,2 ;ham hien thi ki tu
    mov     dl, 0dh ;ve dau dong
    int     21h ; thuc hien ve dau dong
    mov     dl,0ah ;xuong dong
    int     21h ; thuc hien xuong dong
;hien thi ky tu:
    mov     dl,bl   ; lay ky tu            
    int     21h ;va hien thi no
   ;tro ve dos
   mov      ah,4ch  ;ham thoat ve dos
   int      21h ;thoat ve dos
main    endp
    end main
```

####2.4. Gạch đầu dòng<a name="2.4"></a>

Để sử dụng gạch đầu dòng bạn chỉ cần sử dụng cú pháp sau:
```
- Gạch đầu dòng thứ nhất
  <ul>
  <li>Thụt với đầu dòng 1</li>
  <li>Thụt với đầu dòng 1</li>
  </ul>
- Gạch đầu dòng thứ hai
  <ul>
  <li>Thụt với đầu dòng 2</li>
  <li>Thụt với đầu dòng 2</li>
  </ul>
```

Kết quả là : 

- Gạch đầu dòng thứ nhất
  <ul>
  <li>Thụt với đầu dòng 1</li>
  <li>Thụt với đầu dòng 1</li>
  </ul>
- Gạch đầu dòng thứ hai
  <ul>
  <li>Thụt với đầu dòng 2</li>
  <li>Thụt với đầu dòng 2</li>
  </ul>

####2.5. Chèn link, chèn ảnh<a name="2.5"></a>

Để chèn link vào file ta thực hiện  1 tron các cách sau: 

> cách 1

`![ghi chú](link image)`

Sử dụng trang imgur.com để lấy link image


VD: 

`![image](http://i.imgur.com/DtwzYPq.jpg)`

Kết quả:

![image](http://i.imgur.com/DtwzYPq.jpg)

> cách 2 
```
<img src="link_anh_cua_ban">
```

> cách lấy link y như trên
####2.6. Tạo bảng<a name="2.6"></a>

ta thể sử dụng cú pháp sau để tạo bảng:

```javascrpt
|        | Cột 1 | Cột 2| Cột 3 | Cột 4 |
|--------|-------|------|-------|-------|
| Hàng 1 | 1 x 1 | 1 x 2| 1 x 3 | 1 x 4 |
| Hàng 2 | 2 x 1 | 2 x 2| 2 x 3 | 2 x 4 |
| Hàng 3 | 3 x 1 | 3 x 2| 3 x 3 | 3 x 4 |
| Hàng 4 | 4 x 1 | 4 x 2| 4 x 3 | 4 x 4 |
```
Kết quả là: 

|        | Cột 1 | Cột 2| Cột 3 | Cột 4 |
|--------|-------|------|-------|-------|
| Hàng 1 | 1 x 1 | 1 x 2| 1 x 3 | 1 x 4 |
| Hàng 2 | 2 x 1 | 2 x 2| 2 x 3 | 2 x 4 |
| Hàng 3 | 3 x 1 | 3 x 2| 3 x 3 | 3 x 4 |
| Hàng 4 | 4 x 1 | 4 x 2| 4 x 3 | 4 x 4 |

** Mẹo: ** 

- Sử dụng trang http://www.tablesgenerator.com/markdown_tables paste vào đó đoạn markdown bạn viết và xem trước để chỉnh sửa cho phù hợp.
hoặc bạn có thể chạy ngay trên máy  


###3. Cài đặt Sublime Text và tìm kiếm, cài đặt các công cụ hỗ trợ markdown (export HTML, markdown preview, ...<a name="3"></a>

####3.1. Cài đặt Sunlinme Text<a name="3.1"></a>

 - Truy cập vào đường dẫn : https://www.sublimetext.com/3 để download. 

 - Sau đó cài đặt bình thường

####3.2. Cài đặt Plugins<a name="3.2"></a>
#####3.2.1. Cài đặt Package Control<a name="3.2.1"></a>
 - Truy cập vào links để lấy code https://packagecontrol.io/installation#st3 ứng với bản cài đặt

 - Mở Sublime Text, nhấn tổ hợp phím Ctrl+` và nhập đoạn code vừa lấy:
 - Khởi động lại Sublime Text

#####3.2.2. Cài đặt Plugins <a name="3.2.2"></a>

 - Vào Preferences > Package Control > Install Package > Nhập **Plugins**

####3.3. Cài đặt công cụ hỗ trợ Markdown<a name="3.3"></a>
#####3.3.1 Export HTML<a name="3.3.1"></a>

 - Vào Preferences > Package Control > Install Package > Nhập export HTML

![](http://i.imgur.com/IBN0FjT.png)

####3.3.2 Markdown Preview<a name="3.3.2"></a>
 - Vào Preferences > Package Control > Install Package > Nhập Markdown Preview

![](http://i.imgur.com/uWtMjzR.png)

### 4. các công cụ Editor:<a = name="4"></a>
> [stackedit](https://stackedit.io/editor)<a =name="4.1"></a>
> [Jbt](http://jbt.github.io/markdown-editor/)<a= name ="4.2"></a>
> [Tablesgenerator](http://www.tablesgenerator.com/markdown_tables)<a = name="4.3"></a>


