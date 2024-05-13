```bash
#!/bin/bash
param=$1
substring="${param:12}"
substring=${substring,,}

rm counter_$substring.txt
touch counter_$substring.txt

nasm -f elf ./$1.asm
ld -m elf_i386 ./$1.o -o ./$1

{ time ./$1; } 2> counter_$substring.txt

cat counter_$substring.txt

#gdb -x gdbCommands.txt ./$1
```
