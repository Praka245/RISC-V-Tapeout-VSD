# DAY 3

## Combinational and sequential optmizations

## 📚 Contents

- [Combinational Logic Optimization](#Combinational-Logic-Optimization)
- [Sequential Logic Optimization](#Sequential-Logic-Optimization)
- [Combinational Logic Optimization-labs](#Combinational-Logic-Optimization-examples)
- [Sequential-Logic-Optimization-labs](#Sequential-Logic-Optimization-examples)

### Combinational Logic Optimization

#### Objective:
- Squeeze and simplify logic to get the most optimized design in terms of **area** and **power**.

#### Techniques:

- **Constant Propagation**
 
   - Replaces logic blocks with constants when input values are fixed.
  
  - **Example:**
    ```
    Y = ((A·B) + C)' 
    If A = 0 → Y = (0 + C)' = C'
    ```
  - **Result:** Complex gate logic (6 MOS transistors) simplifies to a single inverter (2 MOS transistors).

- **Boolean Logic Optimization**
- <br>
  - Simplify Boolean expressions using:
  
    - Karnaugh Map (K-Map)
      
    - Quine McCluskey Method
      <br>
  - **Example Verilog:**
    ```verilog
    assign y = a ? (b ? c : (c ? a : 0)) : (!c);
    ```
    - Optimized expression:  
      ```
      y = a ⊕ c
      ```
      
**Note:**  
<br>
  - The command to perform logic optimization in Yosys is `opt_clean`.
    
  - Additionally, for a hierarchical design involving multiple sub-modules, the design must be flattened by running the `flatten` command before executing the `opt_clean` command.


---

### Sequential Logic Optimization

#### Basic:

- **Sequential Constant Propagation**
  
  - Propagates known constant values through flip-flops during synthesis.

#### Advanced :
- **State Optimization**
  
  - Reduces the number of states in FSMs to optimize area and transitions.
- **Retiming**
  
  - Moves registers across logic boundaries to balance delay and improve timing.
- **Sequential Logic Cloning**
  
  - Duplicates logic in floorplan-aware synthesis to meet timing and congestion goals.


---

## Advanced Topics 

### Retiming

Retiming involves moving flip-flops across combinational logic to optimize timing without changing circuit behavior.

- Move flip-flops across combinational logic for better timing.

- Reduce the critical path without changing functionality.

- Balance path delays across the circuit.

- Improve maximum clock frequency.

- Minimize the number of sequential stages for efficiency.
  

### Cloning

Cloning is duplicating cells or modules in critical paths to reduce load and improve speed.

- Duplicate cells or modules in critical paths.

- Reduce fanout to improve timing.

- Distribute load evenly across replicated logic.

- Avoid overloading individual gates or flip-flops.

- Improve performance without changing functionality.

### State Reduction Techniques

State reduction simplifies finite state machines (FSMs) by minimizing states, saving hardware and improving efficiency.

- Minimize the number of states in FSMs.

- Merge equivalent states to simplify logic.

- Reduce combinational logic between states.

- Lower hardware cost by reducing flip-flops.

- Simplify sequential design for easier verification.
  
---

## Combinational Logic Optimization examples

<br>

### **Commands for Optimal Synthesis**:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v 
synth -top opt_check
opt_clean -purge # Removes unused or redundant logic #
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
### EXAMPLE 1 : 
<br>

<img width="1920" height="996" alt="2" src="https://github.com/user-attachments/assets/c137cb6f-5873-4836-884d-de93f86715aa" />

<br>

<img width="1920" height="996" alt="3" src="https://github.com/user-attachments/assets/89d97c1e-d8d6-4830-868c-e64820c11acd" />

<br>

### EXAMPLE 2 :

<br>

<img width="1920" height="996" alt="4" src="https://github.com/user-attachments/assets/24442726-0957-406a-a131-47a93317993a" />
<br>

<img width="1920" height="996" alt="5" src="https://github.com/user-attachments/assets/0d94a29c-b2a1-4268-9c1e-f6ca72abfb10" />



## 

![Alt Text](Images/7.png)
![Alt Text](Images/8.png)

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v 
synth -top opt_check3
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```


###  COMMAND 1 :
<br>

- Flatten Design Hierarchy
  
🔸 Flattening combines all submodules into a single level.

🔸 Essential before optimizing multi-module RTL designs for better tool efficiency.


```bash
# --------Phase 1: Flatten the hierarchical RTL design---------------

# Invoke yosys
yosys

# Load standard cell library (Liberty format)
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Read hierarchical RTL design
read_verilog multiple_module_opt.v

# Synthesize top module
synth -top multiple_module_opt

# Map to standard cells using ABC
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Flatten design hierarchy 
flatten

# Write out the flattened netlist
write_verilog -noattr multiple_module_opt_flat.v
```
### COMMAND 2 : 

- opt_clean -purge
🔸 Removes unused wires, cells, and modules from your design.
🔸 Helps reduce netlist size and cleans up after optimizations.

```bash
# ----------Phase 2: Optimize the flattened netlist-------------

# Invoke yosys
yosys

# Load standard cell library (Liberty format)
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Read the flattened netlist for further optimization
read_verilog multiple_module_opt_flat.v

# Synthesize top module
synth -top multiple_module_opt

# Remove unused logic and clean netlist
opt_clean -purge   # Cleans up redundant gates and wires after flattening

# Map to standard cells using ABC
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Visualize optimized gate-level netlist
show
```

---

## Sequential Logic Optimization 
<br>

Here, we can see value of q does not change as soon as reset=0,but q takes the value of 1’b0 at next clock edge,hence here no sequential logic optimization will occur and DFF will be inferred.

### COMMANDS : 

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

<img width="1920" height="996" alt="6" src="https://github.com/user-attachments/assets/f72bb097-4d95-4667-af23-44b6d3ea150e" />


