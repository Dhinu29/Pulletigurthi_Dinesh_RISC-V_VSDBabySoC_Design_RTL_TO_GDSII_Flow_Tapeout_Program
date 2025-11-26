<img width="1389" height="690" alt="download" src="https://github.com/user-attachments/assets/84bdfd1f-08a8-413a-8aea-935776ef4412" /># P_DINESH_WEEK_8_RISC_V_SoC_Tapeout_Program_VSD
## Week 8 Task – Post-Layout STA & Timing Graphs Across PVT Corners for Routed VSDBabySoC

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
