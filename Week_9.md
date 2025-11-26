# 📘 Final Project Documentation

This section contains the complete flow execution results, images, logs, timing reports, floorplan evolution, GDSII layout, and analysis performed across 10 weeks.

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

```
bash

iverilog  FILE_NAME.v FILE_NAME_TB.v

./a.out

```
2️⃣ **GTKWAVE - Simulate Verilog**
Purpose of GTKWave :Waveform Visualization

```
bash

gtkwave filename.vcd

```

3️⃣ **Yosys – Logic Synthesis**
```
bash
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
# 🤖 Introduction to VSDBabySoC

The VSDBabySoC (or BabySoC) is a small yet highly capable SoC with a primary objective of being an educational platform. It was designed to facilitate the simultaneous testing of three specific open-source intellectual property (IP) cores for the first time while also allowing for the calibration of its analog components.
<img width="1248" height="698" alt="496497103-e544b214-f91e-4afe-bfcb-0cb1155be1f8" src="https://github.com/user-attachments/assets/1b5d841a-d1bb-41db-902c-fac8f3562da3" />
<img width="885" height="535" alt="496545350-19c90319-aeec-4f5f-a7c6-9a903cd9c3e3" src="https://github.com/user-attachments/assets/60bcae40-7670-44b2-9e8e-23d2208a9bc5" />
<img width="600" height="556" alt="496545414-4b748df9-6923-4faf-bfd4-625a56c5cf8b" src="https://github.com/user-attachments/assets/7fa069c6-cccc-4351-a263-240c257b7a9c" />

# 🔗 Cloning the Project
To begin, clone the VSDBabySoC repository using the following command:
```
git clone https://github.com/manili/VSDBabySoC.git

cd ~/VLSI/VSDBabySoC/

ls VSDBabySoC/
images  LICENSE  Makefile  README.md  src

DINESH@UBUNTU:~/Desktop/my_project$ cd VSDBabySoC/
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC$ ls
copy_past.txt  images  LICENSE  Makefile  my_project_images  output  output_@  README.md  sp_env  src  vsdbabysoc.synth_1.v  vsdbabysoc.synth.v
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC$ cd src/module/
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC/src/module$ l
avsddac.v  clk_gate.v    pseudo_rand_gen.sv  rvmyth_gen.v  rvmyth.v           testbench.rvmyth.post-routing.v  vsdbabysoc.synth.v
avsdpll.v  primitives.v  pseudo_rand.sv      rvmyth.tlv    sky130_fd_sc_hd.v  testbench.v*                     vsdbabysoc.v
```

# TLV to Verilog Conversion for RVMYTH
Initially, you will see only the rvmyth.tlv file inside src/module/, since the RVMYTH core is written in TL-Verilog.
To convert it into a .v file for simulation, follow the steps below:
🔧 TLV to Verilog Conversion Steps

```
# Step 1: Install python3-venv (if not already installed)
sudo apt update
sudo apt install python3-venv python3-pip

# Step 2: Create and activate a virtual environment
cd VSDBabySoC/
python3 -m venv sp_env
source sp_env/bin/activate

# Step 3: Install SandPiper-SaaS inside the virtual environment
pip install pyyaml click sandpiper-saas

# Step 4: Convert rvmyth.tlv to Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

# VSDBabySoC RTL to Gate-Level Simulation Flow

# 1️⃣ Pre-Synthesis Simulation:
Run the following command to perform a pre-synthesis simulation:
```
iverilog -o output/pre_synth_sim/pre_synth_sim.out   -DPRE_SYNTH_SIM   -I src/include   -I src/module   src/module/testbench.v
```
<img width="1920" height="482" alt="PRE_SYNTH_SCREENSHOT" src="https://github.com/user-attachments/assets/49168a05-ab4d-4c3c-adfe-49c63eb38e8c" />

```
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC/output$ cd pre_synth_sim/
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC/output/pre_synth_sim$ ls
post_synth_sim.out  pre_synth_sim.out
```
<img width="1917" height="906" alt="PRE_SYNTH_SCREENSHOT_1" src="https://github.com/user-attachments/assets/476f7abe-fa56-47c0-8a1c-9f80fc2059c4" />

