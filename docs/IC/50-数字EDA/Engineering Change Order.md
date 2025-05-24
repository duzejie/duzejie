## Engineering Change Order

------

#### Engineering Change Order (ECO)

- Technique to add/ remove the logic with minimum modifications in the design
- To deliver the product to market as fast as possible with minimum Risk-to-Correctness and Schedule
- For fixing post Synthesis/ Route/ Silicon issues
- Fixing both timing and functionality issues
- Spare Cells placed in the design are used for ECO
- A Logic Gate/ Flip-flop can be realized using these Spare Cells
- Different flavors of Gate, of required drive strength can be realized any where in the design
- Only Metal/ Contact changes are needed after fixing the defective design

**Post Synthesis ECO (ECO after Synthesis)**

- ECO with Synthesized Netlist



**Post Route ECO (ECO after P&R)**

- During minute change in the design after full Tape-out is over

- Uses Spare cells and metal layers only

- Metal Layer ECO

	- During minute change in RTL after Active/Base Layer Tape-out is over
	- Metal Layer changes only
	- Cleaning-up routing for Signal Integrity (SI)

- Active Layer ECO (Base Layer ECO)

	- During minute change in RTL just after Routing is over
	- Uses Spare Cells
	- NAND Gate (Universal Logic Gate) based Spare Cell can be used to realize the new ECO logic
  


**Post Silicon ECO (ECO after Fabrication)**

- To recover from minute manufacturing issues
- Uses Spare cells and metal layers

**Metal Layer ECO (example)**
![metal eco in physical design](820_IC/50-数字EDA/attachments/Engineering%20Change%20Order/metal.JPG)

