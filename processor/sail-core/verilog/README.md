# Verilog Modules

This directory contains the Verilog sources for the RV32I pipeline used in this repository. Each file implements a small portion of the processor. The same set of files can also be accessed via the symlinked `processor/verilog` directory.

| Module | Description |
|-------|-------------|
| [`CSR.v`](CSR.v) | Implements the control and status register file (CSRs) providing read/write access to system registers. |
| [`adder.v`](adder.v) | Simple 32‑bit adder used for branch target calculation and PC increments. |
| [`alu.v`](alu.v) | Arithmetic Logic Unit performing the RV32I arithmetic and logical operations. It also outputs branch enable signals. |
| [`alu_control.v`](alu_control.v) | Decodes opcode/function fields and generates the control signals that drive the ALU. |
| [`branch_decide.v`](branch_decide.v) | Determines final branch decisions in the MEM stage and flags mispredictions. |
| [`branch_predictor.v`](branch_predictor.v) | Finite‑state machine for basic dynamic branch prediction. |
| [`control_unit.v`](control_unit.v) | High level instruction decoder producing signals such as `RegWrite`, `MemRead`, `Branch`, and `Jump`. |
| [`cpu.v`](cpu.v) | Top level module that ties the pipeline stages together into a working CPU. |
| [`dataMem_mask_gen.v`](dataMem_mask_gen.v) | Generates byte enable and sign extension masks for load/store operations. |
| [`data_mem.v`](data_mem.v) | Simple data memory/cache with a small state machine supporting word, halfword and byte accesses. |
| [`forwarding_unit.v`](forwarding_unit.v) | Detects data hazards and forwards values from later stages back to the ALU to avoid stalls. |
| [`imm_gen.v`](imm_gen.v) | Extracts and sign extends immediates for all instruction formats. |
| [`instruction_mem.v`](instruction_mem.v) | Read‑only instruction memory populated from `program.hex`. |
| [`mux2to1.v`](mux2to1.v) | Generic two‑input 32‑bit multiplexer. |
| [`pipeline_registers.v`](pipeline_registers.v) | Implements IF/ID, ID/EX, EX/MEM, and MEM/WB pipeline registers with reset values. |
| [`program_counter.v`](program_counter.v) | Holds the current PC and updates it each cycle. |
| [`register_file.v`](register_file.v) | Thirty‑two entry register file with forwarding logic. |
