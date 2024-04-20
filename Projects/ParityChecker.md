```assembly
section .text
    global _start

_start:
    push DWORD[integer]
    call _checkParity
    jmp exit

_checkParity:
    push ebp
    mov ebp,esp

    AND DWORD[ebp+8],1;             evaluate least significant bit to determine if number is even or odd
    call evenOrOdd

    leave
    ret

    evenOrOdd:
        jz even
        jnz odd

even:
    mov ecx,evenString
    mov edx,4
    jmp printParity

odd:
    mov ecx,oddString
    mov edx,3
    jmp printParity

printParity:
    mov eax,4
    mov ebx,1
    int 0x80

    mov eax,4
    mov ebx,1
    mov ecx,newline
    mov edx,newlineLength
    int 0x80

    ret

exit:
    mov eax,1
    int 0x80

section .data
    integer dd 13
    evenString db 'even'
    oddString db 'odd'
    newline dd 10
    newlineLength equ $ - newline
```
