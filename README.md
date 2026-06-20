\# STM32 Fault Tolerance: Checkpoint/Restart



A bare-metal implementation of a Checkpoint/Restart (C/R) fault tolerance mechanism on an STM32 Nucleo-L152RE (ARM Cortex-M3). 



This project is designed to save the computational state of a complex workload into non-volatile memory (EEPROM), allowing the system to survive power outages, brown-outs, and arbitrary hardware resets without losing progress.



\# Official Documentation

The complete architectural analysis, hardware profiling, and performance benchmarks are available in the official project report (Italian):

&#x20;\*\[Read the Official Report (PDF)](docs/Relazione\_PII\_Aceti\_Luca.pdf)\*



\## Key Features

\* \*Bare-Metal Environment:\* No RTOS used. Entirely driven by hardware interrupts and raw memory manipulation.

\* \*Asynchronous Checkpointing:\* Triggered via software using the ARM PendSV exception.

\* \*Naked Assembly Restore:\* Direct manipulation of the Main Stack Pointer (MSP) and the hardware Exception Frame (R0-R3, R12, LR,    PC, xPSR) to restore the CPU context.

* \*Dynamic Stack Dump:\* Calculates the exact stack footprint at runtime (\_estack - MSP) to avoid indiscriminate RAM dumping, minimizing EEPROM write cycles and reducing the vulnerability window.

\* \*Hardware Profiling:\* Dump execution time is strictly measured at the cycle-clock level using the Cortex-M Data Watchpoint and Trace (DWT) unit, bypassing software library overheads.



\# The Benchmark (LU Decomposition)

The system is tested against a heavy numerical workload: an O(n^3) LU matrix decomposition with system resolution (15 x 15 matrix). Checkpoints are strategically placed after each matrix phase and before substitutions.



\# Future Developments

As highlighted in the documentation, the project paves the way for industrial-grade improvements:

1\. \*Incremental Checkpointing:\* Saving only the computed \*delta\* (single rows/columns) instead of a Full State Dump to further decrease EEPROM write times.

2\. \*Dual-Slot Architecture \& CRC:\* Implementing a Ping-Pong buffer and Checksum validation to prevent the loading of torn writes in case a blackout occurs exactly during the EEPROM dumping phase.





\*Author:\* Luca Aceti (acio404)  

\*Progetto di Ingegneria Informatica - Politecnico di Milano (A.A. 2025/2026)\*

