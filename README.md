![image](https://github.com/user-attachments/assets/d6a9f776-218e-44f1-8144-c63680169cf2)


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
- [Section - 2 Good Floorplan vs bad Floorplan and Introduction to library cells](#Section---2-Good-Floorplan-vs-bad-Floorplan-and-Introduction-to-library-cells)
- [Section - 3 Design Library Cell using magic layout and ngspice charcterization](#Section---3-Design-Library-Cell-using-magic-layout-and-ngspice-charcterization)
- [Section - 4 Pre Lay-out Timing Analysis and Importance of Good clock Tree](#Section---4-Pre-Lay-out-Timing-Analysis-and-Importance-of-Good-clock-Tree)


## Overview Of QFN-48 Chip (PicoRV32 - A Size-Optimized RISC-V CPU)
VSD Squadron Board: The VSD Board is shown below. Our focus is on the enclosed region containing the "Microprocessor (PicoRV32A-Cpu)," which will be designed using the RTL to GDS flow, progressing from the abstract design level to the fabrication stage.

![image](https://github.com/user-attachments/assets/844ea7fc-f4f4-4353-ae5d-d75a4507d075)

# Section - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK

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

# Section - 3 Design Library Cell using magic layout and ngspice charcterization

## VTC Spice Simulation

Voltage Transfer Characteristic (VTC) is a key concept in electronic circuit analysis and design, especially in analog and mixed-signal integrated circuits. A VTC SPICE simulation refers to the process of using a SPICE (Simulation Program with Integrated Circuit Emphasis) simulator to analyze and plot the VTC curve of a circuit.

In a VTC simulation, the input voltage of the circuit is gradually swept over a specified range, while the corresponding output voltage is measured. The plot of output voltage versus input voltage generated from this data provides valuable insights into the circuit’s linearity, gain, and various operating regions. This is especially useful for evaluating the performance of amplifiers, logic gates, and signal processing circuits.

Here’s a step-by-step guide for performing a VTC SPICE simulation:

- Define the circuit in SPICE using its netlist.
- Set up a DC sweep simulation for the input voltage.
- Specify the range over which the input voltage will vary.
- Run the simulation to record the output voltage for each input value.
- Plot the VTC curve (output voltage vs. input voltage).
- Analyze the plot to evaluate the circuit’s behavior, including linearity, gain, and transition regions.

**Circuit Design:** The circuit to be analyzed is designed in a schematic capture tool that is compatible with the SPICE simulator being used.

**Simulation Setup:** The SPICE simulation is set up with the appropriate analysis type, which could be a DC sweep analysis if the goal is to plot the VTC over a range of DC input voltages.

**nput Signal Sweep:** The input voltage source is configured to sweep across the desired range of voltages. This could be from the negative supply rail to the positive supply rail, or any other relevant range.

**Output Measurement:**  The output node of the circuit is specified, and the simulator is instructed to record the voltage at this node for each input voltage step.

**Simulation Run:** The simulation is run, and the SPICE engine calculates the circuit's response to each input voltage.

**Data Analysis:**  The resulting data is plotted as a graph with input voltage on the x-axis and output voltage on the y-axis. This plot is the VTC of the circuit.

**Interpretation:**  The VTC is analyzed to determine the circuit's performance characteristics, such as gain, input offset voltage, output swing, and the presence of any non-linearities or distortion.

![Screenshot 2024-08-31 21 19 24](https://github.com/user-attachments/assets/8687e29c-48c4-4f27-adda-fcf81aa23011)



### Concept on Switching Threshold

The concept of the switching threshold is crucial in the context of digital circuits, particularly in logic gates and transistors. The switching threshold refers to the input voltage level at which the circuit transitions from one state to another, typically from a low (logic 0) to a high (logic 1) state or vice versa.

In a digital circuit, the switching threshold is designed to be at a specific voltage level to ensure reliable operation and to minimize the effects of noise and signal degradation. The exact value of the switching threshold can vary depending on the technology and design of the circuit, but it is typically set near the midpoint of the supply voltage range.

For example, in a CMOS (Complementary Metal-Oxide-Semiconductor) inverter, which is a fundamental building block of digital logic, the switching threshold is ideally at the midpoint of the supply voltage (Vdd/2). This ensures that the inverter has equal noise margins for both high and low input levels.

The switching threshold is influenced by several factors, including:

   **1. Device Characteristics:** The physical properties of the transistors used in the circuit, such as their threshold voltage (Vth), affect the overall switching threshold of the circuit.

  **2. Supply Voltage:**  The operating voltage of the circuit can impact the switching threshold. As the supply voltage changes, the switching threshold may also shift.

   **3. Temperature:** Temperature variations can cause changes in the electrical characteristics of the transistors, which can in turn affect the switching threshold.

   **4. Process Variations:** Manufacturing variations can lead to differences in the electrical properties of transistors, which can influence the switching threshold of the circuit.

   **5. Load Capacitance:** The capacitive load connected to the output of the circuit can affect the switching threshold due to the dynamic behavior of the circuit during switching.

![image](https://github.com/user-attachments/assets/31e3b8d8-189c-40b3-bfdb-f3ec66e79c79)





## Static And Dynamic Simulation of CMOS Inverter

### Static Simulation
Static simulation, also known as DC analysis, involves analyzing the circuit's behavior at DC (Direct Current) steady-state conditions. This type of simulation is used to determine the following characteristics:

   Voltage Transfer Characteristic (VTC): The VTC plot shows the relationship between the input voltage (Vin) and the output voltage (Vout) of the inverter. It helps in understanding the transition between the logic levels (0 and 1) and the noise margins.

**Switching Threshold (V_th):** The input voltage at which the inverter switches from one logic state to another. Ideally, for a symmetrical VTC, the switching threshold is at Vdd/2, where Vdd is the supply voltage.

**Noise Margins:** The maximum noise voltage that can be tolerated at the input while the output remains in the correct logic state.

**Static Power Dissipation:** The power consumed by the inverter when it is in a stable state (not switching). This is typically very low in CMOS circuits due to the low leakage currents.

### Dynamic Simulation
Dynamic simulation, also known as transient analysis, involves analyzing the circuit's behavior over time as it responds to time-varying input signals. This type of simulation is used to determine the following characteristics:

**Propagation Delay (t_p):** The time it takes for the output to change in response to a change in the input. It is typically measured as the time from the 50% point of the input transition to the 50% point of the output transition.

**Rise Time (t_r) and Fall Time (t_f):** The times it takes for the output to transition from 10% to 90% (rise time) and from 90% to 10% (fall time) of the output voltage swing.

**Power Consumption:** The dynamic power consumed by the inverter during switching, which is a function of the switching frequency, load capacitance, and supply voltage.

**Transient Response:**  The overall response of the inverter to input signals, including overshoot, undershoot, and ringing.
    
![maxresdefaul4t](https://github.com/user-attachments/assets/5a72fdab-6149-4377-a1b9-b53323efa131)



## Layout and CMOS Fabrication Process

The CMOS (Complementary Metal-Oxide-Semiconductor) fabrication process is a complex series of steps used to manufacture integrated circuits. CMOS technology is widely used in modern electronics due to its low power consumption and high-density integration capabilities. The fabrication process involves several key steps, including substrate preparation, doping, oxidation, photolithography, etching, and metallization. Here is an overview of the CMOS fabrication process:
**1. Substrate Preparation**

Starting Material: The process begins with a pure silicon wafer, which serves as the substrate.
Cleaning: The wafer is thoroughly cleaned to remove any impurities.

**2. Oxidation**

Thermal Oxidation: A layer of silicon dioxide (SiO2) is grown on the surface of the silicon wafer. This oxide layer acts as an insulator and also serves as a mask for subsequent doping processes.

**3. Photolithography**

Photoresist Application: A light-sensitive polymer called photoresist is applied to the wafer surface.
Exposure and Development: The wafer is exposed to ultraviolet (UV) light through a mask that contains the desired pattern. The exposed photoresist is then developed, leaving a patterned layer on the wafer.

**4. Etching**

Wet or Dry Etching: The areas of the wafer not protected by the photoresist are etched away using either wet chemicals or dry plasma etching.

**5. Doping**

Ion Implantation: Selected areas of the wafer are doped with impurities (dopants) such as boron or phosphorus to create n-type or p-type regions. This process alters the electrical properties of the silicon, creating the necessary semiconductor characteristics.

**6. Isolation**

Local Oxidation of Silicon (LOCOS): An oxide layer is grown in selected areas to isolate different components of the circuit.
Trench Isolation: Alternatively, trenches can be etched and filled with oxide to achieve isolation.

**7. Transistor Formation**

**- Gate Oxide Growth:**  A thin layer of oxide is grown on the surface where the transistors will be formed.

**- Polysilicon Deposition:** A layer of polysilicon is deposited and patterned to form the gates of the transistors.

**- Spacer Formation:** Spacers are created on the sides of the gate to control the subsequent doping process.

**- Source/Drain Implantation:** Dopants are implanted to form the source and drain regions of the transistors.

**8. Silicidation**

Silicide Formation: A silicide layer (e.g., tungsten silicide) is formed on top of the polysilicon gate and the source/drain regions to reduce resistance.

**9. Interlayer Dielectric and Planarization**

Dielectric Deposition: A layer of insulating material (e.g., silicon dioxide) is deposited over the entire wafer.
Chemical Mechanical Polishing (CMP): The wafer surface is planarized using CMP to ensure a smooth surface for subsequent metallization.

**10. Metallization**

Metal Layers: Multiple layers of metal (e.g., aluminum or copper) are deposited and patterned to form the interconnections between transistors and other components.
Contact and Via Formation: Contacts and vias are etched and filled with metal to connect the transistors to the metal layers.

**11. Passivation**

Passivation Layer: A final layer of insulating material is deposited to protect the circuit from external contaminants.

**12. Testing and Packaging**

Wafer Probing: The wafer is tested to identify any defective chips.

Dicing: The wafer is cut into individual chips (dice).

Packaging: The chips are mounted in packages, which are then sealed to protect the delicate circuitry.

Final Testing: The packaged chips are tested again to ensure they meet the required specifications.

![Assigning-the-names-of-NMOS-and-PMOS](https://github.com/user-attachments/assets/936498d3-3e2e-41d5-8ee4-da89fd7c5e7c)

![Screenshot 2024-08-31 16 28 33](https://github.com/user-attachments/assets/d2f5f5f2-9e34-4a67-8bc8-ec337f192e6c)

### LABs Exercise

#### 1. IoPlacer revision & adding the "set::env" command

![Screenshot from 2024-10-16 11-59-40](https://github.com/user-attachments/assets/ba67673b-4e31-4fff-8638-7626cb58e62a)


here the the i/o pins are congested and overlapped:
![Screenshot from 2024-10-16 12-01-52](https://github.com/user-attachments/assets/8eec0525-0623-4416-99c8-7dcd5c6e135a)

#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

![image](https://github.com/user-attachments/assets/ea9177d6-88a4-448e-9f96-d85f5ab68509)

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![image](https://github.com/user-attachments/assets/14dcf0cf-ee07-4427-bd26-68a82ceabe35)

NMOS and PMOS identified

![image](https://github.com/user-attachments/assets/7f704bce-91b4-4778-a3ba-5420012cdb2c)
![image](https://github.com/user-attachments/assets/b4060577-28f0-474e-9c7d-1d45ee08310d)

Output Y connectivity to PMOS and NMOS drain verified

![image](https://github.com/user-attachments/assets/3c3f9045-7df6-433f-830f-17bf5f281c29)

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```
![image](https://github.com/user-attachments/assets/664e5b09-9de2-45cd-9c47-59719359fb68)

#### 4. spice tecgnology file, simulation and output graph


![image](https://github.com/user-attachments/assets/8cde7b4e-a65d-469b-a0ca-32aa56679a7c)


![image](https://github.com/user-attachments/assets/e83aca2a-edd4-4611-a076-da3400d4a5dd)


updated tech file:

![image](https://github.com/user-attachments/assets/88803377-45ca-47af-a96b-dd7d91d106c0)

#### 5. Parameters Calculation
  ##### 5.a) rise transition of of output: 20% of VDD to 80% of VDD
```math
rise\_transition = (80 % of 3.3v) - (20 % of 3.3v)
                = (x-coordinate\ pos. of\ 2.64v - x-coordinate\ pos. of\ 0.66v)
                = (6.24247*e-19) - (6.61804*e-19)
                = 0.06163\ ns
```

  ##### 5.b) fall transition of of output: 80% of VDD to 20% of VDD
```math
fall\_transition = (20 % of 3.3v) - (80% of 3.3v)
                = (x-coordinate\ pos. of\ 0.66v - x-coordinate\ pos. of\ 2.64v)
                = (8.09409*e-19) - (8.05156*e-19)
                = 0.04253\ ns
```
                
 ![image](https://github.com/user-attachments/assets/2d02dfa8-f8e6-4098-995b-71cf2decd805)

                
  ##### 5.c) cell rise delay: (50% of o/p rise) - (50% of i/p fall)
```math
 cell\_rise\_delay= (x-coordinate\ pos. of\ 1.65v - x-coordinate\ pos. of\ 1.65v)
                = (6.2089*e-19) - (6.15*e-19)
                = 0.0589\ ns
```
                
 ![image](https://github.com/user-attachments/assets/85d0076f-04ce-46a8-ad8e-1d8b4c4ba54b)

  ##### 5.d) cell fall delay: (50% of o/p fall) - (50% of i/p rise)
```math
 cell\_fall\_delay= (x-coordinate\ pos. of\ 1.65v - x-coordinate\ pos. of\ 1.65v)
                = (8.07657*e-19) - (8.05*e-19)
                = 0.02657\ ns
```
                
 ![image](https://github.com/user-attachments/assets/4e986827-07df-4ec4-8c1b-2dfdc67ac4cb)

#### 6. Introduction to Magic tool and DRC

![image](https://github.com/user-attachments/assets/13b20e46-d226-479b-abc2-b7fe6069cc75)

![image](https://github.com/user-attachments/assets/e06146c8-ef52-463e-a1b1-0b3b5e3fa549)

![image](https://github.com/user-attachments/assets/04462b54-555f-49df-a06a-8af57fb3e3eb)

![image](https://github.com/user-attachments/assets/682edf39-66b4-4b88-979c-c6773de13974)

![image](https://github.com/user-attachments/assets/fd4101a9-ae66-40a4-8bd7-fd9652321248)

![image](https://github.com/user-attachments/assets/db8b8714-ea87-4595-9a06-d01ca4ae3e0e)

![image](https://github.com/user-attachments/assets/5b07a766-cc21-4e37-9aac-cf8e018fffb3)

# Section - 4 Pre Lay-out Timing Analysis and Importance of Good clock Tree
