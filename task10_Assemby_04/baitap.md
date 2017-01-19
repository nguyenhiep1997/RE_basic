##báo cáo

>task 10_4 assembly 

> người thực hiện: nguyễn văn hiệp

> cập nhật lần cuối :19 - 1 - 2017 

###mục lục.

>[1. câu 8](#1)

>[2. câu9](#2)

>[3. câu 10](#3)

>[4. câu11](#4)

>[5. câu 12](#5)

>[6. câu 13](#6)

>[7. câu 14](#7)

> ----------------------
###1. câu 8:	<a name="1"></a>
 - viết một chương trình thông báo cho người sử dụng vào 1 phím bất kỳ , in ra trên các dòng liên tiếp nhau mã ascII của ký tự đó dưới dạng nhị phân, số các chữ số 1 trong mã ascii đó

  - code
```
.stack 100h
.model small
.data 
x1      db  0dh,0ah,'nhap vao 1 ky tu $',0ah,0dh
x2      db  0ah,0dh,'ma asc2 cua no la $',0ah,0dh
tn3     db  0ah,0dh,'so cac bit 1 la $',0ah,0dh
n       db  ?
a       db  ?
.code
main    proc 
    ;khoi tao ds
    mov ax,@data
    mov ds,ax
    mov ah,9
    lea dx,x1
    int 21h
    ;
    mov ah,1
    int 21h
    mov bl,al    ; chuyen gia tri cua al cho bl
    mov a,al     ; chuyen gia tri al cho a
    ;
    mov ah,9     ; hien dong thong bao 2
    lea dx,x2    
    int 21h
    ;
    mov cx,8     ;khoi tao so lan lap
    mov n,0
lap:
    shl bl,1     ;dich trai bl 1 don vi
    inc n        ;tang bien dem 1 don vi
    jc  inso1    ;neu CF =1 thi nhay den inso1
    mov ah,2     ;neu khong thi in so 0
    mov dl,'0'
    int 21h
    jmp nhay
inso1:
    mov ah,2     ;ham in so 1
    mov dl,'1'
    int 21h
    ;
nhay:
    cmp n,4
    jb  tiep
    mov ah,2
    mov dl,' '
    int 21h
    mov n,0   ;cu cach 4 so thi cach 1 khoang
tiep:
    loop lap 
    ;
;ham dem so 1 trong day
    mov dl,a
    mov cx,8
    dem:
    shr dl,1     
    jc cong1
    loop    dem
    jmp thoat
;dem
cong1:
    inc n
    jmp dem
thoat:
    lea dx,tn3
    mov ah,9
    int 21h
    ;
    mov dl,n
    add dl,30h
    mov ah,2
    int 21h
    ;    
    mov ah,4ch
    int 21h
main    endp
end     main
```

###2. câu 9: <a name="2"></a>
 - viết chương trình thông báo cho người sử dụng đánh vào một ký tự và in mã ascii của ký tự dưới dạng hex ở dòng tiếp theo. lập lại cho đến khi người dùng sử dụng đánh enter
 - code

```
.stack  100h
.model  small
.data 
msg1    db  0ah,0dh,'nhap 1 ky tu $',0ah,0dh 
msg2    db  0ah,0dh,'ma ASC2 o dang hex cua ky tu do la$',0ah,0dh
.code
main    proc
    ;khoi tao ds
    mov ax,@data
    mov ds,ax
    ;hien thong bao
    laplai:
    mov ah,9
    lea dx,msg1
    int 21h
    ;nhan ky tu
    mov ah,1
    int 21h
    mov bl,al
    cmp al,0dh
    je  ketthuc
    ;hien thong bao2
    lea dx,msg2
    mov ah,9
    int 21h
    ;
    cmp bl,'a' ;so sanh bl va ky tu 'a'
    jae tiep    ; neu ky tu duoc nhap vao lon hon a thi nhay den tiep 
    sub bl,10h  ; doi thanh so
    jmp inra
tiep:
    sub bl,30h
inra:
    mov ah,2
    mov dl,bl
    int 21h
    mov dl,'h'
    int 21h
    ; lap lai
    jmp laplai
ketthuc:
    mov ah,4ch
    int 21h
main    endp
end main
```

###3. câu 10: <a name="3"></a>
 - viết chương trình thông báo cho người sử dụng đánh vào 1 số hex nhỏ hơn hay bằng 4 chữ số . đưa ra số dưới dạng nhị phân ở dòng kế tiếp, khi người dùng đưa vào 1 ký tự không hợp lệ, thông báo để họ vào lại. chương trình chỉ nhận chữ hoa.
 - code
```
.stack  100h
.model  small
.data   
msg1   db   0dh,0ah,'nhap mot so he hex A-F,0-9 va ket thuc bang enter $',0ah,0dh
msg2    db  0ah,0dh,'so nhi phan tuong ung la $',0ah,0dh
msg3    db  0ah,0dh,'nhap sai hay nhap lai $',0ah,0dh
n   db  ?
.code
main proc
    mov ax,@data
    mov ds,ax
    xor bx,bx ;xoa du lieu bx
    mov cl,4  ; so lan dich
;in ra dong nhac
    lea dx,msg1
    mov ah,9
    int 21h
;nhan gia tri ma hex
nhaplai:
    lea dx,msg3
    mov ah,9
    int 21h
    
    mov ah,1
    int 21h
while_:
    cmp al,46h
    ja  nhaplai
    cmp al,0dh    ;co phai ky tu xuong dong khong
    je  ketthuc   ; co ! ket thuc
;doi ky tu ra gia tri nhi phan
    cmp al,39h   ;kiem tra xem co la so khong
    jg  chu      ; khong phai so ! nhay den chu
; nhap 1 so
    sub al,48h   ;doi ra so nhi phan tuong ung
    jmp chen      ;dem chen vao bx
chu:
    sub al,37h
chen:
    shl bx,cl   ;danh cho gia tri moi
;dua gia tri vao bx
    or  bl,al   ;chen vao bx o 4 bit thap
    int 21h
    jmp while_
ketthuc:  
; xuat ky tu ra dang nhi phan
    mov cx,16
    mov n,0 
;in dong nhac 2
    lea dx,msg2
    mov ah,9
    int 21h
lap:
    shl bl,1     ;dich trai bl 1 don vi
    inc n        ;tang bien dem 1 don vi
    jc  inso1    ;neu CF =1 thi nhay den inso1
    mov ah,2     ;neu khong thi in so 0
    mov dl,'0'
    int 21h
    jmp nhay
inso1:
    mov ah,2     ;ham in so 1
    mov dl,'1'
    int 21h
    ;
nhay:
    cmp n,4
    jb  tiep
    mov ah,2
    mov dl,' '
    int 21h
    mov n,0   ;cu cach 4 so thi cach 1 khoang
tiep:
    loop lap 
;tro ve dos
    mov ah,4ch
    int 21h
    main    endp
end main
```

###4. câu 11 : <a name="4"></a>
 - chương trình thông báo cho người sử dụng đánh vào một số nhị phân có nhỏ hơn hay bằng 16 bit. đưa ra số dưới dạng hex ở dòng kế tiếp. khi người sử dụng đưa vào 1 ký tự không hợp lệ, thông báo để vào lại
 - code:

```
.STACK  100H
.MODEL  SMALL
.DATA
MSG1   DB   0AH,0DH,'NHAP VAO CAC SO 1,0 CUA MA NHI PHAN KET THUC BANG ENTER $',0AH,0DH
MSG2   DB   0AH,0DH,'MA HEX TUONG UNG LA $',0AH,0DH
MSG3    DB  0AH,0DH,'NHAP SAI, NHAP LAI $',0AH,0DH
N   DB 4
.CODE
MAIN    PROC
    MOV AX,@DATA
    MOV DS,AX
 ;IN DONG CHU
    LEA DX,MSG1
    MOV AH,9
    INT 21H
 ;NHAN SO NHI PHAN 
    XOR BX,BX   ;XOA BX
    jmp d1
NHAPLAI:
    LEA DX,MSG3
    MOV AH,9
    INT 21H
d1:
    ;NHAP VAO 1 KY TU
    MOV AH,1
    INT 21H 
WHILE_:
    CMP AL,0DH      ;CR?
    JE  END_WHILE   ; DUNG! KET THUC
    CMP AL,31h       ; NEU SO DO LON HON 1
    JA  NHAPLAI     ; THI NHAP LAI
    SUB AL,30H      ; CHUYEN THANH MA NHI PHAN 
    SHL BX,1        ;GIANH CHO CHO BIT MOI
    OR  BL,AL       ;CHEN GIA TRI VAO
    INT 21H
    JMP WHILE_      ;LAP LAI
END_WHILE:
    ;xuat ra so hex
    MOV Cl,4
    mov si,4
    lea dx,msg2
    mov ah,9
    int 21h
XUAT:
    cmp n,0
    JE  THOAT
    MOV DL,BH   ;CHUYEN BH CHO DL
    SHR DL,CL   ;DICH PHAI DL 4 LAN 
;CHIA TRUONG HOP
    CMP DL,9  ;NEU DL LON HON 9
    JA  CHU     ;THI CHUYEN DEN CHU
    ADD DL,48H  ; CHUYEN VE KY TU SO
    ROL BX,CL   ;QUAY BX 4 LAN BEN TRAI
    dec n
    JMP xuatkytu
CHU:    
    ADD DL,37H  ;CHUYEN VE KY TU CHU
    ROL BX,CL
    dec N 
xuatkytu:
    mov ah,2
    int 21h
    jmp xuat

thoat:
    mov ah,4ch
    int 21h 
main    endp
end main
```

###5. câu 12: <a name="5"></a>
 - viết một chương trình thông báo cho người đung đưa vào 2 số nhị phân, mỗi só có 8 chữ số, in ra màn hình ở dòng tiếp theo tổng của chúng dưới dạng nhị phân. mỗi khi người sử dụng đánh vào một ký tự không hợp lệ sẽ có thông báo yêu cầu nhập lại. mỗi số được nhận sau khi người sử dụng đánh enter.
 - code
```
.stack  100h
.model  small
.data
msg1    db  10,13,'nhap so nhi phan voi 8 bit dau tien va enter de ket thuc $',10,13
msg2    db  10,13,'nhap so nhi phan voi 8 bit dau tien va enter de ket thuc $',10,13
msg3    db  10,13,'nhap sai nhap lai cac so 1 hoac 0$',10,13
msg4    db  10,13,'tong cua 2 so la $',10,13
a   db  ?
b   db  ?
n   db  ? 
.code
main    proc
    ;khoi tao ds
    mov ax,@data
    mov ds,ax
    ;in dong nhac
    lea dx,msg1 
    mov ah,9
    int 21h
    jmp nhapso
;thong bao nhap sai 
nhapsai:
    lea dx,msg3
    mov ah,9
    int 21h        ;xoa bh
    xor bx,bx
    mov cx,8    ;khoi tao so lan dem
nhapso:
    mov ah,1
    int 21h
    cmp al,13
    je  tt
    cmp al,31h
    ja  nhapsai
    cmp al,30h
    jb  nhapsai
    sub al,30h    
    shl bh,1    ;dich trai bh 1 don vi nhuong cho cho bit moi
    or  bh,al   ;chen bit tu al vao bh
    loop   nhapso
    ; gan gia tri cua so thu 1
    mov a,bh
tt: 
    ;in dong nhac thu 2
    lea dx,msg2
    mov ah,9
    int 21h
    ;xoa bh va khoi tao bien dem
    xor bx,bx
    mov cx,8
nhap2:
     mov ah,1    ;nhan 1 so
    int 21h      ;neu do la phim enter
    cmp al,13    ;thi nhay den gan2
    je  gan2
    cmp al,31h
    ja  nhapsai
    cmp al,30h
    jb  nhapsai
    sub al,30h    
    shl bh,1    ;dich trai bh 1 don vi nhuong cho cho bit moi
    or  bh,al   ;chen bit tu al vao bh   
    loop   nhap2
gan2:
    mov b,bh    
;in dong nhac cuoi
    lea dx,msg4
    mov ah,9
    int 21h
    ;tinh toan
    xor bx,bx
    xor cx,cx
    mov bh,a    ; truyen gia tri a cho dx
    mov ch,b
    add bh,ch    ;cong a cho b va luu o dx
    ; sau thao tac nay dx chua tong a+b
    ; in ra
    mov cx,16
    mov n,0 
inra: 
    shl bh,1    ;dich trai bx di 1
    inc n  
    jc  inso1
    mov ah,2
    mov dl,'0'
    int 21h
    jmp nhay
inso1:
    mov ah,2
    mov dl,'1'
    int 21h 
nhay:
    cmp n,4
    jb  tiep
    mov ah,2
    mov dl,' '
    int 21h
    mov n,0
tiep:
    loop   inra
;thoat
    mov ah,4ch
    int 21h
main    endp
end main
```

###6. câu 13 <a name="6"></a>
 - viết chương trình thông báo cho người sử dụng đưa vào 2 số hex không dấu , in ra màn hình ở dòng tiếp theo tổng của chúng dưới dạng hex. mỗi khi người sữ dụng đánh vào một ký tự không hợp lệ sẽ có thông báo nhập lại. chương trình của bạn khả năng kiểm soát tràn không dấu. mỗi số được nhận sau khi nhấn enter.
 - code
```
.stack  100H
.model  small
.data
msg1    db  10,13,'danh vao 1 so hex khong dau va ket thuc bang enter$',10,13
msg2    db  10,13,'nhap so thu 2 ket thuc bang enter$',10,13
msg3    db  10,13,'tong cua chung la$',10,13
msg4    db  10,13,'nhap sai, nhap lai$',10,13  
a   dw  ?
b   dw  ?
n   db  0
.code
main    proc
    ;khoi tao dss
    mov ax,@data
    mov ds,ax
    ;hien thi dong nhac
    lea dx,msg1
    mov ah,9
    int 21h
    jmp nhap1so
nhapsai:
    lea dx,msg4
    mov ah,9
    int 21h
    xor bx,bx   ;xoa bx
    mov cl,4
nhap1so:    
    mov ah,1
    int 21h
nhap1:  
    ;kiem tra
    cmp n,4
    je  gan1
    cmp al,13   ;co dung la enter khong
    je  gan1   ;dung! nhay den nhap so 2
    cmp al,46h  ;kien tra xem co phai ky tu dung khong
    ja  nhapsai ; ky tu sai nhay den nhap lai
    cmp al,39h  ;kiem tra xem al co phai chu khong
    ja  kytu     ; neu >39 thi la chu nhay den chu
    sub al,30h  ;chuyen so thanh ma nhi phan tuong ung
    int 21h
    jmp chen1
kytu:
    sub al,37h  ;chuyen chu thanh ma nhi phan tuong ung
chen1:
    shl bx,cl   ;dich bx di 4 lan ve ben trai
    or  bl,al   ;chen gia tri al vao 4 bit thap cua bx
    ;nhan tiep
    int 21h
    inc n
    jmp nhap1   ; lap lai den khi enter           
gan1:
    mov a,bx
    ;xoa bx va lam lai
    xor bx,bx
    ;in dong nhac2
    lea dx,msg2
    mov ah,9
    int 21h
    jmp nhap2so
nhapsai2:
    lea dx,msg4
    mov ah,9
    int 21h
nhap2so:     
    mov ah,1
    int 21h   
nhap2:
    
     ;kiem tra
    cmp n,0
    je  gan2
    cmp al,13   ;co dung la enter khong
    je  gan2   ;dung! nhay den nhap so 2
    cmp al,46h  ;kien tra xem co phai ky tu dung khong
    ja  nhapsai2 ; ky tu sai nhay den nhap lai
    cmp al,39h  ;kiem tra xem al co phai chu khong
    ja  kytu2     ; neu >39 thi la chu nhay den chu
    sub al,30h  ;chuyen so thanh ma nhi phan tuong ung
    jmp chen2
kytu2:
    sub al,37h  ;chuyen chu thanh ma nhi phan tuong ung
chen2:
    shl bx,cl   ;dich bx di 4 lan ve ben trai
    or  bl,al   ;chen gia tri al vao 4 bit thap cua bx
    ;nhan tiep
    int 21h
    dec n
    jmp nhap2   ; lap lai den khi enter   
    
gan2:
     mov b,bx 
;tinh toan
    add bx,a     ;con a cho b va luu o bx
    jo  thoat    ;neu tran thi thoat
    ;neo khong tran ! tiep tuc
    ;xuat ra so hex
    mov n,4
    MOV Cl,4
    mov si,4
    lea dx,msg3
    mov ah,9
    int 21h
XUAT:
    cmp n,0
    JE  THOAT
    MOV DL,BH   ;CHUYEN BH CHO DL
    SHR DL,CL   ;DICH PHAI DL 4 LAN 
;CHIA TRUONG HOP
    CMP DL,9  ;NEU DL LON HON 9
    JA  CHU     ;THI CHUYEN DEN CHU
    ADD DL,48  ; CHUYEN VE KY TU SO
    ROL BX,CL   ;QUAY BX 4 LAN BEN TRAI
    dec n
    JMP xuatkytu
CHU:    
    ADD DL,37H  ;CHUYEN VE KY TU CHU
    ROL BX,CL
    dec N 
xuatkytu:
    mov ah,2
    int 21h
    jmp xuat

thoat:
    mov ah,4ch
    int 21h 
main    endp
end main
```

###7. câu 14 <a name="7"></a>
 - viết một chương trình thông báo cho người sử dụng vào một chuỗi số các chữ số thập phân và kết thúc bằng enter, in ra màn hình ở dòng tiếp theo tổng của chúng ở dạng hex
```
.stack  100h
.model  small
.data   
msg1    db  10,13,'nhap chuoi so tu 0 den 9 ket thuc bang enter $',10,13
msg2    db  10,13,'tong cua chuoi la$',10,13
msg3    db  10,13,'nhap sai hay nhap lai$',10,13
a   dw  ?
.code
main    proc
    mov ax,@data
    mov ds,ax
    ;in thong bao
    lea dx,msg1
    mov ah,9
    int 21h
    mov cl,4
    jmp nhap1so
sai:
    lea dx,msg3
    mov ah,9
    int 21h
nhap1so:
    xor bx,bx
    mov a,4    
nhapso:
    mov ah,1
    int 21h
    ;kiem tra
    cmp al,13
    je  innhac
    cmp al,39h  ;lon hon 9 hay khong
    ja  sai     ;nhay den sai
    sub al,30h  ;chuyen ve ma nhi phan
    add bx,ax   ; cong ax, vao bx
    jo  ketthuc ;neu tran se nhay
    jmp nhapso
innhac:
    lea dx,msg2
    mov ah,9
    int 21h
inra:    
    cmp a,0
    JE  ketthuc
    MOV DL,BH   ;CHUYEN BH CHO DL
    SHR DL,CL   ;DICH PHAI DL 4 LAN 
;CHIA TRUONG HOP
    CMP DL,9  ;NEU DL LON HON 9
    JA  CHU     ;THI CHUYEN DEN CHU
    ADD DL,48  ; CHUYEN VE KY TU SO
    ROL BX,CL   ;QUAY BX 4 LAN BEN TRAI
    JMP xuatkytu
CHU:    
    ADD Dl,37H  ;CHUYEN VE KY TU CHU
    ROL BX,CL
xuatkytu:
    mov ah,2
    int 21h 
    dec a
    jmp inra

ketthuc:
    mov ah,4ch
    int 21h
main    endp
end main
```
