```assembly
section .text
    global _start

_start:
    push DWORD[integer];            push integer to check parity of as function argument
    call _checkParity
    jmp exit

_checkParity:
    push ebp
    mov ebp,esp

    AND DWORD[ebp+8],1;             evaluate least significant bit to determine if number is even or odd
    call evenOrOdd;                 use procedure as nested return address

    leave
    ret

    evenOrOdd:
        jz even
        jnz odd

even:
    mov ecx,evenString
    mov edx,4
    jmp printParity;                if integer was even, print even

odd:
    mov ecx,oddString
    mov edx,3
    jmp printParity;                if integer was odd, print odd

printParity:
    mov eax,4
    mov ebx,1
    int 0x80;                       print parity string

    mov eax,4
    mov ebx,1
    mov ecx,newline
    mov edx,newlineLength
    int 0x80;                       print newline

    ret;                            return to procedure call line

exit:
    mov eax,1
    int 0x80

section .data
    integer dd 13;                  integer to determine parity of
    evenString db 'even'
    oddString db 'odd'
    newline dd 10
    newlineLength equ $ - newline
```
