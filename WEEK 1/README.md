# WEEK1 📘

## Introduction to Verilog RTL Design and Synthesis ⚡

### Contents
- [Introduction to RTL Design and Simulation](#introduction-to-rtl-design-and-simulation-🖥️)
- [Iverilog-Based Simulation Flow](#iverilog-based-simulation-flow-🔄)
- [Yosys Synthesis Flow](#yosys-synthesis-flow-for-good-mux-🛠️)

---

## Introduction to RTL Design and Simulation 🖥️

### Simulator 🧪
- RTL designs are validated via simulation to ensure correct behavior.
- **Simulator used**: Icarus Verilog (`iverilog`).

### Design ✏️
- Verilog code implementing intended hardware logic.
- Must meet functional specifications and may include multiple modules.

### Testbench 🧩
- Applies stimulus to the design for verification.
- Does **not** have primary inputs/outputs.

### How the Simulator Works ⚙️
- Continuously checks for changes in input signals.
- Evaluates outputs when inputs change.
- If inputs remain stable → outputs remain unchanged.
- Simulator reacts only to signal changes.

---

## Iverilog-Based Simulation Flow 🔄

1. Write RTL design and corresponding testbench.

2. Compile both using `iverilog` to generate a simulation executable.

3. Run the executable to generate a **Value Change Dump (VCD)** file.

4. Open the VCD file in **gtkwave** to analyze waveforms.

![IVERILOG FLOW](https://github.com/user-attachments/assets/d9fecdc4-0ecb-4e52-b8d6-1ed7e353e80e)

---

## Step-by-Step Process 📝
**1. Pass RTL design and testbench to Iverilog simulator**  
   <img width="1907" height="667" alt="image" src="https://github.com/user-attachments/assets/a14cd87e-51fe-40c9-906f-47bf73f6faee" />
  
**2. Generate VCD (Value Change Dump) file** 
   <img width="1907" height="667" alt="image" src="https://github.com/user-attachments/assets/eed1de67-266f-4c3b-b612-21925f4f5061" />
  
**3. Visualize variables using gtkwave**  
   <img width="1907" height="386" alt="image" src="https://github.com/user-attachments/assets/02bf9129-2804-4baf-bb6d-f60e5e0ab568" />
  
**4. View simulation waveform** 
   <img width="1920" height="997" alt="image" src="https://github.com/user-attachments/assets/90bd2368-4b4e-43e0-948c-19ddb05805c1" />


---

## MUX 2:1 Design and Testbench 🛠️

### Design (`good_mux.v`) 💡
- Implements a 2:1 multiplexer using behavioral Verilog.
- Inputs: `i0`, `i1`, `sel`; Output: `y`.
- Uses `always @(*)` block and `if-else` to select input.

<img width="1920" height="997" alt="image" src="https://github.com/user-attachments/assets/2e9d0b9d-5dd3-49a6-896b-ad91edbd91bc" />


### Testbench (`tb_good_mux.v`) 🧩
- Instantiates `good_mux`.
- Applies stimulus through internal registers `i0`, `i1`, `sel`.
- Generates VCD waveform using `$dumpfile` and `$dumpvars`.

<img width="1920" height="997" alt="image" src="https://github.com/user-attachments/assets/221796dc-d55e-41b4-a189-6ff1b19ae546" />

---

## Yosys Synthesis Flow for `good_mux` 🛠️

- RTL-to-gate-level synthesis using **Yosys** and **sky130_fd_sc_hd** library.

### Yosys Commands and Steps ⚡

**Step 0:** Start Yosys  
```bash
yosys
```

**Step 1:** Read Liberty File (for technology mapping) 📚

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

**Step 2:** Read RTL Verilog ✏️

```bash
read_verilog good_mux.v
```

**Step 3:** Synthesize top-level module 🏗️

```bash
synth -top good_mux
```

**Step 4:** Map RTL to standard cells 🧩

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

**Step 5:** View synthesized netlist as schematic 🔌

```bash
show
```

**Step 6:** Write synthesized gate-level netlist 💾

```bash
write_verilog -noattr good_mux_netlist.v
```

**Step 7:** Terminal Command

```bash

```
---

## Synthesized Netlist Images 🔌


This version now has: 

- Emojis next to **subtitles** for visual clarity.  
- Clean, GitHub-friendly Markdown structure.  
- Step-by-step highlights for simulation and synthesis.  

If you want, I can **also add a “Quick Notes & Tips 💡” section at the end** for extra clarity, which makes it perfect for a GitHub repo README style.  

Do you want me to add that?
