
原文：https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop

# Advance Physical Design using OpenLANE and SkyWater 130nm PDK

[![vsd](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/vsd.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/vsd.png)

Table of Contents

- [1. Course Content](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#course-content)
- [2. Day 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#day-1)
    - [2.1. Introduction to IC Design components and terminologies](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#introduction-to-ic-design-components-and-terminologies)
    - [2.2. Software application to hardware execution](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#software-application-to-hardware-execution)
    - [2.3. RTL2GDS OpenLANE ASIC Flow](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#rtl2gds-openlane-asic-flow)
    - [2.4. Open source EDA tools familiarisation](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#open-source-eda-tools-familiarisation)
- [3. Day 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#day-2)
    - [3.1. Chip floorplanning](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#chip-floorplanning)
    - [3.2. Placement](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#placement)
    - [3.3. Standard cell design](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#standard-cell-design)
    - [3.4. Standard cell characterization](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#standard-cell-characterization)
- [4. Day 3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#day-3)
    - [4.1. Design and characterize library cell CMOS inverter](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#design-and-characterize-library-cell-cmos-inverter)
- [5. Day 4](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#day-4)
    - [5.1. Introduction and generation of LEF files using magic tool](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#introduction-and-generation-of-lef-files-using-magic-tool)
    - [5.2. Custom cells in openLANE](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#custom-cells-in-openlane)
    - [5.3. CTS](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#cts)
- [6. Day 5](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#day-5)
    - [6.1. Power distribution network](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#power-distribution-network)
    - [6.2. Routing](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#routing)
    - [6.3. SPEF Extraction](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#spef-extraction)
    - [6.4. GDSII](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#gdsii)
- [7. Acknowledgement](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#acknowledgement)

This repository aims to capture the works done in 5-day workshop of Adavance Physical Design using OpenLANE/SkyWater130. The workshop helps to familiarise with the efabless OpenLANE VLSI design flow RTL2GDS and the Skywater 130nm PDK. OpenLANE is an open source VLSI flow built around open source tools with the goal to produce clean GDSII with no human intervention("no human in the loop"). PicoRV32 is a CPU core that implements the RISC-V RV32IMC Instruction Set which is used as an example in this course.

## [1. Course Content](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#1-course-content)

- **Day 1**
    
    - Introduction to IC Design components and terminologies
        
    - Software application to hardware execution
        
    - RTL2GDS OpenLANE ASIC Flow
        
    - Open source EDA tools familiarisation
        
    
- **Day 2**
    
    - Chip floorplanning
        
    - Placement
        
    - Standard cell design
        
    - Standard cell characterization
        
    
- **Day 3**
    
    - 16 Mask CMOS fabrication process
        
    - Design and characterize library cell CMOS inverter
        
    
- **Day 4**
    
    - Introduction and generation of LEF files using magic tool
        
    - Custom cells in openLANE
        
    - Fixing slack violations
        
    - CTS
        
    
- **Day 5**
    
    - Power distribution network
        
    - Routing
        
    - SPEF extraction
        
    - GDSII
        
    

## [2. Day 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#2-day-1)

### [2.1. Introduction to IC Design components and terminologies](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#21-introduction-to-ic-design-components-and-terminologies)

**Core**

A core is an area in the chip where the fundamental logic of the design is placed. It encapsulates all the combinational circuit, soft and hard IPs, and nets.

**Die**

Die is an area of chip that encapsulates the core and IO pads. Die is imprinted multiple times along the silicon area or wafer to increase the throughput.

**IO Pads**

IO pads are the pins that act as the source of communication between core and the outside world. Pad cells surround the rectangular metal patches where external bonds are made. input,output and power pad.
![ic components](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/ic_components.png)


**IPs**

Foundary IPs are manually designed or need some human interference (or intelligence) essentially to define and create them like SRAM, ADC, DAC, PLLs.

**PDKs**

PDKs are interface between foundary and design engineers. PDKs contains set of files to model fabrication process for the design tools used to design IC like device models, DRC, LVS, Physical extraction, layers, LEF, standard cell libraries, timing libraries etc. SkyWater 130nm is the PDK used in this workshop specifically sky130_fd_sc_hd and openLANE is built around this PDK.

### [2.2. Software application to hardware execution](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#22-software-application-to-hardware-execution)

Applications and softwares running on like PCs and laptops are implemented in languages like C, C++, Python, Java, .NET etc.These applications needs to be converted to bitstream using the compiler and assembler which is understandable the core. Compilers are used for this purpose which generates bitstream based on Instruction set architecture of the native processor. The core is implemented using HDL.

### [2.3. RTL2GDS OpenLANE ASIC Flow](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#23-rtl2gds-openlane-asic-flow)

OpenLANE is an automated RTL to GDSII flow. It is based on several open source components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, CVC, SPEF-Extractor, CU-GR, Klayout and custom methodology scripts for design exploration and optimization.

[![design flow](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/design_flow.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/design_flow.png)

OpenLANE is run as an container inside docker. For OpenLANE setup refer : [**OpenLANE**](https://github.com/The-OpenROAD-Project/OpenLane)

OpenLANE integrated several key open source tools over the execution stages:

|   |   |
|---|---|
|**RTL Synthesis, Technology Mapping, and Formal Verification**|yosys + abc|
|**Static Timing Analysis**|OpenSTA|
|**Floor Planning**|init_fp, ioPlacer, pdn and tapcell Placement: RePLace (Global), Resizer and OpenPhySyn (Optimizations), and OpenDP (Detailed)|
|**Clock Tree Synthesis**|TritonCTS|
|**Fill Insertion**|OpenDP/filler_placement|
|**Routing**|FastRoute or CU-GR (Global) and TritonRoute (Detailed)|
|**SPEF Extraction**|SPEF-Extractor|
|**GDSII Streaming out**|Magic and Klayout|
|**DRC Checks**|Magic and Klayout|
|**LVS checks**|Netgen|
|**Antenna Checks**|Magic|
|**Circuit Validity Checker**|CVC|

The main commands used in openLANE design flow in interactive mode are:

```
prep -design <design> -tag <tag> -config <config> -init_design_config -overwrite similar to the command line arguments, design is required and the rest is optional
run_synthesis
run_floorplan
run_placement
run_cts
run_routing
write_powered_verilog followed by set_netlist $::env(lvs_result_file_tag).powered.v
run_magic
run_magic_spice_export
run_magic_drc
run_lvs
run_antenna_check
```

### [2.4. Open source EDA tools familiarisation](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#24-open-source-eda-tools-familiarisation)

Command to run openlane, needs to executed from directory where openlane is installed:

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane$ docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21
bash-4.2$
```

To run in interactive mode (step by step mode)

```
bash-4.2$ ./flow.tcl -interactive
```

[![interactive mode](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/interactive_mode.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/interactive_mode.png)

**Package import and check**

To import and check whether required openLANE package is installed

```
% package require openlane
```

[![package openlane](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/package_openlane.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/package_openlane.png)

**Prepare design**

To prepare and setup the design

```
% prep -design picorv32a
```

[![prep design](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/prep_design.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/prep_design.png)

Preparation step basically sets up the directory structure, merges the technology LEF (.tlef) and cell LEF(.lef) into one. Tech LEF contains the layer informations and cell LEF contains the cell informations. All the designs are placed under the designs directory for openLANE flow. Directory structure of picrorv32a before and after executing prep command.

[![picorv32a directory](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/picorv32a_directory.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/picorv32a_directory.png)

[![prep design directory structure](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/prep_design_directory_structure.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/prep_design_directory_structure.png)

|   |   |
|---|---|
|**src**|contains verilog files and constraints file|
|**config.tcl**|contains the configurations used by openLANE|

There are three configuration files:

- Each phase used in the process flow has a configuration tcl file under openlane_working_dir/openlane/configuration/<phase_name>.tcl
    
- Each design will have its own config.tcl file
    
- Each design will have its own pdk specific tcl file, sky130A_sky130_fd_sc_hd_config.tcl which has the highest precedence.
    

[![design config](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/design_config.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/design_config.png)

OpenLANE tools configuration files:

[![openLANE config](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/openLANE_config.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/openLANE_config.png)

**Synthesis design**

To synthesize the design

```
% run_synthesis
```

|   |   |
|---|---|
|**yosys**|Performs RTL synthesis|
|**abc**|Performs technology mapping|
|**OpenSTA**|Performs static timing analysis on the resulting netlist to generate timing reports|

[![syn design1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/syn_design1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/syn_design1.png)

[![syn design2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/syn_design2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/syn_design2.png)

Synthesis logs and report will be captured under runs directory.

[![syn design3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/syn_design3.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/syn_design3.png)

---

All the configuration parameters related to synthesis phase are available in

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/configuration/synthesis.tcl
```

---

## [3. Day 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#3-day-2)

### [3.1. Chip floorplanning](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#31-chip-floorplanning)

In floorplanning phase deals with setting die area, core area, core utilization factor, aspect ratio, placing of macros, power distribution networks and placement of IO pins.

**Aspect Ratio**

Specifies the shape of the chip, given by ratio of height to width of the core area. Aspect ratio of 1 indicates square shape else rectangle.

**Utilization Factor**

Specifies the amount of area taken by the netlist, given by ratio of area of netlist to area of the core. For placement optimization and realizable routing utilization factor is kept to 0.5 to 0.7 range.

**Preplaced cells**

Preplaced cells have fixed location on the chip and cannot be moved around in placement phase. The placement of these macros are considered while deciding the placement of standard cells by floor planning tools.Macros can be used several times in a design. Typical examples of macros are memory blocks, clock gating cells, comparators etc.

**Decoupling capacitors**

Decaps are used with preplaced cells to compensate the voltage drop along the long wires and nets which affects the noise margin. Decaps are charged to the supply voltage and used as the supply source for the logic level transitions LOW to HIGH. It decouples the circuit from main supply.

**Power planning**

Power planning means to provide power to the every macros, standard cells, and all other cells are present in the design.Power planning is a step which typically is done with floor planning in which power grid network is created to distribute power to each part of the design equally to mitigate voltage droop and ground bounce issues. In openLANE flow, PDN is done before routing phase.

**Pin placement**

Pins placement also done in floor planning phase and logical cell placement blockage is added to prevent PnR tools from adding cells in this region.

**Floor planning**

To run floorplanning phase

```
% run_floorplan
```

[![floor plan 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_1.png)

[![floor plan 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_2.png)

Floor planning phase generate DEF file which contains core area and placement details of standard cells.

|   |   |
|---|---|
|**init_fp**|Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)|
|**ioplacer**|Places the macro input and output ports|
|**pdn**|Generates the power distribution network|
|**tapcell**|Inserts welltap and decap cells in the floorplan|

[![floor plan 4](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_4.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_4.png)

[![floor plan 3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_3.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_3.png)

DEF file generated by floorplan phase can be utilized by magic tool to get the floorplan view which requires 3 configuration files:

- Magic technology file (sky130A.tech)
    
- DEF file from floorplan phase
    
- Merged LEF file from preparation phase
    

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/30-06_16-01/results/floorplan$ magic -T $PDK/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

[![floor plan 5](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_5.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_5.png)

[![floor plan 6](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_6.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_6.png)

[![floor plan 7](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/floor_plan_7.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/floor_plan_7.png)

---

All the configuration parameters related to floorplanning phase are available in

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/configuration/floorplan.tcl
```

---

### [3.2. Placement](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#32-placement)

Placement determine the locations of standard cells or logic elements within each block.Some circuit elements may have fixed locations while others are movable.

**Global placement**

Global placement assigns general locations to movable objects. Some overlaps are allowed between placed objects.

**Detailed placement**

Detailed placement refines object locations to legal cell sites and enforces non-overlapping constraints. Detailed placement determines the achievable quality of the subsequent routing stages.

|   |   |
|---|---|
|**RePLace**|Performs global placement|
|**Resizer**|Performs optional optimizations on the design|
|**OpenPhySyn**|Performs timing optimizations on the design|
|**OpenDP**|Performs detailed placement to legalize the globally placed components|

To run placement phase

```
% run_placement
```

[![placement 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/placement_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/placement_1.png)

DEF file generated by placement phase can be utilized by magic tool to get the placement view which requires 3 configuration files:

- Magic technology file (sky130A.tech)
    
- DEF file from placement phase
    
- Merged LEF file from preparation phase
    

[![placement 3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/placement_3.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/placement_3.png)

[![placement 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/placement_2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/placement_2.png)

---

All the configuration parameters related to placement phase are available in

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/configuration/placement.tcl
```

### [3.3. Standard cell design](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#33-standard-cell-design)

Standard cell design flow consists of 3 stages

|   |   |
|---|---|
|**Inputs**|PDKs, DRC and LVS rules, SPICE models, library & user-defined specs.|
|**Design Steps**|Involves circuit design, layout design, characterization using GUNA tool. Characterization involves timing, power and noise characterizations.|
|**Outputs**|CDL (Circuit Description Language), GDSII, LEF(Library Exchange Format), Spice extracted netlist, timing, noise, power libs.|

### [3.4. Standard cell characterization](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#34-standard-cell-characterization)

Standard cell characterization refers to gathering data about the behaviour of standard cells. To build the circuit knowledge of logic function of cell alone is not sufficient. Standard cell library has cells with different drive strength and functionalities.These cells are characterized by using tool like GUNA from [`Paripath`](https://www.paripath.com/home).

The standard cell characterization flow involves

- Read the model files
    
- Read the extracted spice netlist
    
- Recognize function or behaviour of the cell
    
- Apply stimulus and characterization setup
    
- Vary the output load capacitance and observe the different characterization behaviours
    
- Provide necessary simulation commands
    

Apply the entire flow to GUNA tool to generate timing, noise and power models.

## [4. Day 3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#4-day-3)

Build basic CMOS inverter netlist spice deck file using ngspice and perform dc and transient analysis. Understanding basic terminologies of CMOS inverter like static and dynamic characteristics.

|   |   |
|---|---|
|**Static characteristics**|`Switching threshold, Vil, Vol, Vil, Voh and noise margins`.|
|**Dyanamic characteristics**|`Propagation delays, rise time and fall time`.|

**Simulation steps on ngspice**

- Source the spice deck file by `source *.cir`
    
- Run the file by `run`
    
- View the available plots mentioned in spice deck file by `setplot` and select desired plot by entering in the window
    
- See the nodes available for plotting by `dispplay`
    
- Obtain output waveform by `plot out vs in` for VTC or `plot out vs time`, out and in are considered as the nodes.
    

### [4.1. Design and characterize library cell CMOS inverter](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#41-design-and-characterize-library-cell-cmos-inverter)

Magic layout view to cmos inverter

To get the cell files refer [`standard cell characterization`](https://github.com/nickson-jose/vsdstdcelldesign)

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ magic -T $PDK_ROOT/sky130A/libs.tech/magic/sky130A.tech sky130_inv.mag &
```

[![cmos inverter magic layout view](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/cmos_inverter_magic_layout_view.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/cmos_inverter_magic_layout_view.png)

[![cmos inverter magic layout view 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/cmos_inverter_magic_layout_view_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/cmos_inverter_magic_layout_view_1.png)

To extract the parasitics and characterize the cell design use below commands in tkcon window.

```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

[![spice extraction](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/spice_extraction.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/spice_extraction.png)

[![spice extraction 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/spice_extraction_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/spice_extraction_1.png)

Extracted spice deck file from the layout

[![spice deck](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/spice_deck.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/spice_deck.png)

Few modifications needs to be done in spice deck file

- Scale needs to be aligned with the layout grid size and check the model name from pshort.lib and nshort.lib
    
- Specify power supply
    
- Apply stimulus
    
- Perform transient analysis
    

[![magic tool grid size](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/magic_tool_grid_size.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/magic_tool_grid_size.png)

[![modifiled spice deck](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/modifiled_spice_deck.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/modifiled_spice_deck.png)

To run the simulation in ngspice, invoke the ngspice tool with the modified extracted spice file as input

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ ngspice sky130_inv.spice
```

[![ngspice output](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/ngspice_output.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/ngspice_output.png)

To plot transient analysis output, where y - output node and a - input node

```
plot y vs time a
```

[![ngspice transient output](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/ngspice_transient_output.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/ngspice_transient_output.png)

## [5. Day 4](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#5-day-4)

### [5.1. Introduction and generation of LEF files using magic tool](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#51-introduction-and-generation-of-lef-files-using-magic-tool)

The entire layout information of the block(macro or standard cell) is not required for the PnR tool to place and route.It requires the PR boundary(bounding box) and pin positions.These minimal and abstract information of the block is provided to PnR tool by the LEF(Library Exchange Format) file. LEF exposes only the necessary things need for the PnR tool and protecting the logic or intellectual property.

|   |   |
|---|---|
|**Cell LEF**|Abstract view of the cell which holds information about PR boundary, pin positions and metal layer information.|
|**Technology LEF**|Holds information about the metal layers, via, DRC technology used by placer and router.|

Below image gives idea regarding difference between layout and LEF.

[![layout vs abstract](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/layout_vs_abstract.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/layout_vs_abstract.png)

Tracks are used in routing stages. Routes are metal traces which can go over the tracks. The information of horizontal and vertical tracks present in each layer is given in `tracks.info` file.

[![tracks info](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/tracks_info.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/tracks_info.png)

Horizontal track in li1 layer has an offset of 0.23um and pitch of 0.46um. Vertical track in li1 layer has an offset of 0.17um and pitch of 0.34um.

**Pin placement**

To ensure the standard cell layout is done as per the requirement of PnR tool

- ports must lie on the intersection of horizontal and vertical tracks. Ensure that in magic tool by aligning grid dimension with the track file.
    
- cell width must be odd multiples of x pitch. Ensure that by counting the number of grid boxes along cell width.
    
- cell height must be odd multiples of y pitch. Ensure that by counting the number of grid boxes along cell height.
    

[![grid allign track](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/grid_allign_track.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/grid_allign_track.png)

The ports lie on the intersection of horizontal and vertical tracks ensure that route can reach the port from x as wells y direction. Ports are in li1(locali)layer.

[![grid allign track1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/grid_allign_track1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/grid_allign_track1.png)

When extracting LEF file, these ports are what are defined as pins of the macro. These are done in magic tool by adding text with enabling port.

[![lef port](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/lef_port.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/lef_port.png)

A and Y is attached to locali layer and Vdd and Gnd attached to metal1 layer. To set port class and port attribute refer [`standard cell characterization`](https://github.com/nickson-jose/vsdstdcelldesign)

To extact LEF file

```
lef write
```

[![lef extract](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/lef_extract.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/lef_extract.png)

[![lef extract1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/lef_extract1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/lef_extract1.png)

[![lef extract2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/lef_extract2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/lef_extract2.png)

[![lef extract3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/lef_extract3.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/lef_extract3.png)

### [5.2. Custom cells in openLANE](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#52-custom-cells-in-openlane)

To include the custom inverter cell into the openLANE flow

- Copy the extracted LEF file from layout into `designs\picorv32a\src` directory along with `sky130_fd_sc_hd_slow/fast/typical.lib` from the reference repository.
    

[![openlan flow custom cell](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/openlan_flow_custom_cell.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/openlan_flow_custom_cell.png)

Custom cell inverter characterization information is included in above mentioned libs.

[![lib with custom cell characterization](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/lib_with_custom_cell_characterization.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/lib_with_custom_cell_characterization.png)

- modify `design\picorv32a\config.tcl`
    

[![modified config tcl](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/modified_config_tcl.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/modified_config_tcl.png)

Now perform openLANE design flow

```
% package require openLANE 0.9
% prep -design picorv32a -tag 03-07_16-04 -overwrite
% set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
% add_lefs -src $lefs
% run_synthesis
% run_floorplan
% run_placement
```

[![prep deisgn custom openlane flow](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/prep_deisgn_custom_openlane_flow.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/prep_deisgn_custom_openlane_flow.png)

[![openlan flow custom cell 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/openlan_flow_custom_cell_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/openlan_flow_custom_cell_1.png)

[![openlan flow custom cell 3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/openlan_flow_custom_cell_3.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/openlan_flow_custom_cell_3.png)

[![openlan flow custom cell 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/openlan_flow_custom_cell_2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/openlan_flow_custom_cell_2.png)

[![custom cell magic placement](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/custom_cell_magic_placement.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/custom_cell_magic_placement.png)

[![custom cell magic placement 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/custom_cell_magic_placement_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/custom_cell_magic_placement_1.png)

STA tool is used to analyze the timing performance of the circuit.STA will report problems such as worst negative slack (WNS)and total negative slack (TNS). These refer to the worst path delay and total path delay in regards to setup timing constraint.Fixing slack violations are analyzed using OpenSTA tool. These analysis are performed out of the openLANE flow and once we get the slack in required range, we save the enhanced netlist using `write_verilog` command and use this in openLANE flow to build clock tree and do further analysis in openROAD.

For the design to be complete, the worst negative slack needs to be above or equal to 0. If the slack is not within the range:

- Review synthesis strategy in OpenLANE
    
- Enable cell buffering
    
- Perform manual cell replacement using the OpenSTA tool
    

---

All openLANE configuration parameters are mentioned in **$OPENLANE_ROOT/configuration/README.md**.

---

### [5.3. CTS](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#53-cts)

The main concern in generation of clock tree is the clock skew, difference in arrival times of the clock for sequential elements across the design.To ensure timing constraints CTS will add buffers throughout the clock tree which will modify our netlist. This will generate new `def` file.

To run clock tree synthesis

```
% run_cts
```

|   |   |
|---|---|
|**TritonCTS**|Synthesizes the clock distribution network (the clock tree)|

[![cts](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/cts.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/cts.png)

[![cts 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/cts_2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/cts_2.png)

[![cts 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/cts_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/cts_1.png)

Further analysis of CTS in done in openROAD which is integrated in openLANE flow using openSTA tool.

```
% openroad
```

In openROAD the timing analysis is done by creating a db file from `lef` and `def` files. `lef` file won’t change as it a technology file, `def` file changes when a new is added.

```
% read_lef /openLANE_flow/designs/picorv32a/runs/03-07_16-12/tmp/merged.lef
% read_def /openLANE_flow/designs/picorv32a/runs/03-07_16-12/results/cts/picorv32a.cts.def
% write_db picorv32a_cts.db
```

[![openroad 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/openroad_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/openroad_1.png)

This creates db file in `$OPENLANE_ROOT` directory.

```
% read_db picorv32a_cts.db
% read_verilog /openLANE_flow/designs/picorv32a/runs/03-07_16-12/results/synthesis/picorv32a.synthesis_cts.v
% read_liberty -max $::env(LIB_SLOWEST)
% read_liberty -min $::env(LIB_FASTEST)
% read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
% set_propagated_clock [all_clocks]
% report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

We have done pre-CTS timing analysis to get setup and hold slack and post-CTS timing analysis to get setup and hold slack. For typical corners (`LIB_SYN_COMPLETE` env variable which points to typical library) setup and hold slack are met.

|   |   |
|---|---|
|`hold slack`|0.0167 ns|
|`setup slack`|4.5880 ns|

```
% echo $::env(CTS_CLK_BUFFER_LIST)
sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8
```

Try removing `sky130_fd_sc_hd__clkbuf_1` from clock tree and do post cts timing analysis

```
% set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8
% echo $::env(CTS_CLK_BUFFER_LIST)
sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8
```

```
% echo $::env(CURRENT_DEF)
/openLANE_flow/designs/picorv32a/runs/03-07_16-12/results/cts/picorv32a.cts.def
%
% set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/03-07_16-12/results/placement/picorv32a.placement.def
```

Now run openROAD and do a timing analysis as mentioned above.

|   |   |
|---|---|
|`hold_slack`|0.1828 ns|
|`setup_slack`|4.7495 ns|

Including large size clock buffers in clock path improves slack but area increases.

To check the clock skew

```
% report_clock_skew -hold
Clock clk
Latency      CRPR       Skew
_35319_/CLK ^
   1.31
_34316_/CLK ^
   0.80      0.00       0.51

% report_clock_skew -setup
Clock clk
Latency      CRPR       Skew
_35319_/CLK ^
   1.31
_34316_/CLK ^
   0.80      0.00       0.51
```

## [6. Day 5](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#6-day-5)

### [6.1. Power distribution network](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#61-power-distribution-network)

Power planning is a step which typically is done with floorplanning in which power grid network is created to distribute power to each part of the design equally. In openLANE flow it is done before routing.

Three levels of power distribution

|   |   |
|---|---|
|**Rings**|Carries VDD and VSS around the chip|
|**Stripes**|Carries VDD and VSS from Rings across the chip|
|**Rails**|Connect VDD and VSS to the standard cell VDD and VSS.|

[![power planning](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/power_planning.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/power_planning.png)

To run pdn

```
% gen_pdn
```

This generates new `def` file in `$OPENLANE_ROOT\designs\picorv32a\run\03-07_16-12/tmp/floorplan/19-pdn.def`

[![pdn 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/pdn_2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/pdn_2.png)

[![pdn 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/pdn_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/pdn_1.png)

### [6.2. Routing](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#62-routing)

Routing is the stage where the interconnnections. This includes interconnections of standard cells, the macro pins, the pins of the block boundary or pads of the chip boundary. Logical connectivity is defined by netlist and design rules are defined in technology file are available to routing tool. In routing stage, metal and vias are used to create the electrical connections.

**Global routing**

Coarse-grain assignment of routes to routing regions. In global routing wire segments are tentatively assigned within the chip layout.

**Detailed routing**

Fine-grain assignment of routes to routing tracks.During detailed routing, the wire segments are assigned to specific routing tracks.

|   |   |
|---|---|
|**FastRoute**|Performs global routing to generate a guide file for the detailed router|
|**CU-GR**|Another option for performing global routing.|
|**TritonRoute**|Performs detailed routing|
|**SPEF-Extractor**|Performs SPEF extraction|

To run routing:

```
% run_routing
```

[![routing 1](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_1.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_1.png)

[![routing 2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_2.png)

[![routing 3](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_3.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_3.png)

[![routing 4](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_4.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_4.png)

[![routing 5](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_5.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_5.png)

After routing magic tool can be used to get routing view

```
akshaym@openlane-workshop-03:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs$ magic -T $PDK_ROOT/sky130A/libs.tech/magic/sky130A.tech lef read 03-07_16-12/tmp/merged.lef def read 03-07_16-12/results/routing/picorv32a.def &
```

[![routing 6](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_6.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_6.png)

[![routing 7](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/routing_7.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/routing_7.png)

### [6.3. SPEF Extraction](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#63-spef-extraction)

After routing has been completed interconnect parasitics can be extracted to perform sign-off post-route STA analysis. The parasitics are extracted into a SPEF file using SPEF-Extractor.

`spef` file will be generated after `run_routing` command at location `$OPENLANE_ROOT/designs/picorv32a/runs/03-07_16-12/results/routing/picorv32a.spef`

[![spef extraction](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/spef_extraction.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/spef_extraction.png)

### [6.4. GDSII](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#64-gdsii)

GDSII files are usually the final output product of the IC design cycle and are given to silicon foundries for IC fabrication.It is a binary file format representing planar geometric shapes, text labels, and other information about the layout in hierarchical form.

To generate GDSII file

```
% run_magic
```

[![gds2](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/raw/main/images/gds2.png)](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop/blob/main/images/gds2.png)

`gds` file will be generated at location `$OPENLANE_ROOT/designs/picorv32a/runs/03-07_16-12/results/magic/picorv32a.gds`

## [7. Acknowledgement](https://github.com/akshaymdas/OpenLANE-SkyWater130-workshop#7-acknowledgement)

- [Kunal Ghosh](https://github.com/kunalg123)
    
- [Nickson Jose](https://github.com/nickson-jose)