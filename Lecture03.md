# ⚙️ Lecture: MIPS Architecture and Assembly Programming

## 📘 Topics Covered
1. Encoding and Decoding of MIPS Machine Instructions  
2. The MIPS CPU Instruction Set Syntax and Semantics  
3. MIPS Assembly Language Programming  

---

## 🧩 1. Encoding and Decoding of MIPS Machine Instructions

### 🧠 Overview
**MIPS (Microprocessor without Interlocked Pipeline Stages)** is a RISC (Reduced Instruction Set Computer) architecture.  
All MIPS instructions are **32 bits long** and follow **three major formats**:

---

### 🔹 MIPS Instruction Formats

| Type | Purpose | Example |
|------|----------|----------|
| **R-type** | Register operations | `add $t0, $t1, $t2` |
| **I-type** | Immediate, load/store, branch | `addi $t0, $t1, 10` |
| **J-type** | Jump instructions | `j 0x00400000` |

---

### 🧮 Instruction Bit Fields

#### 🧩 R-type Format

| Field | Bits | Description |
|--------|------|-------------|
| opcode | 6 | Operation code (usually 0 for R-type) |
| rs | 5 | Source register 1 |
| rt | 5 | Source register 2 |
| rd | 5 | Destination register |
| shamt | 5 | Shift amount (for shift ops) |
| funct | 6 | Function code (defines operation) |

\[
\text{Format: } [opcode][rs][rt][rd][shamt][funct]
\]

Example: `add $t0, $t1, $t2`  
opcode = 0, rs = $t1 = 9, rt = $t2 = 10, rd = $t0 = 8, shamt = 0, funct = 32 (0x20)

Binary:  
`000000 01001 01010 01000 00000 100000`  
→ `0x012A4020`

---

#### 🧩 I-type Format

| Field | Bits | Description |
|--------|------|-------------|
| opcode | 6 | Operation code |
| rs | 5 | Source register |
| rt | 5 | Destination (or target) register |
| immediate | 16 | Constant or address offset |

\[
\text{Format: } [opcode][rs][rt][immediate]
\]

Example: `addi $t0, $t1, 5`  
opcode = 8, rs = $t1 = 9, rt = $t0 = 8, immediate = 0000 0000 0000 0101

Binary:  
`001000 01001 01000 0000 0000 0000 0101`  
→ `0x21280005`

---

#### 🧩 J-type Format

| Field | Bits | Description |
|--------|------|-------------|
| opcode | 6 | Jump operation code |
| address | 26 | Target address / 4 |

\[
\text{Format: } [opcode][address]
\]

Example: `j 0x00400020`  
opcode = 2 (000010), address = 0001 0000 0000 0000 0000 0000 10

Binary:  
`000010 000100000000000000000010`  
→ `0x08100002`

---

### 🧠 Decoding Process

1. **Extract opcode** → determines instruction type (R, I, or J).  
2. **Decode remaining fields** based on type.  
3. **Map funct code** (for R-type) to specific operation.  
4. **Execute** via ALU, register file, or control logic.

---

### 🖼️ MIPS Instruction Format Diagram

```mermaid
graph TD
  A[opcode (6 bits)] --> B[rs (5 bits)]
  B --> C[rt (5 bits)]
  C --> D[rd (5 bits)]
  D --> E[shamt (5 bits)]
  E --> F[funct (6 bits)]
```

---

## ⚙️ 2. The MIPS CPU Instruction Set Syntax and Semantics

### 🧠 Overview
MIPS is designed for **simplicity and efficiency**.  
It uses a **load/store architecture**, meaning only `lw` and `sw` access memory; all other operations occur in registers.

---

### 🔹 General Syntax

```
opcode destination, source1, source2/immediate
```

Example:
```
add $t0, $t1, $t2     # $t0 = $t1 + $t2
addi $t0, $t1, 5      # $t0 = $t1 + 5
lw $t0, 0($t1)        # load word from memory
```

---

### 🔹 Register Conventions

| Register | Number | Purpose |
|-----------|----------|---------|
| `$zero` | 0 | Constant 0 |
| `$at` | 1 | Reserved (assembler) |
| `$v0–$v1` | 2–3 | Function return values |
| `$a0–$a3` | 4–7 | Function arguments |
| `$t0–$t7` | 8–15 | Temporary registers |
| `$s0–$s7` | 16–23 | Saved registers |
| `$sp` | 29 | Stack pointer |
| `$ra` | 31 | Return address |

---

