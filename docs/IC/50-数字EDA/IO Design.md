## IO Design 

------

### IO Pads 

**Input Output Pads**

- Input/ Output circuits (I/O Pads) are intermediate structures connecting internal signals from the core of the integrated circuit to the external pins of the chip package
- Typically I/O pads are organized into a rectangular Pad Frame
- The input/output pads are spaced with a Pad Pitch
- Pads will have pins on all metal layers used in design for easy access while routing the design
- Number of layers depends on technology
- Multiple Power Pads are often used to reduce the power
- Pads consists of some logic cells like level shifters and buffers which will control the voltages of input and output signals and to increase/ decrease drive strength ![physical design input output ports](attachments/IO%20Design/iopads.JPG)

**Structure of Pads**

- Bonding Pad
  - Area to which the bond wire is soldered
  - The wire goes from the bonding pad to a chip pin
- ESD (Electrostatic Discharge) protection circuitry consisting of a pair of big PMOS, NMOS in a reverse biased diode structure
- Driving and Logic Circuitry for which the area of is designated ![pad in physical design](attachments/IO%20Design/structure.JPG)


**Implementation Guidelines**

- Isolate sensitive asynchronous inputs such as Clock or Bidirectional Pins from other switching pads with Power/Ground Pads
- Group Bidirectional Pads together such that all are in the input/ output mode
- Avoid continuous placing of simultaneous switching pads
- 2 extra pins = 1 extra pad on 2 sides and 4 extra pins = 1 extra pad on each side
- Power supply pads must be evenly distributed
- The number of Power Pads required are calculated based on the IO Signal Pads power requirement and Core Power requirement (IR drop limit)
- No. of IO Power Pads required in a design, Thumb Rule: One Pair of Power Pads for every 4 or 6 Signal Pads
- No. of Core Power Pads required in a design, ![IO Signals in physical design](attachments/IO%20Design/equ.JPG)

**Pad Limited design**

- The area of Pad limits the size of Die
- No. of IO pads are more or larger in size (technology dependent)
- Pad limited designs pose several challenges for design implementation and to the backend designers, if Die area is a constraint
- The Solution would be to use Flip Chip or Staggered IO placement techniques

**Core Limited Design**

- The area of Core limits the size of Die
- No. of IO Pads are lesser
- In these designs Inline IOs will be used
- It can be either due to large no. of Macros the design or due to larger logic

![core limited design pad limited design](attachments/IO%20Design/paddesign.JPG)





#### Types of IO Pads 

**Types of Pads according to Logic directions**

- Input Pad
- Output Pad
- Bidirectional Pad

![IO Pads ](attachments/IO%20Design/types.JPG)

**Types of Pads according to Logic Styles**

- Signal Pads
- Power Pads (Core Power and IO Power)
- Corner Pads
	- Corner pads contains only connections in all metal layers defined in technology
	- These pad used only for IO Ring continuity and chip metal density on corners and to maintain yield
- Filler Pads
	- IO Filler Cells contains only the geometrical information of the Power Rings in all metal layers
	- Continuity of Power Rings which is responsible for uniform distribution of power
	- Electrostatic Discharge protection

**According to the Pad locations**

- Peripheral IO Pads
- Area IO Pads
  ![IO Pad location](attachments/IO%20Design/iopads1.JPG)

**Types of Pads according to Implementation Styles**

- Inline
- Staggered
	- CUP (Circuit-Under-Pad)
	- Non-CUP (Circuit-Under-Pad)
- Flip Chip

**Inline IO Pads**
![Inline IO Pad](attachments/IO%20Design/inline.JPG)

- 

- Pads are placed next to each other, with the corresponding bond pads lined up against each other having a small gap in between
- Minimum Pitch is determined by foundry/vendor and is technology dependent

##### Staggered IO Pads 

**CUP (Circuit-Under-Pad)**

- Bonding Pad over the IO body itself
- Bonding Pad have to connected to the PAD Pin of IO
- Pad pin is located close to the center of the IO body for easier routing, signal integrity, and space saving
- Reduce the die size since the Bonding Pad does not take any extra space in addition to the IO body itself
- Advantages include more no. of IO’s, Optimal area utilization, Lower cost

**Non-CUP (Circuit-Under-Pad)**

- Useful technique if design is “Pad Limited”
- Place an inner and outer Bond Pad alternately
- A larger number of pads can be accommodated
- Disadvantage is that the overall height of the pad structure increases significantly

**Flip Chip IO Bumps**

- It is simply a direct connection of a flipped electrical component onto a substrate, carrier, or circuit board by means of conductive Bumps instead of the conventional Wire-bond
- In Flip Chip, IO Bumps and driver cells may be placed in the peripheral or core area
- Note, the large octagonal area IO Bumps overlaying placed cells in the core area
- No chip area benefit for small chips – full Bump array redistribution is very difficult
- In advanced technology nodes a separate Re-distribution layer (RDL) is make use of for the Bump connections ![flip chip](attachments/IO%20Design/flip.JPG)