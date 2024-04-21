```assembly
section .text
    global _start

_start:
	mov ecx,[currentAsciiIndex]; initialize ecx to first desired ASCII corrector
	jmp iterate; begin iterating

iterate:
	cmp ecx,[finalAsciiIndex]

	jle incrementAndPrint; if current index less than target index, increment index and print ASCII corrector 
	call exit; else, exit program

incrementAndPrint:
	mov ecx,currentAsciiIndex
	call output; print ASCII corrector

	mov ecx,newline
	call output; print newline

	mov ecx,[currentAsciiIndex]
	inc ecx
	mov [currentAsciiIndex],ecx; increment and save value of currentAsciiIndex

	jmp iterate; loop

output:
	mov eax,4
	mov ebx,1
	mov edx,1
	int 0x80
	ret

exit:
 mov eax,1
 int 0x80

section .data
	currentAsciiIndex dd 65; ASCII decimal value of first corrector to print
	finalAsciiIndex dd 90; ASCII decimal value of last corrector to print
	newline dd 10; ASCII decimal value of newline corrector
```
