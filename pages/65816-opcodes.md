---
title: 65816 Opcodes
tags: SnesDev
---

Notes about this document:

The psuedo-code described in this document does not describe the 65816
internals but rather the change in machine state.

The phrase *+1 if index crosses page boundary* means +1 cycle if:

 * The Index register is 16 bits, or
 * `(Addr + Index) & 0xffff00` ≠ `Addr 0xffff00`


ADC - Add with Carry
====================

**Flags affected**: `nv----zc`

`A` ← `A + M + c`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`v` ← Signed overflow of result
<br/>`z` ← Set if the result is zero
<br/>`c` ← Carry from ALU (bit 8/16 of result)


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
ADC #const      | Immediate                 | 69    | 2 / 3 | 2 | +1 if m=0
ADC addr        | Absolute                  | 6D    | 3     | 4 | +1 if m=0
ADC long        | Absolute Long             | 6F    | 4     | 5 | +1 if m=0
ADC dp          | Direct Page               | 65    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
ADC (dp)        | Direct Page Indirect      | 72    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
ADC [dp]        | Direct Page Indirect Long | 67    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
ADC addr, X     | Absolute Indexed, X       | 7D    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
ADC long, X     | Absolute Long Indexed, X  | 7F    | 4     | 5 | +1 if m=0
ADC addr, Y     | Absolute Indexed, Y       | 79    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
ADC dp, X       | Direct Page Indexed, X    | 75    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
ADC (dp, X)     | Direct Page Indirect, X   | 61    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
ADC (dp), Y     | DP Indirect Indexed, Y    | 71    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
ADC [dp], Y     | DP Indirect Long Indexed, Y | 77  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
ADC sr, S       | Stack Relative            | 63    | 2     | 4 | +1 if m=0
ADC (sr, S), Y  | SR Indirect Indexed, Y    | 73    | 2     | 7 | +1 if m=0



AND - And Accumulator with Memory
=================================

**Flags affected**: `n-----z-`

`A` ← `A & M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
AND #const      | Immediate                 | 29    | 2 / 3 | 2 | +1 if m=0
AND addr        | Absolute                  | 2D    | 3     | 4 | +1 if m=0
AND long        | Absolute Long             | 2F    | 4     | 5 | +1 if m=0
AND dp          | Direct Page               | 25    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
AND (dp)        | Direct Page Indirect      | 32    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
AND [dp]        | Direct Page Indirect Long | 27    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
AND addr, X     | Absolute Indexed, X       | 3D    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
AND long, X     | Absolute Long Indexed, X  | 3F    | 4     | 5 | +1 if m=0
AND addr, Y     | Absolute Indexed, Y       | 39    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
AND dp, X       | Direct Page Indexed, X    | 35    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
AND (dp, X)     | Direct Page Indirect, X   | 21    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
AND (dp), Y     | DP Indirect Indexed, Y    | 31    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
AND [dp], Y     | DP Indirect Long Indexed, Y | 37  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
AND sr, S       | Stack Relative            | 23    | 2     | 4 | +1 if m=0
AND (sr, S), Y  | SR Indirect Indexed, Y    | 33    | 2     | 7 | +1 if m=0



ASL - Arithmetic Shift Left
===========================

**Flags affected**: `n-----zc`

`M` ← `M + M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero
<br/>`c` ← Most significant bit of original Memory


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
ASL             | Accumulator               | 0A    | 1     | 2
ASL addr        | Absolute                  | 0E    | 3     | 6 | +2 if m=0
ASL dp          | Direct Page               | 06    | 2     | 5 | +2 if m=0, +1 if DP.l ≠ 0
ASL addr, X     | Absolute Indexed, X       | 1E    | 3     | 7 | +2 if m=0, +1 if index crosses page boundary
ASL dp, X       | Direct Page Indexed, X    | 16    | 2     | 6 | +2 if m=0, +1 if DP.l ≠ 0



Branches
========

**Flags affected**: `--------`

**Brach not taken:**
<br/>&mdash;

**Brach taken:**
<br/>`PC` ← `PC + sign-extend(near)`

**Brach taken (BRL):**
<br/>`PC` ← `PC + label`


Syntax          | Name                      | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
BCC near        | Branch if Carry Clear     | 90    | 2     | 2 | +1 if branch taken, +1 if e=1
BCS near        | Branch if Carry Set       | B0    | 2     | 2 | +1 if branch taken, +1 if e=1
BEQ near        | Branch if Equal (z flag=1)| F0    | 2     | 2 | +1 if branch taken, +1 if e=1
BNE near        | Branch if Not Equal (z=0) | D0    | 2     | 2 | +1 if branch taken, +1 if e=1
BMI near        | Branch if Minus           | 30    | 2     | 2 | +1 if branch taken, +1 if e=1
BPL near        | Branch if Plus            | 10    | 2     | 2 | +1 if branch taken, +1 if e=1
BVC near        | Branch if Overflow Clear  | 50    | 2     | 2 | +1 if branch taken, +1 if e=1
BVS near        | Branch if Overflow Set    | 70    | 2     | 2 | +1 if branch taken, +1 if e=1
BRA near        | Branch Always             | 80    | 2     | 3 | +1 if e=1
BRL label       | Branch Always Long        | 82 | 3    | 4



