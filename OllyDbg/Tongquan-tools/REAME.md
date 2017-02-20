##Báo cáo

##Tổng quan về hợp ngữ và công cụ Debug  onllyDBG

>	Tài liệu tham khảo : 

 [1.tutorial 1-9](http://bit.ly/2kh0KnB)

 [2. white hat 5(Revese Engineering)](http://bit.ly/2kHqVSR)

>	Thực hiện:  Nguyễn Văn HIệp

>	Cập nhật lần cuối: 20/02/2017
> ---------------------

###Mục Lục
 
> [1. giới thiệu về ollyDBG](#1)
 
  > - [1.1 tại sao lại chọn ollyDBG](#1.1)

 > - [1.2 download và cài đặt onllyDBG](#1.2)

 > - [1.3 các cửa sổ trong ollyDBG](#1.3)

 >    [1.3.1 The DISASSEMBLER Window ](#1.3.1)
 
 >    [1.3.2 The REGISTERs Window](#1.3.2)
 
 >    [1.3.3 The STACK Window](#1.3.3)
 
 >    [1.3.4 The DUMP Window](#1.3.4)
 
 > - [1.4 một số phím tắt cơ bản ](#1.4)

 >[2. hệ thống số và bảng mã ASCII](#2)
 
 > - [2.1 các hệ thống số ](#2.1)
 
 > - [2.2 hệ thống số có dấu](#2.2)
 
 > - [2.3 bảng mã ASCII](#2.3)
 
 > - [2.4 khay nhớ stack](#2.4)

 >[3. chi tiết về các thanh ghi và công dụng](#3)
 
 > - [3.1 các thanh ghi chính ](#3.1)
 
 >    <ul>
 >   <li>[3.1.1 thay đổi số liệu các thanh ghi](#3.1.1)</li>
 >   </ul>

 > - [3.2 các thanh ghi cờ](#3.2)

 >[4. chi tiết về các câu lệnh ASM thường dùng](#4)

 > - [4.1 lệnh NOP](#4.1)

 > - [4.2 lệnh lệnh PUSH](#4.2)

 > - [4.3 lệnh POP](#4.3)

 > - [4.4 lệnh PUSHAD](#4.4)

 > - [4.5 lệnh POPAD](#4.5)

 > - [4.6 lệnh MOV, MOVSX và MOVZX](#4.6)

 > - [4.7 lệnh LEA](#4.7)

 > - [4.8 lệnh XCHG ](#4.8)

 > - [4.9 lệnh INC và DEC](#4.9)

 > - [4.10 lệnh ADD và ADC ](#4.10)

 > - [4.11 lệnh SUB và SBB](#4.11)

 > - [4.12 lệnh MUL và IMUL](#4.12)

 > - [4.13 lệnh XADD](#4.13)

 > - [4.14 lệnh NEG](#4.14)

 > - [4.15 các lệnh logic](#4.15)
 
 >  <ul>
 >  <li>[4.15.1 lệnh AND](#4.15.1)</li>
 
 >  <li>[4.15.2 lệnh OR ](#4.15.3)</li>
 
 >  <li>[4.15.3 lệnh XOR](#4.15.4)</li>
 
 > <li>[4.15.4 lệnh NOT](#4.15.4)</li>
 >  </ul>

 > [5. các lệnh so sánh và nhảy ](#5)
 
 > - [5.1 lệnh nhảy có điều kiện CMP](#5.1)

 > - [5.2 lệnh TEST](#5.2)
 
 > - [5.3 lệnh nhảy không điều kiện JMP](#5.3)

 > - [5.4 lệnh JE/JZ](#5.4)

 > - [5.5 lệnh JNE/JNZ](#5.5)

 > - [5.6 lệnh JS](#5.6)

 > - [5.7 lệnh JP/JPE, và lệnh JNP/JNPE](#5.7)

 > - [5.8 lệnh JO/JNO ](#5.8)

 > - [5.9 lệnh JB](#5.9)

 > - [5.10 lệnh JBE](#5.10)

 > - [5.11 lệnh JL](#5.11)

 >[6. các lệnh với hàm và cấu trúc (CALL và RET) ](#6)

 >[7. các lệnh liên quan đến chuỗi, vòng lặp và các chế độ địa chỉ](#7)

 > - [7.1 giới thiệu về vòng lặp và lệnh LOOP](#7.1)

 > - [7.2 lệnh MOVS](#7.2)

 > - [7.3 lệnh REP](#7.3)

 > - [7.4 lệnh LODS](#7.4)

 > - [7.5 lệnh STOS](#7.5)

 > - [7.6 lệnh CPMS](#7.6)

 > - [7.7 các chế độ địa chỉ ](#7.7)

 >[8. các thuật ngữ cơ bản, làm việc với API và patch thông qua cờ](#7)

 >[9. patch thông qua sửa code ](#9)

 > ------------------ 

###1. giới thiệu về ollyDBG <a name="1"></a>

####1.1 tại sao lại là onllyDBG <a name="1.1"></a>
 -  Ở đây chúng ta sẽ không bàn luận đến việc tạo ra một công cụ khác hay hơn, mạnh hơn Ollydbg cũng như không để cập tới việc chỉnh sửa lại một chương trình đã quá nổi tiếng từ lâu là SoftIce, chỉ đơn giản là những tín đồ cuồng tín của SoftIce đang dần dần chuyển qua xài Ollydbg bởi tính dễ dùng, không gây crash máy bất thình lình như SoftIce, được hỗ trợ bởi nhiều teams trên thế giới thông qua các Plugins cũng như các bản Ollydbg được mod lại nhằm chống lại các cơ chế anti-debug cũng như anti-Ollydbg. chúng ta sẽ được tìm hiểu về ollyDBG sau đây.

####1.2 download và cài đặt <a name="1.2"></a>
 
 - nhiệm vụ đầu tiên của chúng ta bây giờ là gì ? Do đây là tut viết về Olly nên việc chúng ta phải làm là đi tìm Olly ở đâu để còn load về mà xài. Thứ nhất bạn có thể lên home site của Olly là ollydbg.de để download, còn không thì trong REA có đưa rất nhiều link để download Ollydbg.
 
 - có một cách khác để download ollyDBG là nhấy vào [đây](http://www45.zippyshare.com/v/98256208/file.html) và tải nó về .
 
 - Khi download được Olly về rồi thì rất đơn giản chỉ việc extract nó ra rồi sử dụng, tôi khuyên bạn nên để chung tất cả công cụ liên quan đến RE, Cracking vào 1 thư mục, ví dụ như của tôi trên hình minh họa, như thế ta dễ dàng quản lý hơn. Okie coi như bạn đã có Ollydbg, chúng ta chỉ việc Run cái file OLLYDBG.exe là Olly hoạt động liền, không phức tạp về mặt cài đặt cũng như sử dụng như SoftIce. Giao diện của Ollydbg như sau :


![image1](http://i.imgur.com/PNaBTxg.png)

 -  Phần tiếp theo, tôi sẽ giới thiệu tới các bạn chi tiết các cửa số chính trong Ollydbg và để minh họa cho các phần sau của bài viết, ta sẽ sử dụng một Crackme rất nổi tiếng đó là : CRACKME.EXE của tác giả CRUEHEAD. Để load crackme này vào trong Olly ta nhấn chuột vào biểu tượng sau hoặc vào File > Open (or F3) :
 
 ![image2](http://i.imgur.com/EgUJj0x.png)

 - chúng ta có thể tải các file crackme tại [đây](http://www.woodmann.com/crackz/Archives/Crackmes.zip)

 - chọn crackme 1 để thực hành loạt bài này .

 - ![](http://i.imgur.com/4bMGDKS.png)

 - kết quả sau khi load ta được như sau :

 ![](http://i.imgur.com/f3T5Qg9.png)

 - Chắc các bạn nhìn vào sẽ cảm thấy choáng ngợp, không biết phải bắt đầu từ đâu. Hic ngày đầu tiên khi tôi load một target vào trong Olly, nhìn ngược nhìn xuôi cũng không hiểu gì hết luôn hehe, cứ ngồi ngắm mãi vì chẳng biết làm gì hơn. Nhưng không sao mọi thứ đều có cách giải quyết, khi chưa biết thì phải tìm tài liệu mà đọc, khi đọc mà không hiểu lúc đấy hẵng đi hỏi. Nhưng hỏi cũng phải biết đường mà hỏi, nếu không sẽ chẳng bao giờ bạn nhận được câu trả lời mà có khi còn khiến người khác cảm thấy bực mình. Tôi sẽ cùng các bạn tìm hiểu từng cửa sổ một của Olly. Như các bạn nhìn thấy ở trên màn hình chính của Olly được phân ra làm 5 cửa sổ chính, mỗi cửa sổ có một nhiệm vụ và một tên riêng :

 ![](http://i.imgur.com/kfu92m3.png)

####1.3 các cửa sổ chính trong olly<a name="1.3"></a>

 - **ở đây chúng ta thấy 4 cửa sổ lớn :**

 – **The Disassembler Window** : Ở cửa sổ này các bạn có thể nhìn thấy các đoạn code của chương trình ở dạng ngôn ngữ asm, và đồng thời tại cửa sổ này các bạn cũng có thể chú thích cho từng từng dòng mã asm .

 – **The Registers Window** : Đây là cửa số chứa thông tin chi tiết về các thanh ghi như eax, ebx, ecx v….v…..Các cờ trạng thái cũng được quản lý tại cửa sổ này

 – **The Dump Window** : Tại cửa sổ này bạn có thể xem hoặc chỉnh sửa theo 2 dạng là hex và Ascii bộ nhớ của chương trình mà bạn muốn debug

 – **The Stack Window** : Một cửa sổ không kém phần quan trọng , mọi thứ trước khi được thực hiện phải được nạp vào Stack.

 - Cuối cùng có một cửa sổ nằm bên dưới cửa sổ Disassembler Window : Chúng ta gọi nó là The Tip Window .Khi bạn đang ở tại một dòng code nào đó trong quá trình debug , Olly sẽ cho bạn thấy thông tin chi tiết về dòng code đó . Lấy ví dụ đơn giản như sau : nếu bạn debug tới dòng lệnh ” mov eax , dword ptr [123]” . Thì cửa sổ này sẽ cho bạn biết  được giá trị hay con số nào đang được lưu giữ tại [123] . Và còn nhiều điều thú vị khác nữa mà cửa sổ này sẽ mang lại cho chúng ta .

 - Trên đây là những gì tổng quan nhất mà các bạn nên biết. Phần dưới đây tôi sẽ đi vào giới thiệu về chức năng của từng cửa sổ một thông qua các hình minh họa, tất nhiên không thể giới thiệu chi tiết hết được, chúng ta sẽ tìm hiểu dần dần trong từng trường hợp cụ thể ở các loạt tuts sau

#####1.3.1 The DISASSEMBLER Window  :<a name="1.3.1"></a>

 - Đây là cửa sổ chính đầu tiên của Olly và là cửa sổ rất quan trọng, chúng ta sẽ làm việc rất nhiều trên cửa sổ này. Khi bạn muốn debug một chương trình, bạn load file thực thi của chương trình đó vào trong Olly.Các chương trình mà bạn load vào Olly là những chương trình có thể được code bằng những ngôn ngữ khác nhau như : VB, VC++, Borland Delphi hay MASM nhưng tại cửa sổ này toàn bộ code của chương trình sẽ được list ra dưới dạng các mã ASM. Theo mặc định của Olly thì bất cứ chương trình nào mà bạn load vào Olly sẽ được Olly tiến hành phân tích toàn bộ code chính của chương trình đó và đưa ra các comment thích hợp. Bạn có thể tùy biến chức năng này thông qua hình minh họa dưới đây :

 ![1.10](http://i.imgur.com/zZBVC9a.png)
 ![1.11](http://i.imgur.com/rI7glOD.png)

 - Nếu như bạn chọn sử dụng chức năng này của Olly thì những gì xuất hiện trên cửa sổ bạn sẽ giống với những hình minh họa trước. Còn nếu như bạn không chọn, chúng ta sẽ thấy ngay được sự khác biệt, Olly sẽ không tự động phân tích chương trình nữa công việc phân tích này chúng ta sẽ phải thực hiện một cách manual sau khi chương trình được load vào trong Olly. Okie, tôi thử bỏ chọn và load lại Crackme vào trong Olly, ta sẽ được như sau :

![1.12](http://i.imgur.com/4ImSmU1.png)

 - Như các bạn thấy trên hình trên, nếu như chúng ta không chọn chức năng tự động phân tích của Olly thì sẽ thấy các thông tin trong phần Comment đã bị lược bỏ đi khá nhiều, điều này dẫn đến việc khó khăn trong quá trình debug chương trình. Tuy nhiên không phải lúc nào chức năng này cũng hoạt động một cách hiệu quá, nhiều khi chúng ta để cho Olly tự động phân tích sẽ lại dấn đến một kết quả hoàn toàn ngược lại, đoạn code được phân tích và thể hiện ra không được chính xác, ví dụ như trường hợp dưới đây chúng ta sẽ nhận được đoạn code toàn chứa DB :
 ![](http://i.imgur.com/9wSjIEK.png)

 - Trong trường hợp như thế này chúng ta có thể thực hiên một cách manual để remove những gì mà Olly đã tiến hành phân tích chỉ đơn giản bằng cách nhấn chuột phải tại màn hình này và chọn Analysis > Remove analysis from module.
 ![1.13](http://i.imgur.com/LMVcAvO.png)
 và chúng ta được đoạn code chính xác như sau:

 ![1.14]()

 - Do dó trong quá trình làm việc với Olly các bạn nên linh hoạt trong quá trình sử dụng chức
năng này. Ngoài ra còn một phần khác cũng không kém phân quan trọng, như các bạn
thây trên hình minh họa Olly của tôi các câu lệnh được phân biệt màu sắc một cách rõ ràng,
có thể các bạn không chú trọng đến vân đề này nhưng theo tôi việc chúng ta phân biệt
cũng như tinh chỉnh lại màu sắc trong Olly sẽ khiến cho chúng ta nhận biết các câu lệnh dễ
dàng hơn cũng như phân nào thể hiện năng khiếu thẩm mỹ của bạn . để tinh chỉnh lại
màu sắc trong Olly các bạn vào các Tabs sau :

![1.15](https://kienmanowar.files.wordpress.com/2008/09/olly1_15.png)

![1.16](https://kienmanowar.files.wordpress.com/2008/09/olly1_16.png)

#####1.3.2 The REGISTERs Window :<a name="1.3.2"></a>

 - Một cửa sổ quan trọng tiếp theo, đó chính là cửa sổ Register. Như đã nói đây là cửa sổ chứa thông tin chi tiết về các thanh ghi như eax, ebx, ecx v…v… Các cờ trạng thái cũng được quản lý tại cửa sổ này.
 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_171.png)

 - Cửa số này sẽ cung cấp cho chúng ta rất nhiều thông tin trong quá trình chúng ta làm việc cùng Olly. Nếu như chỉ nhìn vào hình minh họa ở trên các bạn chắc cũng sẽ như tôi cảm thấy rằng nó sẽ không có ý nghĩa nhiều lắm, nhưng kì thực đây là nơi cung cấp nhiều thông tin rất hữu ích.

#####1.3.3 The STACK Window :<a name="1.3.3"></a>

 - Trước tiên chúng ta sẽ đi tìm hiểu sơ qua về Stack. Đây là nơi lưu trữ tạm thời các dữ liệu và địa chỉ, nó là một cấu trúc dữ liệu một chiều. Các phần tử được cất vào và lấy ra từ một đầu của cấu trúc này, tức là nó được xử lý theo phương thức “vào trước, ra sau” (LIFO : Last In First Out). Phần tử được cất vào cuối cùng gọi là đỉnh của Stack. Các bạn có thể hình dung Stack như là một chồng đĩa, chiếc đĩa được đặt lên cuối cùng sẽ nằm trên đỉnh và chỉ có nó mới có thể được lấy ra đầu tiên. Hai thanh ghi chính làm việc với Stack là ESP và EBP. Theo mặc định trong Olly, Stack được biểu diễn theo thanh ghi ESP tuy nhiên chúng ta có thể luân chuyển qua lại giữa ESP và EBP bằng cách nhấn chuột phải và chọn như hình sau :
 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_18.png)

#####1.3.4 The DUMP Window: <a name="1.3.4"></a>

  - Đây là cửa số hiện thị nội dung của bộ nhớ hoặc file. Ta có thể chọn nhiều định dạng khác nhau để biểu diễn nội dung của memory trong cửa số này : byte, text, integer, float, address, disassembly hoặc PE Header. Cửa sổ này cho phép chúng ta tìm kiếm cũng như thực hiện các chức năng chỉnh sửa, thiết lập các Break points v..v…
  ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_19.png)

 - Vậy là chúng ta đã dạo qua 1 vòng các cửa sổ chính của Olly, tuy nhiên bên cạnh đó Olly còn có rất nhiều cửa sổ khác mà chúng ta không nhìn thấy một cách trực tiếp như các cửa sổ trên được.Chúng ta phải truy cập vào các cửa sổ đó thông qua Menu như hình minh họa dưới đây :
 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_20.png)


 - Chúng ta sẽ lướt qua chức năng của từng cửa sổ một.

 _  Nút L dùng để mở cửa sổ Log của Olly, cửa sổ này cho chúng ta thấy những thông tin mà Olly ghi lại. Theo mặc định thì cửa số này sẽ lưu các thông tin về các module, import library hoặc các Plugins được load cùng chương trình tại thời điểm đầu tiên khi ta load chương trình vào Olly. Bên cạnh đó cửa sổ này cũng ghi lại các thông tin về các Break points mà chúng ta đặt trong chương trình. Trong trường hợp crackme của chúng ta, ta có được thông tin như sau :
 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_21.png)

 _ Nút E dùng để mở cửa sổ Executables, cửa sổ này sẽ đưa ra danh sách những file có khả năng thực thi được chương trình sử dụng như file exe, dlls, ocxs , v..v..

 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_22.png)

 - Tại cửa sổ này nếu như bạn click chuột phải sẽ thấy có rất nhiều tùy chọn khác nhau, trong khuôn khổ có hạn của bài viết không thể nói hết được. Sẽ có những phần tiếp theo đề cập đến chúng.

 _ Nút M dùng để mở cửa sổ Memory, cửa sổ này sẽ cho chúng ta thông tin về bộ nhớ đang được sử dụng bởi chương trình của chúng ta và còn nhiều thông tin bổ ích khác nữa :
 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_23.png)

 Tại cửa sổ này chúng ta cũng có thể sử dụng tính năng Search để tìm kiếm thông tin về các strings, các đoạn hexa cụ thể hay unicode v..v.. thêm vào đó nó còn cung cấp cho chúng ta những kiểu thiết lập Break points khác nhau tại các Sections. Việc thiết lập các BPs là tùy thuộc vào yêu cầu và mục đích của chúng ta.

 _Nút K để mở cửa sổ Call Stack, hiển thị một danh sách các lệnh call mà chương trình của chúng ta đã thực hiện khi chúng ta Run bằng F9 và dùng F12 để tạm dừng chương trình.

 _ Nút B để mở cửa sổ Break Points, cửa sổ này sẽ hiển thị tất cả các BPs mà chúng ta đặt trong chương trình. Tuy nhiên nó chỉ hiện thị các BPs được set bằng cách nhấn F2 thôi, còn các dạng BPs khác như : hardware breakpoint hoặc memory breakpoints thì không được liệt kê ra ở đây.

 _ Nút R để mở cửa sổ References, cửa sổ này là kết quả cho những gì chúng ta thực hiện chức năng Search trong Olly, kết quả sẽ được hiện ra ở đây :

 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_24.png)

 - 1 yêu cầu rất quan trọng ngoài việc bạn biết sử dụng Olly ra thì bạn còn phải biết về Asm language, nếu không biết về nó thì hii các bạn nên dành thời gian để tìm hiểu một số kiến thức cơ bản trước khi đọc tiếp các phần sau của bài viết. Ngoài ra để các bạn dễ làm quen hơn trong các phần sau tôi sẽ cố gắng hệ thống lại

 **Cấu hình Olly thành JIT (Just-in-time debugging)**

  - Khi một số chương trình thực thi và nó tạo ra Exception, Windows có thể gọi Registered Debugger (các debuggers được cấu hình thành JIT) và attach nó vào chương trình. Tính năng này được gọi là Just-in-time debugging.

 - Một vài JIT debuggers dừng lại tại System breakpoint. Ollydbg thì tiếp tục thực thi cho đến khi nó đi đến câu lệnh đã tạo ra Exception.

 - Để cấu hình Ollydbg trở thành 1 JIT bạn làm như sau :

 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_25.png)

####1.4 một số phím tắt làm việc với ollyDBG. <a name="1.4"></a>

 - F7 : Khi bạn nhấn F7 sẽ thực thi từng dòng lệnh 1. Nếu trong quá trình Trace mà gặp lệnh Call thì sẽ đi vào trong lòng của lệnh Call đó và thực thi từng câu lệnh trong lệnh Call này cho đến khi gặp lệnh Retn để trở lại chương trình chính, tức là câu lệnh tiếp theo sau lệnh Call.

 - F8 : Cũng tương tự như F7 nhưng có 1 điểm khác biệt là khi Trace code, nếu như gặp lệnh Call nó bỏ qua không cần quan tâm các lệnh bên trong lệnh Call mà thực thi luôn lệnh Call đó và dừng lại tại câu lệnh tiếp theo dưới lệnh Call.

 - F2 : Đặt một Break point trong chương trình. Vậy Break point là gì , đơn giản nó chỉ là việc chúng ta tạo 1 điểm ngắt trong chương trình theo một điều kiện nào đó để khi thực thi chương trình, nếu thỏa điều kiện mà chúng ta đặt ra thì chương trình sẽ dừng lại tại vị trí mà chúng ta đã đặt BP.Bây giờ tôi muốn đặt một BP tại hàm Call gọi tới API: LoadIconA. Tức là khi tôi thực thi chương trình, chương trình gọi tới hàm này thì ngay lập tức nó sẽ dừng lại tại đây.Việc tiếp theo là tôi có thể tùy biến lại hàm này theo mục đích của tôi, chẳng hạn tôi NOP nó để chương trình không còn gọi đến hàm này nữa v..v.. Để làm được điều này bạn nhấn chuột tại vị trí cần Set BP, sau đó nhấn F2. Chỗ chúng ta Set BP sẽ được đánh dấu màu đỏ :  

 ![](https://kienmanowar.files.wordpress.com/2008/09/olly1_26.png)

 - Để bỏ BP mà chúng ta đã set thì chỉ việc chọn vị trí đánh dấu màu đỏ và nhấn F2 hoặc nháy đúp vào vị trí hex dump trên dòng đó.

 - F9 : Cho phép thực thi chương trình trong chế độ Debug, tương tự như việc chúng ta nhấp đúp chuột vào chương trình để thực thi nó. Tuy nhiên khác với việc nhấp đúp chuột, nếu chúng ta nhấn F9 thì Olly sẽ tìm xem có BP nào được Set hay không, chương trình có tung ra các Exception gì không, hay nếu chương trình có cơ chế chống Debug thì nó sẽ terminate ngay lập tức. Nếu như không có bất kì cản trở nào thì chương trình sẽ Run hoàn toàn và trên status bar của Olly sẽ báo cho chúng ta biết điều này

 - F12 : Tạm dừng chương trình lại.

###2. hệ thống số và bảng mã ASCII <a name="2"></a>

####2.1 các hệ thống số <a name="2.1"></a>

 - có ba hệ thống số được sử dụng nhiều nhất đó là Hệ nhị phân, Hệ mười và cuối cùng là hệ thập lục phân.Chúng ta sẽ đi lần lượt định nghĩa về từng hệ thống này.

 -Hệ nhị phân : Trong hệ đếm nhị phân cơ số là 2 và nó chỉ có hai chữ số là 0 và 1.

 - Hệ mười (thập phân) : Có thể nói đây là một hệ thống được chúng ta sử dụng nhiều nhất trong đời sống hàng ngày.Hệ này bao gồm mười chữ số bắt đầu từ 0 đến 9. Hệ đếm này là hệ đếm mà chúng ta quen thuộc nhất.

 - Hệ mười sáu : Các số dưới dạng nhị phân thường là dài và khó nhớ. Việc chuyển đổi các số thập phân sang nhị phân thường khó. Khi chúng ta viết chương trình hợp ngữ chúng ta thường sử dụng cả hai hệ đếm là : nhị phân và thập phân, và cả một hệ đếm thứ ba là hệ 16 hay còn gọi tắt là số hex. Số hex cho phép chúng ta chuyển đổi một cách dễ dàng sang số ở hệ nhị phân và ngược lại.

 - **Note** : Để đổi số hex sang số nhị phân chúng ta chỉ việc biểu diễn các chữ số của nó dưới dạng nhị phân. Còn đổi số nhị phân sang số hex, thì ta nhóm 4 chữ số của số nhị phân lại theo thứ tự lần lượt từ phải qua trái. Sau đó chuyển thành số hex tương ứng.

 - Hệ đếm hex là hệ đếm có cơ số 16 cho nên các chữ số của nó là : 0-9, A-F. (Vì hết các kí hiệu chữ số để biểu diễn nên người ta dùng thêm các chữ cái để biểu diễn: các chữ cái từ A – F tương ứng biểu diễn các số từ 10 – 15).

 - Khi bạn muốn làm quen với công việc debug trong Olly thì điều đầu tiên tôi khuyên bạn nên làm quen với các hệ thống số ở trên, Olly chủ yếu sử dụng hệ 16. Bên cạnh đó các bạn cũng phải học các phương pháp chuyển đổi đơn giản giữa các hệ số với nhau để tiện cho quá trình bạn làm việc. Có thể các bạn sẽ cho lời tôi nói là thừa bởi vì ngày nay có quá nhiều công cụ hỗ trợ cho chúng ta làm việc này, nhưng theo tôi đây vẫn là những kiến thức tiên quyết vì công cụ chỉ là hỗ trợ để chúng ta làm việc nhanh chóng mà thôi, còn muốn hiểu sâu, rộng thì chúng ta không nên bỏ qua những chi tiết dù là vụn vặt nhất.

 - Ở đây trong bài viết này, tôi coi như các bạn đã tự mình trang bị những kiến thức cơ bản rồi. Do đó để dễ dàng hơn cho chúng ta khi làm việc với các hệ thống số, Windows cung cấp cho chúng ta một công cụ khá mạnh mà đôi khi ít người để ý mà thậm chí có khi còn không biết là nó hỗ trợ cho chúng ta các tính năng liên quan đến việc chuyển đổi J, đó chính là tiện ích Calculator. Có nhiều cách thức để mở chương trình này nhưng cách nhanh nhất là vào menu Run và gõ Calc.exe (thậm chí chỉ cần gõ Calc cũng mở được).

  ![](http://i.imgur.com/s7DOsNB.png)
 ![](http://i.imgur.com/d2VVsdy.png)
 - Như bạn thấy trên hình sau khi chúng ta gõ Calc thì ngay lập tức công cụ Calculator sẽ hiện ra dưới dạng một máy tính chuẩn hệt như cái máy tính bình thường mà bạn hay sử dụng. Để có thể chuyển sang sử dụng các tính năng chuyên nghiệp hơn liên quan tới các số hệ nhị phân và hệ 16 cũng như các phép tính liên quan tới hai hệ số này, bạn làm như trên hình vẽ (View > Scientific). Ta có được như sau : 
![](http://i.imgur.com/tNoxegn.png)
 - Trong hình minh họa bên trên, bạn thấy hệ thống số được sử dụng mặc định là hệ 10 (Dec).Tại sao nó lại mặc định như vậy? Một câu trả lời rất đơn giản là vì từ lúc cha sinh mẹ đẻ chúng ta tới giờ chúng ta sử dụng hệ 10, hệ đếm chuẩn của loài người  nên chương trình để default như vậy là hoàn toàn hợp lý. Các bạn có thể luân chuyển sang các hệ khác rất đơn giản thông qua các tùy chọn. Lấy một ví dụ, tôi muốn chuyển một con số từ hệ 10
sang hệ 16 thì tôi làm thế nào? Tại màn hình Calculator bạn chọn Dec và gõ vào một con số bất kì, ví dụ : 1111)

![](http://i.imgur.com/E3uK3gP.png)
 - Để chuyển sang hệ Hex bạn chỉ việc nhấp chọn vào tùy chọn Hex tại cửa màn hình của Calculator, ngay lập tức số ở hệ 10 của bạn sẽ được chuyển sang số ở hệ 16 một cách chính xác.
 ![](http://i.imgur.com/zdJJG9x.png)
 - Trên hình trên bạn đã thấy khi ở hệ 10 thì các chữ cái từ A – F đều bị disable. Khị bạn chọn
chuyển sang hệ hex thì các chữ cái này sẽ được enable lên để phục vụ cho các bạn làm việc
ở hệ hex. Việc chuyển đôi qua lại các hệ số khác cũng làm tương tự như trên, qua đó bạn
thấy công cụ này đã đơn giản hóa cho chúng ta rất nhiều các công việc liên quan đến việc
chuyển đổi bằng tay.Tất cả những gì bạn phải làm là gõ số và nhấn chọn.

####2.2 hệ thống số có dấu <a name="2.2"></a>

 - Phần cứng của máy tính cần giới hạn kích thước của các số để có thể lưu nó trong các thanh ghi hay các ô nhớ. Trong hệ hex vấn đề sẽ nảy sinh khi chúng ta muốn biểu diễn một số âm ví dụ như -1 chẳng hạn, chúng ta không thể làm bằng cách thêm một dấu trừ ở phía trước con số giống như ở trong hệ 10 được.Vì nếu làm thế thì đơn giản quá rồi, đâu cần phải đề cập đến vấn đề này làm gì và vì hệ thống máy tính mà chúng ta đang sử dụng chỉ làm việc với hai số 0 và 1 mà thôi, cho nên để biểu diễn một số có dấu phải có qui định khác.
 - Do chúng ta đang làm việc với hệ thống 32 bít cho nên dải số của nó sẽ được biểu diễn ở hệ hex là từ 00000000 – FFFFFFFF.Dải này sẽ được cắt nửa ra, một nửa dùng để biểu diễn số dương và một nửa dùng để biểu diễn số âm. Vậy số dương sẽ bắt đầu từ 00000000 và kết thúc là 7FFFFFFF, còn số âm sẽ bắt đầu từ 80000000 và kết thúc là FFFFFFFF. Vậy làm thế nào để nhận biết đâu là số âm và đâu là số dương? Các bạn hãy để ý đến một bit đặc biệt, đó là bit nằm ở tận cùng bên trái hay còn được gọi với một cái tên khác là bit có trọng số nặng nhất (MSB - Most significant bit). Tương tự như vậy ta cũng có một bit có trọng số thấp nhất hay còn gọi là bít nhẹ nhất đó là số nằm ở tận cùng bên phải (LSB – Least Significant bit). Nếu như bit có trọng số cao nhất là 0 thì số đó được hiểu là số dương. Còn nếu như bít có trọng số cao nhất là 1 thì số được được hiểu là số âm. Bằng 0 hay bằng 1 là khi chúng ta biểu diễn số đó dưới dạng nhị phân. Các số âm trong máy tính được lưu ở dạng số bù 2
 **(Note: số bù 2 có được bằng cách đảo bít của một số nguyên và cộng với 1).**
 - Theo đó ta có được dải biểu diễn như sau :
SỐ DƯƠNG :
```
00000000h hệ 16 – 0 hệ 10
00000001h hệ 16 – 1 hệ 10
_________________________
7FFFFFFFh hệ 16 – 2147483647 hệ 10 (Số dương lớn nhất)
```
SỐ ÂM :
```
FFFFFFFFh hệ 16 - -1 hệ 10
FFFFFFFEh hệ 16 - -2 hệ 10
__________________________
80000000h hệ 16 - -2147483647 hệ 10 (Số âm nhỏ nhất)
```
- Tôi sẽ làm một ví dụ chuyển đổi sang số bù 2 để các bạn thấy được một cách trực quan nhất. Giả sử tôi có số dương là 1 , giờ tôi muốn biểu diễn số -1 tôi sẽ làm thế nào. Để đơn giản tôi chỉ làm mẫu với số 16 bit.

 - Đầu tiên ta tìm số bù 1 của 1 (có được bằng cách đảo bít) :
 1. Biểu diễn 1 ở dạng nhị phân : 0000 0000 0000 0001
 2. Tìm số bù 1 của 1 : 1111 1111 1111 1110
 
 - Tìm số bù 2 của 1 bằng cách lấy bù 1 đem cộng với 1 :
 1. Theo kết quả ở trên, bù 1 của 1 : 1111 1111 1111 1110
 2. Cộng với 1 : +1
 3. Kết quả là số bù 2 : 1111 1111 1111 1111
 Đem số bù hai này chuyển qua hệ Hex các bạn sé có được là : FFFFh

 - Trong Olly chúng ta có thể giải quyết mọi vấn đề liên quan thông qua Plug-in : Command Bar. Để sử dụng nó cũng rất đơn giản, bạn làm như hình minh họa dưới đây : 
![](http://i.imgur.com/tQeGoEK.png)
 - Rất trực quan và dễ hiểu, bạn không biết giá trị ở hệ 10 của 7FFFFFFFh là bao nhiêu. Trong Plug-in Command Bar bạn chỉ việc gõ ? và theo sau là biểu thức hay giá trị mà bạn cần biết thông tin. Ta thử thực hiện phép chuyển đổi với giá trị 80000000h xem sao? Như ta
biết ở trên, giá trị 80000000h biểu diễn một số âm, nhưng khi sử dụng Command Bar để chuyển đổi thì kết quả ta có được không như những gì chúng ta mong đợi, đây là một bug của Plug-in Command Bar.
![](http://i.imgur.com/kG94Cjc.png)
 - Chúng ta có thể giải quyết vấn đề này thông qua cửa số Register. Giả sử tại cửa số này tôi có giá trị thanh ghi EAX là 80000000h. Tôi muốn xem giá trị của nó ở hệ mười thì phải làm thế nào và giá trị âm dương của nó ra sao?
![](http://i.imgur.com/OpECYiV.png)
 - Để làm được điều này, nhấn chuột phải lên thanh ghi EAX và chọn Modify.Như hình minh
họa dưới đây :
![](http://i.imgur.com/ZNuUkds.png)
 - Cửa sổ Modify sẽ hiện ra cho phép chúng ta muốn thay đổi thanh ghi EAX thế nào tùy thích. Trong trường hợp này kết quả của 80000000h đúng như những gì chúng ta trông đợi đó là -214783648
 ![](http://i.imgur.com/5obFkYi.png)
 - Chúng ta thử sửa giá trị 80000000 đi và thay vào đó là một giá trị khác xem thế nào.
 ![](http://i.imgur.com/iUCGoYb.png)
 - Ok, sau khi chỉnh sửa các bạn có thể lưu lại giá trị mà bạn đã chỉnh hoặc bỏ bằng cách nhấn Cancel.

####2.3 bảng mã ASCII <a name ="2.3"></a>

 - ACSII Không phải mọi số liệu mà máy tính xử lý đều là các con số, các thiết bị ngoại vi như màn hình, bàn phím, máy in đều có xu hướng làm việc với kí tự.Cũng như tất cả mọi loại dữ liệu khác, các kí tự cần phải được biểu diễn thành dạng nhị phân để máy tính có thể xử lý chúng. Một kiểu mã hóa thông dụng nhất cho các kí tự đó là mã ASCII. Khi làm việc trong Ollydbg bắt buộc bạn cũng phải tìm hiểu sơ qua về bảng mã này. Bạn phải hiểu nó để có thể làm các bước chuyển đổi giữa kí tự ở dạng hex sang kí tự cũng như những symbols tương ứng. Dưới đây là bảng mã ACSII mà bạn có thể tham khảo : 

 ![](http://i.imgur.com/m49C6we.png)

 -  Một ví dụ với sự giúp đỡ của **Plug-in Command Bar** sẽ cho bạn thấy được kết quả trực quan :
 ![](http://i.imgur.com/tG1vO9m.png)
 - Ngoài ra cửa sổ **Dump** trong Olly cũng giúp bạn có được những thông tin quan trọng trong quá trình bạn Debug target :
 ![](http://i.imgur.com/b8TebeV.png)

####2.4 khay nhớ và stack <a name="2.4"></a>

 - STACK, nó là một vùng của bộ nhớ dùng để lưu trữ tạm thời các dữ liệu và địa chỉ. Stack làm việc theo nguyên lý **LIFO (Last In, First Out)**, tức là phần tử nào được cất vào cuối cùng trong stack sẽ là phần tử được lấy ra đầu tiên. Bạn cứ tưởng tượng như bạn đang xếp một chồng đĩa, thì chiếc đĩa cuối cùng mà bạn xếp sẽ nằm trên cùng, tức là đỉnh của Stack nó sẽ là chiếc đĩa được lấy ra đầu tiên nếu như bạn muốn lấy tiếp chiếc đĩa thứ hai bên dưới nó. Cấu trúc dữ liệu làm việc theo kiểu LIFO này là ý tưởng cho việc lưu trữ những dữ liệu tạm thời, hoặc những thông tin không cần thiết phải được lưu trữ trong một thời gian dài. Stack thường là nơi lưu trữ các local variables, những lời gọi hàm (function calls) và các thông tin khác được sử dụng để dọn dẹp stack sau khi một hàm hay một thủ tục được gọi.

 - Một tính năng quan trọng khác của stack là nó *grows down* theo không gian địa chỉ: có nghĩa là càng nhiều dữ liệu được thêm vào trong stack, nó được thêm vào tại các giá trị địa chỉ thấp hơn theo cơ chế tăng dần. Xem hình minh họa về sơ đồ không gian bộ nhớ :
 ![](http://i.imgur.com/58GPoAv.png)
 - Làm việc với Stack có 2 thanh ghi chính là ESP và EBP, và các câu lệnh PUSH và POP. Trong Ollydbg bạn có thể quan sát thấy cửa sổ Stack rất trực quan : 
 ![](http://i.imgur.com/9MKlJtL.png)

###3. chi tiết về các thanh ghi và công dụng <a name="3"></a>

####3.1 các thanh ghi chính <a name="3.1"></a>

  **1) thanh ghi ESP**
 - Thanh ghi đầu tiên mà tôi muốn giới thiệu tới các bạn đó chính là thanh ghi **ESP (con trỏ ngăn xếp – Stack pointer)**. Thanh ghi này luôn trỏ tới đỉnh hiện thời của ngăn xếp. Các bạn xem hình minh họa dưới đây :
 ![](http://i.imgur.com/SbcYyuC.png)

 - Như các bạn thấy trên hình, giá trị của thanh ghi ESP là 0x0013FFC4h, quan sát tại cửa sổ Stack các bạn sẽ thấy giá trị này đang nằm tại đỉnh của Stack.
 ![](http://i.imgur.com/5Y4FkBR.png)
 - Thanh ghi ESP trỏ tới địa chỉ vùng nhớ nơi mà thao tác stack tiếp theo sẽ được thực hiện.

 **2. thanh ghi EIP**
  - Để truy cập đến các lệnh, 8086 sử dụng thanh ghi EIP (Instruction Pointer). Đây là một thanh ghi rất quan trọng, nó được cập nhật mỗi khi có một lệnh được thực hiện để sao cho nó luôn trỏ đến lệnh tiếp theo. Khác với các thanh ghi khác EIP không thể bị tác động trực tiếp bởi các lệnh, do đó trong một lệnh chúng ta sẽ thấy thường không có mặt thanh ghi EIP như một toán hạng. Ví dụ quan sát cửa sổ Registers trong Olly chúng ta thấy như sau :
  ![](http://i.imgur.com/t4wHGMV.png)
  - Chúng ta thấy rằng thanh ghi EIP mang giá trị là 0x00401000h, điều này có nghĩa là địa chỉ 0x00401000h chính là địa chỉ của câu lệnh tiếp theo sẽ được thực hiện. Chúng ta quan sát trên cửa sổ CPU sẽ thấy được câu lệnh tại địa chỉ trên là câu lệnh gì :
  ![](http://i.imgur.com/96QfBKU.png)
  - Tại cửa sổ CPU, chúng ta nhấn F8 để thực hiện câu lệnh đầu tiên tại địa chỉ 0x00401000h và quan sát trên cửa sổ Register xem thanh ghi EIP sẽ thay đổi giá trị như thế nào ? Chúng ta sẽ thấy được như sau :
  ![](http://i.imgur.com/wGMDkXe.png)
 **3 thanh ghi EBP **
 - Đây cũng là một thanh ghi không kém phần quan trọng, thanh ghi EBP (Con trỏ cơ sở - Base Pointer) chủ yếu được sử dụng để truy nhập dữ liệu trong ngăn xếp. Tuy nhiên khác với thanh ghi ESP, thanh ghi EBP còn được sử dụng để truy nhập dữ liệu trong các đoạn khác. Thanh ghi EBP thường được kết hợp với ESP khi chúng ta bắt gặp một lời gọi hàm, thì trước khi hàm này được thực hiện địa chỉ trở về của chương trình (tức là địa chỉ của câu lệnh tiếp theo dưới lời gọi hàm) sẽ được cất vào Stack, và bên trong thân hàm giá trị hiện thời của thanh ghi EBP sẽ được đẩy vào Stack, bởi vì giá trị của thanh ghi EBP phải được thay đổi để có thể tham chiếu tới các giá trị trên Stack. 
 ![](http://i.imgur.com/IJapGky.pngg)
 ![](http://i.imgur.com/IxiEK4R.png)

**4. Các thanh ghi dữ liệu EAX, EBX, ECX, EDX:**
 - Đây là 4 thanh ghi đa năng 32 bit, điều đặc biệt là khi cần chứa dữ liệu 16 bit ta có các thanh ghi sau AX, BX, CX, DX và đặc biệt hơn khi ta cần chữa dữ liệu 8 bit thì các thanh ghi AX, BX, CX, DX này có thể phân tách ra thành 2 thanh ghi 8 bit cao và thấp để làm việc độc lập, đó là các thanh ghi AH và AL, BH và BL, CH và CL, DH và DL. Bốn thanh ghi này ngoài ý nghĩa là những thanh ghi công dụng chung thì nó còn mang những ý nghĩa và chức năng đặc biệt sau :
 <ul>
 <li> Thanh ghi EAX (thanh ghi chứa) : thường được sử dụng trong các lệnh số học, logic và chuyển dữ liệu. Trong các thao tác nhân và chia thường sử dụng đến thanh ghi này.</li>
 <li>Thanh ghi EBX (thanh ghi cơ sở) : thanh ghi này cũng đóng vai trò là thanh ghi địa chỉ.</li> 
 <li> Thanh ghi ECX (thanh ghi đếm) : thanh ghi này thường được sử dụng như một bộ đếm số lần lặp. Ngoài ra nó cũng được sử dụng như là biến đếm trong các lệnh dịch hay quay các bit.</li> 
 <li>Thanh ghi EDX (thanh ghi dữ liệu) : thanh ghi này cùng với thanh ghi EAX tham gia vào thao tác của phép nhân hoặc phép chia. Bên cạnh đó nó cũng thường được sử dụng trong các thao tác vào ra.</li>
 </ul>

**5. Các thanh ghi chỉ số ESI, EDI :**
 -  Hai thanh ghi ESI (chỉ số nguồn) và EDI (chỉ số đích) thường được sử dụng trong các thao tác làm việc với chuỗi hoặc mảng. Trong Ollydbg như các bạn đã làm quen trong các bài trước có một cửa sổ cho chúng ta quan sát trạng thái hiện thời của tất cả các thanh ghi, đó chính là cửa sổ Registers:
 ![](http://i.imgur.com/vywE6yo.png)
 - Những thanh ghi này với chữ cái E ở đầu cho chúng ta biết được chúng là những thanh ghi 32 bits. Trong hình minh họa này các bạn thấy Ollydbg biểu diễn nội dung các thanh ghi ở dạng Hexa. Lấy thanh ghi EAX làm ví dụ, ta thấy giá trị của nó là 0x00000000h đây là giá trị nhỏ nhất của một thanh ghi, giá trị lớn nhất mà thanh ghi này có thể lưu trữ là 0xFFFFFFFFh, nếu như chúng ta chuyển nó sang dạng nhị phân thì chúng ta sẽ có được như sau :
 ![](http://i.imgur.com/aJTEEaL.png)
 ![](http://i.imgur.com/7B5gdQ9.png)
 - Chúng ta thấy rằng khi chuyển sang dạng nhị phân sẽ biểu diễn đúng 32 bits, 32 bits này sẽ có thể mang một trong hai giá trị 0 hoặc 1. Tuy nhiên trong lập trình ASM không phải lúc nào chúng ta cũng sử dụng hết 32 bits, để tránh lãng phí chúng ta có thể thao tác, tính toán chỉ trên một phần của các thanh ghi này, trong trường hợp này của tôi tôi có thể chia nhỏ thanh ghi EAX ra. Ta sẽ làm một ví dụ cụ thể : Giả sử thanh ghi EAX tôi muốn thay đổi giá trị của nó thành là **0x12345678h**. Đầu tiên ta load crackme vào trong Ollydbg, sau khi analyse xong chúng ta sẽ dừng lại tại EP (Entry Point) của chương trình. Bây giờ tôi sẽ thay đổi giá trị của thanh ghi EAX, chuột phải tại thanh ghi này và chọn **Modify** :
 ![](http://i.imgur.com/ROvdLuQ.png
 - Chỉnh sửa lại giá trị của thanh ghi EAX như hình dưới đây :
![](http://i.imgur.com/zJf5qYJ.png)
![](http://i.imgur.com/lAyXzrU.png)
- Kết quả sau khi chỉnh sửa :
 ![](http://i.imgur.com/FXCuAlv.png)
 - Khi bạn thay đổi giá trị của một thanh ghi thì kết quả chỉnh sửa sẽ được đánh dấu màu đỏ. Như tôi đã nói ở trên nhiều khi chúng ta không sử dụng hết 32 bits trong quá trình tính toán mà ta chỉ cần một phần của nó thôi. Thông thường người ta thường sử dụng thanh ghi AX (16 bits) - một phần của thanh ghi EAX. Do AX là thanh ghi 16 bits nên nó chiếm ½ trên tổng số bits, vậy ta có giá trị của thanh ghi AX là 0x5678h. Để biết được kết quả này có chính xác hay không, chúng ta sẽ sử dụng Command Bar :
 ![](http://i.imgur.com/uYvS66w.png)
 - Đúng như những gì chúng ta đã lập luận ở trên, giá trị của AX là 0x5678h. Tuy nhiên bản thân thanh chi AX cũng lại được phân tách thành các thanh ghi AH (8 bits cao) và AL (8 bits thấp). Ta thử quan sát giá trị của thanh ghi AL :
 ![](http://i.imgur.com/KQ1FVAU.png)
 - Tổng kết lại ta có một hình ảnh minh họa trực quan như sau :
 ![](http://i.imgur.com/RnokJb4.png)
 ![](http://i.imgur.com/VpFooSC.png)
 - Tất cả những thanh ghi đa năng khác cũng sẽ được phân tách tương tự như thanh ghi EAX
####3.1.1 Thay đổi giá trị của những thanh ghi :<a name="3.1.1"></a>
 - Ở phần trên, các bạn đã quan sát thấy quá trình thay đổi giá trị của một thanh ghi , ví dụ như thanh ghi EAX diễn ra rất đơn giản, gọn nhẹ. Cách thức thay đổi giá trị bằng cách nhấp chuột phải tại thanh ghi đó và chọn Modify. Chúng ta sẽ làm thêm một ví dụ nữa, như các bạn đã biết thanh ghi EIP được sử dụng để trỏ tới lệnh tiếp theo sắp được thực hiện, giờ đây tôi muốn thay đổi nội dung của thanh ghi này thì phải làm thế nào ? Giả sử khi ta load crackme vào trong Olly thì ta có được như sau :
 ![](http://i.imgur.com/lqlOgGG.png)
 ![](http://i.imgur.com/EDnhViH.png)
 - Trong hình minh họa này thì thanh ghi EIP mang giá trị là 0x00401000h, điều này có nghĩa là câu lệnh tại địa chỉ đó sẽ được thực hiện. Nhưng nếu ta muốn thay đổi giá trị EIP để cho chương trình sẽ thực hiện một câu lệnh ở địa chỉ khác chứ không phải là câu lệnh tại địa chỉ 0x00401000h thì làm thế nào? Rất đơn giản, chúng ta chỉ việc chọn dòng lệnh đó nhấn chuột phải và chọn **New origin here**, ngay lập tức giá trị của EIP sẽ thay đổi theo :
 ![](http://i.imgur.com/DypQk38.png)
 ![](http://i.imgur.com/UeMpwXP.png)

###3.2 các thanh ghi cờ <a name="3.2"></a>

 - Trong phần tiếp theo của bài viết này, chúng ta sẽ làm quen với các cờ. Trong cửa sổ Registers thì các cờ sẽ nằm bên dưới các thanh ghi.
 ![](http://i.imgur.com/NC1epet.png)
 - Trong hình trên bạn quan sát sẽ thấy có các cờ sau : **C, P, A, Z, S, T, D** và **O**.Thêm vào đó các bạn cũng thấy là các cờ này chỉ có 1 bit mà thôi do đó giá trị của nó chỉ có thể là 0 hoặc 1. Các cờ này được đặt trong thanh ghi cờ và chúng được phân chia ra thanh hai loại là cờ trạng thái và cờ điều khiển. Cờ trạng thái phản ánh kết quả của các phép tính. Cờ điều khiển được sử dụng để cho phép hoặc không cho phép một thao tác nào đó của bộ vi xử lý. Nói tóm lại là mỗi một thao tác của CPU đều dựa vào các cờ để ra quyết định. Chúng ta sẽ đi vào xem xét từng cờ một.
 - **a. Cờ trạng thái : **
 - Cờ tràn (Overflow Flag): 

 - Cờ OF = 1 khi xảy ra hiện tượng tràn, thường là khi kết quả là một số bù hai vượt ra ngoài giới hạn biểu diễn dành cho nó. Để minh họa về cờ OF các bạn mở Ollydbg và load crackme vào. Sau đó tiến hành sửa giá trị của thanh ghi EAX = 0x7FFFFFFFh (giá trị của số dương lớn nhất), ta có được như sau :
 ![](http://i.imgur.com/W2HuROZ.png)
 - Bây giờ chúng ta sẽ thực hiện cộng thêm 1 vào thanh ghi EAX, chúng ta sẽ nghĩ ngay đến rằng giá trị của thanh ghi EAX sau khi cộng một sẽ là 0x80000000h (giá trị này tương đương với số âm). Để thực hiện được phép cộng này ta sẽ làm như sau : Chuột phải tại lệnh mà bạn muốn thay đổi và chọn Assemble.
 ![](http://i.imgur.com/UJpuGmd.png)
 - Ta có được như sau :
 ![](http://i.imgur.com/iodAGLV.png)
 - Thay câu lệnh Push 0 thành câu lệnh Add EAX, 1.
 - Sau khi thay đổi xong chúng ta quan sát kết quả thay đổi trên cửa sổ CPU
 ![](http://i.imgur.com/jbj9ps6.png)
 - Trước khi tôi và các bạn thực thi câu lệnh ADD EAX,1 – chúng ta sẽ quan sát giá trị của của cờ OF. Ta có được kết quả như sau :
 ![](http://i.imgur.com/9kU5U5A.png)
 - Okie, lúc này giá trị của cờ tràn O vẫn đang là 0. Chúng ta sẽ thử thực thi câu lệnh Add EAX,1 xem thế nào. Nhấn F8 để thực thi lệnh và quan sát cửa sổ Registers :
 ![](http://i.imgur.com/kPwvB9g.png)
 - **Note:** nhận xét về tràn với số có dấu là nếu cộng 2 số cùng dấu mà kết quả có dấu ngược lại thì có tràn xảy ra. Khi ta cộng hai số khác dấu thì không bao giờ có tràn.

**Cờ chẵn lẻ (Parity Flag): **
 - Cờ này phản ánh tính chẵn lẻ của tổng số bit 1 có trong kết quả. Cờ PF = 1 khi tổng số bit 1 trong kết quả là chẵn – parity chẵn.Nó bằng không trong trường hợp ngược lại.Lấy ví dụ cụ thể như sau. Chúng ta có thanh ghi EAX = 0x0h,cờ PF lúc đầu bằng 1, chúng ta sẽ thực hiện lệnh cộng EAX với 1 và xem kết quả giá trị trên cờ PF.
 ![](http://i.imgur.com/riIyVFW.png)
 - Sau khi thực hiện lệnh ta có được như sau :
 ![](http://i.imgur.com/snl1sf5.png)
 - Ok chúng ta thấy rằng PF đã bằng 0 bởi vì giá trị thanh ghi EAX = 1, mà tổng số bit 1 trong thanh ghi EAX lúc này là 1, vậy theo định nghĩa thì giá trị PF = 0 là hoàn toàn chính xác. Quay trở lại vấn đề, lúc này ta chuột phải tại địa chỉ 0x00401000 và chọn**New Origin**. Nhấn F8 để thực hiện lại câu lệnh. Chúng ta có được kết quả như sau : 
 ![](http://i.imgur.com/Z3EfBXd.png)
 - Ta vẫn thấy cờ PF giữ nguyên kết quả. Đó là bởi vì thanh ghi EAX có giá trị là 0x02 (tổng số bit 1 là lẻ). Lại làm như trên và thực hiện lại câu lệnh add EAX,1. Quan sát hình minh họa dưới đây:
 ![](http://i.imgur.com/jBYezhx.png)

 **Cờ Zero (Parity Flag):** 
 - Cờ này được thiết lập 1 khi kết quả bằng không và ngược lại. Cờ này khá quan trọng đối với việc crack. Để minh họa cho cờ này chúng ta vẫn sẽ sử dụng lệnh Add EAX, 1 nhưng lần này chúng ta sẽ thay lại giá trị của thanh ghi EAX thành 0xFFFFFFFF (-1).
 ![](http://i.imgur.com/Vcprl0V.png)
 - Nhấn F8 để thực hiện câu lệnh add, ta sẽ có được thông tin như sau :
 ![](http://i.imgur.com/aGaDVdm.png)
  - do kết quả của phép cộng thì thanh ghi EAX có giá trị là 0x0h cho nên cờ Z sẽ được bật lên thành 1.

 - **Cờ dấu (Sign Flag):** 
 - Cờ SF được thiết lập là 1 khi bít msb của kết quả bằng 1 có nghĩa là kết quả âm nếu bạn muốn làm việc với số có dấu. Tôi sẽ minh họa về cờ SF như sau, trong Olly chúng ta thay giá trị của thanh ghi EAX thành 0xFFFFFFF8 (=-8). Thực hiện câu lệnh add eax, 1 để tính lại giá trị của thanh ghi EAX.
 ![](http://i.imgur.com/3j2UFzS.png)
 - Thực thi lệnh, kết quả của thanh ghi EAX sẽ là 0xFFFFFFFF9 (=-7) :
 ![](http://i.imgur.com/XfQketG.png)

 - **Cờ nhớ(Carry Flag):**

 -  Cờ CF được thiết lập 1 khi có nhớ hoặc mượn từ bit msb
 **b.Cờ điều khiển:**

 - Có 3 cờ được sử dụng trong việc điều khiển đó là cờ T. I và cờ D. Cờ T(cờ bẫy) bằng 1 thì CPU làm việc ở chế độ chạy từng lệnh –chế độ này thường được dùng khi cần tìm lỗi trong chương trình.

 - Cờ I (cờ ngắt) , cờ này bằng 1 thì CPU cho phép các yêu cầu ngắt được tác động. Cờ D (cờ hướng), cờ này bằng 1 khi CPU làm việc với chuỗi kí tự theo tứ tự từ phải sang trái.

###4. chi tiết về các lệnh trong ASM <a name-"4"></a>

####4.1 lệnh NOP <a name="4.1"></a>

 - Cái tên của nó đã cho bạn thấy được ý nghĩa. Lệnh này không thực hiện một công việc gì cả ngoại trừ việc tăng nội dung của thanh ghi EIP, nó không gây ra bất kì thay đổi nào trong thanh ghi, stack hoặc memory. Chính vì ý nghĩa này của nó mà câu lệnh này thường được dùng vào mục đích hủy bỏ bất kì câu lệnh nào (không cho lệnh đó thực hiện), bằng cách ta thay thế câu lệnh sắp thực hiện bằng lệnh NOP chương trình sẽ vẫn thực thi nhưng thay vì thực thi câu lệnh gốc thì giờ đây do được thay thế bằng NOP nên nó sẽ không làm gì cả. Đó là lý do tại sao các bạn hay thấy người ta sử dụng NOP (ví dụ như : tôi muốn loại bỏ một thông báo nào đó, để làm được điều này tôi thay thế lệnh Call đến thông báo bằng lệnh NOP, vậy là thông báo đó sẽ biến mất . Okie, mở Cruehead crackme trong Olly, ta có như sau :
 ![](http://i.imgur.com/Z3t7UxH.png)
 - Trong hình trên, mới đầu khi load vào Olly ta sẽ có được đoạn code gốc của chương trình đã được Olly phân tích sang ASM. Bây giờ chúng ta sẽ thay thế câu lệnh PUSH 0 như trên hình minh họa bằng lệnh NOP. Các bạn để ý vùng tôi khoanh đỏ, lệnh Push này có 2 bytes mà trong khi lệnh NOP chỉ có 1 byte mà thôi, vậy cho nên khi ta thay thế lệnh PUSH bằng lệnh NOP chúng ta sẽ thấy trên màn hình Olly xuất hiện 2 lệnh NOP. Để có thể thay đổi một câu lệnh trong Olly, chúng ta làm như sau : chuột phải trên lệnh cần thay đổi và chọn **Assemble**(hoặc nhấn Space Bar).
 ![](http://i.imgur.com/G6QDwKy.png)
- Ngay lập tức một cửa sổ bật ra thông báo cho chúng ta biết chúng ta đang chuẩn bị thay đổi lệnh ở địa chỉ nào, và một textbox để cho phép ta nhập lệnh mà chúng ta muốn thay đổi :
![](http://i.imgur.com/zfi90cV.png)
Như các bạn thấy trên hình, bây giờ ta muốn thay lệnh PUSH 0 bằng lệnh NOP. Rất đơn giản ta xóa PUSH 0 đi và gõ vào NOP và nhấn Assemble.
![](http://i.imgur.com/saJhHv8.png)
 - Ở đây chúng ta nhận thấy rằng Olly ngoài việc sẽ thay thế bằng lệnh NOP, chương trình này còn nhận biết được rằng lệnh PUSH 0 trước khi thay thế là một câu lệnh 2 bytes cho nên nó sẽ tự động điền thêm một lệnh NOP ở địa chỉ 0x00401001 cho vừa đủ. So sánh lại với hình trước thì các bạn thấy rằng lệnh PUSH 0 đã hoàn toàn được thay thế bằng 2 lệnh NOP, 2 lệnh này sẽ không thực hiện bất kì công việc gì. Để kiểm chứng bạn nhấn F8 để trace lần lượt qua 2 lệnh NOP này và quan sát các cửa sổ xem có sự thay đổi gì không.Oh, các thanh ghi không thay đổi, stack cũng không biến chuyển, các cờ vẫn bình thường chỉ có mỗi một thay đổi xảy ra đó là với thanh ghi EIP (điều này là hiển nhiên rồi vì thanh ghi EIP sẽ luôn trỏ tới câu lệnh tiếp theo được thực hiện).

 - Tiếp theo chúng ta sẽ quan sát sự thay đổi của 2 bytes này trong cửa sổ Dump, để tìm chúng ta sẽ tìm kiếm theo địa chỉ chứa hai lệnh này. Như các bạn thấy ở trên đó là 0x00401000 và 0x00401001. Chúng ta tới cửa sổ DUMP, chuột phải và chọn như sau :
 ![](http://i.imgur.com/Vvim6KD.png)
 - Một cửa sổ hiện ra, tại đây ta gõ vào : 00401000 và nhấn OK.
 - Kết quả ta có được như sau:
 ![](http://i.imgur.com/YPbIvCE.png)

 - Olly sẽ biểu diễn những gì chúng ta thay đổi bằng màu đỏ, trong hình trên chúng ta thấy có 2 mã lệnh 90 90 tương đương với 2 lệnh NOP NOP. Tiếp theo đó là E8 FF .. tương đương với câu lệnh CALL như các bạn quan sát thấy trong hình bên trên.
 - Có một câu hỏi nhỏ đặt ra : Tôi có thể loại bỏ những gì tôi vừa thay đổi và quay trở lại đoạn code gốc được không ? Hoàn toàn có thể được, Olly hỗ trợ cho chúng ta chức năng Undo.Để làm điều này thì trong cửa sổ Dump hoặc cửa số CPU chúng ta chỉ việc đánh dấu những bytes mà chúng ta đã thay đổi, nhấn chuột phải và chọn như trong hình dưới đây :
 ![](http://i.imgur.com/j87PwYD.png)
 - Sau khi làm đúng như trên, bạn sẽ có được lệnh PUSH 0 ban đầu :

####4.2 lệnh PUSH<a name="4.2"></a>
- Về cơ bản lệnh PUSH được dùng để thêm/cất 1 từ (Word (letters hoặc value)) vào trong ngăn xếp. Như các bạn thấy trong Olly câu lệnh đầu tiên của Cruehead crackme là PUSH, cụ thể trong trường hợp này là PUSH 0.Khi chúng ta thực hiện câu lệnh này thì điều gì sẽ xảy ra, nó sẽ đẩy 0 vào đỉnh của Stack sau đó giảm thanh ghi ESP tuy theo kích thước của toán hạng. Quan sát cửa số Stack trước khi chúng ta thực thi câu lệnh PUSH.Lưu ý giá trị của thanh ghi ESP có thể khác nhau tùy theo từng máy.
![](http://i.imgur.com/mn6xDdH.png)
 - Okie, ban đầu khi chưa thực hiện câu lệnh gì thì cửa sổ Stack của tôi giống như hình minh họa ở trên.Đây là những giá trị khởi tạo ban đầu của Stack do đó trên máy của bạn có thể khác máy của tôi chứ không nhất thiết phải giống nhau hoàn toàn.Bây giờ tôi sẽ thực hiện câu lệnh PUSH bằng cách nhấn F8 để trace qua câu lệnh này, ngay lập tức các bạn sẽ thấy được sự thay đổi :
 ![](http://i.imgur.com/p97jkgK.png)
 - Như các bạn thấy giá trị tại đỉnh của Stack đã thay đổi là 0x0013FFC0 và tại đây lưu trữ giá trị 0x0 do câu lệnh PUSH thực hiện. Bạn hãy tưởng tượng như chúng ta có 1 chồng đĩa có sẵn, giờ ta muốn cất thêm một cái đĩa lên trên chồng đĩa này thì ta phải tịnh tiến chồng đĩa đi để dành ra một khoảng không gian cho ta đặt cái đĩa mới, sau khi có được khoảng không gian vừa đủ ta chỉ việc đặt cái đĩa mới lên trên. Vậy là khi chúng ta muốn lấy đĩa ra thì cái đĩa mới được cất vào sẽ được lấy ra đầu tiên. 
 - Thanh ghi liên quan trực tiếp đến Stack là ESP, thanh ghi này luôn trỏ vào đỉnh của Stack. Quan sát tại cửa sổ Registers chúng ta thấy được như sau :
 ![](http://i.imgur.com/EfCQFzv.png)
 - Có nhiều cách thức để đẩy dữ liệu vào Stack chứ không đơn thuần như chúng ta thấy ở trên. Ví dụ nếu như tôi thực hiện câu lệnh PUSH EAX, thì lệnh này sẽ đẩy giá trị của thanh ghi EAX vào đỉnh Stack. Chúng ta sẽ tìm hiểu thêm sự khác nhau giữa câu lệnh PUSH và PUSH [].
 - Giá sử tôi có câu lệnh PUSH 401008. Khi thực hiện câu lệnh này nó sẽ đẩy 401008 vào đỉnh của Stack. Quan sát hình minh họa dưới đây :
 ![](http://i.imgur.com/xLORQUR.png)
 - Thực hiện lệnh PUSH, quan sát trên stack :
 ![](http://i.imgur.com/JhgmVnR.png)
 - Tuy nhiên nếu như tôi thực hiện câu lệnh PUSH [401008] thì điều gì sẽ xảy ra trên Stack? Lệnh này sẽ đẩy nội dung của bộ nhớ tại 401008 vào đỉnh của Stack, chúng ta sẽ quan sát cửa sổ DUMP xem tại 401008 lưu trữ giá trị gì.
 ![](http://i.imgur.com/C2BHGTV.png)
 ![](http://i.imgur.com/DD5kDlj.png)
 - Chúng ta thấy được giá trị là CA 20 40 00. Khi cất vào stack nó sẽ được đảo ngược lại thành 004020CA, giống như dòng Comment như bạn quan sát thấy ở hình bên trên. Nhấn F8 để thực hiện lệnh PUSH và quan sát kết quả trên Stack.
 ![](http://i.imgur.com/MQk0KTN.png)

####4.3 lệnh POP<a name="4.3"></a>

 - Lệnh POP có ý nghĩa hoàn toàn ngược lại với lệnh PUSH.Nó được sử dụng để lấy ra phần tử (giá trị) từ đỉnh của Stack và đặt vào một nơi mà chúng ta chỉ đỉnh để nhận giá trị được lấy ra.Lấy ví dụ, ta thực hiện câu lệnh POP EAX, điều này có nghĩa là chúng ta sẽ lấy giá trị tại đỉnh của Stack và lưu nó vào thanh ghi EAX, đồng thời giá trị tại đỉnh của Stack sẽ được thay đổi để trỏ tới phần tử tiếp theo.

 - như vậy là các bạn đã có cái nhìn tổng quan về 2 lệnh PUSH và POP.Ta chuyển sang lệnh khác đó là hai lệnh PUSHAD và POPAD, đây là hai lệnh không kém phần quan trọng. Tại sao ta phải lưu ý tới câu lệnh này, lý do đơn giản đó là hai câu lệnh này thường được sử dụng để nhận biết các packers có cơ chế thực hiện gần giống với một Packer nổi tiếng mà có thể các bạn đã biết hoặc đã từng nghe tới đó là : UPX.
####4.4 lệnh PUSHAD <a name="4.4"></a>

 - Lệnh PUSHAD khi thực hiện sẽ lưu tất cả nội dung của các thanh ghi vào trong Stack. Vì vậy có thể nói lệnh PUSHAD sẽ tương đương với một loạt các câu lệnh Push như sau : PUSH EAX, ECX, EDX, EBX, ESP, EBP, ESI, EDI. Chúng ta sẽ quan sát câu lệnh này trong Olly, load crackme vào và thay câu lệnh Push 0 bằng lệnh PUSHAD.
 ![](http://i.imgur.com/YX0JSWZ.png)
 - Quan sát cửa sổ Stack và cửa sổ Registers trước khi thực hiện lệnh :
 ![](http://i.imgur.com/ilBHlyL.png)
 ![](http://i.imgur.com/DzwtagT.png)
 - Nhấn F8 để thực hiện lệnh, đồng thời quan sát cửa sổ Stack các bạn sẽ thấy được như sau :
 ![](http://i.imgur.com/qg0TPvy.png)
 - Theo hình minh họa trên các bạn thấy rằng các thanh ghi lần lượt được đẩy vào Stack theo thự tự từ EAX cho tớ EDI. Vậy giá trị của EDI sẽ nằm ở đình của Stack và lúc này ESP sẽ có giá trị là đỉnh của Stack : 0x0013FFA4.

####4.5 Lệnh POPAD :<a name="4.5"></a>

 - Lệnh POPAD thực hiện nhiệm vụ ngược lại với những gì lệnh PUSHAD đã thực hiện.Nó sẽ lấy giá trị tại Stack và cất vào các thanh ghi theo thứ tự từ EDI về EAX. Chính vì vậy câu lệnh này sẽ tương đương với : POP EDI, ESI, EBP, ESP, EBX, EDX, ECX, EAX. 

 - Qua đây ta có thể rút ra một kết luận nho nhỏ là cặp lệnh PUSHAD và POPAD thường đi cùng với nhau. Nếu như ta thấy ở đâu đó trong mã của chương trình xuất hiện lệnh PUSHAD thì có nghĩa là đâu đó ở bên dưới chắc chắn sẽ có câu lệnh POPAD.

####4.6 Câu lệnh MOV, MOVSX và MOVZX<a name="4.6"></a>
 - Lệnh MOV được sử dụng để chuyển dữ liệu giữa các thanh ghi, giữa một thanh ghi và một ô nhớ hoặc chuyển trực tiếp một số vào một thanh ghi hay ô nhớ.

 - Điều này khiến bạn liên tưởng tới ngôn ngữ lập trình bậc cao khi chúng ta thực hiện một câu lệnh gán A:=B. Ở đây EAX được gán cho giá trị của EBX. Ở ví dụ này các bạn sẽ thấy EAX lưu trữ dữ liệu giống hệt EBX, tuy nhiên trong một vài trường hợp đặc biệt tôi chỉ muốn lấy một phần giá trị thôi thì thế nào? Câu lệnh MOV cũng hỗ trợ các bạn thực hiện việc này.
 - Ngoài việc chuyển dữ liệu qua lại giữa các thanh ghi như những gì các bạn đã nhìn thấy và quan sát ở trên chúng ta còn có lệnh MOV dùng để chuyển nội dung của một ô nhớ vào một thanh ghi. Chẳng hạn chúng ta sẽ thực hiện câu lệnh bên dưới đây :
 ![](http://i.imgur.com/pE3qMaD.png)
 - Trong trường hợ này thì nội dung của 405000 sẽ được chuyển vào thanh ghi EAX.Cụm từ DWORD như các bạn nhìn thấy ở trên có nghĩa là toàn bộ 4 bytes tại ô nhớ trên sẽ được chuyển vào EAX.Bây giờ chúng ta vào cửa sổ DUMP và quan sát xem tại 405000 có giá trị như thế nào :
 ![](http://i.imgur.com/yruA7zW.png)
 ![](http://i.imgur.com/1KB1IrG.png)
 - Như ta thấy, tại 405000 là 00 10 00 00, khi thực hiện câu lệnh MOV thì EAX sẽ giữ giá trị là 00001000.
 - Bây giờ chúng ta thử thực hiện ngược lại và chuyển dữ liệu của một thanh ghi vào ô nhớ xem thế nào.Giả sử tôi muốn thực hiện lệnh : MOV DWORD PTR DS:[40500], ECX
 ![](http://i.imgur.com/pE3qMaD.png)
 ![](http://i.imgur.com/6443D3f.png)
 ![](http://i.imgur.com/xFDRVZz.png)
 - F8 thực hiện lệnh MOV, chương trình bị terminate ngay lập tức. Không có lý do gì mà câu lệnh này không thể thực hiện được? Vậy tại sao lại thế ?
 - Câu trả lời hết sức đơn giản là chúng ta không có quyển ghi tại ô nhớ đó. Để kiểm tra xem có đúng không ta nhấn Alt + M để mở cửa sổ memory và thấy được như sau
 ![](http://i.imgur.com/tqOGr6G.png)
 - Vây là đã rõ tại 405000 chúng ta chỉ có quyển Read mà thôi, chính vì vậy khi chúng ta thực hiện lệnh MOV liền bị terminate chương trình ngay lập tức

 - Nếu như tôi muốn chuyển 2 bytes hay 1 bytes dữ liệu của một ô nhớ vào một thanh ghi thì ta có thể dùng WORD và BYTE trong câu lệnh MOV .Ví dụ tôi có lệnh sau đây : MOV AX, WORD PTR DS:[405008]
 ![](http://i.imgur.com/LwEuHLt.png)
 - Tại sao ở đây chúng ta không sử dụng thanh ghi EAX đó là vì ở đây chúng ta chỉ muốn lấy 2 bytes (16 bit) thôi nên phải sử dụng thanh ghi AX là thanh ghi 16 bits. Điều này cũng đã được qui định bởi lệnh MOV : toán hạng đích và gốc có thể tìm được theo các chế độ địa chỉ khác nhau nhưng phải có cùng độ dài và không được phép đồng thời là 2 ô nhớ hoặc 2 thanh ghi đoạn.
 - Giờ chúng ta muốn lấy 1 bytes thì thay thế thanh ghi AX bằng thanh ghi AL. Do đó câu lệnh MOV sẽ là như sau : MOV AL, BYTE PTR DS:[405008]
 ![](http://i.imgur.com/PWxa1Cl.png)

 **câu lệnh MOVSX**

 - câu lệnh này sẽ thực hiện sao chép nội dung của toán hạng thứ hai, có thể là thanh ghi hoặc ô nhớ (với điều kiện toán hạng thứ hai phải có độ dài nhỏ hơn toán hạng thứ nhất) vào toán hạng thứ nhất đồng thời sẽ điền đầy các bit bên trái của toán hạng thứ nhất bằng bít có trọng số cao nhất của toán hạng thứ hai.Chúng ta sẽ lấy ví dụ cụ thể để minh họa, giả sử trong Olly chúng ta bắt gặp lệnh sau :
 ![](http://i.imgur.com/L6oqtlI.png)
 - OK thanh ghi EAX là 0x0 còn BX là 0xF000.Bây giờ chúng ta nhấn F8 để thực hiện lệnh và quan sát trên cửa sổ Registers :
 ![](http://i.imgur.com/KqbwpHV.png)

 - Như trên ta thấy rằng nội dung của BX đã được sao chép sáng AX và điền phần còn lại với giá trị là FFFF, đó là bởi vì thanh ghi BX nếu xét cụ thể thì nó biểu diễn cho số âm, mà số âm thì bít có trọng số cao nhất sẽ là 1. Do đó số 1 này sẽ được điền hết vào trong thanh ghi EAX như các bạn đã thấy trên hình. Nếu như thanh ghi BX biểu diễn một số dương chẳng hạn 0x1234 thì thanh ghi EAX sẽ có giá trị như sau : 0x00001234. Để chứng mình ta giả sử thanh ghi EBX biểu diễn một số dương là 0x7FFF vậy khi thực hiện lệnh MOV ta sẽ được như sau :
 ![](http://i.imgur.com/oB0mAqU.png)
  
  **lệnh MOVZX **
 - Lệnh này cũng tương tự như lệnh MOVSX, nhưng thay vì phụ thuộc vào bít dấu thì phần bên trái luôn luôn được điền đầy bằng những con số 0. Bạn tự kiểm chứng trong Olly nhé

####4.7 câu lệnh LEA <a name="4.7"></a>

 - Lệnh này cũng tương tự như lệnh MOV nhưng khác ở chỗ toán hạng đầu tiên thường là các thanh ghi công dụng chung còn toán hạng thứ hai là một ô nhớ.Câu lệnh này thực sự rất hữu dụng khi ô nhớ này tương ứng với một phép tính toán trước đó. Lấy một ví dụ trực quan trong Olly :
 ![](http://i.imgur.com/0GrslWo.png)
 - Quan sát cửa sổ Registers để thấy được giá trị hiện thời của các thanh ghi khi chưa thực hiện lệnh :
 ![](http://i.imgur.com/rBjPuvo.png)
 - Trong trường hợp tại máy của tôi thì giá trị thanh ghi ECX là 0x0013FFB0, mà trong lệnh trên tôi cho thanh ghi ECX + 38 vậy tức là tôi sẽ có giá trị = 0x0013FFE8. Nếu hiểu như lệnh MOV bạn sẽ nghĩ là nó sẽ chuyển dữ liệu tại ô nhớ có giá trị ở trên vào thanh ghi EAX nhưng ở đây thì không phải như thế. Nó sẽ nạp giá trị của ECX + 38 vào thanh ghi EAX, kết quả cuối cùng là thanh ghi EAX sẽ có giá trị là 0x0013FFE8. Lập luận là như vậy, bây giờ ta thử nhấn thử F8 để thực hiện lệnh và kiểm tra kết quả xem có chính xác hay không.
 ![](http://i.imgur.com/dIT6uGI.png)
 - Trong quá trình làm việc nhiều với các chương trình các bạn sẽ hiểu rõ hơn về lệnh LEA.

####4.8 lệnh XCHG<a name="4.8"></a>
 
 -Câu lệnh này còn được gọi là lệnh tráo đổi nội dung của hai toán hạng. Lệnh XCHG được dùng để hoán chuyển nội dung của hai thanh ghi, thanh ghi và một ô nhớ.Chúng ta thử kiểm tra bằng ví dụ trong Olly, tôi có một lệnh đơn giản như sau :
 ![](http://i.imgur.com/mXlTFjB.png)
 - Nhìn vào câu lệnh trên các bạn hoàn toàn có thể suy ra ngay được kết quả của câu lệnh này sau khi thực hiện . Thanh ghi EAX sẽ mang giá trị của ECX và ngược lại. 

 - Cũng tương tự trong trường hợp câu lệnh MOV, bạn cũng không thể thực hiện lệnh XCHG giữa thanh ghi và một ô nhớ nếu như ô nhớ đó không có quyền ghi mà chỉ có quyển đọc.

####4.9 lệnh INC và DEC<a name="4.9"></a>

 - INC (Increment) được dùng để cộng thêm 1 vào nội dung của một thanh ghi hay một ô nhớ. DEC (Decrement) thực hiện công việc ngược lại, trừ 1 từ nội dung của một thanh ghi hay ô nhớ.
 - Vậy khi chúng ta thực hiện lệnh `INC EAX` thì tức là tương được với việc ta thực hiện `EAX = EAX + 0x1.`
 - Rất dễ hiểu phải không ? Tiếp tục với lệnh DEC ` DEC  AX ` .Kết quả này tương đương với việc ta thực hiện `EAX = EAX - 0x1`
 - tương tự với các lệnh khác , ta củng có thể thực hiện lệnh INC và DEC với các ô nhớ , tuy nhiên ta chỉ có quyền thao tác với các ô nhớ được phép thay đổi số liệu thôi 

####4.10 lệnh ADD<a name="4.10"></a>

- Lệnh này được sử dụng để cộng nội dung của hai thanh ghi, một thanh ghi và một ô nhớ hoặc cộng một số vào một thanh ghi hay một ô nhớ, kết quả sẽ được lưu vào toán hạng đầu tiên. Lấy một ví dụ như sau :
```
 ADD WORDX, EAX
```

 - Lệnh này sẽ thực hiện “cộng EAX vào WORDX”, tức là cộng nội dung của thanh ghi EAX với nội dung của ô nhớ WORDX và chứa tổng cuối cùng trong WORDX và EAX không bị thay đổi. Vậy nếu ta có câu lệnh ADD EAX, 1 thì sẽ tương đương với INC EAX.
 - tất nhiên việc thao tác cộng giá trị của thanh ghi với một ô nhớ cũng được thục hiên y như các câu lệnh khác .

** lệnh ADC**
- Lệnh này tạm dịch là lệnh cộng có nhớ, tức là Cờ nhớ được cộng vào tổng của toán hạng nguồn và toán hạng đích. Nếu cờ CF=1 thì toán hạng đích = toán hạng nguồn + toán hạng đích + 1. Còn nếu CF=0 thì toán hạng đích = toán hạng nguồn + toán hạng đích. Ví dụ minh họa : 
![](http://i.imgur.com/hFO0M2B.png)
![](http://i.imgur.com/TrsvT8h.png)
 - Ta thấy rằng lúc này cờ C đang là 0, thanh ghi EDX lúc này mang giá trị là 0x21(giá trị của DL là 0x21). Ta sẽ thực hiện lệnh ADC DL, 0xEB tức là DL = DL + 0xEB = 0x21 + 0xEB. Nhấn F7 để thực hiện lệnh, rõ ràng trong quá trình cộng sẽ có nhớ lên bit MSB cho nên cờ C sẽ được set là 1 :
 ![](http://i.imgur.com/6Tpwkbl.png)
 - Ok tiếp tục thực hiện 1 lệnh nữa như sau :
 ![](http://i.imgur.com/npQDcLx.png)
 - Lúc này giá trị của DL đang là 0xC, cờ C đang được thiết lập là 1. Nếu ta thực hiện lệnh ADC DL,1 tức là DL = DL + 1 + 1 = 0xE và cờ C lại được set lại thành 0
 ![](http://i.imgur.com/PRY0RRd.png)

####4.11 lệnh SUB và lệnh SBB<a name="4.11"></a>

 - Lệnh SUB Lệnh này thực hiện công việc ngược lại của lệnh ADD

 - ta thực hiện ví dụ minh họa `SUB 	AX,2 ` với AX =0 . Theo tính toán thông thường của chúng ta thì 0 – 2 sẽ cho kết quả là -2. Giá trị của -2 được biểu diễn ở dạng Hexa là : 0xFFFFFFFE.

 - sau khi thực hiện xong lệnh đó t thấy ở cửa sổ register
 ![](http://i.imgur.com/XmjFJG0.png)
 - Các bạn hoàn toàn có thể thực hiện thêm các ví dụ như trừ 2 thanh ghi : SUB EAX, ECX, trừ thanh ghi và ô nhớ : SUB EAX, DWORD PTR DS:[405000]

 ** lệnh SBB**

  - Là lệnh trừ có nhớ, lệnh này thực hiện trừ toán hạng đích cho toán hạng nguồn và nếu CF=1 thì trừ kết quả nhận được đi 1. Kết quả chứa trong toán hạng đích. Tôi lấy một ví dụ nhỏ để minh họa :
![](http://i.imgur.com/M6IyPS4.png)
![](http://i.imgur.com/mlQnDaa.png)
- Cờ C lúc này đang có giá trị là 0, do đó khi ta thực hiện lệnh SBB thì sẽ tương đương với `EDX = EDX – 3`. Kết quả ta có được :
![](http://i.imgur.com/E5rZXg9.png)
 - Ngược lại nếu ta set cờ C có giá trị 1 thì kết quả sẽ tương đương với `EDX = EDX – 4.`

####4.12 lệnh MUL và lệnh IMUL<a name="4.12"></a>

 - Nhân số không dấu, trong trường hợp này toán hạng gốc là số nhân. Tùy theo độ dài của toán hạng gốc mà ta có ba trường hợp để tổ chức phép nhân : 

 - 1. Nếu gốc là số 8 bit: AL * Gốc 
 Số bị nhân phải là số 8 bit để trong AL.
 Sau khi nhân : AX <- tích

 - 2. Nếu gốc là số 16 bit: AX * Gốc
  Số bị nhân phải là số 16 bit để trong AX 
  Sau khi nhân : DX AX <- tích

 - 3. Nếu gốc là số 32 bit: EAX * Gốc 
 Số bị nhân phải là số 32 bit để trong EAX 
 Sau khi nhân : EDX EAX <- tích

 - Ta lấy một ví dụ minh họa như sau :
 ![](http://i.imgur.com/VYznQS1.png)
 - Câu lệnh MUL ECX sẽ thực hiện nhân ECX với EAX sau đó kết quả thu được sẽ được lưu vào EDX:EAX và không quan tâm tới dấu của các toán hạng. Bây giờ ta giả sử giá trị của các thanh ghi EAX, ECX và EDX như sau :
![](http://i.imgur.com/tupYr88.png)
 - Trước khi xem kết quả thu được trong Olly chúng ta hãy tính toán thử đã. Sử dụng calculator của Windows, ta lấy 0xFFFFFFF7 * 0x9 được kết quả như sau :
 ![](http://i.imgur.com/Ln6azka.png)
 - Như vậy bạn thấy kết quả ở đây vượt quá sự biểu diễn của thanh ghi EAX do đó nó sẽ được lưu một phần vào thanh ghi EXD. Vậy kết quả cuối cùng sẽ là thanh ghi EDX lưu giá trị 0x8 còn EAX sẽ là 0xFFFFFFAF. Trong Olly ta trace qua lệnh để xem kết quả :
 ![](http://i.imgur.com/1mQyeal.png)

 **Câu lệnh IMUL**

 - Nhân số có dấu, cách thức thì cũng tương tự như lệnh MUL. Ta lấy 2 ví dụ như sau :
  a. `IMUL EBX `: Câu lệnh này sẽ tương đương với việc lấy `EAX * EBX `và kết quả sẽ được lưu vào `EDX:EAX `tương tự như lệnh MUL nhưng trong đó dấu của toán hạng được quan tâm đến.
  b.` 696E74020080FF imul ebp, dword ptr [esi+74], FF800002 : [ESI+74] x FF800002 - > EBP` Trong ví dụ thứ hai này chúng ta sẽ thấy có 3 toán hạng và nó thực hiện như sau, lấy nội dung của [ESI+74] nhân với FF800002 kết quả được bao nhiêu lưu vào EBP.Ta sẽ thử thực hiện câu lệnh này trong Olly :
  ![](http://i.imgur.com/PInfo3N.png)
  ![](http://i.imgur.com/W2xdquB.png)
  - Ta sửa lại giá trị của thanh ghi ESI thành 0x401000 để chúng ta chắc chắn rằng giá trị của [ESI+74] là có và thể đọc được nội dung của nó.
  ![](http://i.imgur.com/Jgj3yFj.png)
  - Ok sau khi đổi chúng ta xem nội dung của nó là gì :
  ![](http://i.imgur.com/tdnE7kG.png)
  - Ta nhấn thử F7 để thực hiện câu lệnh IMUL :
  ![](http://i.imgur.com/V1PDZ24.png)
  - Nếu ta tính toán trong calculator của windows ta sẽ được như sau :
  ![](http://i.imgur.com/y5qBzEo.png)

####4.13 lệnh XADD<a name="4.13"></a>

 - Câu lệnh này đồng thời thực hiện lần lượt hai lệnh XCHG và ADD. Ví dụ : XADD EAX, ECX thì lúc này giá trị của ECX được thay thế bằng giá trị của EAX còn giá trị của EAX = EAX + ECX.

####4.14 lệnh NEG<a name="4.14"></a>

- Lệnh này thực hiện việc lấy bù hai của một toán hạng hay còn gọi là đảo dấu của một toán hạng. Điều này sẽ tương đương với công việc đảo bít của toán hạng và cộng với 1

####4.15 các lệnh logic<a name="4.15"></a>
#####4.15.1 lệnh AND<a name="4.15.1"></a>

 - Kết quả của lệnh AND là 1 nếu như hai bit là 1, ngược lại là 0 : 
 
|AND|1|0|
|---|-|-|
|1|1|0|
|0|0|0|

 1 and 1 = 1 
 1 and 0 = 0 
 0 and 1 = 0 
 0 and 0 = 0

 - câu lệnh AND chỉ có giá trị 1 (true) khi cả 2 bit toán hạng đều là 1 còn lại sẽ mang giá trị 0( fall)
 
 - Lệnh AND có thể được dùng để xóa các bit nhất định của toán hạng đích trong khi giữ nguyên những bit còn lại. Bit 0 của mặt nạ xóa bit tương ứng, còn bit 1 của mặt nạ giữ nguyên bit tương ứng của toán hạng đích
#####4.15.2 lệnh OR<a name="4.15.2"></a>

|OR|1|0|
|--|-|-|
|1|1|1|
|0|1|0|

 1 xor 1 = 0
 1 xor 0 = 1
 0 xor 1 = 1 
 0 xor 0 = 0 

 -Lệnh OR có thể được dùng để thiết lập các bit xác định của toán hạng đích trong khi vẫn giữ nguyên những bit còn lại. Bit 1 của mặt nạ sẽ thiết lập bit tương ứng trong khi bit 0 của nó sẽ giữ nguyên bit tương ứng trong toán hạng đích.


#####4.15.3 lệnh XOR <a name="4.15.3"></a>

  1 xor 1 = 0 
  1 xor 0 = 1 
  0 xor 1 = 1
   0 xor 0 = 0 

 - Lệnh XOR có thể được dùng để đảo các bit xác định của toán hạng đích trong khi vẫn giữ nguyên các bit còn lại. bit 1 của mặt nạ làm đảo bit tương ứng còn bit 0 của mặt nạ giữ nguyên bit tương ứng của toán hạng đích.

#####4.15.4 lệnh NOT<a name="4.15.4"></a>

 - Thực hiện việc đảo bít :
  not 1 = 0
  not 0 = 1 

 - Ví dụ : not 0110 = 1001. Giả sự ta có thanh ghi EAX mang giá trị là 0x1200:

|Biểu diễn ở dạng Bin|     00000000000000000001001000000000|
|--------------------|-------------------------------------|
|Thực hiện đảo bit| 11111111111111111110110111111111|
|Đưa về dạng Hexa| FFFFEDFF|

![](http://i.imgur.com/sxljGdL.png)
![](http://i.imgur.com/g4oxBdw.png)

 - Kết quả sau khi thực hiện lệnh :
 ![](http://i.imgur.com/yRbCuNp.png) 

###5. các lệnh so sánh và lệnh nhảy <a name="5"></a>

 - Thông thường, khi chúng ta thực hiện việc so sánh là chúng ta so sánh giữa hai đối tượng trở lên và rồi đi đến một quyết định vào đó. Lấy một ví dụ vui : Người A béo hơn người B do đó suy ra người A ăn nhiều hơn người B

 - Trong Cracking, khi chúng ta thực hiện so sánh giữa hai toán hạng thì kết quả của việc so sánh này sẽ quyết định rằng chương trình có thực hiện câu lệnh nhảy bên dưới hay là không. Và đây cũng là những kiến thức cơ bản luôn luôn được đề cập đến trong các bài viết hướng dẫn dành cho Newbie (những người mới bắt đầu làm quen với Crack).

 - Chúng ta biết rằng khi một chương trình yêu cầu ta phải nhập Serial để đăng kí, thì bản thân chương trình đó sẽ quyết định xem liệu rằng cái dãy serial mà chúng ta nhập vào kia có thỏa mãn (đúng) hay không thỏa mãn thông qua việc tính toán và tiến hành các câu lệnh so sánh. Dựa trên kết quả của việc so sánh này chương trình sẽ đưa ra quyết định có thực hiện lệnh nhảy hay là không, điều này tùy thuộc vào chúng ta có nhập serial không, serial của chúng ta nhập vào đúng hay sai ?

####5. lệnh nhảy có điều kiện CMP <a name="5"></a>

 - Đây là câu lệnh so sánh rất thường gặp trong quá trình chúng ta phân tích chương trình, các điều kiện nhảy thường được cung cấp bởi lệnh CMP này. Tổng quát câu lệnh CMP có dạng như sau:
```
  CMP Đích, Nguồn
```

 - Lệnh này thực hiện việc so sánh toán hạng Đích với toán hạng Nguồn bằng cách lấy toán hạng Đích trừ đi toán hạng Nguồn. Có thể nói lệnh này tương đương với lệnh SUB nhưng nó khác với lệnh SUB ở chỗ kết quả không được lưu lại (tức là toán hạng đích không bị thay đổi). Tác dụng chủ yếu của lệnh CMP là tác động lên các cờ. Một điểm chú ý khác nữa là các toán hạng của lệnh CMP không thể cùng là các ô nhớ. Vậy tổng kết lại lệnh này thường được dùng để tạo cờ cho các lệnh nhảy có điều kiện.
 - Dưới đây là bảng trích dẫn các cờ chính theo quan hệ Đích và Nguồn khi so sánh 2 số không dấu :

|   | CF |ZF | SF|
|---|----|---|---|
| Đích = Nguồn| 0| 1| 0|
| Đích > Nguồn| 0 |0 |0|
|Đích < Nguồn |1|0|1|

 - Ta lấy ví dụ trực quan trong Olly :
 - a) CMP EAX, ECX

 - Ở đây tôi giả sử giá trị của 2 thanh ghi EAX và ECX là bằng nhau, kết quả so sánh (kết quả của phép trừ) sẽ không làm thay đổi giá trị của EAX mà nó sẽ tác động lên các cờ, trong đó cờ được thiết lập sẽ là cờ Z. Ok , trong Olly chúng ta làm như sau :

 ![](http://i.imgur.com/0DnpCo7.png)
 ![](http://i.imgur.com/Ipk1AJA.png)
 - Bây giờ tôi nhấn F8 để trace qua lệnh CMP, quan sát tại cửa sổ Register thì ta thấy rằng giá trị của hai thanh ghi EAX và ECX không hề thay đổi. Tuy nhiên cờ Z được thiết lập.
 ![](http://i.imgur.com/Ipk1AJA.png)

 - Mở rộng vấn đề ra một chút, nếu bên dưới của câu lệnh so sánh trên tôi có một lệnh nhảy vậy thì sẽ có hai khả năng xảy ra. Câu lệnh nhảy này sẽ được thực hiện hoặc không thực hiện phụ thuộc vào trạng thái của cờ (mà cụ thể ớ đây là cờ Z). Trong trường hợp ví dụ trên tôi giả sử bên dưới là lệnh JZ, lệnh nãy sẽ thực hiện nhảy nếu như cờ Z được thiết lập là 1 và không nhảy nếu như cờ Z được thiết lập là 0. Cũng với ví dụ trên tôi giả sử serial mà tôi nhập vào được lưu vào thanh ghi EAX, còn Serial do chương trình tính toán ra được đặt trong thanh ECX. Nếu EAX = ECX, vậy tức là câu lệnh CMP sẽ làm cho cờ Z được bật lên thành 1 và câu lệnh JZ sẽ nhảy tới vùng code dành cho người dùng đã đăng kí hợp pháp, còn ngược lại nếu EAX không bằng ECX thì lúc này lệnh CMP sẽ thiết lập cờ Z là 0 và như vậy chúng ta sẽ bị đưa đến vùng code mà sẽ bắn ra Nag thông báo!!

 - Ngoài việc có tác động lên cờ Z thì lệnh CMP còn tác động lên cả cờ S. Tôi lấy một ví dụ cụ thể như sau :

 - b) CMP EAX, ECX trong đó EAX lớn hơn ECX
 ![](http://i.imgur.com/UDYrk8S.png)
  - Bây giờ chúng ta nhấn F8 để thực hiện lệnh CMP và quan sát xem giá trị của các cờ thay đổi ra sao.

  ![](http://i.imgur.com/V9Yf88v.png)

  - Ta thấy cờ Z đã được thiết lập là 0 lý do là vì EAX không băng ECX, đồng thời cờ S cũng được thiết lập là 0 đó là vì EAX lớn hơn ECX (EAX-ECX cho chúng ta kết quả là một số dương). Vậy trong trường hợp EAX nhỏ hơn ECX thì sao, tôi lấy thêm một ví dụ minh họa nữa :

 - c) CMP EAX, ECX trong đó EAX nhỏ hơn ECX
![](http://i.imgur.com/la4eqrr.png)

- Giờ chúng ta thực hiện lệnh và quan sát sự thay đổi :
![](http://i.imgur.com/Iln5OVM.png)

 - Cờ S lúc này được bật lên thành 1 đó là vì kết quả của EAX – ECX sẽ cho chúng ta một số âm. Vậy chúng ta thấy rằng việc quyết định có thực hiện lệnh nhảy hay không sẽ phụ thuộc vào trạng thái của các cờ.Bên cạnh lệnh so sánh giữa hai thanh ghi, thì CMP còn cho phép chúng ta thực hiện so sánh giữa thanh ghi và một ô nhớ (DWORD, WORD hoặc BYTE). Lấy ví dụ minh họa như sau:
 ![](http://i.imgur.com/eLw30XY.png)
 ![](http://i.imgur.com/Lwbzxgn.png)

 - Thực hiện lệnh so sánh và quan sát kết quả, ta sẽ thấy rằng lệnh so sánh này sẽ lấy giá trị của EAX trừ đi giá trị tại ô nhớ [405000] là 1000. Kết quả của phép trừ này sẽ là âm và do đó cờ S sẽ được thiết lập là 1.
 ![](http://i.imgur.com/CxQPW0Y.png)

####5.2 lệnh TEST<a name="5.2"></a>

 - Cũng giống như lệnh CMP, lệnh TEST thực hiện thao tác giữa toán hạng Đích và Nguồn mà không làm thay đổi các toán hạng, kết quả cũng không được lưu giữ. Nó khác với lệnh CMP ở chỗ lệnh này sử dụng phép AND, lệnh TEST sẽ tác động tới các cờ** SF, ZF** và **PF**. Các cờ được tạo ra sẽ được dùng làm điều kiện cho các lệnh nhảy có điều kiện. Tổng quát về lệnh TEST như sau :
```
TEST Đích, Nguồn
```

 - Trong đó toán hạng Đích và Nguồn phải chữa dữ liệu cùng độ dài và không được phép đồng thời là hai ô nhớ.

 - Lấy ví dụ để minh họa: tôi có lệnh TEST EAX, EAX. Câu lệnh TEST này sẽ kiểm tra xem EAX có bằng không hay không ? Trong Olly chúng ta sửa lại như sau :

 ![](http://i.imgur.com/1PJwEzf.png)
 - Giả sử rằng giá trị của thanh ghi EAX lúc này đang là 0x0 và cờ Z cũng đang có giá trị 0
 ![](http://i.imgur.com/AXKs2c9.png)

  - Trong phần trước tôi đã cung cấp cho các bạn bảng tính toán giữa các bit của phép AND rồi, do đó phần này không nhắc lại nữa. Nhấn F8 để trace qua lệnh TEST và quan sát kết quả : 
  ![](http://i.imgur.com/Ma4Btwa.png)

 - Ồ cờ Z đã được bật lên, đó là vì kết quả của phép AND giữa hai toán hạng (ở đây là EAX) có giá trị 0x0 sẽ cho ta là 0x0. Khi kết quả bằng 0x0 thì cờ Z của chúng ta sẽ được bật lên. Tôi lặp lại ví dụ này, nhưng thay vào đó EAX sẽ mang một giá trị khác 0x0. Ví dụ :

 ![](http://i.imgur.com/AUfm5Pm.png)
 - Ok, nhấn F8 và quan sát cờ Z sẽ thay đổi thế nào :
 ![](http://i.imgur.com/B3LsfwL.png)

 - Cờ Z không được bật lên, điều này là hiển nhiên bởi vì kết quả của phép AND cho chúng ta một giá trị khác 0x0. Chúng ta thử tính toán xem thế nào, ta đổi giá trị 0x390 sang dạng Binary (1110010000)và thực hiện phép AND.
 ![](http://i.imgur.com/M2P6Kgj.png)

 - Vậy như các bạn thấy ở trên kết quả sau khi tính toán sẽ là khác 0x0 cho nên cờ Z không được bật lên, và một điều đặc biệt ở trên là kết quả sau khi tính toán giống hệt với hai toán hạng của chúng ta.

 **Các lệnh nhảy** : 
 Bảng tổng hợp các lệnh nhảy (chi tiết về các lệnh nhảy sẽ được đề cập ờ bên dưới) :
 ![](http://i.imgur.com/NzcoKW2.png)

####5.3 lệnh JMP <a name="5.3"></a>

 - Lệnh JMP Đây là lệnh nhảy trực tiếp hay nếu dịch sát nghĩa ra thì nó là lệnh nhảy không điều kiện (tức là lệnh nhảy này luôn luôn thực hiện mà không cần bất kì điều kiện nào). Lấy ví dụ trong Olly như sau :
 ![](http://i.imgur.com/1K8CI34.png)

 - Như quan sát trên hình, khi lệnh JMP thực hiện thì chương trình sẽ chuyển hướng thực thi tới địa chỉ 0x00401031 và thực hiện câu lệnh tiếp theo bắt đầu tại địa chỉ này. Cũng trong hình minh họa trên, các bạn thấy xuất hiện một mũi tên màu đỏ, mũi tên này chỉ cho ta nơi mà lệnh nhảy sẽ nhảy tới. Để có thể thấy được mũi tên này thì các bạn cấu hình trong Olly như sau :
 ![](http://i.imgur.com/wCsyeBb.png)

 - Bây giờ tôi nhấn F8 để thực thi câu lệnh JMP, lúc này giá trị thanh ghi EIP của tôi sẽ thay đồi thành 0x00401031. Đơn giản là bởi vì thanh ghi EIP luôn luôn trỏ tới câu lệnh được thực hiện tiếp theo :
 ![](http://i.imgur.com/k4bSAuJ.png)

####5.4 Lệnh JE / JZ <a name="5.4"></a>

 - Về ý nghĩa thì JE : Nhảy nếu bằng nhau / JZ : Nhảy nếu kết quả bằng không – là hai lệnh nhảy biểu diễn cùng một thao tác nhảy có điều kiện tới một vị trí định trước nếu như cờ ZF = 1. Tôi xét một ví dụ như sau :
 ![](http://i.imgur.com/7hRIrFJ.png)
  - Tôi cho giá trị của hai thanh ghi EAX và ECX bằng nhau, và ZF lúc này = 0 :
  ![](http://i.imgur.com/fyjTTcG.png)
  - Do 2 thanh ghi có giá trị bằng nhau nên khi thực hiện lệnh CMP cho ra kết quả bằng 0 nên lúc này cờ ZF sẽ được bật thành 1. Lệnh JE lúc này sẽ dựa vào trạng thái của cờ ZF để quyết định có thực hiện hay không, do lúc này cờ ZF = 1 nên lệnh nhảy này sẽ được thực hiện :

 ![](http://i.imgur.com/iDgPK8j.png)
 ![](http://i.imgur.com/uJozz9l.png)

 - Lặp lại ví dụ ở trên nhưng lần này giá trị của EAX và ECX sẽ khác nhau, cờ ZF lúc này là 1 : Nhấn F8
 ![](http://i.imgur.com/Uxeab5j.png)

 - Nhấn F8 để trace qua lệnh CMP, do thanh ghi EAX có giá trị lớn hơn ECX nên kết quả của việc thực hiện lệnh CMP sẽ cho ta một giá trị khác 0 dẫn đến cờ ZF sẽ được thiết lập lại là 0. Lúc này lệnh JE thấy cờ ZF bị “tắt” cho nên nó không thực hiện lệnh nhảy nữa :

  - Khi mà lệnh nhảy JE tại địa chỉ 0x00401002 không được thực hiện thì lệnh tiếp theo sau nó, tức là lệnh NOP tại địa chỉ 0x00401004 sẽ là lệnh tiếp theo được thực thi. Nhiều lúc trong quá trình làm việc với Olly bạn không muốn can thiệp vào giá trị các thanh ghi để từ đó thay đổi giá trị các cờ. Vậy có cách nào để chúng ta ép chương trình thực hiện lệnh nhảy mà không cần can thiệp vào giá trị thanh ghi để tạo cờ hay không ? Xin thưa, vô cùng đơn giản . 

  - Bạn lặp lại ví dụ trên và trace qua lệnh CMP. Ở đây bạn muốn làm sao cho lệnh nhảy dưới lệnh CMP sẽ thực hiện, lẽ ra bạn sẽ phải tính toán và chỉnh lại giá trị các thanh ghi để sao cho khi thực hiện lệnh CMP thì cờ ZF được bật lên thành 1 và lệnh nhảy sẽ được thực hiện. Tuy nhiên ở đây tôi không cần quan tâm các thanh ghi chứa giá trị thế nào, việc đơn giản nhất mà tôi làm là nhấn đúp chuột vào cờ mà tôi muốn **thiết lập / hủy thiết lập**. Cụ thể hơn, các bạn nhìn vào hình trên thì thấy lệnh nhảy sẽ không được thực hiện và cờ Z lúc này đang là 0. Để cho lệnh nhảy sẽ được thực thi tôi nhấn đúp chuột lên cờ ZF, lúc này cờ ZF sẽ được bật lên thành 1 và quan sát cửa sổ CPU các bạn sẽ thấy :
  ![](http://i.imgur.com/dGIz7Q2.png)
![](http://i.imgur.com/1Zezs8c.png)

####5.5 Lệnh JNE / JNZ <a name="5.5"></a>

 - Hai lệnh nhảy này là ngược lại của hai lệnh nhảy JE và JZ, ý nghĩa cụ thể của nó là Nhảy nếu không bằng nhau / Nhảy nếu kết quả không bằng không. Chúng biểu diễn cùng một thao tác nhảy có điều kiện tới một vị trí nếu như cờ ZF = 0. Một ví dụ nhỏ để minh họa :

 ![](http://i.imgur.com/tZakum7.png)
 ![](http://i.imgur.com/En17ZZ7.png)
 - Trace qua lệnh CMP, do EAX khác ECX nên cờ ZF được thiết lập lại thành 0. Lệnh JNZ nhận thấy cờ ZF là 0 nên sẽ quyết định thực thi lệnh nhảy.
 ![](http://i.imgur.com/vyVP7QS.png)

####5.6 Lệnh JS <a name="5.6"></a>

 - Đây là lệnh nhảy được thực hiện nếu kết quả âm, tức là lệnh nhảy có điều kiện tới một vị trí định trước nếu SF = 1. Điều này có nghĩa là nếu kết quả âm sau khi thực hiện tính toán các phép toán với các số có dấu thì cờ SF sẽ được thiết lập là 1 và lệnh JS sẽ được thực thi. Ví dụ minh họa :
![](http://i.imgur.com/9ggFKYU.png)

![](http://i.imgur.com/73UNZCM.png)

 - Trace qua lệnh CMP, do EAX < ECX do đó cờ SF sẽ được thiết lập là 1 và lệnh nhảy sẽ được thực thi :

![](http://i.imgur.com/65jvoIm.png)

####5.7 lệnh JP/JPE, và lệnh JNP/JNPE <a name="5.7"></a>

- Đây là hai lệnh nhảy biểu diễn cùng một thao tác nhảy có điều kiện tới một vị trí định trước nếu PF = 1 (PF là cờ Parity).

** lệnh JNP / JNPE**

 - Hai lệnh này có điều kiện nhảy ngược với hai lệnh nhảy ở trên, tức là nó chỉ thực hiện khi mà cờ PF mang giá trị 0.

####5.8 lệnh JO và JNO<a name="5.8></a>"

 -  lệnh JO là lệnh nhảy có điều kiện tới vị trí định trước nếu OF = 1, tức là xảy ra tràn sau khi thực hiện các phép toán với các số có dấu

 - ngược lại với lệnh JO , lệnh JNO sẽ nhảy khi không có hiện tượng tràn xảy ra với các số có dấu.

####5.9 lệnh JB<a name="5.9"></a>

- Lệnh này sẽ thực hiện nếu cờ CF = 1

- ngược với lệnh JB là lệnh JNB

####5.10 lệnh JBE<a name="5.10"></a>

 - Nhảy nếu thấp hơn hoặc bằng, lệnh này sẽ được thực hiện nếu cờ CF = 1 hoặc cờ ZF = 1. Tức là nếu ta có EAX = ECX thì lệnh nhảy sẽ được thực hiện hoặc nếu EAX < ECX thì lệnh nhảy cũng sẽ được thực hiện.

###5.11 lệnh JL <a name="5.11"></a>

- Lệnh này thực hiện khi mà cờ SF và cờ OF khác nhau. Một ví dụ minh họa với trường hợp EAX và ECX là hai số dương và EAX lớn hơn ECX.
![](http://i.imgur.com/IhOFmKA.png)
![](http://i.imgur.com/7bfBSDA.png)

 - Trace qua lệnh CMP, quan sát sẽ thấy lệnh Jmp không được thực hiện bởi vì EAX lớn hơn ECX, kết quả của CMP là một số dương do đó các cờ S và O không bị tác động
 ![](http://i.imgur.com/50TEvRd.png)

 - Ngược lại nếu tôi lấy giá trị của EAX < ECX và lặp lại ví dụ trên :
 ![](http://i.imgur.com/Rni3A6V.png)
 -Thực hiện CMP , quan sát kết quả ta thấy như sau :
 ![](http://i.imgur.com/1lSBIdN.png)

 - SF != OF, cho nên lệnh nhảy JL sẽ được thực hiện

###6. các lệnh với hàm và cấu trúc <a name="6"></a>

 **lệnh CALL và lệnh RET/RETN**

 - Trước tiên chúng ta mở Olly lên và load crackme vào. Tại cửa sổ CPU, bạn nhấn chuột phải và chọn như sau (hoặc nhấn Ctrl + G) :
 ![](http://i.imgur.com/TWrY7LC.png)

 - Tại cửa sổ vừa mới xuất hiện bạn gõ vào địa chỉ như sau : 0x401245 và nhấn OK

 ![](http://i.imgur.com/YHwqJds.png)

 - Olly sẽ đưa chúng ta đến địa chỉ mà chúng ta vừa gõ. Tại đây chúng ta sẽ thấy một lệnh CALL :
 ![](http://i.imgur.com/6OCcC9V.png)

 - Đây là lệnh CALL mà tôi dùng để thực hành trong bài viết này. Để có thể thực thi được lệnh CALL này thì chúng ta phải thiết lập lại thanh ghi EIP trỏ đúng tới địa chỉ 0x401245. Điều này được thực hiện rất đơn giản bằng cách nhấn chuột phải lên lệnh CALL và chọn New origin here như sau :

 ![](http://i.imgur.com/z343HDe.png)
 - Quan sát thanh ghi EIP các bạn sẽ thấy nó thay đổi (xem lại về thanh ghi EIP ở phần trước) :

 ![](http://i.imgur.com/2TmKN4N.png)

 - Ok, giờ quay trở lại lệnh CALL của chúng ta :

  ![](http://i.imgur.com/uuQRefM.png)

 - Về bản chất thì lệnh CALL được dùng để gọi một thủ tục (chương trình con). Có hai kiểu gọi thủ tục đó là gọi trực tiếp và gọi gián tiếp. Cú pháp của lệnh gọi thủ tục trực tiếp :
 
```
CALL Name (trong đó Name là tên của thủ tục)
```

 - Cú pháp của lệnh gọi thủ tục gián tiếp :
```
  CALL addr_expresstion 
  (trong đó addr_expression là một thanh ghi hay ô nhớ chứa địa chỉ của thủ tục)
```
 - *Note : Xem thêm hình minh họa về lệnh CALL trong file CaLL_stack.gif.*

 - Lệnh CALL dùng để chuyển hoạt động của bộ vi xử lý từ chương trình chính sang chương trình con. Có nghĩa là khi chúng ta trace qua một lệnh CALL trong Olly thì các câu lệnh bên trong của câu lệnh CALL này sẽ được thực hiện, điều này giống như việc các bạn lập trình trên các ngôn ngữ bậc cao bằng cách tạo ra các thủ tục hay các hàm. Khi thực hiện chương trình thì ở đâu có lời gọi hàm hay thủ tục thì sẽ thực hiện các lệnh bên trong hàm/thủ tục đó đã rồi mới trả lại quyền điều khiển cho chương trình chính. Một ví dụ minh họa để hiểu rõ vấn đề :

 - Câu lệnh `CALL CRACKME.00401362` có nghĩa là câu lệnh tiếp theo sẽ được thực thi bắt đầu tại địa chỉ `0x00401362` và sau khi thực hiện xong chương trình con (thủ tục/hàm) bắt đầu từ địa chỉ đó, nó sẽ trở về (trả lại quyền điều khiển cho chương trình chính) thực thi câu lệnh tiếp theo bên dưới lệnh CALL. Trong ví dụ của chúng ta thì sau khi thực hiện các lệnh bên trong lệnh `CALL CRACKME.00401362` xong thì nó sẽ trở về lệnh `JMP SHORT CRACKME.004011E6` tại địa chỉ `0x0040124A `và thực thi câu lệnh này theo đúng trình tự thực hiện của chương trình chính. Vậy trong Ollydbg hỗ trợ cho tôi chức năng gì để tôi có thể làm việc với lệnh CALL?

 - Nếu như tôi muốn xem nội dung của lệnh CALL và muốn trace để debug các lệnh bên trong lệnh CALL thì Ollydbg hỗ trợ cho tôi tính năng **Step Into (F7)**. Tức là tại bất kì câu lệnh CALL nào bạn chỉ việc nhấn F7 thì bạn sẽ chui vào trong lòng lệnh CALL đó để xem các lệnh. Tuy nhiên, nếu lệnh CALL nào cũng nhấn F7 thì e rằng nhiều lúc thực sự không cần thiết vì tôi chỉ muốn có cái nhìn tổng quan về lệnh CALL đó mà chưa cần quan tâm đến nội dung bên trong lệnh CALL có những gì, về mặt này Olly hỗ trợ cho chúng ta tính năng **Step Over (F8)**. Tính năng này giúp ta thực thi luôn lệnh CALL mà không cần phải chui vào thực hiện từng lệnh bên trong lệnh CALL như chúng ta nhấn F7, sau khi thực hiện xong lệnh CALL nó sẽ đưa chúng ta thẳng tới lệnh tiếp theo bên dưới lệnh CALL.

 - Vậy từ đây ta đi tới kết luận, khi ta đứng tại một lệnh CALL bất kỳ ta sẽ có hai lựa chọn :
<ul>
<li>1. Nếu như lệnh CALL đó là quan trọng và chúng ta muốn quan sát cách hành xử bên trong lệnh CALL đó thì chúng ta sử dụng chức năng Step Into.</li> 
<li>2. Nếu như lệnh CALL đó không có gì để ta phải quan tâm và ta chỉ cần kết quả trả về của nó mà không cần biết bên trong nó làm những gì thì ta sử dụng chức năng Step Over.</li>
</ul>
 
 - Tuy nhiên hai chức năng này vẫn phải thực thi các lệnh, nhưng ở đây tôi chỉ muốn quan sát bên trong lệnh CALL có những lệnh gì mà không muốn phải thực thi bất kì lệnh nào thì Olly có tính năng hỗ trợ không?? Với một lệnh CALL mà ta quan tâm, Olly hỗ trợ cho chúng ta chức năng **Follow**. Nhấn chuột phải lên lệnh CALL và chọn Follw, xem hình minh họa dưới đây :
 ![](http://i.imgur.com/rP5UXkZ.png)

 - Tính năng này chỉ đơn giản là cho phép ta xem câu lệnh tiếp theo sẽ được thực hiện mà bản thân nó không hề thực thi bất kì câu lênh nào của chương trình. Nó cũng không làm thay đổi thanh ghi EIP, quan sát hình minh họa dưới đây :

 ![](http://i.imgur.com/Kof6HUP.png)

 - Khi tôi Follow theo lệnh CALL tại địa chỉ `0x00401362` tôi sẽ quan sát được các lệnh bên trong lệnh CALL này và rõ ràng rằng địa chỉ của lệnh tiếp theo sẽ được thực thi sẽ bắt đầu tại ` 0x00401362`. Kết thúc lệnh CALL này để trở vể chương trình chính, các bạn quan sát sẽ thấy có một lệnh đặc biệt đó chính là RET/ RETN. Lệnh RET thường được đặt ở cuối chương trình con để bộ vi xử lý lấy lại địa chỉ trở về (địa chỉ của lệnh tiếp theo bên dưới lệnh CALL), địa chỉ này được tự động cất vào stack mỗi khi có lời gọi đến một chương trình con. Theo như ví dụ của chúng ta thì sau khi thực hiện lệnh RET tại `0040137D \. C3 RETN `ta sẽ trở về địa chỉ `0x0040124A`.

 - Đây là những kiến thức rất quan trọng các bạn cần phải nắm được về lệnh CALL. Ở trên các bạn mới chỉ làm những công việc là nhìn và quan sát những gì bên trong lệnh CALL mà chưa thực thi bất kì một câu lệnh nào. Phần tiếp theo dưới đây chúng ta sẽ áp dụng các chức năng Step Into và Step Over. Ok, quay trở lại ví dụ chúng ta đang dừng lại tại lệnh CALL :
 ![](http://i.imgur.com/YeSOS3i.png)

 -Bây giờ chúng ta nhấn F7 để đi vào trong lệnh CALL, khà khà nhưng trước tiên chúng ta cần quan sát cửa sổ Stack trước khi thực hiện nhấn F7. Cửa sổ Stack sẽ cung cấp cho chúng ta một thông tin rất quan trọng đó là địa chỉ trở về (địa chỉ của lệnh bên dưới lệnh CALL), địa chỉ này sẽ được cất vào Stack để từ đó chương trình sẽ biết được nơi mà nó cần trở về khi thực hiện câu lệnh RET.

 ![](http://i.imgur.com/MIlfIpf.png)

 - Lưu ý giá trị trên Stack của tôi có thể khác với máy của các bạn và điều này không quan trọng.Bây giờ chúng ta nhấn F7 để chui vào trong lệnh CALL đông thời quan sát cửa sổ Stack xem có gì xảy ra :

 ![](http://i.imgur.com/aPl7liW.png)

 - Khác với chức năng Follow ở trên lúc này thanh ghi EIP sẽ thay đổi và điều này có nghĩa là chúng ta đang thực thi chương trình con :

 - Cửa số Stack lúc này thay đổi như sau :
![](http://i.imgur.com/nrQnimJ.png)

- Như bạn thấy trên hình minh họa thanh ghi ESP trỏ lên đỉnh của Stack đã được giảm đi thành 0x0013FEA0 và địa chỉ của câu lệnh tiếp theo sau lệnh CALL (địa chỉ trở về) được đẩy vào đỉnh của Stack.Và đây chính là cơ sở cho lệnh RET thoát khỏi chương trình con để quay trở về chương trình chính . Olly cũng đã cung cấp cho chúng ta thông tin : `RETURN to CRACKME.0040124A from CRACKME.00401362`

 - Quay trở lại công việc chúng ta đang làm, các bạn nhấn tiếp F7 để thực hiện lệnh tại địa chỉ 0x00401362 đó là :

```
00401362 /$ 6A 00 PUSH 0		 ; /BeepType = MB_OK
```
 - Lệnh PUSH 0 được thực thi tức là đẩy giá trị 0 vào đỉnh của Stack. Lúc này địa chỉ trở về sẽ được đầy lùi xuống nhường chỗ cho giá trị 0x0 :

 - Chương trình có thể kiểm soát hàng nghìn câu lệnh bên trong một chương trình con, bản thân bên trong chương trình con có thể thực hiện hàng nghìn lệnh PUSH và POP nhưng khi đã ở tại câu lệnh RET thì lúc đó trên stack sẽ chỉ có địa chỉ trở về (chương trình gọi tới chương trình con), để từ đó câu lệnh RET sẽ căn cứ vào địa chỉ trở về này để quay về lệnh tiếp theo dưới lệnh CALL. Tiếp theo ví dụ trên, chúng ta nhấn F8 để thực hiện các lệnh tiếp theo cho tới khi chúng ta dừng lại tại câu lệnh RET.

 - Như chúng ta đã đề cập về lệnh RET ở phía trên, khi ta thực hiện lệnh RET thì nó sẽ căn cứ vào địa chỉ trở về trên Stack để quay về lệnh tiếp theo dưới lệnh CALL. Đồng thời thêm vào đó nó sẽ xóa địa chỉ trở về này khỏi Stack vì giá trị này không còn hữu ích nữa. OK ta nhấn F7 để thực hiện lệnh RETN, ta sẽ được đưa tới địa chỉ `0x0040124A` :

 ![](http://i.imgur.com/QGqydbF.png)

 - Tuy nhiên, trong một số trường hợp ta có thể thực thi lệnh RET mà không cần phải có lệnh CALL. Một ví dụ như sau :
```
PUSH 401256 
RET
```
 - Trong ví dụ trên, lệnh PUSH sẽ đẩy giá trị 0x401256 vào đỉnh của Stack và bên dưới lệnh PUSH này là một lệnh RET. Theo như lý thuyết thì lệnh RET sẽ căn cứ vào địa chỉ trở về được cất trên Stack để quay trở về vậy cho nên khi thực hiện lệnh RET chúng ta sẽ được đưa tới địa chỉ của lệnh tiếp theo sẽ được thực hiện là 0x401256. Qua ví dụ này ta rút ra một nhận xét là cặp lệnh PUSH và RET tương đương với lệnh JMP

 - Bây giờ quay trở lại ví dụ chính của chúng ta, tôi sẽ minh họa một trường hợp khác, trong Olly nhấn Ctrl + G và gõ vào địa chỉ `0x401364`. Ta sẽ ở đây trong Olly

 ![](http://i.imgur.com/gwMEFNu.png)

 - Quá trình này không làm thay đổi thanh ghi EIP, nhấn F2 để đặt một BP tại địa chỉ trên. Mục đích của việc đặt BP là tôi muốn chương trình sau khi thực thi sẽ dừng lại tại nơi mà tôi đã đặt BP.

 ![](http://i.imgur.com/qUPbF99.png)

 - Sau khi đặt BP giống như trên xong, lúc này ta nhấn F9 để Run chương trình :

 ![](http://i.imgur.com/LR6FCUh.png)
 - Tại Crackme v1.0, ta nhấn Help và chọn Register. Cửa sổ Register sẽ hiện ra yêu cầu ta nhập thông tin : 1 Ta nhập thông tin vào :  
 ![](http://i.imgur.com/y3R3qaT.png)

 - Ta nhập thông tin vào :
 ![](http://i.imgur.com/awOQdv7.png)

 - Nhấn OK, ngay lập tức chúng ta sẽ dừng tại điểm mà chúng ta đặt BP trong Olly :
 ![](http://i.imgur.com/CshBLMl.png)

 Lúc này quan sát cửa sổ Stack ta có được như sau :
 ![](http://i.imgur.com/K0PINo6.png)

 - Như các bạn quan sát thấy trên cửa sổ Stack lúc này cố một số dòng “RETURN TO…”, nhưng theo lý thuyết mà chúng ta đã lập luận ở trên thì dòng nào ở vị trí cao nhất trong tất cả các dòng sẽ chính là nơi chứa địa chỉ trở về chương trình chính khi chương trình con thực hiện câu lệnh RET bên trong nó. Vì khi chúng ta ở tại câu lệnh RET thì dòng “RETURN TO…” cao nhất sẽ được đẩy lên đỉnh của Stack và lúc này địa chỉ trở về sẽ là `0x0040124A.`

 - Ta đang dừng lại tại BP đã thiết lập, bây giờ nhấn F8 để trace tới lệnh RET.Tuy nhiên bạn để ý trên hình minh họa trước khi tới được lệnh RET thì sẽ phải đi qua một lệnh CALL :
 `00401378 |. E8 BD000000 CALL MP.&USER32.MessageBoxA> ; \MessageBoxA`

 - Ta không cần quan tâm bên trong nó ra sao chỉ cần biết rằng khi trace over qua nó sẽ hiện ra một thông báo :

 ![](http://i.imgur.com/7jmIDOC.png)

 - Nhấn Ok để tiếp tục, lúc này chúng ta sẽ dừng lại tại câu lệnh RETN và chuẩn bị thực hiện câu lệnh này :
 ![](http://i.imgur.com/oILBH2Q.png)
 - Quan sát trên cửa sổ Stack, ta sẽ có được thông tin của địa chỉ trở về :
 ![](http://i.imgur.com/73Q2KW5.png)

 - Điểm khác biệt của lần phân tích này so với lần phân tích ở trên chính là việc thay đổi thanh ghi EIP, và việc chúng ta thực hiện một câu lệnh CALL (là một phần của chương trình) mà không phải là toàn bộ chương trình.Thanh ghi EIP thay đổi thông qua việc thực thi chương trình và dừng lại tại điểm mà chúng ta đặt BP. Nếu chúng ta tiếp tục thực thi chương trình bằng việc nhấn F9 thì nó sẽ hoàn toàn chạy một cách bình thường như không có gì xảy ra

 - Tuy nhiên điều mà tôi muốn nhấn mạnh ở đây đó là công dụng của cửa sổ Stack, khi chúng ta đang thực thi chương trình và bị dừng lại vì một lý do nào đó thì thông tin mà cửa sổ Stack cung cấp cho chúng ta sẽ cho ta biết được nơi mà thủ tục được gọi cũng như địa chỉ mà nó sẽ trở về. Tuy nhiên các chương trình thường sử dụng các lệnh CALL lồng nhau thì qua cửa sổ Stack này chúng ta cũng có được các thông tin để lần ngược về nơi gọi

 - Phần tiếp theo tôi sẽ lấy một ví dụ minh họa khác, nhấn Ctrl+F2 để restart Olly và edit lại như hình minh họa dưới đây :
 ![](http://i.imgur.com/y1rJXXn.png)

 - Nhấn Enter để Follow vào quan sát nội dung bên trong lệnh CALL :
 ![](http://i.imgur.com/DF9VbJm.png)

 - Như quan sát ở trên thì sau khi chúng ta sửa đổi lúc này câu lệnh đầu tiên sẽ được thực hiện sẽ bắt đầu tại địa chỉ `0x00401245` và kết thục tại câu lệnh RETN tại địa chỉ `0x00401288` (mà ở đây chúng ta thấy là lệnh RETN 10 hơi khác một chút với lệnh RETN).

 - Tại địa chỉ mà chúng ta vừa mới Follow tới là `0x00401245` ta nhấn “-“ để quay về nơi phát sinh ra lời gọi tới nó, sau đó chúng ta nhấn F7 để trace into vào trong lệnh CALL, lú đó thành ghi EIP sẽ thay đổi :
 ![](http://i.imgur.com/CzCktEO.png)

 - Đồng thời trên cửa sổ Stack ta sẽ quan sát thấy địa chỉ trờ về sau khi thực hiện xong :
 ![](http://i.imgur.com/phjwIpS.png)

 - Chúng ta nhìn thấy giá trị là `0x00401005`, điều này là rất đúng và hợp lý. Tuy nhiên ở đây tôi không khẳng định rằng sau khi thực hiện xong Ollydbg sẽ đưa chúng ta trở về đúng nơi mà chúng ta đã thấy ở trên.Lý do là vì chúng ta đã sửa lại hướng thực thi của cả chương trình  điều này chẳng khác gì trong một cỗ máy đang hoạt động trơn tru bị bạn làm thay đổi qui trình hoạt động của nó.

 - Để có thể fix được nó thì trong cửa sổ CPU nhấn chuột phải và chọn
 ![](http://i.imgur.com/0C7vYuK.png)
 - Hoặc có thể nhấn phím tắt là Ctrl + A. Sau khi analyze xong quan sát cửa sổ Stack :
 ![](http://i.imgur.com/8abTwQO.png)
 - Ok nó thông báo cho chúng ta như sau :
 `0x0013FFC0 	00401005 	RETURN to 	CRACKME.<ModuleEntryPoint>+5 	from CRACKME.00401245`

 - Điều này chính là giá trị `0x00401000 (EntryPoint) + 0x5 = 0x00401005`. Rõ ràng là thông tin sau khi Olly analyze mang lại đầy đủ ý nghĩa hơn là thông tin ban đầu mà chúng ta có được. Sau khi nhấn F7 chúng ta sẽ ở đây trong Olly :

 ![](http://i.imgur.com/1C5Aq2P.png)

 - Tại đây chúng ta nhấn tiếp F7 để trace vào bên trong lệnh :
 `00401245 	$ 	E8 	18010000 	CALL 	CRACKME.00401362`

 - Quan sát trên cửa sổ Stack ta có được như sau :
 ![](http://i.imgur.com/1eatwDU.png)

 - Thông qua hình minh họa ở trên ta thấy rằng trước khi đi vào mỗi lệnh CALL địa chỉ của lệnh tiếp theo (dưới lệnh CALL) đều được đẩy lên đỉnh của Stack _ đó chính là địa chỉ trở về. Điều này sẽ giúp cho chúng ta hiểu được cơ chế quản lý khi có các lệnh CALL lồng nhau. Trong ví dụ trên, ta đang ở tại địa chỉ `0x00401362 `và đây chính là nơi mà lệnh tiếp theo được thực hiện.Sau khi thực hiện toàn bộ các lệnh bên trong và tới lệnh RETN tại địa chỉ `0x0040137D`, khi ta thực hiện lệnh RETN này nó sẽ căn cứ vào địa chỉ trở về nằm trên Stack để quay ra ngoài, và lúc này địa chỉ trở về đó là `0x0040124A`. Thực hiện lệnh RETN ta sẽ ở đây theo đúng lý thuyết đã lập luận :

 ![](http://i.imgur.com/LTsXAc1.png)

 - Và trên cửa sổ Stack ta có thông tin giống như hình minh họa ở trên, điều này có nghĩa là ta vẫn đang nằm trong lòng lệnh CALL (`00401000 >/$ E8 40020000 CALL CRACKME.00401245`) và địa chỉ trở về sẽ là` 0x00401005 `bên dưới lệnh CALL này . Tuy nhiên như tôi nói ở trên nếu bạn tiếp tục thực hiện các lệnh tiếp theo thì chương trình sẽ không return về đúng địa chỉ, đó là bởi vì chúng ta đã phá vỡ cấu trúc của chương trình. Nếu bạn cố tình thực hiện thì sẽ crash đó, ráng chịu!!

###7. các lệnh liên quan đến chuỗi, vòng lặp và các chế độ địa chỉ<a name="7"></a>

####7.1 giới thiệu về vòng lặp và lệnh LOOP<a name="7.1"></a>

**giới thiệu về vòng lặp**

 - Một vòng lặp là một chuỗi các lệnh được lặp lại. Số lần lặp có thể đã được xác định trước hoặc phụ thuộc vào điều kiện. Thông thường trong Asm thì bộ đếm vòng lặp thường là thanh ghi ECX, nó được khời tạo bằng số lần lặp. Sau khi thực hiện xong các lệnh bên trong vòng lặp giá trị của thanh ghi ECX sẽ tự động giảm đi 1. Nếu như giá trị của ECX còn khác 0 thì các lệnh trong vòng lặp còn tiếp tục được thực hiện , còn nếu như ECX bằng 0 thì sẽ thoát khỏi vòng lặp và thực hiện các lệnh tiếp theo bên dưới vòng lặp. Lấy ví dụ minh họa:
 ```
 XOR ECX, ECX 	\ Đoạn lệnh này thực hiện nhiệm vụ khởi gán cho thanh ghi 
 ADD ECX, 15 	/ ECX.Số lần lặp lúc này sẽ là 15 và được lưu trong thanh ghi ECX. 
 LOOP: 
 DEC ECX 		// Giá trị của thanh ghi ECX sẽ được giảm đi 1 
 ……………… 		\ Thực hiện các lệnh trong thân vòng lặp 
 ……………… 		/ 
 TEST ECX, ECX 	\ Chừng nào giá trị của ECX còn khác 0 
 JNE LOOP 		/ thì còn tiếp tục thực hiện
 ```

 - Minh họa ví dụ trên trong Olly như sau :
 ![](http://i.imgur.com/K6RUj84.png)

 - Vùng mà tôi khoanh vàng ở trên hình đó chính là vòng lặp, các lệnh trong vòng lặp này sẽ được thực hiện lặp lại cho tới khi nào thanh ghi ECX có giá trị 0. Các câu lệnh bắt đầu từ `0x00401008` cho tới `0x0040100C` chính là thân của vòng lặp. Do chỉ mang tính chất minh họa nên tôi để toàn lệnh NOP, các bạn có thể thay thế bằng các lệnh khác. Hiện tại chúng ta đang ở tại câu lệnh `XOR ECX, ECX `, nhấn F7 để trace và quan sát giá trị của thanh ghi ECX .

 - Lệnh` XOR ECX, ECX` sẽ xóa thanh ghi ECX về 0 :

 ![](http://i.imgur.com/NSm5oMY.png)

 - Lệnh `ADD ECX, 15` tương đương với việc gán ECX = 0x15 :

 ![](http://i.imgur.com/EQbcJXV.png)

 - Nhấn F7 và trace qua lệnh `DEC ECX.` Giá trị của ECX lúc này sẽ đươc giảm đi 1 (`ECX = ECX -1 = 0x15 – 0x1 = 0x14`) :

 - Tiếp tục thực hiện cho tới khi chúng ta dừng lại tại lệnh TEST ECX, ECX, câu lệnh này sẽ kiểm tra xem giá trị của ECX đã bằng 0 chưa ? Nếu bằng 0 thì cờ Z sẽ được bật lên. Tuy nhiên lúc này giá trị ECX đang là 0x14, do đó khi trace qua lệnh TEST thì cờ Z không được active và vì vậy lệnh JNZ ở bên dưới sẽ nhảy lại về vị trí 0x00401007 (nơi bắt đầu của vòng lặp)
 ![](http://i.imgur.com/SjOUtfB.png)

 - Trace tiếp để thực hiện các lệnh cho tới khi giá trị của thanh ghi ECX =0x0, lúc này lệnh TEST kiểm tra thấy giá trị của ECX = 0x0 cho nên nó sẽ active cờ Z. Khi cờ Z được active nên câu lệnh JNZ sẽ dựa vào cờ Z và đưa ra quyết định sẽ không nhảy về vị trí 0x00401007

 ![](http://i.imgur.com/w1O3EeY.png)

 - Các bạn sẽ dễ dàng nhận thấy mũi tên tại lệnh JNZ đã chuyển sang màu xám, điều đó có nghĩa là lệnh nhảy sẽ không được thực hiện. Còn nếu như mũi tên này có màu đỏ như hình bên trên thì tức là lệnh nhảy sẽ được thực hiện. Do mũi tên đã chuyển sang màu xám cho nên khi ta tiếp tục trace, chúng ta sẽ thoát ra khỏi vòng lặp và thực hiện các lệnh tiếp theo bên dưới. Đây chỉ là một ví dụ minh họa rất đơn giản để các bạn hình dùng về vòng lặp, các cấu trúc lặp như For, While và Repeat các bạn có thể đọc thêm trong các tài liệu về lập trình ASM.

 **Lệnh LOOP :**

 -Câu lệnh này thường được sử dụng để thực hiện vòng lặp FOR. Nó có dạng như sau : 
```
LOOP Nhãn
```

 - Bộ đếm vòng lặp được khởi tạo trong thanh ghi ECX. Mỗi lần thực hiện lệnh LOOP, thanh ghi ECX sẽ tự động được giảm đi 1, nếu thanh ghi ECX khác 0 thì điều khiển được chuyển tới Nhãn (tức là lặp lại lệnh bắt đầu từ vị trí của Nhãn). Nếu ECX = 0, lệnh tiếp theo sau lệnh LOOP sẽ được thực hiện. Với câu lệnh LOOP này ta có thể thay thế các lệnh đã minh họa ở mục trước như sau :
 ![](http://i.imgur.com/eHQ1rI0.png)

 - Như đã nói, vì chỉ mang tính chất minh họa cho nên tôi để toàn lệnh NOP cho đơn giản và dễ hiểu. Tượng tự như ở trên ta thực hiện lệnh XOR và lệnh ADD để khởi tạo giá trị cho thanh ghi ECX. Sau đó ta trace cho tới câu lệnh LOOP, giá trị của ECX lúc này đang là 0x15.
 ![](http://i.imgur.com/q1C6xYj.png)

 - Bây giờ ta trace để thực hiện lệnh LOOP, ngay lập tức thanh ghi ECX sẽ được giảm xuống (trừ đi 1) và quyền điều khiển được chuyển tới **Nhãn** (địa chỉ `0x401007`).

 ![](http://i.imgur.com/4wZNLHx.png)

 - Tiếp tục thực hiện cho tới khi giá trị của thanh ghi ECX được giảm xuống còn 0x1, và trace tới lệnh LOOP.

 - Lúc này khi ta thực hiện lệnh `LOOP, ECX `sẽ được giảm xuống là 0x0, quyển điều khiển sẽ không được chuyển cho Nhãn nữa mà chuyển cho câu lệnh bên dưới lệnh LOOP. Ngoài lệnh LOOP trên các bạn có thể tham khảo thêm về các lệnh` LOOPE/LOOPZ, LOOPNE/LOOPNZ` trong các tài liệu.

####7.2 lệnh MOVS<a name="7.2"></a>

 - Câu lệnh này sẽ sao chép nội dung dữ liệu được định địa chỉ bởi DS:ESI đến vùng dữ liệu được định địa chỉ bởi ES:EDI. Nội dung của chuối gốc không bị thay đổi. Sau khi chuối được chuyển cả hai thanh ghi ESI và EDI đếu được tự động tăng lên 1 nếu như cờ DF = 0 hay giảm đi nếu DF = 1

 - Nếu DF = 0, ESI và EDI được xử lý theo chiều tăng của các địa chỉ trong bộ nhớ : từ trái qua phải trong chuỗi. Ngược lại nếu DF = 1, ESI và EDI được xử lý theo chiều giảm dần các địa chỉ bộ nhớ : từ phải qua trái trong chuỗi.

 - Lệnh MOVS thường không cần bất kì tham số nào, tuy nhiên khi viết trong Olly chúng ta phải chỉ rõ ra như sau : `MOVS DWORD PTR IS:[EDI], DWORD PTR DS:[ESI]`. Lấy một ví dụ nhỏ để minh họa :
![](http://i.imgur.com/IKnohS8.png)

 - Câu lệnh đầu tiên sẽ khời tạo giá trị cho ESI và nội dung dữ liệu sẽ được đọc ra từ đó. EDI cũng được khởi tạo một giá trị, và tại đó dữ liệu sẽ được copy vào. Chúng ta có thể xem nội dung tại hai địa chỉ trên bằng cách vào cửa sổ Dump, nhấn Ctrl+G và gõ vào địa chỉ cần xem nội dung hoặc tại cửa sổ CPU, chuột phải chọn **Follow in Dump > Immediate Constant **:

 ![](http://i.imgur.com/ZAoj9ex.png)

 - OK vậy là ta đã biết được nội dung của chuỗi gốc và chuối đích (Chuỗi gốc là nơi mà ta đọc dữ liệu ra và chuỗi đích là nơi mà ta copy dữ liệu vào) Bây giớ nấn F7 để trace qua lệnh MOVS, quan sát cửa sổ dump ta sẽ thấy được 4bytes của chuối gốc được copy vào chuỗi đích :

![](http://i.imgur.com/qiGgCmH.png)


####7.3 lệnh REP <a name="7.3"></a>

 - Không giống như lệnh MOVS chỉ chuyển một doubleword tại địa chỉ DS:(E)SI tới địa chỉ ES:(E)DI, lệnh REP cho chúng ta chuyển cả chuỗi . Để chuyển cả chuỗi đầu tiên chúng ta phải khởi tạo thanh ghi (E)CX với số N bằng số doubleword trong chuỗi nguồn và thực hiện lệnh như sau :

 ```
 REP MOVS
 ```
  - Lệnh REP có tác dụng làm cho MOVS đươc thực hiện N lần. Sau mỗi lệnh MOVS thì (E)CX được giảm đi 1 cho đến khi nó bằng 0. Tôi lấy ví dụ minh họa như sau :

  ![](http://i.imgur.com/3dJC4Ip.png)

  - Ta quan sát nội dung tại 0x0040365C , nơi chứa chuỗi gốc :

  - Trong đoạn code trên, bạn đế ý thấy ECX được khởi tạo giá trị là 4. Vậy tức là sẽ có 4 lần copy từ chuỗi nguồn vào chuỗi đích, mỗi lần copy một doubleword. Bây giờ ta Trace từ từ tới lệnh REP :

  - Các thông tin hiện ra trong Tip Window rất đầy đủ, giờ ta nhấn F7 và quan sát nội dung tại chuỗi đích, tức là tại 0x0040369C. Tại sao lại nhấn F7 mà không phải là F8, vì nếu các bạn nhấn F8 lệnh REP MOVS sẽ copy thẳng một mạch toàn bộ 4 doubleword từ chuỗi gốc sang chuỗi đích,đồng thời thanh ghi ECX sẽ được giảm về 0. Ta sử dụng F7 để quan sát từng lần copy một :

![](http://i.imgur.com/GbYzt6n.png)

 - Như quan sát trên hình minh họa, ta thấy 4 bytes đầu tiên được chuyển tới và thanh ghi ECX lúc này bị giảm đi 1 (`ECX = 0x4 -0x1 = 0x3`). Thêm vào đó 2 thanh ghi ESI và EDI được tăng thêm 4 để trỏ vào vị trí tiếp theo :

 - Tiếp tục nhấn F7 và quan sát :
 ![](http://i.imgur.com/4nvE1kY.png)

 - Cuối cùng nhấn F7 cho tới khi ECX = 0x0, lúc này toàn bộ chuỗi gốc đã được copy sang chuỗi đích. Rất rõ ràng và dễ hiểu!

####7.4 Lệnh LODS :<a name="7.4"></a>

 - Lệnh LODS nạp vào EAX một double word do ESI chỉ ra trong đoạn DS, sau đó ESI tự động tăng/ giảm để chỉ vào phần tử tiếp theo tùy theo cờ hướng DF. Ví dụ minh họa :
 ![](http://i.imgur.com/8vxEVSp.png)

 - Trace từ từ đến lệnh LODS và quan sát :

 ![](http://i.imgur.com/rMk8yhV.png)
 - Nhấn F7 để thực hiện lệnh LODS, kết quả ta sẽ có được như sau :

 ![](http://i.imgur.com/YVpVUuZ.png)

####7.5 Lệnh STOS<a name="7.5"></a>

 - Lệnh này ngược lại với lệnh LODS, có tác dụng chuyển nội dụng của thanh EAX đến vị trí đã được đĩnh nghĩa bởi ES:EDI. Sau khi thực hiện xong thì EDI tự động tăng/giảm để chỉ vào phần tử tiếp theo tùy theo cờ hướng. Ví dụ :

 - Trace tới lệnh STOS và quan sát cửa sổ TIP :

 ![](http://i.imgur.com/KCjVvEN.png)

 - Sau khi thực hiện lệnh STOS, kết quả ta có được như sau :
 ![](http://i.imgur.com/lKHoZJz.png)

####7.6 Lệnh CPMS :<a name="7.6"></a>

 - Cú pháp : CPMS Chuỗi đích, Chuỗi gốc Lệnh này dùng để so sánh nội dung của ESI với nội dung của EDI. Lệnh này chỉ tạo cờ, không lưu kết quả so sánh, sau khi so sánh các toán hạng không bị thay đổi.

 - Về bản chất của lệnh CPMS là lệnh này trừ dw tại địa chỉ DS:ESI cho dw tại địachỉ ES:EDI và thiếp lập cờ. Nếu kết quả của phép trừ bằng 0x0 thì cờ Z được bật cũng đồng nghĩa hai chuỗi này giống nhau :

####7.7 các chế độ địa chỉ <a name="7.7"></a>

**DIRECT (Trực tiếp) :**

 - Trong chế độ địa chỉ trực tiếp, một toán hạng chứa địa chỉ lệch của ô nhớ dùng chứa dữ liệu còn toán hạng kia chỉ có thể là thanh ghi (không được là ô nhớ). Đây là cách chung được sử dụng để tham chiếu tới một địa chỉ trong bộ nhớ, một số ví dụ minh họa :

```
mov dword ptr [00513450], ecx 
mov ax, word ptr [00510A25] 
mov al, byte ptr [00402811] 
CALL 452200 JMP 421000 
```
 - Trong trường hợp này chúng ta sẽ không gặp vấn đề gì trong việc biểu diễn địa chỉ cũng như biết được nơi mà lệnh nhảy sẽ nhảy tới v..v... Chúng ta chỉ việc chỉ rõ các địa chỉ này trong các lệnh mov, lệnh jmp và cả lệnh Call.

**INDIRECT (Gián tiếp) :**

 - Thường được gọi là chế độ địa chỉ gián tiếp qua thanh ghi, trong chế độ địa chỉ này một toán hạng là một thanh ghi được sử dụng để chứa địa chỉ lệch của ô nhớ chứa dữ liệu, còn toán hạng kia chỉ có thể là thanh ghi mà không được là ô nhớ.
```
mov dword ptr [eax], ecx  
CALL EAX  
JMP [ebx + 4]
```

 - Như các ban thấy với cơ chế đánh địa chỉ như trên chúng ta sẽ gặp khó khăn trong việc xác định vị trí, không biết được vị trí mà câu lệnh nhảy sẽ nhảy đến, đoạn routine nào sẽ được gọi bởi lệnh call. Chỉ có sử dụng Olly để debug thì chúng ta mới biết được chính xác thông qua việc xem xét các giá trị của các thanh ghi

 - Rất nhiều chương trình sử dụng cơ chế địa chỉ này để ngăn cản hay hạn chế quá trình phân tích chương trình của của cracker/reverser. Lấy một ví dụ để dễ hiểu, khi bạn cho Olly phân tích một chương trình bằng cách load target vào trong Olly, bạn sẽ gặp câu lệnh tương tự như sau :

```
PUSH 		DWORD PTR SS:[EBP+8] 		; |hWnd
```
 - Nếu như bạn không tiến hành debug chương trình, thì thử hỏi khi bạn nhìn vào lệnh trên bạn làm cách nào để biết được nội dung gì sẽ được đẩy lên Stack ?

 - Câu trả lời như sau, để biết được nội dung gì sẽ được đẩy lên Stack bạn có thể debug trong Olly bằng cách trace từ từ tới câu lệnh trên hoặc đặt một Break point tại câu lệnh này và thực thi chương trình để chương trình dừng tại chỗ mà chúng ta đã set BP. Qua đó chúng ta sẽ quan sát được sự thay đổi và giá trị của thanh ghi EBP, dựa vào thông tin thu được ta sẽ xác định được nôi dung của ô nhớ [EBP + 8].Cụ thể như sau :
 ![](http://i.imgur.com/KjSeIuI.png)

 - Nhấn F9 để run chương trình, ngay lập tức chúng ta sẽ break tại chỗ mà ta vừa set BP. Quan sát trên cửa sổ Registers, ta thấy được giá trị của EBP là `0x0013FFF0`, vậy EBP + 8 sẽ là `0x0013FFF8`. Cửa sổ tip sẽ cho ta biết nội dung tại ô nhớ `0x0013FFF8` là gì :
 ![](http://i.imgur.com/D1IssFO.png)

 - Vậy tóm lại lệnh PUSH [EBP+8] sẽ PUSH nội dung của 0x0013FFF8 lên Stack. Nếu cẩn thận các bạn có thể vào cứa sổ DUMP, nhấn Ctrl + G, nhập địa chỉ trên vào để quan sát và kiểm tra :
 ![](http://i.imgur.com/Or3OoUg.png)

 - Giờ ta nhấn F7 để thực hiện lệnh Push, đồng thời quan sát cửa sổ Stack để thấy được kết qủa :
 ![](http://i.imgur.com/m1akASZ.png)

###8. các thuật ngữ cơ bản, làm việc với API và patch thông qua cờ<a name="8"></a>

 **EntryPoint (EP) và  OEP (Original Entry Point)**

 - Về cơ bản thuật ngữ EntryPoint (EP) ám chỉ điểm bắt đầu của một chương trình nơi mà tại đó trở đi chương trình sẽ được thực thi một cách bình thường. Không nên bị nhầm lẫn giữa EP và OEP (Original Entry Point), OEP là một thuật ngữ khác mà chúng ta sẽ tìm hiểu ở các phần tiếp theo sau của bộ tuts này. Sau khi chúng đã open một chương trình trong Olly, đợi cho quá trình phân tích kết thúc thì Olly sẽ đưa chúng ta dừng lại tại EntryPoint của chương trình đó.

 ![](http://i.imgur.com/H1XQKiz.png)

 - Cụ thể trong trường hợp của chúng ta, crackme này có EP là `0x401000` và Olly cũng đã chỉ cho chúng ta thấy sau khi analyze crackme trên nó đang dừng lại tại EP như hình minh họa mà các bạn đã thấy ở trên. Hầu hết tất cả các chương trình (tức là khoảng 99% các trường hợp) khi chúng ta load nó bằng Olly thì đều dừng lại tại EP của chương trình đó, ngoài trừ một số trường hợp đặc biệt có sự **“can thiệp”** khiến cho sau khi load chươn trình vào Olly ta lại không dừng lại tại EP, đây cũng là mà thủ thuật đặc biệt mà chúng ta có thể sẽ có dịp tìm hiểu sau này. Còn trong lúc này nó mới chỉ là khái niệm mà thôi , chúng ta còn nhiều thời gian để mò mẫm lắm!

 - Tiếp theo là một khái niệm khác nữa mà chúng ta cũng cần xem xét đến đó chính là các hàm Application Programming Interface (APIs) và thư viện DLL.
 ![](http://i.imgur.com/BdZDgwc.png)
  - Lý thuyết cũng như kiến thức về API và DLL các bạn có thể tham khảo quyển PE File Format mà tôi đã dịch hoặc các nguồn từ Internet. Theo như hình minh họa ở trên các bạn thấy chỗ khoanh đỏ chính là một lời gọi tới hàm API .
```
 CALL 	LoadIconA
```
 - Có thể nói nôm na về API như sau, hệ điều hành Windows xây dựng nên một tập hợp rất nhiều các hàm/thủ tục, những hàm/thủ tục này sẽ giúp bạn thực hiện những công việc mà bạn phải lặp đi lặp lại hàng ngày, rất nhàm chán trong quá trình coding. Tập hợp những hàm/thủ tục mà Windows xây dựng được đặt cho cái tên chung là API, với sự có mặt của API các lập trình viên không phải phí công sức cho những công việc vốn đã được xây dựng sẵn. Các API này tuy theo nhóm công việc, mục đích thực hiện sẽ được tập hợp vào trong một file thư viện DLL để khi cần đến người lập trình chỉ cần tra từ thư viện đó xem hàm đó có nằm trong thư viện đó không, nếu có thì chỉ việc gọi ra và sử dụng mà thôi.

 - Nhìn vào hình minh họa ở trên, các bạn thấy Olly đã chỉ cho ta thấy hàm LoadIconA nằm trong Dll là **User32.dll.**

 - Ta lấy một ví dụ đơn giản với hàm MessageBoxA như sau, tôi không hề biết hàm này nằm ở thư viện dll nào và cũng chẳng biết địa chỉ của nó là gì? Vậy tôi làm thế nào đây để có được thông tin về hàm này, rất đơn giản Olly đã hỗ trợ cho chúng ta khả năng tìm kiếm địa chỉ theo tên hàm. Tại chỗ Command Bar của Olly ta gõ tên hàm vào như sau :

 ![](http://i.imgur.com/MB4LRUp.png)

  - Wow, ngay lập tức Olly tìm cho ta ngay địa chỉ của hàm MessageBoxA, bây giờ ta đi tới địa chỉ này để xem hàm mà chúng ta tìm nằm trong thư viện nào. Tại Olly, nhấn chuột phải và chọn **Go to > Expression :**

  ![](http://i.imgur.com/JG14usu.png)

 - Nhập địa chỉ đã tìm được vào textbox và nhấn OK.

 - Olly sẽ đưa ta tới địa chỉ của hàm MessageBoxA
 ![](http://i.imgur.com/cxFJck7.png)

  - Theo như hình trên thì ta thấy ngay rằng hàm **MessageBoxA** thuộc về thư viện `Dll là User32.dll.` Hàm này bắt đầu tại `0x7e45058a` và kết thúc bằng lệnh Retn 10 tại `0x7e4505d0.`

  - Cũng có một cách khác nữa giúp cho chúng ta tìm thấy hàm MessageBoxA, cách tương tự như trên nhưng thay vì gõ địa chỉ hàm thì ta gõ thẳng tên của hàm vào và nhấn OK:

  ![](http://i.imgur.com/VcdHxHe.png)

  - Như bạn thấy ở trên việc tìm ra hàm **MessageBoxA** có vẻ rất dễ dàng, tuy nhiên không phải lúc này cũng đơn giản như thế. Với 2 phương pháp trên bắt buộc bạn phải nhớ chính xác từng chữ cái cũng như cú pháp chữ hoa chữ thường trong tên hàm đó. Vậy trong trường hợp ta chỉ nhớ mang máng tên hàm và không nhớ viết đúng tên hàm theo đúng form thì thế nào, Olly đã hỗ trợ cho ta một cách khác để tìm đến hàm đó. Ok, để thực hiện, ta quay lại chỗ EP của chương trình (đơn giản bằng cách bấm phím „–„ trên bàn phím vì lúc này bạn đang ở tại đ/c của MessageBoxA), sau đó thực hiện như hình dưới (phím tắt là Ctrl + N) :

  ![](http://i.imgur.com/uoO0dAv.png)
 - ngay lập tức một loạt các hàm được sử dụng trong module hiện tại được liệt kê ra như các bạn thấy ở hình sau :

 ![](http://i.imgur.com/yhVkrT0.png)

 - Nhìn như trên thì rối quá ta không biết phải mò ra** MessageBoxA** ở đâu trong một rừng tên như thế này, để tìm kiểm được đúng hàm cần tìm trước tiên tại chính cửa sổ trên ta gõ chữ cái đầu của tên hàm mà cụ thể ở đây là chữ M. Olly sẽ đưa chúng ta đến vị trí của những hàm bắt đầu bằng chữ M

 ![](http://i.imgur.com/ApiQahA.png)

 - Tiếp tục gõ những chữ cái tiếp theo trong tên hàm Olly sẽ đưa ta đến đúng vị trí cần tìm :

 ![](http://i.imgur.com/2PLgrm8.png)

 - Tại hàm tìm được ta nhấn chuột phải và chọn Follow import in Disassembler
 ![](http://i.imgur.com/OZak7BV.png)
 - Ok, vậy là chúng ta đã trải qua một số phương pháp khác nhau để tìm kiếm thông tin về một hàm API, bây giờ chúng ta tiếp tục trở lại với phần tiếp theo của bài viết. Sau khi tìm kiếm được thông tin về hàm MessageBoxA như hình minh họa ở trên, ta tiến hành đặt một điểm ngắt hay còn gọi với một thuật ngữ là Break Point (BP) . Ta làm như sau :
 ![](http://i.imgur.com/qNve0NN.png)

 - Việc thiết lập BP như trên cũng tượng tự với cách làm khác như sau, tại cửa sổ Command Bar ta gõ vào : Bp MessageBoxA

 - Ok ta vừa mới đặt BP, giờ ta kiểm tra xem kết quả ta đặt như thế nào. Chuyển qua cửa số BP bằng cách nhấn phím tắt (Alt + B) :
 ![](http://i.imgur.com/hmxpREy.png)

 - Như bạn thấy ở trên, tôi đã đặt một BP tại địa chỉ bắt đầu của hàm** MessageBoxA,** bây giờ khi tôi cho thực thi crackme này trong Olly nếu như có bất kì một thông báo nào bắn ra thì ta sẽ dừng lại tại vị trí mà ta đã đặt BP. Để kiểm nghiệm điều này, ta tiến hành thực thi crackme bằng cách nhấn F9 :

 - Vào menu Help và chọn Register, cửa sổ yêu cầu nhập User Name và Serial hiện ra :

 ![](http://i.imgur.com/wxWahMj.png)

 - Ta nhập thử Fake info vào những text box, sau đó nhấn Ok. Ngay lập tức Olly sẽ dừng lại và dừng đúng chỗ mà chúng ta đặt BP :
 ![](http://i.imgur.com/7fxRq9T.png)
  - Vậy ta đoán ngay lúc ta nhấn Ok sẽ có một thông báo bắn ra, tuy nhiên ta đã cho Olly bắt hành động này nên Olly đã dừng lại tại đầu hàm. Bây giờ ta chuyển qua cửa sổ Stack ta sẽ có được những thông tin sau :
  ![](http://i.imgur.com/7ml4jXJ.png)

  - Theo thông tin mà hình cung cấp các bạn có thể thấy rằng mỗi hàm Api trước khi chuẩn bị được gọi thì các tham số của hàm sẽ được đẩy lên Stack. Các tham số này bạn có thể tham khảo tại file Help là Win32.hlp. Ok giờ tại cửa sổ Stack ta chọn như sau :

  ![](http://i.imgur.com/qCvsbQ6.png)

  - Ta sẽ quay lại cửa sổ code của chương trình và dừng tại vị trí sau :

  ![](http://i.imgur.com/knalrFo.png)

  - Theo như lý thuyết về hai lệnh CALL và RET mà tôi đã giới thiệu ở phần trước thì chúng ta sẽ không ngạc nhiên lắm khi ta Follow theo địa chỉ trên thì Olly lại đưa ta đến lệnh Ret mà không phải là lệnh Call.

 - Ok vậy là như các bạn đã thấy, khi thông tin về User name và Serial mà chúng ta nhập vào không đúng thì chúng ta sẽ nhận được một thông báo với nội dung như sau :

```
004013AD |. 6A 30 push 30 ; /Style = MB_OK|MB_ICONEXCLAMATION|MB_APPLMODAL
004013AF |. 68 60214000 push 00402160 ; |Title = "No luck!"
004013B4 |. 68 69214000 push 00402169 ; |Text = "No luck there, mate!"
004013B9 |. FF75 08 push dword ptr [ebp+8] ; |hOwner
004013BC |. E8 79000000 call <jmp.&USER32.MessageBoxA> ; \MessageBoxA

```
 - Giờ ta quay trở lại chỗ đặt BP để kiểm chứng thông tin mà ta vừa nói ở trên.
 ![](http://i.imgur.com/MeQ3H89.png)

 - Ta đặt một BP tại lệnh Ret 10 :

 ![](http://i.imgur.com/C5Ezubq.png)
 - Sau đó nhấn F9 để thực thi chương trình, chúng ta nhận được thông báo xong :

 ![](http://i.imgur.com/Yq8t1Vx.png)
 - đúng như thông tin mà ta có được ở trên, giờ ta nhấn OK ngay lập tức sẽ dừng lại tại lệnh Ret 10 :
 ![](http://i.imgur.com/64KiNEi.png)

 - Lệnh Ret 10 này cho ta biết ta đang ở điểm kết thúc của hàm API MessageBoxA, khi lệnh này được thực thì thi ta sẽ quay trở lại đoạn code chính của chương trình. Nhưng trước khi thực hiện lệnh này, ta nhìn qua cửa sổ Stack sẽ có được thông tin địa chỉ mà khi thực hiện lệnh Ret 10 ta sẽ quay về đó :
![](http://i.imgur.com/V1bIms8.png)
 - Địa chỉ mà tôi khoanh đỏ ở trên chính là địa chỉ của lệnh bên dưới lời gọi tới hàm **MessageBoxA**. Ta nhấn F7 để trace qua lệnh Retn 10, khi đó ta sẽ trở về địa chỉ `0x004013C1` nhưng đồng thời khi ta thực hiện lệnh này thì thanh ghi Esp cũng tự động được cộng thêm 0x10 vào, tức là `ESP =ESP + 0x10 = 0x0013FE90 + 0x10 = 0x0013FEA0.` Ok, sau khi nhấn F7 như đã nói ta sẽ tới đây :

 ![](http://i.imgur.com/a6InnhE.png)
 - Để ý cửa sổ Stack :
 - Như hình trên ta đang ở 0x004013C1, bên trên nó là một lời gọi tới hàm MessageBoxA, hình này cho chúng ta biết được chúng ta đã nhập thông tin về Name và Serial bị sai cho nên thông báo “No luck…” sẽ bắn ra!! Bây giờ ta tiếp tục nhấn F9 thêm một lần nữa :

 ![](http://i.imgur.com/RKSxsTe.png)

 - Bùm…ta lại break tại MessageBoxA, tiếp tục dòm qua cửa sổ Stack :
 - Chuột phải tại dòng đầu tiên và chọn Follow in Disassembler :
 ![](http://i.imgur.com/wg5fOTG.png)
 - Olly đưa ta đến địa chỉ `0x0040137D`, bên trên tại `0x00401378` tiếp tục là một lời gọi tới hàm **MessageBoxA** :
![](http://i.imgur.com/5jHcWC0.png)

- Như vậy, tổng kết lại chúng ta thấy rằng có hai đoạn code đều Show ra cái Nag là “No luck…”, vậy ta phỏng đoán rằng vậy chúng ta sẽ có hai đoạn check liên quan đển UserName và Serial nhập vào. Có thể cái Nag đầu tiên mà chúng ta nhận là cái Nag liên quan tới việc Check Name, còn cái Nag tiếp theo mà chúng ta thấy ở trên hình là cái Nag liên quan đến check Serial . Chà chà có vẻ mệt đây!!

- Tại vị trí trên, dịch lên một chút bạn sẽ thấy có thêm một lời gọi tới hàm **MessageBoxA **nữa :
![](http://i.imgur.com/TY266nZ.png)

 - Hình trên sẽ cho ta biết có 2 Nag liên quan đến việc nhập Serial, nếu ta nhập đúng thì hiện thông báo ở chỗ được tô vàng, nếu nhập sai thì sẽ hiện thông báo ở chỗ được tô xanh. Để ý trong hình trên ta thấy được có dấu „$’, dấu này thông báo cho chúng ta biết ta đang ở trong thân của một lời gọi hàm/thủ tục, vậy để biết được lời gọi này xuất phát ở đâu chúng ta chỉ việc chọn dòng có chứa dấu $ và nhìn xuống cửa sổ bên dưới :
 ![](http://i.imgur.com/0Yu22Os.png)
 - Vậy là Olly đã giúp chúng ta biết được địa chỉ nơi mà có lời gọi gọi tới đoạn code trên đó chính là tại `0x00401245`, nhấn chuột phải tại dòng tô màu xanh ở trên và chọn `Go to Call from 00401245` :

 ![](http://i.imgur.com/lh7L8qJ.png)

 - có vẻ như chúng ta đang đứng tại vị trí chứa đoạn code so sánh. Vậy ta lập luận như sau, nếu kết quả so sánh là đúng thì chúng ta sẽ nhảy tới địa chỉ 0x0040124C và thực hiện lời gọi hàm tại địa chỉ này, mà theo hình trên thì lời gọi này sẽ hiện thông báo “Greate work…”, còn ngược lại nếu kết quả so sánh là sai thì ta sẽ đi tới lời gọi tại 0x00101245 và thực hiện lời gọi hàm hiện thông báo “No luck …” . Ta đặt thử một BP tại lệnh JE :
 - Xóa hết các BP liên quan đến API MessageBoxA đi, mở cửa sổ Break Point (Alt + B)
 ![](http://i.imgur.com/N4z7jpD.png)

 - Ta loại bỏ hai BP mà ta đặt tại Module User32 đi chỉ để lại BP mà ta đặt tại lệnh JE, để loại bỏ một BP thì chỉ việc nhấn chuột phải tại BP đó và chọn Remove hoặc nhấn phím tắt Del.

 ![](http://i.imgur.com/2ZoUvnT.png)

 - Sau khi remove BP chỉ để lại một BP duy nhất như hình trên, ta nhấn F9 để thực thi chương trình. Lúc này cái Nag “No luck…” sẽ xuất hiện, lý do là vì lúc trước ta set BP tại MessageBoxA nhưng ta vừa bỏ đi rồi nên nhận cái Nag đó là đúng thôi . Nhấn Ok, sau đó tiến hành nhập lại thông tin về Name và Serial, lần này ta nhập thử một cái name khác thử xem :
 ![](http://i.imgur.com/NCNSOhf.png)

  - Nhấn OK, ta sẽ dừng lại tại BP. Oh… như vậy kết luận sơ bộ ban đầu của tôi về việc crackme này có tới hai chỗ check liên quan tới Name và Serial là đúng. Vì nếu như bạn nhập Name như tôi nhập lần đầu ở trên thì khi chúng ta nhấn Ok chúng ta sẽ nhận Nag “No luck…” trước khi chúng ta break tại điểm đặt BP. Sau khi chúng ta nhấn Ok tại Nag thì chúng ta mới dừng lại tại BP mà ta đã set :
  ![](http://i.imgur.com/DvEVOx4.png)

  - Chúng ta thấy rằng dựa theo kết quả so sánh ở lệnh CMP, nếu như giá trị của eax không bằng giá trị của ebx thì lệnh nhảy sẽ không được thực hiện. Khi lệnh nhảy không được thực hiện thì lệnh Call bên dưới nó tại địa chỉ 0x00401245 sẽ được thực hiện tiếp theo, và chúng ta biết rằng lệnh CALL 401362 chính là hàm show ra Nag là “No luck..”, nếu như bạn không nhớ thì bạn chọn lệnh Call đó và nhấn Enter để Follow :
  ![](http://i.imgur.com/728STu2.png)

  - Như chúng ta thấy lệnh JE không được thực hiện vì Serial mà chúng ta nhập vào là sai. Ngoài ra ta biết rằng lệnh nhảy JE này phụ thuộc vào cờ Z, vậy muốn cho lệnh này thực hiện thì ta có một mẹo nhỏ là thay đổi giá trị của cờ bằng cách nhấp đúp vào cờ Z :

  ![](http://i.imgur.com/JV792FW.png)

  - Theo hình trên thì cờ Z đã chuyển thành 1, điều này đồng nghĩa với việc là giá trị của thanh ghi eax bằng giá trị của thanh ghi ebx, cũng đồng nghĩa với nếu như thực hiện lệnh Cmp thì lệnh này sẽ thực hiện phép trừ hai toán hạng bằng nhau cho ra kết quả là 0. Mà khi kết quả bằng 0 thì cờ Z được bật lên  và chúng ta cùng “nhảy” lolz … như hình minh họa dưới đây
  ![](http://i.imgur.com/Er9lEIs.png)

  - Vậy là nếu ta nhấn F8 để trace thì lệnh JE sẽ đưa chúng ta tới lệnh tiếp theo tại địa chỉ `0x0040124C`. Ta biết rằng lệnh Call tại địa chỉ này chính là show Nag “`Great work …`” , nếu bạn không nhớ hãy thử Follow tại lệnh Call này bằng cách nhấn Enter :

  ![](http://i.imgur.com/R9nqSBF.png)

  - Nào ta cùng thử xem có đúng không nhé, nhấn F9 để run và bùm :
![](http://i.imgur.com/kK4v2HD.png)

 - Vậy tại đây chúng ta có thể khẳng định lệnh Cmp eax, ebx chính là lệnh so sánh liên quan đến Serial, phụ thuộc vào kết quả đầu ra tức là (eax = ebx?) mà lệnh nhảy sẽ quyết định việc show Bad boy hay Show good boy. Cũng từ đây ta kết luận crackme này có hai đoạn check riêng biệt, một đoạn liên quan đến Name và một đoạn liên quan đến Serial. Vì nếu chúng ta nhập Name mà trong Name nhập vào có số thì chúng ta sẽ nhận Nag luôn. Ok ta kiểm tra thử nhé :
 ![](http://i.imgur.com/h454wg1.png)
 - Nhấn Ok để xác nhận thông tin nhập vào và …: Ta nhấn Ok một lần nữa và lần này ta mới dừng lại tại BP mà ta đã thiết lập ở trên :
![](http://i.imgur.com/BidiZeU.png)

 - Nếu như ta bypass nột đoạn check trên thì mới vượt qua được Nag cuối cùng của Crackme này, tức là khúc này sẽ check serial, đúng thì sẽ hiện “Greate work…” mà sai thì tiếp tục hiện “No luck..”. Ok, giờ ta quay lại khúc mà ta break khi ta đặt BP tại hàm api **MessageBoxA**
 ![](http://i.imgur.com/GnfpaT9.png)

 - Quan sát ta thấy địa chỉ trở về là `0x004013C1`, follow theo địa chỉ này
 ![](http://i.imgur.com/PEFAloo.png)

  - Tại đây, chúng ta thấy rằng Olly chỉ cho ta biết đoạn routine này bắt đầu từ 0x0040137E và kết thúc tại 0x004013C1. Để ý một chút chúng ta cũng thấy rằng tại địa chỉ 0x004013AC có một dấu “>”. Kí hiệu này sẽ chỉ cho ta biết lệnh nhảy ở vị trí nào nhảy tới nó, để biết chi tiết ta chỉ việc chọn vị trí có dấu trên và quan sát cửa sổ Tip Window :
  ![](http://i.imgur.com/m5Aq7Lg.png)
 - Lại một lần nữa chúng ta để ý thấy rằng có một lệnh so sánh và một lệnh nhảy phụ thuộc vào kết quả so sánh, vậy ta đặt một BP tại lệnh nhảy tại địa chỉ `0x0040138B` :
 - Ok nhấn F9 để run chương trình và nhập thông tin như lúc trước ta nhập :
 ![](http://i.imgur.com/zVjJYC1.png)
  - Sau đó nhấn Ok, ta sẽ break tại chỗ ta vừa set BP :
  ![](http://i.imgur.com/dESHSBA.png)
 - Sau khi break, ta quan sát thấy rằng lần đầu tiên break lệnh nhảy này sẽ không nhảy. Lý do là vì nó sẽ kiểm tra dần dần từng kí tự của chuỗi Name ta nhập vào, nếu có chứa chữ số thì nó mới nhảy. Giờ ta nhấn F9 thêm 4 lần nữa lúc này ta quan sát thấy rằng nó kiểm tra đến kí tự thứ 5, vị trí này chính là số “4” cho nên lệnh nhảy sẽ được thực thi :

 ![](http://i.imgur.com/EnkJ3A0.png)

 -Quan sát các giá trị của các cờ ta thấy rằng cờ C được bật lên :
 ![](http://i.imgur.com/3tbe7Pv.png)

 -Chọn giá trị của cờ C và nhấn đúp để set về giá trị 0, quan sát sang cửa sổ CPU ta thấy lệnh nhảy không còn hiệu lực nữa :
 ![](http://i.imgur.com/QmskHa5.png)

 - Ta tiếp tục nhấn F9 và bypass tương tự cho hai số tiếp theo. Sau khi vượt qua đoạn check này chúng ta sẽ break tại đây :

 - Tiếp tục sử dụng tiểu xảo để active lệnh nhảy bằng cách nhấp đúp chuột tại cờ Z :

 ![](http://i.imgur.com/Re8ibsa.png)

 - Cuối cùng ta nhấn F9 thêm một lần nữa và lần này là những gì chúng ta mong đợi :
 ![](http://i.imgur.com/aZhQeWU.png)

 - Như các bạn thấy răng để đến được cái thông báo Greate work như trên chúng ta đã thực hiện bằng cách thay đổi các cờ, tuy nhiên các bạn biết rằng các cờ này thay đổi liên tục do đó ta không có cách nào để lưu lại những gì chúng ta làm được. Vì vây, để có thể ép chương trình của chúng ta chấp nhận bất kì username và serial nào mà chúng ta nhập vào thì ta buộc phải thay đổi code của chương trình!! Ta sẽ làm như thế nào?

###9. Patch bằng cách sửa code <a name="9"></a>

 - Đầu tiên ta xử lý cái lệnh nhảy ở chỗ check liên quan đến Name nhập vào để nó chấp nhận trong Name có số :
 ![](http://i.imgur.com/0Akr0Wd.png)

 - Hehe giờ ta sẽ không thay đổi cờ nữa mà edit lại code luôn, tại địa chỉ 0x0040138B chứa lệnh nhảy ta nhấn phím Space bar :
 - Thay bằng lệnh NOP :

 - Sau đó nhần Assemble ta có kết quả như sau :
 ![](http://i.imgur.com/HJVdycB.png)

  - Ok như vậy chúng ta đã loại bỏ được lệnh nhảy này, giờ ta nhấn F2 để bỏ việc thiết lập BP tại vị trí trên :
  - Tiếp theo ta đi tới chỗ lệnh nhảy tại chỗ kiểm tra Serial :
  ![](http://i.imgur.com/wpQSPfj.png)
  - Tại đây ta cũng không cần tác động cờ Z để active lệnh nhảy nữa. Ta nhấn Space Bar và thay bằng lệnh sau :
  ![](http://i.imgur.com/CGJmmm9.png)
  - Thay bằng jmp tức là ta ép cho nó luôn nhảy dù có đúng hay sai đi nữa, ok sau khi edit ta bỏ BP tại vị trí trên. Nhấn F9 để run và kiểm tra kết quả :
  ![](http://i.imgur.com/2V8UePw.png)
  - Chọn Ok và khà khà :
  ![](http://i.imgur.com/J6zf6y2.png)
  - Như các bạn thấy vừa rồi tôi đã hướng dẫn các bạn cách edit lệnh trực tiếp trong Olly, nhưng nếu chúng ta restart chương trình thì những thay đổi này sẽ không còn tác dụng có nghĩa là nó ko lưu lại những gì ta đã làm. Vậy để lưu các thay đổi vừa rồi trước khi đóng Olly bạn làm như sau :
  ![](http://i.imgur.com/XZgYM2r.png)

  - Nhấn chuột phải sau đó chọn **copy to executable > All modifications** , một cửa sổ bật ra :

  - Ta chọn Copy all để lưu lại toàn bộ những gì ta đã thay đổi :

  ![](http://i.imgur.com/GMYouv7.png)

  - Một cửa sổ khác bật ra như trên, tại đây chúng ta nhấn chuột phải và chọn : Save file
  ![](http://i.imgur.com/NdHzlqG.png)
  - Đặt cho file mà chúng ta Save dưới một tên mới, ví dụ Crackme_fixed.exe và chọn Save. Đóng Olly lại và test thử kết quả bằng cách run file mới :
  ![](http://i.imgur.com/c7kCPIt.png)
  - Nhập thông tin để đăng kí và Nhấn Ok.

  ![](http://i.imgur.com/es0ydg8.png)

 - Khà khà vậy là file mới mà ta vừa save hoạt động rất tốt!! Quá khỏe, giờ ta nhập loạn tùng phèo nó cũng nói là “Greate work …”

 

