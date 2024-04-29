```assembly
section .text
    global _start

_start:
    push DWORD[counterMax]
    call _countIterative

    xor ecx,ecx

    push DWORD[counterMax]
    call _countRecursive

    jmp exit

_countIterative:
    push ebp
    mov ebp,esp

    call evaluateCounter

    leave
    ret

    evaluateCounter:
        cmp ecx,DWORD[ebp+8]

        jl incrementCounter
        jge terminateCounter

        terminateCounter:
            ret

        incrementCounter:
            inc ecx
            jmp evaluateCounter

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
    counterMax dd 3
```