BIT - Test Memory Bits against Accumulator
==========================================

**Flags affected**: `nv----z-`
<br/>**Flags affected (Immediate addressing mode only)**: `------z-`

`A & M`
<br/>
<br/>`n` ← Most significant bit of memory
<br/>`v` ← Second most significant bit of memory
<br/>`z` ← Set if logical AND of memory and Accumulator is zero



Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
BIT #const      | Immediate                 | 89    | 2 / 3 | 2 | +1 if m=0
BIT addr        | Absolute                  | 2C    | 3     | 4 | +1 if m=0
BIT dp          | Direct Page               | 24    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
BIT addr, X     | Absolute Indexed, X       | 3C    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
BIT dp, X       | Direct Page Indexed, X    | 34    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0



Software Interrupts
===================

**Flags affected**: `----di--`

**Native Mode:**
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 4`
<br/><tt>[SP+4]</tt> ← `PB`
<br/><tt>[SP+3]</tt> ← `PC.h`
<br/><tt>[SP+2]</tt> ← `PC.l`
<br/><tt>[SP+1]</tt> ← `P`
<br/><tt>PB&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `0`
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← interrupt address
<br/>
<br/><tt>d&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `0`
<br/><tt>i&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `1`


**Emulation Mode:**
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 3`
<br/><tt>[SP+3]</tt> ← `PC.h`
<br/><tt>[SP+2]</tt> ← `PC.l`
<br/><tt>[SP+1]</tt> ← `P`
<br/><tt>PB&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `0`
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← interrupt address
<br/>
<br/><tt>d&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `0`
<br/><tt>i&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `1`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
BRK param       | Interrupt                 | 00    | 2     | 7 | +1 if e=0
COP param       | Interrupt                 | 02    | 2     | 7 | +1 if e=0



Clear Status Flags
==================

**Flags affected (`CLC`)**: `-------c`
<br/>**Flags affected (`CLD`)**: `----d---`
<br/>**Flags affected (`CLI`)**: `-----i--`
<br/>**Flags affected (`CLV`)**: `-v------`

**CEC:**
<br/>`c` ← `0`

**CED:**
<br/>`d` ← `0`

**CEI:**
<br/>`i` ← `0`

**CLV:**
<br/>`v` ← `0`

Syntax          | Name                      | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
CLC             | Clear Carry Flag          | 18    | 1     | 2
CLI             | Clear Interrupt Disable Flag | 58 | 1     | 2
CLD             | Clear Decimal Flag        | D8    | 1     | 2
CLV             | Clear Overflow Flag       | B8    | 1     | 2



CMP - Compare Accumulator with Memory
=====================================

**Flags affected**: `n-----zc`

`A - M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero (Set if A == M)
<br/>`c` ← Carry from ALU (Set if A >= M)


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
CMP #const      | Immediate                 | C9    | 2 / 3 | 2 | +1 if m=0
CMP addr        | Absolute                  | CD    | 3     | 4 | +1 if m=0
CMP long        | Absolute Long             | CF    | 4     | 5 | +1 if m=0
CMP dp          | Direct Page               | C5    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
CMP (dp)        | Direct Page Indirect      | D2    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
CMP [dp]        | Direct Page Indirect Long | C7    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
CMP addr, X     | Absolute Indexed, X       | DD    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
CMP long, X     | Absolute Long Indexed, X  | DF    | 4     | 5 | +1 if m=0
CMP addr, Y     | Absolute Indexed, Y       | D9    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
CMP dp, X       | Direct Page Indexed, X    | D5    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
CMP (dp, X)     | Direct Page Indirect, X   | C1    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
CMP (dp), Y     | DP Indirect Indexed, Y    | D1    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
CMP [dp], Y     | DP Indirect Long Indexed, Y | D7  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
CMP sr, S       | Stack Relative            | C3    | 2     | 4 | +1 if m=0
CMP (sr, S), Y  | SR Indirect Indexed, Y    | D3    | 2     | 7 | +1 if m=0



CPX - Compare Index Register X with Memory
==========================================

**Flags affected**: `n-----zc`

`X - M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero (Set if `X == M`)
<br/>`c` ← Carry from ALU (Set if `X >= M`)


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
CPX #const      | Immediate                 | E0    | 2 / 3 | 2 | +1 if x=0
CPX addr        | Absolute                  | EC    | 3     | 4 | +1 if x=0
CPX dp          | Direct Page               | E4    | 2     | 3 | +1 if x=0, +1 if DP.l ≠ 0



CPY - Compare Index Register Y with Memory
==========================================

**Flags affected**: `n-----zc`

