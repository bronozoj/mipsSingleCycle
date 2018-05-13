# Single Cycle MIPS Processor

This is a work in progress implementation of a MIPS32 compliant processor using Logisim. Implementation details (documentation) will also be contained in this file.

## ALU

The ALU, or arithmetic logic unit accepts two 32-bit operands `op1` and `op2`, a shift amount `shiftamt`, and a 6-bit opcode based on the R-type instruction format as shown below:

6-bit opcode | 5-bit Rs | 5-bit Rt | 5-bit Rd | 5-bit shift constant | 6-bit ALU opcode
:---: | :---: | :---: | :---: | :---:| :---:
`000000` | `op1` register address | `op1` register address | `dest` register address | shift amount input | ALU operation

The output of the ALU is the value calculated, a zero flag `z`, an overflow flag `v`, and a carry-out flag `c`. These flags can trigger breakpoints or can be used in other parts of the processor. The following ALU opcodes and their equivalent operation descriptions are shown in the table:

ALU opcode | Description | Instruction
:---: | :--- | :---:
`000000` | Shifts `op1` by shift amount input | `sll`
`000010` | Shifts `op1` by shift amount input | `srl`
`000011` | Shifts `op1` by shift amount input | `sra`
`000100` | Shifts `op1` by last 5 bits of `op2` | `sllv`
`000110` | Shifts `op1` by last 5 bits of `op2` | `srlv`
`000111` | Shifts `op1` by last 5 bits of `op2` | `srav`
`00100x` | Outputs `op1` (non-standard opcode) | `jr`, `jalr`
`010000` | Outputs value of internal `HI` register | `mfhi`
`010001` | Transfers value of `op1` to internal `HI` register | `mthi`
`010010` | Outputs value of internal `LO` register | `mflo`
`010011` | Transfers value of `op1` to internal `LO` register | `mtlo`
`011000` | Multiplies `op1` and `op2` using signed notation and stores result to internal `HI` and `LO` registers | `mult`
`011001` | Multiplies `op1` and `op2` using unsigned notation and stores result to internal `HI` and `LO` registers | `multu`
`011010` | Divides `op1` and `op2` using signed notation and stores result to internal `HI` and `LO` registers | `div`
`011011` | Divides `op1` and `op2` using unsigned notation and stores result to internal `HI` and `LO` registers | `divu`
`100000` | Adds `op1` and `op2` using signed notation | `add`, `addi`
`100001` | Adds `op1` and `op2` using unsigned notation | `addu`, `addiu`
`100010` | Subtracts `op1` and `op2` using signed notation | `sub`
`100011` | Subtracts `op1` and `op2` using unsigned notation | `subu`
`100100` | Bitwise AND `op1` and `op2` | `and`, `andi`
`100101` | Bitwise OR `op1` and `op2` | `or`, `ori`
`100110` | Bitwise Exclusive OR `op1` and `op2` | `xor`, `xori`
`100111` | Bitwise NOR `op1` and `op1` | `nor`
`10100x` | Shift `op2` by 16 bits (non-standard opcode) | `lui`
`101010` | Set result to `1` when `op1` is less than `op2` using signed notation | `slt`, `slti`
`101011` | Set result to `1` when `op1` is less than `op2` using unsigned notation | `sltu`, `sltiu`