<img width="1902" height="878" alt="PRE_SYNTH_SCREENSHOT_2" src="https://github.com/user-attachments/assets/003d7024-f521-4cb8-81b4-5ac5592db595" />

- Output: output/pre_synth_sim/pre_synth_sim.vcd (waveform if $dumpfile is used in testbench).
- This is your reference RTL behavior.

# 2️⃣ RTL Synthesis using Yosys

```
DINESH@UBUNTU:~/Desktop/my_project$ cd VSDBabySoC/
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC$ yosys
```
```
yosys> read_verilog  -sv -I src/include/ -I src/module/ src/module/vsdbabysoc.v src/module/clk_gate.v src/module/rvmyth.v

yosys> read_liberty -lib /home/DINESH/Desktop/Open_Source_EDA_Tool/yosys/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -lib src/lib/avsddac.lib
 
read_liberty -lib src/lib/avsdpll.lib

read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

synth -top vsdbabysoc

write_verilog vsdbabysoc.synth.v

abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 
```
<img width="1917" height="830" alt="image" src="https://github.com/user-attachments/assets/64c29aa6-3b43-4851-b258-ca2044340033" />

# 3️⃣ Post-Synthesis Simulation:
```
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM  -I src/include/ -I src/module/ src/module/testbench.v
```
<img width="1893" height="461" alt="POST_SYNTH_SIM" src="https://github.com/user-attachments/assets/8f1531d5-04d1-4e6b-9bfd-22833d452493" />
```
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC/output$ cd post_synth_sim/
DINESH@UBUNTU:~/Desktop/my_project/VSDBabySoC/output/post_synth_sim$ ls
post_synth_sim.out  post_synth_sim.vcd
```
<img width="1910" height="903" alt="POST_SYNTH_SIM_2" src="https://github.com/user-attachments/assets/abfdae8c-c282-471f-bb89-62d01b9c8b80" />



# We perform Static Timing Analysis (STA) on the RISC-V VSD Babysoc using OpenSTA.
# Cloning the Project
```
BASH
# To begin, clone the VSDBabySoC repository using the following command:
git clone https://github.com/manili/VSDBabySoC.git
```
# TIMING LIBRARIES
```
BASH
The timing libraries can be downloaded from:
https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing
```
# Git cannot clone the subdirectory:
```
Use this instead :

Option 1 - only the timing directory

git clone --no-checkout https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd.git
cd skywater-pdk-libs-sky130_fd_sc_hd
git sparse-checkout init --cone
git sparse-checkout set timing
git checkout

Option 2 :

sudo apt install subversion

svn export https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/trunk/timing

```
# VSDBABYSOC SDC SCRIPT:

```
SDC
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period 11
```

# VSDBABYSOC TCL SCRIPT:
```
TCL
 set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
 set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
 set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
 set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
 set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
 set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
 set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
 set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
 set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
 set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
 set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
 set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
 set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

 read_liberty /home/DINESH/Downloads/OpenSTA/examples/skywater-pdk-libs-sky130_fd_sc_hd/timing/avsdpll.lib
 read_liberty /home/DINESH/Downloads/OpenSTA/examples/skywater-pdk-libs-sky130_fd_sc_hd/timing/avsddac.lib

 for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
 read_liberty /home/DINESH/Downloads/OpenSTA/examples/skywater-pdk-libs-sky130_fd_sc_hd/timing/$list_of_lib_files($i)
 read_verilog /home/DINESH/Downloads/OpenSTA/VSDBabySoC/src/vsdbabysoc.synth.v
 link_design vsdbabysoc
 current_design
 read_sdc /home/DINESH/Downloads/OpenSTA/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc
 #check_setup -verbose
 report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/min_max_$list_of_lib_files($i).txt

 exec echo "$list_of_lib_files($i)" >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_worst_max_slack.txt
 report_worst_slack -max -digits {4} >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_worst_max_slack.txt

 exec echo "$list_of_lib_files($i)" >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_worst_min_slack.txt
 report_worst_slack -min -digits {4} >>/home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_worst_min_slack.txt

 exec echo "$list_of_lib_files($i)" >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_tns.txt
 report_tns -digits {4} >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_tns.txt

 exec echo "$list_of_lib_files($i)" >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_wns.txt
 report_wns -digits {4} >> /home/DINESH/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT/sta_wns.txt
 }
```
#FILES GENERATED:
<img width="1917" height="472" alt="image" src="https://github.com/user-attachments/assets/51c2e739-1be4-4a93-aa10-b19800bccef2" />

