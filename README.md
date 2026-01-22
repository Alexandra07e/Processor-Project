# 🧠 16-bit General Purpose Processor (GPP) + ASIP Extension

This project implements and simulates a **16-bit General Purpose Processor** in **structural Verilog**, inspired by the classical **IAS-style accumulator-based architecture**. The design includes a full **CPU + Memory + I/O** System-on-Chip integration and an **ASIP extension** for tensor-oriented operations.

---

## ✨ Core Features

- 🧾 **Fixed-length 16-bit instructions** (6-bit opcode)
- 🧮 **Accumulator-based datapath** with compact register set (**AC, X, Y**)
- 🧷 **FLAGS register** with **N, Z, C, V** for conditional branching
- 🧠 **Unified memory** (instructions + data in same address space)
- 🔁 **Handshake-based I/O** (safe input/output synchronization)
- 🧩 **ASIP extension** for tensor operations (memory-resident data)

---

## 🧱 Architecture Overview

### 🧩 Main Modules (SoC)
- 🧠 **CPU Core**: fetch → decode → execute (central control)
- 🗃️ **Memory Unit**: 512 × 16-bit word-addressable memory, initialized to zero
- 🔌 **Input Unit** / **Output Unit**: handshake-based communication

### 🧾 Register Set
- 📌 **PC** (Program Counter)
- 📌 **SP** (Stack Pointer, stack grows downward)
- 📌 **IR** (Instruction Register)
- 📌 **AR** (Address Register)
- 📌 **AC** (Accumulator)
- 📌 **X, Y** (general-purpose registers)
- 🚩 **FLAGS**: Negative (N), Zero (Z), Carry (C), Overflow (V)

---

## 🔌 I/O Handshake Protocol

- **Input**
  - CPU asserts `inp_req`
  - Input unit replies with `inp_ack` + valid `inp_data`
  - Transfer happens only when both are asserted

- **Output**
  - CPU asserts `out_req` + drives `out_data`
  - Output unit replies with `out_ack` after receiving data
  - Guarantees output is not lost

---

## 🧮 ALU Subsystem

- 🧠 **ALU has its own dedicated Control Unit**
- 🔁 Supports **multi-cycle operations** coordinated by an **FSM**
- 📦 Internal registers include **A**, **Q**, **M**, plus a **counter** for iterative steps
- ✅ Updates FLAGS after operations to support conditional execution

---

## 🧠 CPU Control Unit

- 🎛️ Central **one-hot FSM** driving the whole datapath
- 🧷 Generates control for:
  - register enables + mux selects
  - memory read/write
  - ALU start/ack synchronization
  - I/O request/ack synchronization
  - branching based on FLAGS
- 🧠 Large-state design (instruction sequencing and coordination)

---

## ⚡ ASIP Extension (Tensor Instructions)

The ASIP adds high-level, domain-specific instructions that operate **directly on memory-resident tensors**, reducing loop/control overhead compared to scalar code:

- ➕ **ADDM** — tensor addition  
- ➖ **SUBM** — tensor subtraction  
- ✖️ **MULM** — tensor/matrix multiplication (most complex)  
- 🧮 **ELMULM** — element-wise multiplication  

Each ASIP instruction expands internally into a **multi-state micro-operation sequence**, hidden from the programmer.

---

## 🛠 Technologies Used

- **Verilog HDL** — modular + structural coding style
- **GTKWave** — waveform inspection and debugging
- **Testbenches** — module-level + instruction-level verification

---

## 🧪 Testing & Validation

- ✅ Dedicated testbenches for major modules (registers, ALU, control, memory, I/O, ASIP)
- ✅ Instruction-level programs for:
  - arithmetic + memory access
  - branching and FLAGS validation
  - handshake I/O sequencing
  - ASIP tensor operations (including matrix multiplication)
- 🔍 Verified via **waveform analysis** (FSM transitions, timing, RD/WR cycles, ack signals)

---

## 📁 Suggested Repository Structure

- `src/` 🧩 Verilog modules (CPU, ALU, Memory, I/O, ASIP)
- `tb/` 🧪 testbenches
- `programs/` 🧾 test programs / instruction encodings
- `docs/` 📄 processor documentation, diagrams, flowcharts

---

## 🚀 How to Run (Simulation)

1. 📦 Open the project in your preferred Verilog simulator
2. 🧪 Run testbenches from `tb/`
3. 🔍 Inspect signals in **GTKWave**
4. ✅ Confirm correct execution using `start` → `finish` and handshake/FLAGS behavior

---

## 📌 Notes

This design emphasizes **clarity, modularity, and structural hardware logic**, showcasing:
- full instruction execution flow,
- synchronized memory + I/O transactions,
- and real integration of a domain-specific ASIP accelerator into a general-purpose CPU.

---
