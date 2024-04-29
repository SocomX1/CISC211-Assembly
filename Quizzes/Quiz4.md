```assembly
section .text
    global _start

_start:
    push DWORD[x];          push initialized variables as function arguments
    push DWORD[y]
    push DWORD[z]

    call _sumIntegers
    jmp exit

_sumIntegers:
    push ebp
    mov ebp,esp

    mov eax,DWORD[ebp+8];   sum function arguments in eax register
    add eax,DWORD[ebp+12]
    add eax,DWORD[ebp+16]

    mov [result],eax;       save function return value in uninitialized variable

    leave
    ret

exit:
    mov eax,1
    int 0x80

section .data
    x dd 3
    y dd 6
    z dd 9

segment .bss
    result resb 4
```