```
DINESH@UBUNTU:~/Downloads/OpenSTA/VSDBabySoC/BABY_SOC/STA_OUTPUT$ tree
.
├── min_max_sky130_fd_sc_hd__ff_100C_1v65.lib.txt
├── min_max_sky130_fd_sc_hd__ff_100C_1v95.lib.txt
├── min_max_sky130_fd_sc_hd__ff_n40C_1v56.lib.txt
├── min_max_sky130_fd_sc_hd__ff_n40C_1v65.lib.txt
├── min_max_sky130_fd_sc_hd__ff_n40C_1v76.lib.txt
├── min_max_sky130_fd_sc_hd__ss_100C_1v40.lib.txt
├── min_max_sky130_fd_sc_hd__ss_100C_1v60.lib.txt
├── min_max_sky130_fd_sc_hd__ss_n40C_1v28.lib.txt
├── min_max_sky130_fd_sc_hd__ss_n40C_1v35.lib.txt
├── min_max_sky130_fd_sc_hd__ss_n40C_1v40.lib.txt
├── min_max_sky130_fd_sc_hd__ss_n40C_1v44.lib.txt
├── min_max_sky130_fd_sc_hd__ss_n40C_1v76.lib.txt
├── min_max_sky130_fd_sc_hd__tt_025C_1v80.lib.txt
├── sta_tns.txt
├── sta_wns.txt
├── sta_worst_max_slack.txt
└── sta_worst_min_slack.txt
```

# RISC-V VSD Babysoc STA Analysis with OpenSTA

<img width="880" height="337" alt="image" src="https://github.com/user-attachments/assets/c0f8cf51-d7d8-4b98-87d8-57c46894b551" />

