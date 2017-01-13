#báo cáo 
##lập trình hợp ngữ (assembly)

>	Tài liệu tham khảo : [assembly](http://bit.ly/2iZ45FN)

>	Thực hiện:  Nguyễn Văn HIệp

>	Cập nhật lần cuối: 13/01/2017
> ---------------------

###mục lục

> [1. tổng quan](#1)

> [2. lệnh nhảy](#2)

> - [2.1 lệnh nhảy có điều kiện](#2.1)

> - [2.2 lệnh CMP (nhảy có điều kiện)](#2.2)

> - [2.3 dịch các lệnh có điều kiện](#2.3)

> - [2.4 so sánh lệnh nhảy không dấu và lệnh nhảy có dấu](#2.3)

> - [2.5 lệnh JMP (nhảy không điều kiện)](#2.5)

> [3. các cấu trúc ngôn ngữ bậc cao](#3)

> - [3.1 cấu trúc rẽ nhánh](#3.1)

> - [3.2 cấu trúc IF-THEN-ELSE](#3.2)

> - [3.3 cấu trúc case](#3.3)

> - [3.4 điều kiện kép với AND- OR](#3.4)

> [4. cấu trúc vòng lặp for_while_REPEAT](#4)

> - [4.1 vòng lặp FOR](#4.1)

> - [4.2 vòng lặp WHILE](#4.2)

> - [4.3 vòng lặp REPEAT](#4.3)

>------

###1. tổng quan <a name="1"></a>
 - để đảm bảo thực hiện các công việc có ích, các chương trình bằng hợp ngữ phải có các phương pháp lựa chọn và lặp lị các đoạn mã lệnh. trong phần này, chúng ta chỉ ra các phương pháp đó được thực hiện như thế nào nhờ các lệnh nhảy và lặp.

 - các lệnh nhảy và lặp chuyển điều khiển cho các phần khác trong chương trình. sự chuyển giao này có thể không có điều kiện, có thể phụ thuộc vào các tổ hợp riêng rẽ các cờ trạng thái.

 - sau khi làm quen với các cấu trúc nhảy. chúng ta sẽ sử dụng chúng để thực hiện các cấu trúc lựa chọn và lặp của các ngôn ngữ bậc cao. các ứng dụng này sẽ làm cho việc đổi một thuật toán với các lệnh giả sang các lệnh hợp ngữ dễ dàng hơn.

###2. lệnh nhảy <a name="2"></a>

####2.1 lệnh nhảy có điều kiện <a name="2.1"></a>
 - JNZ là lệnh **nhảy có điều kiện** , cú pháp :
  ` JNZ        nhãn_đích `
  - nếu như điều kiện của lệnh nhảy được thỏa mãn. lệnh có nhãn nhãn_đích sẽ được thi hành. lệnh này có thể ở trước hoặc sau lệnh nhảy. nếu điều kiện không thỏa mãn, lệnh ngay sau lệnh nhãy được thi hành. với lệnh JNZ điều kiện là kết quả của lệnh trước nó khác 0
**phạm vi của lệnh nhảy có điều kiện**
 - cấu trúc mã máy của lệnh nhảy có điều kiện đòi hỏi nhãn_đích phải đứng trước lệnh nhảu không quá 126 byte hoặc đứng sau không quá 127 byte.
 
 **CPU thực hiện một lệnh nhảy như thế nào ?.**

 - để thực hiện một lệnh nhảy, CPU nhìn vào thanh ghi cờ. ta đã biết rằng thanh ghi phản ánh kết quả công việc gần nhất mà bộ vi xử lý thực hiện. nếu điều kiện của lệnh nhảy(được phát biểu như một tổ hợp các sự lập cờ trạng thái) thỏa mãn, CPU điều chỉnh IP trỏ đến nhãn đích và như thế lệnh ở sau nhãn này sẽ được thi hành tiếp theo. nếu điều kiện nhảy không thỏa mãn, IP không bị sửa đổi. điều này có nghĩa là lệnh trên dòng tiếp theo sẽ được thi hành.

 - **bảng 6.1** chỉ ra các lệnh nhảy có điều kiện. có 3 loại đó là :
  <ul>
  <li>(1) các lệnh nhảy có dấu được dùng khi kết quả trả về là các số có dấu </li>
  <li>(2) các lệnh nhảy không dấu dùng với các số không dấu.</li>
  <li>(3) các lệnh nhảy điều kiện đơn: điều kiện phụ thuộc vào một cờ riêng biệt. </li>
 </ul>
 ** lưu ý:** các lệnh nhảy tự nó không ảnh hưởng tới các cờ.
  - cột đầu tiên trong **Bảng 6.1** là mã lệnh nhảy. rất nhiều lệnh nhảy có hai mã lệnh. ví dụ:JG và JNLE cả hai mã lệnh này có cùng một mã máy. khi thực hiện chúng cho kết quả hoàn toàn giống nhau.

  **Bảng 6.1 các lệnh nhảy có điều kiện.**

 | ký hiệu | chức năng | điều kiện nhảy |
 |---------|-----------|-----------------|
 |JG/JNGE|nhảy đếu lớn hơn, nhảy nếu không nhỏ hơn hay bằng|ZF=0 và SF=OF|
 |JGE/JNL|nhảy nếu lớn hơn hay bằng, nhảy nếu không nhỏ hơn|SF=OF|
 |JL/JNGE|nhảy nếu nhỏ hơn, nhảy nếu không lớn hơn hay bằng| SF<>OF|
 |JLE/JNG|nhảy nếu nhỏ hơn hay bằng, nhảy nếu không lớn hơn|ZF=1 hay SF=OF|

**các lệnh nhảy không dấu**

 | ký hiệu | chức năng | điều kiện nhảy |
 |---------|-----------|-----------------|
 |JA/JNBE|nhảy nếu lớn hơn, nhảy nếu không nhỏ hơn hay bằng|CF=0 và ZF=0|
 |JAE/JNB|nhảy nếu lớn hơn hay bằng, nhảy nếu không nhỏ hơn|CF=0|
 |JB/JNAE|nhảy nếu nhỏ hơn, nhảy nếu không lớn hơn hay bằng|CF=1|
 |JBE/JNA|nhảy nếu nhỏ hơn hay bằng,nhảy nếu không lớn hơn|CF=1 hay ZF=1|

**các lệnh nhảy đơn.**

 | ký hiệu | chức năng | điều kiện nhảy |
 |---------|-----------|-----------------|
 |JE/JZ|nhảy nếu bằng, nhảy nếu bằng 0|ZF=1|
 |JNE/JNZ|nhảy nếu không bằng, nhảy nếu khác 0|ZF=0|
 |JC|nhảy nếu có nhớ|CF=1|
 |JCN|nhảy nếu không nhớ|CF=0|
 |JO|nhảy nếu tràn| OF=1|
 |JS|nhảy nếu dấu âm|SF =1|
 |JNS|nhảy nếu dấu dương|SF=0|
 |JP/JPE|nhảy nếu cờ chẵn|PF=1|
 |JNP/JPO|nhảy nếu cờ lẻ|PF=0|

###2.2 lệnh CMP<a name="2.2"></a>
 - các điều kiện nhảy thường được cung cấp bởi lệnh CMP (compare). nó có dạng sau:
 `CMP   đích,nguồn`
 - lệnh này so sánh toán tử đích với toán tử nguồn bằng cách lấy toán tử đích trừ đi toán tử nguồn. kết quả không được lưu lại nhưng các cờ bị ảnh hưởng. các toán hạng của lênh CMP không thể cùng là các ô nhớ. toán hạng đích không được phép là hằng số. chú ý:CMP giống hệt SUB ngoại trừ việc toán hạng đích không bị thay đổi.

**ví dụ**. giả thiết chương trình chứa hai dòng lệnh sau đây:
```
CMP AX,BX
JG  BELOW
```
 - ở đây AX =7FFFh và BX=0001. kết quả so sánh AX và BX là 7FFFh - 0001h = 7FFFEh.
 - Bảng 6.1 chỉ ra rằng điều kiện nhảy cho lệnh JG đã đươc jthỏa mãn bởi vì ZF=SF=OF=0 do đó điều khiển được chuển đến nhã BELOW
###2.3 dịch các lệnh có điều kiện <a name="2.3"></a>

  - trong các ví dụ trên ,khi xem xét các cờ sau kênh CMP ta thấy rằng điều khiển sẽ được chuyển đến nhãn BELOW. đó cũng là cách CPU thực hiện một lệnh nhảy có điều kiện. người lập trình không nhất thiết phải suy nghĩ nhiều về các cờ, bạn có thể chỉ dùng tên của lệnh nhảy để quyết định việc chuyển điều khiển đến nhãn đích. các lệnh sau đây:
  ```
  CMP  AX,BX
  JG   BELOW
  ```
 - nếu như AX lớn hơn BX ( coi là số có dấu) thì JG (jump if greater than) sẽ chuyển đến BELOW.
 - đặc biệt cả khi nghĩ rằng lệnh CMP được thiết kế chỉ dùng cho các lệnh nhảy có điều kiện, chúng vẫn có thể đi kèm với các lệnh khác như trong chương trình . một ví dụ khác:
 ```
 DEC  AX
 JL   THERE
 ```
 - trường hợp này nếu nội dung của AX (coi là số có dấu) nhỏ hơn 0, điều khiển sẽ được chuyển đến THERE.

###2.4 so sánh các lệnh nhảy có dấu và không dấu.<a name="2.4"></a>
 - mỗi lệnh nhảy có dấu đều tương ứng với một lệnh nhảy không dấu. ví dụ lệnh nhảu có dấu JG và lệnh nhảy không dấu JA. dùng lệnh nhảy có dấu hay không dấu tùy thuộc vào kiểu số được đưa ra, Trên thực tế, như bảng đã chỉ ra, các lệnh này thao tác với các cờ khác nhau : các lệnh nhảy có dấu sử dụng các cờ ZF,SF và OF trong khi các lệnh nhảy không dấu lại dùng các cờ ZF và CF. sử dụng không đúng loại có thể đưa ra kết quả sai.
**ví dụ** giả thiết rằng chúng ta làm việc với các số không dấu. nếu như AX=7FFFh và BX = 8000h, khi ta thực hiện:
```
CMP  AX,BX
JA   BELOW
```
 - thậm chí FFFh > 8000h trong dạng có dấu, chương trình vẫn không nhảy đến nhãn BELOW, nguyên nhân ở đây là 7FFFh <8000h ở dạng không dấu và ở đây chúng ta lại dùng lệnh nhảy không dấu JA.

**làm việc với các ký tự.**

 - khi làm việc với tập hợp ký tự ASCII chuẩn cả các lệnh nhảy cí điều kiện và không điều kiện đều có thể được sử dụng bởi lẽ bit dấu của byte chứa mã ký thự luôn là 0. dù sao thì các lệnh nhảy không dấu phải được sử dụng khi so sánh các ký tự ASCII mở rộng(mã từ 80h đến FFH)

###2.5 lệnh JMP (nhảy không điều kiện)<a name="2.5"></a>
 - lệnh JMP (jump) dẫn đến việc chuyển điều khiển không điều kiện(nhảy không điều kiện), cú pháp:
 ` JMP  đích  `
 - ở đâu đích phải là một nhãn trong cùng một đoạn JMP 
 - JMP có thể được dùng để khăc phục khoảng giới hạn của lệnh nhảy có điều kiện. 
 - **ví dụ: **giả sử chúng ta muốn thực hiện vòng lặp:
```
 top:
 ;thân vòng lặp
 	DEC  DX   ; giảm bộ đếm
 	JNZ  top  ; lặp nếu CX>0
 	mov  AX,BX
```
 - nhưng thân vòng lặp lại chứa quá nhiều lệnh đến mức nhãn top nằm ngoài khoảng giới hạn của lệnh JNZ (nhiều hơn 126 byte trước JNZ top). chúng ta có thể sửa lại:
```
top:
; thân vòng lặp
		DEC	CX ;giảm bộ đếm
		JNZ BOTTOM  ;lặp nếu CX>0
		JMP EXIT
	BOTTOM: JMP TOP
	EXIT: MOV AX,BX
```

###3. Cấu trúc ngôn ngữ bậc cao<a name="3"></a>

 - chúng ta đã biết cấu trúc nhảy có thể được dùng để thực hiện các công việc rẽ nhánh và lặp tuy nhiên do các lệnh nhảy quá sơ khai nên rất khó mã hóa một thuật toán (có hoặc không có các dòng hướng dẫn) nhất là đối với những người mới lập trình.
 - bởi vì đa số chúng ta đã có chút ít kinh nghiệm về cấu trúc của ngôn ngữ bậc cao như cấu trúc IF_THEN_ELSE hay các vòng lặp 	WHILE chúng ta sẽ nêu ra cách giả lập các cấu trúc này trong ngôn ngữ hợp ngữ. trong trường hợp đầu tiên chúng tôi sẽ đưa ra cấu trúc của các toán tử giả của ngôn ngữ bậc cao.

###3.1 các cấu trúc rẽ nhánh <a name="3.1"></a>

###3.2 cấu trúc IF_THEN_ELSE <a name="3.2"></a>

 - trong ngôn ngữ bậc cao, các cấu trúc rẽ nhánh của một chương trình để chọn các hướng dẫn khác nhau và phụ thuộc vào các điều kiện. phần này chúng ta sẽ xem xét 3 cấu trúc.
`IF_THEN`
 - Cấu trúc IF_THEN có thể được khai báo dưới dạng toán tử giả như sau:
```
IF 	điều_kiện
	THEN
		nhánh_đúng
	END_IF
```
 - điều kiện là một biêt thưc có thể đúng hoặc sai nếu nó đúng ,nhánh_đúng sẽ được thực hiện. ngược lại cấu trúc không thực hiện lệnh nào, chương trình tiếp tục với các lệnh theo sau.
 - sơ đồ được biểu diễn như sau:
 ![hình ảnh 1](http://i.imgur.com/kXz1P60.jpg)
 
**ví dụ** thay số trong AX bằng giá trị tuyệt đối của nó.
**trả lời**: một thuật toán với mã lệnh giả:
```
IF AX>0
THEN
	Thay AX bằng -AX
END_IF
```
nó có thể được mã hóa như sau:
```
; if AX<0
	CMP AX,0  ;AX<0?
	JNP END_IF   ;không, thoát ra.
;then
	NEG AX  ;đúng, đổi dấu
END IF:
```
 - điều kiện AX<0 được kiểm tra bởi lệnh CMP AX,0 nếu AX không nhỏ hơn 0, ta không phải làm gì cả, JNL được dùng để nhảy qua lệnh NEG AX. nếu điều kiện AX<0 thỏa mãn, chương trình tiếp tục thực hiện lệnh NEG AX. nếu điều kiện AX<0 thỏa mãn, chương trình tiếp tục thực hiện lệnh NEG AX.
```
 IF_THEN_ELSE
 IF  điều_kiện
 THEN
 	nhánh_nhánh đúng
 ELSE
 	nhánh_sai
 END_IF
```
 - trong cấu trúc này nếu điều_kiện là đúng. nhóm lệnh nhánh_đúng sẽ được thi hành. còn nếu điều_kiện sai, nhóm lệnh nhánh_sai sẽ được thi hành.
**ví dụ** giả sử AL và BL chứ các ký tự ASCII mở rộng, hãy hiển thị lý tự đứng trước trong bảng mã.
**trả lời:**
```
IF AL<=BL
	THEN 
		hiển thị ký tự trong AL
ELSSE
		Hiển thị kýtự trong BL
END_IF
```
 - ta có thể mã hóa nó như sau.
```
         MOV AH,2		;chuẩn bị hiển thị
IF  AL<=BL
		CMP  AL,BL 		;AL<=BL ?
		JNBE ELSE_ 		; không, hiển thị ký tự trong BL
;THEN 					;AL<=BL
		MOV DL,BL 		;chuyển ký tự vào DL để hiển thị
		JMP DISPLAY 	;tới DISPLAY
ELSE_:					;BL<AL
		MOV DL,BL 		;chuyển ký tự vào DL để hiển thị
DISPLAY:
		INT  21H 		; hiển thị nó.
;END_IF    
```
**CHÚ Ý** ta dùng nhãn else_ vì else là từ dành riêng.

 - điều kiện AL<=BL được kiểm tra bởi lênh CMP AL<BL nếu nó sai, chương trình sẽ nhảy qua nhánh_đúng tới ELSE_. chúng ta sử dụng lệnh nhảy không dấu JNBE bởi lẽ chúng ta đang so sánh các ký tự mở rộng.
 - nếu AL<=BL thỏa mãn, nhánh_đúng được thực hiện. lưu ý rằng chỉ thị JMP DISPLAY là cần thiết để nhảy qua nhánh_sai. điều này khác với trong ngôn ngữ bậc cao: nhánh_sai được tự động nhảy qua nếu nhanh_đúng được thực hiện.

###3.3 cấu trúc case<a name="3.3"></a>
 - CASE là một cấu trúc đa nhánh , nó kiểm tra các thanh ghi, các biến hay các biểu thức với các giá trị riêng rẽ trong miền giá trị, dạng tổng quát của nó là :
```
CASE 	phát_biểu
	giá_trị_1: dòng_lệnh_1
	giá_trị_2: dòng_lệnh_2
	.....................
	.....................
	giá_trị_n: dòng_lệnh_n
END_CASE
```
 - trong cấu trúc này phát_biểu được kiểm tra. nếu giá trị của nó bằng với giá_trị_i thì dòng_lệnh_i sẽ được thi hành. ta giả thiết tập hợp giá_trị_1.....giá_trị_n tách biệt nhau.
 - sơ đồ của cấu rúc case là :
 ![hình ảnh 2](http://i.imgur.com/SSDTyk2.png)

 **ví dụ:** nếu AX chứ một số âm, hãy nhập -1 và BX, nếu AX chứa 0, cho BX =0, nếu AX dương đổi BX thành 1

 **lời giải:**
```
 CASE AX
 	<O:gán BX bằng -1
 	=0: gán BX bằng 0
 	>0: gán BX bằng 1
 END_CASE
```
ta có thể mã hóa lại như sau
```
;case AX
	CMP 	AX,0 		;kiểm tra AX
	JL 		NAGATIVE	;AX<0
	JE 		ZERO		;AX=0
	JG 		POSITIVE	;AX>0	
NAGATIVE:
	MOV 	BX,-1 		; nhập -1 vào BX
	JMP 	END_CASE	; rồi thoát
ZERO:
	MOV 	BX,0 		; nhập 0 vào BX
	JMP 	END_CASE	;rồi thoát
POSITIVE:
	MOV 	BX,1 		; nhập 1 vào BX
END_CASE:
```
####3.4 các nhánh với điều kiện kép.<a name="3.4"></a>

 - đôi khi điều kiện của IF hay CASE có dạng:
 `điều_kiện_1  AND điều_kiện_2`
 hay 
 `điều_kiện_1 OR điều_kiện_2 `
 - ở đây điều_kiện_1 và điều_kiện_2 có thể đúng hoặc sai. đầu tiên chúng ta ãy xem xét điều kiện AND sau đó là OR

#####điều kiện AND
 - điều kiện AND chỉ đúng khi cả hai điều kiện: điều_kiện_1 và điều_kiện_2 đều đúng. ngược lại nếu một trong chúng sai, điều kiện AND cũng sẽ sai.

 **ví dụ**: đọc một ký tự. nếu là chữ hoa thì hiển thị nó.
 **lời giải**
 ``` 
 đọc 1 ký tự vào (vào DL)
 IF ('A'<=ký_tự) và (ký_tự <= 'Z')
 THEN
 	hiển thị ký tự
 END_IF
 ```
  - để mã hóa, đầu tiên chúng ta kiểm tra xem ký tự trong AL có đừng sau 'A' trong bảng mã không, nếu sai ta có thể kết thúc. nếu, đúng trước khi hiển thị ký tự ta vẫn còn phải kiểm tra ký tự có đứng trước 'Z' hay không. sau đây là mã lệnh.
```
; đọc một ký tự
	MOV 	AH,1	;chuẩn bị đọc
	INT 	21h 	; ký tự vào AL
;if ('A' <= ký tự vào) và (ký tự <='Z')
	CMP 	AL,'A' 	; ký tự >='A'?
	JNGE 	END_IF 	;không, thoát ra
	CMP 	AL, 'Z' ;ký tự <='Z' ?
	JNGE 	END_IF 	; không, thoát ra
;then hiển thị ký tự
	MOV 	DL,AL 	;lấy ký tự
	MOV 	AH,2 	; chuẩn bị hiển thị
	INT 	21H 	; hiển thị
EDN_IF :
```

** các điều kiện OR.**

 - điều_kiện_1 OR điều_kiện_2 là đúng khi điều_kiện_1 hoặc điều_kiện_2 đúng . nó chỉ sai khi cả 2 điều kiện cùng sai.

**ví dụ**: đọc một ký tự. nếu là "y" hay "Y" thì hiển thị nó. ngược lại, kết thúc chương trình

**lời giải**
```
đọc một ký tự (vào AL)
IF (ký_tự = 'y') hoặc (ký_tự = 'Y')
THEN
	hiển thị ký tự
ELSE
	kết thúc chương trình
END_IF
```

 - để mã hóa , đầu tiên ta kiểm tra xem ký_tự ="y "? . nếu thỏa mãn, điều kiện OR đúng và chúng ta có thể thực hiện dòng lệnh THEN , ngược lại vẫn còn cơ hội để điều kiện OR đúng, đó là khi ký_tự bằng "Y", và dòng lệnh THEN được thi hành. nếu đều này vẫn sai, điều kiện OR là sai và chúng ta sẽ thực hiện dòng lệnh ELSE sau đây là mã lệnh.

 ```
 ; đọc một ký tự
 	MOV 	AH,1  		;CHUẨN BỊ NHẬN 1 KÝ TỰ
 	INT 	21H 		; KÝ TỰ NHẬN VÀO Ở AL
 ;if (ký_tự ='y') hoặc (ký_tự = 'Y')
 	CMP 	AL,'y' 		; KÝ_TỰ = 'y' ?
 	JE 		THEN 		; đúng, hiển thị ký tự
 	CMP 	AL,'Y' 		;ký_tự = 'Y' ?
 	JE 		THEN 		;đúng, chuyển đến hiển thị ký tự
 	JMP 	ELSE_ 		; sai, kết thúc
 THEN:
 	MOV 	AH,2 		;chuẩn bị in ra màn hình
 	MOV 	DL,AL  		;lấy ký tự trong AL chuyển cho DL
 	INT 	21h 		; hiển thị nó
 	JMP 	END_IF 		;VÀ THOÁT
 ELSE_ :
 	MOV 	AH,4CH 		;
 	INT 	21H
 END_IF:
```

###4. cấu trúc vòng lặp for_while_REPEAT <a name="4"></a>

 - một vòng lặp là một chuỗi các lệnh được lặp lại. số lần ặp cóp thể đã xác định trước hoặc phụ thuộc vào các điều kiện.

#####4.1 vòng lặp FOR <a name="4.1"></a>

 - đây là vòng lặp mà số lần lặp lại các dòng lệnh đã biết trước (vòng lặp điều khiển bằng biến đếm). dãng mã lệnh giả:
```
FOR  	số_lần_lặp 	DO
		các dòng lệnh
END_FOR
```
 - ta có thể sử dụng lệnh LOOP để thực hiện vòng lặp FOR. lệnh này có dạng:
 `LOOP 	nhãn_đích`
  - bộ đếm vòng lặp là thanh ghi CX được khởi tạo bằng số_lần_lặp. mỗi lần thực hiện lệnh LOOP, thanh CX tự động giảm đi 1 và nếu CX khác 0 thì điều khiển được chuyển tới nhãn đích. nếu CX=0 lệnh tiếp theo lệnh LOOP sẽ được thi hành. nhãn đích phải ở trước lệnh lặp không quá 126 byte.
  - vòng lặp FOR có thể thực hiện nhờ lệnh LOOP như sau:
```
	; khởi tạo CX bằng số_lần_lặp
TOP:
	;thân vòng lặp
	LOOP TOP
```
**ví dụ**: viết một vòng lặp điều khiển bằng biến đếm hiển thị một dòng 80 dấu sao(*)
 **lời giải:**
```
FOR 80 times DO
	hiển thị '*'
END_FOR
```
 - mã lệnh tương ứng là:
```
	MOV 	CX,80		; số các dấu * được hiển thị
	MOV 	AH,2 		;hàm hiển thị ký tự
	MOV 	DL,'*' 		; ký tự hiển thị
TOP:
	INT  	21H 		; hiển thị một dấu *
	LOOP 	TOP 		; lặp lại 80 lần
```
 - bạn hãy lưu ý rằng vòng lặp FOR thực hiện bởi lệnh LOOP sẽ được thực hiện ít nhất 1 lần. thực ra nếu CX bằng 0 khi vào vòng lặp, lệnh LOOP giản CX thành 0FFFFh và lệnh LOOP sẽ được thực hiện 0FFFFh = 655535 lần nữa. để khắc phục điều này, lệnh JCXZ (jump if CX is zero) được đặt trước vòng lặp. cú pháp của nó là: 
`JCXZ 	nhãn đích `
 - nếu CX bằng 0, điều khiển sẽ được chuyển đến nhãn đích. như vậy vòng lặp sẽ bị bỏ qua nếu CX bằng 0:
```
	JCXZ 	SKIP
TOP: 
	; thân vòng lặp
	LOOP  TOP
```
####4.2 vòng lặp WHILE <a name="4.2"></a>
 
 - đây là vòng lặp phụ thuộc vào một điều kiện. dạng mã lệnh giả:
```
WHILE 	điều_kiện 	DO
	các dòng lệnh
END_while
```
 - điều_kiện được kiểm tra ở đầu vòng lặp. nếu nó đúng thì các dòng lệnh sẽ được thi hành. ngược lại nếu điều_kiện sai, chuwong trình tiếp tục thự hiện lệnh ở sau vòng lặp. rất có thể ngay khi khởi đầu_điều kiện đã không thỏa mãn. trong trường hợp này thân vòng lặp sẽ không được thực hiện lần nào. vòng lặp tiếp tục được thực hiện khi điều kiện còn đúng

 **ví dụ** : viết các lệnh để đếm số ký tự trong 1 dòng.
 **lời giải**:
```
 	MOV 	DX,O 		;DX đếm số ký tự
 	MOV 	AH,1 		;chuẩn bị đọc
 	INT  	21H 		;ký tự trong AL
 WHILE_:
 	CMP 	AL,0DH 		; CR ?
 	JE 		END_WHILE 	;đúng, thoát ra
 	INC 	DX 			; không đúng, tăng bộ đếm
 	INT  	21H 		;đọc 1 ký tự
 	JMP 	WHILE_ 		; lặp lại
 END_WHILE:
```
 - lưu ý là do vòng lặp WHILE kiểm tra điều kiện kết thúc ở đầu vòng lặp nên bạn cần chắc chắn rằng bất cứ biến đếm nào liên quan đến điều kiện vòng lặp đều phải được khởi tạo trước khi vào vòng lặp. vì vậy bạn phải đọc một ký tự trơcs khi vào vòng lặp rồi phải đọc ký tự khác ở cuối nó . ta dùng nhãn WHILE_ vì WHILElà từ dành riêng.

####4.3 vòng lặp REPEAT<a name="4.3"></a>
 - có một vòng lặp có điều kiện khác đó là vòng lặp REPEAT. dạng mã lệnh giả của nó như sau:
```
REPEAT
	các dòng lệnh
UNTILL 	điều_kiện
```
 - trong một vòng lặp REPEAT ...UNTILL, các dòng lệnh được thi hành sau đó mới kiểm tra điều kiện. nếu điều_kiện đúng, vòng lặp kết thúc, nếu nó sai điều khiển rẽ nhánh đến đầu vòng lặp.

**ví dụ:** viết các lệnh đọc vào các ký tự, kết thúc khi gặp một ký tự trắng.
**lời giải**
```
	MOV 	AH,1 	 	;chuẩn bị đọc
REPEAT:
	INT 	21H 		;ký tự trong AL
;UNTILL
	CMP 	AL,' ' 		;ký tự trắng ?
	JNE 	REPEAT 		; không, đọc tiếp
```
#####so sánh WHILE và REPEAT.
 - trong nhiều trường hợp khi cần một vòng lặp có diều kiện, sử dụng vòng lặp WHILE hay REPEAT là tùy ý thích mỗi người . ưu điểm của vòng lặp WHILE là vòng lặp có thể được bỏ qua khi điều kiện kết thúc khởi tạo giá trị logic sai, trong khi đó các lệnh trong vòng lặp REPEAT luôn được thực hiện ít nhất 1 lần. tuy nhiên các lệnh trong vòng  lặp REPEAT có vẽ ngắn hơn đôi chút bởi lẽ nó chỉ có một số lệnh nhảy có điều kiện ở cuối trong khi vòng lặp WHILE có những hai, một lệnh nhảy có điều kiện ở đầu và lệnh JMP ở cuối.

