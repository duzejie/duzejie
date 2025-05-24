
A typical IC design cycle involves several steps:


- [[Microarchitecture and system-level design]]
	- [[System Specification]]
		- Requirements
		- Feasibility study and die size estimate
		- Function analysis
		- [[docs/IC/22-系统设计/Instruction Set]]
	- [[docs/IC/22-系统设计/electronic system-level design]]
		-  
	- [[docs/IC/22-系统设计/Microarchitecture]]

- [[digital front end]] 前端
	- [[docs/IC/22-RTL设计/RTL 设计]]: This step converts the user specification (what the user wants the chip to do) into a register transfer level (RTL) description. The RTL describes the exact behavior of the digital circuits on the chip, as well as the interconnections to inputs and outputs.
		- Logic Design
		-  Analogue Design, Simulation & Layout
		-  Digital Design & Simulation
		-  System Simulation, Emulation & Verification
	-  Circuit Design
		- Digital design synthesis
		- Design For Test and Automatic test pattern generation 
		- Design for manufacturability (IC)
	- [Logic synthesis](https://en.wikipedia.org/wiki/Logic_synthesis "Logic synthesis"): The RTL is mapped into a gate-level netlist in the target technology of the chip.
	- Functional verification

- [[Digital Back-End]] 后端
	- [[docs/IC/28-物理实现/Physical design]]: This step takes the RTL, and a library of available logic gates (standard cell library), and creates a chip design. This step involves use of IC layout editor, layout and floor planning, figuring out which gates to use, defining places for them, and wiring (clock timing synthesis, routing) them together.
		- [Floorplanning]( https://en.wikipedia.org/wiki/Floorplan_ (microelectronics) "Floorplan (microelectronics)"): The RTL of the chip is assigned to gross regions of the chip, input/output (I/O) pins are assigned and large objects (arrays, cores, etc.) are placed.
		-   [Placement](https://en.wikipedia.org/wiki/Placement_(EDA) "Placement (EDA)"): The gates in the netlist are assigned to nonoverlapping locations on the die area.
		-   Logic/placement refinement: Iterative logical and placement transformations to close performance and power constraints.
		-   [Clock insertion](https://en.wikipedia.org/wiki/Clock_Distribution_Networks "Clock Distribution Networks"): Clock signal wiring is (commonly, [clock trees](https://en.wikipedia.org/wiki/Clock_tree "Clock tree")) introduced into the design.
		-   [Routing]( https://en.wikipedia.org/wiki/Routing_ (EDA) "Routing (EDA)"): The wires that connect the gates in the netlist are added.
		- Parasitic Extraction
		-   Postwiring optimization: Performance ([timing closure](https://en.wikipedia.org/wiki/Timing_closure "Timing closure")), noise ([signal integrity](https://en.wikipedia.org/wiki/Signal_integrity "Signal integrity")), and yield ([Design for manufacturability](https://en.wikipedia.org/wiki/Design_for_manufacturability_(IC) "Design for manufacturability (IC)")) violations are removed.
		-   [Design for manufacturability](https://en.wikipedia.org/wiki/Design_for_manufacturability_(IC) "Design for manufacturability (IC)"): The design is modified, where possible, to make it as easy and efficient as possible to produce. This is achieved by adding extra vias or adding dummy metal/diffusion/poly layers wherever possible while complying to the design rules set by the foundry.
		-   Final checking: Since errors are expensive, time-consuming and hard to spot, extensive error checking is the rule, [making sure the mapping to logic was done correctly](https://en.wikipedia.org/wiki/Formal_equivalence_checking "Formal equivalence checking"), and [checking that the manufacturing rules were followed faithfully](https://en.wikipedia.org/wiki/Design_rule_checking "Design rule checking").
		-   Chip finishing with [Tapeout]( https://en.wikipedia.org/wiki/Tape-out "Tape-out") and mask generation: the design data is turned into [photomasks]( https://en.wikipedia.org/wiki/Photomask "Photomask") in [mask data preparation]( https://en.wikipedia.org/wiki/Mask_data_preparation "Mask data preparation"). [[1]] ( https://en.wikipedia.org/wiki/Integrated_circuit_design#cite_note-Layout_book2-1 )
	- Physical Verification& Signoff  
	    - Static timing
	    -  Co-simulation and timing





---

## Fabrication

- Mask data preparation (LayoutPost Processing)
	-  Chip finishing with Tape out
	-  Reticle layout
	-  Layout-to-mask preparation
-  Reticle fabrication
-  Photomask fabrication
-  Wafer fabrication

## Packaging and testing
-  Packaging
-  Die test
	    - Post silicon validation and integration
	    -  Device characterization
	    -  Tweak (if necessary)
-  Chip Deployment
	    -  Datasheet generation (of usually a Portable Document Format (PDF) file)
	    -  Ramp up
	    -  Production
	    -  Yield Analysis / Warranty Analysis Reliability (semiconductor)
	    -  Failure analysis on any returns
	    -  Plan for next generation chip using production information if possible



