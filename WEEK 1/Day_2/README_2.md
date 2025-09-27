# ⚡ DAY2

## 🕒 Timing Libraries, Hierarchical vs Flat Synthesis, and Efficient Flop Coding Styles  

---

## 📚 Contents  
- [📖 Liberty Format](#-liberty-format)  
- [🔧 Standard Cell Example: `a2111o_1`](#-standard-cell-example-a2111o_1)  
- [🏗️ Hierarchical vs Flat Synthesis (Yosys)](#️-hierarchical-vs-flat-synthesis-yosys)  
- [⏱️ Why Do We Use Flops? (To Avoid Glitches)](#️-why-do-we-use-flops-to-avoid-glitches)  
- [🔄 DFF with Asynchronous Reset](#-d-flip-flop-with-asynchronous-reset)  
- [🔄 DFF with Asynchronous Set](#-d-flip-flop-with-asynchronous-set)  
- [🔄 DFF with Synchronous Reset](#-d-flip-flop-with-synchronous-reset)  
- [🛠️ Synthesis Examples](#️-synthesis-examples)  
- [✨ Interesting Optimizations](#-interesting-optimizations)  

---

## 📖 Liberty Format  
Liberty (`.lib`) files describe standard cells, including timing, power, and functional details.  
They help synthesis tools understand how to map RTL code into real hardware.  
<br>
📸 **Example:**

<br> 

<img width="1920" height="996" alt="4" src="https://github.com/user-attachments/assets/44849150-e17f-459e-9f8f-e1853e521f29" />

<img width="1920" height="996" alt="3" src="https://github.com/user-attachments/assets/4b077e03-6479-4a8e-b773-e9a4e6d6421d" />
<br>

---

## 🔧 Standard Cell Example: `a2111o_1`  
The `a2111o_1` cell implements the logic **X = (A1 & A2) | B1 | C1 | D1**.  
It is a complex gate combining AND and OR to optimize area and speed.  
<br>
📸 **Cell:**  

<br>

<img width="1920" height="996" alt="5" src="https://github.com/user-attachments/assets/e9b8a2b4-6a08-4309-abec-7f53e4618148" />


---

## ⚖️ Standard Cell Comparison: `and2_1`, `and2_2`, `and2_4`  
These are 2-input AND gates with different **drive strengths**.  
Higher drive cells switch faster but use more area and power.  
<br>
📸 **Cells:** 

<br>

<img width="1920" height="996" alt="6" src="https://github.com/user-attachments/assets/a9a2fc2b-50a0-428e-ad83-be3bc9c69781" />
<img width="1920" height="996" alt="7" src="https://github.com/user-attachments/assets/a9b16877-dc53-402a-aa20-a958a44d32e7" />
<img width="1920" height="996" alt="8" src="https://github.com/user-attachments/assets/730e4500-e23a-4b84-b3e4-1407db150f87" />


---

## 🏗️ Hierarchical vs Flat Synthesis (Yosys)  
Hierarchical synthesis keeps submodules intact, making large designs easier to manage.  
Flat synthesis merges everything into one level, giving faster optimization for small designs.  
<br>
📸 **Design:**

<br> 

<img width="1920" height="996" alt="9" src="https://github.com/user-attachments/assets/b6da0b9b-bed9-4856-8f07-0d849d4133ca" />


---

- Hierarchical Flow 

```blash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog -noattr multiple_modules.v
```

<img width="1920" height="996" alt="11" src="https://github.com/user-attachments/assets/cc814472-b3ef-4455-bebe-9aca8c473fab" />

---

- Flatten Flow
```blash
flatten
write_verilog -noattr multiple_modules_flat.v
```

<img width="1920" height="996" alt="12" src="https://github.com/user-attachments/assets/304b0a9a-65a9-4ea0-90a0-1cecb47e0301" />

---

## ⏱️ Why Do We Use Flops? (To Avoid Glitches) 


1. Combinational circuits can produce glitches — short, unwanted changes in output — when inputs change at slightly different times.

2. Glitches occur because the logic takes time to settle when multiple signals arrive with different delays.

3. These glitches can cause problems if other parts of the circuit capture them.

4. To fix this, we insert flip-flops between combinational blocks. Flip-flops capture data only at a clock edge, ensuring only stable, correct values are passed forward.

📸 **Example:**

<p align="center">
  <img src="https://github.com/user-attachments/assets/3f355ae1-1c91-4bc4-8868-ee7d1b77ab5f" alt="image" width="300" height="500">
</p>


---

## 🔄 D Flip-Flop with Asynchronous Reset  
The async reset forces output to 0 immediately, regardless of clock.  
This ensures the circuit can be quickly reset to a known state.  

<br>
📸 **Design:**

<br>

<img width="1920" height="996" alt="13" src="https://github.com/user-attachments/assets/4f710bc8-cff3-4c7c-8ecc-35c9cd9880c9" />
<br>

📸 **WaveForm:** 

<br>

<img width="1920" height="996" alt="14" src="https://github.com/user-attachments/assets/d04a64c8-d73a-4f6c-bc8c-82db48a9e848" />
 
---

## 🔄 D Flip-Flop with Asynchronous Set  
The async set forces output to 1 immediately, ignoring the clock.  
It is useful when a signal must be initialized quickly to logic 1.  

📸 Design:  
<img width="1920" height="996" alt="15" src="https://github.com/user-attachments/assets/b0ad499c-7a1f-41e5-a718-db8a5fc34c21" />
  
📸 WaveForm:  
<img width="1920" height="996" alt="16" src="https://github.com/user-attachments/assets/e03adc82-4857-4487-ab3c-7d3823323ca7" />


---

## 🔄 D Flip-Flop with Synchronous Reset  
Here, the reset is checked only during a clock edge.  
It provides more controlled reset behavior, aligned with clock timing.  
<br>
📸 **Design:** 

<br>

<img width="1920" height="996" alt="17" src="https://github.com/user-attachments/assets/ca3c1bfd-a18c-4c7f-b211-d9b968d0ef84" />

📸 **WaveForm:**

<br>

<img width="1920" height="996" alt="18" src="https://github.com/user-attachments/assets/bec8cbad-ea66-40d3-915a-2426249c39f7" />
  

---
 

## 🛠️ Synthesis Examples  
Yosys synthesis maps Verilog RTL into standard cells using `.lib` files.  
The flow involves reading libraries, synthesizing, and mapping to technology cells.  

Commands To Synthesis the Sequential Circuit : 

```blash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
### NOTE: 

- **abc -liberty <.lib file path>** : This command is used for for technology mapping of yosys’s internal gate library to a target architecture.

- **synth -top <module_name>** : This command runs the yosys synthesis script on the mentioned module name of our design

- **dfflibmap -liberty <.lib file path>** : This command maps internal flipflop cells to the flipflop cells in the technology library specified in the given liberty file.

---
<img width="1920" height="996" alt="19" src="https://github.com/user-attachments/assets/4ef2ae35-16e0-43f9-944f-b245b858889c" />
<br>

<img width="1920" height="996" alt="20" src="https://github.com/user-attachments/assets/f35f0065-576c-43cb-9074-a433069f2e7a" />
<br>

<img width="1920" height="996" alt="21" src="https://github.com/user-attachments/assets/9e94ff36-1767-4c52-afe9-5f6b3df907b6" />


## ✨ Interesting Optimizations  
- Yosys reduces gate count by removing redundant logic.  
- For multipliers of 2 and 8, it replaces them with simple bit shifts.  

---

## Example : `mul2`  
Multiplication by 2 is simplified into a **1-bit left shift**.  
This makes the design very small and efficient.  
<br>
📸 **RTL:** 

<br>

<img width="1920" height="996" alt="22" src="https://github.com/user-attachments/assets/0d68f2ac-4458-427b-82a5-a1c65312219e" />
<br>
📸 **Synthesis:**

<br>

<img width="1920" height="996" alt="24" src="https://github.com/user-attachments/assets/6906093c-7aaa-438a-899c-d2bb0feb8165" />

<br>

<img width="1920" height="996" alt="23" src="https://github.com/user-attachments/assets/f2d68952-72be-4062-a7e5-ba9fccf878a9" />

<br>

📸 **Output:**  

<br>

<img width="1920" height="996" alt="25" src="https://github.com/user-attachments/assets/07a9565f-7a2d-4ab6-ba80-ea0fb44e77a7" />

---

```blash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib # no need to do this step becoz no cells are used #
show
write_verilog -noattr mult2_net.v
```

---

✅ By doing these experiments, you learn how `.lib` files work, why synthesis styles matter,  
and how optimizations make your design smaller, faster, and more power-efficient.  
