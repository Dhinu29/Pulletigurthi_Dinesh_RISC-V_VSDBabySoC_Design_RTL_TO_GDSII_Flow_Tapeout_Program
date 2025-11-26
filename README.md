# RISC-V_Reference_SoC_Tapeout_Program

# 🧠 RISC-V Reference SoC Tapeout Program — Weekly Reports

This document contains all my weekly project reports for the **RISC-V Reference SoC Tapeout Program** (10-week open-source VLSI design training).  
All work was done using open-source tools — Icarus Verilog, GTKWave, Ngspice, Yosys, OpenSTA, and OpenLane with the Sky130 PDK.

---

## 📅 Weekly Report Links

| Week | Topic | GitHub Repository | Description | Status |
|------|--------|------------------|--------------|---------|
| **Week 0** | Tool Installation & Environment Setup | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_0_RISC_V_SoC_Tapeout_Program_VSD) | Installed and configured all open-source tools (Icarus Verilog, GTKWave, Ngspice, Yosys, OpenSTA, OpenLane, Magic, KLayout, Sky130 PDK). Verified environment on Ubuntu. | ✅ Completed |
| **Week 1** | Digital Design Flow: RTL, Simulation & Synthesis | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_1_RISC_V_SoC_Tapeout_Program_VSD) |  Designed Verilog RTL modules (combinational & sequential logic),Simulated using Icarus Verilog and analyzed waveforms in GTKWave,Synthesized the RTL to a gate-level netlist with Yosys (targeting a standard cell library) | ✅ Completed |
| **Week 2** | Pre/Post-Synthesis Simulation & Synthesis of VSDBabySoC | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_2_RISC_V_SoC_Tapeout_Program_VSD) |Performed pre-synthesis RTL simulation, synthesized the design using Yosys, and verified functionality with post-synthesis gate-level simulation of VSDBabySoC. | ✅ Completed |
| **Week 3** | Static Timing Analysis (STA) Fundamentals | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_3_RISC_V_SoC_Tapeout_Program_VSD) | Performed Static Timing Analysis using OpenSTA to check timing of the synthesized VSDBabySoC netlist and ensured design meets constraints. | ✅ Completed |
| **Week 4** | Ngspice Analog Simulation | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_4_RISC_V_SoC_Tapeout_Program_VSD) | Simulated analog/mixed-signal circuits (inverter, op-amp) using **Ngspice** with Sky130 device models. | ✅ Completed |
| **Week 5** |OpenROAD Installation|   [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_5_RISC_V_SoC_Tapeout_Program_VSD)  | OpenROAD (Open Routing, Optimization, and Analysis for Designs) is an open-source physical design toolchain that automates the digital ASIC design flow. ||  ✅ Completed |
| **Week 6** | physical design | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_6_RISC_V_SoC_Tapeout_Program_VSD) | Implemented picorv32a through the stages of Synthesis, Floorplanning, Placement, STA, and CTS.|  ✅ Completed  |
| **Week 7** | OpenROAD Flow from RTL to GDSII | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_7_RISC_V_SoC_Tapeout_Program_VSD)|Implemented the complete physical design flow (RTL-to-GDSII) for VSDBabySoC using OpenROAD. | ✅ Completed  |
| **Week 8** | Multi-Stage Static Timing Analysis | [GitHub Link](https://github.com/Dhinu29/P_DINESH_WEEK_8_RISC_V_SoC_Tapeout_Program_VSD) |As part of the RTL-to-GDSII flow, I performed Static Timing Analysis (STA) at the post-synthesis and post-CTS stages.| ✅ Completed |
| **Week 9** | End-to-End RTL → GDSII Flow Summary | [GitHub Link](https://github.com/Dhinu29/Pulletigurthi_Dinesh_RISC-V_VSDBabySoC_Design_RTL_TO_GDSII_Flow_Tapeout_Program) | A complete overview of the full design flow carried out from Verilog RTL to final mask-ready GDSII generation using open-source tools. | ✅ Completed |
---

## 🧩 Tools Used
- Icarus Verilog, GTKWave  
- Ngspice  
- Yosys  
- OpenSTA  
- OpenLane, Magic, KLayout 
- SkyWater SKY130 PDK  

---
## ⚡Summary Table
| Section Title | What It Covers | Tools Used |
|--------------|----------------|------------|
| End-to-End RTL → GDSII Flow Summary | Complete RTL → GDSII flow overview | Icarus Verilog, Yosys, OpenSTA, OpenROAD, Magic, KLayout, Sky130 PDK |
| Design Architecture & Block Overview | SoC hierarchy and module connectivity | Draw.io / Excalidraw for block diagram |
| Toolchain & Flow Configuration | Setup, PDK path, OpenLane configs, scripts | OpenLane, Sky130A PDK, Yosys config, STA SDC files |
| STA Timing Summary Across Stages | WNS/TNS analysis at multiple stages | OpenSTA (Pre-CTS, Post-CTS, Post-Route) |
| Physical Design Results | Floorplan, placement, routing, CTS results | OpenROAD/OpenLane (FP → Place → CTS → Route) |
| DRC / LVS / Antenna Reports | Signoff verification and physical checks | Magic, KLayout, OpenLane DRC/Antenna check |
| Final GDSII Layout Visuals | Final chip layout screenshots & zooms | KLayout (viewing) / Magic (editing) |
| Achievements & Milestones | Completed flow deliverables summary | All tools combined output |
| Challenges Faced & Fixes Applied | Timing closure, routing issues, tool errors | STA debugging in OpenSTA / routing fix in OpenROAD |
| Future Work & Improvements | Next steps like power optimization, macros | OpenROAD advanced configs / SRAM macros |
| Key Learnings & Technical Takeaways | What was learned through RTL → GDSII | Static Timing, Floorplan, Routing, GDS signoff knowledge |


---

## 🏁 Final Output
Generated a **tapeout-ready RISC-V SoC layout (GDSII)** using open-source tools.

---

**Author:** Pulletigurthi Dinesh  
**Program:** RISC-V Reference SoC Tapeout (10 Weeks)  
**Year:** 2025


## Final Documentation & Project Completion
