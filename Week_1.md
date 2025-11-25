 # 🚀 P_DINESH_WEEK_1_RISC_V_SoC_Tapeout_Program_VSD

This repository documents my Week 1 progress in the RISC-V SoC Tapeout Program organized by VLSI System Design (VSD).
The main goal of this week was to set up a complete digital design flow using open-source tools and gain hands-on experience in RTL design, simulation, verification, and synthesis.

🌟 **Week 1 Highlights**

✅ 1. **RTL Design**  
   - Implemented basic digital modules to understand hardware description at the **register-transfer level (RTL)**.  
   - Created **testbenches** to verify functionality by applying input vectors and monitoring outputs.


✅ 2. **Simulation with Icarus Verilog**  
   - Compiled Verilog design and testbench into a **simulation executable**.  
   - Verified design behavior under different test conditions.


✅ 3.  **Waveform Analysis with GTKWave**  
   - Visualized **signal transitions, timing diagrams, and logic behavior**.  
   - Debugged and confirmed correctness of the designorrectness.

     
✅ 4. **Logic Synthesis with Yosys**  
   - Transformed **Verilog RTL** into a **gate-level netlist** mapped to a standard cell library.  
   - Optimized design using `synth` and `abc` commands.  
   - Prepared synthesized netlist for further verification or tapeout.


1️⃣ **Icarus Verilog (iverilog) – Compile  Simulate Verilog**

Purpose: Compile Verilog code and testbenches to produce a simulation executable.

Steps:

Open a terminal in the folder containing your Verilog files.

```
bash

iverilog  FILE_NAME.v FILE_NAME_TB.v

./a.out

```

2️⃣ **GTKWAVE - Simulate Verilog**
Purpose of GTKWave :Waveform Visualization

Steps:

Compile the design with the testbench:
```
bash

gtkwave filename.vcd

```

3️⃣ **Yosys – Logic Synthesis**

### Purpose of Yosys

1. **RTL to Gate-Level Synthesis**  
   - Converts your **Verilog RTL code** into a **gate-level netlist**.  
   - Maps the design to standard cells from a library like SkyWater 130nm.

2. **Design Optimization and Verification**  
   - Uses commands like `synth` and `abc` to optimize logic.  
   - Produces synthesized Verilog and netlists for further flow steps.

3. **Integration with Open-Source EDA Flow**  
   - Works with **Icarus Verilog**, **GTKWave**, and other open-source tools.  
   - Essential for preparing designs for tapeout or FPGA implementation.

---

### Step-by-Step Commands

**Set Environment Variable for PDK Library**

```bash
# 1️⃣ Open Yosys shell
yosys

# 2️⃣ Read your Verilog design
read_verilog FILE_NAME.v

# 3️⃣ Read standard cell library using environment variable
read_liberty -lib $SKY130_LIB

# 4️⃣ Synthesize top module
synth -top FILE_NAME

# 5️⃣ Write synthesized Verilog netlist
write_verilog synthesized.v

# 6️⃣ Optimize logic using ABC with the same library
abc -liberty $SKY130_LIB

# 7️⃣ Exit Yosys
exit
```
# --------------------------------------------
# Project: good_mux.v – 2-to-1 Multiplexer
# Demonstrates RTL design, simulation, waveform viewing, and synthesis
# --------------------------------------------

# 1️⃣ Compile Verilog design and testbench using Icarus Verilog
```bash
iverilog  good_mux.v tb_good_mux.v
```
<img width="1811" height="110" alt="IVERILOG" src="https://github.com/user-attachments/assets/e1897852-3847-4df5-ba5f-ec04d31695eb" />

# 2️⃣ View waveform in GTKWave
```bash
gtkwave good_mux.vcd
```
<img width="1883" height="932" alt="IVERILOG_GTKWAVE" src="https://github.com/user-attachments/assets/b305c92b-6b4c-40fa-baee-1f759b4cd88f" />

