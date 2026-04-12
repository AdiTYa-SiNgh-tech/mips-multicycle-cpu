# Multicycle MIPS Processor

Created by: Aditya Singh

A 32-bit **multicycle MIPS processor** designed and implemented from scratch using **SystemVerilog**. The processor is based on the MIPS instruction set architecture and uses an FSM-based control unit to execute instructions over multiple clock cycles.

## Overview

This project implements a simplified MIPS CPU using a reusable RTL datapath and a finite-state-machine (FSM) controller.

Unlike a single-cycle processor, where an instruction is completed in one clock cycle, the multicycle architecture divides instruction execution into multiple stages and reuses hardware such as the ALU across different stages.

### High-Level Architecture

```text
                    ┌──────────────────────┐
                    │    Control Unit     │
                    │     FSM Controller  │
                    └──────────┬───────────┘
                               │
                         Control Signals
                               │
                               ▼
        ┌─────────────────────────────────────────┐
        │                 DATAPATH                │
        │                                         │
        │   ┌─────┐      ┌──────────────┐         │
        │   │ PC  │─────►│ Instruction  │         │
        │   └─────┘      │    Memory    │         │
        │                └──────┬───────┘         │
        │                       │                 │
        │                       ▼                 │
        │                ┌─────────────┐          │
        │                │ Register    │          │
        │                │    File     │          │
        │                └──────┬──────┘          │
        │                       │                 │
        │                       ▼                 │
        │                  ┌─────────┐            │
        │                  │   ALU   │            │
        │                  └────┬────┘            │
        │                       │                 │
        │              ┌────────┴────────┐        │
        │              ▼                 ▼        │
        │        Data Memory         ALUOut/MDR   │
        │                                         │
        └─────────────────────────────────────────┘
```

## Key Features

- 32-bit MIPS processor
- Multicycle instruction execution
- FSM-based control unit
- Reusable ALU datapath
- Register file
- Instruction memory
- Data memory
- Program Counter (PC)
- Instruction Register (IR)
- ALU output register
- Memory Data Register (MDR)
- MUX-based datapath control
- SystemVerilog RTL implementation
- Simulation-based functional verification

## Instruction Execution

Instructions are divided into multiple clock cycles depending on their type.

Typical execution flow:

```text
Instruction Fetch
        ↓
Instruction Decode / Register Read
        ↓
Execute / Address Calculation
        ↓
Memory Access
        ↓
Write Back
```

Not every instruction requires every stage.

For example, an R-type instruction such as:

```asm
add $t0, $t1, $t2
```

performs register read, ALU execution, and register write-back without requiring a data-memory access.

A load instruction such as:

```asm
lw $t0, 4($t1)
```

requires address calculation, memory read, and register write-back.

## Processor Components

### Datapath

The datapath contains the hardware required to process instructions:

- Program Counter (PC)
- Instruction Register (IR)
- Register File
- ALU
- ALUOut register
- Memory Data Register (MDR)
- Instruction Memory
- Data Memory
- Multiplexers
- Immediate/sign-extension logic

### Control Unit

The control unit is implemented as a finite state machine.

It:

- Decodes instructions
- Generates datapath control signals
- Selects ALU operations
- Controls register writes
- Controls memory reads/writes
- Controls PC updates
- Determines the next processor state

## Supported Instruction Set

The initial implementation targets a subset of the MIPS instruction set, including:

| Type | Instructions |
|------|--------------|
| R-Type | `add`, `sub`, `and`, `or`, `slt` |
| Memory | `lw`, `sw` |
| Branch | `beq` |
| Jump | `j` |

The instruction set may be extended as the project develops.

## Project Structure

```text
multicycle-mips/
│
├── rtl/
│   ├── alu.sv
│   ├── register_file.sv
│   ├── control_unit.sv
│   ├── datapath.sv
│   ├── instruction_memory.sv
│   ├── data_memory.sv
│   └── mips_cpu.sv
│
├── tb/
│   └── mips_cpu_tb.sv
│
├── programs/
│   └── ...
│
├── simulation/
│   └── ...
│
└── README.md
```

> The structure will be updated as additional modules and verification components are added.

## Verification

The processor is verified using a SystemVerilog testbench.

Verification includes:

- Reset behavior
- Instruction fetch
- Register operations
- ALU operations
- Load/store operations
- Branch instructions
- Jump instructions
- Register write-back
- Correct FSM state transitions

Simulation waveforms are used to inspect important signals such as:

```text
PC
IR
ALU inputs
ALU result
Register addresses
Register write data
Memory address
Memory data
Control signals
FSM state
```

## Future Enhancements

Planned modifications to the base architecture include:

- Additional MIPS instructions
- Interrupt handling
- Memory-mapped I/O
- Instruction/cycle counter
- Extended verification
- RTL synthesis and resource analysis
- Timing analysis

## Tools & Technologies

- **SystemVerilog**
- **RTL Design**
- **MIPS ISA**
- **Digital Logic**
- **Finite State Machines**
- **Computer Architecture**
- **Simulation & Verification**
- **Xilinx Vivado**

## Learning Objectives

This project is intended to provide practical experience with:

- Processor datapath design
- Control-unit design
- MIPS instruction execution
- Multicycle CPU architecture
- FSM-based control
- RTL hardware description
- SystemVerilog
- Functional verification
- FPGA-oriented design flow

## Status

🚧 **In Development**

The processor is being implemented incrementally, starting with the core multicycle datapath and control unit, followed by instruction support, verification, and architectural enhancements.