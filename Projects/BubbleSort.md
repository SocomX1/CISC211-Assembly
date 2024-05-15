```assembly
section .text
    global _start

_start:
    mov eax,integerArray
    jmp pushArrayElements

postSort:
    cmp edx,1
    je _start

    mov eax,integerArray
    jne verifySort

pushArrayElements:
    push DWORD[eax]

    add eax,4
    cmp eax,integerArrayEndAddress

    jl pushArrayElements
    call _sortArray

    jmp postSort

_sortArray:
    push ebp
    mov ebp,esp

    mov eax,integerArray
    xor edx,edx
    call compareAdjacent

    leave
    ret

    compareAdjacent:
        cmp eax,finalElementAddress
        jge terminate

        mov ebx,[eax]
        mov ecx,[eax+4]

        cmp ebx,ecx
        jg swapAdjacent
        jle incrementIndex

        swapAdjacent:
            mov [eax],ecx
            mov [eax+4],ebx
            mov edx,1
            jmp compareAdjacent

        incrementIndex:
            add eax,4
            jmp compareAdjacent

        terminate:
            ret

verifySort:
    mov edx,[eax]
    add eax,4

    cmp eax,finalElementAddress
    jg exit
    jle verifySort

exit:
    mov eax,1
    int 0x80

section .data
    integerArray dd 3,2,-4,5,10,7,-8; to be sorted in ascending order
    integerArrayEndAddress equ $
    finalElementAddress equ $ - 4
```
