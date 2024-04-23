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
    mov ecx,1
    jg iterateDifferences;      else reset ebx to 0, ecx to 1, and begin calculating sums

    iterateDifferences:;            only called once to establish initial highest sum
        mov ebx,DWORD[eax];         set ebx to value of first diffArray element
        add eax,4;                  point eax to address of second diffArray element
        add ebx,DWORD[eax];         add value of second diffArray element to ebx

        jmp updateLargestSum;       starting sum is largest by default, so update cooresponding variables

            updateLargestSum:;                  called whenever new largest sum is found
                mov [currentLargestSum],ebx;    update variable representing value of the current largest sum
                mov [highestEndingMonth],ecx;     
                mov edx,[currentStartingMonth]; set highest starting month to current starting month
                mov [highestStartingMonth],edx
                jmp checkSums

                checkSums:
                    cmp eax,diffArrayEndAddress
                    jge shrinkArray

                    inc ecx
                    add eax,4
                    add ebx,DWORD[eax]

                    cmp ebx,[currentLargestSum]
                    jg updateLargestSum
                    jmp checkSums

                    shrinkArray:
                        mov edx,[arrayOffset]
                        add edx,4
                        mov [arrayOffset],edx

                        mov edx,diffArrayStartAddress
                        add edx,[arrayOffset]
                        cmp edx,diffArrayEndAddress

                        jge exit

                        mov eax,edx

                        mov edx,[currentStartingMonth]; increment current starting month
                        inc edx
                        mov [currentStartingMonth],edx
                        mov ecx,[currentStartingMonth]

                        mov ebx,DWORD[eax]
                        add eax,4
                        add ebx,DWORD[eax]
                        jmp checkSums

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

segment .bss
    currentLargestSum resb 4
    highestStartingMonth resb 4
    highestEndingMonth resb 4
```
