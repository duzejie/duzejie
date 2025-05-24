## Routing 

------

### Importance of Routing as Technology Shrinks 

- Device (Gate) delay decreases
- Interconnect resistance increases
- Vertical heights of interconnect layers increase, in an attempt to offset increasing interconnect resistance
- Area component of interconnect capacitance no longer dominates
- Lateral (sidewall) and fringing components of capacitance start to dominate the total capacitance of the interconnect
- Interconnect capacitance dominates total Gate loading

![routing in physical design](shrink.JPG)





### Routing Objectives 

- Skew requirements
- Open/Short circuit clean
- Routed paths must meet setup and hold timing margin
- DRVs max. Capacitance/ Transition must be under the limit
- Metal traces must meet foundry physical DRC requirements
- Layout geometries should meet Current Density specification

------

### Routing 

![routing in physical design](routing.JPG)

- Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined. Electrical connections using metals and vias are created in the layout, defined by the logical connections present in the netlist (i.e. Logical connectivity converted as physical connectivity).
- After CTS, we have information of all the placed cells, blockages, clock tree buffers/inverters and I/O pins. The tool relies on this information to electrically complete all connections defined in the netlist such that:
  - There are minimal DRC violations while routing.
  - The design is 100% routed with minimal LVS violations.
  - There are minimal SI related violations.
  - There must be no or minimal congestion hot spots.
  - The Timing DRCs & QOR are met and good respectively.

------

### Inputs of Routing 

- Netlist
- All cells & ports should be legally placed with clock tree structure & CTS DEF file
- NDRs
- Routing blockages
- Technology data (metal layers (lef, tech file etc...), DRC rules, via creation rules, grid rules (metal pitch) etc...)

------

### Goals of Routing 

- Minimize the total interconnect/wire length.
- Minimize the critical path delay.
- Minimize the number of layer changes that the connections have to make (minimizing the number vias).
- Complete the connections without increasing the total area of the block.
- Meeting the Timing DRCs and obtaining a good Timing QoR.
- Minimizing the congestion hotspots.
- SI driven: reduction in cross-talk noise and delta delays.

------

### Routing Constraints 

- Set constraints to number of layer to be used during routing.
- Seting limits on routing to specific regions.
- Seting the maximum length for the routing wires.
- Blocking routing in specific regions.
- Set stringent guidelines for minimum width and minimum spacing.
- Set preferred routing directions to specific metal layers during routing.
- Constraining the routing density.
- Constraining the pin connections.

------

------

### Routing Flow 

The different tasks that are performed in the routing stage are as follows:

![routing flow in physical design](routingflow.JPG)

------

### Trial/Global Routing 

![global routing in physical design](820_IC/50-数字EDA/attachments/Routing/global.JPG)

- Identifying routable path for the nets driving/driven pins in a shortest distance
- Does not consider DRC rules, which gives an overall view of routing and congested nets
- Assign layers to the nets
- Identify and assign net segments over the specific routable window called Global Route Cell (GRC)
- Avoid congested areas and also long detours
- Avoid routing over blockages
- Avoid routing for pre-route nets such as Rings/Stripes/Rails
- Uses Steiner Tree and Maze algorithm

------

### Track Assignment 

- Takes the Global Routed Layout and assigns each nets to the specific Tracks and layer geometry
- It does not follow the physical DRC rules
- It will do the timing aware Track Assignment
- It helps in Via Minimization

------

### Detail/Nano Routing 

![routing in physical design](detailed.JPG)

- Detailed routing follows up with the track routed net segments and performs the complete DRC aware and timing driven routing
- It is the final routing for the design built after the CTS and the timing is freeze
- Filler Cells are adding before Detailed Routing
- Detail Routing is done after analyze the cause for congestion in the design, add density screen or change flooplan etc.

------

### Grid Based Routing 

![routing in physical design](grid.JPG)

- Metal traces (routes) are built along and centered upon routing tracks on the grid points
- Various types of grids are Manufacturing Grid, Routing Grid (Pitch) and Placement Grid
- Grid dimension should be multiple of Manufacturing Grid

------

#### Routing Preferences 

- - Typically Routing only in “Manhattan” N/S E/W directions
         E.g. layer 1 – N/S Layer 2 – E/W
  - Spacing checks with the adjacent layers
  - Width checks for all layers
  - Via dimension rules
  - Slotting rules
  - A segment cannot cross another segment on the same wiring layer
  - Wire segments can cross wires on other layers
  - Power and Ground have their own layers, mostly the top layers

![routing in physical design](manhattan.JPG)

- Layer Routing directions: Each metal layer has its own preferred routing direction and are defined in a technology rule file

  - M1: Horizontal, M2: Vertical , M3: Horizontal, M4: Vertical and so on

  

- In some cases, we can avoid following preferred routing direction for smart routing (Non-preferred direction)

------

#### Detailed Routing - Incremental Fixes: 

- Based on DRC type, tool will either spread the wires or fill a gap. These fixes might cause some more violations like Notch filling can cause spacing violations because of increased width and wire spreading can cause violation in adjacent region.
- Tool will analyze them and fix them. This is iterative loop.

------

#### Search and Repair: 

- The search-and-repair stage is performed during detailed routing after the first iteration. In search-and-repair, shorts and spacing violations are located and rerouting of affected areas to fix all possible violation isexecuted.

------


## Post Routing Optimization 

- Signal Integrity (SI) Optimization by NDRs and Shielding for the sensitive nets
- Types of Shielding for sensitive nets
  - Same layer shielding
  - Adjacent layer/ Coaxial shielding

![routing in physical design](signal.JPG)

------

### Filler Cell insertion 

- Filler Cells can be inserted before or after Detailed Routing
- If Fillers contain metal routing other than Pre-Routing then Fillers should be inserted before Routing
- Width of the smallest Filler Cell is the Placement Grid Width
- Once Fillers are inserted then the placement is fixed and tool can’t move Cells for further optimization

![routing in physical design](820_IC/50-数字EDA/attachments/Routing/filler.JPG)

------

### Metal Fill 

- Filling up the empty metal tracks with metal shapes to met metal density rules

- 2 types of Metal Fill

  - Floating Metal Fill: Doesn’t completely shield the aggressor nets, so SI will be prominent
  - Grounded Metal Fill: Completely shields the aggressor nets, so less SI impact
  - Grounded Metal Fill is complex as compared to Floating Metal Fill

  

- Metal Density Rule helps to avoid Over Etching/ Metal Erosion

------

### Spare Cells Tie-up/ Tie-down 

- Tie Cells connects the Gate of Cells to VDD/ VSS so reduces ESD
- Tie-up Cells help in avoid Power Bounce
- Tie-down Cells help in avoid Ground Bounce
- Tie Cells are basically MOS in Diode-Connected configuration

------

### Outputs of Routing: 

- Routing db file or DEF file with no opens & shorts
- Timing report
- Congestion report
- Skew & Insertion delay report
- Geometric layouts of all nets

