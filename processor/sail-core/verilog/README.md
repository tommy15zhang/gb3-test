# Verilog Modules

This directory contains the Verilog sources for the RV32I pipeline used in this repository. These files are also accessible through the symlinked `processor/verilog` directory.

## Module overview

| Module | Purpose |
|-------|---------|
| [`CSR.v`](CSR.v) | Control and status register file for machine CSRs |
| [`adder.v`](adder.v) | 32‑bit adder for PC updates and branch address math |
| [`alu.v`](alu.v) | Implements RV32I arithmetic/logic operations and branch flags |
| [`alu_control.v`](alu_control.v) | Decodes instruction fields to select an ALU operation |
| [`branch_decide.v`](branch_decide.v) | Final branch decision logic in the MEM stage |
| [`branch_predictor.v`](branch_predictor.v) | Simple finite‑state machine branch predictor |
| [`control_unit.v`](control_unit.v) | Top level instruction decoder producing pipeline controls |
| [`cpu.v`](cpu.v) | Top level CPU tying all pipeline units together |
| [`dataMem_mask_gen.v`](dataMem_mask_gen.v) | Generates byte enable masks and sign extension control |
| [`data_mem.v`](data_mem.v) | Data memory with simple read/write state machine |
| [`forwarding_unit.v`](forwarding_unit.v) | Detects hazards and forwards results to the ALU |
| [`imm_gen.v`](imm_gen.v) | Immediate generator for all instruction formats |
| [`instruction_mem.v`](instruction_mem.v) | Read‑only instruction memory loaded from `program.hex` |
| [`mux2to1.v`](mux2to1.v) | Generic 2‑input 32‑bit multiplexer |
| [`pipeline_registers.v`](pipeline_registers.v) | IF/ID, ID/EX, EX/MEM and MEM/WB pipeline registers |
| [`program_counter.v`](program_counter.v) | Holds and updates the program counter |
| [`register_file.v`](register_file.v) | 32‑register file with synchronous write and forwarding |

## Module details

### [`CSR.v`](CSR.v)
Implements the machine control and status registers. The module exposes a write port and a read port addressed by CSR number. On each rising clock edge it updates the selected CSR if `write` is asserted and outputs the value at `rdAddr_CSR`.

### [`adder.v`](adder.v)
Simple 32‑bit adder. Used primarily for computing `PC + 4` and branch targets. Both operands are 32‑bit values and the output is their sum.

### [`alu.v`](alu.v)
Main arithmetic logic unit for the processor. Supports addition, subtraction, logical operations, shifts and comparisons. It also generates branch enable signals that feed into the branch decision stage.

### [`alu_control.v`](alu_control.v)
Translates opcode, funct3 and funct7 fields into the control signals that drive the ALU. This module isolates the decode logic from the ALU itself and selects the correct operation for each instruction.

### [`branch_decide.v`](branch_decide.v)
Receives branch condition results from the ALU and determines the final branch decision in the MEM stage. It checks the actual branch outcome against the predictor and outputs a misprediction flag when needed.

### [`branch_predictor.v`](branch_predictor.v)
A small two‑bit saturating finite‑state machine that predicts whether a branch will be taken. The predictor computes a potential branch target address and outputs a prediction bit used by the fetch stage.

### [`control_unit.v`](control_unit.v)
High level instruction decoder. Produces pipeline control signals such as `RegWrite`, `MemRead`, `MemWrite`, branch types and CSR access flags based on the current instruction opcode.

### [`cpu.v`](cpu.v)
Top level module of the RV32I pipeline. Instantiates the program counter, memories, register file, ALU, branch predictor and all pipeline registers. It connects the pipeline stages and coordinates the overall data flow.

### [`dataMem_mask_gen.v`](dataMem_mask_gen.v)
Generates byte write‑enable masks and sign‑extension control bits for load and store instructions. Supports byte, halfword and word accesses to the data memory.

### [`data_mem.v`](data_mem.v)
Implements the main data memory. A small state machine handles read and write cycles, including a register mapped to on‑board LEDs. Wait states are inserted through `clk_stall` when memory accesses are in progress.

### [`forwarding_unit.v`](forwarding_unit.v)
Detects data hazards between pipeline stages. When a destination register is being written by a later stage, this unit forwards the value back to the ALU inputs to avoid pipeline stalls.

### [`imm_gen.v`](imm_gen.v)
Extracts immediates from instruction fields for all RISC‑V encoding formats (I‑type, S‑type, B‑type, U‑type and J‑type) and sign‑extends them to 32 bits.

### [`instruction_mem.v`](instruction_mem.v)
Read‑only memory storing the program instructions. The contents are loaded from `program.hex` and indexed by the program counter address.

### [`mux2to1.v`](mux2to1.v)
A generic two‑input multiplexer that selects between 32‑bit values based on a single select signal.

### [`pipeline_registers.v`](pipeline_registers.v)
Defines the register sets between pipeline stages: IF/ID, ID/EX, EX/MEM and MEM/WB. Each register holds a bundle of pipeline signals and is cleared to zero at reset.

### [`program_counter.v`](program_counter.v)
Holds the current program counter value. Updated on each clock edge with the next PC calculated by the branch/PC logic and reset to zero on start‑up.

### [`register_file.v`](register_file.v)
Thirty‑two entry register file with two read ports and one synchronous write port. Includes simple forwarding to support CSR instructions and avoids read‑after‑write hazards.
