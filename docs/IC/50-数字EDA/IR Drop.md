### IR Drop Analysis 

   **IR Drop**
                       ![img](irdrop.JPG)

- The voltage that gets to the internal circuitry is less than that applied to the chip, since every metal layer offers resistance to the flow of current
- When a current, I passes through a conductor with resistor R, it exhibits a voltage drop V which is equal to the resistance times the current,
                              **Ohm’s law, V=IR**
- IR Drop is defined as the average of the peak currents in the power network multiplied by the effective resistance from the power supply pads to the center of the chip
- IR Drop is a reduction in voltage that occurs on both Power and Ground networks
- IR Drop Analysis ensures that Power Delivery Network (PDN) is robust, and that your system will function to specification
- IR Drop is determined by the current flow and the supply voltage
- As distance between supply voltage and the component increases the IR Drop also increases

**IR Drop Analysis**

- IR Drop Analysis will compute the actual IDD and ISS currents, because these values are time-dependent
- IR Drop Analysis will compute Global IR drop which is important and more accurate, but cannot be compute separately (parallel) for smaller blocks, which may led to bigger run time ![img](irdrop1.JPG)
- Local IR Drop
  - IR Drop become a local phenomenon when a number of gates in close proximity switches at once
  - Local IR Drop can also be caused by a higher resistance to a specific portion of the Grid
- Global IR Drop
  - IR Drop is a global phenomenon when activity in one region of a chip causes an IR Drop in other regions
- In a well-meshed power grid with equally distributed currents, the power grid typically has a set of equipotential IR Drop surfaces that form concentric circles cantered in the middle of the chip
- So the center of the chip usually has the largest IR Drop or the lowest supply voltage
- Peak IR Drop is much larger than the Average IR Drop
- Peak IR Drop happens in the worst-case switch patterns of the gates

#### Types of IR Drop 

**Static IR Drop**

- Static IR drop is average voltage drop for the design
- The average current depends totally on the time period
- Static IR drop was good for signoff analysis in older  
    technology nodes where sufficient natural decoupling  
    capacitance from the power network and  
    non-switching logic were available
- Localized switching is only considered
- Only be a few % of the supply voltage
- Can be reduced by lowering the resistance of Supply  
    and Signal Paths

**Static IR Drop methodology**

- Extract power grid to obtain R
- Select stimulus
- Compute time averaged power consumption for a typical operation to obtain I(current)
- Compute: V = IR
- Non time-varying 
 ![IR Drop analysis](irdrop2.JPG)

------

**Dynamic IR Drop**

- When large amounts of circuitry switch simultaneously causing peak current demand
- Dynamic IR drop is mainly due to Instantaneous Voltage Drop (IVD) and it can be controlled by inserting Decap Cells in the Power network
- Dynamic IR drop depends on switching activity and switching time of the logic and is less dependent on the a clock period
- Instantaneous current demand could be highly localized and could be brief within a single clock cycle (a few hundred ps)
- Vector dependent, so VCD-based analysis is required

**Dynamic IR Drop methodology**

- Extract power grid to obtain on-chip R and C
- Include RLC model of the package and bond wires
- Select stimulus
- Compute time varying power for specific operation to obtain I(t)
- Compute $V(t) = I(t)*R + C*dv/dt*R + L*di/dt$

![ Dynamic IR Drop analysis](irdrop3.JPG)


#### IR Drop: Reasons 

- Improper placement of Power/Ground Pads
- Wrong Block placement
- Bad global power routing
- Insufficient Core Ring, Power Strap width
- Lesser no of Power Straps
- Missing Vias
- Insufficient number of Power Pads

#### IR Drop: Robustness Checks 

- Open circuits
- Missing or insufficient Vias
- Current Density violations
- Insufficient Power Rail design


![IR Drop analysis](irdrop4.JPG) 

#### IR Drop: Impacts 

- IR Drop Analysis confirms that the worst case voltage drop (which is considered for the worst corner for timing) on a chip meets IR Drop targets

- Impacts in Timing

  - If this Voltage Drop is too severe, the circuit will not get enough voltage, resulting in the malfunction or timing failure
  - If IR Drop increases Clock Skew then it will result in Hold Time Violations
  - If IR Drop increases Signal Skew then it will result in Setup Time Violations

  ![[Pasted image 20230925171111.png]]

### IR Drop Plot 

   Power grid has a set of equipotential surfaces that form concentric circles centered in the middle of a block ![IR Drop analysis](irdrop6.JPG)



### IR Drop: Remedies 

- Stagger the firing of buffers (bad idea: increases skew)
- Use different power grid tap points for clock buffers (but it makes routing more complicated for automated tools)
- Use smaller buffers (but it degrades edge rates/increases delay)
- Rearrange blocks
- More VDD pins
- Connect bottom portion of grid to top portion
- Distributing supplies symmetrically on the chip
- Lowering the resistance of Supply and Signal Paths by making supply wires thicker in dimensions than signal wires, R = ρ.L / A
- Decap insertion can solve Dynamic IR drop, at later stage of the design
- Amount of decap depends on:
	- Acceptable ripple on VDD-VSS (typically 10% noise budget)
	- Switching activity of logic circuits (usually need 10X switched cap)
	- Current provided by power grid (di/dt)
	- Required frequency response (high frequency operation)

#### Ldi/dt Effects 

- In addition to IR drop, power system inductance is also an issue
- Inductance may be due to power pin, power bump or power grid
- Overall voltage drop is:
                     **Vdrop = IR + Ldi/dt**
- As a solution to this effect, distribute decoupling capacitors (decaps) liberally throughout design

![electromigration](irdrop7.JPG)