# 🚀 Day 4  
## GLS, Blocking vs Non-Blocking & Synthesis-Simulation Mismatch  

## 📚 Contents  

- [📌 Overview](#-overview)  
- [🏗️ Gate Level Simulation (GLS)](#️-gate-level-simulation-gls)  
- [🔀 Synthesis of Bad MUX design](#-synthesis-of-bad-mux-design)  
- [⚡ GLS of Bad MUX design](#-gls-of-bad-mux-design)  
- [❗ Synthesis-Simulation Mismatch of Bad MUX design](#-synthesis-simulation-mismatch-of-bad-mux-design)  

---


## 📌 Overview  
<br>

🔹 RTL simulations are done before synthesis, while **Gate Level Simulations (GLS)** are done after synthesis.

🔹 GLS ensures that the **post-synthesis netlist behaves the same** as the RTL design.  

🔹 Issues such as **blocking vs non-blocking assignments** or **incomplete sensitivity lists** can cause **simulation-synthesis mismatches**.  

---

## 🏗️ Gate Level Simulation (GLS)  

- GLS runs **post-synthesis** using the **gate-level netlist**.
 
- Uses the same testbench as RTL simulation.  

- Ensures **logical equivalence** between RTL and netlist.
  
- Can run in:
    
  1️⃣ **Zero delay (functional)** mode
  
  2️⃣ **Full timing validation** mode (if timing info is available)

  <br>

  ![GLS](https://github.com/user-attachments/assets/49e76bbd-ca16-44aa-ad1e-263f0eb0f941)


<br>

📌 Command format for GLS using **iverilog**:  
```bash
iverilog <gate-level-models> <netlist_file.v> <testbench.v>
./a.out
gtkwave tb.vcd
```

### 🔀 Synthesis of bad_mux

- ✔️ A clean MUX design using the ternary operator.
  
- ✔️ Synthesized with Sky130 Liberty and tested with RTL + GLS.

### 📌 RTL Simulation:

<br>

<img width="1920" height="996" alt="1" src="https://github.com/user-attachments/assets/11ef4d7c-9635-4232-adaf-aa5c760823c1" />


```blash
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave bad_mux.vcd
```

<p align="center">
  <img width="1920" height="996" alt="2" src="https://github.com/user-attachments/assets/29cf7dfb-7882-4fc3-93e5-c1bbe5472639" />
  <br>
  <b>RTL Simulation without GLS Model</b>
</p>


### 📌 Yosys Synthesis Flow:

### Note : You must in the directory of *verilog_files*

<br>

```blash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr bad_mux_net.v
```

<br>

#### GLS of bad_mux

- 📌 Run GLS using synthesized netlist with these Commands 


```bash
iverilog ../my_lib/verilog_model/primitives.v \
        ../my_lib/verilog_model/sky130_fd_sc_hd.v \
        bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

<p align="center">
 <img width="1920" height="997" alt="3" src="https://github.com/user-attachments/assets/db6a510a-30f6-422a-9476-97ea411def34" />
<br>
  <b>GLS Simulation </b>
</p>
