# Computer System Architecture – CPU Sim Practicals

**Simulating Mano's Basic Computer in CPU Sim 4.0.11**

| | |
| :--- | :--- |
| **Name** | Shubhi Rai |
| **Roll No.** | 26570059 |
| **Course / Semester** | B.Sc. Computer Science (Hons.) / Sem I |
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

**To run any program:**

1) Start CPU Sim (Cpusim4.bat or java -jar CPUSim-4.0.11.jar).

2) File → Open machine… → select BasicComputer.cpu.

3) File → Open text… → select the target .a file.

4) Press Ctrl+2 (Assemble & load).

5) Press Ctrl+R (Run). When the console turns yellow, CPU Sim is waiting at an INP instruction: type the number and press Enter.

6) For step-by-step register tracing practicals, press Ctrl+D (Debug Mode), set the Registers pane Data selector to Unsigned Dec, and step through with Step by Instr.

## Practical 1: Create a Machine (Basic Computer Architecture)

**Aim:** To create, in CPU Sim, a machine based on Mano's Basic Computer: its registers, condition bits, memory, microinstructions, instruction fields, and machine instructions.

**Tool:** CPU Sim 4.0.11 (Java 8 with JavaFX)

**Theory:**

CPU Sim models a computer at the register-transfer level using four core primitives:

<img width="895" height="536" alt="image" src="https://github.com/user-attachments/assets/5dbe239a-3c89-451a-8001-c18f0828b408" />


Because every instruction is executed via a stor*ed sequence of microinstructions, CPU Sim simulates a **microprogrammed control unit.**

The Basic Computer uses a 16-bit word size and a 4096-word memory. The 16-bit instruction format:

```
15   14 13 12   11 ........................ 0
+---+----------+------------------------------+
| I |  opcode  |           address            |
+---+----------+------------------------------+
Memory-reference : opcode 000-110 (hex 0xxx to 6xxx when I = 0)
Register-reference: opcode 111, I = 0 (hex 7xxx)
Input-output     : opcode 111, I = 1 (hex Fxxx)
```
Bit Indexing: CPU Sim indexes bits from the left (bit 0 is MSB, Mano's bit 15). Mano's IR(0-11) corresponds to CPU Sim's IR start bit 4 with length 12. Mano's AC(0) (LSB) is CPU Sim's AC bit 15.

*Choose File → New machine (Ctrl+Shift+N). Under Execute → Options…, ensure bits are indexed from the left.*

## Procedure
**Step 1 – Start a new machine**
*Choose File* → New machine (Ctrl+Shift+N). Under Execute → Options…, ensure bits are indexed from the left.


**Step 2 – Create the registers**
*Open Modify* → Hardware Modules (Ctrl+K), choose Register, click New, and add each register:

<img width="892" height="601" alt="image" src="https://github.com/user-attachments/assets/01f8d08b-de4e-4903-9480-32c540e168e4" />


------------------------------------------------------

**Step 3 – Create condition bits and RAM**

1) Change module type to ConditionBit, click New:

- carry-E: Register E, bit 0, halt unchecked.
- halt-S: Register S, bit 0, halt checked (setting this halts execution).

2) Change module type to RAM, click New:

- Name M, length 4096, cell size 16 (word-addressed memory).

--------------------------------------------

**Step 4 – Create the microinstructions**

*Open Modify* → Microinstructions (Ctrl+Shift+M) and define all 33 microinstructions:

- ## TransferRtoR:

- PC->AR (PC 0 to AR 0, 12 bits)

- IR(0-11)->AR (IR 4 to AR 0, 12 bits)

- AR->PC (AR 0 to PC 0, 12 bits)

- DR->AC (DR 0 to AC 0, 16 bits)

- AC(0)->TMP (AC 15 to TMP 0, 1 bit)

- AC(15)->TMP (AC 0 to TMP 0, 1 bit)

- E->AC(15) (E 0 to AC 0, 1 bit)

- E->AC(0) (E 0 to AC 15, 1 bit)

- TMP->E (TMP 0 to E 0, 1 bit)

## MemoryAccess:

- M[AR]->IR (read, memory M, data IR, address AR)

- M[AR]->DR (read, memory M, data DR, address AR)

- AC->M[AR] (write, memory M, data AC, address AR)

- DR->M[AR] (write, memory M, data DR, address AR)

## Increment:

- PC+1->PC (delta 1)

- DR+1->DR (delta 1)

- AC+1->AC (delta 1)

## Arithmetic:

- AC+DR->AC,E (ADD, source1 AC, source2 DR, dest AC, carry carry-E)

## Logical:

- AC^DR->AC (AND, source1 AC, source2 DR, dest AC)