`Y - M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero (Set if `Y == M`)
<br/>`c` ← Carry from ALU (Set if `Y >= M`)


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
CPY #const      | Immediate                 | C0    | 2 / 3 | 2 | +1 if x=0
CPY addr        | Absolute                  | CC    | 3     | 4 | +1 if x=0
CPY dp          | Direct Page               | C4    | 2     | 3 | +1 if x=0, +1 if DP.l ≠ 0



DEC - Decrement
===============

**Flags affected**: `n-----z-`

`M` ← `M - 1`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
DEC             | Accumulator               | 3A    | 1     | 2
DEC addr        | Absolute                  | CE    | 3     | 6 | +2 if m=0
DEC dp          | Direct Page               | C6    | 2     | 5 | +2 if m=0, +1 if DP.l ≠ 0
DEC addr, X     | Absolute Indexed, X       | DE    | 3     | 7 | +2 if m=0, +1 if index crosses page boundary
DEC dp, X       | Direct Page Indexed, X    | D6    | 2     | 6 | +2 if m=0, +1 if DP.l ≠ 0



DEX, DEY - Decrement Index Registers
====================================

**Flags affected**: `n-----z-`

`R` ← `R - 1`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
DEX             | Implied                   | CA    | 1     | 2
DEY             | Implied                   | 88    | 1     | 2



EOR - Exclusive OR Accumulator with Memory
==========================================

**Flags affected**: `n-----z-`

`A` ← `A ^ M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
EOR #const      | Immediate                 | 49    | 2 / 3 | 2 | +1 if m=0
EOR addr        | Absolute                  | 4D    | 3     | 4 | +1 if m=0
EOR long        | Absolute Long             | 4F    | 4     | 5 | +1 if m=0
EOR dp          | Direct Page               | 45    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
EOR (dp)        | Direct Page Indirect      | 42    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
EOR [dp]        | Direct Page Indirect Long | 47    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
EOR addr, X     | Absolute Indexed, X       | 5D    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
EOR long, X     | Absolute Long Indexed, X  | 5F    | 4     | 5 | +1 if m=0
EOR addr, Y     | Absolute Indexed, Y       | 59    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
EOR dp, X       | Direct Page Indexed, X    | 55    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
EOR (dp, X)     | Direct Page Indirect, X   | 41    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
EOR (dp), Y     | DP Indirect Indexed, Y    | 51    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
EOR [dp], Y     | DP Indirect Long Indexed, Y | 57  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
EOR sr, S       | Stack Relative            | 43    | 2     | 4 | +1 if m=0
EOR (sr, S), Y  | SR Indirect Indexed, Y    | 53    | 2     | 7 | +1 if m=0



INC - Increment
===============

**Flags affected**: `n-----z-`

`M` ← `M + 1`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
INC             | Accumulator               | 1A    | 1     | 2
INC addr        | Absolute                  | EE    | 3     | 6 | +2 if m=0
INC dp          | Direct Page               | E6    | 2     | 5 | +2 if m=0, +1 if DP.l ≠ 0
INC addr, X     | Absolute Indexed, X       | FE    | 3     | 7 | +2 if m=0, +1 if index crosses page boundary
INC dp, X       | Direct Page Indexed, X    | F6    | 2     | 6 | +2 if m=0, +1 if DP.l ≠ 0



INX, INY - Increment Index Registers
====================================

**Flags affected**: `n-----z-`

`R` ← `R + 1`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
INX             | Implied                   | E8    | 1     | 2
INY             | Implied                   | C8    | 1     | 2



JMP, JML - Jump
===============

**Flags affected**: `--------`

**JMP:**
<br/><tt>PC&nbsp;&nbsp;&nbsp;</tt> ← `M`

**JML:**
<br/><tt>PB:PC</tt> ← `M`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
JMP addr        | Absolute                  | 4C    | 3     | 3
JMP (addr)      | Absolute Indirect         | 6C    | 3     | 5
JMP (addr, X)   | Absolute Indexed Indirect, X | 7C | 3     | 6
JMP long        |                           |       |       |
JML long        | Absolute Long             | 5C    | 4     | 4
JMP [addr]      |                           |       |       |
JML [addr]      | Absolute Indirect Long    | DC    | 3     | 6



JSR, JSL - Jump to Subroutine
=============================

**Flags affected**: `--------`

**JSR:**
<br/><tt>PC&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `PC - 1`
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 2`
<br/><tt>[SP+2]</tt> ← `PC.h`
<br/><tt>[SP+1]</tt> ← `PC.l`
<br/><tt>PC&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `M`

**JSL:**
<br/><tt>PC&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `PC - 1`
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 3`
<br/><tt>[SP+3]</tt> ← `PB`
<br/><tt>[SP+2]</tt> ← `PC.h`
<br/><tt>[SP+1]</tt> ← `PC.l`
<br/><tt>PB:PC&nbsp;</tt> ← `M`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
JSR addr        | Absolute                  | 20    | 3     | 6
JSR (addr, X)   | Absolute Indexed Indirect, X | FC | 3     | 8
JSL long        | Absolute Long             | 22    | 4     | 8



LDA - Load Accumulator from Memory
==================================

**Flags affected**: `n-----z-`

