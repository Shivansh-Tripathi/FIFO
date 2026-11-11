# High-Performance Synchronous and Asynchronous FIFO Architecture & Verification

A robust, production-grade implementation of **Synchronous FIFOs** and **Dual-Clock Asynchronous FIFOs** designed in **Verilog HDL**, complete with self-checking testbenches, constrained-random verification, and clean clock-domain crossing (CDC) structures.

---

## 🌟 Key Features

1. **Parameterizable Architecture**: Highly scalable modules supporting customizable `DATA_WIDTH` and `FIFO_DEPTH` to fit arbitrary data paths and buffer requirements.
2. **Safe Clock Domain Crossing (CDC)**:
   * **Gray-Coded Pointers**: Prevents multi-bit transition instability by ensuring only a single bit toggles between adjacent pointer states.
   * **2-Flop Synchronizers**: Multi-stage register synchronizers designed to eliminate metastability when crossing independent clock domains.
3. **Advanced Verification Methodology**:
   * **Self-Checking Testbenches**: Automated reference models (scoreboards) validating output data against expected queues transaction-by-transaction.
   * **Constrained-Random Stimulus**: Extensive stress-testing over thousands of randomized read and write bursts.
   * **Corner-Case Coverage**: Exhaustively verified handling for **Overflow**, **Underflow**, **Full**, and **Empty** boundaries.

---

## 📂 Repository Structure

```text
├── sim_1/
│   └── new/
│       ├── async_fifo_tb.v    # Multi-clock self-checking testbench for async FIFO
│       └── sync_fifo_tb.v     # Self-checking testbench for synchronous FIFO
├── sources_1/
│   └── new/
│       ├── async_fifo.v       # Dual-clock asynchronous FIFO top-level module
│       ├── sync_fifo.v        # Single-clock synchronous FIFO controller & memory
│       ├── fifo_mem.v         # Dual-port RAM / memory array inference
│       ├── rptr_handler.v     # Read pointer and empty flag generation logic
│       ├── wptr_handler.v     # Write pointer and full flag generation logic
│       └── synchronizer.v     # Multi-stage register synchronizer for CDC
├── assets/                    # Waveform simulation screenshots and diagrams
└── README.md                  # Project documentation
```

---

## 🛠️ Microarchitecture & Design Details

### 1. Synchronous FIFO
* **Clock Domain**: Operates on a single unified clock for both write (`wr_clk`) and read (`rd_clk`) operations.
* **Pointers**: Utilizes binary pointers with wrap-around bit tracking for full and empty generation.

### 2. Asynchronous FIFO (Dual-Clock)
* **Clock Domains**: Completely asynchronous write (`wr_clk`) and read (`rd_clk`) domains with zero phase correlation.
* **Pointer Synchronization**: 
  * Write pointers are translated to **Gray code**, synchronized into the read domain via 2-flop synchronizers to compute the `empty` condition.
  * Read pointers are translated to **Gray code**, synchronized into the write domain via 2-flop synchronizers to compute the `full` condition.

---

## 🔬 Simulation & Verification Metrics

* **Synchronous Validation**: Stress-tested across **4,000+ continuous random transactions** with zero data corruption.
* **Asynchronous Validation**: Verified over **3,000 words** transferred across highly skewed clock ratios (e.g., **100 MHz write domain** vs. **37 MHz read domain**), proving robust multi-clock reliability.
* **Corner Cases Covered**:
  * Writing to a full FIFO asserts `full` and blocks memory overwrites (Overflow protection).
  * Reading from an empty FIFO asserts `empty` and prevents invalid data readouts (Underflow protection).

---

## 🚀 Getting Started & Simulation

You can simulate these designs using **Icarus Verilog** (`iverilog`) and view waveforms using **GTKWave**:

1. Compile the source files and testbenches:
   ```bash
   iverilog -o async_sim sources_1/new/async_fifo.v sources_1/new/fifo_mem.v sources_1/new/rptr_handler.v sources_1/new/wptr_handler.v sources_1/new/synchronizer.v sim_1/new/async_fifo_tb.v
   ```
2. Run the simulation:
   ```bash
   ./async_sim
   ```
3. Open waveform outputs for inspection:
   ```bash
   gtkwave dump.vcd
   ```

---

## 👤 Author
**Shivansh Tripathi**  
*B.Tech ECE, National Institute of Technology, Patna (2027)*