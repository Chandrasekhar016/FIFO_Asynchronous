# FIFO_Asynchronous

## Overview

An **Asynchronous FIFO (First-In, First-Out)** is a memory buffer used to safely transfer data between two circuits that operate on **different clock signals**. Unlike a synchronous FIFO, which requires both the read and write operations to use the **same clock**, an asynchronous FIFO uses **separate write and read clocks**, making it suitable for clock domain crossing (CDC). It is required because directly transferring data between different clock domains can lead to **metastability, data corruption, or data loss**. By using synchronization techniques and separate clock handling, an asynchronous FIFO ensures reliable communication between modules running at different frequencies or phases, enabling safe and efficient data transfer in digital systems.

## Key Highlights

- Independent read and write clocks.
- Safe Clock Domain Crossing (CDC).
- Prevents data loss and metastability.
- Supports different clock speeds.
- Uses Gray code for synchronization.

## Objectives

- Design a FIFO memory, Flip Flop Synchronizer, Write Pointer Handler and Read Pointer Handler using Verilog HDL
- Generate Full and Empty status flags to prevent overflow and underflow
- Verify the Asynchronous FIFO functionality using a Verilog testbench
- Analyze waveforms and simulation results in Vivado

  ## Tools Used

- Vivado 2020.1
- Verilog HDL
- SystemVerilog



  ## Verification Flow

1. RTL Desig
2. Testbench Development
3. Apply Test Stimulus
4. Simulation
5. Functional Verification

## Key Learning Outcomes

- FIFO Architecture
- Verilog RTL Design
- SystemVerilog Testbench Development
- Functional Verification
- Simulation and Debugging
- Waveform Analysis

  
## Author

**Chandrasekhar Kanike**

B.Tech Electronics & Communication Engineering (ECE)