`A` ← `M`
<br/>
<br/>`n` ← Most significant bit of Accumulator
<br/>`z` ← Set if the Accumulator is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
LDA #const      | Immediate                 | A9    | 2 / 3 | 2 | +1 if m=0
LDA addr        | Absolute                  | AD    | 3     | 4 | +1 if m=0
LDA long        | Absolute Long             | AF    | 4     | 5 | +1 if m=0
LDA dp          | Direct Page               | A5    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
LDA (dp)        | Direct Page Indirect      | B2    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
LDA [dp]        | Direct Page Indirect Long | A7    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
LDA addr, X     | Absolute Indexed, X       | BD    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
LDA long, X     | Absolute Long Indexed, X  | BF    | 4     | 5 | +1 if m=0
LDA addr, Y     | Absolute Indexed, Y       | B9    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
LDA dp, X       | Direct Page Indexed, X    | B5    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
LDA (dp, X)     | Direct Page Indirect, X   | A1    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
LDA (dp), Y     | DP Indirect Indexed, Y    | B1    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
LDA [dp], Y     | DP Indirect Long Indexed, Y | B7  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
LDA sr, S       | Stack Relative            | A3    | 2     | 4 | +1 if m=0
LDA (sr, S), Y  | SR Indirect Indexed, Y    | B3    | 2     | 7 | +1 if m=0



LDX - Load Index Register X from Memory
=======================================

**Flags affected**: `n-----z-`

`X` ← `M`
<br/>
<br/>`n` ← Most significant bit of X
<br/>`z` ← Set if the X is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
LDX #const      | Immediate                 | A2    | 2 / 3 | 2 | +1 if x=0
LDX addr        | Absolute                  | AE    | 3     | 4 | +1 if x=0
LDX dp          | Direct Page               | A6    | 2     | 3 | +1 if x=0, +1 if DP.l ≠ 0
LDX addr, Y     | Absolute Indexed, Y       | BE    | 3     | 4 | +1 if x=0, +1 if index crosses page boundary
LDX dp, Y       | Direct Page Indexed, Y    | B6    | 2     | 4 | +1 if x=0, +1 if DP.l ≠ 0



LDY - Load Index Register Y from Memory
=======================================

**Flags affected**: `n-----z-`

`Y` ← `M`
<br/>
<br/>`n` ← Most significant bit of Y
<br/>`z` ← Set if the Y is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
LDY #const      | Immediate                 | A0    | 2 / 3 | 2 | +1 if x=0
LDY addr        | Absolute                  | AC    | 3     | 4 | +1 if x=0
LDY dp          | Direct Page               | A4    | 2     | 3 | +1 if x=0, +1 if DP.l ≠ 0
LDY addr, X     | Absolute Indexed, X       | BC    | 3     | 4 | +1 if x=0, +1 if index crosses page boundary
LDY dp, X       | Direct Page Indexed, X    | B4    | 2     | 4 | +1 if x=0, +1 if DP.l ≠ 0



LSR - Logical Shift Right
=========================

**Flags affected**: `n-----zc`

`M` ← `M >> 1`
<br/>
<br/>`n` ← cleared
<br/>`z` ← Set if the result is zero
<br/>`c` ← Bit 0 of original memory

NOTE: This is an unsigned operation, the MSB of the result is always 0.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
LSR             | Accumulator               | A4    | 1     | 2
LSR addr        | Absolute                  | 4E    | 3     | 6 | +1 if m=0
LSR dp          | Direct Page               | 46    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
LSR addr, X     | Absolute Indexed, X       | 5E    | 3     | 7 | +1 if m=0, +1 if index crosses page boundary
LSR dp, X       | Direct Page Indexed, X    | 56    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0



MVN - Block Move Next
=====================

**Flags affected**: `--------`

**Parameters:**
<br/>`X`: source address
<br/>`Y`: destination address
<br/>`C`: length - 1

`DB` ← `destBank`
<br/>`repeat:`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;`T`</tt> ← `srcBank:X`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;`DB:Y`</tt> ← `T`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;X</tt> ← `X + 1`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;Y</tt> ← `Y + 1`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;C</tt> ← `C - 1`
<br/>`until C == 0xffff`

<br/>
NOTES:

 * The number of bytes transferred is `C + 1`
 * After the transfer is complete:
    * `DB` = destination bank
    * `C` = `0xFFFF`
    * `X` = the byte after the end of the source block
    * `Y` = the byte after the end of the destination block.
 * If bit 4 (x) of the status register is set, `MVN` will only be able
   to access the first page of the source and destination banks.
 * `MVN` should be used if the blocks do not overlap or if the
   destination address is less than the source address.
 * `MVN` can be used to fill an array or memory block:

        set value of first element

        set X = array_start
        set Y = array_start + element_size
        set C = (array_count - 1) * element_size - 1

        MVN array_bank array_bank


Syntax                | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------------|---------------------------|-------|-------|--------|--------
MVN srcBank, destBank | Block Move                | 54    | 3     |  | 7 per byte moved



MVP - Block Move Previous
=========================

**Flags affected**: `--------`

**Parameters:**
<br/>`X`: address of last source byte
<br/>`Y`: address of last destination byte
<br/>`C`: length - 1

`DB` ← `destBank`
<br/>`repeat:`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;`T`</tt> ← `srcBank:X`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;`DB:Y`</tt> ← `T`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;X</tt> ← `X - 1`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;Y</tt> ← `Y - 1`
<br/><tt>&nbsp;&nbsp;&nbsp;&nbsp;C</tt> ← `C - 1`
<br/>`until C == 0xffff`

