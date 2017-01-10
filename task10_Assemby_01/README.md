##Báo cáo

#Lập Trình Hợp Ngữ(Assembly)

>	Tài liệu tham khảo : [assembly](http://bit.ly/2iZ45FN)

>	Thực hiện:  Nguyễn Văn HIệp

>	Cập nhật lần cuối: 10/01/2017
> ---------------------

###Mục Lục
[1. Cấu trúc chương trình Assembly](#1)

 - [1.1 Tổng quan](#1.1)
 
 - [1.2. Cú pháp câu lệnh Assembly](#1.2)

 - [1.3. Cấu trúc chương chình hợp ngữ dùng cho tập tin thực thi ***.EXE**](#1.3)

 - [1.4. Cấu trúc chương trình hợp ngữ dành cho tập tin lệnh ***.com**](#1.4)

 - [1.5. Chương trình Netwide Assemble(NASM)](#1.5)

 - [1.6. Chương trình Macro Assembly(MASM)](#1.6)

[2. Các phép toán, biểu thức, câu lệnh gán và các sử lý dữ liệu đơn giản.](#2)
 
 - [2.1. Các phép toán](#2.1)

 - [2.2. Biểu thức](#2.2)

 - [2.3. Câu lệnh gán](#2.3)

 - [2.4. Sử lý số liệu đơn giản](#2.4)	

[3. Các lệnh cơ bản](#3)

[4. Cài đặt IDE Emu8086 và code và chạy lại các ví dụ](#4)


####1. Cấu trúc chương trình Assembly <a name="1"></a>
#####1.1 Tổng quan về asembly <a name="1.1"></a>

 - Ngôn ngữ lập trình Assembly(hợp ngữ) sữ dụng các từ gợi nhớ, từ viết tắt,... thay cho các chỉ thị(lệnh) phức tạp trong vi xử lý, giúp con người lập trình tạo ra các chương chỉnh có khả năng thực thi nhanh và sử dụng tài nguyên tối ưu.

 -  Hợp ngữ hỗ trợ thiết kế cả hai dạng cấu trúc chương trình EXE và COM, mỗi dạng phù hợp với một nhóm trình biên dịch nào đó. Muốn biên dịch một chương trình hợp ngữ sang dạng EXE thì ngoài việc nó phải được viết theo cấu trúc dạng EXE ta còn cần phải sử dụng một trình biên dịch phù hợp. Điều này cũng tương tự với việc muốn có một chương trình thực thi dạng COM.

 -  Văn bản của một chương trình hợp ngữ dạng EXE cũng cho thấy rõ nó gồm 3 đoạn: Code, Data và Stack. Tương tự, văn bản của chương trình hợp ngữ dạng COM cho thấy nó chỉ có 1 đoạn: Code, cả Data và Stack (không tường minh) đều nằm ở đây.

#####1.2. Cú pháp câu lệnh Assembly <a name="1.2"></a>


`[tên]   toán tử    [toán hạng]   [;lời bình]`


Trong đó:

 - [tên]: Có thể xem là tên đại diện của lệnh. Nó thường được sử dụng trong các đoạn lệnh có lệnh nhảy (jump), lệnh lặp (loop), lệnh gọi chương trình con (call),…

 - **toán tử :** Tên của lệnh ở dạng gợi nhớ (như: mov, add, sub, int…). Đây là một trong các lệnh thuộc tập lệnh gợi nhớ của một processor nào đó. 

 - [toán hạng]: Đây là các đối tượng chịu sự tác động của lệnh: Biến, hằng, thanh ghi, địa chỉ ô nhớ,… Một lệnh ngôn ngữ Assembly có thể không có, có 1 hoặc 2 operand (toán hạng)

 - [;lời bình]: Phần chú thích lệnh. Được đặt sau dấu chấm phẩy (;).

####1.3 Cấu trúc chương trình hợp ngữ dùng cho tập tin thực thi ***.EXE**<a name="1.3"></a>
 
 - Phần **"Data segment"**: khai báo các hằng số, biến mảng...
 Các toán thử thường dùng:
  <ul>
  <li>org -> Xác định vị trí mã lệnh</li>
  <li>EQU -> gán giá trị cho 1 biến</li>
  <li>DB -> khai báo biến kích thích 1 byte</li>
  <li>DD -> Khai báo biến kích thước 4 byte</li>
  <li>DQ -> Khai báo biến kích thước 8 byte</li>
  <li>DT -> Khai báo biến kích thước 10 byte</li>
  <li>dup(?) -> tạo một mảng và gán cùng một giá trị</li>
 </ul>

Ví dụ:

```
xuongdong  EQU  0Dh    ; xuống dòng
Tmp        DB   ?      ; Tạo một biến tên Tmp
Str        DB   "2"    ; Tạo một mảng chứa chuỗi 
bufferdata DB   100 dup(?) ; Tạo mảng có 100 ô nhớ
```

 - Phần **"Stack segment"**: khai báo bộ ngăn xếp. Do x86 sử dụng các thanh ghi 16bit nên kích thước bộ đệm luôn được gán bằng toán tử **DW**

VD:
```
	Stack 
		DW	256	dup (?) ; tạo ngăn xếp có 256 ô nhớ.
	End
```

 - Phần "Code Segment": phần chính của chương trình chứa các lệnh hợp ngữ gồm chương chình chính và chương trình con - Procedure(Nếu có)
 VD:
 ```
	Code 
		...						;các lệnh 
		call chuongtrinhcon		;gọi chương trình con
		...						;các lệnh 
	End
 ```

 ```
 	chuongtrinhcon		Proc
 		...							;các lệnh 
 		ret							;quay về chương trình chính
 	chuongtrinhcon		EndP
 ```

#####1.4. Cấu trúc chương trình hợp ngữ dành cho tập tin lệnh ***.com** <a name="1.4"></a>

 - Tập tin gồm 2 phần (data và code), không có bộ nhớ ngăn xếp.
 VD:

```
	Code Segment
		org 	h 	;địa chỉ đầu của chương trình
	Start:		JMP 	CONTINUE	
		;Các khai báo biến, mảng, hằng...  tại đây
	CONTINUE:
		MAIN 	PROC 	
	; Các lệnh của chương trình chính, trở về DOS bằng INT 20h
		MAIN 	ENDP
	;Các chương trình con(nếu có) khai báo ở đây.
	END strart
```

#####1.5. Biên dịch và liên kết chương trình trong NASM <a name="1.5"></a>

 - Các Netwide Assembler, NASM, là 80x86 và x86-64 lắp ráp thiết kế cho tính di động và mô đun.
Nó hỗ trợ một loạt các định dạng tập tin đối tượng, bao gồm cả Linux và BSD * a.out, ELF, COFF, Mach-O,
Microsoft 16-bit OBJ, Win32 và Win64. Nó cũng sẽ ra thuần tập tin nhị phân. Cú pháp của nó được thiết kế để
đơn giản và dễ hiểu, tương tự như Intel, nhưng ít phức tạp hơn. Nó hỗ trợ tất cả x86 hiện đang được biết đến
mở rộng kiến trúc, và đã hỗ trợ mạnh mẽ cho các macro.

#####1.6. Chương trình Macro Assembly(MASM) <a name="1.6"></a>

 - Chương trình A86 Macro Assembly (tập tin chính là: A86.com) thường được sử dụng để dịch chương trình hợp ngữ sang chương trình thực thi dạng COM.

 - Chương trình Macro Assembly (tập tin chính là: `MASM.exe`) thường được sử dụng để dịch chương trình hợp ngữ sang chương trình thực thi dạng `EXE`. Tuy nhiên, `MASM` chỉ có thể dịch tập tin chương trình hợp ngữ sang dạng tập tin đối tượng mã máy dạng Obj. Để chuyển tập tin Obj sang tập tin chương trình thực thi EXE ta phải sử dụng chương trình liên kết của MSDOS, đó là `Link.exe`. Để chuyển tập tin thực thi dạng EXE sang tập tin thực thi dạng COM ta phải sử dụng thêm một chương trình khác của `MS_DOS`, đó là `EXE2Bin.com`.   

 - Có thể sử dụng các tập tin `TASM.Exe` và `TLINK.Exe` để thay thế cho `MASM.exe` và `Link.exe`. Các tập tin này, và cả tập tin `EXE2Bin.com`, có thể tìm thấy trong bộ chương trình Turbo Pascal.

 - `MASM` có thể dịch tập tin chương trình hợp ngữ sang các tập tin: tập tin đối tượng (`.Obj`), tập tin liệt kê thông tin (`.Lst`), tập tin tham khảo chéo (`.Crf`).

    - **Tập tin đối tượng** (Object File): Chứa bảng dịch mã máy của các lệnh trong chương trình nguồn hợp ngữ, và các thông tin cần thiết để có thể tạo nên một tập tin thực thi. Đây là tập tin chính để tạo nên tập tin thực thi.
    - **Tập tin liệt kê thông tin** (`List File`): Là một tập tin văn bản cho biết địa chỉ offset của từng lệnh trong đoạn Code; mã lệnh của các lệnh trong chương trình; danh sách các tên/nhãn dùng trong chương trình; các thông báo lỗi và một số thông tin khác. Đây là tập tin cơ sở hỗ trợ việc gỡ rối chương trình.
     - **Tập tin tham khảo chéo** (Cross Reference File): Liệt kê các tên sử dụng trong chương trình và dòng mà chúng xuất hiện.

####2. Các phép toán, biểu thức, câu lệnh gán và các sử lý dữ liệu đơn giản. <a name="2"></a>
#####2.1. Các phép toán <a name="2.1"></a>

 - Đối với một chỉ thị, trường toán hạng xác định dữ liệu sẽ được tác động lên. Một chỉ thị có thể không có, có 1 hoặc 2 toán hạng. VD:

 - ADD : Cộng 2 thanh ghi

 - SUB : Trừ 2 thanh ghi

 - INC : cộng thêm 1 nội dung của một thanh ghi hoặc 1 vị trí ô nhớ.

 - DEC : giảm bớt 1 thanh ghi hặc 1 vị trí nhớ.

 - NEG : Đổi dấu(lấy bù 2 của một thanh ghi hoặc một vị trí nhớ)

#####2.2. Biểu thức <a name="2.2"></a>
 - Biểu thức logic: Bao gồm các phép **NOT-AND-OR-XOR-TEST**

| A | B | A And B | A Or B | A Xor B | Not A |
|---|---|---------|--------|---------|-------|
| 0 | 0 |    0    |    0   |     0   |    1  |
| 0 | 1 |    0    |    1   |     1   |    1  |
| 1 | 0 |    0    |    1   |     1   |    0  |
| 1 | 1 |    1    |    1   |     0   |    0  |

 -  Cú pháp:

     - Not     [Toán hạng đích]

     - And     [Toán hạng đích], [Toán hạng nguồn]

     - Or      [Toán hạng đích], [Toán hạng nguồn]

     - Xor     [Toán hạng đích], [Toán hạng nguồn]

     - Test    [Toán hạng đích], [Toán hạng nguồn]

 - Lệnh `Not` (Logical Not): Thực hiện việc đảo ngược từng bít trong nội dung của [Toán hạng đích]. Lệnh này không làm ảnh hưởng đến các cờ.

Lệnh `Not` thường được sử dụng để tạo dạng bù 1 của [Toán hạng đích].        

- Lệnh `And` (Logical And): Thực hiện phép tính logic And trên từng cặp bít (tương ứng về vị trí) của [Toán hạng nguồn] với [Toán hạng đích], kết quả lưu vào [Toán hạng đích].

Lệnh `And` thường được sử dụng để xóa (= 0) một hoặc nhiều bít xác định nào đó trong một thanh ghi.

- Lệnh `Or`(Logical Inclusive Or):Thực hiện phép tính logic Or trên từng cặp bít (tương ứng về vị trí) của [Toán hạng nguồn] với [Toán hạng đích], kết quả lưu vào [Toán hạng đích].

Lệnh`Or`thường dùng để thiết lập (= 1) một hoặc nhiều bít xác định nào đó trong một thanh ghi.

- Lệnh `Xor` (eXclusive OR):Thực hiện phép tính logic Xor trên từng cặp bít (tương ứng về vị trí) của [Toán hạng nguồn] với [Toán hạng đích], kết quả lưu vào [Toán hạng đích].

Lệnh `Xor` thường dùng để so sánh (bằng nhau hay khác nhau) giá trị của hai toán hạng, nó cũng giúp phát hiện ra các bít khác nhau giữa hai toán hạng này.  

- Lệnh `Test`: Tương tự như lệnh And nhưng không ghi kết quả vào lại [Toán hạng đích], nó chỉ ảnh hưởng đến các cờ CF, OF, ZF,...

#####2.3. Câu lệnh gán<a name="2.3"></a>

 `Mov      [Toán hạng đích], [Toán hạng nguồn]`
 trong đó:

- [Toán hạng đích]: Có thể là thanh ghi (8 bít hay 16 bít), ô nhớ (chính xác hơn là địa chỉ của một ô nhớ) hay một biến nào đó. [Toán hạng đích] không thể là hằng số.   

- [Toán hạng nguồn]: Có thể là hằng số, biến, thanh ghi, ô nhớ (chính xác hơn là địa chỉ của một ô nhớ) nào đó.

**Tác dụng**: Lấy nội dung (giá trị) của [Toán hạng nguồn] đặt vào [Toán hạng đích]. Nội dung của [Toán hạng nguồn] không bị thay đổi.



#####2.4. Sử lý số liệu đơn giản<a name="2.4"></a>
 
 - `IN     AL, <Địa chỉ cổng> `      

 Lênh **In**(Input): Đọc một lượng dữ liệu 8 bít từ cổng được chỉ ra ở <Địa chỉ cổng> đưa vào lưu trữ trong thanh ghi AL.  

 Nếu địa chỉ cổng nằm trong giới hạn từ 0 đến FF (hệ thập lục phân) thì có thể viết trực tiếp trong câu lệnh, nếu địa chỉ cổng lớn hơn FF thì ta phải dùng thanh ghi Dx để chỉ định địa chỉ cổng.

 - `OUT   <Địa chỉ cổng>, AL`

  Lệnh **Out** (Output): Gởi một lượng dữ liệu 8 bít từ thanh ghi AL ra cổng được chỉ ra ở <Địa chỉ cổng>. Tương tự lệnh In, địa chỉ cổng có thể được viết trực tiếp trong câu lệnh hoặc thông qua thanh ghi Dx.

####3. Các lệnh cơ bản<a name="3"></a>

 - `Mov      [Toán hạng đích], [Toán hạng nguồn]`

 **Tác dụng**: Lấy nội dung (giá trị) của [Toán hạng nguồn] đặt vào [Toán hạng đích]. Nội dung của [Toán hạng nguồn] không bị thay đổi.

   - `Inc        [Toán hạng đích]`

Lệnh Inc (Increment): làm tăng giá trị của [Toán hạng đích] lên 1 đơn vị tương tự như toán tử ++ trong c.

   - `Add       [Toán hạng đích],[Toán hạng nguồn]`

Lệnh Add (Addition): lấy giá trị/nội dung của [Toán hạng nguồn] cộng vào giá trị/nội dung của [Toán hạng đích], kết quả này đặt vào lại [Toán hạng đích].   

   - `Dec       [Toán hạng đích]`

Lệnh Dec (Decrement): làm giảm giá trị của [Toán hạng đích] xuống 
1 đơn vị tương tự như -- trong c.

   - `Sub       [Toán hạng đích],[Toán hạng nguồn]`

Lệnh Sub (Subtract): lấy giá trị/nội dung của [Toán hạng đich] trừ

**Note**: Trong đó: [Toán hạng đích], [Toán hạng nguồn]: tương tự lệnh Mov.

 - `Loop      <Nhãn đích>`

 Trong đó: <Nhãn đích> là một nhãn lệnh và nó phải đứng trước lệnh lặp Loop **không quá 126 byte**.

 Tác dụng: Khi gặp lệnh này chương trình sẽ lặp lại việc thực hiện các lệnh sau <Nhãn lệnh> đủ n lần, với n được đặt trước trong thanh ghi **CX**. Sau mỗi lần lặp **CX** tự động giảm 1 đơn vị (**Cx** = **Cx** - 1) và lệnh lặp sẽ dừng khi Cx = 0.

 Lệnh Loop thường được sử dụng để cài đặt các đoạn chương trình lặp với số lần lặp xác định, được cho trước trong thanh ghi **Cx** (tương tự các vòng lặp For trong các ngôn ngữ lập trình bậc cao).    
 - `LEA     [Toán hạng đích],[Toán hạng nguồn]`

 Trong đó: [Toán hạng đích]: Là các thanh ghi 16 bít. [Toán hạng nguồn]: Là địa chỉ của một vùng nhớ hay tên của một biến.

Tác dụng: Lệnh **LEA** (load effective address) có tác dụng chuyển địa chỉ offset của [Toán hạng nguồn] vào [Toán hạng đích]. Lệnh này thường được sử dụng để lấy địa chỉ offset của một biến đã được khai báo trong chương trình. Thanh ghi được sử dụng trong trường hợp này là thanh ghi cơ sở (**BX**) và thanh ghi chỉ mục (**SI** và **DI**).   

 - `Mul     [Toán hạng nguồn]`

 Lệnh **Mul** (Multiply): Thực hiện phép nhân trên số không dấu. Nếu [Toán hạng nguồn] là toán hạng 8 bít thì lệnh sẽ nhân nội dung của [Toán hạng nguồn] với giá trị thanh ghi **AL**, kết quả 16 bít chứa ở thanh ghi **Ax**.   

Nếu [Toán hạng nguồn] là toán hạng 16 bít thì lệnh sẽ nhận nội dung của [Toán hạng nguồn] với giá trị thanh ghi **Ax**, kết quả 32 bít chứa ở cặp thanh ghi **Dx**,**Ax**, phần thấp ở **Ax**, phần cao ở **Dx**. Nếu phần cao của kết quả (**AH** hoặc **DX**) bằng 0 thì các cờ **CF** = 0 và **OF** = 0.

 - `IMul   [Toán hạng nguồn]`

 Lệnh **IMul** (Interger Multiply): Tương tự lệnh **Mul** nhưng thực hiện phép nhân trên hai số có dấu. Kết quả cũng là một số có dấu. 

 - `Div      [Toán hạng nguồn]`

 Lệnh **Div** (Divide): Thực hiện phép chia trên số không dấu. Nếu [Toán hạng nguồn] là toán hạng 8 bít thì lệnh sẽ lấy giá trị của thanh ghi **Ax** (số bị chia) chia cho [Toán hạng nguồn]  (số chia), kết quả thương số chứa trong thanh ghi **Al**, số dư chứa trong thanh ghi **AH**.      

Nếu [Toán hạng nguồn] là toán hạng 16 bít thì lệnh sẽ lấy giá trị của cặp thanh ghi **Dx**:**Ax** (số bị chia) chia cho [Toán hạng nguồn]  (số chia), kết quả thương số chứa trong thanh ghi **Ax**, số dư chứa trong thanh ghi **Dx**.

Nếu phép chia cho 0 xảy ra hay thương số vượt quá giới hạn của thanh ghi **AL** (chia 8 bít) hay **Ax** (chia 16 bít) thì CPU sẽ phát sinh lỗi “Divice overflow”.

 - `IDiv    [Toán hạng nguồn]`

  Lệnh **Idiv** (Integer Divide): Tương tự lệnh  **Div** nhưng thực hiện phép chia trên hai số có dấu. Kết quả cùng là các số có dấu.

  - Trong đó: [Toán hạng nguồn]có thể là thanh ghi hay ô nhớ. Với các lệnh nhân: [Toán hạng đích] ngầm định là thanh ghi **Al** hoặc **Ax**. Với các lệnh chia: [Toán hạng đích] là một trong các thanh ghi đa năng **Ax**, **Bx**,...

  - `Not     [Toán hạng đích]`

   Lệnh **Not** (Logical Not): Thực hiện việc đảo ngược từng bít trong nội dung của [Toán hạng đích]. Lệnh này không làm ảnh hưởng đến các cờ.
   Lệnh Not thường được sử dụng để tạo dạng bù 1 của [Toán hạng đích].    

  - `Or       [Toán hạng đích], [Toán hạng nguồn]`

Lệnh **Or** (Logical Inclusive Or):Thực hiện phép tính logic Or trên từng cặp bít (tương ứng về vị trí) của [Toán hạng nguồn] với [Toán hạng đích], kết quả lưu vào [Toán hạng đích].

Lệnh **Or** thường dùng để thiết lập (= 1) một hoặc nhiều bít xác định nào đó trong một thanh ghi.

 - `Xor     [Toán hạng đích], [Toán hạng nguồn]`

 Lệnh **Xor** (eXclusive OR):Thực hiện phép tính logic Xor trên từng cặp bít (tương ứng về vị trí) của [Toán hạng nguồn] với [Toán hạng đích], kết quả lưu vào [Toán hạng đích].

Lệnh **Xor** thường dùng để so sánh (bằng nhau hay khác nhau) giá trị của hai toán hạng, nó cũng giúp phát hiện ra các bít khác nhau giữa hai toán hạng này.

 - `Test    [Toán hạng đích], [Toán hạng nguồn]`

Lệnh **Test**: Tương tự như lệnh And nhưng không ghi kết quả vào lại [Toán hạng đích], nó chỉ ảnh hưởng đến các **cờ** CF, OF, ZF,...

####4. Cài đặt IDE Emu8086 <a name="4"></a>

 - Emu8086 là chương trình mô phỏng bộ VXL 8086/86 rất hay với đầy đủ chức năng của một text editor, assembler, disassembler, và software emulator, bạn có thể theo dõi trạng thái của thanh ghi, cờ và bộ nhớ khi chương trình đang chạy.

 - Vào <a href="http://www.emu8086.com/">đây</a> để tải.

 - Sau khi giải nén. Ta bắt đầu cài đặt.

![Bước 1](http://i.imgur.com/5S3Kcty.png)

`Next`

![Bước 2](http://i.imgur.com/wWouGHe.png)

`Next`

![Bước 3](http://i.imgur.com/ET6cICC.png)

`Next`

![Bước 4](http://i.imgur.com/zC3m2UB.png)

`Next`

![Bước 5](http://i.imgur.com/HvIsPlE.png)

`Next`

![Bước 6](http://i.imgur.com/zL7fCyY.png)

`Finshed`

 - Sau khi cài đặt xong ta được icon và giao diện ban đầu như sau: ![icon](http://i.imgur.com/3XIjTj7.png)

![Giao diện ban đầu](http://i.imgur.com/gBExLxa.png)


###Ví dụ : Giả sử tại địa chỉ 0100:0120 trong bộ nhớ có chứa một mảng dữ liệu, gồm 100 ô nhớ, mỗi ô là 1 word. Hãy tính tổng nội dung của các ô nhớ này, kết quả chứa trong thanh ghi Dx.
code

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

**Kết Quả**
![](http://i.imgur.com/jayGYqY.png)
