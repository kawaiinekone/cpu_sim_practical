# Computer System Architecture – CPU Sim Practicals

**Simulating Mano's Basic Computer in CPU Sim 4.0.11**

| | |
| :--- | :--- |
| **Name** | Shubhi Rai |
| **Roll No.** | 26570059 |
| **Course / Semester** | B.Sc. Computer Science (Hons.) / Sem I |
| **Section** | 1 |
| **College** | Ramanujan College, University of Delhi |
| **Tool** | CPU Sim 4.0.11 (Java 8 with JavaFX) |

---

## Contents

| No. | Practical | Program File |
| :---: | :--- | :--- |
| 1 | [Create a machine based on the Basic Computer](#practical-1-create-a-machine-basic-computer-architecture) | `BasicComputer.cpu` |
| 2 | [Create the fetch routine of the instruction cycle](#practical-2-create-the-fetch-routine-of-the-instruction-cycle) | `P03_ADD.a` (test) |
| 3 | [ADD two user-entered numbers](#practical-3-add-operation-on-two-user-entered-numbers) | `P03_ADD.a` |
| 4 | [SUBTRACT two user-entered numbers](#practical-4-subtract-operation-on-two-user-entered-numbers) | `P04_SUBTRACT.a` |
| 5 | [AND, OR, NOT, XOR, NOR, NAND](#practical-5-logical-operations-and-or-not-xor-nor-nand) | `P05_LOGIC.a` |
| 6 | [Memory-reference instructions ADD, LDA, STA, BUN, ISZ](#practical-6-memory-reference-instructions-add-lda-sta-bun-isz) | `P06_MEMORY_REFERENCE.a` |
| 7 | [Register-reference: CLA, CMA, CME, HLT](#practical-7-register-reference-instructions-cla-cma-cme-hlt) | `P07_REGISTER_REF_CLA_CMA_CME_HLT.a` |
| 8 | [Register-reference: INC, SPA, SNA, SZE](#practical-8-register-reference-instructions-inc-spa-sna-sze) | `P08_REGISTER_REF_INC_SPA_SNA_SZE.a` |
| 9 | [Register-reference: CIR, CIL](#practical-9-register-reference-instructions-cir-cil) | `P09_REGISTER_REF_CIR_CIL.a` |
| 10 | [Sum of integers until a negative number](#practical-10-sum-of-integers-until-a-negative-number-is-read) | `P10_SUM_UNTIL_NEGATIVE.a` |
| 11 | [Sum of integers until zero](#practical-11-sum-of-integers-until-zero-is-read) | `P11_SUM_UNTIL_ZERO.a` |

---

## How this repository is organised

```text
BasicComputer.cpu                      # Machine configuration file (built in Practicals 1 & 2)
P03_ADD.a                              # Assembly source files for each practical
P04_SUBTRACT.a
P05_LOGIC.a
P06_MEMORY_REFERENCE.a
P07_REGISTER_REF_CLA_CMA_CME_HLT.a
P08_REGISTER_REF_INC_SPA_SNA_SZE.a
P09_REGISTER_REF_CIR_CIL.a
P10_SUM_UNTIL_NEGATIVE.a
P11_SUM_UNTIL_ZERO.a
screenshots/                           # Simulation traces, memory maps, and outputs
README.md                              # Complete lab practical portfolio
```

### To run any program

1. Start CPU Sim (`Cpusim4.bat` or `java -jar CPUSim-4.0.11.jar`).
2. **File → Open machine…** → select `BasicComputer.cpu`.
3. **File → Open text…** → select the target `.a` file.
4. Press **Ctrl+2** (Assemble & load).
5. Press **Ctrl+R** (Run). When the console turns yellow, CPU Sim is waiting at an `INP` instruction: type the number and press Enter.
6. For step-by-step register tracing practicals, press **Ctrl+D** (Debug Mode), set the Registers pane *Data* selector to **Unsigned Dec**, and step through with **Step by Instr**.

---

## Practical 1: Create a Machine (Basic Computer Architecture)

**Aim:** To create, in CPU Sim, a machine based on Mano's Basic Computer: its registers, condition bits, memory, microinstructions, instruction fields, and machine instructions.

**Tool:** CPU Sim 4.0.11 (Java 8 with JavaFX)

### Theory

CPU Sim models a computer at the register-transfer level using four core primitives: **Registers, RAM, ConditionBits, and Microinstructions**. Because every instruction is executed via a stored sequence of microinstructions, CPU Sim simulates a microprogrammed control unit.

The Basic Computer uses a 16-bit word size and a 4096-word memory. The 16-bit instruction format:

```text
15   14 13 12   11 ........................ 0
+---+----------+------------------------------+
| I |  opcode  |            address           |
+---+----------+------------------------------+

Memory-reference  : opcode 000-110 (hex 0xxx to 6xxx when I = 0)
Register-reference: opcode 111, I = 0 (hex 7xxx)
Input-output      : opcode 111, I = 1 (hex Fxxx)
```

**Bit indexing:** CPU Sim indexes bits from the left (bit 0 is the MSB, which is Mano's bit 15). Mano's `IR(0-11)` corresponds to CPU Sim's `IR` start bit 4 with length 12. Mano's `AC(0)` (LSB) is CPU Sim's `AC` bit 15.

### Procedure

1. **Start a new machine:** choose **File → New machine** (Ctrl+Shift+N). Under **Execute → Options…**, ensure bits are indexed from the left.
2. **Create the registers:** open **Modify → Hardware Modules** (Ctrl+K), choose *Register*, click *New*, and add: `PC` (12-bit), `AR` (12-bit), `IR` (16-bit), `DR` (16-bit), `AC` (16-bit), `TR` (16-bit), `INPR` (8-bit), `OUTR` (8-bit), `E` (1-bit), `S` (1-bit), `TMP` (1-bit).
3. **Create condition bits and RAM:**
   - ConditionBit `carry-E`: register `E`, bit 0, halt unchecked.
   - ConditionBit `halt-S`: register `S`, bit 0, halt checked (setting it halts execution).
   - RAM `M`: length 4096, cell size 16 (word-addressed memory).
4. **Create the microinstructions:** under **Modify → Microinstructions** (Ctrl+Shift+M), create all 33 microinstructions across the TransferRtoR, MemoryAccess, Increment, Arithmetic, Logical, Shift, Set, Test, Decode, SetCondBit, and IO categories.
5. **Create instruction fields:** under **Modify → Machine Instructions → Edit Fields…**:
   - `op`: 4 bits, required, absolute, unsigned.
   - `addr`: 12 bits, required, absolute, unsigned.
   - `opcode`: 16 bits, required, absolute, unsigned.
6. **Create the 21 machine instructions:**
   - Memory-reference (7): `AND`, `ADD`, `LDA`, `STA`, `BUN`, `BSA`, `ISZ`.
   - Register-reference (12): `CLA`, `CLE`, `CMA`, `CME`, `CIR`, `CIL`, `INC`, `SPA`, `SNA`, `SZA`, `SZE`, `HLT`.
   - I/O (2): `INP`, `OUT`.
7. **Save the machine:** select `PC` as the program counter under **Execute → Options…**, then save as `BasicComputer.cpu`.

### Result

A fully functional machine based on Mano's Basic Computer architecture was created and verified in CPU Sim.
<img width="1920" height="1080" alt="Screenshot (1)" src="https://github.com/user-attachments/assets/a187823d-a529-49fb-a787-3a82d1a4a220" />


**Machine instruction format**: 
<img width="1920" height="1080" alt="machine instr format" src="https://github.com/user-attachments/assets/98cea013-4c1a-46d1-b897-77b97ef0f77e" />


**Machine instruction implement  format**:
<img width="1920" height="1080" alt="machine instr implement" src="https://github.com/user-attachments/assets/af6118e8-979b-42bd-b094-73a2f082285e" />


---

## Practical 2: Create the Fetch Routine of the Instruction Cycle

**Aim:** To create the fetch and decode routine of the instruction cycle and observe it single-stepping one microinstruction at a time.

**Tool:** CPU Sim 4.0.11 (Java 8 with JavaFX)

### Theory

The instruction cycle fetch sequence in Mano's architecture:

```text
T0: AR <- PC
T1: IR <- M[AR], PC <- PC + 1
T2: D0..D7 <- decode IR(12-14), AR <- IR(0-11), I <- IR(15)
```

In CPU Sim, the microinstructions execute sequentially:

1. `PC->AR` ($T_0$)
2. `M[AR]->IR` ($T_1$)
3. `PC+1->PC` ($T_1$)
4. `IR(0-11)->AR` ($T_2$)
5. `decode-IR` ($T_2$)

### Procedure

1. Open `BasicComputer.cpu` → **Modify → Fetch Sequence** (Ctrl+Y).
2. Add `PC->AR`, `M[AR]->IR`, `PC+1->PC`, `IR(0-11)->AR`, and `decode-IR`. Save (Ctrl+B).
3. Open `P03_ADD.a`, assemble and load (Ctrl+2), switch to debug mode (Ctrl+D), and set the Registers view to *Unsigned Dec*.
4. Step five times using **Step by Micro**.

### Observations

| Micro-step | Microinstruction | AR | PC | IR |
| :---: | :--- | :---: | :---: | :--- |
| Initial | — | 0 | 0 | 0 |
| 1 | `PC->AR` | 0 | 0 | 0 |
| 2 | `M[AR]->IR` | 0 | 0 | 63488 (`F800` hex) |
| 3 | `PC+1->PC` | 0 | 1 | 63488 (`F800` hex) |
| 4 | `IR(0-11)->AR` | 2048 (`800` hex) | 1 | 63488 (`F800` hex) |
| 5 | `decode-IR` | 2048 (`800` hex) | 1 | 63488 (`F800` hex) → `INP` |

### Result

The universal fetch routine was implemented and validated by tracing register state transitions microinstruction by microinstruction.

![P02_Fetch_Cycle_Trace](screenshots/p02_fetch_cycle.png)

---

## Practical 3: ADD Operation on Two User-Entered Numbers

**Aim:** To write an assembly program that reads two numbers entered by the user, adds them, and displays the sum.

**Program file:** `P03_ADD.a`

### Theory

The input instruction `INP` reads an integer from the console into the accumulator (`AC`). The first number is saved to memory location `A` using `STA`. The second number is read into `AC` and added to `A` using `ADD A`; the carry-out of the 16-bit addition is stored in `E`. `OUT` prints the sum to the console.

### Source Code

```asm
; ==============================================================
; Practical 3 : ADD two numbers typed by the user
; Machine     : BasicComputer.cpu (Mano's Basic Computer)
; Formula     : SUM = A + B
; ==============================================================
        INP             ; AC <- first number from console
        STA A           ; Save first number in memory location A
        INP             ; AC <- second number from console
        ADD A           ; AC <- AC + M[A], E <- carry out
        STA SUM         ; Store total in SUM
        OUT             ; Display AC
        HLT             ; Stop machine

A:      .data 1 0       ; Storage for first number
SUM:    .data 1 0       ; Storage for sum
```

### Memory Map

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
| :---: | :---: | :---: | :---: | :--- | :--- |
| 0 | 000 | `F800` | | `INP` | AC ← first number typed by user |
| 1 | 001 | `3007` | | `STA A` | M[A] ← AC (save first number) |
| 2 | 002 | `F800` | | `INP` | AC ← second number |
| 3 | 003 | `1007` | | `ADD A` | AC ← AC + M[A], E ← carry out |
| 4 | 004 | `3008` | | `STA SUM` | M[SUM] ← AC (save result) |
| 5 | 005 | `F400` | | `OUT` | display AC (the sum) |
| 6 | 006 | `7001` | | `HLT` | stop |
| 7 | 007 | `0000` | `A` | `.data 1 0` | first number |
| 8 | 008 | `0000` | `SUM` | `.data 1 0` | result |

### Observations

| Input(s) typed | Output displayed |
| :--- | :---: |
| 25, 17 | 42 |
| -40, 15 | -25 |
| -1, 1 | 0 |
| 30000, 10000 | -25536 |

- With inputs 25 and 17, the final registers are: `AC` = 42, `DR` = 25, `PC` = 7, `IR` = 28673 (`7001` hex), `E` = 0.
- For 30000 and 10000, the sum 40000 exceeds the 16-bit signed limit (+32767), wrapping around as two's complement overflow: 40000 − 65536 = −25536.

### Result

The program correctly adds two user-entered integers: 25 + 17 = 42.

![P03_ADD_Console_Output](screenshots/p03_add_output.png)

---

## Practical 4: SUBTRACT Operation on Two User-Entered Numbers

**Aim:** To write an assembly program that reads two numbers $A$ and $B$ and displays $A - B$ using two's complement.

**Program file:** `P04_SUBTRACT.a`

### Theory

The Basic Computer has no dedicated subtract instruction. Subtraction is performed by adding the two's complement of the subtrahend:

$$A - B = A + (\overline{B} + 1)$$

`CMA` forms the one's complement ($\overline{B}$), and `INC` adds 1 to form the two's complement ($-B$). This value is then added to $A$ using `ADD`.

### Source Code

```asm
; ==============================================================
; Practical 4 : SUBTRACT operation on two user-entered numbers
; Machine     : BasicComputer.cpu (Mano's Basic Computer)
; Logic       : DIFF = A - B = A + (2's complement of B)
;               2's complement of B = B' + 1 (CMA, then INC)
; ==============================================================
        INP             ; AC <- A (minuend)
        STA A           ; M[A] <- AC
        INP             ; AC <- B (subtrahend)
        CMA             ; AC <- AC' (1's complement of B)
        INC             ; AC <- AC + 1 (2's complement of B = -B)
        ADD A           ; AC <- A + (-B) = A - B
        STA DIFF        ; M[DIFF] <- AC
        OUT             ; display the difference
        HLT

A:      .data 1 0       ; minuend
DIFF:   .data 1 0       ; result
```

### Memory Map

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
| :---: | :---: | :---: | :---: | :--- | :--- |
| 0 | 000 | `F800` | | `INP` | AC ← A (minuend) |
| 1 | 001 | `3009` | | `STA A` | M[A] ← AC |
| 2 | 002 | `F800` | | `INP` | AC ← B (subtrahend) |
| 3 | 003 | `7200` | | `CMA` | AC ← AC' (1's complement of B) |
| 4 | 004 | `7020` | | `INC` | AC ← AC + 1 (2's complement of B) |
| 5 | 005 | `1009` | | `ADD A` | AC ← A + (−B) = A − B |
| 6 | 006 | `300A` | | `STA DIFF` | M[DIFF] ← AC |
| 7 | 007 | `F400` | | `OUT` | display difference |
| 8 | 008 | `7001` | | `HLT` | halt |
| 9 | 009 | `0000` | `A` | `.data 1 0` | minuend |
| 10 | 00A | `0000` | `DIFF` | `.data 1 0` | result |

### Observations

| Input(s) typed | Output displayed |
| :--- | :---: |
| 50, 18 | 32 |
| 18, 50 | -32 |
| -7, -7 | 0 |
| 0, 1 | -1 |

### Result

The program subtracts two user-entered numbers using two's complement arithmetic (50 − 18 = 32 and 18 − 50 = −32).

![P04_SUBTRACT_Console_Output](screenshots/p04_subtract_output.png)

---

## Practical 5: Logical Operations: AND, OR, NOT, XOR, NOR, NAND

**Aim:** To simulate the bitwise logic operations AND, OR, NOT, XOR, NOR, and NAND using only `AND` and `CMA`.

**Program file:** `P05_LOGIC.a`

### Theory

Mano's Basic Computer provides only `AND` and `CMA` (NOT). All other logic functions are derived using Boolean algebra and De Morgan's laws:

- **NOT:** $\overline{A}$ (directly via `CMA`)
- **AND:** $A \cdot B$ (directly via `AND`)
- **OR:** $A + B = \overline{\overline{A} \cdot \overline{B}}$
- **NAND:** $\overline{A \cdot B}$
- **NOR:** $\overline{A + B}$
- **XOR:** $A \oplus B = (A + B) \cdot \overline{(A \cdot B)}$

### Source Code

```asm
; ==============================================================
; Practical 5 : Logical operations AND, OR, NOT, XOR, NOR, NAND
; Machine     : BasicComputer.cpu (Mano's Basic Computer)
; ==============================================================
        INP             ; AC <- A
        STA A
        INP             ; AC <- B
        STA B

; ---------- AND ------------------------------------------------
        LDA A           ; AC <- A
        AND B           ; AC <- A . B
        STA RAND
        OUT             ; output 1 : A AND B

; ---------- OR = (A' . B')' -----------------------------------
        LDA B
        CMA             ; AC <- B'
        STA NB          ; NB <- B'
        LDA A
        CMA             ; AC <- A'
        STA NA          ; NA <- A'
        AND NB          ; AC <- A' . B'
        CMA             ; AC <- (A' . B')' = A + B
        STA ROR
        OUT             ; output 2 : A OR B

; ---------- NOT A, NOT B ---------------------------------------
        LDA NA
        OUT             ; output 3 : NOT A
        LDA NB
        OUT             ; output 4 : NOT B

; ---------- XOR = (A + B) . (A . B)' ---------------------------
        LDA RAND
        CMA             ; AC <- (A . B)' = NAND
        STA RNAND
        AND ROR         ; AC <- (A + B) . (A . B)'
        STA RXOR
        OUT             ; output 5 : A XOR B

; ---------- NOR = (A + B)' -------------------------------------
        LDA ROR
        CMA             ; AC <- (A + B)'
        STA RNOR
        OUT             ; output 6 : A NOR B

; ---------- NAND = (A . B)' ------------------------------------
        LDA RNAND
        OUT             ; output 7 : A NAND B
        HLT

A:      .data 1 0
B:      .data 1 0
NA:     .data 1 0       ; A'
NB:     .data 1 0       ; B'
RAND:   .data 1 0       ; A AND B
ROR:    .data 1 0       ; A OR B
RXOR:   .data 1 0       ; A XOR B
RNOR:   .data 1 0       ; A NOR B
RNAND:  .data 1 0       ; A NAND B
```

### Memory Map

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
| :---: | :---: | :---: | :---: | :--- | :--- |
| 0 | 000 | `F800` | | `INP` | AC ← A |
| 1 | 001 | `3023` | | `STA A` | M[A] ← AC |
| 2 | 002 | `F800` | | `INP` | AC ← B |
| 3 | 003 | `3024` | | `STA B` | M[B] ← AC |
| 4 | 004 | `2023` | | `LDA A` | AC ← A |
| 5 | 005 | `0024` | | `AND B` | AC ← A · B |
| 6 | 006 | `3027` | | `STA RAND` | Save AND |
| 7 | 007 | `F400` | | `OUT` | Output 1: A AND B |
| 8 | 008 | `2024` | | `LDA B` | AC ← B |
| 9 | 009 | `7200` | | `CMA` | AC ← B' |
| 10 | 00A | `3026` | | `STA NB` | NB ← B' |
| 11 | 00B | `2023` | | `LDA A` | AC ← A |
| 12 | 00C | `7200` | | `CMA` | AC ← A' |
| 13 | 00D | `3025` | | `STA NA` | NA ← A' |
| 14 | 00E | `0026` | | `AND NB` | AC ← A' · B' |
| 15 | 00F | `7200` | | `CMA` | AC ← (A' · B')' |
| 16 | 010 | `3028` | | `STA ROR` | Save OR |
| 17 | 011 | `F400` | | `OUT` | Output 2: A OR B |
| 18 | 012 | `2025` | | `LDA NA` | |
| 19 | 013 | `F400` | | `OUT` | Output 3: NOT A |
| 20 | 014 | `2026` | | `LDA NB` | |
| 21 | 015 | `F400` | | `OUT` | Output 4: NOT B |
| 22 | 016 | `2027` | | `LDA RAND` | |
| 23 | 017 | `7200` | | `CMA` | AC ← (A · B)' = NAND |
| 24 | 018 | `302B` | | `STA RNAND` | |
| 25 | 019 | `0028` | | `AND ROR` | AC ← (A + B) · (A · B)' |
| 26 | 01A | `3029` | | `STA RXOR` | |
| 27 | 01B | `F400` | | `OUT` | Output 5: A XOR B |
| 28 | 01C | `2028` | | `LDA ROR` | |
| 29 | 01D | `7200` | | `CMA` | AC ← (A + B)' |
| 30 | 01E | `302A` | | `STA RNOR` | |
| 31 | 01F | `F400` | | `OUT` | Output 6: A NOR B |
| 32 | 020 | `202B` | | `LDA RNAND` | |
| 33 | 021 | `F400` | | `OUT` | Output 7: A NAND B |
| 34 | 022 | `7001` | | `HLT` | |
| 35–43 | 023–02B | `0000` | Labels | `.data 1 0` | Working storage |

### Observations

For inputs $A = 12$ (`0000 0000 0000 1100`) and $B = 10$ (`0000 0000 0000 1010`):

| Operation | Binary (16-bit) | Hexadecimal | Signed Decimal Output |
| :--- | :--- | :---: | :---: |
| A AND B | `0000 0000 0000 1000` | `0008` | 8 |
| A OR B | `0000 0000 0000 1110` | `000E` | 14 |
| NOT A | `1111 1111 1111 0011` | `FFF3` | -13 |
| NOT B | `1111 1111 1111 0101` | `FFF5` | -11 |
| A XOR B | `0000 0000 0000 0110` | `0006` | 6 |
| A NOR B | `1111 1111 1111 0001` | `FFF1` | -15 |
| A NAND B | `1111 1111 1111 0111` | `FFF7` | -9 |

### Result

All six logical operations were synthesized using only `AND` and `CMA`. For inputs 12 and 10, the outputs were verified as 8, 14, −13, −11, 6, −15, −9.

![P05_LOGIC_Console_Output](screenshots/p05_logic_output.png)

---

## Practical 6: Memory-Reference Instructions: ADD, LDA, STA, BUN, ISZ

**Aim:** To write an assembly program that simulates the memory-reference instructions `ADD`, `LDA`, `STA`, `BUN`, and `ISZ`.

**Program file:** `P06_MEMORY_REFERENCE.a`

### Theory

A memory-reference instruction has an opcode from 0 to 6 and a 12-bit operand address. This program computes a product by repeated addition:

$$\text{PROD} = X \times N = \underbrace{X + X + \cdots + X}_{N \text{ times}}$$

`CTR` is initialized to $-N$. In each iteration, `ISZ CTR` increments the counter. When `CTR` reaches 0, `ISZ` increments `PC` by 1 to skip `BUN LOOP`, terminating the loop.

### Source Code

```asm
; ==============================================================
; Practical 6 : Memory-reference instructions ADD, LDA, STA, BUN, ISZ
; Machine     : BasicComputer.cpu (Mano's Basic Computer)
; Task        : multiply X by N using repeated addition (5 * 3 = 15)
; ==============================================================
LOOP:   LDA PROD        ; AC <- M[PROD]
        ADD X           ; AC <- AC + M[X]
        STA PROD        ; M[PROD] <- AC
        ISZ CTR         ; M[CTR] <- M[CTR] + 1; skip if zero
        BUN LOOP        ; repeat loop
        LDA PROD        ; AC <- final product
        HLT

X:      .data 1 5       ; multiplicand
CTR:    .data 1 -3      ; -N (loop counter)
PROD:   .data 1 0       ; product accumulator
```

### Memory Map

| Addr (dec) | Addr (hex) | Code (hex) | Label | Instruction | Comment |
| :---: | :---: | :---: | :---: | :--- | :--- |
| 0 | 000 | `2009` | `LOOP` | `LDA PROD` | AC ← M[PROD] |
| 1 | 001 | `1007` | | `ADD X` | AC ← AC + M[X] |
| 2 | 002 | `3009` | | `STA PROD` | M[PROD] ← AC |
| 3 | 003 | `6008` | | `ISZ CTR` | M[CTR] ← M[CTR] + 1; skip if zero |
| 4 | 004 | `4000` | | `BUN LOOP` | PC ← LOOP |
| 5 | 005 | `2009` | | `LDA PROD` | Load final product |
| 6 | 006 | `7001` | | `HLT` | Stop execution |
| 7 | 007 | `0005` | `X` | `.data 1 5` | Multiplicand |
| 8 | 008 | `FFFD` | `CTR` | `.data 1 -3` | Counter (−3) |
| 9 | 009 | `0000` | `PROD` | `.data 1 0` | Product |

### Step-by-Step Register Trace (Unsigned Dec)

| Step | PC before | Instruction | IR (hex) | AC | DR | E | PC | AR | IR (dec) |
| :---: | :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | 0 | `LDA PROD` | `2009` | 0 | 0 | 0 | 1 | 9 | 8201 |
| 2 | 1 | `ADD X` | `1007` | 5 | 5 | 0 | 2 | 7 | 4103 |
| 3 | 2 | `STA PROD` | `3009` | 5 | 5 | 0 | 3 | 9 | 12297 |
| 4 | 3 | `ISZ CTR` | `6008` | 5 | 65534 (−2) | 0 | 4 | 8 | 24584 |
| 5 | 4 | `BUN LOOP` | `4000` | 5 | 65534 (−2) | 0 | 0 | 0 | 16384 |
| 6 | 0 | `LDA PROD` | `2009` | 5 | 5 | 0 | 1 | 9 | 8201 |
| 7 | 1 | `ADD X` | `1007` | 10 | 5 | 0 | 2 | 7 | 4103 |
| 8 | 2 | `STA PROD` | `3009` | 10 | 5 | 0 | 3 | 9 | 12297 |
| 9 | 3 | `ISZ CTR` | `6008` | 10 | 65535 (−1) | 0 | 4 | 8 | 24584 |
| 10 | 4 | `BUN LOOP` | `4000` | 10 | 65535 (−1) | 0 | 0 | 0 | 16384 |
| 11 | 0 | `LDA PROD` | `2009` | 10 | 10 | 0 | 1 | 9 | 8201 |
| 12 | 1 | `ADD X` | `1007` | 15 | 5 | 0 | 2 | 7 | 4103 |
| 13 | 2 | `STA PROD` | `3009` | 15 | 5 | 0 | 3 | 9 | 12297 |
| 14 | 3 | `ISZ CTR` | `6008` | 15 | 0 | 0 | 5 | 8 | 24584 |
| 15 | 5 | `LDA PROD` | `2009` | 15 | 15 | 0 | 6 | 9 | 8201 |
| 16 | 6 | `HLT` | `7001` | 15 | 15 | 0 | 7 | 1 | 28673 |

### Result

At step 14, `ISZ` increments `CTR` from −1 to 0, so `PC` advances an extra step (4 → 5) and `BUN LOOP` is skipped. The program then loads `PROD` and halts with `AC` = 15 (5 × 3).
