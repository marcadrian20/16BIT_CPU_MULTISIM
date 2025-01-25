
# 16-Bit CPU Architecture

## Overview

This is a custom 16-bit CPU designed with a von Neumann, 5-stage multicycle architecture, implemented in Multisim. The architecture supports a fixed-length 2-byte instruction format, with maskable and nonmaskable interrupts. Interrupts are handled asynchronously when they occur during non-memory load/store/stack operations.

### Key Features

- **Instruction Length:** 2 Bytes (16 bits)
- **Addressing Space:** 12-bit memory space
- **Interrupts:** Maskable and nonmaskable interrupts
- **Registers:** 8 registers (A to H)
- **Ports:** 16 data lines, 12 address lines, interrupt, halt, reset, clock

### CPU Operation

The CPU operates with a fixed-length 16-bit instruction format. The instruction format is divided into the following sections:

```
| Opcode (4 bits) | Instruction Specific Data (12 bits) |
```

## Supported Operations

### ALU Operations
The following ALU operations are supported, with the format:

```
[1000|SRC|DST|xx|ALU_OP]
```

Where:
- **SRC:** Source register
- **DST:** Destination register
- **ALU_OP:** The ALU operation:
    - `0000`: ADD
    - `0001`: ADC (Add with Carry)
    - `0010`: SUB
    - `0011`: SUBB
    - `0100`: CMP
    - `0101`: AND
    - `0110`: OR
    - `0111`: XOR
    - `1000`: NOT

Example:
```
[1000|SRC|DST|xx|0000]  --> ADD operation (DST = DST + SRC)
```

### Load and Store Operations

- **Load (LD):**  
    ```
    [0010|ADDR]  
    ```
- **Load Immediate (LDI):**  
    ```
    [0100|VALUE]  
    ```
- **Store (STR):**  
    ```
    [0011|ADDR]
    ```

### Mov Operation
The format for the `MOV` operation is:
```
[0001|SRC|DST|xxxxxx]
```

### Control Operations
- **HALT:**  
    ```
    [0000|x____x]  
    ```
- **Jump Operations (JMP, JC, JZ):**
    ```
    [0101|ADDR]   --> JMP
    [0110|ADDR]   --> JC (Jump if Carry)
    [0111|ADDR]   --> JZ (Jump if Zero)
    ```

- **Push/Pop Operations:**
    ```
    [1011|REG|xxxxxx]  --> PUSH
    [1010|REG|xxxxxx]  --> POP
    ```

- **Call/Return Operations:**
    ```
    [1100|ADDR]  --> CALL
    [1101|xxxx]  --> RET
    ```

### I/O Operations
- **I/O Operations Format:**
    ```
    [0011|101111111|I/O OP]
    ```

    Supported I/O operations:
    - **TO DISPLAY:**  
        `I/O OP: 011` (Address: `3BFB`)
    - **Input:**  
        `I/O OP: 001` (Address: `3BF9`)
    - **I/O:**  
        `I/O OP: 000` (Address: `3BF8`)
    - **Output:**  
        `I/O OP: 010` (Address: `3BFA`)
    - **Keyboard:**  
        `I/O OP: 100` (Address: `3BFC`)

## Register File

The CPU has a register file with 8 registers (A to H). It supports dual-port output and single-port input.

### Register Naming:

- **A, B, C, D, E, F, G, H** (8 registers)

## Ports

The CPU has the following ports:
- **Data Lines:** 16
- **Address Lines:** 12
- **Interrupt (INT):** Signal for interrupt requests
- **Halt (HLT):** Halt the CPU
- **Reset (RST):** Reset the CPU
- **Clock (CLK):** Clock signal

## Interrupt Handling

Interrupts are maskable and nonmaskable. Interrupts are handled asynchronously when they occur during non-memory load/store/stack operations.

## Example Instructions

| Opcode | Operation | Example Instruction   |
|--------|-----------|-----------------------|
| 0000   | HALT      | `[0000|x____x]`        |
| 0010   | Load      | `[0010|ADDR]`          |
| 0100   | Load Imm. | `[0100|VALUE]`         |
| 0011   | Store     | `[0011|ADDR]`          |
| 1000   | ADD       | `[1000|SRC|DST|xx|0000]` |
| 0101   | JMP       | `[0101|ADDR]`          |
| 1010   | POP       | `[1010|REG|xxxxxx]`    |
| 1011   | PUSH      | `[1011|REG|xxxxxx]`    |
| 1100   | CALL      | `[1100|ADDR]`          |
| 1101   | RET       | `[1101|xxxx]`          |

---

For detailed implementation, see the Multisim project files.
```
