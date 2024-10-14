<!---
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow)1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/92eb860b-7a88-4c6f-8143-ad3e09fd9c5b)
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow) (1)1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/4285c5e4-d5df-43e4-b460-ead45ff67f9b)
-->
![Digital_VLSI_SoC_Design_ _Planning_(RTL2GDSII_Flow) (1)2](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5b8bdb5f-95c7-41c0-b809-711b2b8ad171)
# Nasscom-VSD Digital VLSI SoC Design and Planning

![Static Badge](https://img.shields.io/badge/OS-linux-orange)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-OpenLANE--Flow%2C_Yosys%2C_abc%2C_OpenROAD%2C_TritonRoute%2C_OpenSTA%2C_magic%2C_netgen%2C_GUNA-navy)
![Static Badge](https://img.shields.io/badge/languages-verilog%2C_bash%2C_TCL-crimson)
![GitHub last commit](https://img.shields.io/github/last-commit/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub language count](https://img.shields.io/github/languages/count/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub top language](https://img.shields.io/github/languages/top/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub repo size](https://img.shields.io/github/repo-size/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/fayizferosh/soc-design-and-planning-nasscom-vsd)
![GitHub repo file count (file type)](https://img.shields.io/github/directory-file-count/fayizferosh/soc-design-and-planning-nasscom-vsd)
<!---
Comments
-->

> 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM

## Table of Contents
- [Section - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK](#Section---1-Introduction-of-Open-Source-EDA-OpenLane-and-Sky130-PDK)

## Overview Of QFN-48 Chip (PicoRV32 - A Size-Optimized RISC-V CPU)
VSD Squadron Board: The VSD Board is shown below. Our focus is on the enclosed region containing the "Microprocessor (PicoRV32A-Cpu)," which will be designed using the RTL to GDS flow, progressing from the abstract design level to the fabrication stage.

![image](https://github.com/user-attachments/assets/844ea7fc-f4f4-4353-ae5d-d75a4507d075)

## Section - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK

### Theory

<details>
  <summary>
Expand or Collapse
  </summary>

#### Introduction to IC Design components and terminologies
![image](https://github.com/user-attachments/assets/27c6db30-a2c8-4ec5-b9ff-82bdabe66d2f)
![image](https://github.com/user-attachments/assets/7fbe0214-8bf7-491a-b26d-3e698c1d2615)

#### Chip

* Now, taking a look inside the chip, all the signals from the external world to the chip and vice versa is passed through ***PADS***. The area bound by the pads is ***CORE*** where all the digital logic of the chip is placed. Both the core and pads make up the ***DIE*** which is the basic manufacturing unit in regards to semiconductor chips.

![image](https://github.com/user-attachments/assets/eb006c7f-93c8-4348-8cb8-8fa51160735c)

* ***FOUNDRY*** is the place where the semiconductor chips are manufactured and ***FOUNDRY IP's*** are Intellectual Properties based on a specific foundry and these IP's require a specific level of intelligence to be produced whereas, repeatable digital logic blocks are called ***MACROS***.

![image](https://github.com/user-attachments/assets/e6118285-f7f7-4d0d-8b2e-f2684f6a3eac)

#### ISA (Intruction Set Architecture)

* A C program which has to be run on a specific hardware layout which is the interior of a chip in your laptop, there is certain flow to be followed.
* Initially, this particular C program is compiled in it's assembly language program which is nothing but ***RISC-V ISA (Reduced Instruction Set Compting - V Intruction Set Architecture)***.
* Following this, the assembly language program is then converted to machine language program which is the binary language logic 0 and 1 which is understood by the hardware of the computer.
* Directly after this, we've to implement this RISC-V specification using some ***RTL (a Hardware Description Language)***. Finally, from the RTL to ***Layout*** it is a standard PnR or RTL to GDSII flow.

![image](https://github.com/user-attachments/assets/85b9292f-ec1c-427f-a7f2-3e17def8b1fd)

* For an application software to be run on a hardware there are several processes taking place. To begin with, the apps enters into a block called system software and it converts the application program to binary language. There are various layers in system software in which the major layers or components are OS (Operating System), Compiler and Assembler.
* At first the OS outputs are small function in C, C++, VB or Java language which are taken by the respective compiler and converted into instructions and the syntax of these instructions varies with the hardware architecture on which the system is implemented.
* Then, the job of the assembler is to take these instructions and convert it into it's binary format which is basically called as a machine language program. Finally, this binary language is fed to the hardware and it understands the specific functions it has to perform based on the binary code it receives.

![image](https://github.com/user-attachments/assets/e7af04bd-ba36-4f0a-bb04-f83067cd0b3a)

* For example, if we take a stopwatch app on RISC-V core, then the output of the OS could be a small C function which enters into the compiler and we get output RISC-V instructions following this, the output of the assembler will be the binary code which enters into your chip layout.

![image](https://github.com/user-attachments/assets/732a552f-c0a7-485f-ba44-f955aeec5c3f)

* For the above stopwatch the following are the input and output of the compiler and assembler.

![image](https://github.com/user-attachments/assets/9c47e8ed-ee6a-42d7-86d6-ecdd365d208b)

* The output of the compiler are instructions and the output of the assembler is the binary pattern. Now, we need some RTL (a Hardware Description Language) which understands and implements the particular instructions. Then, this RTL is synthesised into a netlist in form of gates which is fabricated into the chip through a physical design implementation.

![Screenshot (282)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/e349cb06-45e3-4ae4-b85f-9020a0a62737)

* There are mainly 3 different parts in this course. They are:
1. RISC-V ISA
2. RTL and synthesis of RISC-V based CPU core - picorv32
3. Physical design implementation of picorv32

![Screenshot (283)](https://github.com/fayizferosh/risc-v-myth-report/assets/63997454/832f0ea6-2d60-4d9a-937c-a2dedd5f8cac)

#### Open-source Implementation

* For open-source ASIC design implemantation, we require the following enablers to be readily available as open-source versions. They are:-
1. RTL Designs
2. EDA Tools
3. PDK Data

* Initially in the early ages, the design and fabrication of IC's were tightly coupled and were only practiced by very few companies like TI, Intel, etc.
* In 1979, Lynn Conway and Carver Mead came up with an idea to saperate the design from the fabrication and to do this they inroduced structured design methodologies based on the λ-based design rules and published the first VLSI book "Introduction to VLSI System" which started the VLSI education.
* This methodology resulted in the emergence of the design only companies or ***"Fabless Companies"*** and fabrication only companies that we usually refer to as ***"Pure Play Fabs"***.
* The inteface between the designers and the fab by now became a set of data files and documents, that are reffered to as the ***"Process Design Kits (PDKs)"***.
* The PDK include but not limited to Device Models, Technology Information, Design Rules, Digital Standard Cell Libraries, I/O Libraries and many more.
* Since, the PDK contained variety of informations, and so they were distributed only under NDAs (Non-Disclosure Agreements) which made it in-accessible to the public.
* Recently, Google worked out an agreement with skywater to open-source the PDK for the 130nm process by skywater Technology, as a result on 30 June 2020 Google released the first ever open-source PDK.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/87384374-e66b-4ec6-b9c4-3fb92ad4d275)

* ASIC design is a complex step that involves tons of steps, various methodologies and respective EDA tools which are all required for successful ASIC implementation which is achieved though an ASIC flow which is nothing but a piece of software that pulls different tools togather to carry out the design process.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/1762d6d6-c5f8-4bd9-8a3d-968eb4360889)

#### OpenLANE Open-source ASIC Design Implementation Flow

#### Typical Flow in OpenLane:
1. **Synthesis:**- Converting RTL code to a gate-level netlist.

 ![image](https://github.com/user-attachments/assets/2c619ac7-5117-48aa-92f1-516a3f191dc1)

2. **Floorplanning:**- Defining the layout of the chip, including the placement of blocks and the arrangement of IOs.
 ![image](https://github.com/user-attachments/assets/3581a3b6-24aa-49e8-8632-5a3e8de32f74)

3. **Placement:**- Placing the synthesized gates on the chip floorplan.
 ![image](https://github.com/user-attachments/assets/4d947db7-68e4-40ae-ad60-fabf697abd43)

4. **Clock Tree Synthesis (CTS):**- Designing a clock distribution network to meet timing requirements.

   
 ![image](https://github.com/user-attachments/assets/64904d1c-1c2e-4820-ba7c-e2d86367aa7e)

5. **Routing:**- Creating the metal connections between gates and blocks.
 ![image](https://github.com/user-attachments/assets/2dd57219-bae8-4afd-b4f8-54d695ed71f0)

6. **Signoff:**- Performing final checks, including static timing analysis, design rule checks (DRC), and layout versus schematic (LVS) checks.

* The main objective of the ASIC Design Flow is to take the design from the RTL (Register Transfer Level) all the way to the GDSII, which is the format used for the final fabrication layout.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/533f58ee-4524-4a18-abb5-36b4d6a56b1f)

* Synthesis is the process of convertion or translation of design RTL into circuits made out of Standard Cell Libraries (SCL) the resultant circuit is described in HDL and is usually reffered to as the Gate-Level Netlist.
* Gate-Level Netlist is functionally equivalent to the RTL.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e43891a0-ab09-4df2-898d-7e843158e936)

* The fundemental building blocks which are the standard cells have regular layouts.
* Each cell has different views/models which are utilised by different EDA tools like liberty view with electrical models of the cells, HDL behavioral models, SPICE or CDL views of the cells, Layout view which include GDSII view which is the detailed view and LEF view which is the abstract view.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/48df884c-c894-4a2a-a511-09321c208d6b)

* Chip Floor Planning

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/38ecd866-ac83-42c7-83ba-2ba995f9ba4e)

* Macro Floor Planning

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/35ac760c-bba9-4bd1-9c65-e8ceeee2ccf5)

* Power Planning typically uses upper metal layers for power distribution since thay are thicker than lower metal layers and so have lower resistance and PP is done to avoid electron migration and IR drops.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/f18013a7-e4c7-4a6d-ba53-33362c04689a)

* Placement

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/3654398b-40bc-4e42-9205-77f673d5584c)

* Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/817c504d-0b8e-4a0a-9c3c-e9d110c23535)

* Clock Tree Synthesis

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6db284d4-065f-450a-9200-a6e6cbfd7fbb)

* Clock skew is the time difference in arrival of clock at different components.
* Routing

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/7458495f-b527-4f21-813e-8dbcbf29ed9b)

* skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer the following 5 layers are all Aluminium layers.

![stackup](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e4438c12-d83e-4083-a58f-33a410a47927)

* Global and Detailed Routing

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b54ebd4c-127a-441f-829e-6000531e9b8d)

* Once done with the routing the final layout can be generated which undergoes various Sign-Off checks.
* Design Rules Checking (DRC) which verifies that the final layout honours all design fabrication rules.
* Layout Vs Schematic (LVS) which verifies that the final layout functionality matches the gate-level netlist that we started with.
* Static Timing Analysis (STA) to verify that the design runs at the designated clock frequency.

![image](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d8bc72cd-2fe9-4ae2-9431-68a6fa77c671)

</details>

### Implementation

Section 1 tasks:- 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the flop ratio.

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```

#### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

Screenshots of running each commands

![1](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/d19f6d0f-16f8-4e79-aa5a-f2a34b9fb203)
![2](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5e03c8ca-8c7f-4579-a7bc-10161007910e)
![3](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/5f196a31-059e-4192-a208-8a15ba1a0dd7)

#### 2. Calculate the flop ratio.

Screenshots of synthesis statistics report file with required values highlighted

![Screenshot from 2024-10-07 16-23-44](https://github.com/user-attachments/assets/325a932d-66bd-4de5-9614-88a47e794c19)
![Screenshot from 2024-10-07 16-24-05](https://github.com/user-attachments/assets/6ea028a4-b292-47cb-975e-8e741d54790e)

Calculation of Flop Ratio and DFF % from synthesis statistics report file

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```

# Section - 2 Good Floorplan vs bad Floorplan and Introduction to library cells

## Utilization factor and Aspect ratio of a chip or die:

The utilization factor and aspect ratio are critical metrics in chip or die design and fabrication:

**Utilization Factor:**
- **Definition:**  The utilization factor, often referred to as area utilization, is a measure of how efficiently the available area on a chip or die is used by the actual logic, memory, and other functional components. It is calculated as the ratio of the area occupied by the functional elements to the total available area of the chip.
Formula:
Utilization Factor = (Area Occupied by Functional Components/Total Chip Area)×100 %

- Importance: A higher utilization factor indicates that more of the chip's area is used for functional purposes, which is generally desirable for cost efficiency. However, it needs to be balanced with considerations like routing space, power distribution, and thermal management.

**Aspect Ratio:**
- **Definition:** The aspect ratio of a chip or die refers to the ratio of its width to its height. It is an important parameter in the physical design and packaging of semiconductor devices.
Formula:
Aspect Ratio = Height of the Chip/Width of the Chip

 
- Importance: The aspect ratio affects the chip's mechanical stability, ease of packaging, and manufacturability. Ideally, the aspect ratio should be close to 1 (i.e., the chip is nearly square), as this minimizes stress and simplifies the packaging process. However, depending on the application and design constraints, the aspect ratio may vary.


![Screenshot 2024-08-24 221207](https://github.com/user-attachments/assets/c41d2c60-621c-4e05-a93e-bfe2dc929d52)

**Concept of pre-placed cells and de-coupling capacitors:**
- **Pre-placed Cells:**
Pre-placed cells are components or functional units within an IC or on a PCB that are placed at specific locations before the main placement and routing process begins. These cells are typically used for power management, signal integrity, or other critical functions that require precise positioning to meet design constraints. Pre-placed cells can include decoupling capacitors, power pads, voltage regulators, or other components that are essential for the proper functioning of the circuit.

- **Decoupling Capacitors:**
Decoupling capacitors are small-value capacitors placed between the power supply rails (Vcc and Gnd) of an integrated circuit or on a PCB to provide a local source of energy that can supply transient current demands of the active devices. They help to "decouple" the local power supply from the rest of the circuit, ensuring that rapid changes in current demanded by one component do not affect the supply voltage of other components.

![image](https://github.com/user-attachments/assets/e150ba66-28c5-4678-abab-5ddfd327764a)

**Run Floorplan**
```bash
run_floorplan
```
![Screenshot from 2024-08-30 21-50-33](https://github.com/user-attachments/assets/7c5ccc63-ce5e-4c6c-8729-c41e0280defb)

![Screenshot from 2024-08-30 21-50-41](https://github.com/user-attachments/assets/33a247a0-b421-4293-ab8f-07d3bc7e0b3d)

**Picorv32a floorplan def file:**


![Screenshot from 2024-08-30 22-01-51](https://github.com/user-attachments/assets/00e525ea-022c-497c-99cc-4b920998e174)

![Screenshot from 2024-08-30 21-59-56](https://github.com/user-attachments/assets/961be1a5-bba4-4001-b640-c6759e889625)

According to floorplan def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

#### Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![image](https://github.com/user-attachments/assets/eda02393-4a7b-4d77-96f1-f50bda007f74)

Equidistant placement of ports

![image](https://github.com/user-attachments/assets/c5ad6d2d-1d1d-4637-a279-b66c4788823c)

Port layer as set through config.tcl

![image](https://github.com/user-attachments/assets/82c6539a-e8df-4cc9-bfa1-cacb187e037b)
![image](https://github.com/user-attachments/assets/cc2fec69-cfed-42dd-b604-80aa311e460d)

Decap Cells and Tap Cells

![image](https://github.com/user-attachments/assets/8e557f9f-4fe6-4b90-bade-f976d22f6f9e)

Diogonally equidistant Tap cells

![image](https://github.com/user-attachments/assets/baee36ed-c151-45e6-a19e-fe9abe128b52)

Unplaced standard cells at the origin

![image](https://github.com/user-attachments/assets/ccca9bbf-4d49-4ef6-ab27-a49f3b64ccbb)

#### Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run

![image](https://github.com/user-attachments/assets/d6371139-e405-4f6b-bf50-8a4b290d1e1d)
![image](https://github.com/user-attachments/assets/2587dd6d-f728-481a-b028-39711526e8fb)

#### Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![image](https://github.com/user-attachments/assets/d3d93873-21de-4067-8c18-b63098296f61)

Standard cells legally placed 

![image](https://github.com/user-attachments/assets/41aa67c0-7571-4dcf-86a5-5e68ed096464)

Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
## Placement in VLSI Design
Placement plays a crucial role in VLSI (Very Large Scale Integration) design. It involves determining the physical locations of standard cells or logic elements within a chip or block. Let's break it down:

- **Global Placement:**

  - Global placement assigns general locations to movable objects (cells).
  - Some overlaps between placed objects are allowed during this stage. 
  - The goal is to achieve a rough layout that satisfies area constraints.

- **Detailed Placement:**

  - Detailed placement refines the object locations obtained from global placement.
  - It enforces non-overlapping constraints and ensures that cells are placed on legal cell sites.
  - The quality of detailed placement significantly impacts subsequent routing stages.

``` magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def & ```

![Screenshot from 2024-08-26 18-04-15](https://github.com/user-attachments/assets/ee8746ed-8c5d-429f-a0b1-4f8b11288677)
![Screenshot from 2024-08-26 18-11-42](https://github.com/user-attachments/assets/e8130182-c3f6-4658-8d2b-ad2b5f3b6385)
![Screenshot from 2024-08-26 18-12-21](https://github.com/user-attachments/assets/590e4e06-9d00-45be-bbe2-59636d6a5f11)
![Screenshot from 2024-08-26 18-16-10](https://github.com/user-attachments/assets/8051e35b-b5b8-43b1-9d4d-7a1f1dc5838a)

## CELL DESIGN AND CHARACETRIZATION FLOWS

Library is a place where we get information about every cell. It has differents cells with different size, functionality,threshold voltages. There is a typical cell design flow steps.

    Inputs : PDKS(process design kit) : DRC & LVS, SPICE Models, library & user-defined specs.
    Design Steps :Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power).
    Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files
## Standard Cell Characterization Flow
A typical standard cell characterization flow that is followed in the industry includes the following steps:

- Read in the models and tech files
- Read extracted spice Netlist
- Recognise behavior of the cells
- Read the subcircuits
- Attach power sources
- Apply stimulus to characterization setup
- Provide neccesary output capacitance loads
- Provide neccesary simulation commands
Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing 
 characterization, power characterization and noise characterization.





| Timing defintion   | value| 
|----------|----------|
| `slew_low_rise_thr`|`20% value`|
|`slew_high_rise_thr`| `80% value`   |
| `slew_low_fall_thr`| `20% value`   |
|`slew_high_fall_thr`| `80% value`   |
| `in_rise_thr`      | `50% value`   |
| `in_fall_thr`      | `50% value`   |
| `out_rise_thr`     | `50% value`   |
| `out_fall_thr`     | `50% value`   |



## Propagation Delay and Transition Time
**Propagation delay** is the time it takes for a signal to travel from the input to the output of a circuit. It's typically measured as the time difference between when the input signal reaches 50% of its final value and when the output signal reaches 50% of its final value.

If the threshold values used to measure this delay are not chosen carefully, it can result in negative delay values, which are not physically meaningful. However, even with well-chosen thresholds, the delay might still appear positive or negative due to variations in the slew rate, which is how quickly the signal transitions from one value to another.

![image](https://github.com/user-attachments/assets/f1c39625-2bf0-4afd-b492-5dd4608aa694)


``` bash
Propagation delay = time(out_thr) - time(in_thr)
```

**Transition Time**
Transition time is the time it takes for a signal to move between its low and high states (or vice versa). It’s typically measured between the points where the signal reaches 10% and 90% of its final value, or sometimes between 20% and 80%. This metric is crucial for understanding the speed of signal changes in a circuit.


``` bash
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```