### 🔹 Arithmetic and Logic Instructions

| Instruction | Operation | Description |
|--------------|------------|-------------|
| `add $d, $s, $t` | rd = rs + rt | Signed addition |
| `sub $d, $s, $t` | rd = rs - rt | Signed subtraction |
| `and $d, $s, $t` | rd = rs AND rt | Bitwise AND |
| `or $d, $s, $t` | rd = rs OR rt | Bitwise OR |
| `slt $d, $s, $t` | rd = 1 if rs < rt | Set less than |

---

### 🔹 Immediate Instructions

| Instruction | Operation |
|--------------|------------|
| `addi` | Add immediate |
| `andi` | AND with immediate |
| `ori` | OR with immediate |

Example:
```assembly
addi $t0, $t1, 10   # $t0 = $t1 + 10
```

---

### 🔹 Load and Store Instructions

| Instruction | Operation |
|--------------|------------|
| `lw $t0, offset($t1)` | Load 32-bit word from memory |
| `sw $t0, offset($t1)` | Store 32-bit word to memory |
| `lb`, `sb` | Load/store byte |

Example:
```assembly
lw $t0, 0($sp)      # Load from memory[sp]
sw $t1, 4($sp)      # Save value
```

---

### 🔹 Branch and Jump Instructions

| Instruction | Description |
|--------------|-------------|
| `beq $s, $t, label` | Branch if equal |
| `bne $s, $t, label` | Branch if not equal |
| `j label` | Jump unconditionally |
| `jr $ra` | Jump to address in $ra (return) |

---

### 🧩 Example: Conditional Branch
```assembly
beq $t0, $t1, equal_label
sub $t2, $t0, $t1
j exit
equal_label:
add $t2, $t0, $t1
exit:
```

---

## 🧮 3. MIPS Assembly Language Programming

### 🧠 Overview
MIPS assembly is a **low-level programming language** that directly maps to hardware instructions.  
It uses **pseudo-instructions** for convenience, which the assembler converts into real MIPS instructions.

---

### 🔹 Program Structure
```
.data     # Data segment (variables)
.text     # Code segment (instructions)
.globl main  # Declare entry point
```

---

### 🧩 Example 1: Simple Addition Program
```assembly
.data
prompt1: .asciiz "Enter a number: "
prompt2: .asciiz "Enter another number: "
result:  .asciiz "The sum is: "

.text
.globl main
main:
    li $v0, 4           # syscall: print string
    la $a0, prompt1
    syscall

    li $v0, 5           # syscall: read integer
    syscall
    move $t0, $v0

    li $v0, 4
    la $a0, prompt2
    syscall

    li $v0, 5
    syscall
    move $t1, $v0

    add $t2, $t0, $t1   # Compute sum

    li $v0, 4
    la $a0, result
    syscall

    li $v0, 1           # print integer
    move $a0, $t2
    syscall

    li $v0, 10          # exit
    syscall
```

---

### 🔹 Example 2: Using Branches and Loops
```assembly
# Sum numbers from 1 to 5
.data
sum_msg: .asciiz "Sum = "

.text
.globl main
main:
    li $t0, 1      # i = 1
    li $t1, 0      # sum = 0
loop:
    add $t1, $t1, $t0
    addi $t0, $t0, 1
    ble $t0, 5, loop
done:
    li $v0, 4
    la $a0, sum_msg
    syscall

    li $v0, 1
    move $a0, $t1
    syscall

    li $v0, 10
    syscall
```

---

### 🧠 System Calls (MARS/SPIM)

| Service | $v0 | Description | Arguments | Returns |
|----------|------|--------------|------------|-----------|
| 1 | Print integer | $a0 = integer | — |
| 4 | Print string | $a0 = address | — |
| 5 | Read integer | — | $v0 = integer |
| 10 | Exit program | — | — |

---

### ⚙️ MIPS Execution Cycle
\[
\text{Fetch} \rightarrow \text{Decode} \rightarrow \text{Execute} \rightarrow \text{Memory} \rightarrow \text{Write-back}
\]

```mermaid
flowchart LR
  A[Fetch] --> B[Decode]
  B --> C[Execute]
  C --> D[Memory Access]
  D --> E[Write Back]
  E --> A
```

---

### 🧩 Summary

| Topic | Key Point |
|--------|------------|
| Encoding/Decoding | Instructions divided into R, I, J formats |
| MIPS Syntax | Simple, register-based with fixed instruction length |
| Assembly Programming | Load/store, arithmetic, control flow, syscalls |

---


