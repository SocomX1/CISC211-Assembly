```assembly
section .text
    global _start

_start:
    mov ecx,1
    jmp initDifferenceArray;    populate difference array with values

initDifferenceArray:
    mov eax,salesArray;         set eax to address of salesArray  
    mov edx,ecx;                ecx represents index of SUBSEUQNT salesArray element
    imul edx,4;                 edx represents offset (x4 for dd) of SUBSEQUENT salesArray element

    mov ebx,DWORD[eax+edx];     move SUBSEQUENT salesArray element to ebx
    sub edx,4;                  subtract 4 from edx to calculate offset of CURRENT salesArray element
    sub ebx,DWORD[eax+edx];     subtract current salesArray element from SUBSEQUENT salesArray element to calculate difference
    
    mov eax,diffArray;          set eax to address of diffArray
    mov DWORD[eax+edx],ebx;     set leftmost uninitialized element of diffArray to ebx (previously calculated difference)

    inc ecx;                    increment index of salesArray
    cmp ecx,11                 
    jle initDifferenceArray;    if index of salesArray <= 11, next iteration

    xor ebx,ebx
    xor ecx,ecx
    xor edx,edx

    jg beginNextIteration

    ;first iteration:
    ;ebx set to difArray[0]
    ;currentStartingMonth = 2 (1 + 1)
    ;ecx set to 3 (currentStartingMonth + 1), and represents current ending month
    beginNextIteration:
        mov edx,diffArrayStartAddress
        add edx,[arrayOffset]
        mov ebx,DWORD[edx]

        mov eax,edx
        add eax,4

        mov edx,[currentStartingMonth]
        inc edx
        mov [currentStartingMonth],edx

        inc edx
        mov ecx,edx

        jmp sumDifferences

        ;ebx acts as summation
        sumDifferences:
            add ebx,DWORD[eax];             summation

            cmp ebx,[currentLargestSum]

            jg updateLargestSum
            jle checkForNextElement

            updateLargestSum:
                mov [currentLargestSum],ebx

                mov edx,[currentStartingMonth]
                mov [highestStartingMonth],edx

                mov [highestEndingMonth],ecx
                jmp checkForNextElement

            checkForNextElement:
                inc ecx
                add eax,4;                      traversal

                ;short circuit eval
                cmp eax,diffArrayEndAddress
                jle sumDifferences
                jg removeElement

                ;reset eax to address of new first diffArray element
                removeElement:
                    mov edx,[arrayOffset]
                    add edx,4
                    mov [arrayOffset],edx

                    cmp edx,40
                    jl beginNextIteration
                    jge exit

exit:
    mov eax,[highestStartingMonth]
    mov ebx,[highestEndingMonth]
    mov ecx,[currentLargestSum]
    mov eax,1
    int 0x80

section .data
    salesArray dd 100,113,110,85,81,101,94,106,105,102,86,63

    diffArray dd 0,0,0,0,0,0,0,0,0,0,0
    diffArrayEndAddress equ $ - 4;address of second to last element
    diffArrayStartAddress equ diffArray

    arrayOffset dd 0
    currentStartingMonth dd 1

    ;currentLargestSum dd 0

segment .bss
    currentLargestSum resb 4
    highestStartingMonth resb 4
    highestEndingMonth resb 4
```
