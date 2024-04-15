```assembly
section .text
	global _start

_start:
	mov eax,[currentElement]
	mov ebx,[previousElement]
	mov ecx,[remainingElements]

	loop iterate

iterate:
	add eax,[previousElement]
	mov ebx,[currentElement]

	mov [currentElement],eax
	mov [previousElement], ebx

	loop iterate
	jmp exit

exit:
	mov [targetElement], eax
	mov eax,1
	int 0x80

section .data
	remainingElements dd 25
	currentElement dd 1
	previousElement dd 0

segment .bss
	targetElement resb 4

```
