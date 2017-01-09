### lập trình hợp ngữ (assembly)

> tài liệu tham khảo : [assembly](http://bit.ly/2iryedL)
> 
> thực hiện bởi : Nguyễn Văn Hiệp
>
>cập nhật ngày 09 - 01 - 2017

----

###mục lục:

- [-1. cấu trúc chương trình](#muc1)
<ul>
<li>[1.1 trình biên dịch NASM](#muc1.1) 1</li>
<li>[1.2 trình biên dịch MASM](#muc1.2) 1</li>
</ul>
- [2. toán tử và các lệnh đơn giản](#muc2)
- [3. các lệnh cơ bản](#muc3)
- [4. lệnh và ngắt ra vào dòng nhập chuẩn](#muc4)
- [5. cài đặt emu8086](#muc5)

----

> #### tổng quan về assembly

> -Ngôn ngữ assembly (còn gọi là hợp ngữ) là một ngôn ngữ bậc thấp được dùng trong việc viết các chương trình máy tính. Ngôn ngữ assembly sử dụng các từ có tính gợi nhớ, các từ viết tắt để giúp ta dễ ghi nhớ các chỉ thị phức tạp và làm cho việc lập trình bằng assembly dễ dàng hơn. Mục đích của việc dùng các từ gợi nhớ là nhằm thay thế việc lập trình trực tiếp bằng ngôn ngữ máy được sử dụng trong các máy tính đầu tiên thường gặp nhiều lỗi và tốn thời gian. Một chương trình viết bằng ngôn ngữ assembly được dịch thành mã máy bằng một chương trình tiện ích được gọi là assembler (Một chương trình assembler khác với một trình biên dịch ở chỗ nó chuyển đổi mỗi lệnh của chương trình assembly thành một lệnh Các chương trình viết bằng ngôn ngữ assembly liên quan rất chặt chẽ đến kiến trúc của máy tính. Điều này khác với ngôn ngữ lập trình bậc cao, ít phụ thuộc vào phần cứng. 

> -Trước đây ngôn ngữ assembly được sử dụng khá nhiều nhưng ngày nay phạm vi sử dụng khá hẹp, chủ yếu trong việc thao tác trực tiếp với phần cứng hoặc làm các công việc không thường xuyên. Ngôn ngữ này thường được dùng cho trình điều khiển (tiếng Anh: driver), hệ nhúng bậc thấp (tiếng Anh: low-level embedded systems) và các hệ thời gian thực. Những ứng dụng này có ưu điểm là tốc độ xử lí các lệnh assembly nhanh.

> -Ngôn ngữ máy cực kỳ khó hiểu đối với lập trình viên vì thế họ không thể sử dụng trực tiếp ngôn ngữ máy để viết chương trình. Một sự trừu tượng khác là ngôn ngữ assembly. Nó cung cấp những tên dễ nhớ cho các lệnh và một ký hiệu dễ hiểu hơn cho dữ liệu. Bộ dịch được gọi là assembler chuyển ngôn ngữ assembly sang ngôn ngữ máy. Ngay cả những ngôn ngữ assembly cũng khó sử dụng. Những ngôn ngữ cấp cao như C++ cung cấp các ký hiệu thuận tiện hơn nhiều cho việc thi hành các giải thuật. Chúng giúp cho các lập trình viên không phải nghĩ nhiều về các thuật ngữ cấp thấp, và giúp họ chỉ tập trung vào giải thuật. Trình biên dịch (compiler) sẽ đảm nhiệm việc dịch chương trình viết bằng ngôn ngữ cấp cao sang ngôn ngữ assembly. Mã assembly được tạo ra bởi trình biên dịch sau đó sẽ được tập hợp lại để cho ra một chương trình có thể thực thi. Một ngôn ngữ bất kỳ từ ngôn ngữ giao tiếp của con người tới ngôn ngữ máy tính đều xây dựng trên một bộ ký tự. Các ký tự ghép lại thành các từ có nghĩa gọi là từ vựng. Các từ lại được viết thành các câu tuân theo cú pháp và ngữ pháp của ngôn ngữ để diễn tả hành động sự việc cần thực hiện. Bộ ký tự của Assembly gồm có:

> -Các chữ cái latin: 26 chữ hoa A-Z, 26 chữ thường a-z. - Các chữ số thập phân: ‘0’ - ‘9’ - Các ký hiệu phép toán, các dấu chấm câu và các ký hiệu đặc biệt: + - * / @ ? $,.: < > { } & %! \ # v.v... - Các ký tự ngăn cách: space và tab.

<a name="muc1"></a>

### 1. cấu trúc chương trình (NASM và MASM)

<a name="muc1.1"></a>

> ####1.1- cấu trúc trong NASM:
> một chương trình assembly có thể được chia thành 3 section ( phân đoạn): data section, Bss section và text section 

> ###### -phân đoạn data:
> Được sử dụng để khai báo các dữ liệu khởi tạo và các hằng của chương trình. Dữ liệu ở đây sẽ không được thay đổi khi chạy chương trình.
> Chúng ta có thể khai báo giá trị hằng, tên file, kích thước buffer,… tại đây.
> cú pháp :
```
.DATA
```
> section này thường được gọi là **data segment**
> ###### - phân đoạn Bss:
> Được sử dụng để khai báo các biến sử dụng trong chương trình.
> có cú pháp là:
```
.Bss 
```
> ###### -phân đoan text
>Được sử dụng để giữ mã lệnh (code) của chương trình. Phân đoạn này phải bắt đầu với khai báo **global _start,** để báo với hệ điều hành (Kernel) vị trí bắt đầu của chương trình là tại đây.
> cú pháp :
```
.text
global_start
_start:
```
> section này được gọi là **code segment**.
<a name="muc1.2"></a>
> #### cấu trúc trong MASM
> các chương trình bằng ngôn ngữ máy gồm có mã, dữ liệu và ngăn xếp. mỗi phần chiếm lấy một đoạn bộ nhớ. chương trình bằng hợp ngữ củng có tổ chức như vậy, trong trường hợp này, mã dữ liệu và ngăn xếp được cấu trúc như các đoạn chương trình. mỗi đoạn chương trình sẽ được dịch thành một đoạn bộ nhớ bởi trình biên dịch.
>chúng ta sửu dụng các định nghĩa đơn giản hóa mà đã được dùng cho MASM 5.0
> **các chế độ bộ nhớ **.
> kích thước của đoạn mã và dữ liệu trong một chương trình có thể được xác định bằng cách chỉ ra chế độ nhớ nhờ sử dụng dẫn hướng biên dịch .MODE
> cú pháp:
```
.MODEL   kiểu_bộ_nhớ
```
> các chế độ thường sữ dụng nhất là **SMALL**, **MEDIUM**, **COMPACT** VÀ **LARGE**. trừ khi có quá nhiều mã lệnh hay số liệu, kiểu thích hợp nhất là SMALL. dẫn hướng biên dịch **.MODEL** phải được đưa vào trước bất kỳ một định nghĩa nào.
> ** đọa dữ liệu (Data segment)**.
> đoạn dữ liệu của một chương trình chứa tất cả các định nghĩa biến, cũng như vậy định nghĩa hằng củng thường được tạo ra ở đây nhưng chũng cũng có thể được đưa vào một chổ khác quan trong chương trình bởi vì không có ô chứa nào liên quan đến nó. để khai báo một đoạn dữ liệu chúng ta sử dụng dẫn hướng biên dịch **.DATA**. theo sau là các khai báo biến hay hằng. ví dụ:
```
.DATA
WORD1  DW  2
WORD2  DW  5
MSG  DW 'THIS IS A MESSAGE'
MASK  EQU  10010010B
```
> **đoạn ngăn xếp (Stack segment)**
> mục đích của khai báo đoạn ngăn xếp là tạo ra một khối bộ nhớ ( vùng ngăn xếp) để chứa ngăn xếp. vùng ngưn xếp có thể đủ lớn để chứa ngăn xếp với kích thước lớn nhất của nó. cú pháp khai báo như sau:
```
.STACK  100H
```
> sẽ tạo 100h byte cho vùng ngăn xếp 
>** đoạn mã (code segment)**
> đoạn mã chứa các lệnh của chương trình cú pháp khai báo là:
```
.CODE  TÊN	
```
> trong đó tên là một tên tùy ý ( không nhất thiết phải có )
. bên trong doạn mã lênh đuwọc tổ chức như các thủ tục. một định nghĩa thủ tục đơn giản nhất là :
```
tên_thủ_tục    PROC
;thân của thủ tục
tên_thủ_tục    ENDP
```
>ở đây tên thủ tục là tên của thủ tục ; PROC và ENDP là các toán tử đánh dấu bắt đầu và kết thúc thủ tục.
> ví dụ 1 đoạn mã:
```
.CODE
MAIN  PROC
;các lệnh của chương trình chính 
MAIN   ENDP
; các thủ tục khác 
```

<a name="muc2"></a>
----
> ### 2. các phép toán , biểu thức  
> các phép toán trong assembly gồm có :
> add, mov, sub, dec, inc, equ.... 
> các phép toán được thực hiện bằng các lệnh chỉ thị chứ không như các ngôn ngữ bậc cao 
> biểu thức toán học là tập hợp các phép toán nên nó củng là tập hợp các lệnh xử lý của máy 
> ví dụ cho việc dùng biểu thức A= B-2*A
```
MOV  AX,B  ;truyền giá trị của B vào AX
SUB  AX,A  ; AX mang giá trị của B_A
SUB  AX,A
MOV  A,AX  : trả lại kết quả cho A
```
> ví dụ về việc dùng câu lênh gán :
``` 
LF   EQU   0ah
```
> sẽ gán tên LF cho 0ah là mã ascii của ký tự xuống dòng . tên LF bây giờ có thể thay thế cho oah tại bất cứ đâu trong chươg trình.
> với câu lệnh gán A=B trong C thì trong asembly cần chỉ thị như sau
```
mov  ax,a ; chuyển a vào ax
mov  b,ax ; chuyển giá trị của ax vào b
```

<a name="muc3"></a>
----
> ### -3 các lệnh cơ bản
> một chương trình gồm một tập các dòng lệnh , dòng lệnh là một chỉ thị để cho máy hoạt động. mỗi lệnh thường có 4 trường.
> **Tên**  ,  **Toán tử** ,  **Toán hạng** ,  *lời bình**
> ví dụ:
``` 
start:  mov   cx,5 ;khởi tạo bộ đếm 
```
> trong trường này trường tên là   nhãn start , toán tử là mov , toán hạng là cx và 5 , lời bình là " khởi tạo bộ đếm"
>** dữ liệu chương trình **
> các số nhị phân được viết như là một chuỗi các bit kết thúc bằng chữ 'B' ví dụ: 1011b.
> các số thập phân là chuỗi các số và kết thúc bằng 'd' hay không viết gì cả 
> các số hệ 16 ( hex ) kết thúc bằng chữ 'h' và bắt buộc phải bắt đầu bằng 1 số thập phân.
>** các toán tử giả định nghĩa số liệu **
> DW   =    kiểu dữ liệu byte
> DW   =    kiểu dữ liệu word
> DD   =    định nghĩa từ kép 
> DQ   =    định nghĩa 4 word ( 4 từ liên tiếp )
> DT   =    định nghĩa 10 byte liên tiếp 

> ####Lệnh EQU (coi như , bằng)
> Dùng để gán giá trị của hằng cho 1 cái tên nào đấy 
> Cú pháp :  
```
Tên 	EQU 	hằng _số 
```
> ví dụ 
```
AX 	EQU 	10
```
> Ý nghĩa : sẽ gán cho AX giá trị bằng 10.

> #### -lệnh XCHG 
> dùng để hoán vị hai biến :
> câu lệnh :
```
XCHG đích,nguồn
```
>VD:
```
 XHCG	 AX,BY
```
>  sẽ lấy giá trị của AX để chuyển cho BY và giá trị của BY cho AX

> ####-Lệnh MOV
> Câu lệnh :
```
 mov đích,nguồn
```  
> Dùng để tạo ra một bản sao giá trị của biến câu lệnh của nó là:
```
 mov	 AX,BY 
```
> ý nghĩa gán giá trị của BY cho AX và giá trị của BY được giữu nguyên , từ nay khi viết AX  hoặc BY sẽ có chung nghĩa

> ####-lệnh ADD
> Dùng để cộng nội dung của hai thanh ghi, một thanh ghi và một ô nhớ hoặc cộng 1 số vào một thanh ghi hay một ô nhớ. Cú pháp 
```
ADD 	đích, nguồn
```
> VD add 		AX,BY 
> Thao tác này sẽ cộng giá trị của thanh ghi BY vào giá trị của thanh ghi AX và chứ nội dung tổng đó trong thanh ghi AX còn giá trị của BY được giữ nguyên.
```
ADD 	AX,5
```
> Sẽ cộng 5 vào giá trị của AX

> ####-lệnh SUB
> Ngược lại với add nó dùng để trừ nội dung của hai thành ghi , một thanh ghi và một ô nhớ hoặc một số vào một thanh ghi hay một ô nhớ. Cú pháp 
```
SUB 	đích, nguồn 
```
> VD
```
SUB		AX ,BY
```
> Sẽ lấy giá trị AX-BY và gán giá trị đó cho AX , BY được giữ nguyên
> **Chú ý: việc kết hợp MOD, XCHG, ADD, SUB  trực tiếp giữa các ô nhớ là không hợp lệ**

> ####-Lệnh INC và DEC 
>Lệnh INC (increment) dùng để cộng 1 vào giá trị của 1 thanh ghi hay một ô nhớ
> Lệnh DEC(decrement) dùng để trừ đi 1 giá trị của một thanh ghi hay một ô nhớ
> Cú pháp :
```
INC 	đích
DEC 	đích
```
> VD INC 	word1
> (Cộng 1 vào nội dung của word1)
> DEC 	word1
> (trừ giá trị của word1 đi 1)

<a name="muc4"></a>
--------
> ### Lệnh và ngắt ra vào dòng nhập chuẩn
> có 2 loại chương trình phục vụ vào/ra . các chương trình của DOS và BIOS. các chương trình BIOS được chứa trong ROM và tác động trực tiếp tới các cổng vào/ra. 
>
> **lệnh INT**
> lệnh INT được dùng để gọi các chương trình ngắt của DOS và BIOS. nó có dạng sau:
```
INT  số_hiệu_ngắt
```
> ở đây số hiệu ngắt là một con số xác định một chương trình ví dụ INT 16, sẽ gọi các phụ vụ bàn phím của BIOS.

> **ngăt 21H**
> ngắt 24 H được dùng để gọi rát nhiều hàm của DOS . mỗi hàm được gọi bằng cách đặt số hàm vào trong thanh ghi AH và gọi INT 21H. chúng ta xét các hàm.
> 1 : vào 1 phím 
> 2 : đưa ra màn hình 
> 9 : đưa ra một chuỗi ký tự
> các hàm của ngắt 21H nhận dữ liệu trong các thanh ghi nào đó và trả về kết quả trong các thanh ghi khác. các thanh ghi này sẽ được liệt kê khi mô tả mỗi hàm .
> **hàm 1 **
> **vào một phím**.

> vào : AH=1
> ra: AL= mã ascii nếu một phím bất kỳ được ấn .
>       = 0 nếu một phím điều khiển hay chức năng được nhấn 

> để gọi phục vụ ta thực hiện các lệnh sau:
```
mov   AH,1   ;hàm vào một phím
INT   21H    ; mã ascii trong AL
```
bộ vi xử lý sẽ đợi người sử dụng ấn một phím nếu cần thiết. nếu một phím kí tự được ấn ,AL sẽ nhận mã ASCii và ký tự được hiện lên màn hình . trong các lệnh tiếp theo INT 21H có thể kiểm tra AL và thực hiện tác vụ thích hợp.
>
> **hàm 2**
> **hiển thị 1 ký tự hay thi hành 1 chức năng điều khiển**.

> vào:  AH=2
>       DL= mã ASCII của ký tự hiển thị hay ký tự điều khiển.
> ra:  	AL=mã ASCII của ký tự hiển thị hay ký tự điều khiển .
> dùng hàm này hiển thị một ký tự. ta đặt mã ASC của nó trong DL . ví dụ các lệnh sau đây sẽ làm xuất hiện dấu chấm hỏi '?' trên màn hình :
```
MOV   AH,2
MOV   DK,'?'
INT   21H
```
> các ký tự điều khiển quan trọng được chỉ ra sau đây

|mã ASCII | ký hiệu | chức năng |
|---------|---------|-----------|
| 7        |    BELL |     phát tiếng bíp (beep)| 
| 8	      |BS       | lùi lại 1 vị trí|
| 9 | HT | tab |
| A | LF | xuống dòng |
| D | CR | xuống dòng và về đầu dòng|

<a name="muc5"></a>
> ### cài đặt IDE emu8086 và chạy các ví dụ
> link để tải IDE emu8086[ IDE](http://www.emu8086.com/)
> sau khi tải xong giải nén và chạy 
![hình ảnh 1](http://i.imgur.com/YoVn4FE.png)
![hình ảnh 2](http://i.imgur.com/AGtSf5o.png)
![hình ảnh 3](http://i.imgur.com/7zj1ve1.png)
![hình ảnh 4](http://i.imgur.com/ZuMtuea.png)
> ví dụ 1 được code như sau
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
> và kết quả là 
![hình ảnh 1](http://i.imgur.com/jayGYqY.png)
