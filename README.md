# RISC-V Single-Core CPU (LBU Extension)
RISC-V Single-Core CPU in SystemVerilog. 
Not all instructions are implemented, but enough are supported for full computational capability.
Extended the existing single-cycle RISC-V processor to support the `lbu` (load byte unsigned) instruction. 
The design was simulated and verified in ModelSim.

---

## Overview
Added support for byte-level memory access in a processor originally designed with word-addressable memory. 
Since the data memory returns 32-bit words, additional logic was introduced to correctly extract and 
zero-extend a single byte.

---

## Features Added
* Support for `lbu rd, imm(rs1)`
* Byte selection using `ALUResult[1:0]`
* Zero-extension of selected byte to 32 bits
* New control signal `LBU` to distinguish `lbu` from `lw`

---

## Datapath Changes
* Added a 4-to-1 MUX to select a byte from the 32-bit memory output
* Added zero-extension logic:
  ```
  ZeroExtByte = {24'b0, SelectedByte}
  ```
* Added a 2-to-1 MUX to select between:
  * Full word (lw)
  * Zero-extended byte (lbu)

---

## Control & Decoder Changes
* RegWrite is asserted
* MemWrite is deasserted
* the ALU computes the effective address
* the write-back path selects the lbu result
