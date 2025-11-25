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
| **Week 9** | 🔜Coming soon | — | _Pending Update_ | ✅ Completed |
| **Week 10** | 🔜Coming soon | — | _Pending Update_ | ⏳ Pending |
---

## 🧩 Tools Used
- Icarus Verilog, GTKWave  
- Ngspice  
- Yosys  
- OpenSTA  
- OpenLane, Magic, KLayout 
- SkyWater SKY130 PDK  

---

## 🏁 Final Output
Generated a **tapeout-ready RISC-V SoC layout (GDSII)** using open-source tools.

---

**Author:** Pulletigurthi Dinesh  
**Program:** RISC-V Reference SoC Tapeout (10 Weeks)  
**Year:** 2025
