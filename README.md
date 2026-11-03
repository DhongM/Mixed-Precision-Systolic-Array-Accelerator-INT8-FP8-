Mixed-Precision Systolic Array Accelerator (INT8 / FP8)

A full-stack tensor accelerator for transformer and CNN inference, taken from SystemVerilog RTL through RTL-to-GDSII on the ASAP7 7nm FinFET PDK, with bit-exact verification against a cycle-accurate Python golden model.

Metric	Result
Array	16×16 (256 PEs), weight-stationary
Precision	Dense INT8 and FP8 (E4M3), switchable at runtime
Technology	ASAP7 7nm FinFET (predictive PDK)
Logic cells	450,206
Area	0.129 mm²
Frequency	666 MHz
Dynamic power	2.84 W budget
Verification	100% bit-for-bit RTL parity vs. Python golden model over a continuous 256-token inference workload
Architecture
Polymorphic systolic array
16×16 weight-stationary array of processing elements (PEs).
Each PE uses a shared datapath that switches between dense INT8 and FP8 (E4M3) streams at runtime, instead of instantiating separate INT8 and FP8 execution blocks.
Removing the redundant execution blocks saves about 40% logic area compared with a dual-datapath PE.
Decoupled memory dataflow
Double-buffered SRAM subsystem: one buffer feeds the array while the other is filled, hiding DRAM latency.
Nearest-neighbor "shift-and-lock" dataflow: operands move PE-to-PE instead of being broadcast, which removes a 2,048-wire broadcast routing network and the wiring congestion that comes with it.
SIMD vector post-processor
16-lane SIMD unit for post-GEMM operations.
Performs dequantization natively with integer Dynamic Radix arithmetic, so no hardware floating-point units are needed in the post-processing path.
<!-- TODO: add a block diagram (e.g. docs/architecture.png) -->
Verification
A cycle-accurate Python golden model mirrors the RTL datapath and dataflow.
RTL outputs are compared bit-for-bit against the golden model across a continuous 256-token inference workload, with 100% parity.
<!-- TODO: describe the testbench, simulator (e.g. Verilator), and how to run the comparison -->
Physical implementation
Synthesis and place-and-route with Yosys and OpenROAD on the ASAP7 7nm FinFET PDK.
Results: 450,206 logic cells, 0.129 mm², 666 MHz, within a 2.84 W dynamic power budget.
<!-- TODO: add the final layout screenshot and timing/area report excerpts -->
Repository layout
<!-- TODO: replace with the actual directory names -->
rtl/        SystemVerilog source (PE, array, buffers, SIMD post-processor)
model/      Cycle-accurate Python golden model
tb/         Testbenches and comparison scripts
flow/       Yosys / OpenROAD configuration and constraints
docs/       Diagrams and implementation reports
Getting started
<!-- TODO: fill in the real commands -->
bash
# Run RTL simulation and compare against the golden model
# make sim

# Run the OpenROAD flow on ASAP7
# make -C flow
Tools

SystemVerilog, Python, PyTorch, Verilator, Yosys, OpenROAD, ASAP7 7nm PDK

Author

Hongmei Dang, Ph.D.# Mixed-Precision-Systolic-Array-Accelerator-INT8-FP8-
