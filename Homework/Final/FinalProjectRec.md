```assembly
section .text
    global _start

_start:
    jmp analyzeRecursiveCounter
    
analyzeRecursiveCounter:
    push DWORD[counterMax]
    call _countRecursive

    jmp exit

_countRecursive:
    push ebp
    mov ebp,esp

    cmp ecx,DWORD[ebp+8]
    jge terminateRecursion

    inc ecx

    push DWORD[ebp+8]
    call _countRecursive

    leave
    ret

    terminateRecursion:
        leave
        ret

exit:
    mov eax,1
    int 0x80

section .data
    counterMax dd 20000
```
