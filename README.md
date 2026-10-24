# 8085 Microprocessor Laboratory Manual 

 

**Course:** Microprocessors and Interfacing 

**Target audience:** B.Tech (Electronics and Communication / Electrical / Computer Science), second or third year 

**Prerequisites:** Number systems (binary, hexadecimal), basic digital electronics 

 

--- 

 

## Table of Contents 

 

1. [General Instructions](#general-instructions) 

2. [8085 Quick Reference](#8085-quick-reference) 

3. [How to Enter and Run a Program on the Trainer Kit](#how-to-enter-and-run-a-program-on-the-trainer-kit) 

4. [Experiment 1: 8-bit Addition](#experiment-1-8-bit-addition) 

5. [Experiment 2: 8-bit Subtraction](#experiment-2-8-bit-subtraction) 

6. [Experiment 3: 16-bit Addition](#experiment-3-16-bit-addition) 

7. [Experiment 4: 8-bit Multiplication](#experiment-4-8-bit-multiplication) 

8. [Experiment 5: 8-bit Division](#experiment-5-8-bit-division) 

9. [Experiment 6: Largest Number in an Array](#experiment-6-largest-number-in-an-array) 

10. [Experiment 7: Sorting an Array in Ascending Order](#experiment-7-sorting-an-array-in-ascending-order) 

11. [Experiment 8: Block Data Transfer](#experiment-8-block-data-transfer) 

12. [Experiment 9: 1's and 2's Complement of an 8-bit Number](#experiment-9-1s-and-2s-complement-of-an-8-bit-number) 

13. [Experiment 10: Interfacing the 8255 PPI (LEDs and Switches)](#experiment-10-interfacing-the-8255-ppi-leds-and-switches) 

14. [Appendix A: Instruction Summary](#appendix-a-instruction-summary) 

15. [Appendix B: Lab Report Format](#appendix-b-lab-report-format) 

 

--- 

 

## General Instructions 

 

1. Read the experiment fully before coming to the lab. 

2. Write the algorithm and program in your record before running it on the kit. 

3. Switch off the kit power before connecting or disconnecting any cable or interface board. 

4. Do not change the monitor program area of the trainer kit. Use only the user RAM area specified in your kit manual. 

5. Record the input data, expected result, and observed result in the observation table for every experiment. 

6. Get each experiment checked and signed by the instructor before starting the next one. 

 

> **Note on addresses:** All programs in this manual start at address `2000H` and use data locations in the `2050H` to `20FFH` range. These are common user-RAM locations on many 8085 trainer kits. If your kit uses a different user RAM range, change the addresses accordingly. 

 

--- 

 

## 8085 Quick Reference 

 

### Registers 

 

| Register | Size | Purpose | 

|---|---|---| 

| A (Accumulator) | 8-bit | Main register for arithmetic and logic operations | 

| B, C, D, E, H, L | 8-bit | General purpose; usable as pairs BC, DE, HL | 

| HL pair | 16-bit | Memory pointer; the pseudo-register **M** means the memory location addressed by HL | 

| SP | 16-bit | Stack pointer | 

| PC | 16-bit | Program counter | 

 

### Flags 

 

| Bit | D7 | D6 | D5 | D4 | D3 | D2 | D1 | D0 | 

|---|---|---|---|---|---|---|---|---| 

| Flag | S | Z | X | AC | X | P | X | CY | 

 

| Flag | Meaning | 

|---|---| 

| S (Sign) | Set if bit D7 of the result is 1 | 

| Z (Zero) | Set if the result is zero | 

| AC (Auxiliary carry) | Set on carry from bit 3 to bit 4 | 

| P (Parity) | Set if the result has an even number of 1s | 

| CY (Carry) | Set on carry out of bit 7 (or borrow in subtraction) | 

 

### Addressing modes used in this manual 

 

| Mode | Example | Description | 

|---|---|---| 

| Immediate | `MVI A, 05H` | Data is part of the instruction | 

| Register | `MOV B, A` | Data is in a register | 

| Direct | `LDA 2050H` | Address is part of the instruction | 

| Register indirect | `MOV A, M` | Address is held in the HL pair | 

 

--- 

 

## How to Enter and Run a Program on the Trainer Kit 

 

The exact key names depend on your kit. The general procedure is: 

 

1. Switch on the kit and press **RESET**. 

2. Press the key for **examine memory** or **substitute memory** and enter the start address (for example `2000`). 

3. Enter the hex machine code (opcode) of each instruction, byte by byte, pressing **NEXT** after each byte. The opcode of every instruction is given in the 8085 instruction set table. 

4. Enter the input data at the data addresses given in the experiment. 

5. Press **RESET**, then **GO**, enter the start address `2000`, and press **EXEC**. 

6. The program ends at `HLT`. Press **RESET**, then examine the result memory locations to read the output. 

 

> **Student task:** For each program, fill in the hex opcode for every instruction in your record book, using the instruction set table. 

 

--- 

 

## Experiment 1: 8-bit Addition 

 

### Aim 

To add two 8-bit numbers stored in memory and store the sum and the carry in memory. 

 

### Apparatus 

8085 microprocessor trainer kit with hex keyboard. 

 

### Theory 

The `ADD` instruction adds the contents of a register or memory location to the accumulator. If the sum exceeds 255 (`FFH`), the carry flag (CY) is set. To keep the carry, we clear a register-free location by loading `00H` into A and adding it back with `ACI 00H`, which adds the carry flag to the accumulator. Note that `MVI` does not change any flag, so the carry from `ADD` is preserved. 

 

### Algorithm 

1. Load the first number from memory into the accumulator and move it to register B. 

2. Load the second number into the accumulator. 

3. Add B to the accumulator. 

4. Store the sum in memory. 

5. Load `00H` into A and add the carry flag using `ACI 00H`. 

6. Store the result (carry) in memory. 

7. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | First number | 

| 2051H | Second number | 

| 2052H | Sum (output) | 

| 2053H | Carry (output) | 

 

### Program 

 

```asm 

        LDA 2050H       ; A <- first number 

        MOV B, A        ; B <- A 

        LDA 2051H       ; A <- second number 

        ADD B           ; A <- A + B, CY set if overflow 

        STA 2052H       ; store sum 

        MVI A, 00H      ; A <- 0 (flags unchanged) 

        ACI 00H         ; A <- 0 + 0 + CY 

        STA 2053H       ; store carry 

        HLT 

``` 

 

### Observation table 

 

| Case | 2050H | 2051H | Expected 2052H | Expected 2053H | Observed 2052H | Observed 2053H | 

|---|---|---|---|---|---|---| 

| 1 | 25 | 3A | 5F | 00 | | | 

| 2 | F0 | 20 | 10 | 01 | | | 

| 3 | (your choice) | (your choice) | | | | | 

 

### Result 

The sum of two 8-bit numbers was obtained and the carry was stored separately. 

 

### Viva questions 

1. What is the difference between `ADD` and `ADC`? 

2. Why does `MVI A, 00H` not disturb the carry flag? 

3. Which flags are affected by the `ADD` instruction? 

 

--- 

 

## Experiment 2: 8-bit Subtraction 

 

### Aim 

To subtract one 8-bit number from another and store the result and the borrow. 

 

### Theory 

The `SUB` instruction subtracts the operand from the accumulator. If the accumulator value is smaller than the operand, the result is negative: it appears in 2's complement form and the carry flag (CY) is set to indicate a borrow. 

 

### Algorithm 

1. Point HL to the first number. 

2. Load the first number into A. 

3. Increment HL to point to the second number. 

4. Subtract the second number from A. 

5. Store the result. 

6. Capture the borrow (CY) using `MVI A, 00H` and `ACI 00H`, then store it. 

7. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Minuend (first number) | 

| 2051H | Subtrahend (second number) | 

| 2052H | Difference (output) | 

| 2053H | Borrow (output): 00 = no borrow, 01 = borrow | 

 

### Program 

 

```asm 

        LXI H, 2050H    ; HL <- 2050H 

        MOV A, M        ; A <- first number 

        INX H           ; HL <- 2051H 

        SUB M           ; A <- A - second number 

        STA 2052H       ; store difference 

        MVI A, 00H 

        ACI 00H         ; A <- borrow flag 

        STA 2053H       ; store borrow 

        HLT 

``` 

 

### Observation table 

 

| Case | 2050H | 2051H | Expected 2052H | Expected 2053H | Observed 2052H | Observed 2053H | 

|---|---|---|---|---|---|---| 

| 1 | 50 | 20 | 30 | 00 | | | 

| 2 | 20 | 50 | D0 | 01 | | | 

 

### Result 

Subtraction was performed; when the first number was smaller, the borrow was set and the difference appeared in 2's complement form. 

 

### Viva questions 

1. What does `D0H` with a borrow of 1 represent in case 2? 

2. How is the carry flag interpreted after a subtraction? 

3. Which instruction would you use to subtract with the borrow included? 

 

--- 

 

## Experiment 3: 16-bit Addition 

 

### Aim 

To add two 16-bit numbers and store the 17-bit result (16-bit sum plus carry). 

 

### Theory 

The 8085 stores 16-bit data in memory in **little-endian** order: the lower byte at the lower address and the higher byte at the next address. `LHLD addr` loads the L register from `addr` and the H register from `addr + 1`. `DAD rp` adds a 16-bit register pair to HL and affects only the carry flag. `SHLD addr` stores L at `addr` and H at `addr + 1`. 

 

### Algorithm 

1. Load the first 16-bit number into HL, then exchange it into DE using `XCHG`. 

2. Load the second 16-bit number into HL. 

3. Add DE to HL using `DAD D`. 

4. Store HL (the 16-bit sum) in memory. 

5. Capture the carry and store it. 

6. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H, 2051H | First number (low byte, high byte) | 

| 2052H, 2053H | Second number (low byte, high byte) | 

| 2054H, 2055H | Sum (low byte, high byte) | 

| 2056H | Carry | 

 

### Program 

 

```asm 

        LHLD 2050H      ; HL <- first number 

        XCHG            ; DE <- first number 

        LHLD 2052H      ; HL <- second number 

        DAD D           ; HL <- HL + DE, CY updated 

        SHLD 2054H      ; store 16-bit sum 

        MVI A, 00H 

        ACI 00H         ; A <- carry 

        STA 2056H       ; store carry 

        HLT 

``` 

 

### Observation table 

 

| Case | First number | Second number | Expected sum | Expected carry | Observed sum | Observed carry | 

|---|---|---|---|---|---|---| 

| 1 | 1234H | 2BCDH | 3E01H | 00 | | | 

| 2 | F000H | 2000H | 1000H | 01 | | | 

 

For case 1, memory should contain: `2050H = 34`, `2051H = 12`, `2052H = CD`, `2053H = 2B`. The result is `2054H = 01`, `2055H = 3E`. 

 

### Result 

Two 16-bit numbers were added and the result was stored in little-endian order with the carry. 

 

### Viva questions 

1. What is little-endian storage? 

2. Which flags does `DAD` affect? 

3. What does the `XCHG` instruction do? 

 

--- 

 

## Experiment 4: 8-bit Multiplication 

 

### Aim 

To multiply two 8-bit numbers using repeated addition and store the 16-bit product. 

 

### Theory 

The 8085 has no multiply instruction. Multiplication is performed by adding the multiplicand to itself as many times as the multiplier. The product of two 8-bit numbers can be up to 16 bits (`FFH Ã— FFH = FE01H`), so a 16-bit accumulation in HL is used with `DAD D`. 

 

### Algorithm 

1. Load the multiplicand into E and clear D, so DE holds the 16-bit multiplicand. 

2. Load the multiplier into C (the counter). 

3. Clear HL (the product register). 

4. If the multiplier is zero, go to the store step. 

5. Repeat: add DE to HL, decrement C, until C becomes zero. 

6. Store HL (the product) in memory. 

7. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Multiplicand | 

| 2051H | Multiplier | 

| 2052H | Product, low byte (output) | 

| 2053H | Product, high byte (output) | 

 

### Program 

 

```asm 

        LDA 2050H       ; A <- multiplicand 

        MOV E, A 

        MVI D, 00H      ; DE <- 16-bit multiplicand 

        LDA 2051H       ; A <- multiplier 

        MOV C, A        ; C <- counter 

        LXI H, 0000H    ; HL <- 0 (product) 

        MOV A, C 

        ORA A           ; test multiplier for zero 

        JZ  STORE 

LOOP:   DAD D           ; HL <- HL + DE 

        DCR C 

        JNZ LOOP 

STORE:  SHLD 2052H      ; store product 

        HLT 

``` 

 

### Observation table 

 

| Case | 2050H | 2051H | Expected 2052H | Expected 2053H | Observed 2052H | Observed 2053H | 

|---|---|---|---|---|---|---| 

| 1 | 0C | 0A | 78 | 00 | | | 

| 2 | FF | FF | 01 | FE | | | 

| 3 | 05 | 00 | 00 | 00 | | | 

 

### Result 

Multiplication by repeated addition gave the correct 16-bit product. 

 

### Viva questions 

1. Why is a 16-bit register pair needed to hold the product? 

2. What happens if the zero check (`ORA A`, `JZ`) is removed and the multiplier is `00H`? 

3. How would shift-and-add multiplication be faster than repeated addition? 

 

--- 

 

## Experiment 5: 8-bit Division 

 

### Aim 

To divide one 8-bit number by another using repeated subtraction and store the quotient and remainder. 

 

### Theory 

Division is performed by subtracting the divisor from the dividend repeatedly and counting the number of subtractions. This count is the quotient. When the dividend becomes smaller than the divisor, the remaining value is the remainder. The `CMP` instruction compares without changing the accumulator: CY is set if A is less than the operand. 

 

### Algorithm 

1. Load the divisor into B and the dividend into A. 

2. Clear C (the quotient). 

3. Compare A with B. If A is less than B, go to step 6. 

4. Subtract B from A and increment C. 

5. Go to step 3. 

6. Store A as the remainder and C as the quotient. 

7. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Dividend | 

| 2051H | Divisor (must not be zero) | 

| 2052H | Quotient (output) | 

| 2053H | Remainder (output) | 

 

### Program 

 

```asm 

        LDA 2051H       ; A <- divisor 

        MOV B, A        ; B <- divisor 

        LDA 2050H       ; A <- dividend 

        MVI C, 00H      ; quotient = 0 

LOOP:   CMP B           ; compare A with B 

        JC  DONE        ; if A < B, finished 

        SUB B           ; A <- A - B 

        INR C           ; quotient + 1 

        JMP LOOP 

DONE:   STA 2053H       ; store remainder 

        MOV A, C 

        STA 2052H       ; store quotient 

        HLT 

``` 

 

> **Caution:** If the divisor is `00H`, the loop never ends. Always make sure the divisor is non-zero. As an exercise, add a zero check before the loop. 

 

### Observation table 

 

| Case | 2050H | 2051H | Expected 2052H | Expected 2053H | Observed 2052H | Observed 2053H | 

|---|---|---|---|---|---|---| 

| 1 | 64 | 07 | 0E | 02 | | | 

| 2 | 09 | 03 | 03 | 00 | | | 

| 3 | 03 | 09 | 00 | 03 | | | 

 

### Result 

Division by repeated subtraction produced the correct quotient and remainder. 

 

### Viva questions 

1. How does `CMP` differ from `SUB`? 

2. What would happen if the divisor is zero? 

3. In case 3, why is the quotient zero? 

 

--- 

 

## Experiment 6: Largest Number in an Array 

 

### Aim 

To find the largest number in an array of 8-bit numbers. 

 

### Theory 

The first element is assumed to be the largest. Each next element is compared with the current largest using `CMP M`. If the current value in A is smaller (CY = 1), it is replaced by the new element. 

 

### Algorithm 

1. Point HL to the count byte and load the count into C. 

2. Load the first element into A. 

3. Decrement C. If C is zero, the array has one element and A is the answer. 

4. For each remaining element: increment HL, compare A with M, and if A is smaller, load M into A. 

5. Decrement C and repeat until C is zero. 

6. Store A in memory. 

7. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Array size N | 

| 2051H onwards | Array elements | 

| 2060H | Largest number (output) | 

 

### Program 

 

```asm 

        LXI H, 2050H    ; HL <- address of count 

        MOV C, M        ; C <- N 

        INX H           ; HL <- first element 

        MOV A, M        ; A <- first element (assumed largest) 

        DCR C           ; remaining comparisons 

        JZ  STORE       ; only one element 

LOOP:   INX H           ; next element 

        CMP M           ; compare A with M 

        JNC SKIP        ; if A >= M, keep A 

        MOV A, M        ; else A <- M 

SKIP:   DCR C 

        JNZ LOOP 

STORE:  STA 2060H       ; store the largest 

        HLT 

``` 

 

### Observation table 

 

| N (2050H) | Array (from 2051H) | Expected 2060H | Observed 2060H | 

|---|---|---|---| 

| 05 | 12, 4A, 09, F1, 33 | F1 | | 

| 03 | 07, 07, 07 | 07 | | 

 

### Result 

The largest element of the array was found and stored at 2060H. 

 

### Viva questions 

1. How would you modify the program to find the smallest number? 

2. What does `JNC` test? 

3. Why do we initialise A with the first element instead of `00H`? 

 

--- 

 

## Experiment 7: Sorting an Array in Ascending Order 

 

### Aim 

To sort an array of 8-bit unsigned numbers in ascending order using the bubble sort method. 

 

### Theory 

In bubble sort, adjacent elements are compared and swapped if they are in the wrong order. After each full pass, the largest remaining element moves to the end. For N elements, N âˆ’ 1 passes are needed. 

 

### Algorithm 

1. Load N. If N is less than 2, halt (nothing to sort). 

2. Set D = N âˆ’ 1 (number of passes). 

3. For each pass: set HL to the first element and C = D (number of comparisons). 

4. Load the current element into A, move HL to the next element, and compare. 

5. If current element is greater than the next, swap them. 

6. Decrement C and repeat from step 4 until C is zero. 

7. Decrement D and repeat from step 3 until D is zero. 

8. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Array size N (must be 2 or more) | 

| 2051H onwards | Array elements (sorted in place) | 

 

### Program 

 

```asm 

        LDA 2050H       ; A <- N 

        CPI 02H 

        JC  END         ; N < 2, nothing to sort 

        MOV D, A 

        DCR D           ; D <- N - 1 passes 

OUTER:  LXI H, 2051H    ; HL <- first element 

        MOV C, D        ; C <- comparisons in this pass 

INNER:  MOV A, M        ; A <- current element 

        INX H           ; HL <- next element 

        CMP M           ; A - next 

        JC  NOSWAP      ; current < next 

        JZ  NOSWAP      ; current = next 

        MOV B, M        ; B <- next element 

        MOV M, A        ; next position <- current element 

        DCX H           ; back to current position 

        MOV M, B        ; current position <- next element 

        INX H           ; return to next position 

NOSWAP: DCR C 

        JNZ INNER 

        DCR D 

        JNZ OUTER 

END:    HLT 

``` 

 

### Observation table 

 

| N | Input array | Expected output array | Observed output | 

|---|---|---|---| 

| 05 | 45, 12, 99, 03, 27 | 03, 12, 27, 45, 99 | | 

| 04 | 04, 03, 02, 01 | 01, 02, 03, 04 | | 

| 03 | 01, 02, 03 (already sorted) | 01, 02, 03 | | 

 

### Result 

The array was sorted in ascending order in place. 

 

### Viva questions 

1. What is the maximum number of passes needed for N elements? 

2. How can the program be changed to sort in descending order? 

3. How could you stop early if a pass makes no swaps? 

 

--- 

 

## Experiment 8: Block Data Transfer 

 

### Aim 

To transfer a block of data from one set of memory locations to another. 

 

### Theory 

Register indirect addressing is used. HL points to the source and DE points to the destination. `MOV A, M` reads from the source, and `STAX D` writes the accumulator to the address held in DE. Both pointers are incremented after each byte. 

 

### Algorithm 

1. Load the number of bytes into C. 

2. Load the source address into HL and the destination address into DE. 

3. Read a byte from the source into A and store it at the destination. 

4. Increment both pointers and decrement C. 

5. Repeat until C is zero. 

6. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Number of bytes N (must be non-zero) | 

| 2060H onwards | Source block | 

| 2070H onwards | Destination block (output) | 

 

> The source and destination blocks must not overlap. 

 

### Program 

 

```asm 

        LDA 2050H       ; A <- N 

        MOV C, A        ; C <- counter 

        LXI H, 2060H    ; HL <- source address 

        LXI D, 2070H    ; DE <- destination address 

LOOP:   MOV A, M        ; A <- source byte 

        STAX D          ; destination <- A 

        INX H 

        INX D 

        DCR C 

        JNZ LOOP 

        HLT 

``` 

 

### Observation table 

 

| N | Source (from 2060H) | Expected destination (from 2070H) | Observed | 

|---|---|---|---| 

| 04 | 11, 22, 33, 44 | 11, 22, 33, 44 | | 

| 06 | A1, B2, C3, D4, E5, F6 | A1, B2, C3, D4, E5, F6 | | 

 

### Result 

The block of data was copied from the source to the destination locations. 

 

### Viva questions 

1. Why can `STAX` be used only with the BC and DE pairs? 

2. What goes wrong if the two blocks overlap? 

3. How would you transfer the block in reverse order? 

 

--- 

 

## Experiment 9: 1's and 2's Complement of an 8-bit Number 

 

### Aim 

To find the 1's complement and 2's complement of an 8-bit number. 

 

### Theory 

The 1's complement is obtained by inverting every bit, using the `CMA` instruction. The 2's complement is the 1's complement plus 1. The 2's complement is used to represent negative numbers in signed arithmetic. 

 

### Algorithm 

1. Load the number into the accumulator. 

2. Complement the accumulator (`CMA`) and store it as the 1's complement. 

3. Add 1 to the accumulator (`ADI 01H`) and store it as the 2's complement. 

4. Halt. 

 

### Data locations 

 

| Address | Content | 

|---|---| 

| 2050H | Number | 

| 2051H | 1's complement (output) | 

| 2052H | 2's complement (output) | 

 

### Program 

 

```asm 

        LDA 2050H       ; A <- number 

        CMA             ; A <- 1's complement 

        STA 2051H       ; store 1's complement 

        ADI 01H         ; A <- 2's complement 

        STA 2052H       ; store 2's complement 

        HLT 

``` 

 

### Observation table 

 

| 2050H | Expected 2051H | Expected 2052H | Observed 2051H | Observed 2052H | 

|---|---|---|---|---| 

| 5A | A5 | A6 | | | 

| 01 | FE | FF | | | 

| 00 | FF | 00 | | | 

 

### Result 

The 1's and 2's complements of the given number were obtained. 

 

### Viva questions 

1. Does `CMA` affect any flags? 

2. What is the 2's complement of `00H`, and why? 

3. How would you find the 2's complement of a 16-bit number? 

 

### Exercise 

Extend the program to find the 2's complement of a 16-bit number stored at 2050H and 2051H. 

 

--- 

 

## Experiment 10: Interfacing the 8255 PPI (LEDs and Switches) 

 

### Aim 

To interface the 8255 Programmable Peripheral Interface with the 8085 and read the status of switches on one port to display it on LEDs connected to another port. 

 

### Apparatus 

8085 trainer kit with an 8255 interface board, 8 DIP switches, 8 LEDs, connecting cables. 

 

### Theory 

The 8255 has three 8-bit ports (A, B, C) and a control register. In **Mode 0** (basic input/output), each port acts as a simple input or output port. The mode and direction are set by writing a **control word** to the control register. 

 

Port addresses depend on how the 8255 is wired on your kit. This manual assumes: 

 

| Register | Address | 

|---|---| 

| Port A | 80H | 

| Port B | 81H | 

| Port C | 82H | 

| Control register | 83H | 

 

Check your kit manual and change these values if needed. 

 

**Control word format (I/O mode):** 

 

| D7 | D6 D5 | D4 | D3 | D2 | D1 | D0 | 

|---|---|---|---|---|---|---| 

| 1 (mode set) | Group A mode | Port A (1 = in) | Port C upper (1 = in) | Group B mode | Port B (1 = in) | Port C lower (1 = in) | 

 

For Port A as output, Port B as input, Port C as output, both groups in Mode 0: 

 

`1 00 0 0 0 1 0` = `1000 0010` = **82H** 

 

### Algorithm 

1. Write the control word `82H` to the control register. 

2. Read the switch status from Port B. 

3. Write the same value to Port A to light the LEDs. 

4. Repeat from step 2 continuously. 

 

### Connections 

- Connect the 8 switches to the Port B pins (PB0 to PB7). 

- Connect the 8 LEDs to the Port A pins (PA0 to PA7). 

 

### Program 

 

```asm 

        MVI A, 82H      ; control word: A out, B in, C out, Mode 0 

        OUT 83H         ; write to control register 

LOOP:   IN  81H         ; A <- switches on Port B 

        OUT 80H         ; LEDs on Port A <- A 

        JMP LOOP        ; repeat forever 

``` 

 

### Observation table 

 

| Switch pattern (PB7 to PB0) | Expected LED pattern (PA7 to PA0) | Observed | 

|---|---|---| 

| 0000 0001 | 0000 0001 | | 

| 1010 1010 | 1010 1010 | | 

| 1111 1111 | 1111 1111 | | 

 

### Result 

The 8255 was configured in Mode 0, and the switch status was successfully mirrored on the LEDs. 

 

### Viva questions 

1. What is the purpose of the control word in the 8255? 

2. How does the BSR mode differ from the I/O mode? 

3. What control word is needed if all three ports are output ports? 

 

### Exercise 

Modify the program so that the LEDs show the complement of the switch pattern. 

 

--- 

 

## Appendix A: Instruction Summary 

 

| Instruction | Operation | Flags affected | 

|---|---|---| 

| `MVI r, data` | Load register with 8-bit data | None | 

| `LXI rp, data16` | Load register pair with 16-bit data | None | 

| `MOV r1, r2` | Copy r2 into r1 (r can be M) | None | 

| `LDA addr` / `STA addr` | Load or store accumulator from or to memory | None | 

| `LHLD addr` / `SHLD addr` | Load or store HL from or to memory | None | 

| `LDAX rp` / `STAX rp` | Load or store accumulator via BC or DE | None | 

| `XCHG` | Exchange HL and DE | None | 

| `ADD r` / `ADI data` | Add to accumulator | All | 

| `ADC r` / `ACI data` | Add with carry | All | 

| `SUB r` / `SUI data` | Subtract from accumulator | All | 

| `DAD rp` | Add register pair to HL | CY only | 

| `INR r` / `DCR r` | Increment or decrement register | All except CY | 

| `INX rp` / `DCX rp` | Increment or decrement register pair | None | 

| `CMP r` / `CPI data` | Compare with accumulator (CY = 1 if A < operand, Z = 1 if equal) | All | 

| `CMA` | Complement accumulator | None | 

| `ORA r` | Logical OR with accumulator | All (CY cleared) | 

| `JMP addr` | Unconditional jump | None | 

| `JZ` / `JNZ` | Jump if zero or not zero | None | 

| `JC` / `JNC` | Jump if carry or no carry | None | 

| `IN port` / `OUT port` | Read from or write to an I/O port | None | 

| `HLT` | Halt the processor | None | 

 

--- 

 

## Appendix B: Lab Report Format 

 

Each experiment in your record should contain, in this order: 

 

1. **Experiment number and title** 

2. **Aim** 

3. **Apparatus** 

4. **Theory** (brief, in your own words) 

5. **Algorithm or flowchart** 

6. **Program** with addresses, labels, mnemonics, hex codes, and comments 

7. **Observation table** with input, expected output, and observed output 

8. **Result** 

9. **Viva answers** 

 
