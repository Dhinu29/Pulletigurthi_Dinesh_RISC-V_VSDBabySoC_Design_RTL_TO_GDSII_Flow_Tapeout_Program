# ☀️P_DINESH_WEEK_3_RISC_V_SoC_Tapeout_Program_VSD
# Static Timing Analysis (STA) Fundamentals
This task introduces the fundamental principles of Static Timing Analysis (STA), a critical step in the digital design flow used to verify a circuit's performance after synthesis. Unlike dynamic simulation, STA is an exhaustive, mathematical method that checks all possible signal paths to ensure the design can operate reliably at its target clock frequency without requiring functional test vectors.

#The Core Concept: Timing Paths and Slack
The foundation of STA is the analysis of timing paths, which measure signal delays between sequential elements (like flip-flops) or I/O ports. The primary metric is Slack (Slack=Required Time−Arrival Time). Slack determines the timing margin and indicates whether a path is meeting its timing constraint.

# ⚡Critical Timing Checks
STA focuses on two essential checks:

1️⃣ **Setup Time Check (Circuit Speed):** This verifies that data arrives early enough before the clock edge to be reliably captured. Negative Setup Slack (known as Worst Negative Slack or WNS) signifies a timing failure because the path is too slow, limiting the maximum operational clock frequency.

2️⃣ **Hold Time Check (Data Stability):** This verifies that data remains stable long enough after the clock edge has passed. Negative Hold Slack signifies a timing failure because the data changed too quickly, which can cause data corruption.

# Practical Application with OpenSTA
Using tools like OpenSTA, the task involves loading the synthesized netlist and constraints to perform a complete timing audit. The practical goal is to generate timing graphs and reports that clearly identify the critical path—the path in the design with the smallest (most negative) slack. By interpreting the slack values and critical paths, you gain crucial insights necessary to optimize the circuit and ensure all timing constraints are met.

# 🤖To ```Install``` OpenSTA
```
bash
# On your Home Directory type the following command
sudo apt-get update
sudo apt-get install build-essential tcl-dev tk-dev cmake git

# Clone the OpenSTA repository by executing the following command:
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..

```
If it shows an error
```
bash
# Move to the home directory using following command:
cd

# Install the eigen by using following command:
sudo apt-get install libeigen3-dev
cmake ..

#move to your home directory and Clone the CUDD repository by executing the followingcommand:
git clone https://github.com/ivmai/cudd.git

sudo apt-get install automake
sudo apt-get install autoconf m4 perl
cd cudd
autoreconf -i
mkdir build
cd build
../configure --prefix=$HOME/cudd
make
make install

```
```
bash
# Move into the OpenSTA directory that was created during the cloning process using the
cd OpenSTA
cd build
cmake .. -DUSE_CUDD=ON -DCUDD_DIR=$HOME/cudd
make
sudo make install
sta
```
<img width="1918" height="303" alt="image" src="https://github.com/user-attachments/assets/341a864d-423b-4ee0-83d5-a1b8100eaebc" />

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

The process of performing Static Timing Analysis (STA) and visualizing its Timing Graph Analysis in Google Colab primarily involves three stages: running an STA tool, parsing the report, and then using Python libraries for visualization.


1. Understanding STA and Timing Graph Analysis
Static Timing Analysis (STA) is a method used in digital circuit design (like VLSI/FPGA) to verify the timing of a circuit. Instead of simulating the circuit's logic for all possible inputs (dynamic simulation), STA analyzes the circuit's netlist and calculates the signal propagation delay along every possible path.


2. Running STA in Google Colab
Since Colab is a Python environment, you cannot directly run proprietary commercial STA tools (like PrimeTime). However, you have two primary methods to perform the analysis:

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