<br/>
NOTES:

 * The number of bytes transferred is `C + 1`.
 * After the transfer is complete:
    * `DB` = destination bank
    * `C` = `0xFFFF`
    * `X` = the byte before the start of the source block
    * `Y` = the byte before the start of the destination block.
 * If bit 4 (x) of the status register is set, `MVP` will only be able
   to access the first page of the source and destination banks.
 * `MVP` should be used if the blocks could overlap and the source
   address is less than the destination address.


Syntax                | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------------|---------------------------|-------|-------|--------|--------
MVP srcBank, destBank | Block Move                | 44    | 3     |  | 7 per byte moved



NOP - No Operation
==================

**Flags affected**: `--------`

&mdash;


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
NOP             | Implied                   | EA    | 1     | 2



ORA - OR Accumulator with Memory
================================

**Flags affected**: `n-----z-`

`A` ← `A | M`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
ORA #const      | Immediate                 | 09    | 2 / 3 | 2 | +1 if m=0
ORA addr        | Absolute                  | 0D    | 3     | 4 | +1 if m=0
ORA long        | Absolute Long             | 0F    | 4     | 5 | +1 if m=0
ORA dp          | Direct Page               | 05    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
ORA (dp)        | Direct Page Indirect      | 12    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
ORA [dp]        | Direct Page Indirect Long | 07    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
ORA addr, X     | Absolute Indexed, X       | 1D    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
ORA long, X     | Absolute Long Indexed, X  | 1F    | 4     | 5 | +1 if m=0
ORA addr, Y     | Absolute Indexed, Y       | 19    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
ORA dp, X       | Direct Page Indexed, X    | 15    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
ORA (dp, X)     | Direct Page Indirect, X   | 01    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
ORA (dp), Y     | DP Indirect Indexed, Y    | 11    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
ORA [dp], Y     | DP Indirect Long Indexed, Y | 17  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
ORA sr, S       | Stack Relative            | 03    | 2     | 4 | +1 if m=0
ORA (sr, S), Y  | SR Indirect Indexed, Y    | 13    | 2     | 7 | +1 if m=0



PEA - Push Effective Absolute Address
=====================================

**Flags affected**: `--------`

<tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 2`
<br/><tt>[SP+2]</tt> ← `addr.h`
<br/><tt>[SP+1]</tt> ← `addr.l`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
PEA addr        | Stack (Absolute)          | F4    | 3     | 5



PEI - Push Effective Indirect Address
=====================================

**Flags affected**: `--------`

<tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 2`
<br/><tt>[SP+2]</tt> ← `[0:D+dp+1]`
<br/><tt>[SP+1]</tt> ← `[0:D+dp]`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
PEI (dp)        | Stack (DP Indirect)       | D4    | 2     | 6 | +1 if DP.l ≠ 0




PER - Push Effective PC Relative Indirect Address
=================================================

**Flags affected**: `--------`

<tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 2`
<br/><tt>T&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `PC + Label`
<br/><tt>[SP+2]</tt> ← `T.h`
<br/><tt>[SP+1]</tt> ← `T.l`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
PER label       | Stack (PC Relative Long)  | 62    | 3     | 6



Push to Stack
=============

**Flags affected**: `--------`

**8 bit register:**
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 1`
<br/><tt>[SP+1]</tt> ← `R`

**16 bit register:**
<br/><tt>SP&nbsp;&nbsp;&nbsp;&nbsp;</tt> ← `SP - 2`
<br/><tt>[SP+2]</tt> ← `R.h`
<br/><tt>[SP+1]</tt> ← `R.l`


Syntax          | Name                      | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
PHA             | Push Accumulator          | 48    | 1     | 3 | +1 if m=0
PHB             | Push Data Bank            | 48    | 1     | 3
PHD             | Push Direct Page Register | 0B    | 1     | 4
PHK             | Push Program Bank Register| 4B    | 1     | 3
PHP             | Push Processor Status Register| 08| 1     | 3
PHX             | Push Index Register X     | DA    | 1     | 3 | +1 if x=0
PHY             | Push Index Register Y     | 5A    | 1     | 3 | +1 if x=0



Pull from Stack
================

**Flags affected**: `n-----z-`
<br/>**Flags affected (`PLP`)**: `nvmxdizc`

**8 bit register:**
<br/><tt>R&nbsp;&nbsp;</tt> ← `[SP+1]`
<br/><tt>SP&nbsp;</tt> ← `SP + 1`
<br/>
<br/><tt>n&nbsp;&nbsp;</tt> ← Most significant bit of register
<br/><tt>z&nbsp;&nbsp;</tt> ← Set if the register is zero

**16 bit register:**
<br/><tt>R.l</tt> ← `[SP+1]`
<br/><tt>R.h</tt> ← `[SP+2]`
<br/><tt>SP&nbsp;</tt> ← `SP + 2`
<br/>
<br/><tt>n&nbsp;&nbsp;</tt> ← Most significant bit of register
<br/><tt>z&nbsp;&nbsp;</tt> ← Set if the register is zero

**PLP (Native Mode):**
<br/><tt>P&nbsp;&nbsp;</tt> ← `[SP+1]`
<br/><tt>SP&nbsp;</tt> ← `SP + 1`

