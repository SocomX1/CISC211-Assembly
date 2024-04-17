This program executes a loop to create a specified amount of empty text files.
```assembly
section .text
    global _start

_start:
    mov edx,[filesToCreate]
    mov ebx,fileName
    jmp iterate

    jmp exit

iterate:
    cmp edx,0
    jle exit

    ;begin file create operation
    mov eax,8;          file handling system call sys_creat
    mov ecx,0771o;      file permissions to set
    int 0x80
    ;end file create operation

    dec edx;            decrement loop index
    add ebx,4;          point address of ebx to 5th character in the string
    add DWORD[ebx],1;   increment decimal value of 5th character in string
    sub ebx,4;          return address of ebx to 1st character in the string

    jmp iterate

exit:
    mov eax,1
    int 0x80

section .data
    fileName db 'spam1.txt',0h
    filesToCreate dd 9
```
