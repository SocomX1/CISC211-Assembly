```assembly
section .text
	global _start

_start:
	mov eax,intArray
	jmp pushArrayElements

pushArrayElements:
	push DWORD[eax]; push current array element to function, then increment index to next element
	add eax,4
	cmp eax,intArrayEndAddress
	jl pushArrayElements; if current array element index is less than final array element index, loop

	call _compareArrayElements; else, start comparing element values
	jmp exit

_compareArrayElements:
	push ebp
	mov ebp,esp
	sub esp,8

	;var1
	;var2
	;var3
	;...
	;varN
	;function return address
	;ebp
	;temporary function variable holding largest current integer
	;temporary function variable holding current stack search index

	mov DWORD[ebp-8],intArrayElements
	mov eax,DWORD[ebp-8]
	mov eax,DWORD[ebp+eax]; access var1 parameter and store in eax
	mov DWORD[ebp-4],eax; var1 begins as largest current integer

	call iterate

	mov eax,DWORD[ebp-4]
	mov [largestInt],eax; retrieve final largest integer from temporary function variable

	leave
	ret

iterate:
	mov eax,DWORD[ebp-8]
	sub eax,4; decrement current stack index
	mov DWORD[ebp-8],eax
	cmp eax,4
	jle terminateLoop; if no remaining function parameters to compare, exit loop

	mov eax,DWORD[ebp+eax]
	cmp DWORD[ebp-4],eax; determine if next function parameter is larger than currently stored value

	jl updateIndex; if so, update stored value to new largest number
	jge iterate; otherwise, iterate again

updateIndex:
	mov DWORD[ebp-4],eax
	jmp iterate

terminateLoop:
	ret

exit:
	mov eax,1
	int 0x80

section .data
	intArray dd -10,-5,20,1,50
	intArrayEndAddress equ $
	intArrayElements equ $ - intArray + 4

segment .bss
	largestInt resb 4

```