- AC'->AC (NOT, source1 AC, dest AC)

- E'->E (NOT, source1 E, dest E)

## Shift:

- shr AC (logical right, distance 1)

- shl AC (logical left, distance 1)

## Set:

- 0->AC (start 0, numBits 16, value 0)

- 0->E (start 0, numBits 1, value 0)

## Test:

- if(DR!=0)skip-1 (DR, start 0, numBits 16, NE, 0, omission 1)

- if(AC(15)!=0)skip-1 (AC, start 0, numBits 1, NE, 0, omission 1)

- if(AC(15)==0)skip-1 (AC, start 0, numBits 1, EQ, 0, omission 1)

- if(AC!=0)skip-1 (AC, start 0, numBits 16, NE, 0, omission 1)

- if(E!=0)skip-1 (E, start 0, numBits 1, NE, 0, omission 1)

## Decode:

- decode-IR (ir: IR)

## SetCondBit:

- 1->S(halt) (bit halt-S, value 1)

## IO:

- input-int->AC (input, integer, buffer AC, connection Console)

- output-AC->int (output, integer, buffer AC, connection Console)

---------------------------------------------------------------------------

**Step 5 – Create instruction fields**

*Under Modify* → Machine Instructions (Ctrl+M) → Edit Fields…:

- op: 4 bits, required, absolute, unsigned (opcode for memory instructions 0–6).

- addr: 12 bits, required, absolute, unsigned (12-bit address field).

- opcode: 16 bits, required, absolute, unsigned (full code for register/IO instructions).

-------------------------------------------------------------------------------

## Step 6 – Create the 20 machine instructions

<img width="1120" height="786" alt="image" src="https://github.com/user-attachments/assets/59e54203-3d82-4717-a5c4-8a4f5cee71e6" />

<img width="1112" height="800" alt="image" src="https://github.com/user-attachments/assets/7e2978d1-5b3b-4f20-af24-78a5722e8da5" />

<img width="1087" height="72" alt="image" src="https://github.com/user-attachments/assets/eabd48a7-cf45-42ca-8246-eb620c56dae4" />

----------------------------------------------------------------------------------------------------------


**Step 7 – Save machine**

Select PC as program counter under Execute → Options…, then save the machine as BasicComputer.cpu.

**Result:**

A fully functional machine based on Mano's Basic Computer architecture was created and verified in CPU Sim.

--------------------------------------


## Practical 2: Create the Fetch Routine of the Instruction Cycle
**Aim:** To create the fetch and decode routine of the instruction cycle and observe it single-stepping one microinstruction at a time.
**Tool:** CPU Sim 4.0.11 (Java 8 with JavaFX)
```
T0: AR <- PC
T1: IR <- M[AR], PC <- PC + 1
T2: D0..D7 <- decode IR(12-14), AR <- IR(0-11), I <- IR(15)
```


In CPU Sim, microinstructions execute sequentially:
1) PC->AR (T0)
2) M[AR]->IR (T1)
3) PC+1->PC (T1)
4) IR(0-11)->AR (T2)
5) decode-IR (T2)
## Procedure
1) Open BasicComputer.cpu → Modify → Fetch Sequence (Ctrl+Y).
2) Add PC->AR, M[AR]->IR, PC+1->PC, IR(0-11)->AR, and decode-IR. Save (Ctrl+B).
3) Open P03_ADD.a, assemble and load (Ctrl+2), and switch to debug mode (Ctrl+D). Set Registers view to Unsigned Dec.
4) Step five times using Step by Micro.


## Observations

```
Micro-stepMicroinstructionARPCIR
Initial—000
1PC->AR000
2M[AR]->IR0063488 (F800)
3PC+1->PC0163488 (F800)
4IR(0-11)->AR2048 (800)163488 (F800)
5decode-IR2048 (800)163488 (F800) → INP
```

## Result

The universal fetch routine was implemented and validated by tracing register state transitions.


-----------------------------------------------------------------


## Practical 3: ADD Operation on Two User-Entered Numbers

**Aim:**  To write an assembly program that reads two numbers entered by the user, adds them, and displays the sum.
**Program File:**  P03_ADD.a


## Source Code
```
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

## Memory Map

## Observations

## Result

The program correctly adds two user-entered integers: 25 + 17 = 42.

-------------------------------------------------------------------

## Practical 4: SUBTRACT Operation on Two User-Entered Numbers

**Aim:**  To write an assembly program that reads two numbers A and B and displays A - B using two's complement.
**Program File:** P04_SUBTRACT.a


## Theory
Subtraction is performed by adding the 2's complement of the subtrahend:

        A – B = A + (B' + 1)