# Perform STA analysis: Timing graph analysis.
```
import pandas as pd
import matplotlib.pyplot as plt
from io import StringIO
import re

# Data from your previous query
data = """PVT_CORNER	WORST SETUP SLACK	WORST HOLD SLACK	WNS	TNS
sky130_fd_sc_hd_tt_025C_1v80	0.8595	0.3096	0	0
sky130_fd_sc_hd_ff_100C_1v65	2.2764	0.2491	0	0
sky130_fd_sc_hd_ff_100C_1v95	3.7138	0.196	0	0
sky130_fd_sc_hd_ff_n40C_1v56	0.8214	0.2915	0	0
sky130_fd_sc_hd_ff_n40C_1v65	1.8597	0.2551	0	0
sky130_fd_sc_hd_ff_n40C_1v76	2.7707	0.2243	0	0
sky130_fd_sc_hd_ss_100C_1v40	-13.6381	0.9053	-13.6381	-7567.7964
sky130_fd_sc_hd_ss_100C_1v60	-6.7098	0.642	-6.7098	-2833.05
sky130_fd_sc_hd_ss_n40C_1v28	-51.2061	1.8296	-51.2061	-36861.4102
sky130_fd_sc_hd_ss_n40C_1v35	-32.0887	1.3475	-32.0887	-23317.6035
sky130_fd_sc_hd_ss_n40C_1v40	-23.829	1.1249	-23.829	-17211.252
sky130_fd_sc_hd_ss_n40C_1v44	-19.201	0.9909	-19.201	-13652.0469
sky130_fd_sc_hd_ss_n40C_1v76	-4.4548	0.5038	-4.4548	-1842.5518
"""

# 1. Load and prepare the data
df = pd.read_csv(StringIO(data), sep='\t')
df['PVT_CORNER'] = df['PVT_CORNER'].str.strip().str.replace('"', '')

# Simplify Corner Names for better readability on the X-axis
df['Corner_Label'] = df['PVT_CORNER'].str.replace('sky130_fd_sc_hd_', '')

# Define the metrics to plot, with their display titles
plot_metrics = {
    'WORST SETUP SLACK': 'Worst Setup Slack',
    'WORST HOLD SLACK': 'Worst Hold Slack',
    'WNS': 'WNS (Worst Negative Slack)',
    'TNS': 'TNS (Total Negative Slack)'
}

# Define custom colors for each plot
colors = ['#4682B4', '#FFA07A', '#3CB371', '#FF6347']

# 2. Generate a separate plot for each metric
for i, (col_name, plot_title) in enumerate(plot_metrics.items()):
    plt.figure(figsize=(12, 6)) # Create a new figure for each plot
    current_color = colors[i % len(colors)] # Cycle through colors
    
    # Plot the line graph
    plt.plot(
        df['Corner_Label'],
        df[col_name],
        marker='o',         # Circles for markers
        linestyle='-',      # Connect markers with a line
        color=current_color,
        linewidth=2,
        markersize=7
    )
    
    # Add data labels (readings) to each point
    for x, y in zip(df['Corner_Label'], df[col_name]):
        plt.text(x, y, f'{y:.2f}', ha='center', va='bottom' if y >= 0 else 'top',
                 fontsize=8, color='black') # Adjust vertical alignment based on positive/negative value

    # Add a horizontal red line at Slack = 0 (critical for timing analysis)
    plt.axhline(0, color='red', linestyle='--', linewidth=1.5, alpha=0.7)
    
    # Set title and labels
    plt.title(f'{plot_title} Across PVT Corners', fontsize=16)
    plt.xlabel('PVT Corner', fontsize=12)
    plt.ylabel(f'{plot_title} (ns)', fontsize=12)
    
    # Rotate X-axis labels for better readability
    plt.xticks(rotation=45, ha='right')
    
    # Add a grid for easier reading
    plt.grid(axis='y', linestyle='--', alpha=0.6)
    plt.tight_layout() # Adjust layout to prevent labels from overlapping
    
    # Display the plot
    plt.show()
```
# timing graphs
<img width="1484" height="1229" alt="OPEN_STA_SETUP_HOLD_TNS_WNS" src="https://github.com/user-attachments/assets/2caf65bd-cbd8-42a3-b2c1-0e20255ad2aa" />
#WORST SETUP SLACK
<img width="1189" height="590" alt="worst_set_up_opensta" src="https://github.com/user-attachments/assets/8f22bfe8-286d-407c-ab42-a5850f8187a2" />
#WORST HOLD SLACK
<img width="1189" height="590" alt="worst_hold_opensta" src="https://github.com/user-attachments/assets/c80c3a09-f7aa-424a-9255-68d5b587e348" />
#WNS
<img width="1189" height="590" alt="wns_opensta" src="https://github.com/user-attachments/assets/602d868c-dcc5-46e4-b296-d28ada0721f7" />
#TNS
<img width="1190" height="590" alt="tns_opensta" src="https://github.com/user-attachments/assets/369b3227-5316-479d-ba38-02b9d0d1e225" />

# VSDBabySoC OpenLane & OpenROAD Flow
This repository contains the VSDBabySoC design implemented using the Sky130 PDK and processed with OpenLane and OpenROAD tools. Below is a summary of the steps followed, including synthesis, floorplanning, routing, and viewing the layout in Magic.

# 1. Design Configuration
The design is configured with a config.tcl file in OpenLane:

