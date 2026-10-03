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
