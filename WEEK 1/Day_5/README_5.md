# 🚀 Day 5  
## Procedural & Conditional Statements in Verilog (for, if-else, case, generate) & Latch Inference  

## 📚 Contents  

- [Overview](#overview)  
- [If-Else with Multiplexer](#if-else-with-multiplexer)  
- [Case Statement with Demultiplexer](#case-statement-with-demultiplexer)  
- [Full Adder using Case & If](#full-adder-using-case--if)  
- [For Loop & Generate Blocks](#for-loop--generate-blocks)  
- [Latch Inference in Combinational Circuits](#latch-inference-in-combinational-circuits)  

---

## 📌 Overview  

🔹 Verilog provides control-flow constructs (`if-else`, `case`, `for`, `generate`) to design complex logic efficiently.  
🔹 `if-else` and `case` are widely used for **decision making** in combinational circuits.  
🔹 `for` loops and `generate` blocks help replicate structures like adders or multiplexers systematically.  
🔹 **⚠️ Caution**: Incomplete assignments in `if-else` or `case` blocks may infer unintended **latches**, which can cause timing issues.  
🔹 These constructs are synthesizable (with proper usage) and map to real hardware gates.  

---

## 🧩 If-Else with Multiplexer  

✅ A **MUX** selects one of many inputs based on the select line.  
✅ Can be written using **if-else statements** for clarity.  
✅ Ensure that *all branches* of an **if-else** provide assignments, otherwise synthesis infers **latch behavior**.  

---

## 🎛 Case Statement with Demultiplexer  

✅ A **DEMUX** routes the input to one of many outputs depending on the select line.  
✅ Implemented efficiently using `case` statements.  
✅ If not all cases are covered (missing `default:`), a **latch may be inferred**.  

---

## ➕ Full Adder using Case & If  

✅ Demonstrates designing combinational logic using procedural constructs.  
✅ Highlights differences in readability between `if-else` vs. `case`.  
✅ Proper completion of all possible input combinations avoids **latch inference**.  

---

## 🔁 For Loop & Generate Blocks  

✅ `for` loops iterate over procedural assignments, useful in testbenches and small logic replications.  
✅ `generate` blocks replicate hardware systematically (e.g., instantiating N full adders for a ripple-carry adder).  
✅ They do not cause latches if assignments are complete.  

---

## ⚡ Latch Inference in Combinational Circuits  

🔹 A **latch is inferred** in RTL when the synthesizer needs to *remember a value* due to incomplete assignments.  
🔹 Common causes:  
- Missing `else` in `if-else`  
- Missing `default` in `case`  
- Partial assignments to signals inside procedural blocks  

### 🔹  Example of latch inference:  

```
always @(*) begin
  if (sel)
    y = a;   // no else → latch inferred (y holds value when sel=0)
end
```

<img width="1920" height="997" alt="1" src="https://github.com/user-attachments/assets/7836e2e6-f043-4926-88c5-c413f23f3175" />

<br>

<img width="1920" height="997" alt="2" src="https://github.com/user-attachments/assets/9a1f9cbb-9ed7-4482-ac40-e1b6a2aa9ad5" />

<br>

 🔹 Corrected version (pure combinational):  

```
always @(*) begin
  if (sel)
    y = a;
  else
    y = b;   // assigns for all conditions → no latch
end
```

🔹 **Rule of thumb**: *Always assign output signals in every branch of `if-else` and `case` for combinational logic.*  

### 🎛 Incomplete Case Statements  

🔹 An incomplete `case` statement occurs when **not all possible input combinations are handled**.  

🔹 This can cause **unintended latches** because the output may need to "remember" its previous value.  

🔹 Always include a `default` branch or cover all input cases to ensure **combinational behavior**.  

🔹 Helps maintain **predictable RTL synthesis** and avoids simulation-synthesis mismatches.  

🔹 Essential for writing **clean and synthesis-friendly Verilog**.

 <br>
<img width="1920" height="997" alt="5" src="https://github.com/user-attachments/assets/bfc1bf77-440d-4814-bfb5-697b6d0a18b0" />
<br>

### ✅ Complete Case Statements  

🔹 A complete `case` statement handles **all possible input combinations**.  

🔹 Ensures **no unintended latches** are inferred in combinational logic.  

🔹 May include a `default` case to cover any unspecified inputs.  

🔹 Guarantees **predictable synthesis** and consistent RTL behavior.  

🔹 Recommended for writing **safe and synthesis-friendly Verilog**.

<br>
<img width="1920" height="997" alt="6" src="https://github.com/user-attachments/assets/8e814470-aea0-4b5f-ae5f-1c3c9da667ae" />
<br>
<img width="1920" height="997" alt="7" src="https://github.com/user-attachments/assets/49f66c4d-ba71-45ce-980e-f2c3149729ed" />
<br>

### ⚠️ Partial Case Statements  

🔹 A partial `case` statement **handles only some input combinations**, leaving others unspecified.  

🔹 Can lead to **unintended latches** as the output may retain its previous value.  

🔹 Missing `default` or incomplete coverage is the main cause.  

🔹 Should be used cautiously, mainly when **memory-like behavior** is intended.  

🔹 Can cause **simulation vs synthesis mismatches** if not handled properly.

<br>
<img width="1920" height="997" alt="8" src="https://github.com/user-attachments/assets/0bf3ea50-a9ad-4c23-bb6e-391ef991d4a0" />

<br>
<img width="1920" height="997" alt="9" src="https://github.com/user-attachments/assets/4e37556c-38da-48c1-8052-8d47cfc8955e" />

---

### 🔄 For Loops

- Iteratively executes code multiple times.

- Useful in testbenches or repeated logic.

- Synthesized as unrolled hardware.

- Loop bounds must be constant for synthesis.

```blash
Example:

integer i;
always @(*) begin
    for(i=0; i<4; i=i+1)
        c[i] = a[i] | b[i];
end
```
### ⚙️ Generate Blocks

- Creates multiple instances of modules or logic.

- Useful for parameterized designs (N-bit adders, DFF arrays).

- Expanded statically at compile time.

- Requires constant parameters for indexing.

Example:
```blash
genvar i;
generate
    for(i=0; i<4; i=i+1) begin : dff_array
        dff u_dff (
            .clk(clk),
            .d(d[i]),
            .q(q[i])
        );
    end
endgenerate
```

## 📝 Summary :  

- Verilog control-flow constructs (`if-else`, `case`, `for`, `generate`) are essential for **writing clean, modular, and reusable RTL**.  

- **If-Else** and **Case Statements** allow decision-making in combinational circuits, but incomplete cases can cause **unintended latches**.  

- **For Loops** simplify repetitive logic in both RTL and testbenches, unrolling into hardware during synthesis.  

- **Generate Blocks** help create **parameterized hardware structures**, like multi-bit adders or arrays of flip-flops.  

- Proper usage ensures **synthesizable designs**, avoids glitches or latches, and maintains **simulation vs synthesis consistency**.  

---
