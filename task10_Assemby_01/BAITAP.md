TASK 11 (ASESMBLY)

người thực hiện : Nguyễn Văn HIệp

cập nhật lần cuối : 11/1/2017

> --------------------------
###mục lục
- [1. câu 7a](#7a)
- [2. câu 7b](#7b)
- [3. câu 8](#8)
- [4. câu 9](#9)
- [5. câu 10](#10)
- [6. câu 11](#11)
- [7. câu 12](#12)
- [hết](#het)

###câu 7A<a name="7a"></a> 
```
title cau7A
.model small
.stack 100h
.data
.code
main proc
    mov   AH,2        ; am hien thi ki tu
    mov   DL,'?'      ;ky tu la ?
    int   21H         ; hien thi dau ?
 ;vao 1 ky tu

    mov   AH,1        ; ham nhan 1 ky tu tu phim
    int   21H         ; ky tu duoc luu trong AL
    mov   BL,AL       ; luu gia tri cua AL trong BL

; tab ra 
    mov   AH,2        ; ham hien thi 1 ky tu
    mov   DL,09H      ;cach ra mot dau tab
    int   21H         ;thuc hien viec cach ra
; hien thi ky tu
    mov   AH,2
   mov    DL,BL       ; lay ky tu cua AL luc dau da duoc luu o BL
    int   21h         ; hien thi no
;tro ve DOS
    mov   AH,4CH       ;ham thoat ra khoi chuong trinh
    int   21H          ;thoat ra tro ve DOS
    main  endp
end     main
```
###CÂU 7B <a name="7b"></a>


```
title baitap7b
.model
.stack 100h
.data 
msg1 db 'nhap vao 1 chu hoa: $'
msg2 db 09h,'chu cai thuong la:'
char db ?,'$'
.code
main proc
; khoi tao DS
     mov   ax,@data
     mov    ds,ax
;in dong nhac nguoi su dung
     lea    dx,msg1     ; lay dia chi cua msg1 cho dx
     mov    AH,9        ; ham hien thi chuoi, choi do lay tu DX
     int    21h         ;thuc hien no
; vao mot ky tu và doi thanh chu thuong
     mov    ah,1        ;ham doc 1 ky tu
     int    21h         ; bat dau doc va luu no vao AL
     add    al,20h      ;doi chu ha thanh chu thuong
     mov    char,al     ; luu ky tu do vao 
     lea    dx,msg2
     mov    ah,9        ; hien thi chu
     INT    21H         ;ham hien thi msg2 va chu thuong
;tro ve DOS
     mov    ah, 4ch
     int    21h
     main   endp
end          main
```

###CÂU 8 <a name="8"></a>

```
title     bt8
.model    small
.stack    100h
.data
msg1   db    'nhap 1 so co 2 chu so ab, a+b<10 !$'
msg2   db    0dh,0ah,'tong cua 2 so la:$'
char   db    ?
a      dw    ?
b      dw    ?
.code
main proc
;khoi tao data ds
    mov     ax,@data
    mov     ds,ax 
;in dau ?
    mov      ah,2      ; ham xuat 1 ky tu
    mov      dl,'?'    ; ky tu la dau ?
    int      21h
;in dong nhac
    lea      dx,msg1    ; lay thong bao msg1
    mov      ah,9       ;ham hien thi
    int      21h        ; hien thi thong bao
;vao so dau tien
    mov      ah,1       ; ham vao 1 ky tu
    int      21h        ; doc ky tu do 
    sub      al,30h     ; chuyen chu thanh so
    mov      a,ax       ; truyen gia tri do cho a
;vao so thu 2
    mov      ah,1       ; ham vao 1 ky tu
    int      21h        ; nhan ky tu do
    sub      ax,30h     ; chuyen dl thanh so
    mov      b,ax       ; truyen ax cho b  
hien dong 2
    lea      dx,msg2    ; nap msg2 cho dx
    mov      ah,9 
    int      21h 
;cong
    mov      ax,a
    add      ax,b
    MOV      A,Ax
; hien thi tong 2 so
    mov      dx,ax
    add      dl, 30h
    mov      ah,2
    int      21h
; tro ve dos
     mov     ah,4ch
     int     21h
     main       endp
end             main
```

###câu 9<a name="9"></a>

```
title cau9
.stack 100h
.model small
.data 
msg1 dw 'nhap vao 3 chu cai dau cua ten$'
m1 db 0dh,0ah,'$' 
a db ?,'$'
b db ?,'$'
c db ?,'$'
.code
main proc 
    mov ax,@data
    mov ds,ax
;in dong nhac
    lea dx,msg1 
    mov ah,9 
    int 21h
;nhan vao chu cai
    mov ah,1 ; ham nhan 1 ky tu
    int 21h ;nhan
    mov a,al
    mov ah,1 ;nhan ky tu thu 2
    int 21h  ;nhan
    mov b,al  ; gan no cho b
    mov ah,1
    int 21h 
    mov c,al
; in ra
    lea dx,m1 
    mov ah,9
    int 21h
;in ky tu 1
    lea dx,a 
    mov ah,9
    int 21h
    lea dx,m1
    mov ah,9
    int 21h
;in ky tu 2
    lea dx,b
    mov ah,9
    int 21h
    lea dx,m1
    mov ah,9
    int 21h
;in ky tu 3
    lea dx,c
    mov ah,9
    int 21h
    lea dx,m1
    mov ah,9
    int 21h
 ;tro ve dos
    mov    ah,4ch
    int 21h
 main   endp
end  main    
```

###câu 10<a name="10"></a>

```
.MODEL SMALL
.STACK 100h
.DATA
MSG DB 'NHAP KY TU  (A - F): $'
MSG1 DB 0DH,0AH,'SO DO LA: $'
MSG10 DB '10$'
MSG11 DB '11$'
MSG12 DB '12$'
MSG13 DB '13$'
MSG14 DB '14$'
MSG15 DB '15$'
a db ?
.CODE
MAIN PROC
    MOV AX,@DATA
    MOV DS,AX
;Thong bao nhap
    LEA DX, MSG
    MOV AH,9
    Int 21h
;Nhap
    Mov ah, 1
    int 21h;
    mov a, al
;thong bao xuat
    LEA Dx,msg1
    mov ah,9
    int 21h
;kiemtra
    mov al, a
;
    cmp al, 65
    je In10
;
    cmp al, 66
    je In11
;
    cmp al, 67
    je In12
;
    cmp al, 68
    je In13
;
    cmp al, 69
    je In14
;
    cmp al, 70
    je In15
;IN
    In10:
    LEA dx, msg10
    mov ah,9
    int 21h
    jmp thoat
;
    In11:
    LEA dx, msg11
    mov ah,9
    int 21h
    jmp thoat
;
    In12:
    LEA dx, msg12
    mov ah,9
    int 21h
    jmp thoat
;
    In13:
    LEA dx, msg13
    mov ah,9
    int 21h
    jmp thoat
;
    In14:
    LEA dx, msg14
    mov ah,9
    int 21h
    jmp thoat
;
    In15:
    LEA dx, msg15
    mov ah,9
    int 21h
    jmp thoat
;dos
    thoat:
    mov ah,4ch
    int 21h
main endp
end main
```

###câu 11<a name="11"></a>

```
.STACK     100h
.MODEL    small
.DATA
MSG     Db    0ah,0dh,'**********$'
.code
main proc
    mov      ax,@data      ;khoi tao data
    mov      ds,ax
;khoi tao vong lap =10
     mov     cx,10
     xuat:
     lea     dx,msg
     mov     ah,9
     int     21h
     loop xuat      ; nhan loop se lap voi so lan dc luu trong cx va moi lan cx tru di 1
;tro ve dos
     mov     ah,4ch
     int     21h
;ket thuc
     main endp
end main
```

###câu 12<a name="12"></a>

```
.stack 100h
.model small
.data
msg1  dw     0dh,0ah,'***********$'
a     dw     ?,'$',0ah,0dh
b     dw     ?,'$',0ah,0dh
c     dw     ?,'$',0ah,0dh 
.code
main proc
    mov     ax,@data
    mov     ds,ax
;in ra dau ?
    mov     ah,2
    mov     dx,'?'
    int     21h
; nhan 3 ky tu và gan cho a,b,c
    mov     ah,1
    int     21h
    mov     a,ax
    ;
    mov     ah,1
    int     21h
    mov     b,ax
    ;
    mov     ah,1
    int     21h
    mov     c,ax 
;in ma tran 11x11
    mov     cx,10    ;khoi tao so lan lap
    lap:
    lea     dx,msg1
    mov     ah,9
    int     21h
    loop lap
    mov     ah,2
    mov     dl,7h
    int     21h  
;in a,b,c ra
    lea     dx,a
    mov     ah,9
    int     21h
    ;
    lea     dx,b
    mov     ah,9
    int     21h
    ;
    lea     dx,c
    mov     ah,9
    int     21h
;tra ve dos
    mov     ah,4ch
    int     21h
    main    endp
end          main

```
###hết <a name="het"></a>
