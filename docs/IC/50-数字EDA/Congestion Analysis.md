## Physical Design Analysis

------

### **Congestion Analysis**

- As the Technology advances, millions of transistors can be packed onto the surface of a chip

- Thus the increased circuit density introduces additional Congestion

- Intuitively speaking, Congestion in a layout means too many nets are routed in local regions

- This causes detoured nets and un-routable nets in Detailed Routing

  

- Congestion Analysis

	- Routing Congestion Analysis
		- Congestion in general referred to Routing Congestion
		- Routing congestion is the difference between supplied and available tracks
		- A track is nothing but a routing resource which fills the entire Core
	- Placement Congestion Analysis
		- Placement Congestion is due to overlap of Standard Cells, it is called Overlappingrather than called as Congestion
		- Overlapping issue can be fixed by aligning cells to the Placement Grid by Legalization

- In recent years, several congestion estimation and removal methods have been proposed
- They fall into two categories: Congestion estimation and removal during global routing stage, and Congestion estimation and removal during Placement stage
- To estimate Congestion, tool does Initial/ Global Routing
- Congestion reports are generated after each Routing stages which shows the difference between supplied and demanded Tracks or G-cells
- Overflow = Routing Demand - Routing Supply (0% otherwise)
- Usually starts the initial Target Utilization with 65% to 70%
- 7/3 in a 2D congestion map : There are 7 routes that are passing through a particular edge of a Global Route Cell (GRC), but there are only 3 routing tracks available. There is an overflow of 4.



#### **Causes for Routing Congestion**

- Missing Placement Blockages
- Inefficient floorplan
- Improper macro placement and macro channels (Placing macros in the middle of floorplan etc.)
- Floorplan the macros without giving routing space for interconnection between macros
- High Cell Density (High local utilization)
- If your design had more number of AOI/OAI cells you will see this congestion issue
- Placement of standard cells near macros
- High pin density on one edge of block
- Too many buffers added for optimization
- No proper logic optimization
- Very Robust Power network
- High via density due to dense power mesh
- Crisscross IO pin alignment is also a problem
- Module splitting


![congestion in physical design](docs/IC/50-数字EDA/attachments/Congestion%20Analysis/congestion.JPG)


------

#### **Congestion Fixes**
- Add placement blockages in channels and around macro corners
- Review the macro placement
- Reduce local cell density using density screens
- Reordering scan chain to reduce congestion
- Congestion driven placement with high effort
- Continue the iterations until good congestion results
- Density screen is applied to limit the density of standard cells in an area to reduce congestion due high pin density 

![congestion fixes in physical design](docs/IC/50-数字EDA/attachments/Congestion%20Analysis/congestion1.JPG)


------

- Routing congestion, results when too many routes need to go through an area with insufficient “routing tracks” to accommodate them ![routing cpngestion](docs/IC/50-数字EDA/attachments/Congestion%20Analysis/congestion2.JPG)
- Two major categories: Global Congestion and Local Congestion
	- **Global Congestion:** This occurs when there are a lot of chip-level or interblock wires that need to cross an area
	- **Local Congestion:** This occurs when the floorplan has macros and other routing blockages that are too close together to get enough routes through to connect to the macros

------

#### **Congestion Profiles**

![congestion profiles, global local congestion](docs/IC/50-数字EDA/attachments/Congestion%20Analysis/congestion3.JPG)