**PLP (Emulation Mode):**
<br/><tt>P&nbsp;&nbsp;</tt> ← `[SP+1]`
<br/><tt>SP&nbsp;</tt> ← `SP + 1`
<br/><tt>x&nbsp;&nbsp;</tt> ← `1`
<br/><tt>m&nbsp;&nbsp;</tt> ← `1`

Note about PLP: If bit 4 (x) of the status register is set, then the high
byte of the index registers will be set to 0.


Syntax          | Name                      | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
PLA             | Pull Accumulator          | 68    | 1     | 4 | +1 if m=0
PLB             | Pull Data Bank            | AB    | 1     | 4
PLD             | Pull Direct Page Register | 2B    | 1     | 5
PLP             | Pull Processor Status Register| 28| 1     | 4
PLX             | Pull Index Register X     | FA    | 1     | 4 | +1 if x=0
PLY             | Pull Index Register Y     | 7A    | 1     | 4 | +1 if x=0



REP - Reset Status Bits
=======================

**Flags affected**: `nvmxdizc`

**Native Mode:**
<br/>`P` ← `P & (~M)`

**Emulation Mode:**
<br/>`P` ← `P & (~M)`
<br/>`x` ← `1`
<br/>`m` ← `1`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
REP #const      | Immediate                 | C2    | 2     | 3



ROL - Rotate Left
=================

**Flags affected**: `n-----zc`

`M` ← `M + M + c`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero
<br/>`c` ← Most significant bit of original Memory


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
ROL             | Accumulator               | 2A    | 1     | 2
ROL addr        | Absolute                  | 2E    | 3     | 6 | +1 if m=0
ROL dp          | Direct Page               | 26    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
ROL addr, X     | Absolute Indexed, X       | 3E    | 3     | 7 | +1 if m=0, +1 if index crosses page boundary
ROL dp, X       | Direct Page Indexed, X    | 36    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0



ROR - Rotate Right
==================

**Flags affected**: `n-----zc`

`M` ← `(c << (m ? 7 : 15)) | (M >> 1)`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`z` ← Set if the result is zero
<br/>`c` ← Bit 0 of original memory


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
ROR             | Accumulator               | 6A    | 1     | 2
ROR addr        | Absolute                  | 6E    | 3     | 6 | +1 if m=0
ROR dp          | Direct Page               | 66    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
ROR addr, X     | Absolute Indexed, X       | 7E    | 3     | 7 | +1 if m=0, +1 if index crosses page boundary
ROR dp, X       | Direct Page Indexed, X    | 76    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0



RTI - Return From Interrupt
===========================

**Flags affected**: `nvmxdizc`

**Native Mode:**
<br/><tt>P&nbsp;&nbsp;&nbsp;</tt> ← `[SP+1]`
<br/><tt>PC.l</tt> ← `[SP+2]`
<br/><tt>PC.h</tt> ← `[SP+3]`
<br/><tt>DB&nbsp;&nbsp;</tt> ← `[SP+4]`
<br/><tt>SP&nbsp;&nbsp;</tt> ← `SP + 4`

**Emulation Mode:**
<br/><tt>P&nbsp;&nbsp;&nbsp;</tt> ← `[SP+1]`
<br/><tt>x&nbsp;&nbsp;&nbsp;</tt> ← `1`
<br/><tt>m&nbsp;&nbsp;&nbsp;</tt> ← `1`
<br/><tt>PC.l</tt> ← `[SP+2]`
<br/><tt>PC.h</tt> ← `[SP+3]`
<br/><tt>SP&nbsp;&nbsp;</tt> ← `SP + 3`

Note: If bit 4 (x) of the status register is set, then the high byte of
the index registers will be set to 0.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
RTI             | Stack (return interrupt)  | 40    | 1     | 6 | +1 if e=0



RTS, RTL - Return From Subroutine
=================================

**Flags affected**: `--------`

**RTS:**
<br/><tt>PC.l</tt> ← `[SP+1]`
<br/><tt>PC.h</tt> ← `[SP+2]`
<br/><tt>SP&nbsp;&nbsp;</tt> ← `SP + 2`
<br/><tt>PC&nbsp;&nbsp;</tt> ← `PC + 1`

**RTL:**
<br/><tt>PC.l</tt> ← `[SP+1]`
<br/><tt>PC.h</tt> ← `[SP+2]`
<br/><tt>DB&nbsp;&nbsp;</tt> ← `[SP+3]`
<br/><tt>SP&nbsp;&nbsp;</tt> ← `SP + 3`
<br/><tt>PC&nbsp;&nbsp;</tt> ← `PC + 1`



Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
RTS             | Stack (return)            | 6B    | 1     | 6
RTL             | Stack (return long)       | 60    | 1     | 6



SBC - Subtract with Borrow from Accumulator
===========================================

**Flags affected**: `nv----zc`

