# Some Concepts in Hardware Design


## Concepts

**source:** [Quora](https://www.quora.com/What-is-compilation-elaboration-and-simulation-in-VLSI)



- A digital design consits of several HDL files that together form a so called toplevel, and modules.
- A design must be verified before translated into a target hardware.
- The toplevel design is called the **RTL code** (register-transfer level code) since it can be translated by a tool into a **netlist** (interconnection of basic cells, like flipflops and logic gates). Next to RTL, there is **behavioural code** (higher-level code that can’t be synthesized) as well.
- A testbench providing stimulus to the toplevel (**DUT**, design under test) and verifies that the outputs are as expected.

### Compilation

- Every language has a **LRM** (language reference manual) that defines the syntax that is valid.
- Both RTL and behavioural code need to be syntactically correct before elaboration.
- The compiler is in fact a parser that **check every HDL files for syntax errors**.

### Elaboration

- A design is a bunch of files that connect to each other in a certain way.
- Elaboration **checks if your design and testbench is connected up in the right way.**

### Simulation

- A synchronous design is always a combination of combinational logic (boolean algebra) and flipflops.
- A simulator executes the testbench code that applies stimulus to the RTL design. It can show a waveform.

### Tools

We need **a compiler, an elaboration tool, a simulator and a waveform viewer** for EDA (electronic design automation).