```
bash
# Design name
set ::env(DESIGN_NAME) "vsdbabysoc"

# Source files
set ::env(VERILOG_FILES) "$::env(DESIGN_DIR)/src/vsdbabysoc_synth.v"
set ::env(VERILOG_INCLUDE_DIRS) "$::env(DESIGN_DIR)/src/include"
set ::env(EXTRA_LIBS) [glob $::env(DESIGN_DIR)/src/lib/*.lib]
set ::env(EXTRA_LEFS) [glob $::env(DESIGN_DIR)/src/lef/*.lef]
set ::env(EXTRA_GDS_FILES) [glob $::env(DESIGN_DIR)/src/gds/*.gds]

# SDC constraints
unset ::env(BASE_SDC_FILE)
set ::env(BASE_SDC_FILE) "$::env(DESIGN_DIR)/src/sdc/vsdbabysoc_synthesis.sdc"

# Clock configuration
set ::env(CLOCK_PERIOD) "20.0"
set ::env(CLOCK_PORT) "CLK"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

# IO and power nets (for PDN/tap)
set ::env(FP_IO_UNMATCHED_ERROR) 0
set ::env(VDD_PIN) "VPWR"
set ::env(GND_PIN) "VGND"
set ::env(FP_POWER_NET) $::env(VDD_PIN)
set ::env(FP_GROUND_NET) $::env(GND_PIN)

set ::env(PDN_POWER_STRAPS) "VPWR"
set ::env(PDN_GROUND_STRAPS) "VGND"
set ::env(FP_PDN_ENABLE_MACROS_GRID) 0

# Floorplanning configuration
set ::env(FP_PIN_ORDER_CFG) [glob $::env(DESIGN_DIR)/src/layout_conf/vsdbabysoc/pin_order.cfg]
set ::env(FP_SIZING) "absolute"
set ::env(DIE_AREA) "0 0 2500 2500"
set ::env(BOTTOM_MARGIN_MULT) 40
set ::env(TOP_MARGIN_MULT) 40
set ::env(LEFT_MARGIN_MULT) 150
set ::env(RIGHT_MARGIN_MULT) 150

# Tap/Decap configuration
set ::env(FP_TAP_HORIZONTAL_HALO) 10
set ::env(FP_TAP_VERTICAL_HALO) 10
set ::env(FP_TAPCELL_DIST) 14
set ::env(FP_ENDCAPCELL) "sky130_fd_sc_hd__decap_3"

# Placement configuration
set ::env(MACRO_PLACEMENT_CFG) [glob $::env(DESIGN_DIR)/src/layout_conf/vsdbabysoc/macro.cfg]

# Magic configuration
set ::env(MAGIC_ZEROIZE_ORIGIN) 0
set ::env(MAGIC_EXT_USE_GDS) 1

# PDN configuration
set ::env(FP_PDN_MACRO_HOOKS) ""
set ::env(FP_PDN_CHECK_NODES) 0

# Run options
set ::env(RUN_MCSTA) 0
set ::env(WRITE_DB) 1

# Ready for interactive flow
puts "Config loaded for design $::env(DESIGN_NAME). Ready for interactive flow."
```
#2. Running OpenLane Interactive Flow
```
cd /openlane
prep -design designs/VSDBabySoC
```
```
run_synthesis
run_floorplan
run_placement
run_cts
run_routing
run_magic
run_klayout
```

<img width="1855" height="886" alt="VSDBABYSOC_GDSII" src="https://github.com/user-attachments/assets/a366c0ca-bc03-4353-9d9f-f4a0f04cd4c6" />
<img width="1911" height="943" alt="ROUTING_!" src="https://github.com/user-attachments/assets/210e0ff2-3472-4c5b-8b14-0ae2d5737d94" />
<img width="1910" height="882" alt="HEATMAP_PLACEMENT" src="https://github.com/user-attachments/assets/86f8e322-5053-4e2b-93b3-edaa6cd72b4e" />
<img width="1913" height="947" alt="PLACEMENT" src="https://github.com/user-attachments/assets/e099fe6b-5d3c-4fdf-8fe1-c0d1fd59e12c" />
<img width="1916" height="966" alt="PLACEMENT_1" src="https://github.com/user-attachments/assets/5fbb1529-262a-44ad-9798-44aa0c1477c1" />
<img width="1918" height="882" alt="ROUTING" src="https://github.com/user-attachments/assets/0c2df9ac-7390-47b1-b440-9fd6ea551fcc" />

# Post-Layout STA & Timing Graphs Across PVT Corners for Routed VSDBabySoC

#Objective
To perform Post-Layout Static Timing Analysis (STA) using the SPEF generated after routing in Week 7, analyze timing across all PVT corners, and compare post-route results with post-synthesis timing data from Week 3.

1. Load Post-Route Design into OpenSTA
• Import:
  - Gate-level netlist
  - Library files for all PVT corners
  - Extracted SPEF file
  - Timing constraints (SDC)
  - Use the provided TCL script to run STA across multiple corners (TT, SS, FF, etc.).

