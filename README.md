# RISC-V Processor Simulator
This project implements a functional simulator for a subset of the RISC-V instruction set architecture. It features two distinct processor models: a Single-Stage Core and a Five-Stage Pipelined Core. The simulator reads instruction and data memory from text files, executes the program, and outputs the architectural state and performance metrics.

## Supported Instructions
The simulator supports the following instruction types:

R-Type: add, sub, xor, or, and

I-Type: addi, xori, ori, andi, lw

S-Type: sw

B-Type: beq, bne

J-Type: jal

## Project Structure
InsMem: Handles instruction memory loading and reading (from imem.txt).

DataMem: Manages data memory (from dmem.txt) and handles 32-bit word alignment for reads and writes.

RegisterFile: Implements the 32 architectural registers (x0 is hardwired to 0).

SingleStageCore: Executes one instruction per cycle by completing all phases (Fetch, Decode, Execute, Memory, Write-back) in a single step.

FiveStageCore: Implements a pipelined architecture.

IF: Instruction Fetch

ID: Instruction Decode & Hazard Detection

EX: Execution / ALU Operations

MEM: Data Memory Access

WB: Register Write-back

## Pipelining & Hazard Handling
The FiveStageCore includes logic to handle common pipeline hazards to ensure architectural correctness:

Data Hazards:

Forwarding: Implemented logic to detect dependencies between the current instruction in the ID stage and results currently in the MEM or WB stages.

Stalling: The pipeline introduces a hazard_nop (stall) when a Load-Use dependency is detected (e.g., a lw followed immediately by an instruction using that register).

Control Hazards:

Branching: For jal and conditional branches (beq, bne), the pipeline flushes the subsequent instruction (sets nop) and updates the PC to the target address if the branch is taken.

## How to Run
### Prerequisites

Python 3.x

Input files (imem.txt, dmem.txt) located in the input directory.

### Execution

Run the simulator by providing the directory containing your input files via the --iodir argument:

Bash
python main.py --iodir ./path/to/your/input/
### Output Files

The simulator generates several files in the specified directory:

SS_RFResult.txt / FS_RFResult.txt: Register file state for each cycle.

StateResult_SS.txt / StateResult_FS.txt: Detailed internal state of the core per cycle.

SS_DMEMResult.txt / FS_DMEMResult.txt: Final state of data memory.

PerformanceMetrics_Result.txt: Summary of cycles, instruction counts, CPI, and IPC.

## Performance Metrics
The simulator calculates the following for both cores:

Total Cycles: The number of clock cycles to complete the program.

Instruction Count: Total number of instructions retired.

CPI (Cycles Per Instruction): Average cycles per retired instruction.

IPC (Instructions Per Cycle): Throughput of the processor.
