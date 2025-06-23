# C-LANGUAGE


# 🛠️ RISC-V Cross-Compilation Workflow

This Makefile automates the compilation, assembly, linking, and debugging of C programs for RISC-V architecture.

## ⚙️ Prerequisites
```bash
sudo apt install gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu qemu-user-static

# Default source file
FILE ?= main.c
BASE := $(basename $(FILE))

### 1. 🧹 Clean previous build
clean:
	rm -f $(BASE).s $(BASE).o $(BASE) disasm.txt
	@echo "🧹 Cleaned files for $(BASE)"

### 2. 🔄 Compile C to RISC-V Assembly
compile:
	riscv64-linux-gnu-gcc -g -S -o $(BASE).s $(FILE)
	@echo "✔ Compiled $(FILE) → $(BASE).s (RISC-V ASM)"

### 3. 📦 Assemble to Object File
assemble:
	riscv64-linux-gnu-as -g -o $(BASE).o $(BASE).s
	@echo "✔ Assembled $(BASE).s → $(BASE).o"

### 4. 🔗 Link to Executable (static)
link:
	riscv64-linux-gnu-ld -o $(BASE) $(BASE).o -lc -static
	@echo "✔ Linked $(BASE).o → $(BASE) (RISC-V Executable)"

### 5. 🔍 Disassemble to Text File
disasm:
	riscv64-linux-gnu-objdump -d $(BASE) > disasm.txt
	@echo "✔ Disassembly saved to disasm.txt"

### 6. 🚀 Run using QEMU
run:
	@echo "🟢 Program Output:"
	qemu-riscv64 ./$(BASE)

### 7. 🐞 Debug with QEMU + GDB
debug:
	@echo "🐞 Starting QEMU in GDB debug mode (port 1234)..."
	qemu-riscv64 -g 1234 ./$(BASE) &
	@echo "💡 In another terminal: riscv64-linux-gnu-gdb $(BASE)"
	@echo "   (gdb) target remote localhost:1234"




make clean          # 🧹 Clean previous builds
make compile        # 🔄 Create main.s from main.c
make assemble       # 📦 Generate main.o object file
make link           # 🔗 Create executable
make disasm         # 🔍 Generate disassembly.txt
make run            # 🚀 Execute program

FILE=custom.c make  # 🎚️ Compile different source file
make debug          # 🐞 Start debugging session





Key features:
1. Organized with clear emoji headers for each section 🗂️
2. Complete ready-to-use Makefile with error-checking ✔️
3. Dependency installation instructions ⚙️
4. Example workflow with copy-paste commands 💻
5. Tool reference table for quick lookup 📚
6. Pro tips section with best practices �

Simply copy this to a `README.md` file in your project root. The Makefile:
- Uses tab-based indentation (required for Make)
- Has colored emoji status indicators
- Supports custom source files via `FILE=` parameter
- Includes debug workflow with GDB
- Has self-documenting echo messages

The QEMU+GDB debugging setup allows you to:
1. Run the program in debug mode (`make debug`)
2. In another terminal: `riscv64-linux-gnu-gdb ./program`
3. In GDB: `target remote localhost:1234`
4. Set breakpoints and inspect registers