```
TCL

# === Liberty corner list ===
set list_of_lib_files(1)  "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2)  "sky130_fd_sc_hd__tt_100C_1v80.lib"
set list_of_lib_files(3)  "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(4)  "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(5)  "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(6)  "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(7)  "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(8)  "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(9)  "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

# === Run tag ===
set run_tag "RUN_2025.11.19_05.52.46"

# === Units ===
set_cmd_units  -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
set_units      -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um

# === Extra libs ===
set base_libs "
/home/DINESH/OpenLane/designs/VSDBabySoC/skywater-pdk-libs-sky130_fd_sc_hd/timing/avsdpll.lib
/home/DINESH/OpenLane/designs/VSDBabySoC/skywater-pdk-libs-sky130_fd_sc_hd/timing/avsddac.lib
"

#########################
#  DEFINE FILES / OUTDIRS
#########################

# Post-Synthesis
set design_verilog_synth "/home/DINESH/OpenLane/designs/VSDBabySoC/runs/$run_tag/results/synthesis/vsdbabysoc.v"
set sdc_synth "/home/DINESH/OpenLane/designs/VSDBabySoC/src/sdc/vsdbabysoc_synthesis.sdc"
set outdir_synth "/home/DINESH/OpenLane/designs/VSDBabySoC/sta_output_$run_tag/post_synth"
exec mkdir -p $outdir_synth

# Post-Placement (Placed netlist)  <-- explicit stage added
set design_verilog_place "/home/DINESH/OpenLane/designs/VSDBabySoC/runs/$run_tag/results/placement/vsdbabysoc.pnl.v"
set sdc_place "/home/DINESH/OpenLane/designs/VSDBabySoC/src/script/pre_cts.sdc"
set outdir_place "/home/DINESH/OpenLane/designs/VSDBabySoC/sta_output_$run_tag/post_placement"
exec mkdir -p $outdir_place

# Pre-CTS (if you want separate pre-CTS SDC)
set design_verilog_prects "/home/DINESH/OpenLane/designs/VSDBabySoC/runs/$run_tag/results/placement/vsdbabysoc.pnl.v"
set sdc_pre_cts "/home/DINESH/OpenLane/designs/VSDBabySoC/src/script/pre_cts.sdc"
set outdir_pre_cts "/home/DINESH/OpenLane/designs/VSDBabySoC/sta_output_$run_tag/pre_cts"
exec mkdir -p $outdir_pre_cts

# Post-CTS
set design_verilog_cts "/home/DINESH/OpenLane/designs/VSDBabySoC/runs/$run_tag/results/cts/vsdbabysoc.v"
set sdc_post_cts "/home/DINESH/OpenLane/designs/VSDBabySoC/src/script/post_cts.sdc"
set outdir_cts "/home/DINESH/OpenLane/designs/VSDBabySoC/sta_output_$run_tag/post_cts"
exec mkdir -p $outdir_cts

# Post-Routing
set design_verilog_route "/home/DINESH/OpenLane/designs/VSDBabySoC/runs/$run_tag/results/routing/vsdbabysoc.pnl.v"
set sdc_route "/home/DINESH/OpenLane/designs/VSDBabySoC/src/sdc/vsdbabysoc_layout.sdc"
set outdir_route "/home/DINESH/OpenLane/designs/VSDBabySoC/sta_output_$run_tag/post_route"
exec mkdir -p $outdir_route

###############################################################
# STA PROCEDURE — KEEP ORIGINAL LOOP/STYLE
###############################################################
proc run_sta_stage {design_verilog sdc_file outdir stage_name} {

    global list_of_lib_files
    global base_libs

    puts "\n==============================="
    puts "  RUNNING STA STAGE: $stage_name"
    puts "==============================="

    for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {

        puts "\n=== Corner $i: $list_of_lib_files($i) ==="

        # Load corner liberty
        read_liberty "/home/DINESH/OpenLane/designs/VSDBabySoC/skywater-pdk-libs-sky130_fd_sc_hd/timing/$list_of_lib_files($i)"

        foreach lib $base_libs {
            read_liberty $lib
        }

        # Load netlist
        read_verilog $design_verilog
        link_design vsdbabysoc

        # Load SDC (constraints)
        read_sdc $sdc_file

        # Sanity check
        check_setup -verbose

        # Create report files on 1st iteration
        if { $i == 1 } {
            exec echo -n > $outdir/corners_list.txt
            exec echo -n > $outdir/sta_worst_max_slack.txt
            exec echo -n > $outdir/sta_worst_min_slack.txt
            exec echo -n > $outdir/sta_tns.txt
            exec echo -n > $outdir/sta_wns.txt
        }

        exec echo "$list_of_lib_files($i)" >> $outdir/corners_list.txt

        # Full path check
        report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits 4 \
            > $outdir/sta_min_max_$list_of_lib_files($i).txt

        # Worst setup
        exec echo -ne "$list_of_lib_files($i)    " >> $outdir/sta_worst_max_slack.txt
        report_worst_slack -max -digits 4 >> $outdir/sta_worst_max_slack.txt

        # Worst hold
        exec echo -ne "$list_of_lib_files($i)    " >> $outdir/sta_worst_min_slack.txt
        report_worst_slack -min -digits 4 >> $outdir/sta_worst_min_slack.txt

        # TNS / WNS
        exec echo -ne "$list_of_lib_files($i)    " >> $outdir/sta_tns.txt
        report_tns -digits 4 >> $outdir/sta_tns.txt

        exec echo -ne "$list_of_lib_files($i)    " >> $outdir/sta_wns.txt
        report_wns -digits 4 >> $outdir/sta_wns.txt
    }

    puts "=== FINISHED STAGE: $stage_name ==="
}

###############################################################
# RUN STAGES (in order)
###############################################################

# 1) Post-Synthesis
run_sta_stage $design_verilog_synth  $sdc_synth     $outdir_synth  "Post-Synthesis"

# 2) Post-Placement (Placed netlist)
run_sta_stage $design_verilog_place  $sdc_place     $outdir_place  "Post-Placement"

# 3) Pre-CTS (placement SDC / checks before CTS)
run_sta_stage $design_verilog_prects $sdc_pre_cts   $outdir_pre_cts "Pre-CTS"

# 4) Post-CTS
run_sta_stage $design_verilog_cts    $sdc_post_cts  $outdir_cts    "Post-CTS"

# 5) Post-Routing (original block)
run_sta_stage $design_verilog_route  $sdc_route     $outdir_route  "Post-Routing"

puts "\n==============================="
puts "     ALL STA FINISHED"
puts "==============================="

exit


```

