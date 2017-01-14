###BÁO CÁO BÀI TẬP CHƯƠNG 6

> người thực hiện :nguyễn văn hiệp

> cập nhật lần cuối 14/1/2017

> ----------------

###mục lục

>[1. câu 1](#1)

> - [1.1 câu 1a](#1.1)

> - [1.2 câu 1b](#1.2)

> - [1.3 câu 1c](#1.3)

> - [1.4 câu 1d](#1.4)

> - [1.5 câu 1e](#1.5)

> - [1.6 câu 1f](#1.6)

> [2. câu 2](#2)

> [3. câu 3](#3)

> - [3.1 câu 3a](#3.1)

> - [3.1 câu 3b](#3.2)

> [4. câu 4](#4)

> [5. câu 5](#5)

> [6. câu 6](#6)

> [7. câu 7](#7)


> -------------------


###1. câu 1 <a name="1"></a>

 - viết các lệnh hợp ngữ cho cấu trúc lựa chọn
###1.1 câu 1A <a name="1.1"></a>

```
CMP 	AX,0		;so sánh AX với 0
JL 	gan				;nếu AX<0 thì nhảy đến nhãn gan
JMP 	out			;nếu AX>= 0 thì nhảy đến nhãn out
gan:
MOV 	BX,-1 		
out: 
MOV 	AH,4CH
INT 	21H
```
###1.2 câu 1B <a name="1.2"></a>

```
CMP 	AX,0		; so sánh AX với 0
JL 	gan				; nếu AX<0 thì nhảy đến gan
JMP 	out			;còn nếu AX>=0 thì nhảy đến out
gan:
MOV 	BX,-1
out: 
MOV 	AH,4CH
INT 	21H
```

###1.3 câu 1C <a name="1.3"></a>

```
CMP 	AX,0 		;so sánh AX với 0
JL 	gan				; nếu AX<0 thì nhảy đến gán
JMP 	out			; nếu AX>=0 thì nhảy đến out
gan:
MOV 	BX,-1 			
out: 
MOV 	AH,4CH
INT 	21H
```

###1.4 câu 1D<a name="1.4"></a>

```
CMP	AX,BX   		;so sánh AX với BX
JL 	SS2				; nếu AX<BX thì nhảy đến ss2
JMP  	GAN2 		; nếu AX>=BX thì nhảy đến gan2
SS2:
CMP 	BX,CX 		;tiếp tục so sánh BX với CX
JL 	GAN1 			; nếu BX<CX thì nhảy đến gan1
GAN2:
MOV 	BX,0 
JMP 	THOAT 		;nhảy đến thoat
GAN1:
MOV 	AX,0
JMP 	THOAT 		;nhảy đến thoat
THOAT:
MOV 	AH,4CH
INT 	21H
```

###1.5 câu 1E <a name="1.5"></a>
 
```
CMP 	AX,BX 		;so sánh AX với BX
JGE 	SS2 		; nếu AX>= BX thì nhảy đến ss2
JMP 	GAN1 		; nếu không thì nhảy đến gan1
SS2:
CMP 	BX,CX 		; so sánh xem BX và CX 
JGE 	GAN2 		; nếu BX>=CX thì nhảy đến gan2 
JMP 	GAN1 		; nếu không thì nhảy đến gan1
GAN1:
MOV 	DX,0
JMP 	OUT 		; nhảy đến out
GAN2:
MOV 	DX,1
OUT:
MOV		AH,4CH
INT 	21H
```

###1.6 câu1F <a name="1.6"></a>

```
CMP 	AX,BX 		;so sánh AX và BX
JL 	GAN1 			; nếu AX>BX thì nhảy đến gan1
CMP 	BX,CX 		; so sánh BX và CX
JL 	GAN2 			; nếu BX>CX nhảy đến gan2
MOV 	CX,0 		; nếu AX <=BX và BX<= CX thì gán CX=0
JMP 	OUT  		;và nhảy đến out để kết thúc
GAN1:
MOV 	AX,0
JMP	OUT
GAN2:
MOV 	BX,0
OUT:
MOV 	AH,4CH
INT 	21H
```

###2. câu 2 <a name="2"></a>

```
.stack 100h
.model  small 
.data 
m1  dw 'nhap mot ky tu vao$'
.code
main 	proc 
;khoi tao ds
    MOV     AX,@DATA
    MOV     DS,AX
; HIEN THI KY TU
    LEA     DX,M1
    MOV     AH,9
    INT     21H
;NHAN KY TU DAU TIEN    
    MOV 	AH,1       ;HAM NHAN 1 KY TU
    INT 	21H 	  ;NHAN VA LUU VAO AL
;so sanh ky tu do voi A
    CMP 	AL,'A'
    JE 	    NHAY1
;so sanh ky tu do voi B
    CMP 	AL,'B' 
    JE 	NHAY2
;neu khong phai a va b
    JMP 	THOAT
    NHAY1:
    MOV 	DL,0DH
    MOV 	AH,2
    INT 	21H
    JMP 	THOAT
    NHAY2:
    MOV 	DL,0AH
    MOV 	AH,2
    INT 	21H
    THOAT:
    MOV 	AH,4CH
    INT 	21H
main    endp         
end main
```

###3. Câu 3 <a name="3"></a>

####3.1 câu 3A <a name="3.1"></a>

```
.stack 100h
.model small
.data
.code
main	proc 
MOV 	AX,0    ;GIA TRI BAN DAU CUA AX
MOV 	DX,1    ;KHOI TAO BIEN
LAP:
CMP 	DX,151  ; SO SANH DX VOI 151
JE 	THOAT       ;NEU DX=151 THI NHAY DEN THOAT
ADD 	AX,DX   ;CONG AX THEM CHO DX
ADD 	DX,3    ;KHOI TAO DX MOI
LOOP 	LAP     ;TIEP TUC LAP
THOAT:
MOV 	AH,4CH
INT 	21H  
MAIN    ENDP
END MAIN
;bai toan tinh tong AX=1+4+7+...+148  	
```
####3.2 câu 3B <a name="3.2"></a>

```
.stack  100h
.data
.model  small
.CODE
main	proc      
MOV 	AX,0 		;khởi tạo giá trị AX=0
MOV 	DX,100 		;DX = 100
LAP:
CMP 	DX,0 		; so sánh DX với 0
JE 	THOAT 			;nếu DX =0 thì nhảy đến thoat
ADD 	AX,DX 		; nếu DX khác 0 thì cộng AX với DX
SUB 	DX,5 		; khởi tạo giá trị mới DX=DX-5
LOOP 	LAP 		;nhảy trở lại
THOAT:
MOV 	AH,4CH
INT 	21H
main    endp
end main
;bài toán tính tổng 100 +95+90....+5
```
###4.câu4 <a name="4"></a>

** câu 4A**
```
.stack 100h
.model  small
.data
.code
main	proc
MOV 	BX,1    ; KHOI TAO GIA TRI
MOV 	AX,0
MOV 	DX,0
LAP:
CMP 	AX,50  	;SO SÁNH AX VỚI 50
JE 	THOAT 		; NẾU AX = 50 THÌ THOAT
ADD 	DX,BX 	;NẾU AX CHƯA BẰNG 50 THÌ CỘNG BX CHO DX
ADD 	BX,4 	; KHỞI TẠO TIẾP GIÁ TRỊ CHO BX=BX+4
INC 	AX 		; TĂNG BIẾN ĐẾM THÊM 1 
LOOP 	LAP
THOAT:
MOV 	AH,4CH
INT 	21H
MAIN    ENDP
END MAIN
;bài toán tính tổng DX= 1+5+9+...n cộng 50 lần như thế
```

**câu 4B**
```
.DATA
MSG 	DB	0AH,0DH,'$'
m1      dw  ?'$'
.code
main	proc
MOV 	AX,@DATA
MOV	    DS,AX
;nhan vao 1 ky tu
MOV 	AH,1
INT 	21H
MOV     M1,AX
;xuong dong va ve dau dong
LEA 	DX,MSG
MOV 	AH,9
INT 	21H
;IN RA
MOV 	CX,80	;KHOI TAO BIEN DEM
LAP:           
LEA     DX,M1
MOV 	AH,9
INT 	21H
LOOP 	LAP
;ket thuc
MOV 	AH,4CH
INT	    21H
main    endp
end main
;bài toán nhập 1 ký tự và hiển thị nó 80 lần ở dòng tiếp theo
```

**CÂU 4C **
```
.STACK  100H
.DATA
.MODEL  SMALL
.code
main	proc
MOV	CX,5
MK:
MOV 	AH,1
INT	    21H
LOOP	MK
;tro ve dau dong
MOV 	AH,2
MOV 	DX,0DH
INT 	21H
;GHI DE LEN
MOV 	CX,5
PRINT:
MOV 	DX,'X'
MOV 	AH,2
INT 	21H
LOOP    PRINT
;KET THUC
MOV 	AH,4CH
INT 	21H
main    endp
end main
;chương trình nhập vào 5 ký tự mật khẩu sau đó trở về đầu hàng và chèn lại nó bằng 5 dấu 'x'
```

###5.CÂU5 <a name="5"></a>
```
.MODEL SMALL
.STACK 100H
.DATA
M1 DB 0DH,0AH,'NHAP SO THU NHAT: $'
M2 DB 0DH,0AH,'NHAP SO THU HAI: $'
M3 DB 0DH,0AH,'$ '
A DW ?
B DW ?
.CODE
MAIN PROC
    MOV 	AX,@DATA
    MOV 	DS, AX
    ;NHAP_A
    LEA 	DX,M1
    MOV 	AH,9
    INT 	21H
    ;
    MOV 	AH,1
    INT 	21H
    SUB 	AL, 30H
    MOV 	A, AX
    ;NHAP_B
    LEA 	DX,M2
    MOV 	AH,9
    INT 	21H
    ;
    MOV 	AH,1
    INT 	21H
    SUB 	AL, 30H
    MOV 	B, AX
    ;CHUYEN
    MOV 	AX, A
    SUB 	AX,100H
    MOV 	BX, B
    SUB 	BX,100H
    ;DIV
    MOV 	CX, 0H
    WHILE_:
    CMP 	AX,BX
    JNGE	 END_WHILE
    ADD 	CX,1H
    SUB 	AX, BX
    JMP 	WHILE_
    END_WHILE:
    ;
    LEA		DX,M3
    MOV 	AH,9
    INT 	21H
    ;
    MOV DX, CX
    ADD DL,30H
    MOV AH,2
    INT 21H
    ;DOS
    MOV AH,4CH
    INT 21H
    MAIN ENDP
END MAIN

;khởi tạo thương số bằng 0
;while số bị chia >= số chia
;tăng thương số
;trừ bớt số chia từ số bị chưa
;end_while
;AX: số bị chia, BX: số chia, CX: thương số
```

###6. CÂU6 <a name="6"></a>
```

.model small
.stack 100h
.data
M1  DW  'HAY NHAP SO THU NHAT$',0DH,0AH
M2  DW  'HAY NHAP SO THU HAI $',0AH,0DH 
M3  DW  'TICH HAI SO LA $'0AH,0DH
a	dW	?
b	dW  ?
C   DW  ?
.code
main	proc 
    ;khoi tao ds
    MOV     AX,@DATA
    MOV     DS,AX
;nhap vao a 
    LEA     DX,M1
    MOV     AH,9       
    INT     21H
    MOV	    AH,1    ;gia tri cua a duoc luu o ax
    INT	    21H
    SUB     AX,30H
    MOV	    A,AX    ;gan gia tri cua ax cho bien a
;nhap vao so b
    LEA     DX,M2
    MOV     AH,9       
    INT     21H
    MOV	    AH,1    ;gia tri cua b nam trong ax
    INT 	21H
    SUB     AX,30H
    MOV     B,AX    ;gan gia tri ax cho b
; GAN GIA TRI
    MOV	    BX,B    ;chuyen gia tri cua b cho bx
    MOV	    AX,A    ;chuyen gia tri cua a cho ax
;nhan AX VAO BX
    MOV	    C,0     ;khoi tao tich =0
    LAP: 
    ADD	    C,AX    ;neu bx khac 0 thi cong c them voi ax
    DEC	    BX      ;tru gia tri cua b di 1           
    CMP	    BX,0    ;so sanh bx voi 0
    JG	    LAP     ;neu bx >0 thi nhay den LAP
   
;IN VA THOAT
    LEA 	DX,M3
    MOV 	AH,9
    INT 	21H
    MOV 	DX,C        ;gan gia tri cua c cho dx 
    ADD 	DX,30H
    MOV 	AH,2
    int 	21h         ;chuan bi hien thi no
    ;THOAT
    MOV		AH,4CH
    INT		21H
    MAIN    ENDP
END MAIN
  ;BAI TOAN AX NHAN BX 
```
###7. câu7 <a name="7"></a>
```
.MODEL SMALL
.STACK 100H
.DATA
MSG_1 DB 0DH,0AH,'CAU A: $'
MSG_2 DB 0DH,0AH,'CAU B: $'
TMP DW ?
.CODE
MAIN PROC
    MOV		 AX,@DATA
    MOV 	DS,AX
    ;CAU A
    LEA 	DX,MSG_1
    MOV 	AH,9
    INT 	21H
    ;
    MOV 	AX, 0H;
    MOV 	TMP, AX
    CAU_A:
        MOV 	AH,1
        INT 	21H
        MOV 	AX, TMP
        INC 	AX
        MOV 	TMP, AX
        MOV 	AX,TMP
        CMP 	AX,50H
    LOOPE CAU_A
        CMP 	AL,' '
    LOOPE 		CAU_A
    ;FINISH_A
    ;CAU_B
    LEA		 	DX,MSG_2
    MOV 		AH,9
    INT 		21H
    ;
    MOV 		AX,0H
    MOV 		TMP,AX
    CAU_B:
        MOV	 	AH,1
        INT 	21H
        MOV 	AX, TMP
        INC 	AX
        MOV 	TMP, AX
        ;
        MOV 	AX,TMP
        CMP 	AX,50H
    LOOPNE 		CAU_B
        CMP 	AL,0DH
    LOOPNE 		CAU_B
    ;DOS
    MOV AH,4CH
    INT 21H
   main 	ENDP
END 	MAIN
```
###hết
