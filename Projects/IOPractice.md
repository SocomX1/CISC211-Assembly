Creates a file within a subdirectory, writes to the file, then reads from the file and prints what was read to console.
```assembly
section .text
    global _start

_start:
    ;directory create
    mov eax,39
    mov ebx,dirName
    mov ecx,0755o
    int 0x80

    jmp generateFile

generateFile:
    ;file create
    mov eax,8
    mov ebx,fileName
    mov ecx,0711o
    int 0x80

    ;file open
    mov ecx,2;                      read/write
    call openFile

    mov [fileDescriptor],eax

    ;file write
    mov eax,4
    mov ebx,[fileDescriptor]
    mov ecx,contents
    mov edx,contentsLength;         number of bytes written
    int 0x80

    jmp readAndPrint

    readAndPrint:
        ;lseek (reset cursor to beginning of file)
        mov eax,19
        mov ebx,[fileDescriptor]
        mov ecx,0
        mov edx,0
        int 0x80

        ;file read
        mov eax,3
        mov ebx,[fileDescriptor]
        mov ecx,readBuffer
        mov edx,bufferLength
        int 0x80

        ;eax now contains the length of the read data

        ;stdout print
        mov DWORD[ecx+eax],0xa;          concatenate input buffer and newline corrector
        mov eax,4
        mov ebx,1
        mov ecx,readBuffer
        mov edx,bufferLength
        int 0x80

        ;file close
        call closeFile

        jmp exit

openFile:
    ;file open
    mov eax,5
    mov ebx,fileName
    ;ecx must be specified prior to call
    mov edx,0711o
    int 0x80
    ret

closeFile:
    ;file close
    mov eax,6
    mov ebx,[fileDescriptor]
    int 0x80   
    ret 

exit:
    mov eax,1
    int 0x80

section .data
    dirName db 'IO',0h
    fileName dd 'IO/IOFile.txt',0h
    contents db 'abcdefghijklmnopqrstuvwxyz',0h
    contentsLength equ $ - contents

    bufferLength equ 128

segment .bss
    fileDescriptor resb 4
    readBuffer resb 128
```