## Post-Synthesis Static Timing Analysis (STA) Results

The following table summarizes the timing analysis across different Process, Voltage, and Temperature (PVT) corners.

| Library Corner (sky130_fd_sc_hd) | WNS (Max) | TNS (Max) | Worst Slack (Max) | Worst Slack (Min) | Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **tt_025C_1v80** | 0.00 | 0.00 | 3.17 | 0.32 | PASS |
| **tt_100C_1v80** | 0.00 | 0.00 | 3.22 | 0.32 | PASS |
| **ff_100C_1v65** | 0.00 | 0.00 | 5.24 | 0.25 | PASS |
| **ff_100C_1v95** | 0.00 | 0.00 | 6.50 | 0.20 | PASS |
| **ff_n40C_1v56** | 0.00 | 0.00 | 3.89 | 0.29 | PASS |
| **ff_n40C_1v65** | 0.00 | 0.00 | 4.81 | 0.26 | PASS |
| **ff_n40C_1v76** | 0.00 | 0.00 | 5.63 | 0.23 | PASS |
| **ff_n40C_1v95** | 0.00 | 0.00 | 6.56 | 0.19 | PASS |
| **ss_100C_1v40** | -13.11 | -2847.00 | -13.11 | 0.92 | **FAIL** |
| **ss_100C_1v60** | -5.36 | -786.55 | -5.36 | 0.65 | **FAIL** |
| **ss_n40C_1v28** | -48.72 | -14854.05 | -48.72 | 1.92 | **FAIL** |
| **ss_n40C_1v35** | -29.98 | -8542.96 | -29.98 | 1.41 | **FAIL** |
| **ss_n40C_1v40** | -22.19 | -5933.80 | -22.19 | 1.17 | **FAIL** |
| **ss_n40C_1v44** | -17.61 | -4419.96 | -17.61 | 1.03 | **FAIL** |
| **ss_n40C_1v60** | -6.96 | -1195.05 | -6.96 | 0.68 | **FAIL** |
| **ss_n40C_1v76** | -1.92 | -109.97 | -1.92 | 0.51 | **FAIL** |

