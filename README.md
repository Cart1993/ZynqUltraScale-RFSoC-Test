# ZynqUltraScale-RFSoC-Test

This repository is a professional-grade development environment for the **Zynq UltraScale+ RFSoC (ZCU208)**. It is designed to validate a high-performance HW/SW workflow using **Vivado 2023.2** and **PetaLinux 2023.2**, specifically focusing on DMA functionality, automated project regeneration, and a defensive version control strategy.

---

## 📂 Project Structure

The repository is organized to decouple hardware design from software configuration, following a "Single Source of Truth" architecture.

```text
ZynqUltraScale-RFSoC-Test/
├── Vivado/                 # Hardware Development (PL)
│   ├── [project_name].xpr  # Project file
│   ├── rebuild_hw.tcl      # Script to recreate the project from scratch
│   ├── [project_name].xsa  # Hardware Handoff File (The bridge)
│   └── ...                 # HDL sources (.v, .vhd) and Constraints (.xdc)
├── [petalinux-project]/    # Software Development (PS)
│   ├── project-spec/       # Custom recipes, device tree, and configurations
│   ├── config              # Main PetaLinux configuration
│   └── .petalinux/metadata # Essential project metadata
└── .gitignore              # Global defensive filter

🛠 Workflow & Methodology
1. Hardware Mastery (Vivado)
The hardware design is focused on RFSoC capabilities and DMA data movement. Instead of tracking gigabytes of temporary synthesis data, we use a TCL-based regeneration flow.

Automation: The rebuild_hw.tcl script ensures that the Block Design and project settings can be restored on any machine.

Handoff: The .xsa file is the definitive link to the software domain.

2. Software Agility (PetaLinux)
A custom Linux distribution built for the ZCU208.

Portability: Only the logic and configurations are versioned.

Storage Strategy: Optimized for a QSPI + TFTP boot flow, allowing for rapid kernel iterations without physical SD card handling.

🛡 Defensive Version Control Strategy
This repository employs a Defensive Whitelist Approach in its .gitignore.

Senior Engineer Note: "Control the environment, or the environment will control you."

By ignoring everything by default (*) and explicitly allowing only source files, we ensure:

Zero Bloat: No build/, runs/, or cache/ folders in the history.

Integrity: The repository remains under 100MB instead of several GBs.

Compliance: We strictly follow AMD/Xilinx official documentation for PetaLinux metadata persistence.

🔄 How to Rebuild
Rebuild the Vivado Project
Open Vivado 2023.2.

In the Tcl Console, navigate to the Vivado/ folder.

Run:

Tcl
source ./rebuild_hw.tcl
Rebuild the PetaLinux Project
Source your PetaLinux environment.

Navigate to the project folder and import the hardware:

Bash
petalinux-config --get-hw-description=../Vivado/
Build the system:

Bash
petalinux-build
