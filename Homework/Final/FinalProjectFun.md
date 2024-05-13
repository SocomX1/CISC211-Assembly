```assembly
section .text
    global _start

_start:
    jmp analyzeIterativeCounter
    
analyzeIterativeCounter:
    push DWORD[counterMax]
    call _countIterative

    jmp exit

_countIterative:
    push ebp
    mov ebp,esp

    xor ecx,ecx
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

exit:
    mov eax,1
    int 0x80

section .data
    counterMax dd 20000
```
