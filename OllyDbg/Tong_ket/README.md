## báo cáo : tổng quan về ollyDBG.

> tài liệu tham khảo .
  [1. tut 10 - 27 ](http://bit.ly/2kh0KnB)

  [2. white hat -5 Revese Engineering](http://bit.ly/2kHqVSR)

> thực hiện : Nguyễn Văn Hiệp

> cập nhật lần cuối : 16/2/2017
>---------

### mục lục :

> [1. break points trong ollyDBG ](#1)

>  - [1.1 Common Breakpoint or BPX](#1.1)

>  - [1.2 Breakpoints on Memory (Memory Breakpoint)](#1.2)

>  - [1.3 Hardware Breakpoints](#1.3)

>  - [1.4 Conditional Breakpoints](#1.4)

>  - [1.5 Conditional Log Breakpoints](#1.5)

>  - [1.6 Message Breakpoints](#1.6)

> [2. khái niệm về pack và unpack](#2)

> [3. các phím tắt và công cụ liên quan](#3)

> [4. Fishing Serial](#4)

>  - [4.1. LESSON 13 HARDCODED 1](#4.1)

>  - [4.2. LESSON 13 HARDCODED 2](#4.2)

>  - [4.3 Detten‟s crackme](#4.3)


> [5. Detect OllyDbg – IsDebuggerPresent](#5)

> [6. Phân tích DaXXoR - Decryptme](#6)

> [7. Phân tích buggers3.exe](#7)

>----------------

### 1. break points trong ollyDBG <a name="1"></a>

#### 1.1 ommon Breakpoint or BPX <a name="1.1"></a>

 - Đây có thể nói là phương pháp đặt breakpoint phổ biến mà chúng ta đã làm việc ngay từ các phần trước của bộ tuts này, nó còn được gọi là BPX bởi vì nó có nhiều điểm tương đồng với lệnh BPX của SoftIce (ai đã từng dùng qua SoftIce thì chắc là biết còn tôi chỉ đụng SoftIce đúng một lần duy nhất ), thay vì phải viết dài dòng là BreakPoint thì để tránh rườm rà trong quá trình làm việc thông qua Command Line, tác giả đã rút gọn lại là BPX, tuy nhiên trong Olly chữ “X” thường được sử dụng trong việc đặt BP với các hàm API (liên quan tới version của Windows) cho nên tác giả của Olly đã rút gọn lại và thêm một command khác mà ta hay dùng đó là BP (vừa có thể đặt BP tại API và address). Thông thường, đối với một câu lệnh nào đó mà chúng ta muốn đặt BPX lên nó thì đơn giản nhất ta chỉ việc chọn dòng lệnh mà ta muốn và nhấn F2, nếu nhấn F2 một lần nữa tức là ta remove BP khỏi câu lệnh.

 - Giải thích như trên có thể khiến các bạn bị rối, tóm lại nếu muốn đặt BP tại một dòng lệnh bất kì các bạn chỉ cần nhớ là bấm F2 tại dòng lệnh đó.

 - để quản lý các break poin chúng ta đã đặt thì có 1 công cụ đó là chữ B ở góc trên 
 ![h1](http://i.imgur.com/xju7O6G.png)
 - tại đây chúng ta có các tùy chọn chức năng như sau:
<ul>
<li> **Remove** : loại bỏ breakpoint ra khỏi danh sách các điểm đặt BP mà cửa sổ này quản lý.</li>
<li> **Disable** : tạm thời disable bp của chúng ta mà không loại nó khỏi danh sách, khi cần ta lại active nó lên.</li>
<li> **Edit Condition** : khi chọn chức năng này là ta đang muốn chuyển đổi bp của chúng ta sang một dạng khác là conditional bp, kiểu đặt bp này sẽ được bàn tới ở dưới.</li>
<li> **Follow in Disassembler** : Với một danh sách dài các bp thì chúng ta khó có thể nhớ được nó liên quan tới đoạn code nào, tính năng này cho phép chúng ta tìm tới điểm mà chúng ta đặt bp tại cửa sổ code.</li>
<li> **Disable all **: tương tự như tính năng Disable tuy nhiên có khác là nó sẽ disable hết toàn bộ các bp có trong cửa sổ Breakpoints.</li>
<li> **Copy to clipboard** : sao chép thông tin về bp vào clipboard. Khi chọn tính năng này sẽ xuất hiện thêm một số chức năng con đi kèm
</ul>
 
 - chọn chức năng Copy to Clipboard -> Whole line, tính năng này sẽ copy nguyên dòng mà chúng ta chỉ định. Còn Whole table sẽ copy toàn bộ tất cả những danh sách bp hiện có trong cửa sổ Breakpoints.

 - vậy câu hỏi đặt ra là điều gì đã làm chương trình dừng lại tại breakpoin , chúng ta sẽ tìm hiểu bằng ví dụ sau: load olly vào và tìm đến địa chỉ `0x00401018` , 
 ![](http://i.imgur.com/P9nHGpP.png)

 - chọn như trên hình và quan sát tại cửa sổ dump

 - So sánh thông tin ở cửa sổ Dump với thông tin ở cửa sổ CPU ta thấy không có gì thay đổi cả, ở cửa sổ Dump các bạn thấy các bytes 0B C0 tương đương với câu lệnh or eax, eax tại cửa sổ CPU. Điều này khiến chúng ta không khỏi thắc mắc, nếu như không có sự thay đổi gì thì tại sao khi tôi run chương trình thì nó lại dừng lại tại điểm mà tôi đặt BP? Để kiểm chứng xem có sự thay đổi hay không, các bạn hãy restart lại Olly (Ctrl+F2) và chắc chắn rằng chúng ta vẫn đang đặt BP tại địa chỉ 0x00401018 . Sau khi Restart lại Olly, ta sẽ dừng lại tại EP của chương trình, tiến hành edit một đoạn code như sau tại EP
 ![](http://i.imgur.com/zooizBL.png)
 - Nhìn vào cửa sổ Dump ta sẽ biết được đây là những bytes gốc ban đầu, chưa bị thay đổi gì.Nhưng khi ta thử nhấn F7 để trace và kết quả là thanh ghi EAX đã có sự thay đổi rất thú vị :

 - Để ý cửa sổ Dump bạn thấy các bytes vẫn được giữ nguyên như ban đầu, nhưng ở thanh ghi EAX thì ta lại thấy giá trị đã khác là : 0x0174C0CC. Điều này được giải thích sơ bộ như sau: khi tôi đặt một BP tại địa chỉ 0x401018 thì Olly sẽ tiến hành thay thế byte đầu tiên mà ở đây là giá trị 0B bằng một giá trị khác là 0xCC. Nếu như bạn quy đổi byte CC này sang lệnh asm thì nó là int3, đây là một câu lệnh đặc biệt (nó còn được gọi là Trap to Debugger) nó sẽ gây ra một exception khi chúng ta cố gắng thực thi chương trình. Các bạn đọc thêm thông tin sau :

 `“So generally it is complex to set a breakpoint in an arbitrary place of the program. The debugger should save the current value of the memory location at the specified address, then write the code 0xCC there. Before exiting the debug interrupt, the debugger should return everything to its former place, and should modify IP saved in the stack so that it points to the beginning of the restored instruction. (Otherwise, it points to its middle.)”`

 - Hơi lằng nhắng một chút nhưng hi vọng các bạn cũng thấm vào đầu được ít nhiều , ngoài việc đặt BP thông qua việc chọn câu lệnh và nhấn F2 thì ta còn một cách đặt BP khác nữa như sau, tìm địa chỉ cần đặt BP và gõ lệnh tại Command Bar plugin

 - Giờ ta tìm hiểu một chút về cách đặt BP cho hàm API. Như ở trên tôi đã nói lệnh BPX thường được sử dụng cho việc đặt BP tại các hàm API của chương trình, thêm nữa nó lại phụ thuộc vào phiên bản Windows mà bạn đang sử dụng. Đối với những ai sử dụng Windows NT/2000/XP/2003 thì việc đặt BP tại một API function cụ thể rất đơn giản, chỉ việc gõ BP [tên của hàm] trong Command Bar

 - Bạn nhớ gõ đúng chữ hoa chữ thường trong tên hàm nhé!! Trên Windows 98 thì ngược lại, việc đặt BP tại 1 hàm API cụ thể không được hỗ trợ mà thay vào đó là việc đặt BP tới những nơi có lời gọi tới hàm API mà chúng ta cần.

 - ta đã thấy được sự khác biệt giữa BPX và BP, BPX không thiết lập một breakpoint tại một địa chỉ cụ thể đã được chỉ định như BP đã làm mà nó chỉ đặt BP tại những câu lệnh tham chiếu tới địa chỉ đó mà thôi. 

 - Ngoài việc hỗ trợ đặt BP thông qua phím F2 và thông qua command line, Olly còn hỗ trợ cho chúng ta việc đặt bp thông qua việc nhấn đúp chuột. Để thực hiện việc đặt bp tại một địa chỉ, bạn chỉ việc chọn cột Hex Dump tại cửa sổ CPU và nhấp đúp chuột lên đó.Nếu nhấp đúp chuột một lần nữa thì sẽ loại bỏ BP

#### 1.2 Breakpoints on Memory (Memory Breakpoint)<a name="1.2"></a>

 - Ở phần trên các bạn đã tìm hiểu về việc đặt breakpoint thông qua BP và BPX, tuy nhiên việc đặt bp này là đối với những câu lệnh hay những opcodes không bị thay đổi trong suốt quá trình thực thi chương trình. Còn ở phần này chúng ta sẽ tìm hiểu về việc đặt bp trên memory(nơi mà dữ liệu thường xuyên thay đổi). Tại một thời điểm Olly chỉ cho phép duy nhất một memory breakpoint. Olly hỗ trợ cho chúng ta hai kiểu đặt bp trên memory là : Breakpoint Memory on access và Breakpoint memory on write.Vậy hai cách đặt BP này có gì khác nhau :
 <ul>
 <li> **BreakPoint Memory on access**: Với kiểu đặt bp này lên một vùng nhớ sẽ cho phép chúng ta dừng sự thực thi của chương trình khi có bất kì một sự thực thi, đọc hay ghi lên vùng nhớ mà ta đã đặt bp.</li>
 <li> **BreakPoint Memory on write**: Khác một chút với kiểu đặt bp ở trên, kiểu này cho phép chúng ta dừng sự thực thi của chương trình khi có bất kì dữ liệu nào được ghi lên vùng nhớ mà ta đặt bp.</li>
 </ul>
 
 **lưu ý ** khác với breakpoin thông thường , khi ta đặt memory breakpoin , chúng ta phải đi đến cửa sổ dump và bôi đen các byte cần đặt BP để tiến hành đặt.

 - Olly chỉ cho phép tại một thời điểm có một memory bp duy nhất, nếu bạn đặt một bp khác thì bp đã đặt trước đó sẽ tự động bị loại bỏ.

 - việc đặt BP này sẽ gây ra một exceptions khiến cho dừng sự thực thi của chương trình.

 - Việc đặt bp Memory on Write cũng tương tự như những gì tôi đã làm ở trên, các bạn tự thực hành để rút ra kết luận nhé.

 - Ngoài việc đặt bp như trên, Olly còn cho phép chúng ta đặt Memory bp lên các section của file. Để thực hiện được điều này các bạn mở cửa sổ Window -> Memory map hoặc nhấn phím tắt là Alt + M :
![](http://i.imgur.com/5MLBV11.png)

 - Ta chọn đại lấy một section để làm ví dụ, ở đây để đơn giản tôi chọn section CODE và đặt một Memory bp lên nó. Chỉ việc chọn section, nhấn chuột phải và chọn như hình dưới đây để thiết lập một bp :

 - Mở rộng vấn để ra một chút, ở phần trước tôi đã thực hiện việc phân tích sơ bộ chương trình và các bạn cũng đã biết rằng khi chúng ta nhập sai thông tin về UserName và Serial thì ta sẽ nhận được một thông báo lỗi. Bây giờ tôi muốn đặt BP tại hàm MessageBoxA (vd: Bp MessageBoxA), nhưng vì một lý do nào đó chương trình sử dụng cơ chế anti-bp và phát hiện ra rằng có CC (tức int3) được sử dụng ngay lập tức nó sẽ terminate Olly của ta. Vậy không lẽ ta phải chịu vậy sao và không thể đặt bp tại API MessageBoxA. Xin thưa với các bạn, đến lúc này việc đặt Memory Bp mới có tác dụng và hiệu quả. Để tìm địa chỉ của hàm MessageBoxA tôi làm như sau trong Olly:
 ![](http://i.imgur.com/xSjCl4q.png)
 - Vậy là địa chỉ hàm MessageBoxA của tôi là ở 0x7E45058A (trên máy các bạn có thể khác), lúc này tôi chỉ việc qua cửa sổ CPU và nhấn Ctrl+G, gõ địa chỉ này vào hoặc thay vào đó tôi gõ trực tiếp tên hàm vào và nhấn Ok, Olly sẽ đưa tôi đến đúng nơi cần tìm . sau đó ta đặt BP bằng cách nhấn chuột phải và chọn Memory on access.

#### 1.3  Hardware Breakpoints<a name="1.3"></a>

- `Hardware breakpoint (available only when running Debugger under Windows ME, NT, 2000 or XP). 80x86-compatible processors allow you to set 4 hardware breakpoints. Unlike memory breakpoint, hardware breakpoints do not slow down the execution speed, but cover only up to 4 bytes. OllyDbg can use hardware breakpoints instead of INT3 when stepping or tracing through the code.`

 - đó là những gì chúng ta có được về hardware breakpoints khi seach trong help của olly

 - Điều đầu tiên ta thu thập được qua đoạn này là Hardware breakpoint (viết tắt HWBP) không thể sử dụng được nếu như bạn dùng Olly trên môi trường Windows 98. Điều thứ hai là chúng ta được phép thiết lập tới 4 HWBP, so với Memory BP là quá đã rồi vì như ta đã biết tại một thời điểm Olly chỉ cho phép có duy nhất một memory bp. Thêm vào đó HWBP không làm châm quá trình thực thi của chương trình. Điều cuối cùng ta nhận được là HWBP không sử dụng INT3, vậy thì nó sử dụng lệnh gì để dừng sự thực thi của chương trình?? Chúng ta tiếp tục tìm hiểu thêm vậy . Đi lòng vòng một hồi tôi cũng có thêm được một chút thông tin: HWBP được hỗ trợ trực tiếp bởi CPU, sử dụng một số thanh ghi đặc biệt hay còn được gọi là debug registers. Có bốn thanh ghi đó là : **DR0, DR1, DR2, DR3,** bốn thanh ghi này sẽ được sử dụng để lưu giữ những địa chỉ mà ta thiết lập HWBP. Điều kiện của mỗi break points để cho dừng sự thực thi của chương trình lại được lưu trong một thanh ghi đặc biệt khác là CPU register, đó là thanh ghi **DR7**. Khi bất kì một điều kiện nào thỏa mãn (TRUE) thì processor sẽ quăng một exception là INT1 (khà khà vậy là nó dùng INT1 nhé) và quyền điều khiển lúc này sẽ được trả về cho trình Debug của chúng ta. Có bốn khả năng để dừng sự thực thi của một chương trình :

 <ul>
 <li>1. Khi một câu lệnh được thực thi.</li>
 <li>2. Khi nội dung của memory có thay đổi (modified).</li>
 <li>3. Khi một ví trí memory được đọc ra hoặc được cập nhật(updated).</li>
 <li>4. Khi một input-output port được tham chiếu tới. (cái này tôi cũng chưa tìm hiểu).</li>
 </ul>

 - Việc đặt HWBP on Execution cũng gần tương tự như cách ta đặt BP thông thường tại một câu lệnh, chỉ có một điều hơi khác là HWBP không có thay đổi code do đó khiến cho việc phát hiện HWBP trở nên khó khăn hơn. Mặc dù vậy vẫn có những chương trình có khả năng sử dụng một số tricks để xóa HWBP, những kĩ thuật như vậy và cách thức chống lại nó chúng ta sẽ tìm hiểu trong các phần tiếp theo. Bây giờ quay trở lại vấn đề, ta muốn đặt một HWBP tại địa chỉ 0x00401013 thì làm như thế nào? Câu trả lời cực kì đơn giản, ta chọn địa chỉ đó nhấn chuột phải và chọn như sau :
![10.7](http://i.imgur.com/LyfH5Nv.png)
 - Hoặc một cách khác là thực hiện thông qua command line:

 - để kiểm soát các HWBP đã được đặt thì olly cho chúng ta 1 công cụ để kiểm soát . nhấn chuột phải và chọn hardware breakpoin ta nhận được 1 cửa sổ như sau:
![10.8](http://i.imgur.com/yCQdpaq.png)
 - chắc không cần nói nhiều về cửa sổ này nữa bởi vì nó khá dễ hiểu .

 - **NOTE** khác với BP thường , HWBP không làm thay đổi code . khi ta restart lại chương trình thì Memory breakpoin bị mất đi nhưng khi ta đặt HWBP thì sau khi restart nó vẫn còn ở đó.

 - Tiếp theo ta tìm hiểu tiếp về HWBP on Write và HWBP on Access. Hai kiểu HWBP này cho phép thiết lập với Byte, Word, Dword (tương ứng 1, 2 hoặc 4 bytes). Không khó để thực hiện hai kiểu hwbp này, đơn giản bạn chỉ việc qua cửa sổ Dump, sau đó đánh dấu vùng bytes tương ứng và đặt BP. 

 - điểm đặc biệt khi ta đặt HWBP memory là khi có sự ghi hay đọc dữ liệu từ một byte ta đã đăth HWBP thì chương trình sẽ break trước câu lệnh đó chứ không phải break ngay tại câu lệnh đó .

#### 1.4 Conditional Breakpoints <a name="1.4"></a>

 - Conditional Breakpoint là gì? Thử tra help của Olly xem có thông tin gì không, tôi có được như sau :
 
 `Conditional breakpoint (shortcut Shift+F2) is an ordinary INT3 breakpoint with associated expression. Each time Debugger encounters this breakpoint, it estimates expression and, if result is non-zero or expression is invalid, stops debugged program. Of course, the overhead caused by false conditional breakpoint is very high (mostly due to latencies of the operational system). On PII/450 under Windows NT OllyDbg processes up to 2500 false conditional breakpoints per second. An important case of conditional breakpoint is break on Windows message (like WM_PAINT). For this purpose you can use pseudovariable MSG together with proper interpretation of arguments. If window is active, consider message breakpoint described below.`

 - Về mặt khách quan thì nó cũng giống như bp thông thường, tức là nó cũng dùng lệnh Int3 để dừng sự thực thi của chương trình, thêm nữa Condtional bp cũng được quản lý thông qua cửa sổ Breakpoints(B). Tuy nhiên, điểm khác biệt nằm ở chỗ điều kiện dừng phải thỏa một điều kiện đã được thiết lập từ trước (Các biểu thức điều kiện như thế nào mời các bạn đọc thêm Help file, phần expression được tôi đánh dấu đỏ ở trên) . Điểm quan trọng khác của conditional bp là nó hay được sử dụng để “tóm” các Windows Message (Ví dụ như: WM_CLOSE, WM_PAINT v..v..). Window Message là gì, chắc tôi không cần giải thích vì nếu các bạn học về lập trình hay muốn nghiên cứu RE thì các bạn phải biết về nó 

 - để đặt Conditional Breakpoints ta chỉ cần nhấn chuột phải vào dòng lệnh muốn đặt và chọn như trên hình 
 ![10.9](http://i.imgur.com/AXg0RDm.png)
 - hay đơn giản hơn ta nhấn tổ hợp **shift + F2** để mở cửa sổ 

#### 1.5 Conditional Log Breakpoints <a name="1.5"></a>

```
Conditional logging breakpoint (Shift+F4) is a conditional breakpoint with the option
to log the value of some expression or arguments of known function each time the 
breakpoint is encountered or when condition is met. For example, you can set logging breakpoint 
to some window procedure and list all calls to this procedure, or only identifiers of 
received WM_COMMAND messages, or set it to the call to CreateFile and log names of the files 
opened for read-only access etc. Logging breakpoints are as fast as conditional, and of course it's 
much easier to look through several hundred messages in the log window than to press 
F9 several hundred times. You can select one of several predefined interpretations to your expression.
```
 - Bp này cũng là một dạng conditional bp tuy nhiên cao cấp hơn một chút. Nó có thêm tùy chọn cho phép ta lưu “vết” giá trị của biểu thức hoặc các tham sổ của function mỗi khi xảy ra bp hoặc khi thỏa mãn điều kiện mà ta đặt ra. Những thông tin này được lưu lại trong cửa sổ Log(L) của Olly. Thực hiện một ví dụ để minh họa, trước tiên chúng ta restart lại Olly cái đã. Sau đó nhấn Ctrl+G và tìm tới hàm MessageBoxA
![10.10](http://i.imgur.com/6azmmDR.png)
 - Hoặc đơn giản chỉ việc nhấn phím tắt Shift+F4, cửa sổ Conditional Log BP xuất hiện với rất nhiều tùy chọn :
 
  - nó đây !!
  ![10.11](http://i.imgur.com/LGHD9ij.png)
  - Trong trường hợp này của tôi, tôi không muốn dừng sự thực thi tại hàm MessageBoxA mà đơn giản tôi chỉ muốn ghi lại các giá trị mà tôi cần quan tâm, tôi đặt diều kiện như sau :
  ![](http://i.imgur.com/wg9RCgh.png)

  - Những gì tôi giải thích trên hình minh họa chắc các bạn cũng hiểu phần nào mục đích của việc đặt BP. Tôi xin nói chi tiết lại như sau, trong phần Expresstion tôi nhập vào là [esp] có nghĩa là tôi muốn đọc ra nội dung của thanh ghi Esp. Phần Pause program tôi chọn là Nerver, có nghĩa là tôi không muốn dừng sự thực thi của chương trình lại. Hai thành phần tiếp theo là Log value of expression và Log function arguments tôi chọn là Always, có nghĩa là tôi muốn ghi lại nội dung của biểu thức mà cụ thể ở đây là giá trị của thanh ghi esp, đồng thời tôi cũng muốn ghi lạ các tham số truyền vào cho function mà cụ thể ở đây là hàm MessageBoxA. Giải thích vậy các bạn đã hiểu chưa . Sau khi thiết lập như trên xong, nhấn Ok ta có được như sau.
  ![10.13](http://i.imgur.com/sWOfxz0.png)
  - các thông tin sẽ được lưu giữ tại cửa sổ log.

#### 1.6 Message Breakpoints<a name="1.6"></a>

 - Message Breakpoint là gì nhỉ? Sao lại lắm lọai BP đến thế . Khi gặp bất kì những vấn đề nào mới, trong đầu tôi luôn xuất hiện những câu hỏi liên quan đến vấn đề đó. Sau đó, tôi tìm cách tiếp cận thông tin để giải quyết cho những câu hỏi của tôi. Trước tiên, tôi chẳng biết Message Breakoint nó là cái gì, cho nên tôi tra help của Olly trước, hi vọng sẽ tìm ra chút thông tin nào đó về nó. Đọc trong help file của Olly thì chỉ nhận được một chút thông tin như sau :

 - *Message breakpoint is same as conditional logging except that OllyDbg automatically generates condition allowing to break on some message (like WM_PAINT) on the entry point to window procedure. You can set it in the Windows window*
 
 - Thông tin đầu tiên gợi mở cho tôi là thằng Message breakpoint này gần tương tự như Conditional Log BP ngoài trừ việc khác ở chỗ Olly hoàn toàn tự động tạo ra các điều kiện cho phép dừng lại tại các Message. Mà cái Message ở đây chính là các Windows Message. Tôi không phải là dân chuyên lập trình cho nên tôi tìm hiểu tiếp các thông tin khác trên Net về Windows Message.

 - *The messages in Windows are used to communicate most of the events, at least in the basic levels. If you want a window or control (which is a specialized window) does something, you must send a message to him. If another window wants you do something, then it sends a message to you. If an event occurs, such as when the user moves the mouse, presses the keyboard, etc… the system sends a message to the affected window. This window receives the message and acts suitably*

> - Message là một trong những phương tiện giao tiếp quan trọng nhất trong môi trường Windows. 

> - Lập trình trong Windows chủ yếu là đáp ứng lại những sự kiện.

> - Message có thể báo hiệu nhiều sự kiện gây ra bởi người sử dụng, hệ điều hành, hoặc chương trình khác.

> - Có hai lọai message: window message và thread message.

> **Window Message:** 

> - Tất cả các message đều được trữ trong một Message Queue(một nơi trong bộ nhớ). Những message này sau đó sẽ được luân chuyển giữa các ứng dụng.

> **Message Loop:**

> - Bạn có thể gọi những message bằng cách tạo ra một Message Queue

> - Message Loop là một vòng lặp kiểm tra những message trong Message Queue

> - Khi một message được nhận, Message Loop giải quyết nó bằng cách gọi Message Handler (một hàm được thiết kế để giúp Message Loop xử lý message)<hầu hết công việc lập trình của mình sẽ tập trung vào đây>

> - Message Loop sẽ kết thúc khi nhận được message WM_QUIT (lúc người dùng chọn File/Exit || click vào nút Close || bấm Alt+F4)

> - Khi bạn tạo cửa sổ (ứng dụng) Windows sẽ tạo cho bạn một Message Handler mặc định. Bạn sẽ vào đây để sửa chữa giúp ứng dụng phản hồi lại những sự kiện theo ý bạn muốn ->chương trình của bạn.

> - Tất cả những control chuẩn đều như thế. Lấy một button làm ví dụ: khi nó nhận WM_PAINT message nó sẽ vẽ button; khi bạn click chuột trái lên nó sẽ nhận WM_LBUTTONDOWN message và nó sẽ vẽ hình button bị nhấn xuống; khi buông chuột ra nó nhận WM_LBUTTONUP message và sẽ vẽ lại button bình thường.

> - Tên của window message thường có dạng WM_ và hàm để xử lý message đó thường có dạng On. Ví dụ hàm xử lý WM_SIZE message là OnSize.

> - Message thường có hai tham số lưu trữ thông tin về sự kiện(32 bit): lParam kiểu LONG và wParam kiểu WORD. Ví dụ: WM_MOUSEMOVE sẽ trữ tọa độ chuột trong một tham số còn tham số kia sẽ có cờ hiệu để ghi nhận trạng thái của phím ATL, Shift, CTRL và những nút trên con chuột.

> - Message có thể trả về một giá trị giúp bạn gửi dữ liệu ngược trở về chương trình gửi nó. Ví dụ: WM_CTLCOLOR message chờ bạn trả về một HBRUSH (khi dùng AppWizard để tạo nhanh ứng dụng bạn chú ý những hàm có dạng On... và nhận xét những kiểu trả về của nó, bạn sẽ hiểu hơn về cơ chế liên kết giữa các thành phần ứng dụng với nhau)

- Ok, qua những thông tin tổng hợp ở trên tôi chắc rằng tôi và các bạn cũng đã phần nào hiểu được ý nghĩa và mục đích của Windows Message. Bây giờ chúng ta sẽ cùng nhau làm một ví dụ, như các bạn đã đọc ở các phần trước, tôi đã hướng dẫn các bạn cách đặt BP tại các hàm APIs để dừng sự thực thi của chương trình cũng như tìm ra những điểm mấu chốt để patch chương trình. Lần này ta thực hành như sau : để tìm ra Serial của chương trình chúng ta muốn chương trình dừng sự thực thi khi chúng ta tiến hành nhập fake Username và Serial và nhấn nút OK, cụ thể hơn có nghĩa là sau khi ta nhấn nút OK thì chương trình sẽ dừng lại theo đúng mong muốn của ta.

- Nhưng trước khi thực hiện phương pháp Message BP ta ôn lại phương pháp đặt BP tại APIs cái đã. Lần này tôi không dùng hàm MessageBoxA nữa mà sẽ dùng hàm GetDlgItemTextA, hàm này có nhiệm vụ như sau :

```
The GetDlgItemText function retrieves the title or text associated with a control in a dialog box.
UINT GetDlgItemText
(
HWND 	hDlg, 			// handle of dialog box
int 	nIDDlgItem, 	// identifier of control
LPTSTR 	lpString, 		// address of buffer for text
int 	nMaxCount 		// maximum size of string
);
```
- Mục đích của tôi là, thông qua hàm này tôi tìm ra nơi lưu thông tin mà tôi nhập vào để dùng cho các quá trình xử lý tiếp theo trong chương trình. Vậy để đặt BP tại hàm GetDlgItemTextA tôi làm như sau, trước tiên tôi tìm nó cái đã :
![](http://i.imgur.com/TYjJrPz.png)
- Các bạn còn nhớ cách tìm hàm API trong cửa sổ Names mà tôi đã hướng dẫn ở các phần trước chứ, nếu không nhớ thì các bạn chỉ việc : tại cửa sổ Names gõ đúng tên hàm API cần tìm là Olly sẽ đưa chúng ta đến đúng hàm ta cần. Tôi có được như sau :

![12.1](http://i.imgur.com/xg1hGxw.png)

 - ta tiến hành đặt BP tại đây hoặc có thể dùng commend bar để đánh dấy BP . sau khi đặt xong ta chạy thử chương trình nhập name và serial vào ,sau khi nhấn ok ngay lập tức olly break tại vị trí ta đã đặt tại hàm GetDlgItemText

 - Đảo mắt qua cửa sổ Stack để xem ta thu được những thông tin gì. như các bạn thấy thông tin các tham số truyền vào cho hàm GetDlgItemTextA là hết sức rõ ràng, kết hợp với mô tả về hàm này ở trên các bạn sẽ thấy rằng tham số Buffer sẽ là nơi lưu đoạn text mà chúng ta nhập vào, nhưng lúc này chúng ta chưa biết là vùng Buffer này sẽ lưu Name hay Serial. Ta phỏng đoán theo thứ tự lần lượt từ trên xuống thì nó sẽ lấy UserName trước. Để xác định chính xác ta làm như sau, chuột phải tại chỗ Buffer và chọn Follow in Dump hoặc chuyển qua cửa sổ Dump, nhấn Ctrl + G và nhập vào địa chỉ của vùng Buffer là `0x0040218E `:
 ![](http://i.imgur.com/MNOSz8H.png)
 - Lúc này tại cửa sổ Dump các bạn nhận được kết quả là toàn byte 0x00. Đơn giản là vì ta đã thực thi hàm đâu mà lấy được đoạn text nhập vào.
 - Bây giờ ta cho thực thi hàm GetDlgItemTextA và quan sát kết quả thu được tại cửa sổ Dump :
 ![](http://i.imgur.com/54EX1pz.png)

 - đúng như ta phỏng đoán, chương trình sẽ lấy UserName nhập vào trước và lưu nó vào vùng Buffer có địa chỉ là 0x0040218E. Tiếp theo ta lại cho thực thi chương trình bằng cách nhấn F9 :

 - Olly một lần nữa lại Break tại hàm GetDlgItemTextA. Không cần nói chắc các bạn cũng có thể đoán ra ngay được là nó sẽ lấy Serial mà ta nhập vào.Nhưng lúc này Serial sẽ được lưu vào một vùng Buffer khác mà các bạn thấy ở trên, đó là `0x0040217E`

 - Như vậy là với việc đặt BP tại hàm GetDlgItemTextA tôi đã dừng lại tại nơi lấy những thông tin mà tôi nhập vào, bao gồm UserName và Serial, bước tiếp theo tôi sẽ trace dần dần từng đoạn code cho tới khi nào tôi có thể tìm ra một Serial hợp lệ. Nhưng có lẽ tạm thời ta nên dừng lại tại đây để chuyển sang phần thực hành với Message BP, xem có thể thu được những thông tin tương tự như những gì ta vừa mới làm với việc đặt BP tại hàm APIs hay không? Trước khi thực hành ta xóa hết các BP đã đặt, vào cửa sổ BreakPoint và xóa hết.

 - Sau khi xóa xong, ta restart lại Olly và cho thực thi chương trình , tiến hành nhập thông tin mà chương trình yêu cầu :
 ![](http://i.imgur.com/tmYVFiY.png)
  - Không giống như đặt BP tại các hàm API là ta có thể đặt BP tại đầu hàm thì để đặt Message BP chúng ta phải làm việc với cửa sổ Windows. Hiện tại chương trình của chúng ta đang thực thi, chúng ta chuyển qua cửa sổ Window bằng cách nhấn vào nút W :

 - Sau khi bạn nhấn vào nút W thì cửa sổ Windows sẽ hiện ra. Nếu như bạn thấy nó trống trơn không có gì cả thì nhấn chuột phải và chọn  **Actualize**. Kết quả ta có được như sau :
 ![](http://i.imgur.com/40TKH7U.png)

 - Như đã nói ở trên, mục đích của chúng ta là sau khi bấm Button Ok thì chương trình sẽ dừng lại. Vậy quan sát trong cửa sổ Windows, ở phần Class ta thấy có dòng Button, sau đó nhìn qua phần Title ta thấy được tên của Button là OK. Vậy đây chính là mục tiêu của chúng ta. Để đặt Message BP ta làm như sau, chuột phải tại nơi của Button OK và chọn **Message Breakpoint on ClassProc**. Cửa sổ cho phép ta thiết lập BP hiện ra :
 ![](http://i.imgur.com/tWhSgcX.png)
  - Ở phần Messages là một danh sách liệt kê những dạng Messages mà chúng ta có thể thiết lập BP

 - Như các bạn thấy, Olly hỗ trợ đủ loại Messages từ Text,Mouse, Keyboard v…v.. Song bên cạnh đó nó còn hỗ trợ cho ta một loạt danh sách các Messages thông dụng nhất. Quay trở lại ví dụ của chúng ta: ta mong muốn khi ta nhấn chuột vào nút OK thì chương trình sẽ dừng sự thực thi. Vậy ta phân tích một chút, lúc ta bấm chuột mà cụ thể ở đây là chuột trái lên nút OK thì chương trình sẽ gửi một thông điệp là WM_LBUTTONDOWN (L ở đây có nghĩa là Left, BUTTONDOWN có nghĩa là ta bấm chuột xuống). Lúc ta nhả chuột thì chương trình cũng sẽ gửi một thông điệp là WM_LBUTTONUP. Do vậy trong trường hợp cụ thể của ta, ta sẽ nhờ Olly bắt thông điệp WM_LBUTTONUP khi ta nhả chuột khỏi nút OK . Trong phần Messages ta cuộn chuột xuống và tìm thấy.

 ![](http://i.imgur.com/5Y4gdA0.png)

 - Sau khi cấu hình như trên ta nhấn OK :
![](http://i.imgur.com/xgk75bh.png)

 - Ta sẽ thấy chỗ ClsProc của hai Button Cancel và OK được hightlight. Bạn sẽ đặt câu hỏi là tại sao tôi đặt cho nút OK mà lại dính cả vào nút Cancel, đó là vì trong phần cấu hình BP ở trên bạn chọn là Break on any window, nếu bạn chọn là Break on all windows with same title thì sẽ có kết quả tương tự nhưng lúc đó điều kiện đặt BP sẽ khác, các bạn hãy tự mình khám phá thêm . Bây giờ sau khi đặt BP như trên, ta nhấn nút OK để kiểm tra việc đặt BP :

 - vậy là quá trình thực thi của chương trình đã bị dừng lại và quyền điều khiển lúc này là của Olly. Tuy nhiên,như các bạn thấy thông thường khi dừng lại tại các hàm API ta luôn muốn tìm cách trở về đoạn code chính của chương trình, để rồi từ đó lần ra các manh mối tiếp theo. Vậy trong trường hợp này ta làm cách nào để quay về? Rất đơn giản các bạn nhấn Alt + M để mở cửa sổ Memory sau đó đặt 1 breakpoin tại địa chỉ `401000`. và F9 thì chương trình sẽ break tại hàm chính .

 - đi tiếp 1 đoạn ta sẽ thấy đoạn code sau:
 ![](http://i.imgur.com/Hm3jWsG.png)

 - Để ý bạn sẽ thấy rằng chúng ta đang ở tại chỗ sắp sửa thực thi hàm API GetDlgItemTextA, mà hàm này sẽ lấy thông tin về UserName và Serial ta nhập vào để lưu vào vùng Buffer. Qua đây ta thấy rằng không cần sử dụng đến phương pháp đặt bp tại hàm API ta cũng có thể thông qua Messages BP để lần tới những điểm quan trọng

 - Tuy nhiên, giả sử trong một trường hợp nào đó mà chương trình không sử dụng hàm API để lấy text do ta nhập vào thì ta làm thế nào, lúc đó ta cần sử dụng đến Messages BP để tiếp cận mục tiêu. Ta sẽ thực hiện một demo nhỏ, trước tiên xóa Memory BP và Message BP đã đặt trước đó đi .Restart lại Olly và cho thực thi chương trình :
 ![](http://i.imgur.com/yXRXAGR.png)
 - Ở đây tôi chưa nhập thông tin gì vội, chuyển qua cửa số Windows và chọn Actualize

 - Tại lần minh họa này tôi thực hiện đặt một Message BP như sau.
 ![](http://i.imgur.com/Rg7zfuH.png)

 - Các bạn sẽ hỏi tại sao tôi chọn WM_KEYUP, đơn giản là vì khi tôi gõ một kí tự bất kì và nhả phím thì sẽ có một thông điệp WM_KEYUP sinh ra. Tôi muốn Olly bắt lấy thông điệp này và dừng sự thực thi của chương trình lại. Thông tin thêm về WM_KEYUP.
```
The WM_KEYUP message is posted to the window with the keyboard focus when a
 nonsystem key is released.
 A nonsystem key is a key that is pressed when the ALT key is not pressed,
  or a keyboard key that is pressed when a window has the keyboard focus.
WM_KEYUP

nVirtKey = 	(int) wParam;	 // virtual-key code
lKeyData = 	lParam; 			// key data
```

 - Sau khi đặt BP như trên, tôi tiến hành gõ thử một kí tự vào Textbox Name tuy nhiên khi tôi nhả phím thì chẳng thấy chương trình dừng lại gì cả . Đó là bởi vì chương trình mà chúng ta làm việc không xử lý các Messages liên quan tới WM_KEYUP, nhưng qua đây ta cũng biết được một hướng khác để tiếp cận mục tiêu.

 - Quay trở lại vấn đề chính, tôi muốn tìm hiểu xem thực sự Message BP là gì và hoạt động của nó. Tôi tiến hành đặt lại BP tại Message 202 WM_LBUTTONUP :
 ![](http://i.imgur.com/Vkpipne.png)

 - Chúng ta thấy rằng Message BP cũng được quản lý bởi cửa sổ BP, nhấn chuột phải vào BP này và chọn :
 ![](http://i.imgur.com/yXvxxlt.png)

 - vậy bản chất của Message BP thực ra là một Conditional Log BP, trong đó điều kiện để dừng sự thực thì của chương trình là [ESP+8]==WM_LBUTTONUP (tức là [ESP+8]==202). Lúc này ta để ý cửa sổ Stack. Giá trị tại [esp + 8] đúng là 202, để cho rõ ràng hơn bạn nhấp đúp chuột tại cột chứa giá trị của ESP sẽ có được như sau
 ![](http://i.imgur.com/5y7hfkg.png)

 - **“$+8”** ở đây chính là “ESP + 8”. Hehe qua đó tôi biết chắc một điều rằng, giá trị [esp + 8] sẽ chứa giá trị của các Messages. Vậy đề dò các giá trị tôi sửa lại BP như sau.

![](http://i.imgur.com/RlzqNi1.png)

 - Ý nghĩa của những thiết lập trong hình trên tôi không cần phải giải thích lại nữa . Sửa lại BP xong tôi cho thực thi chương trình và nhập thông tin vào. Tiếp theo bấm OK và chuyển qua cửa sổ Log để quan sát các giá trị mà tôi thu được
 ![](http://i.imgur.com/DokDKBk.png)

 - Khà khà nhiều quá trời, để ý các bạn thấy là chương trình này xử lý hai Message là WM_LBUTTONDOWN(201) và WM_LBUTTONUP(202), đồng thời ta cũng thấy là nó không hề xử lý các Messages như WM_KEYUP hay WM_KEYDOWN

 - Ngoài ra để kiểm soát toàn bộ các Message cho tất cả các chương trình chúng ta có thể đặt một BP conditional log tại các hàm APIs chuyên kiểm soát các Messages. Hai hàm API đó là TranslateMessage và DefWindowProc.

### 2. khái niệm về pack và unpack<a name="2"></a>

 - Packer là một kiểu chương trình nén hoặc che dấu file thực thi (executable file). Các chương trình này ra đời bắt nguồn từ mục đích giảm kích thước của file, làm cho việc tải file nhanh hơn.

 - Rất nhiều các coder hay các hãng phần mềm sử dụng Packer nhằm mục đích khiến cracker/reverser khó khăn và tốn thời gian hơn trong việc bẻ khỏa hoặc đảo ngược phần mềm của họ. Đối với những kẻ chuyên phát tán các phần mềm độc hại (malicious software) thì Packer còn có tác dụng giúp kéo dài thời gian tồn tại của phần mềm càng lâu bị phát hiện càng tốt.

 - Cụ thể khi sử dụng, Packer nén, mã hóa, lưu hoặc dấu code gốc của chương trình, tự động bổ sung một hoặc nhiều section, sau đó sẽ thêm đoạn code Unpacking Stub và chuyển hướng Entry Point (EP) tới vùng code này. Bình thường một file không đóng gói (Nonpacked) sẽ được tải bởi OS. Với file đã đóng gói thì Unpacking Stub sẽ được tải bởi OS, sau đó chương trình gốc sẽ tải Unpacking Stub. Lúc này code EP của file thực thi sẽ trỏ tới Unpacking Stub thay vì trỏ vào mã gốc. Để hiểu được các phần trên thì các bạn cần hiểu cấu trúc PE file và cách thực thi của nó

 - Nói là xử lý nhưng chúng ta phải làm thế nào nhỉ? Người có kiến thức và kinh nghiệm thì bảo : “Hãy kiểm tra chương trình trước xem có bị pack bởi packer nào không? Nếu bị pack thì giải quyết packer trước rồi tính tiếp. Còn nếu không bị pack thì quá khỏe, chạy thử chương trình xem nó hoạt động ra sao và tìm kiếm thông tin. Sau khi có được những thông tin quan trọng thì load chương trình vào Olly, tìm các cách để tiếp cận, đặt BP ở những điểm mấu chốt, sau đó trace code, comment những chỗ quan trọng, nếu fish được serial thì tốt, còn không thì tìm ra thuật toán và code keygen v..v..Bạn hãy tự mình thực hành đi đã, nếu bị bí chỗ nào hãy post lên để hỏi!”

 - Những người biết thì thưa thớt nhưng lại thích khoe khoang cũng phán đại :”Thì load vào Olly, tìm cái chuỗi liên quan đến Nag ấy, rồi đặt BP chứ còn làm gì nữa! Không làm được thì show code lên đây tôi giúp cho v..v..”

 - Riêng cá nhân của tôi thì thấy rằng: “Phải tự mình đúc kết các kinh nghiệm trước khi lâm trận cái đã, khi bạn chưa biết gì mà đã vội nhảy vào trận chiến thì chẳng khác nào lấy trứng chọi đá. Vậy kinh nghiệm ở đâu ra? Kinh nghiệm có được khi bạn đọc những bài viết của người khác, có được khi bạn thực hành với những trường hợp tương tự nhưng bạn thử nghiệm những hướng tiếp cận khác ,kinh nghiệm có được khi bạn tham gia thảo luận một chủ đề kĩ thuật v..v.. Để rồi từ đó bạn rút tỉa dần dần và tích lũy lại thành kinh nghiệm của riêng mình.Rồi sẽ đến một lúc nào đó, lại có người muốn ta chia sẻ kinh nghiệm của mình. Không ai có đủ thời gian và kiên nhẫn để chỉ dạy từng bước cho bạn, bạn phải tự mình tìm tòi và khám phá, khi nào bạn cảm thấy thực sự cần đến sự giúp đỡ tôi nghĩ lúc đó sẽ có người sẵn sàng giúp bạn.

 - Quay trở lại phần chính của bài viết này là giải quyết crackme CrueHead để tỉm ra môt valid serial. Một hướng tiếp cận cơ bạn sẽ như tôi trình bày bên dưới đây, đương nhiên không nằm ngoài khả năng có những cách tiếp cận khác, điều đó nằm ở sự khám phá của các bạn

 **Kiểm tra xem chương trình có bị pack hay không**

 - Pack file nghĩa là như thế nào? Tại sao phải kiểm tra xem có bị pack? Hiểu một cách đơn giản thì pack file là nén file thực thi (PE file : .dll, .exe, .ocx, v..v..) để làm giảm kích thước của file, việc nèn này ngoài việc nén code, data của chương trình thì trình packer còn thêm cả đoạn decompress stub vào PE file để làm nhiệm vụ unpack chương trình trong memory. Việc nén này không nên hiểu như ta dùng Winrar/Winzip để nén file, vì Winrar/Winzip sau khi nén file xong ta không thể thực thi file đó được mà ta phải làm một bước là extract file, sau đó mới run file

 - Khi một file không bị pack thì lúc ta load chương trình vào Olly ta sẽ dừng lại tại EP của chương trình (hay còn gọi là OEP gốc). Còn nếu chương trình đã bị pack, khi ta load vào Olly ta sẽ dừng lại tại EP của packer chứ không phải là EP của chương trình. Do đó nhiệm vụ của chúng ta là phải unpack chương trình trước đã (tức là ta đi tìm lại OEP gốc), rồi mới thực hiện các hướng tiếp cận khác. Đó chính là lý do tại sao ta phải kiểm tra chương trình. Vậy ta kiểm tra như thế nào? Tôi thường sử dụng một số chương trình sau để check :
### 3. các phím tắt và công cụ liên quan <a name="3"></a>
 -a) PeiD v0.94/v0.95
 ![](http://i.imgur.com/hngUYP7.png)
 - b) ExeInfo PE
 ![](http://i.imgur.com/ReflBlu.png)
 - c) RDG Packer Detector:
 ![](http://i.imgur.com/o6aLAbB.png)
 - d) PE Detective
 ![](http://i.imgur.com/uCCfvxm.png)

 - Kết quả như các bạn đã thấy, sau khi sử dụng một loạt các chương trình PE detector ta nhận được kết quả là :

<ul>
<li>Chương trình không bị pack bởi bất kì packer nào.</li>
<li>Chương trình có thể được code bằng Masm32 hoặc Tasm32</li>
</ul>

 - Việc chương trình không bị pack cũng đồng nghĩa với việc ta không cần phải unpack chương trình nữa, vậy là nhẹ được một bước.Ta chuyển qua bước kế tiếp

 **Chạy thử chương trình để tìm kiếm mục tiêu cần tiếp cận**

 - Việc chạy thử chương trình nhằm mục đích giúp cho ta có một cái nhìn tổng quan về hoạt động của chương trình, biết được nhưng mục tiêu mà ta cần giải quyết. Song song với đó ta sẽ tìm kiếm các cách thức để tiếp cận mục tiêu. Ok giờ tôi chạy thử chương trình xem thế nào đã. Sau khi run tôi thấy nó không show nag gì cả. Nhìn sơ qua cũng chưa biết là mục tiêu tiếp cận ở đâu. Nếu các bạn để ý khi sử dụng các chương trình thì chức năng Register thường được đặt ở Menu Help. Do đó tôi nhấn chuột thử vào menu Help xem thế nào, blah blah..tôi thấy có phần Register, nhấn vào đó tôi nhận được
 ![](http://i.imgur.com/pKaVnCv.png)

 - Chà, tiếp theo làm gì nữa đây? Chúng ta nhập đại Name và Serial vào và hi vọng là nó đúng . Sau khi nhập và nhấn OK tôi nhận được nguyên cái Nag!

 - Ok, thông qua việc thực thi chương trình và nhập thông tin đăng kí như trên ta rút ra được một số mục tiêu tiếp cận như sau

 <ul>
 <li>Tiếp cận thông qua việc tìm kiếm chuỗi `“No luck there, mate!”`
 <li>Tiếp cận thông qua hàm **MessageBoxA**</li>
 <li>Tiếp cận thông qua việc nhận Name và Serial (thông qua **GetDlgItemTextA**)</li>
 <li>v...v...</li>
 </ul>

 - Như vậy sơ sơ ta cũng đã có 3 hướng tiếp cận rồi, việc chọn cách nào là tùy ở bạn. Bạn cũng có thể tự tìm ra một hướng khác với những hướng đã liệt kê ở trên. Ở đây tôi chọn cách thứ 3 là dụng hàm **GetDlgItemTextA** để tiếp cận và giải quyết crackme này!

 - **Dùng Olly + Brain để giải quyết bài toán**

  - Sau khi phân tích chúng ta đã thu lượm được những thông tin cần thiết cho việc giải quyết bài toán. Ở bước 3 này chúng ta sẽ nhờ đến Ollydbg + Brain để giải quyết bài toán hóc búa này . Tại sao lại là Olly và Brain nhi? Vì đơn giản Ollydbg chỉ là một công cụ phục vụ cho mục đích của chúng ta, việc làm chủ và sử dụng thành thạo nó là kĩ năng tối thiểu và cần thiết khi chúng ta muốn debug một chương trình. Còn việc phải làm ra sao, tìm các tuyệt chiêu thế nào, phương hướng tiếp cận làm sao nhanh nhất có thể v..v.. lại nằm ở trí thông minh và sức sáng tạo của chúng ta. Quay trở lại bài toán của chúng ta, load Crackme vào trong Olly đã :
   - Như đã nói ở trên, hướng tiếp cận của tôi lúc này là đi theo hàm GetDlgItemTextA. Thông tin về hàm này như sau :
  `The GetDlgItemText function retrieves the title or text associated with a control in a dialog box.
UINT GetDlgItemText(
HWND hDlg, // handle of dialog box
int nIDDlgItem, // identifier of control LPTSTR lpString, // address of buffer for text
int nMaxCount // maximum size of string
);
Return Values
If the function succeeds, the return value specifies the number of characters copied to the buffer, not including the terminating null character.
If the function fails, the return value is zero.`

 - Trờ lại Olly, tôi tiến hành đặt BP tại hàm **GetDlgItemTextA** .Chuyển tới cửa sổ Breakpoints ta thấy có hai BP được thiết lập, vậy ta phỏng đoán hai hàm này tương ứng với hai lần Get Name và Get Serial

 - Tiếp theo ta nhấn F9 để thực thi chương trình, nhập Name và Serial vào sau đó nhấn Ok và hi vọng Olly sẽ break tại nơi ta vừa thiết lập BP.

 ![](http://i.imgur.com/zCqTJh5.png)

- Như các bạn thấy trên hình, ta đang dừng lại tại lời gọi tới hàm GetDlgItemTextA đầu tiên
![](http://i.imgur.com/ntcq3DM.png)
- Không cần giải thích chắc các bạn cũng hiểu các thông tin này, chúng tương ứng với các tham số truyền vào cho hàm trước khi hàm được thực hiện. Ở đây chúng ta quan tâm tới vùng Buffer, vì nếu hàm thành công thì đoạn text nhập vào sẽ được lưu tại vùng Buffer này. Ta chọn Buffer, chuột phải và chọn Follow in Dump .

 - Bây giờ ta nhấn F8 để trace và thực hiện hàm GetDlgItemTextA, sau đó quan sát kết quả tại vùng Buffer :

 - Như vậy là ta đã có được chuỗi Name rồi . Để ý một chút bên cửa sổ Register ta sẽ thấy thanh ghi eax lưu độ dài của chuỗi Name (eax=00000007). Sau khi có được chuỗi Name như trên, tiếp tục nhấn F9 để thực thi chương trình, ta sẽ Break tại lời gọi tới hàm GetDlgItemTextA tiếp theo

 - Sau khi thu được hai chuỗi Name và Serial, ta tiếp tục nhấn F8 để trace code.Trace qua mấy đoạn code ta tới đây
```
00401284 	|> /5F 	pop 	edi
00401285 	|. |5E 	pop 	esi
00401286 	|. |5B 	pop 	ebx
00401287 	|. |C9 	leave
00401288 	|. |C2 	1000 	retn 10
```
 - Tiếp tục trace và thực hiện lệnh Retn 10, ta bị return tới vùng code không phải là code chính của crackme :

 - Nếu các bạn tiếp tục trace tiếp thì sẽ vẫn luẩn quẩn trong đám code này, vậy để cho nhanh chóng về vùng code chính của chương trình ta chọn:
![](http://i.imgur.com/TEN0meG.png)
![](http://i.imgur.com/0kcJY9C.png)

 -Tại đây ta dừng lại một chút và suy nghĩ đã, quan sát đoạn code. 
```
00401223 . 83F8 00 			cmp 	eax, 0
00401226 .^ 74 BE 			je 		short 004011E6
00401228 . 68 8E214000	 	push 	0040218E ; 	ASCII "manowar"
0040122D . E8 4C010000 		call 	0040137E (1)
00401232 . 50 push eax
00401233 . 68 7E214000 		push 	0040217E ;	 ASCII "1234567890"
00401238 . E8 9B010000 		call 	004013D8 (2)
0040123D . 83C4 04 			add 	esp, 4
00401240 . 58				pop 	eax
00401241 . 3BC3 			cmp 	eax, ebx (3)
00401243 . 74 07 			je 		short 0040124C
00401245 . E8 18010000 		call 	00401362
0040124A .^ EB 9A 			jmp 	short 004011E6
0040124C > E8 FC000000 		calls 	0040134D
00401251 .^ EB 93 			jmp 	short 004011E6
```

 - Đầu tiên, để ý đến lệnh `call 	0040137E` (1) trước nó là lệnh push chuỗi Name vào Stack. Vậy ta đoán khả năng lệnh call này sẽ dùng chuỗi Name để tính toán gì đó với chuỗi Name. Kết quả tính toán này sẽ được lưu vào thanh ghi eax vì ta thấy thanh ghi eax sau đó được lưu vào Stack và được sử dụng lại trong quá trinh so sánh. Tiếp theo là lệnh `call 	004013D8` (2),trước nó là lệnh push chuỗi Serial vào Stack, khả năng lệnh call này cũng tính toán gì đó với Serial , kết quả sau đó chắc là được lưu vào thanh ghi ebx. Vì tay thấy rằng sau đó thanh ghi eax được đem so sánh với thanh ghi ebx. Phụ thuộc vào kết quả so sánh này sẽ là một lệnh nhẩy đưa ta đến Good/Bad boy

 - Quá trình suy nghĩ và phân tích sơ bộ đã xong, giờ ta trace vào từng đoạn code để hiểu thêm xem về cơ chế tính toán ra sao. Trước tiên ta làm việc với lệnh `call 0040137E `(1) 
![](http://i.imgur.com/doADuA2.png)

 - Tôi phân tích từng phần một để các bạn dễ hiểu, đầu tiên là khúc tôi khoanh màu vàng. Nó có nhiệm vụ như sau :

 <ul>
 <li>1. Đọc chuỗi Name từ Buffer và lưu vào thanh ghi esi</li>
 <li>2. Kiểm tra từng kí tự trong chuỗi Name xem có nằm trong khoảng từ ‘A’ – ‘Z’ không?</li>
 <li>3. Nếu có kí tự nào có mã Ascii < 0x41 (‘A) thì sẽ show Nag (tức là Name không chứa chữ số v..v.)</li>
 <li>4. Nếu kí tự nào là chữ hoa thì thôi, nếu không là chữ hoa thực hiện convert sang chữ hoa (`call 004013D2`).</li>
 <li>5. Kết quả cuối cùng được lưu lại vào vùng Buffer</li>
 </ul>

 - Tiếp theo ta sẽ trace into vào lệnh call tại :` 0040139D |. E8 20000000 call 004013C2`, mục đích để tìm hiểu xem lệnh call này sẽ tính toán gì tiếp theo với chuỗi buffer.

 - Đoạn code này làm nhiệm vụ cộng dồn toàn bộ các kí tự trong chuỗi Name lại vào lưu vào thanh ghi edi, kết quả cuối cùng của thanh ghi edi đối với chuỗi Name mà tôi nhập vào là
![](http://i.imgur.com/4szKQkX.png)

 - Sau khi có được kết quả tại thanh ghi edi ta thực hiện lệnh 004013D1 \> \C3 retn để trở về.

 - Tại đoạn code này, giá trị của edi sau khi tính toán được ở trên được đem đi xor với một giá trị mặc định của chương trình là `0x5678`. Kết quả được bao nhiêu sẽ lưu lại vào thanh ghi eax. Sau đó quay về main code của chương trình.

 - Vậy tổng kết lại ta có được quá trình tính toán liên quan tới chuỗi Name như sau:

 <ul>
 <li>1) Chuỗi Name nhập vào phải là các chữ cái trong khoảng ‘A’-‘Z’, ‘a’-‘z’.</li>
 <li>2) Toàn bộ chuỗi sau đó sẽ được convert thành chữ in hoa.</li>
 <li>3) Sau khi được convert, đem các kí tự trong chuỗi cộng dồn lại ở dạng hexa, và lưu vào edi.</li>
 <li>4) Thanh ghi edi tiếp tục được xor với giá trị mặc định là 0x5678.</li>
 <li>5) Cuối cùng được kết quả bao nhiêu sẽ lưu vào eax</li>
 </ul>
 
  - Sau khi trở về main code của crackme ta ở đây :
  ![](http://i.imgur.com/9lf2Joz.png)

  - Nhìn vào hình trên ta thấy, thanh ghi eax lưu kết quả của quá trình tính toán liên quan đến chuỗi Name sẽ được cất tạm vào Stack. Tiếp theo chương trình sẽ đẩy chuỗi Serial lên Stack và tính toán gì đó với chuỗi này. Việc tiếp theo ta cần làm là tìm hiểu xem nó tính toán gì tại : `call 	004013D8` (2). Trace into vào lệnh call này ta tới đây :
  ![](http://i.imgur.com/zRIi3Lb.png)

  - Ngay đầu tiên ta đã thấy thanh ghi eax bị clear, chính vì thế các bạn thấy tác giả đã lưu lại thanh ghi eax trước. Tổng thể toàn bộ đoạn code trên làm nhiệm vụ : chuyển chuỗi Serial về dạng hexa. Trong ví dụ của tôi nhập vào là `1234567890`, qua đoạn code trên nó sẽ được convert về thành giá trị là `0x499602D2` và lưu vào thanh ghi edi.

  - Giá trị tại thanh ghi edi sau đó lại được đem xor với một giá trị mặc định khác là` 0x1234` (hehe ở trên thì là` 0x5678`). Kết quả được bao nhiêu sẽ được lưu lại tại thanh ghi ebx. Sau đó trở về main code để tới quá trình so sánh tại :
```
00401241 . 	3BC3 	cmp 	eax, ebx (3)
00401243 . 	74 07 	je 		short 0040124C
```
 - Vậy là ta đã tìm hiểu xong phần tính toán liên quan đến chuỗi Name và Serial. Bây giờ ta cần phải suy nghĩ và lập luận để tìm ra real serial cho chuỗi Name của chúng ta. Như các bạn thấy, chuỗi Name của ta nhập vào được chuyển sang chữ hoa, sau đó cộng dồn, cuối cùng đem xor với `0x5678` để cho ra kết quả và lưu vào eax. Trong trường hợp của tôi giá trị sau khi tính toán là `0x0000546D`, chuyển giá trị này sang dạng decimal tôi có `21613`. Tiếp theo tôi thấy rằng chuỗi Serial mà tôi nhập vào cũng được convert thành dạng hexa (Giả sử nếu tôi nhập vào là `21613`, thì tức là chuyển giá trị `21613` về dạng hexa là` 0x546D`). Sau đó giá trị hexa này được đem đi xor với giá trị mặc định là `0x1234`. Mà tôi thì biết rằng lệnh XOR có ý nghĩa như sau:

 -  *Lệnh XOR có thể được dùng để đảo các bit xác định của toán hạng đích trong khi vẫn giữ nguyên những bit còn lại. Bit 1 của mặt nạ làm đảo bit tương ứng còn bit 0 giữ nguyên bit tương ứng của toán hạng đích.*

 - Vậy từ đó ta kết luận rằng Serial của chúng ta sẽ là giá trị tính toán được của chuỗi Name và đem xor với `0x1234`. Trong trường hợp cụ thể của tôi thì real serial sẽ là : `0x546D xor 0x1234 = 0x4659` (chuyển sang decimal là : `18009`). Thử kiểm chứng lại xem có đúng không nhé, tôi nhấn F9 để run chương trình. Nhập Name và Serial như sau.
 ![](http://i.imgur.com/kgHBTCp.png)

 - Nhấn Ok và trace tới đoạn code
 ![](http://i.imgur.com/eksdadO.png)

 - Ta dừng lại tại đoạn so sánh, để ý cửa sổ Tip Window ta sẽ thấy. Khà khà quá chuẩn rồi, nhấn F9 để thực thi chương trình các bạn sẽ nhận được good boy

### 4.  Fishing Serial <a name="4"></a>

 - Chà Fishing Serial tức là gì nhỉ? Nghe như kiểu chúng ta đang đi câu cá, giữa một hồ cá rộng và sâu, làm sao ta câu được một con cá ưng ý. trong ngữ cảnh của Cracking thì ý nghĩa cũng gần như vậy. Fishing Serial ở đây có nghĩa là chúng ta đi câu Serial, mà phải là valid Serial nhé chứ câu lung tung là mệt và dễ stress. Đối với những bạn mới vào nghề thì việc tìm được một valid Serial luôn mang lại một cảm giác lâng lâng khó tả như tìm được một “kho báu” giữa lòng đại dương. 

#### 4.1 LESSON 13 HARDCODED 1 <a name="4.1"></a>

 - Sau khi load vào Olly, ta dừng lại tại EP của crackme như các bạn thấy . Dòm qua cái code thấy quá trong sáng, các bạn sẽ bắt gặp cái thông báo Error :
```
00401078 |. 68 35204000 push 00402035 ; |Text = "Mal Muy MAL"
```

 - và một cái thông báo khác, tôi đoán chắc là thông báo Serial hợp lệ :
```
0040108B|. 68 28204000 push 00402028 ;|Text = "Muy BIEN",A1,"",A1,"",A1,"",A1,"
```
 - Vậy Serial này ở đâu? Vì nó là dạng HardedCode, tức là người code đã include nó vào trong chương trình rồi, ta thử mò bằng cách tìm toàn bộ các String xem thế nào. Chuột phải và chọn như sau :

![img1](http://i.imgur.com/tVJJmGL.png)
 - và ta có được kết quả như trên hình .
 
 - Ồ, kết quả có được cũng không nhiều, dòng thứ 3 thì ta đã biết được nó là thông báo lỗi rồi, còn dòng thứ hai là gì nhỉ, thử đưa lên anh google nhờ anh ấy dịch hộ xem nó có nghĩa gì không

 - vậy có thể nghi ngờ nó là valid Serial. Tuy nhiên đây mới chỉ là nghi ngờ thôi, tôi không khuyến cáo các bạn áp dụng cách này để tìm Serial đơn giản vì lý do sau :

 *Do đây là crackme đơn giản cho nên tác giả đã code hết sức gọn nhẹ để chúng ta thực hành, cho nên kết quả trả về cho việc tìm kiềm chỉ là hai dòng text duy nhất. Nhưng với các target phức tạp khác, kết quả trả về có thể lên đến hàng trăm hoặc hàng nghìn dòng, việc dò và kiểm thử xem có đúng hay không là điều khó thực hiện và mất thời gian.*

 - Giờ chúng ta đã có điểm nghi ngờ là dòng text thứ hai, nếu nhanh gọn các bạn có thể chạy crackme và nhập vào để kiểm tra tính hợp lệ của nó. Tuy nhiên để chắc chắn hơn chúng ta cần chứng minh đúng là nó chứ không phải dựa vào phán đoán nhất thời. Ta tiến hành tìm kiếm các hàm API mà crackme này sử dụng :

 - kết quả ta có được như sau. 

 ![](http://i.imgur.com/wYPoZIn.png)

 - ta quan tâm nhất tới hàm `user32.GetDlgItemTextA`. Đặt BP tại hàm này, sau khi đặt BP xong ta run chương trình và tiến hành nhập Serial :

 - Sau khi nhập xong các bạn nhấn Verify, ngay lập tức Olly sẽ break 

 - Ta đang dừng lại tại` GetDlgItemTextA`, dòm qua cửa sổ Stack ta có thông tin như sau :
 ![](http://i.imgur.com/cIPtWsw.png)

 - Ở đây ta cần quan tâm tới Buffer, đây sẽ là nơi nhận lưu Serial mà chúng ta đã nhập vào. Chuột phải vào Buffer và chọn Follow in Dump , Do hàm API của chúng ta chưa được thực hiện nên vùng buffer hiện thời vẫn còn trống :

 - Giờ chúng ta cho thực thi hàm `API GetDlgItemTextA`

 - Olly sẽ dừng lại tại lệnh RET, lúc này quan sát vùng Buffer ta có được kết quả như sau :
 ![](http://i.imgur.com/FNzLQ3D.png)

 - Không cần giải thích chắc các bạn cũng biết được đó là gì rồi! Giờ nhấn F8 để trace qua lệnh Ret để trở về code chính của chương trình 

 - Ok, ta dừng lại quan sát và phân tích một chút. Để ý các bạn sẽ thấy có lệnh so sánh và lệnh nhảy, phụ thuộc vào kết quả so sánh mà lệnh nhảy sẽ đưa chúng ta đến một trong hai thông báo :

1. Nếu sai chúng ta sẽ được nhận thông báo "Mal Muy MAL" ( «bad, very bad» - approx. Ed.)
2. Nếu đùng chúng ta sẽ nhận thông báo "Muy BIEN" ( «very good »- Prim.per.).

- Rõ ràng trong trường hợp của chúng ta chúng ta muốn nhận được thông báo “Very good”. Vậy ta trace code để xem điều gì sẽ xảy ra tiếp theo. Đầu tiên ta sẽ thấy chương trình chuyển vào thanh ghi edx một hardcoded string là FIACA :

```
00401061 |. BA 08304000 mov edx, 00403008 ; ASCII "FIACA"
```
 - Tiếp theo tại `0x00401066`, ta thấy thanh ghi ebx sẽ nhận được giá trị từ vùng nhớ [403010], mà theo phân tích ở trên thì đây chính là vùng Buffer nhận vào chuỗi Serial của chúng ta .Trace qua lệnh này và quan sát thanh ghi EBX

 - Sau khi trace qua lệnh `00401066 |. 8B1D 10304000 mov ebx, dword ptr [403010],` ta sẽ đến lệnh CMP. Ta thấy nội dung của thanh ghi ebx sẽ được đem đi so sánh với vùng chứa chuỗi hardcoded

 - Theo như hình trên thì các bạn có thể thấy rằng, không phải toàn bộ chuỗi Hardcoded sẽ được đem đi so sánh với toàn bộ chuỗi serial mà ta nhập vào,mà ở đây chương trình chỉ lấy hai dword (tức là 4 chữ cái đầu tiên) đem so sánh với nhau. Nếu giống nhau thì quá tốt, còn không thì .Giờ tôi trace qua lệnh CMP :
 ![](http://i.imgur.com/SWTKxbc.png)

 - Đương nhiên là lệnh nhảy sẽ không thực hiện ví lý do Serial của tôi nhập vào không trùng khớp với Serial mà chương trình đưa ra. Bây giờ tôi đã biết được Serial của chương trình là gì rồi, đó chính là chuỗi “FIAC”, không cần phải đầy đủ hết cả chuỗi vì như tôi đã phân tích ở trên. Chúng ta bỏ BP tại GetDlgItemTextA đi, sau đó đặt BP tại lệnh nhảy ở trên. Tiếp theo Restart Olly và cho thực thi chương trình 

 - Nhập valid Serial vào, sau đó nhấn Verify. Olly sẽ break . Khà khà Serial đã hợp lệnh, câu lệnh so sánh đã thành công và lệnh nhảy sẽ được thực hiện.Nhấn F9 phát nữa nào và ta có được điều ta cần hehe

#### 4.2 HARDCODED 2 <a name="4.2"></a>

 - với HARDCODED 2 chúng ta cugr thực hiện tương tự như trên , các bạn tự mình tìm cách nhá.

#### 4.3 Detten‟s crackme <a name="4.3"></a>

 - đầu tiên ta load crackme vào ollyDBG để xem nó thế nào đã , sau khi lướt vào ta dừng lại tại OEP của nó ,tiền hành tìm kiếm thông tin về các hàm APIs được sử dụng trong crackme này bằng cách nhấn chuột phải và chọn **seach for** ta có kết quả như sau

 ![](http://i.imgur.com/O8Sp3wB.png)

 - Các bạn thấy có khá nhiều hàm API, tuy nhiên để ý sẽ thấy có 3 hàm API quan trọng (các hàm mà tôi đã đánh dấu) có thể chọn làm đầu mối để tiếp cận mục tiêu.Trong 3 hàm này thì hàm MessageBoxA các bạn đã quá quen thuộc rồi, chỉ còn hai hàm GetWindowTextA và lstrcmpA. Ta sẽ lần lượt xem qua ý nghĩa của hai hàm này : 

 ![](http://i.imgur.com/ByzCLaZ.png)

 - Đọc các thông tin cung cấp ở trên các bạn đã phần nào hiểu được mục đích sử dụng và ý nghĩa của từng hàm API rồi chứ. Như vậy các bạn có thể đặt BP tại các hàm API này để giải quyết bài toán. Giờ chúng ta tìm kiếm thử các String quan trọng , kết quả có được như sau.

 ![](http://i.imgur.com/IYHeD0u.png)

 - Các String quan trọng show ra rõ như ban ngày . Ta nhấn đúp chuột tại ASCII "You entered the right password!", Olly sẽ đưa ta tới đây :
 ![](http://i.imgur.com/yRcmQlk.png)

 - Nhìn tổng thể toàn bộ đoạn code trên ta có thể đưa ra những nhận xét cơ bản như sau :
 <ul>
 <li>1. Chương trình sử dụng hàm API **GetWindowTextA** để copy chuỗi Serial mà ta nhập vào và lưu vào vùng Buffer.</li>
 <li>2. Sau đó chuỗi Fake serial này sẽ được đem đi so sánh với chuỗi Hardcoded serial thông qua hàm API **lstrcmpA**.</li>
 <li>3. Phụ thuộc vào kết quả so sánh mà quyết định hiển thị Good boy hay Bad boy.</li>
 </ul>

 Để kiếm chứng những gì chúng ta vừa nhận xét ở trên, tôi đặt bp tại GetWindowTextA, sau đó nhấn F9 để run chương trình.Tiến hành nhập Fake Serial
 Sau khi nhập xong nhấn Check, Olly sẽ ice tại BP mà ta đã đặt. Ta chuẩn bị thực hiện hàm GetWindowTextA, vùng Buffer sẽ là nơi lưu Serial mà ta nhập vào. Sau khi thực hiện hàm GetWindowTextA, ta dừng lại tại đây :
 ![](http://i.imgur.com/nL2wDuB.png)

 - Khà khà vậy không còn nghi ngờ gì nữa, tới đây ta có thể hoàn toàn kết luận rằng chuỗi Hardcoded Serial của crackme này là “cannabis”.

 ### 5. Detect OllyDbg – IsDebuggerPresent<a name="5"></a>

 - đầu tiên để mở đầu chúng ta thực hiện với crack me ty123.

 - load nó vào ollyDBG và nhấn F9 để xem điều gì xảy ra . bùmmmm , dính ngay Terminate luôn, không làm được gì hết . Đành restart lại Olly và dò xem crackme này “gài” cái gì vào khiến cho em Olly bị đá văng như thế.

- Như đã đề cập ở phần giới thiệu, có một phương pháp rất phổ biến và hay được dùng để detect debugger là sử dụng API **IsDebuggerPresent**, cho nên ta sẽ tập trung sự nghi ngờ ở hàm này. Sau khi restart lại Olly, ta tìm kiếm trong danh sách các API mà crackme sử dụng, để xem có **IsDebuggerPresent** không. Chuột phải và chọn `Search for > Name(label) in current module`, tìm kiếm và ta có được kết quả :

![](http://i.imgur.com/re6wUgm.png)

 - Nhấn chuột phải tại hàm API **IsDebuggerPresent** và chọn Find references to import, kết quả như sau

![](http://i.imgur.com/0DNZdob.png)
 - Như vậy là tôi đã biết được nơi sẽ đưa ra lời gọi tới hàm API IsDebuggerPresent. Tiến hành đặt một BP tại API này thông qua plugin Command Bar:

 - Nhấn F9 để run crackme, Olly sẽ break tại đây :
 ![](http://i.imgur.com/SZV9dcX.png)

 - Ta thấy rằng tại cửa sổ Stack là lời gọi tới hàm API **IsDebuggerPresent**, để ý sẽ thấy không có tham số nào được truyền vào cho hàm này. Ta có cảm giác kiểu như đây là một câu hỏi “Chương trình này có đang bị debug hay không?”. Để có thêm thông tin về hàm API này, chúng ta sẽ sử dụng thư viện Microsoft® Win32® Programmer's Reference để tra cứu. Thư viện này nằm trong file win32.hlp. Ta cấu hình Olly để có thể truy cập được file này như sau
 ![](http://i.imgur.com/P9BwLol.png)

 - Chọn tới đường dẫn chứa file win32.hlp. Sau đó để xem thông tin về một hàm API nào đó, ở đây là **IsDebuggerPresent** thì làm như sau :
 ![](http://i.imgur.com/va2roXI.png)
 ![](http://i.imgur.com/hkMQGQO.png)

 - Đúng hàm cần tra cứu rồi , nhần Display để hiện thị thông tin chi tiết về hàm :
 ![](http://i.imgur.com/WiYiwc4.png)

 - OK đọc toàn bộ thông tin ở trên, các bạn sẽ hiểu thêm về cách thức hoạt động của hàm IsDebuggerPresent . Nó là hàm được export từ Kernel32.dll, dùng để chỉ ra một chương trình có đang bị debug bởi Debugger, nếu bị debug thì hàm này sẽ trả về một giá trị khác 0 (cụ thể trả về 1), ngược lại là bằng 0. Đó là những thông tin quan trọng mà ta thu được, giờ ta cho Olly thực thi API này và dừng lại tại lệnh RETN.

 - Quan sát cửa sổ Registers, ta phát hiện thanh ghi EAX đã thay đổi 

 - Giống như bất kỳ một hàm API nào khác, kết quả trả về luôn được lưu tại thanh ghi EAX. Ở đây thanh ghi EAX bằng 1, điều này chứng tỏ hàm này xác nhận rằng crackme đang bị debug bởi Debugger (cụ thể là Olly). Theo như lý thuyết ta biết được ở trên, nếu giá trị trả về mà là 0 thì đồng nghĩa với việc là không bị debug. Thử sửa lại giá trị của thanh ghi EAX về 0 

 - Thay đổi xong ta nhấn F9 để thực thi chương trình :
![](http://i.imgur.com/mryqLPA.png)

 - Khà khà, chương trình đã thực thi một cách bình thường và Olly của chúng ta không bị terminate giống như lúc đầu nữa. Như vậy, một lần nữa ta khẳng định việc detect, sau đó Olly bị terminate sẽ thông qua kết quả trả về của API `IsDebuggerPresent`. Để tìm hiểu kĩ hơn ta restart lại Olly, sau đó run chương trình cho tới khi Olly break và dừng lại tại các lệnh mov giá trị vào thanh ghi EAX . Nhần F8 để trace cho tới RETN, quan sát giá trị của EAX tại cửa sổ Registers, Sau khi dừng lại tại lệnh RETN ta thấy giá trị của EAX là 1. Tiếp tục nhấn F8 để trace qua RETN và trở về code chính của crackme 

 - Lệnh OR ở `4011A9` mục đích để kiểm tra xem EAX là 0 hay là 1. Do thanh ghi EAX đang có giá trị là 1 nên kết quả lệnh OR này sẽ là 1 và cờ ZF sẽ có giá trị 0. Khi cờ ZF có giá trị 0 thì lệnh nhảy JE bên dưới sẽ không thể thực hiện được và Vậy lệnh JMP bên dưới JE sẽ thực hiện và ta sẽ bị đưa đến đoạn code Terminate Olly

![](http://i.imgur.com/ULwzVkk.png)

 - Hàm này sẽ gửi ra một thông điệp là WM_QUIT, yêu cầu terminate. F8 để thực hiện lệnh JMP và trace tới đoạn code chuẩn bị thực thi hàm này 

 ![](http://i.imgur.com/uz6F1co.png)
 - Thực thi hàm này và tiếp tục trace tiếp, cho tới khi ta dừng lại tại đây :
Chà ExitProcess
` 
 The ExitProcess function ends a process and all its threads. 
 VOID ExitProcess(UINT uExitCode // exit code for all threads );
  Parameters
   uExitCode
   Specifies the exit code for the process, and for all threads that are terminated as a result of this call.
   Use the GetExitCodeProcess function to retrieve the process's exit value. 
   Use the GetExitCodeThread function to retrieve a thread's exit value.
`
 - Vậy coi như là xong, trace tiếp thì Olly của chúng ta bị đá văng luôn. Giờ ta quay trở lại vần đề, như đã phân tích ở trên sau khi thực hiện lệnh OR để kiểm tra thanh ghi EAX, lệnh JE bên dưới sẽ dựa vào kết quả kiểm tra thông qua cờ ZF để đưa ra quyết định nhảy hay không nhảy. Do đó chúng ta có một cách bypass thứ hai để cho chương trình thực thi một cách bình thường trong Olly đó là patch thẳng cái lệnh nhảy đó. Ta restart lại Olly và thực hiện như trên cho tới khi dừng lại tại ` OR EAX,EAX`

 - Ta patch lệnh nhảy JE thành như sau :
 ![](http://i.imgur.com/xudRyaG.png)

 - Sau khi patch xong các bạn có thể lưu lại thành một file khác. Sau đó nhần F9 để kiểm tra . YEP , ĐÃ THÀNH CÔNG. Crackme run ngon lành . Như vậy sơ sơ ta thấy có hai cách :
 <ul>
<li>1. Sửa giá trị của EAX thành 0, tuy nhiên cách làm này ta phải lặp đi lặp lại mỗi khi load chương trình.</li>
<li>2. Patch thẳng lệnh JE thành JMP và lưu thành file mới, với cách này ta chỉ cần thực hiện đúng 1 lần duy nhất.</li>
</ul>

 - Ngoài ra, có một cách đơn giản hơn rất nhiều là sử dụng plugin có sẵn do nhiều tác giả thực hiện để fix cách Anti-debug này. Tuy nhiên, sẽ là thú vị hơn khi tôi và các bạn muốn tìm hiểu xem cách thức các Plugin đó hoạt động thế nào. Ta restart lại Olly, tìm đến đoạn code của API IsDebuggerPresent :
 
 - Chỉ có vẻn vẹn 3 lệnh MOV dùng để detect chương trình có bị debug hay là không.Ghi nhờ 3 lệnh này :
```
7C813093 > 64:A1 18000000 mov eax,dword ptr fs:[18] 
7C813099 8B40 30 mov eax,dword ptr [eax+30] 
7C81309C 0FB640 02 movzx eax,byte ptr [eax+2] 
```
 - Quay trở lại EP của Crackme, sửa lại thành như sau :
 ![](http://i.imgur.com/WZV9nx8.png)

 - Nhần F8 để trace qua các lệnh này, sau khi thực hiện lệnh MOVZX thứ 3 ta thấy giá trị của EAX sẽ là 1, tương tự như việc chúng ta thực hiện hàm IsDebuggerPresent :

 - Oạch chưa gì đã thấy EAX bằng 1 rồi, chúng ta đã thực thi chương trình và trace qua các code của API IsDebuggerPresent đâu, lạ thật!! Như vậy là các giá trị được MOV vào thanh ghi EAX đã được xác định từ trước, nhiệm vụ của thằng IsDebuggerPresent chỉ đơn giản là lấy các giá trị đó ra mà thôi. Vậy ta làm thế nào để mò ra cho lưu các giá trị này nhỉ. Ta thiệt lập lại EP như sau :
 ![](http://i.imgur.com/GQltBkM.png)

 - Sau khi thiết lập xong, nhần F8 để trace qua lệnh move đầu tiên, quan sát cửa sổ Registers :
 ![](http://i.imgur.com/dZ7o2zc.png)

 - Quan sát trên cửa sổ Registers ta thấy có một giá trị rất quan trọng, giá trị này trỏ tới một cấu trúc lưu trữ các thông tin liên quan tới crackme mà chúng ta đang phân tích bằng Olly . Ta Follow in Dump tại giá trị này (giá trị này có thể khác trên máy các bạn) 

 - Cấu trúc này được gọi với tên là TEB (Thread Environment Block). Các bạn có thể tìm hiểu thêm về nó tại hai link sau

 1. [Blog của anh Benina ](http://rootbiez.spaces.live.com/?_c11_BlogPart_BlogPart=blogview&_c=BlogPart&partqs=cat%3dSys%2520Info)
 2. [và tham khảo thêm ](http://undocumented.ntinternals.net/UserMode/Undocumented%20Functions/NT%20Objects/Thread/TEB.html)

 - Quan sát trên hình ta thấy, tại `0x7FFDF000` ta thấy có giá trị E0 FF 13 00 -> 0x0013FFE0 . Nhìn qua cửa sổ Stack ta sẽ thấy giá trị này tương ứng 

 - Nó được đánh dấu là 0013FFE0 FFFFFFFF End of SEH chain .Bài viết này không có thời gian nói về nó và tôi cũng không đủ sức để giải thích, các bạn tự tìm hiểu nhé. Nó có liên quan tới Exceptions. Vậy cụ thể giá trị này tương ứng là gì ?? Xem lại một chút lý thuyết mà anh Benina đã lý giải :

 - **muôn truy xuất vào TEB và PEB đầu tiên ta phải biết base address của các cấu trúc này, chúng ta bắt đầu tìm hiểu base addr của cấu trúc trên. **

 - **window duy trì cấu trúc TEB và PEB cho mọi thread và process đang thực thi trong hệ thống và thanh ghi đoạn FS được SET để truy xuất đến cấu trúc của TEB và thread đang thực thi. vậy địa chỉ VA(virual address) của FS:[0] là bass address của TEB.**

 - ** việc tính toán VA của FS:[0] được bộ vi xử lý thực hiện, vì vậy rất khó khăn để biết được VA của FS:[0] vì do bộ vi xử lý thực hiện , nếu chúng ta thực hiện lệnh** `MOV ax, FS:[0] ` 
 **thìchúng ta chỉ truy xuất được giá trị chứa tại FS:[0] chứ không phải VA của FS:[0] **

 - Vậy đây là Base Address của TEB, tức là FS:[0]. Ta kiểm chứng lại thông qua Command Bar như sau :
![](http://i.imgur.com/XHKoZ8F.png)

- Quay trở lại cửa sổ code, ta có lệnh đầu tiên : 
`00401000 > 64:A1 18000000 mov eax,dword ptr fs:[18] Vậy FS:[18] `
tương ứng với giá trị bao nhiêu sẽ được MOV vào EAX :

 - Tại lại đọc thêm chút lý thuyết :
 - ** nhưng may mắn thay, windown đã hỗ trợ chúng ta như tôi đã đề cập trong phần cấu trúc NT_TIB, base address của NT_TIB chính là base address của TEB do đó ta dễ dàng truy xuất BA qua lệnh mov**

```
MOV EAX, FS:[18H]
```

 - **vậy BA cuả TEB được tham chiếu qua FS:[18h] , để tìm BA của PEB ta dựa vào cấu trúc của TEB, theo cấu trúc của TEB ta có:**

```
0x030  _PEB* ProcessEnvironmentBlock; 
```

 - **vậy tại FS:[30h] chứa BASE ADDRESS của PEB do đó chúng ta có thể dùng chỉ thị mov để truy xuất base address của PEB**

```
 mov eax,FS:[30h]
```

 - **Vậy là FS:[18] là giá trị base address của TIB và cũng là của TEB. Từ giá trị base này mới truy xuất được các giá trị tiếp theo. Để ý lệnh tiếp theo :**

 `00401006 	8B40 	30	 mov 	eax,dword 		ptr [eax+30]`

 - Giá trị của EAX được cộng thêm với 30h, tương ứng với `0x7FFDF000+0x30=0x7FFDF030` :

 - Lệnh MOV sẽ thực hiện chuyển nội dung của 0x7FFDF030 hay FS:[30] vào thanh ghi EAX. Mà ở đây như hình trên ta thấy giá trị đó là 0x7FFD6000. Nhần F8 để thực hiện lệnh MOV này và Follow in Dump tại giá trị vừa MOV vào EAX :

 - Câu lệnh MOVZX cuối cùng : `00401009 	0FB640 	02 	movzx 	eax,byte 	ptr [eax+2]` Ở đây giá trị của EAX được cộng thêm 0x2h, tương ứng với 0x7FFD6000+0x2=0x7FFD6002 :

 - Con số này tương ứng với giá trị sau trong cấu trúc của PEB : 

```
 typedef struct _PEB { BOOLEAN InheritedAddressSpace; 
 BOOLEAN ReadImageFileExecOptions; 
 BOOLEAN BeingDebugged; --> tương ứng với giá trị này!!!
```
 
 - Vậy tại 0x7FFD6002 ta có gì ? .

 - Như vậy qua đây ta thấy hàm API IsDebuggerPresent chẳng có gì là ghê gớm cả, đơn giản nó chỉ lấy ra nội dung của PEB, đã được khởi tạo sau khi load crackme vào Olly. Vậy ta có thể đoán được các plugin sẽ fix thẳng vào giá trị của BeingDebugged trong cấu trúc của PEB. Ta làm như sau, restart lại Olly và dừng lại tại EP. Chuyển qua cửa sổ Registers ta tìm thấy giá trị base address của TIB : Follow in Dump tại giá trị này -->Tìm tới vị trí FS:[30] và xem giá trị tại đó -->Tiếp tục Follow in Dump tại giá trị vừa tìm được --> Cộng thêm 2 ta sẽ tìm được byte cần fix như trên hình. Nếu như để ý kĩ một chút bạn sẽ thấy thanh ghi EBX của chúng ta đang chứa giá trị FS:[30] :

 - vậy là ta lần mò một hồi từ nãy tới giờ .. giờ lại té ngửa ra ngay từ đầu thanh ghi EBX đã lưu giá trị FS:[30] rồi . Vậy thì những gì phải làm để code một plugin là quá đơn giản!! Công việc tiếp theo của ta là sửa lại giá trị của byte mà ta tìm được thành quả

 - Sau khi sửa xong, nhần F9 để thực thi chương trình. Olly sẽ break tại IsDebuggerPresent. Oh yeah!! Ta thấy gì nào, giá trị của EAX đã là 0 đồng nghĩa với việc chương trình không bị Debug lolz . Giờ ta nhần F9 một phát, crackme run vù vù :

### 6. Phân tích DaXXoR - Decryptme<a name="6"></a>

 - Ta đi tổng quan một chút, bình thường các bạn hay sử dụng Task Manager hay một chương trình nào đó để xem các Process đang chạy trên hệ thống của mình (Trên máy tôi sử dụng Process Explorer). Khi chúng ta đang chạy OllyDbg và xem danh sách các Process, ta sẽ thấy như sau :
 ![](http://i.imgur.com/SpreYw5.png)

 - Như các bạn thấy trên hình minh họa, danh sách các Process hiện ra rất rõ ràng. Điều này đồng nghĩa với việc sẽ có một cơ chế Anti-OllyDbg sử dụng cách thức nào đó liệt kê ra danh sách các Process đang hoạt động, sau đó so sánh tên của Process với OLLYDBG, nếu như phát hiện ra có Olly đang chạy thì bùm…coi như Olly đã hi sinh, tương tự như việc ta chọn Process và Kill nó

 - Trong bài viết này ta sẽ phân tích DaXXoR – Decryptme, để xem cơ chế Anti-Olly của nó như thế nào. Sau đó tìm cách vượt qua các cơ chế này. Để kiểm nghiệm lý thuyết vừa nói ở trên, ta chạy Olly trước sau đó chạy Crackme DaXXoR . Ngay khi ta chạy crackme thì Olly lập tức cũng bị terminate liền. Công nhận là ác thật, Olly bay luôn mà không kịp trăng trối điều gì .

 - Ok, bắt đầu nghiên cứu … ta load target vào Olly, Ta đang dừng lại ở EP của target, tiến hành tìm kiếm danh sách các hàm API.

 - Chà một danh sách khá dài, làm sao biết hàm nào là hàm cần tìm, hàm nào được chương trình sử dụng trong việc Anti-Olly đây . Như các bạn thấy, khi ta chạy crackme thì nó mới thực hiện cơ chế Anti-Debug của nó, như vậy có nghĩa là hàm API được sử dụng sẽ không được load từ đầu và đưa vào danh sách các hàm API như trên. Tự hỏi, hàm không được nạp vào trong danh sách API thì làm sao mò ra nó ? Rất may mắn là Windows cung cấp cho chúng ta một thủ thuật để tìm kiếm các APIs được load khi chúng ta thực thi chương trình, từ đó tìm ra hàm API quan trọng. Nếu các bạn là dân coder chuyên nghiệp hay là những người am tường về hệ thống Windows thì chắc rằng các bạn đã biết tôi nhắc tới API nào. Đó chính là hàm GetProcAddress! Giờ ta tìm thử trong Crackme này có sử dụng hàm này không nhé

``` 
**The GetProcAddress function returns the address of the 
specified exported dynamic-link library (DLL) function.
  FARPROC GetProcAddress( 
  
  HMODULE hModule, 	// handle to DLL module 
  LPCSTR lpProcName // name of function );
  
  Parameters
  
  hModule: Identifies the DLL module that contains the function. The 
  LoadLibrary or GetModuleHandle function returns this handle.
  
  lpProcName: Points to a null-terminated string containing 
  the function name, or specifies the function's ordinal value
  . If this parameter is an ordinal value, it 
  must be in the low-order word;
  the high-order word must be zero.**
```

```
Return Values 
If the function succeeds, the return value is the address of 
the DLL's exported function.If the function fails, the return value is NULL.
To get extended error information, call GetLastError. 
Remarks The GetProcAddress function is used to retrieve addresses of exported 
functions in DLLs.
```

 - Tóm lại, nhiệm vụ của hàm GetProcAddress là tìm ra địa chỉ của một hàm trong file Dll. Thường được sử dụng khi chương trình cần load một hàm API mới mà không được liệt kê trong danh sách. Ta đặt BP tại hàm này :

 -Sau khi thực hiện đặt BP xong, nhấn F9 để thực thi chương trình … Olly sẽ dừng lại

 - Quan sát trên cửa sổ Stack ta có được thông tin như trên. `0013FF68 00467BA8 \ProcNameOrOrdinal = "___CPPdebugHook"` là tên của hàm truyền vào để `GetProcAddress` tìm ra địa chỉ của hàm đó. Sau khi thực hiện `GetProcAddress` xong thì kết quả trả về là địa chỉ hàm nằm ở thanh ghi EAX. Do đây chưa phải là hàm mà ta quan tâm nên tiếp tục nhấn F9, cho tới khi Olly dừng lại tại hàm sau trên cửa sổ Stack.

![](http://i.imgur.com/Si9aPKl.png)

 - Tại sao ta lại quan tâm tới hàm này, đơn giản là vì tôi thấy tên của nó có dính tới Proccess nên đặt nghi ngờ. Nhấn Ctrl + F9 (Execute till Return) và quan sát giá trị của EAX ở cửa sổ Registers :

  - Thanh ghi EAX của tôi đang lưu địa chỉ của hàm EnumProcesses, cụ thể là 0x76BF3A9A. Giá trị này có thể khác trên máy của các bạn. Như đã nói ở trên, do hàm này không được liệt kê trong danh sách các hàm APIs, cho nên nếu ta thử đặt BP tại hàm này thì sẽ nhận được thông báo **UNKNOWN IDENTIFIER**

  - Nhưng giờ ta đã biết được địa chỉ của hàm này nên ta có thể đặt BP `pb   76BF3A9A`

  - Đặt BP xong tiếp tục nhấn F9 để tìm kiếm thông tin về các hàm APIs khác , Chà lại một hàm nữa, ta thực hiện tương tự như trên và đặt BP lần này là hàm `enumprocessmodules`. Tiếp tục nhấn F9 và Olly lại break. Đặt BP tương tư như những gì đã thực hiện. Sau đó nhấn F9, lúc này Olly sẽ break tại hàm EnumProcesses mà ta đã đặt BP hàm này là `GetModuleBaseNameA`.

  - Có quá nhiều BP mà ta đã đặt, cho nên để phân biệt là ta đang dừng lại tại BP nào chúng ta sẽ ghi chú tại BP đó như sau, chuột phải tại nơi cần comment và chọn , Nhập thông tin và sau đó nhấn OK, 
  - Thử tìm kiếm thông tin về hàm EnumProcesses trong file win32.hlp, ta nhận được là không có kết quả nào cho hàm này. Không lẽ hàm này thuộc dạng Private. Cách tốt nhất là sử dụng Google để tìm kiếm thông tin. Sau khi tìm kiếm tôi có được thông tin như sau.
  
![](http://i.imgur.com/0v9nFY0.png)

 - Theo kết quả tìm kiếm về hàm này cho thấy nó được sử dụng để lấy thông tin về Process Identifier (PID) của các chương trình đang chạy trên hệ thống. Mỗi chương trình khi thực thi sẽ được nhận biết bởi một con số, con số này sẽ thay đổi khác nhau mỗi khi ta thực thi chương trình đó. Quan sát danh sách các Process đang chạy ta có được như sau
![](http://i.imgur.com/sWk16Es.png)

 -  Như các bạn thấy PID lúc này của Olly đang là 432 ở dạng decimal, để tìm kiếm thông tin về PID trong Olly chúng ta cần đổi về dạng hexa. Dùng chương trình calculator của Windows để chuyển về dạng hexa

 - Như vậy, tôi có được PID ở dạng hexa là 0x1B0. Xin nhắc lại một lần nữa rằng con số này ứng với thời điểm hiện tại mà tôi đang làm việc, nếu như tôi đóng Olly và run lại thì con số này sẽ thay đổi không còn như trên hình minh họa nữa. Ta đã có được PID của Olly, giờ bước tiếp theo là làm sao tìm được giá trị này trong quá trình phân tích target. Ta quay trở lại màn hình Olly và quan sát cửa sổ Stack.
 - Tại sao tôi lại khẳng định được giá trị 0x0013EDE4 tương ứng với tham số pProcessIds. Đơn giản là vị hàm EnumProcesses nhận 3 tham số truyền vào, mà cơ chế push tham số lên Stack thì các bạn chắc cũng đã hiều rồi
 ![](http://i.imgur.com/Ndxlg2Q.png)

  - Thứ tự đưa các biến lên Stack như hình tôi mình họa ở trên. Do đó : 0x0013EDE4 là một “pointer to an array that receives the list of process identifiers”. Giờ ta nhấn Ctrl+F9 để thực hiện hàm EnumProcesses và Follow in Dump tại giá trị 0x0013EDE4 để tìm PID của Olly
  ![](http://i.imgur.com/wR5Pdam.png)

 - Ta đặt một BP lên giá trị vừa tìm được, Nhấn F9 để thực thi target, Olly sẽ dừng lại tại đoạn code có truy xuất tới giá trị PID của Olly

 - Như các bạn thấy trên hình mình họa, target của chúng ta sử dụng hàm API là OpenProcess để kiểm tra xem Process của chúng ta có đang chạy hay không, và nếu có thì sẽ trả về handle của Process đó.
 ![](http://i.imgur.com/FsdoE5B.png)

 - Chà giờ đây ta có thêm một giá trị nữa là handle. Vậy PID và handle khác nhau thế nào? Trong phạm vi hiểu biết của tôi, tôi chỉ có thể giải thích như sau : PID là một con số định danh chung cho một Process đang chạy, con số này sẽ thay đổi mỗi khi bạn chạy lại chương trình. Còn handle, nó cũng là một con số liên quan tới Process mà hệ thống trả về cho chương trình của bạn, để từ đó chương trình của bạn có thể điều khiển và kiểm soát được Process đó. Với các Process khác nhau thì giá trị handle cũng khác nhau. Handle được sử dụng mỗi khi ta muốn kiểm soát một ứng dụng.
 - Ta tiếp tục, nhấn F8 để trace qua hàm OpenProcess. Quan sát cửa sổ Registers ta sẽ thấy EAX đang giữ một con số, đó chính là handle của Process OllyDbg
 - Trên máy tôi nhận được giá trị là 0x88 (giá trị này có thể khác ở máy các bạn). Để kiểm tra giá trị này có chính xác không ta mở cửa sổ Handles (View > Handles), Ta thấy có giá trị 0x88 tương ứng với kiểu Type là Process, như vậy khẳng định đây là handle của OllyDbg. Trở lại cửa sổ code, nhấn F8 và trace tới đây

 - Tra cứu và tìm kiếm thông tin về hàm **EnumProcessModules** xem nó hoạt động thế nào.
 ![](http://i.imgur.com/WU6ioBP.png)

 - Hàm này có nhiệm vụ tìm kiếm và trả về handle của mỗi module trong Process được chỉ định. Ta thấy nó có 4 tham số được truyền vào, tham số mà ta quan tâm ở đây là **hProcess [in]: A handle to the process **và **lphModule [out]: An array that receives the list of module handles**. Nhấn F7 để trace vào lời gọi hàm, Quan sát các tham số trên cửa sổ Stack

 - Nhấn Ctrl+F9 và Follow in Dump tại giá trị **lphModule** ta nhận được giá trị 004000, Giá trị này có nghĩa là khi ta yêu cầu handle của module thì hệ thống sẽ trả về cho ta giá trị base address. Base address là địa chỉ vùng nhờ mà process của chúng ta bắt đầu. Trong hình trên đó là giá trị 0x00400000, tức là nơi OllyDbg bắt đầu tại đó. Tiếp tục trace tiếp cho tới khi xuất hiện hàm API thứ 3 Đó chính là hàm GetModuleBaseNameA, một lần nữa ta đi tìm thông tin về hàm này.

 ![](http://i.imgur.com/iFzToK0.png)

 - Mục đích của nó quá rõ ràng, đó là lấy ra tên của module đang được chỉ định. Trong đó:
 `lpBaseName [out]: A pointer to the buffer that receives the base name of the module. If the base name is longer than maximum number of characters specified by the nSize parameter, the base name is truncated.`

 - Tham số này sẽ nhận kết quả trả về của hàm là tên của module. Nhấn F7 để trace vào trong lời gọi hàm, Quan sát cửa sổ Stack
 ![](http://i.imgur.com/IoR41kD.png)
 - Như trên hình 0x88 là handle của Olly, 0x400000 là base address của module và 0x0013E7F4 là vùng buffer dùng để chứa tên của module. Đây là 3 tham số mà ta cần quan tâm. Ta Follow in Dump tại lpBaseName và nhấn Ctrll + F9, quan sát cửa sổ Dump để xem kết quả có được :
 ![](http://i.imgur.com/djwblxf.png)

 - Ồ cái tên gì mà đẹp thế kia lolz ! Có phải là OLLYDBG.EXE không nhỉ ? Như ta thấy có được tên rồi, giờ nếu đem so sánh chuỗi với cái Name vừa tìm được, kết quả mà giống nhau thì là toi Olly. Tiếp tục nhấn F8 để trace, ta tới đây
 ![](http://i.imgur.com/WBQ51N2.png)

 - Khi ta đọc thông tin về hàm OpenProcess thì thấy có đoạn sau:
 - When you are finished with the handle, be sure to close it using the CloseHandle function.

 - Nhấn F8 để thực hiện hàm này và quan sát cửa sổ Handle, ta thấy handle của Olly không còn trong danh sách nữa.
 -Tiếp tục trace, ta tới đoạn code sau :
 ![](http://i.imgur.com/ahBCEAJ.png)

 - Ta thấy rằng giá trị tại vùng buffer lpBaseName được đẩy vào Stack, theo sau đó làm một lời gọi hàm. Nhấn F7 để trace vào hàm `00401D9A |. E8 21BD0500 |call <DaXXoR.__lstrupr> ; \DaXXoR.0045DAC0`, sau đó trace tiếp đến đoạn code :
 ![](http://i.imgur.com/hExd7cE.png)

 - Như ta thấy, đoạn code này đọc ra kí tự đầu tiên của chuỗi OLLYDBG.EXE, đưa vào thanh ghi EAX và đẩy lên Stack. Sau đó thực hiện một lệnh CALL khác, kết quả sau đó lại lưu trở lại vùng lpBaseName :
 ![](http://i.imgur.com/NSdLn5B.png)
 - Kết quả không thay đổi, vậy có nghĩa là mục đích của hàm `00401D9A |. E8 21BD0500 |call <DaXXoR.__lstrupr> ; \DaXXoR.0045DAC0 `là dùng để convert từ chứ thường sang chữ hoa. Trace ra khỏi hàm này và trace tiếp tới đây :

 ![](http://i.imgur.com/hCRHyWo.png)

 Thông tin có được là quá rõ ràng, trên cửa sổ Stack là hai tham số s1 và s2 chứa hai chuỗi và hàm `00401DA1 |. E8 F29C0500 |call <DaXXoR._strcmp>` được gọi là để so sánh hai chuỗi này với nhau. Mà ta thấy hai tham số s1 và s2 cùng chứa một chuỗi là OLLYDBG.EXE . Đây là toàn bộ đoạn code thực hiện công việc so sánh :
 ![](http://i.imgur.com/PxFK5TS.png)

 - Kết quả so sánh thế nào sẽ tác động trực tiếp lên lệnh nhảy bên dưới, khác nhau thì nhảy và tiếp tục quá trình tiếp theo, còn giống nhau thì chắc các bạn đã tưởng tượng được điều gì sẽ diễn ra
 - Ở đây do kết quả so sánh là giống nhau, thanh ghi EAX có giá trị là 0x0 cho nên lệnh nhảy không thực hiện. Lúc này target của chúng ta lại gọi lại hàm API là OpenProcess để lấy lại handle của Olly, đồng thời truyên thêm tham số là:
```
 PROCESS_TERMINATE Enables using the process handle in the TerminateProcess function to terminate the process.
```

 - Nhấn F8 để thực hiện OpenProcess quan sát cửa sổ Registers ta thấy giá trị handle của Olly, Ta trace tiếp tới lời gọi hàm` 00401DD5 |. E8 78300600 |call <DaXXoR.TerminateProcess> ; \TerminateProcess` .

 - Tới đây coi như Olly của chúng ta chuẩn bị bay rồi đấy, nhấn F8 một phát và bùm …, Olly văng luôn, bị terminate thẳng tay. Giờ làm cách nào để bypass cơ chế này bây giờ, có 3 cách như sau :
 <ul>
 <li>1. Thay đổi kết quả trả về của hàm **OpenProcess**.</li>
 <li>2. Patch thẳng vào lệnh nhảy để vượt qua lời gọi hàm **TerminateProcess**.</li>
 <li>3. Đổi tên của Olly thành tên khác .</li>
 </ul>

 - Với cách đầu tiên ta làm như sau, mở Olly lên và đặt BP tại hàm OpenProcess và nhấn F9 để thực thi :
 Ta fix như sau
 ![](http://i.imgur.com/1VoOeHd.png)

 - Patch xong ta xóa BP đã đặt đi, sau đó nhấn F9 để kiểm tra kết quả, Tuy nhiên cách patch này không được khuyến khích vì nó tác động trực tiếp vào hàm API, ảnh hưởng tới quá trình sử dụng hàm sau này.

 - Cách thứ hai đơn giản hơn, ta patch lệnh JNZ thành JMP

 - Cuối cùng là cách 3, đơn giản nhất mà lại hiệu quả cao. Ta đổi tên của OllyDbg thành tên bất kì mà ta muốn.

 ### 7. Phân tích buggers3.exe<a name="7"></a>

  - Ta sử dụng Plugin HideDebugger để bypass cơ chế Anti bằng IsDebuggerPresent. Ở đây tôi không quan tâm là target có sử dụng cơ chế này hay không, mục đích đơn giản là ta đã hiểu biết cơ chế này rồi, nếu như target này có sử dụng thì ta dùng plugin để bypass luôn cho đỡ mất thời gian :

  - Ta Save lại và sau đó restart Olly để thiết lập trên có hiệu lực. Tiếp theo, sử dụng Task Manager hay Process Explorer để xem danh sách các process đang chạy (trong đó có OLLYDBG.exe của chúng ta)

  - OK .. như các bạn thấy trên hình là tên nguyên gốc của Olly, tôi không có thay đổi gì hết. Ta quay trở lại màn hình chính của Olly và tìm kiếm danh sách các hàm API được target sử dụng. Nhấn phím tắt là Ctrl + N
  - Kết quả hơi ngạc nhiên, có mỗi hàm ExitProcess. Ở phần 20 trước, ta còn thấy crackme sử dụng hàm `GetProcAddress` để mà mò theo, chứ còn ở target này thì không thấy được liệt kê trong danh sách . Giờ không có thì làm thế nào đây, thôi thì cứ đặt BP tại hàm `GetProcAddress` thử xem thế nào.
  -Sau khi đặt xong BP, nhấn F9 để thực thi target, Olly sẽ break tương tự như hình
![](http://i.imgur.com/8NzXCU4.png)

 - Mục đích của GetProcAddress được sử dụng làm gì thì các bạn cũng đã hiểu rồi, ở đây ta thấy nó đang chuẩn bị lấy địa chỉ của hàm FreeLibrary. Kiểm tra thông tin về hàm này thấy không có gì quan trọng, ta bỏ qua và tiếp tục nhấn F9

 - Tiếp tục F9 cho tới khi ta nhận được thông tin về hàm `CreateToolhelp32Snapshot`

 - Hơi nghi ngờ hàm này! Tại sao tôi lại nghi ngờ nó, đơn giản tôi thấy có từ **Snapshot **trong tên hàm, thường thì như các bạn hay thấy là từ snapshot liên quan tới việc chụp ảnh, nhưng trong ngữ nghĩa của hệ thống thì có thể là nó được dùng để “chụp” các thông tin gì đó về hệ thống mà ta chưa biết rõ ngay lúc này. Do đó, ta cứ đặt BP tại hàm này cho chắc. Nhấn Ctrl + F9 (execute till return), quan sát thanh ghi EAX ở cửa sổ Registers :

 - Ta thấy, EAX đang lưu địa chỉ của hàm CreateToolhelp32Snapshot . Đặt BP tại hàm này bằng cách dùng comment bar .

 - Sau đó ta thêm comment và label để ghi nhớ hàm này. Mục đích để phân biệt các địa chỉ ta đặt BP, Tiếp tục nhấn F9 và quan sát cửa sổ Stack, Ta thấy có OpenProcess, chức năng hàm này thế nào thì bài trước tôi giới thiệu rồi. Ta cũng tiến hành đặt BP ở hàm này bằng cách tương tự như trên, Tiếp tục nhấn F9 . Hàm tiếp theo Process32First, ta cũng đặt BP tại hàm này. Khai thác tiếp -->Kết quả ta có là Process32Next, target này dùng nhiều API mới lạ quá . Ta đặt BP tại hàm này. tiếp tục F( , Ái chà, tìm địa chỉ của hàm TerminateProcess kìa lolz. Hàm này ở bài 20 các bạn đã biết nó dùng để làm gì rồi, do đó khỏi cần đặt BP tại hàm này. Ta tiếp tục nhấn F9

- Hàm tiếp theo như ta thấy là FindWindowA, chắc là nó dùng để tìm kiếm thông tin gì đó. Ta đặt BP tại hàm này, Tổng kết toàn bộ từ đầu đến giờ chúng ta đã thiết lập những BP sau:

![](http://i.imgur.com/DrQM5W8.png)

 - F9 thêm một lần nữa, lần này ta sẽ break tại hàm CreateToolhelp32Snapshot, ta Tìm kiếm thông tin về hàm này:
 ![](http://i.imgur.com/mk8Slk2.png)
 - Theo thông tin có được ở trên, ta nắm được mục đích sử dụng của hàm này như sau : Hàm này được sử dụng để chụp nhanh thông tin của các Process, nó nhận hai tham số truyền vào là dwFlags và th32ProcessID. Sau khi thực hiện, hàm sẽ trả về một handle (gọi là handle của snapshot), mà handle này sẽ được sử dụng bởi các hàm khác. Cụ thể với target này ta có :

<ul>
<li>a. dwFlags = TH32CS_SNAPPROCESS có nghĩa là tất cả các process đang chạy trên hệ thống. Sau đó sẽ sử dụng hàm Process32First để lấy tiếp thông tin cụ thể</li>
<li>b. th32ProcessID = 0 có nghĩa là nó muốn lấy thông tin của Process hiện tại</li>
</ul>

 - Để đơn giản hơn ta hiểu như kiểu ta chụp một bức ảnh, trong bức ảnh đó có rất nhiều người - đại diện cho các process trên hệ thống. Sau khi chụp xong, ta đánh một mã hay một con số cho bức ảnh ta chụp được để sau này ta cần sử dụng lại bức ảnh thì sẽ dễ dàng hơn, con số này tương ứng với handle.

 - OK, sau khi phân tích về hàm xong ta nhấn Ctrl + F9 và kiểm tra kết quả trả về ở thanh ghi EAX

 - EAX có giá trị là 0x30, đó chính là handle của snapshot. Ta kiểm tra cửa sổ Handles của Olly xem có thông tin gì về nó không

 - Như các bạn thấy, thông tin mang lại cho chúng ta không nhiều và hơi trừu tượng. Nhưng tóm lại ta hiểu là chúng ta đã có được handle của snapshot - mà snapshot này bao gồm danh sách các process. Ta nhấn F9 để thực thi target, Olly sẽ break 

 - Như thông tin ta đọc về hàm CreateToolhelp32Snapshot thì cờ khi dwFlags = TH32CS_SNAPPROCESS, để liệt kê các process ta cần sử dụng hàm Process32First. Ta tìm hiểu về hàm này:
![](http://i.imgur.com/5pXHUjP.png)
 - Ta thấy hàm này dùng để lấy ra thông tin của Process đầu tiên dựa vào handle của snapshot có được thông qua hàm` CreateToolhelp32Snapshot`, sau đó thông tin của process sẽ được lưu vào một cấu trúc có tên gọi là `PROCESSENTRY32`.Căn cứ vào thông tin từ cửa sổ Stack ta biết được `hSnapshot = 00000030` và vùng nhớ để lưu thông tin về Process là `pProcessentry = offset <buggers3.dword_403134>`.Tìm hiểu thông tin về cấu trúc dùng để lưu thông tin về Process
 ![](http://i.imgur.com/SdtUy0W.png)

 - Follow in Dump tại vùng nhớ 0x00403134, nơi dùng để chứa thông tin. Nhấn Ctrl+F9 và quan sát kết quả tại vùng nhớ này :
 ![](http://i.imgur.com/ADU6hUA.png)

 - Ta quan tâm tới hai thành phần của cấu trúc này là szExeFile và th32ProcessID. Như các bạn thấy trên hình, process đầu tiên của chúng ta có tên là System Process, và PID của nó là 0x0. Ta dùng ProcessExplorer để kiểm tra xem nó trùng với process nào, sau đó thì nhấn F9 để tiếp tục

 - mấu chốt vấn đề bắt đầu tại đây. Ta thấy target sử dụng hàm API FindWindowA để tìm kiếm thông tin. Thông tin cụ thể về hàm này như sau
 ![](http://i.imgur.com/79nydxT.png)
 - Như vậy, hàm này dùng để tìm ra handle của cửa sổ (top-level) mà có tên cửa sổ hoặc tên class trùng với thông tin mà nó chỉ định. Cụ thể với target của chúng ta, nó mượn hàm này để tìm kiếm thông tin về Class có tên là OllyDbg (**0013FFBC 004030AE |Class = "OllyDbg"**). Trước khi đi tiếp chúng ta dừng lại một chút để sử dụng một chương trình tìm kiếm thông tin về các window, đó là chương trình **Greatis WinDowse - Advanced Windows Analyser**. Tôi có kèm theo trong bài viết này. Ta cài đặt và chạy chương trình này, sau đó tìm kiếm thông tin liên quan tới Olly
 ![](http://i.imgur.com/wq6p0oG.png)

 - Chuyển qua tab Class , Kết quả cho ta thấy Class name là OLLYDBG. Giờ ta quay lại Olly và nhấn Ctrl+F9 để thực hiện hàm FindWindowA, đồng thời quan sát giá trị của thanh ghi EAX tại cửa sổ Registers :

 - Kết quả này trùng với giá trị mà chương trình WinDowse thu thập được. Ta nhấn F8 trace qua lệnh RETN để trở về code chính của target, quan sát xem nó sẽ làm gì với giá trị handle có được

 - Code thể hiện rất rõ ràng, EAX lưu giá trị handle có được sau quá trình tìm kiếm, nó được đem so sánh với 0. Nếu như kết quả tìm kiếm trả về cho thanh ghi EAX giá trị là 0 thì CMP sẽ kiểm tra và bật cờ ZF thành 1. Lệnh OR sau đó sẽ kiểm tra xem EAX có bằng 0 hay không, nếu bằng thì vẫn giữ nguyên cờ ZF và lệnh nhảy JNZ bên dưới sẽ không thực hiện. Nhưng lúc này EAX đang giữ giá trị handle <> 0, cho nên lệnh nhảy JNZ sẽ thực hiện
 ![](http://i.imgur.com/sBVCvOE.png)

 - Nhìn vào đoạn code này, điều mà ta mong muốn là lệnh nhảy sẽ không thực hiện vì nếu nó thực hiện nó sẽ tới đoạn code gọi tới hàm API ExitProcess như bạn đang thấy trên hình. Mà để nó không thực hiện thì kết quả trả về của hàm FindWindowA phải là thanh ghi EAX có giá trị 0x0. Như vậy trong trường hợp này ta phải tìm cách nào đó để thay đổi Class Name của Olly window. Nếu để ý ở plugin HideDebugger các bạn sẽ thấy có một option cho phép ta vượt qua kiểu Anti-Debug này 

 - Tuy nhiên, ta sẽ không chọn option này vào thời điểm này. Vì nếu ta chọn thì phải restart lại OllyDbg mới có hiệu lực. Do đó ta bypass bằng tay, để không cho lệnh nhảy thực hiện thì có hai cách :
 <ul>
 <li>1. Fix cứng bằng cách patch lệnh JNZ thành JMP</li>
 <li>2. Fix tạm thời bằng cách thay đổi giá trị cờ ZF.</li>
 </ul>

 - Ở đây tôi chọn cách 2, nhấn đúp chuột vào cờ ZF để thay đổi giá trị cờ này, Quan sát cửa sổ code, ta thấy lệnh nhảy sẽ không được thực hiện

 - Nhấn F8 để trace tới lệnh JMP, lệnh này sẽ nhảy qua lời gọi hàm ExitProcess.
 - OK … vậy là tôi và các bạn vừa bypass qua cơ chế Anti-Debug sử dụng hàm FindWindowA. Không lẽ chỉ có mỗi cơ chế này, mấy cái hàm liên quan tới Process phía trên chỉ để làm cảnh thôi sao? Nghi ngờ quá, ta tiếp tục nhấn F9 để thực thi chương trình.
 - Olly break và trên cửa sổ Stack ta có thông tin như trên. Target sử dụng tiếp hàm API Process32Next để tìm kiếm thông tin về các process tiếp theo. Thông tin về process tiếp tục được lưu vào vùng buffer 0x00403134. Nhấn Ctrl + F9 để thực hiện hàm này, quan sát kết quả có được tại vùng buffer :

 - Kết quả này tương ứng với process sau
 ![](http://i.imgur.com/SRmSbgR.png)
 - Nhấn F8 trace qua lệnh RETN để trở về code chính, ta dừng lại tại đây
 ![Imgur](http://i.imgur.com/lB8JAhK.png)

 - Ta có thông tìn gì từ đoạn code này nào? Nó sử dụng hàm `lstrcmpA` để so sánh 2 chuỗi, chuỗi đầu tiên là tên của target (ASCII "`buggers3.exe`"), chuỗi thứ hai là tên của Process mà ta có được thông qua hàm `Process32Nex`t. Nếu hai chuỗi này là giống nhau thì ta sẽ nhận được thông báo là **not debugged!** và sau đó thực hiện lệnh nhảy tới địa chỉ `004012AB .^\E9 E5FEFFFF jmp <buggers3.loc_401195>`. Nơi sẽ gọi hàm `ExitProcess` để thoát khỏi chương trình.

 - Tuy nhiên, do hai chuỗi của ta là không giống nhau cho nên kết quả không được đẹp như mô tả ở trên. Do EAX <> 0x0 cho nên kết quả lệnh TEST bên dưới sẽ không tác động lên cờ ZF, do đó lệnh nhảy sẽ thực hiện. Lệnh nhảy này đưa ta tới đoạn code sau:
![Imgur](http://i.imgur.com/hYpKDS9.png)

 - Chà đoạn này hay đây!! So sánh tên của Process tìm được với chuỗi (ASCII "OLLYDBG.EXE"). Nếu giống nhau thì EAX sẽ bằng không và lệnh nhảy sẽ không thực hiện. Đoạn code ở dưới sẽ gọi tới hàm OpenProcess để lấy handle của OllyDbg, sau đó truyền handle này cho hàm TerminateProcess để thực hiện kill Olly của chúng ta . Do hiện tại tên Process đưa vào để so sánh chưa trùng cho nên target tiếp tục thực hiện hàm Process32Next để tìm kiếm tiếp

 - Để xem Process tiếp theo là gì nào, nhấn Ctrl+F9 thực thi hàm. Quan sát kết quả
 ![Imgur](http://i.imgur.com/j7fEMGa.png)

 - Process tiếp theo là smss.exe, nó là child process của System process. Như vậy, tuần tự nó cứ gọi hàm để lấy thông tin về Process, sau đó lấy tên process có được so sánh với chuỗi OLLYDBG.EXE. Cứ đà đó không sớm thì muộn, kiểu gì cũng tìm thấy OllyDbg process của chúng ta, Tới đây thì ta đã hiểu được cơ chế Anti-Debug thứ hai được sử dụng bởi target này rồi. Để không phải mất thời gian thêm nữa, ta patch lệnh nhảy tại địa chỉ 0x004011B1 như sau
 ![Imgur](http://i.imgur.com/ndTIrVq.png)

 - Patch xong ta disable tất cả các BP đã đặt. Sau đó nhấn F9 để run, ta nhận được kết quả cuối cùng như mong muốn 

 - Phù toàn bộ công sức mà chúng ta bỏ ra đã được đền bù xứng đáng. Với việc phân tích target như trên chúng ta đã hoàn toàn có thể manual để bypass được 2 cơ chế Anti-Debug mới. Tuy nhiên, để đỡ phải lặp đi lặp lại việc này ta sẽ sử dụng plugin + tool để fix Olly, sao cho có thể bypass luôn hai kiểu Anti-Debug mà không phải mất công nữa. Đầu tiên, mở Olly mà ta đã đổi tên như ở bài 20 lên (của tôi là M4n0W4R.EXE), sau đó chọn tùy chọn thứ hai của HideDebugger plugin để bypass cơ chế sử dụng FindWindowA. Khi ta đổi tên Olly thì tức là ta đã pass luôn cơ chế Anti-Debug sử dụng (CreateToolhelp32Snapshot, Process32First, Process32Next).

 - các phần khác làm tương tự và nhớ chú ý đến các hàm **SetUnhandledExceptionFilter** , **UnhandledExceptionFilter**, **ZwQueryInformationProcess**, **NtQueryInformationProcess**,

 - ngoài ra hãy chú ý đến các phương pháp sử dụng **NtGlobalFlag** và **HeapFlags** tại tut 23 hoặc tìm hiểu thêm trong cuốn Malware Analysis - Michael Sikorski & Andrew Honig.

 - tổng quan ta có các phương pháp anti - ollyDBG như sau
 <ul>
 <li>Sử dụng kĩ thuật **“Self-Modifying-Code”**, kết hợp với phá vỡ cấu trúc lệnh `pushad/popad` làm cho vùng Stack của Crackme bị thay đổi, gây ra exception.</li> 
 <li>Sử dụng một trick để anti-SoftIce đem áp dùng cho OllyDBG là `int 68 `để tạo ra exception.</li>
 <li>Sử dụng hàm **CreateToolhelp32Snapshot** và các API liên quan để lấy thông tin về các Process đang chạy trên hệ thống, sau đó đem so sánh với các chuỗi encrypt được decrypt về dạng clear text, nếu trình Debug/ Disassembler trùng tên với danh sách các black list thì sẽ gọi hàm **TerminateProcess**.</li>
 <li>Sử dụng hàm **GetWindowsText** để lấy thông tin về tiêu đê hay tên của trình Debug, sau đó cũng đem so sánh với một danh sách black list, nếu giống gọi hàm **PostQuitMessage**.</li>
<li>Sử dụng hàm **GetAsyncKeyState** để phát hiện xem có nhấn phím Ctrl hay không, nếu có cũng sẽ gọi hàm **PostQuitMessage**.</li>
</ul>


