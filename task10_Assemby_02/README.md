#báo cáo 
##lập trình hợp ngữ (assembly)

>	Tài liệu tham khảo : [assembly](http://bit.ly/2iZ45FN)

>	Thực hiện:  Nguyễn Văn HIệp

>	Cập nhật lần cuối: 12/01/2017
> ---------------------

###mục lục
>
[1. thanh ghi cờ](#1)
 - [1.1 tổng quan](#1.1)
 - [1.2 thanh ghi cờ](#1.2)
 <ul>
 <li>[1.2.1 cờ trạng thái](#1.2.1)</li>
 <li>[1.2.2 cờ nhớ](#1.2.2)</li>
 <li>[1.2.3 cờ chẵn lẽ](#1.2.3)</li>
 <li>[1.2.4 cờ nhớ phụ](#1.2.4)</li>
 <li>[1.2.5 cờ zero](#1.2.5)</li>
 <li>[1.2.6 cờ dấu](#1.2.6)</li>
 <li>[1.2.7 cờ tràn](#1.2.7)</li>
 </ul>
[2. hiện tượng tràn](#2)<ul>
 <li>[2.1 thế nào là hiện tượng tràn](#2.1)</li>
  <li>[2.2 các ví dụ về hiện tượng tràn](#2.2)</li>
  <li>[2.3 CPU khi có hiện tượng tràn xảy ra](#2.3)</li>
  <li>[2.4 tràn không dấu và tràn có dấu](#2.4)</li>
  </ul>
[3. sự ảnh hưởng của các lệnh đến các cờ](#3)

>--------------------

###1. thanh ghi cờ <a name="1"></a>

####  1.1 tổng quan <a name="1.1"></a>

 - một điểm quan trọng giữa máy tính và các loại máy khác đó là máy tính có khả năng quyết định. các mạch trong máy tính có khả năng thực hiện những quyết định đơn giản dựa trên trạng thái hiện tại của bộ vi xử lý. Đối với bộ vi xử lý 8086, trnagj thái của bộ vi xử lý được thể hiện trong 9 bit riêng biệt gọi là các cờ. Mọi quyết định của bộ vi xử lý đều dựa trên giá trị của các cờ này.
 - Các cờ đặt trong thanh ghi cờ và chúng được phân chia thành 2 loại cờ trạng thái và cờ điều kiển. cờ trạng thái phản ánh kết quả của phép tính. 

####1.2 thanh ghi cờ <a name="1.2"></a>

|15|14|13|12|11|10| 9| 8| 7| 6| 5| 4| 3| 2| 1| 0|
|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|--|
|  |  |  |  |OF|DF|LF|TF|SF|ZF|  |AF|  |PF|  |CF|
 - bản trên cho ta thấy cấu trúc của các thành ghi cờ. các cờ trạng thái nằm ở các bit 0, 2, 4, 6, 7 và 11 còn các cờ điều khiển nằm ở các bit 8, 9 và 10. Các bit khác không có ý nghĩa. chú ý rằng không cần nhớ cờ nào nằm ở bit nào .Bảng trên trình bày tên các cờ và ký hiệu của chúng. trong chương này chúng ta tập trung vào các cờ trạng thái

####1.2.1 các cờ trạng thái <a name="1.2.1"></a>
 - bộ xử lý sử dụng cờ trạng thái để phản ánh kết quả của một phép tính chẳng hạn khi lệnh SUB 	AX,AX  được thực hiện cờ ZF sẽ được thiết lập 1 nhờ vậy nó chỉ ra rằng kết quả bằng 0 đã được tạo ra. bây giờ chúng ta sẽ tìm hiểu kỹ về các cờ trạng thái .

####1.2.2 cờ nhớ (carry flag - CF)<a name="1.2.2"></a>

 - cờ nhớ CF được thiết lập 1 khi có nhớ từ bit MSB trong phép cộng hay có vay vào bit MSB trong phép trừ. ngược lại nó bằng 0. cờ CF cũng bị ảnh hưởng bởi các lệnh quay và dịch  

####1.2.3 cờ chẵn lẽ (Parity Flag - PF)<a name="1.2.3"></a>

 - cờ PF được thiết lập 1 nếu byte thấp của kết quả có số chẵn các bit 1(parity chẵn). nó bằng 0 nếu byte thấp có số lẽ bit 1 (parity lẻ). ví dụ kết quả của một phép cộng các word là FFFEh, chuyển sang hệ nhị phân là 1111111111111110 như vậy byte thấp có 7 bit 1 do đó PF=0

####1.2.4 cờ nhớ phụ (Auxililary Flag - AF)<a name="1.2.4"></a>

 - cờ AF được thiết lập 1 nếu có nhớ từ bit 3 trong phép cộng hoặc có vay vào bit 3 trong phep trừ. cờ À được sữ dụng trong các thao tác với số thập phân mã hóa nhị phân (số BCD).

####1.2.5 cờ zero( Zero Flag - ZF)<a name="1.2.5"></a>

 - cờ ZF được thiết lập 1 khi kết quả bằng 0 và nếu khác 0 thì được thiết lập là 0

####1.2.6 cờ dấu (Sign Flag - SF)<a name="1.2.6"></a>

 - cờ SF được thiết lập 1 khi bit của msb của kết quả bằng 1 có nghĩa là kết quả là âm nếu bạn làm việc với số có dấu. ngược lại SF=0 nếu bit msb của kết quả bằng 0.

####1.2.7 cờ tràn (overlow Flag OF)<a name="1.2.7"></a>

 - cờ OF được thiết lập 1 khi xảy ra tràn ngược lại nó bằng 0 , khái niệm tràn sẽ được trình bày như sau:

 |cờ trạng thái    |
 |-------------|-|-|
 |Bit|tên gọi|ký hiệu|
 |0|cờ nhớ |CF|
 |2|cờ chẵn lẽ|PF|
 |4|cờ nhớ phụ|AF|
 |6|cờ ZERO|ZF|
 |7|cờ dấu|SF|
 |11|cờ tràn|OF|

 |cờ điều khiển    |
 |-------------|-|-|
 |Bit| tên gọi| ký hiệu|
 |8| cờ bẫy| TF|
 |9| cờ ngắt |IF|
 |10| cờ định hướng |DF|

###2. hiện tượng tràn <a name="2"></a>

####2.1 thế nào là hiện tượng tràn <a name="2.1"></a>

 - hiện tượng tràn gắn liền với một sự thật là phạm vi của các số biểu diễn trong máy tính có giới hạn.

 - phạm vi của số thập phân có dấu có thể biểu diễn bằng 1 word 16bit là từ -32768 đến 32767, với một byte 8 bit thì phạm vi là từ -128 đến 127.

 -đối với các số không dấu thì phạm vi từ 0 đến 65535 cho một word và từ 0 đến 255 cho môt byte. nếu kết quả của phép tính nằm ngoài phạm vi này thì hiện tượng tràn sẽ xảy ra và kết quả nhận được bị cắt bớt sẽ không phải là kết quả đúng.

####2.2 một số ví dụ về hiện tượng tràn <a name="2.2"></a>

 - sự tràn có dấu và tràn không dấu là các hiện tượng độc lập nhau. khi chúng ta thựchiện một thao tác số học như cộng hay trừ, có 4 khả năng xảy ra là: (1) không tràn, (2) chỉ tràn có dấu, (3) chỉ tràn không dấu, (4) tràn cả không dấu và có dấu.

  - ví dụ về hiện tượng tràn không dấu nhưng không tràn có dấu:
  giả sử AX chứ FFFh, BX chứ 0001h và lệnh ADD  AX,BX được thực hiện , kết quả ở dạng nhị phân như sau.
  ``` 
   1111 1111 1111 1111
  +
   0000 0000 0000 0001
   ___________________
 1 0000 0000 0000 0000
 ```

 - nếu chúng ta làm việc với số không dấu, kết quả đúng phải là 100000h =65536, nhưng  kết quả này nằm ngoài phạm vi biểu diễn của một word nên kết quả còn lại trong thành ghi AX là 0h, đây là một kết quả sai, như vậy hiện tượng tràn không dấu đã xãy ra. nhưng kết quả nhận được lại đúng với số có dấu, FFFh=-1 khi hiểu là số có dấu, trong khi 0001h =1 vậy tổng của chúng bằng 0, rõ ràng hiện tượng tràn có dấu đã không xảy ra.
 - bây giờ chúng ta xem xét một ví dụ về hiện tượng tràn có dấu nhưng lại không trang không dấu. giả sử AX và BXX cùng chứa 7FFFh, hãy thực hiện lệnh ADD AX,BX 
 - kết quả ở dạng nhị phân như sau:
 ```
  0111 1111 1111 1111
 +
  0111 1111 1111 1111
  ___________________ 
  1111 1111 1111 1111
 =FFFEh
 ```
 - trong cả 2 dạng có dấu và không dấu 7FFFh đều bằn 32767 bởi vậy trong cả 2 phép cộng không dấu và có dấu đều cho kết quả là 32767+32767 =65534 . giá trị này nằm ngoài phạm vi của số có dấu , kết quả nhận được có dạng FFFEh =-2 như vậy tràn có dấu đã xảy ra. tuy nhiên FFFEh lại bằng 65534 ở dạng không dấu do vậy không có hiện tượng tràn không dấu.

####2.3 CPU khi có hiện tượng tràn xảy ra <a name="2.3"></a>

 - bộ xử lý lập cờ OF =1 đẻ báo có hiện tượng tràn có dấu và CF =1 để báo có hiện tượng tràn không dấu. thông báo này dùng cho chương trình để tiến hành những hành động thích hợp, nếu như không có hành động nào được thực hiện ngay lập tức thì kết quả của lệnh sau có thể xoácờ báo tràn.
 - khi xác định có hiện tượng tràn bộ vi xử lý kgông coi kết quả như là số không dấu củng như số có dấu, thay vào đó nó sử dụng 2 cách hiểu có dấu và không dấu cho mỗi thao tác đồng thời thiết lập các cờ CF hay OF báo tràn có dấu hay không dấu một các tương ứng.
 trách nhiệm thuộc về người lập trình, người có quy ước về kết quả. nếu anh ta đang làm việc với số có dấy thì chỉ có cờ OF đáng quan tâm trong khi cờ CF có thể bỏ qua, ngược lại khi làm việc với số không dấu thì cờ quan trọng là CF chứ không phải OF.

####2.4 tràn không dấu và có dấu <a name="2.4"></a>

** hiện tượng tràn không dấu**
 - khi thực hiện phép cộng, hiện tượng tràn xảy ra khi có nhớ từ bit msb. có nghĩa là kết quả đúng của phéo tình lớn hơn số không dấu lớn nhất có thể biểu diễn, đó là FFFFh cho 1 word và FFh cho 1  byte. khi thực hiện phép trừ hiện tượng tràn không dấu xản ra khi có sự vay vào bit msb, có nghĩa là kết quả đúng nhỏ hơn 0

**hiện tượng tràn có dấu**
 - trong phép cộng các số cùng dấu hiện tượng tràn có dấu xảy ra khi tổng nhận được có dấu khác với dấu của 2 số hạng. điều này đã xảy ra trong các vd trước . hiện tượng tràn xảy ra khi kết quả có dấu khác với kết quả chúng ta chờ đợi.
 - thực ra bộ vi xử lý sử dụng phương pháp sau để thiết lập OF:
 nếu việc nhớ vào bit msb và việc nhớ ra từ nó không đồng thời, có nghĩa là có nhớ vào msb nhưng không có nhớ ra từ nó, hay ngược lạ thì có hiện tượng tràn và OF được thiết lập 1

###3. sự ảnh hưởng của các lệnh đến các cờ <a name="3"></a>
 - nhìn chung mỗi khi bộ xử lý thực hiện một lệnh, các cờ được thay đổi để phản ánh kết quả. tuy nhiên có một số lệnh không ảnh hưởng tới cờ, chỉ ảnh hướng tới một số trong chúng hay có thể làm cho chúng không xác định.trở lại với 7 lệnh cơ bản. chúng có ảnh hưởng đến các cờ như sau:

|chỉ thị| các cờ bị ảnh hưởng|
|-------|--------------------|
|MOV/XCHG| không cờ nào |
|ADD/SUB | tất cả các cờ |
|INC/DEC | tất cả các cờ trừ cờ CF|
|NEG    | tất cả các cờ (CF=1 trừ khi kết quả là 0, OF =1 nếu toán hạng word là 80000h hay toán hạng byte là 80h)|

 - để làm quen với việc các lệnh ảnh hưởng như thế nào đến các cờ, chúng ta sẽ làm một vài ví dụ. trong mỗi ví dụ chúng ta sẽ trình bày một lệnh, nội dung của các toán hạng và dự đoán kết quả củng như việc thiết lập các cờ CF, PF , ZF và OF ( bỏ qua AF vì nó chỉ thực hiện với các phép toán số học)

**ví dụ 1** ; thực hiện phép công AX ,BX trong đó AX, BX đều chứa FFFFh
 **trả lời :**
```  
   FFFFh
+  FFFFh
---------
  1FFFEh
```
kết quả thu được chứa trong AX là FFFFEh =1111 1111 1111 1110.
 - SF = 1 vì msb =1
 - PF = 0 vì có 7 bit 1 (lẻ bit 1) trong byte thấp của kết quả
 - ZF = 0 vì kết quả thu được khác 0
 - CF = 1 vì có nhớ từ bit msb trong phép cộng
 - OF = 0 vì dấu của tổng nhận được giống như dấu của các số hạng tham gia phép cộng (còn khi thực hiện phép cộng dưới dạng nhị phân bạn sẽ thấy có nhớ vào bit msb)

**ví dụ 2:** thực hiện phép cộng AL,BL cùng chứa 80h
 - **trả lời :**
 ```
   80
   80
   __
  100
 ```
 - Kết quả nhận được trong AL bằng 0

 - SF = 0 vì msb = 0
 - PF = 1 vì tất cả các bit của tổng bằng 0
 - ZF = 1 vì kết quả thu được bằng 0
 - CF = 1 vì có nhớ từ bit msb trong phép cộng
 - OF = 1 vì các số hạng tham gia phép cộng đều là các số âm nhưng kết quả nhận được là một số dương (còn khi thực hiện phép cộng dứoi dạng nhị phân bạn sẽ thấy không có nhứo vào bit msb nhưng lại có nhớ từ bit msb).

**Ví dụ 3:** Thực hiện phép trừ AX, BX trong đó AX chứa 8000h còn BX chứa 0001h

**trả lời**
```
  8000
  0001
  ____
  7FFF
```
 - Kết quả nhận được trong AX là 7FFFh = 0111 1111 1111 1111

 - SF = 0 vì msb = 0 
 - PF = 1 vì có 8 bit 1 (chẵn các bit 1) trong byte thấp của kết quả.
 - ZF = 0 vì kết quả thu được khác 0
 - CF = 0 vì số không dấu nhỏ hơn bị trừ từ số lớn hơn
 - Còn về cờ OF, xét về phương diện các số có dấu, chúng ta đã trừ một số dương từ một số âm cũng giống như cộng 2 số âm. Tuy nhiên kết quả nhận được lại là số dương (sai dấu) do đó OF = 1.

**Ví dụ 4:** Tăng AL lên 1 khi AL chứa FFh

**trả lời**
```
  FF
   1
 ___
 100
```
 Kết quả nhập được trong AL là 00h, SF = 0, PF = 1, ZF = 1. Mặc dù có nhứo CF không bị ảnh hưởng bởi lệnh INC. Có nghĩa là nếu trước đó CF = 0 thì cuối cùng CF vẫn bằng 0.
 OF = 0 vì 2 số hạng có dấu khác nhau (có nhớ vào msb đồng thời cũng có nhớ từ msb)

**Ví dụ 5:**  Chuyển -5 vào AX

**Trả lời:**
```
             8000h   1000 0000 0000 0000
 số bù 1 của 8000h   0111 1111 1111 1111
                                      +1
_________________________________________
 số bù 2 của 8000h   1000 0000 0000 0000
```
 Kết quả nhận được trong AX là 1000 0000 0000 0000 = 8000h

 SF = 1, PF = 1, ZF = 0
 CF = 1 vì trong lệnh NEG, CF luôn bằng 1 trừ khi kết quả bằng 0
 OF = 1 vì kết quả là 8000h, khi lấy nghịch đảo của một số kết quả nhận được phải có dấu ngược lại nhưng ở đây số bù 2 của 8000h lại chính thức bằng nó tức là dấu không thay đổi. 