**Legend:**
* **WNS (Max):** Worst Negative Slack (Setup Time)
* **TNS (Max):** Total Negative Slack
* **Worst Slack (Min):** Hold Time
* 
<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/ce9590ca-b312-4b91-af73-95cfa7e880c4" />
<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/6740e07d-c82a-42c3-95cb-df196249594a" />
<img width="1189" height="590" alt="download" src="https://github.com/user-attachments/assets/2cf034bb-937d-46a8-bc01-74464f5709d6" />
<img width="1190" height="590" alt="download" src="https://github.com/user-attachments/assets/28b6976c-51e5-4e17-917a-8b3bf4dbc1a4" />


## Post-CTS Static Timing Analysis (STA) Results

The following table summarizes the timing analysis after Clock Tree Synthesis (CTS) across different PVT corners.

| Library Corner (sky130_fd_sc_hd) | WNS (Max) | TNS (Max) | Worst Slack (Max) | Worst Slack (Min) | Status |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **tt_025C_1v80** | 0.00 | 0.00 | 3.17 | 0.32 | PASS |
| **tt_100C_1v80** | 0.00 | 0.00 | 3.22 | 0.32 | PASS |
| **ff_100C_1v65** | 0.00 | 0.00 | 5.24 | 0.25 | PASS |
| **ff_100C_1v95** | 0.00 | 0.00 | 6.50 | 0.20 | PASS |
| **ff_n40C_1v56** | 0.00 | 0.00 | 3.89 | 0.29 | PASS |
| **ff_n40C_1v65** | 0.00 | 0.00 | 4.81 | 0.26 | PASS |
| **ff_n40C_1v76** | 0.00 | 0.00 | 5.63 | 0.23 | PASS |
| **ff_n40C_1v95** | 0.00 | 0.00 | 6.56 | 0.19 | PASS |
| **ss_100C_1v40** | -13.11 | -2847.00 | -13.11 | 0.92 | **FAIL** |
| **ss_100C_1v60** | -5.36 | -786.55 | -5.36 | 0.65 | **FAIL** |
| **ss_n40C_1v28** | -48.72 | -14854.05 | -48.72 | 1.92 | **FAIL** |
| **ss_n40C_1v35** | -29.98 | -8542.96 | -29.98 | 1.41 | **FAIL** |
| **ss_n40C_1v40** | -22.19 | -5933.80 | -22.19 | 1.17 | **FAIL** |
| **ss_n40C_1v44** | -17.61 | -4419.96 | -17.61 | 1.03 | **FAIL** |
| **ss_n40C_1v60** | -6.96 | -1195.05 | -6.96 | 0.68 | **FAIL** |
| **ss_n40C_1v76** | -1.92 | -109.97 | -1.92 | 0.51 | **FAIL** |

<img width="1389" height="690" alt="download" src="https://github.com/user-attachments/assets/88694cff-c10a-47d1-88c0-dc754c403024" />
<img width="1389" height="690" alt="download" src="https://github.com/user-attachments/assets/a152d6b5-5d9a-487a-a5f8-a6684076a8e2" />
<img width="1389" height="690" alt="download" src="https://github.com/user-attachments/assets/6986e87c-e9f9-470a-bc89-97d8b9f4155d" />
<img width="1389" height="690" alt="download" src="https://github.com/user-attachments/assets/fbff6a78-a6e3-4937-bca1-fe71da3f6edd" />




**Legend:**
* **WNS (Max):** Worst Negative Slack (Setup Time) - *Negative values indicate violations.*
* **TNS (Max):** Total Negative Slack - *Sum of all setup violations.*
* **Worst Slack (Min):** Hold Time - *Positive values indicate successful hold timing.*


# post synthesis vs post cts
<img width="1389" height="690" alt="image" src="https://github.com/user-attachments/assets/ef0c6be1-d3ae-4cf4-9b88-ffc1d88393f4" />
