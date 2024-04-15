```assembly
section .text
    global _start

_start:
    push DWORD[factorialBase]
    mov eax,[factorialBase]
    call _recursiveFactorial
    jmp exit

_recursiveFactorial:
    push ebp
    mov ebp,esp
;    sub esp,4

    cmp DWORD[ebp+8],1
    je terminateFunction

    dec DWORD[ebp+8]
    imul eax,DWORD[ebp+8]

    push DWORD[ebp+8]
    call _recursiveFactorial

    mov [factorialValue],eax

    leave
    ret

terminateFunction:
    leave
    ret  

exit:
    mov eax,1
    int 0x80

section .data
    factorialBase dd 5

segment .bss
    factorialValue resb 4
    
```
