# C-LANGUAGE





# ///////for *86 ////////////

# Clean previous build

rm -f main.o main.s main

# Convert C to Assembly
gcc -S main.c -o main.s

# Convert Assembly to Object
as main.s -o main.o

# Link and create Executable
gcc main.o -o main

# Run the program
./main



# output check from memory

objdump -d mian.o or main etc




# text file banane ke liye 
 
objdump -d main> disasm.txt
// objdump : linux tool to inspect binaries (object/executable files)
// -d diassembler :  machine code to assembly code conveter 
// > is use to output with out terminat to save in new file 
// disasm.txt output ko text file to save


# coloure full output 

mv disasm.txt disasm.s
 




# ////////////// for riscv64 //////////////////



# Clean previous build

rm -f main.s main.o main disasm.txt


# Convert C to Assembly
riscv64-linux-gnu-gcc -S -o main.s main.c

# Convert Assembly to Object
riscv64-linux-gnu-as -o main.o main.s

# Link and create Executable
riscv64-linux-gnu-ld -o main main.o -lc -static

# Run the program
qemu-riscv64 ./main

# disaseemble  for text file
riscv64-linux-gnu-objdump -d main > disasm.txt









::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

# Default value (if FILE not provided)
FILE ?= main.c

# Get filename without extension
BASE := $(basename $(FILE))

# 1. Compile C to Assembly (RISC-V)
compile:
	riscv64-linux-gnu-gcc -g -S -o $(BASE).s $(FILE)
	@echo "✔ Compiled $(FILE) → $(BASE).s (RISC-V ASM)"

# 2. Assemble to Object File (RISC-V)
assemble:
	riscv64-linux-gnu-as -g -o $(BASE).o $(BASE).s
	@echo "✔ Assembled $(BASE).s → $(BASE).o"

# 3. Link to Executable (RISC-V static)



link:
	riscv64-linux-gnu-ld -o $(BASE) $(BASE).o -lc -static
	@echo "✔ Linked $(BASE).o → $(BASE) (RISC-V Executable)"

# 4. Disassemble
disasm:
	riscv64-linux-gnu-objdump -d $(BASE) > disasm.txt
	@echo "✔ Disassembly saved to disasm.txt"

# 5. Run using QEMU
run:
	@echo "🟢 Output (via QEMU):"
	qemu-riscv64 ./$(BASE)

# 6. Debug using QEMU + GDB
debug:
	@echo "🐞 Starting QEMU in GDB debug mode..."
	qemu-riscv64 -g 1234 ./$(BASE) & \
	echo "💡 Now in another terminal, run: riscv64-linux-gnu-gdb $(BASE)" && \
	echo "   Then inside gdb: target remote localhost:1234"

# 7. Clean
clean:
	rm -f $(BASE).s $(BASE).o $(BASE) disasm.txt
	@echo "🧹 Cleaned files for $(BASE) (RISC-V)"