# 3️⃣ Run Yosys for logic synthesis
```bash
yosys << EOF
# Read the Verilog design
read_verilog good_mux.v

# Load the standard cell library
read_liberty -lib \$SKY130_LIB

# Synthesize the top module
synth -top good_mux

# Write synthesized gate-level netlist
write_verilog good_mux_synth.v

# Optimize logic using ABC
abc -liberty \$SKY130_LIB

# Exit Yosys
exit
```
<img width="1902" height="936" alt="YOSYS" src="https://github.com/user-attachments/assets/a4bbe7e9-68a0-43bc-b74e-d36dc158126c" />

# --------------------------------------------
# End of good_mux.v workflow
# --------------------------------------------

🌟 **Summary and Achievements:**

   - By the end of Week 1, I successfully:
   
   - Set up a complete digital design flow using open-source tools.
   
   - Developed, simulated, and verified RTL modules.
   
   - Visualized signal behavior using GTKWave and debugged designs efficiently.
   
   - Generated a synthesized gate-level netlist using Yosys, ready for further verification.

## LAB: good_mux.v
<img width="1902" height="936" alt="YOSYS" src="https://github.com/user-attachments/assets/d6f1da52-7f1b-4cc3-a61d-1b46ef58ced1" />

## LAB: MULTIPLE_MODULES
<img width="1860" height="943" alt="MULTIPLE_MODULES" src="https://github.com/user-attachments/assets/86ada8dc-66d6-4bf4-a9d3-29fbd157339f" />

## LAB: INCOMP_IF
<img width="1918" height="957" alt="INCOMP_IF" src="https://github.com/user-attachments/assets/803cb894-3c95-45d3-ae73-0d7da89a4c2a" />

## LAB: INCOMP_IF2
<img width="1918" height="952" alt="INCOMP_IF2" src="https://github.com/user-attachments/assets/2a9cd29a-6586-4a95-8ae4-89118e960cfe" />

## LAB: OPT_CHECK2
<img width="1901" height="962" alt="OPT_CHECK2" src="https://github.com/user-attachments/assets/54b3064e-6538-4f70-93af-0c58752a1a6a" />

## LAB: MULTIPLE_MODULES
<img width="1860" height="943" alt="MULTIPLE_MODULES" src="https://github.com/user-attachments/assets/cad416d5-1423-4f67-8316-e270bf2439f2" />

## LAB: PARTIAL_CASE_ASSIGN
<img width="1920" height="950" alt="PARTIAL_CASE_ASSIGN" src="https://github.com/user-attachments/assets/5180033f-0047-4a35-89e8-b02117dc1908" />


## LAB: TERNARY_OPERATOR_MUX
<img width="1898" height="937" alt="TERNARY_OPERATOR_MUX" src="https://github.com/user-attachments/assets/f4de4b4e-5b1a-4b75-94df-909a7ed8e848" />

## LAB: DFF_SYNCRES
<img width="1902" height="966" alt="DFF_SYNCRES" src="https://github.com/user-attachments/assets/c9590b66-865a-49c3-a9e2-17bc359cefab" />

## LAB: DFF_CONST1
<img width="1900" height="965" alt="DFF_CONST1" src="https://github.com/user-attachments/assets/ebf3c7aa-a0d2-4110-b8e4-0ea7b22cb4d8" />

## LAB: DFF_CONST3
<img width="1902" height="957" alt="DFF_CONST3" src="https://github.com/user-attachments/assets/1a9975f0-5f13-4585-9c22-5b345404e60f" />

## LAB: DFF_ASYNCRES
<img width="1902" height="945" alt="DFF_ASYNCRES" src="https://github.com/user-attachments/assets/f2c9ceef-ed6e-463b-a8be-4d61afa63d5f" />

## LAB: BAD_MUX
<img width="1898" height="937" alt="BAD_MUX" src="https://github.com/user-attachments/assets/47764ada-d636-4e0c-a6c5-9074e4799ffd" />
