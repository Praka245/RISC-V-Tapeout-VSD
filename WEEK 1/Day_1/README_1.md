# DAY1 📘

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
<img width="1907" height="742" alt="5" src="https://github.com/user-attachments/assets/0b1419eb-f784-476c-b6eb-c9379e2bd46d" />

---

**Step 1:** Read Liberty File (for technology mapping) 📚

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
---

**Step 2:** Read RTL Verilog ✏️

```bash
read_verilog good_mux.v
```
<img width="1920" height="997" alt="6" src="https://github.com/user-attachments/assets/86add2e9-a2ec-4eed-a895-ec3de9a79d6a" />

---

**Step 3:** Synthesize top-level module 🏗️

```bash
synth -top good_mux
```
<img width="1920" height="997" alt="7" src="https://github.com/user-attachments/assets/28f4b3e2-e9da-48e7-bb7d-d90113112a76" />
<img width="1920" height="997" alt="8" src="https://github.com/user-attachments/assets/11dba931-b7a5-410e-8201-949938a16d7c" />

---

**Step 4:** Map RTL to standard cells 🧩

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="1920" height="997" alt="9" src="https://github.com/user-attachments/assets/f3ec1d64-b451-4613-8df7-57fab00c3da8" />

<img width="1920" height="997" alt="10" src="https://github.com/user-attachments/assets/6ad6bb2c-d7c6-4d76-8de7-f456d1f14444" />

---

**Step 5:** View synthesized netlist as schematic 🔌

```bash
show
```
<img width="1920" height="997" alt="11" src="https://github.com/user-attachments/assets/a1386708-0b48-4cf2-9a8d-8a434455b06d" />

---

**Step 6:** Write synthesized gate-level netlist 💾

```bash
write_verilog -noattr good_mux_netlist.v
```
<img width="1920" height="997" alt="14" src="https://github.com/user-attachments/assets/c8a4fb26-6cc1-4e56-a2b6-018a74a6fcac" />

---

**Step 7:** Shell Command to view the Netlist file 

```bash
!gedit good_mux_netlist.v
```
## Synthesized Netlist

<img width="1920" height="997" alt="15" src="https://github.com/user-attachments/assets/6ec16e50-a508-469c-9c16-91e29628f555" />

---
