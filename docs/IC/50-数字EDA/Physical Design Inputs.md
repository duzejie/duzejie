## Physical Design Inputs

### Netlist (.v or .vhd)

It is the combination of sequential elements and their logical connectivity.

**Netlist contains**

- Std. Cell instance – Name & Drive Strength
- Macros & Memories instances

**Netlist also consists of**

- Ports of Standard Cells and Macros
- Interconnection details

**Eg of a Netlist:**

```verilog
Module half-adder (C,S,A,B);
  Input A,B;
  Output C,S;
  AND2 U1(.Y(C), .A(A), .B(B));
  EXOR U2(.Y(S), .I1(A), .I2(B));
Endmodule
```

------
### Constraints

**Types of Constraints**

- Design Rule Constraints

- Optimization Constraints


— Design Rules from the Fab.
	- Max. Cap./ Transition/Fanout
	- Clock Uncertainties


— Optimization Constraints from the designer
	- Timing Constraints/ Exceptions
	- Delay Constraints (Latency, Input Delay,Input Transition, Output Load and Output Transition)
	- Power and Area Constraints
	- System interface

</br>

**Synopsys Design Constraints (SDC)**
— Timing Constraints
	- Clock Definition (Time Period, Duty Cycle)
	- Timing Exceptions (False Paths,Asynchronous Paths)

  
— Non-Timing Constraints

- Operating conditions
- Wire load models
- System interface, Design rule constraints (DRVs - Max. Cap./ Transition/Fanout)
- Area constraints, Multi-voltage and Power optimization constraints
- Logic assignments

------

### Liberty Timing File (.lib or .db)

— Cell Logical View/ The Timing Library
— Std. Cell lib, Macro lib, IO lib
— Gate Delay = function of input transition time and output capacitance.

**LIB contains**

- Cell Type and Functionality
- Delay Models (WLD/ NLDM/ CCS)
- Pin/ Cell Timings and design rules
- PVT Conditions
- Power Details (Leakage and Dynamic)

**.lib file Example:**

```lib
Cell (AND2_3) {
area : 8.000
pin (o) {
direction : output;
timing () {
related_pin : “A”;
rise_propagation () }
rise_transition () }
function : “(A & B)”;
max_cap :
min_cap : }
pin (a) {
dir : input;
cap : ;
}
```

------

### Library Exchange Format(LEF)

— Cell Abstract View/ The Physical Library
— Std. Cell LEF
— Macro LEF
— IO LEF

**LEF contains**

- Cell Name, Shape, Size, Orientation & Class
- Port/Pin Name, Direction and Layout Geometries
- Obstruction/ Blockages
- Antenna Diff. Area

**.lef file Example:**

```lef
Layer m1
 Type routing
 width 0.50;
End m1
Layer via
 Type cut
End via
Macro NAND_1
Foreign NAND_1 0.00.00
Origin 0.00.00
Size 4.5 by 12.0
Symmetry x y;
Site core;
Pin A
 Dir input;
 Port
 Layer m1 End
```

------
### Technology Related files

**Technology file**

- Defines Units and Design Rules for Layers and Vias as per the Technology

- Name and Number conventions of Layers and Vias

- Physical and Electrical parameters of Layers and Vias

- E.g. Direction/ Type/ Pitch/ Width/ Offset/ Thickness/ Resistance/ Capacitance/ Max. Metal Density/ Antenna Rule/ Blockages/Design Rules

- Manufacturing Grid definition

- Site/Unit Tile definition

- Technology file has to load before loading other LEF files since it holds the layer information for that particular technology

- .tech.lef (Cadence Format)

- .tf - technology file (Synopsys Format)

  

**Interconnect Parasitic file**

- Used for layer parasitic extraction

- Contains Layer/ Via capacitance and resistance values in a Lookup Table (LUT) format

- Also used to generate parasitic formats for the extraction tools (e.g. nxtgrd, captbl)

- Extraction tool formats are more accurate than interconnect parasitic formats

- .ict - Interconnect Technology Format (Cadence Format)

- .itf - Interconnect Technology Format (Synopsys Format)

- .ptf - Process Technology File (Mentor Graphics Format)

  

**— Map file**

- Useful if is there is 2 different naming convections in Technology file, LEF or Interconnect Parasitic file

------

### TLU+ file

TLU+ file is a binary table format that stores the RC Coefficients. The TLU+ models enable accurate RC extraction results by including the effect of width, space, density and temperature on the resistance coefficients. The map file matches the layer and via names in the Milkyway technology file with the names in the ITF (Interconnect Technology Format) file.

The main functions of this file can be given as finding:

- R,C parasitics of metal per unit length.
- These parasitics are used for calculation Net delays.
- If TLU+ files are not given, then these are extracted from .ITF file.
- For loading TLU+ files, we have to load three files Max TLU+, Min TLU+ and Map file.
- Map file maps the .ITF file & .tf file of the layer and via names.

------

### Milkyway Library

**Note:** Milkyway library was used in ICC1 in ICC2 we called it as NDM (New data model)

Milkyway is a Synopsys library format that stores all of circuit files from synthesis through place and route all the way to signoff. Most Synopsys tools can read and write in the Milkyway format including Design Compiler, IC Complier, StarRCXT, Hercules, Jupiter & Prime Time.

The Milkyway database consists of libraries that contain information about your design. Libraries contain information about design cells, std cells, macro cells and so on. They contain physical descriptions and also logical information.

Milkyway provides 2 types of libraries that we can use
(i) reference lib and
(ii) design lib. Ref lib contains std cells and hard (or) soft macro cells which are typically created by vendors.

Ref lib contains physical info such as routing directions and the placement unit tile dimensions which is the width & height of the smallest instance that can be placed. A design lib contains a design cell which contains reference to multiple reference libraries (std cells & macro cells).
The most commonly used Milkyway views are CEL & FRAM. CEL is the full layout view and FRAM is the abstract view for place and route operations.

------


### Power Specification File

— Power Modes & Power Domains
— Tie Up supply & Tie Low supply
— Power Nets & GND Nets

------


### Optimization Directives

Don’t use
- Cells that are not supposed to optimize

Size only/ use only
- Upsizing/ Downsizing only with this list of cells

------


### Design Exchange Formats

— List & locations of Components, Vias,Pins, Nets, Special nets
— Die dimensions, Row definitions, Placement and Bounding Box Data, Routing Grids, Power Grids, Pre-routes
— .def, .fp are the common formats

------


### Clock Tree Constraints/Specification

- Root Pin Definition
- Insertion Delay (ID) and Skew Target
- Maximum Capacitance/ Transition/ Fanout (DRVs)
- Transition can be classified into Leaf Transition and Buffer Transition
- No. of Buffer Levels (Tree depth)
- List of Buffers/ Inverters for CTS
- List of Through pin, Preserved Pin, Exclude Pin
- NDRs can be defined in CTS Spec. for the Clock Tree Routing
- Macro Models


------

### IO Information File 

— Pin/ Pad locations
— Edge and order for IO Placement
— .tdf, .io are common formats



Ref: [PD Inputs | Physical Design | VLSI Back-End Adventure (vlsi-backend-adventure.com)](https://www.vlsi-backend-adventure.com/pd_inputs.html)