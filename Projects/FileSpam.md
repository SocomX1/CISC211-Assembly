This program executes a loop to create a specified amount of empty text files.
```assembly
section .text
    global _start

_start:
    ;begin directory create operation
    mov eax,39;                 system call mkdir
    mov ebx,dirName;            name of directory to create
    mov ecx,0755o;              drwxr-xr-x
    int 0x80
    ;end directory create operation

    jmp generateNextFile

 generateNextFile:
    inc edx;                    increment index first since the initial file should be numbered as 1
    push edx;                   _integerToString modifies edx, so save it

    mov eax,edx;                move current index value to eax, since eax will be converted to a string via the _integerToString function
    call _integerToString;      convert current index value to string in order to append to the end of file name

    pop edx;                    recall edx value from before call to _integerToString

    mov ebx,baseFileName
    mov DWORD[ebx+9],eax;       append string in eax (value of edx but in string format) to end of base file name (see big-endianness: https://en.wikipedia.org/wiki/Endianness)

    ;begin file create operation
    mov eax,8;                  file handling system call sys_creat
    mov ecx,0711o;              -rwx--x--x
    int 0x80
    ;end file create operation

    cmp edx,[filesToCreate];    if current index is equal to specified number of files to create:
    jge exit;                   exit program
    jmp generateNextFile;       else, iterate

_integerToString:
    mov esi,emptyString;        the esi (extended source index) register is used, as its purpose is to serve as a pointer for string and memory array copying
    add esi,9;                  point esi to the end of the empty string buffer
    mov BYTE[esi],0;            insert 0 to denote end of string

    mov ebx,10;                 all digits of eax will be divided by 10 (effectively digit modulo 10)  

    convertNextDigit:
        xor edx,edx;            reset edx

        div ebx;                divide eax by ebx (10), remainder will be equal to the least significant digit of eax
        add dl,'0';             convert least significant digit to string         
        dec esi;                move esi pointer to the left
        mov [esi],dl;           insert converted character from right to left

        test eax,eax;           determine if all digits have been converted (eax = 0)
        jnz convertNextDigit;   if not, iterate

        mov eax,[esi];          save fully converted string in eax
        ret

exit:
    mov eax,1
    int 0x80
  
section .data
    filesToCreate dd 33;        number of empty files to create, numbered 1 through the value of this variable
    dirName db 'spam',0h;       must alter baseFileName AND ebx offset if you modify this
    baseFileName dt 'spam/spam';must start with the same string as dirName, must also alter ebx offset if you modify this   

segment .bss
    emptyString resb 10;        blank string used as a buffer to convert integer to string
```