`A` ← `A + (~M) + c`
<br/>
<br/>`n` ← Most significant bit of result
<br/>`v` ← Signed overflow of result
<br/>`z` ← Set if the Accumulator is zero
<br/>`c` ← Carry from ALU (bit 8/16 of result) (set if borrow not required)


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
SBC #const      | Immediate                 | E9    | 2 / 3 | 2 | +1 if m=0
SBC addr        | Absolute                  | ED    | 3     | 4 | +1 if m=0
SBC long        | Absolute Long             | EF    | 4     | 5 | +1 if m=0
SBC dp          | Direct Page               | E5    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
SBC (dp)        | Direct Page Indirect      | F2    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
SBC [dp]        | Direct Page Indirect Long | E7    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
SBC addr, X     | Absolute Indexed, X       | FD    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
SBC long, X     | Absolute Long Indexed, X  | FF    | 4     | 5 | +1 if m=0
SBC addr, Y     | Absolute Indexed, Y       | F9    | 3     | 4 | +1 if m=0, +1 if index crosses page boundary
SBC dp, X       | Direct Page Indexed, X    | F5    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
SBC (dp, X)     | Direct Page Indirect, X   | E1    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
SBC (dp), Y     | DP Indirect Indexed, Y    | F1    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0, +1 if index crosses page boundary
SBC [dp], Y     | DP Indirect Long Indexed, Y | F7  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
SBC sr, S       | Stack Relative            | E3    | 2     | 4 | +1 if m=0
SBC (sr, S), Y  | SR Indirect Indexed, Y    | F3    | 2     | 7 | +1 if m=0



Set Status Flags
================

**Flags affected (`SEC`)**: `-------c`
<br/>**Flags affected (`SED`)**: `----d---`
<br/>**Flags affected (`SEI`)**: `-----i--`

**SEC:**
<br/>`c` ← `1`

**SED:**
<br/>`d` ← `1`

**SEI:**
<br/>`i` ← `1`

Syntax          | Name                      | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
SEC             | Set Carry Flag            | 38    | 1     | 2
SEI             | Set Interrupt Disable Flag | 78 | 1     | 2
SED             | Set Decimal Flag          | F8    | 1     | 2



SEP - Set Status Bits
=====================

**Flags affected**: `nvmxdizc`

`P` ← `P | M`

NOTE: If bit 4 (x) of the status register is set, then the high byte of
the index registers will be set to 0.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
SEP #const      | Immediate                 | E2    | 2     | 3



STA - Store Accumulator to Memory
=================================

**Flags affected**: `--------`

`M` ← `A`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
STA addr        | Absolute                  | 8D    | 3     | 4 | +1 if m=0
STA long        | Absolute Long             | 8F    | 4     | 5 | +1 if m=0
STA dp          | Direct Page               | 85    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
STA (dp)        | Direct Page Indirect      | 92    | 2     | 5 | +1 if m=0, +1 if DP.l ≠ 0
STA [dp]        | Direct Page Indirect Long | 87    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
STA addr, X     | Absolute Indexed, X       | 9D    | 3     | 5 | +1 if m=0
STA long, X     | Absolute Long Indexed, X  | 9F    | 4     | 5 | +1 if m=0
STA addr, Y     | Absolute Indexed, Y       | 99    | 3     | 5 | +1 if m=0
STA dp, X       | Direct Page Indexed, X    | 95    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0
STA (dp, X)     | Direct Page Indirect, X   | 81    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
STA (dp), Y     | DP Indirect Indexed, Y    | 91    | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
STA [dp], Y     | DP Indirect Long Indexed, Y | 97  | 2     | 6 | +1 if m=0, +1 if DP.l ≠ 0
STA sr, S       | Stack Relative            | 83    | 2     | 4 | +1 if m=0
STA (sr, S), Y  | SR Indirect Indexed, Y    | 93    | 2     | 7 | +1 if m=0



STP - Stop the Processor
========================

**Flags affected**: `--------`

`CPU clock enabled` ← `0`

Note, this instruction can cause some builds of snes9x to freeze.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
STP             | Implied                   | DB    | 1     | 3



STX - Store Index Register X to Memory
======================================

**Flags affected**: `--------`

`M` ← `X`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
STX addr        | Absolute                  | 8E    | 3     | 4 | +1 if x=0
STX dp          | Direct Page               | 86    | 2     | 3 | +1 if x=0, +1 if DP.l ≠ 0
STX dp, Y       | Direct Page Indexed, Y    | 96    | 2     | 4 | +1 if x=0, +1 if DP.l ≠ 0



STY - Store Index Register Y to Memory
======================================

**Flags affected**: `--------`

`M` ← `Y`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
STY addr        | Absolute                  | 8C    | 3     | 4 | +1 if x=0
STY dp          | Direct Page               | 84    | 2     | 3 | +1 if x=0, +1 if DP.l ≠ 0
STY dp, X       | Direct Page Indexed, X    | 94    | 2     | 4 | +1 if x=0, +1 if DP.l ≠ 0



STZ - Store Zero to Memory
==========================

**Flags affected**: `--------`

`M` ← `0`


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
STZ addr        | Absolute                  | 9C    | 3     | 4 | +1 if m=0
STZ dp          | Direct Page               | 64    | 2     | 3 | +1 if m=0, +1 if DP.l ≠ 0
STZ addr, X     | Absolute Indexed, X       | 9E    | 3     | 5 | +1 if m=0
STZ dp, X       | Direct Page Indexed, X    | 74    | 2     | 4 | +1 if m=0, +1 if DP.l ≠ 0



Transfer Registers
==================

**Flags affected**: `n-----z-`
<br/>**Flags affected (TCS, TXS)**: `--------`

