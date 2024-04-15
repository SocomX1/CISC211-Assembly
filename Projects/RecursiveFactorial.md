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

    cmp DWORD[ebp+8],2; base case is reached when current index is 2
    je terminateFunction

    ; begin recursive case
    dec DWORD[ebp+8]
    imul eax,DWORD[ebp+8]

    push DWORD[ebp+8]
    call _recursiveFactorial
    ; end recursive case

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
