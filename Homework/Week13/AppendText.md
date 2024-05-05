```assembly
section .text
    global _start

_start:
    ;file create
    mov eax,8
    mov ebx,fileName
    mov ecx,0755o
    int 0x80

    ;file open
    mov eax,5
    mov ebx,fileName
    mov ecx,1;                      write only
    mov edx,0755o
    int 0x80

    mov [fileDescriptor],eax

    ;file write
    mov eax,4
    mov ebx,[fileDescriptor]
    mov ecx,initialString
    mov edx,initialStringLength
    int 0x80

    ;lseek
    mov eax,19
    mov ebx,[fileDescriptor]
    mov ecx,0
    mov edx,2;                      end of file
    int 0x80

    ;file write (append)
    mov eax,4
    mov ebx,[fileDescriptor]
    mov ecx,appendString
    mov edx,appendStringLength
    int 0x80

    ;file close
    mov eax,6
    mov ebx,[fileDescriptor]
    int 0x80

exit:
    mov eax,1
    int 0x80

section .data
    fileName db "quotes.txt",0h

    initialString db 'To be, or not to be, that is the question.',0xa,'A fool thinks himself to be wise, but a wise man knows himself to be a fool.',0xa
    initialStringLength equ $ - initialString

    appendString db 'Better three hours too soon than a minute too late.',0xa,'No legacy is so rich as honesty.'
    appendStringLength equ $ - appendString

segment .bss
    fileDescriptor resb 1
```
