# P_DINESH_WEEK_5_RISC_V_SoC_Tapeout_Program_VSD

# 🧾 OpenROAD Installation Report

# 📘 1. Introduction

OpenROAD (Open Routing, Optimization, and Analysis for Designs) is an open-source physical design toolchain that automates the digital ASIC design flow.
This week’s objective was to install and configure OpenROAD on a Linux system (Ubuntu 22.04 / WSL2).

# 🎯 2. Objective

- To install OpenROAD successfully on Ubuntu.

- To verify the installation through sample design runs.

- To document issues, dependencies, and resolutions.

 # 🧩 3. Installation Steps
```
#steps followed:

sudo apt install -y libgtest-dev cmake build-essential
sudo apt install libspdlog-dev
sudo apt install -y liblemon-dev

git clone https://github.com/google/or-tools.git
cd or-tools
mkdir build && cd build
cmake -DBUILD_DEPS=ON -DCMAKE_BUILD_TYPE=Release ..
make -j$(nproc)
sudo make install

git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD/
sudo ./etc/DependencyInstaller.sh -base
mkdir build
cd build
cmake ..

```


<img width="1918" height="880" alt="openroad" src="https://github.com/user-attachments/assets/44ea40a4-454f-419e-8a4f-ac79fe68fc9d" />
<img width="1918" height="931" alt="openroad_example" src="https://github.com/user-attachments/assets/1e1243c8-226e-45ba-bf8d-54228d6336d8" />