`Rd` ← `Rs`
<br/>
<br/>`n` ← Most significant bit of register
<br/>`z` ← Set if the register is zero

The number of bits transferred depends on the state of the m, x and e
flags:

 * Accumulator to Index:
    * 8 bit Index (x=1): 8 bits transferred, high byte of Accumulator is
      unchanged
    * 16 bit Index (x=0): 16 bits transferred, no matter the state of m
 * Accumulator to/from Direct Page: 16 bits transferred
 * Index to Accumulator:
    * 8 bit A (m=1): 8 bits transferred
    * 16 bit A (m=0): 16 bits transferred (when x=0, the high byte is 0)
 * Stack Pointer to X:
    * 8 bit Index (x=1): 8 bits transferred, high byte of X = 0
    * 16 bit Index (x=0): 16 bits transferred
 * Stack Pointer to Accumulator: 16 bits transferred
 * A/X to Stack Pointer:
    * Native mode: 16 bits transferred
    * Emulation mode: 8 bits transferred, high byte of SP = 1


Syntax          | Name                      | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
TAX             | Transfer A to X           | AA    | 1     | 2
TAY             | Transfer A to Y           | A8    | 1     | 2
TCD             | Transfer 16 bit A to DP   | 5B    | 1     | 2
TCS             | Transfer 16 bit A to SP   | 1B    | 1     | 2
TDC             | Transfer DP to 16 bit A   | 7B    | 1     | 2
TSC             | Transfer SP to 16 bit A   | 3B    | 1     | 2
TSX             | Transfer SP to X          | BA    | 1     | 2
TXA             | Transfer X to A           | 8A    | 1     | 2
TXS             | Transfer X to SP          | 9A    | 1     | 2
TXY             | Transfer X to Y           | 9B    | 1     | 2
TYA             | Transfer Y to A           | 98    | 1     | 2
TYX             | Transfer Y to X           | BB    | 1     | 2




TRB - Test and Reset Memory Bits Against Accumulator
====================================================

**Flags affected**: `------z-`

`M` ← `M & (~A)`
<br/>
<br/>`z` ← Set if logical AND of memory and Accumulator is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
TRB addr        | Absolute                  | 1C    | 3     | 6 | +2 if m=0
TRB dp          | Direct Page               | 14    | 2     | 5 | +2 if m=0, +1 if DP.l ≠ 0



TSB - Test and Set Memory Bits Against Accumulator
==================================================

**Flags affected**: `------z-`

`M` ← `M | A`
<br/>
<br/>`z` ← Set if logical AND of memory and Accumulator is zero


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
TSB addr        | Absolute                  | 0C    | 3     | 6 | +2 if m=0
TSB dp          | Direct Page               | 04    | 2     | 5 | +2 if m=0, +1 if DP.l ≠ 0



WAI - Wait for Interrupt
========================

**Flags affected**: `--------`

`RDY pin` ← `0`
<br/>`wait for NMI, IRQ or ABORT signal`
<br/>`execute interrupt handler if interrupt is not masked`
<br/>`RDY pin` ← `1`

When the `RDY` (Ready) pin is pulled low the processor enters a low
power mode.

This instruction is useful when you want as little delay as possible
between the interrupt signal and the processor executing the interrupt
handler.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
WAI             | Implied                   | CB    | 1     | 3 | additional cycles needed by interrupt handler to restart the processor



WDM - Reserved for Future Expansion
===================================

**Flags affected**: `--------`

&mdash;

On the SNES it does nothing. This instruction should not be used in your
program.

Note, the bsnes-plus debugger can set a breakpoint on this instruction.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
WDM             | Implied                   | 42    | 2     | 2 | 



XBA - Exchange the B and A Accumulators
=======================================

**Flags affected**: `n-----z-`

<tt>T&nbsp;</tt> ← `Ah`
<br/>`Ah` ← `Al`
<br/>`Al` ← `T`
<br/>
<br/><tt>n&nbsp;</tt> ← Value of bit 7 of the new Accumulator (even in 16 bit mode)
<br/><tt>z&nbsp;</tt> ← Set if bits 0-7 of the new Accumulator are 0 (even in 16 bit mode)


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
XBA             | Implied                   | EB    | 1     | 3



XCE - Exchange Carry and Emulation Bits
=======================================

**Flags affected**: `--mx---c : e`

`c` ← Previous e flag
<br/>`e` ← Previous c flag
<br/>`m` set if enabling native mode
<br/>`x` set if enabling native mode

Note: If bit 4 (x) of the status register is set, then the high byte of
the index registers will be set to 0.


Syntax          | Addressing Mode           | Opcode| Bytes | Cycles | Extra
----------------|---------------------------|-------|-------|--------|--------
XCE             | Implied                   | FB    | 1     | 2


Sources
=======
 * 65816info.txt (author unknown)
 * Programming the 65816, by David Eyes and Ron Lichty
 * [All_About_Your_64 - 65816 Reference](http://www.unusedino.de/ec64/technical/aay/c64/ebmain.htm),
   by Ninja/The Dreams in 1995-2005
 * [higan source code](https://gitlab.com/higan/higan/tree/master/higan/processor/wdc65816), by [byuu](https://byuu.org/)

