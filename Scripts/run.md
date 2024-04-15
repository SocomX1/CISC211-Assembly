```bash
#!/bin/bash
nasm -f elf ./$1.asm
ld -m elf_i386 ./$1.o -o ./$1

./$1

gdb -x gdbCommands.txt ./$1
```
