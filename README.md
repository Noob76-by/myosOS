
# myosOS v0.1.0
> A Minimalist 32-bit Monolithic-style Research Kernel Built from Scratch

myosOS is a low-level, bare-metal monolithic kernel written in C and x86 Assembly. Operating entirely in 32-bit Protected Mode, the project avoids reliance on high-level standard libraries or pre-existing operating system layers, booting directly from the bare hardware via the Multiboot 1 specifications.

---

## 🏗️ Core Architecture & Technical Specifications

| Component | Specification | Description |
| :--- | :--- | :--- |
| **Target Architecture** | x86 (i386) | Compiled strictly for 32-bit Intel/AMD execution models. |
| **Boot Protocol** | Multiboot 1 | Validated by magic signature `0x1BADB002` for GRUB/QEMU compliance. |
| **Kernel Layout** | Monolithic | Direct hardware-to-software execution path without microkernel abstraction layers. |
| **Memory Map** | Flat Memory Model | Runs entirely on physical addressing starting at the 1MB threshold (`0x00100000`). |
| **Display Mode** | VGA Text Mode | Standard 80x25 text layout memory mapped directly to `0xB8000`. |
| **I/O Subsystem** | PS/2 Keyboard Controller | Port-mapped hardware polling driving ports `0x60` and `0x64`. |

---

## 🚀 Implemented Subsystems

### 1. Multi-color Console Rendering Engine
Unlike generic monochrome text buffers, the custom video memory subsystem partitions character attributes dynamically to enhance readability:
- **System Branding Layer**: Hardcoded to physical row 0 with High-Intensity Red parameters (`0x0C`).
- **Interactive Prompt Subsystem**: Anchored at row 2 (`v_idx = 160`) utilizing Cyan attributes (`0x09`).
- **User Execution Stream**: Renders active characters in Matrix Green attributes (`0x0A`).

### 2. Hardware I/O Peripheral Handling
The input pipeline interfaces with the motherboard's integrated keyboard matrix:
- Direct hardware bus interaction using custom inline x86 Assembly instructions (`inb`).
- Custom implementation of a standard US Layout translation matrix (`kbd_us`) mapping raw Scancodes to reliable ASCII bytes.
- Direct capture of critical system keystrokes: Backspace (`0x0E` -> `\b`) for inline memory clearance and Enter (`0x1C` -> `\n`) for command parsing.

### 3. Isolated String Parsing & Command Engine
A lightweight, zero-dependency execution shell driven by static memory comparisons:
- Custom implementations of fundamental string manipulation routines (`strcmp`).
- Synchronized console scroll boundaries preventing physical frame memory overflows.
- Native integrated shell instructions:
  - `clear`: Purges user-allocated frame regions from row 2 downward and resets execution indexes.
  - `help`: Discharges available shell entryways.
  - `about`: Emits core architectural classifications.
  - `version`: Outputs absolute system releases.
  - `logo`: Renders heavy high-impact ASCII Art branding graphs.
  - `matrix`: Executes deep terminal theme code arrays.

---

## 🛠️ Project Structure
- `boot.asm`: Assembles the early initialization vector, enforces stack memory alignments, and validates the hardware signatures.
- `kernel.c`: Operates the core executive logic, controls the I/O bus, drives string operations, and handles the interactive prompt.
- `linker.ld`: Instructs the compilation toolchain to bypass lower system memory reserved for hardware mappings.
- `Makefile`: Automates deep compilation configurations and launches execution environments instantly.

---

## 🏁 Build & Verification Instructions

### Prerequisites
Ensure your local environment or cross-compiler workspace possesses the following binary toolchains:
- **NASM** (Netwide Assembler)
- **LLVM Clang** (Targeting independent bare-metal targets)
- **LLD Linker** (Targeting ELF formatting)
- **QEMU Emulator** (x86_64 architecture engine)

### Compilation Execution
To construct the compiled binary objects and assemble the unified kernel frame:
```bash
make
```

### Emulation Launch
To boot the finalized kernel within the QEMU environment mapped to standard VNC loops:
```bash
make run
```
## How to Run

1. Clone the repository and navigate into the project directory:
   ```bash
   git clone https://github.com
   cd myosOS
   ```

2. Build and run the kernel using QEMU:
   ```bash
   make run
   ```

