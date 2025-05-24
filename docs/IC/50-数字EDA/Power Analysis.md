## Physical Design Analysis

------

### **Power Analysis**

- Power Density of the Integrated Circuit increase exponentially with every Technology generation
       PTOTAL = PDYNAMIC + PLEAKAGE
       PDYNAMIC = PSWITCHING + PSHORT_CIRCUIT

- Leakage Power (Static Power):    Leakage at almost all junctions due to various effects

	- Reverse Biased Diode Leakage
	- Gate Induced Drain Leakage
	- Gate Oxide Tunnelling
	- Sub-threshold Leakage

  

- **Switching Power:** When signal change their state, energy is drawn from the power supply to charge up the loadcapacitance from 0 to VDD

  

- **Short Circuit Power (Crowbar Power/ EM Rush Through Power):** Finite non-zero rise and fall times of transistors which causes a direct current path between the Power and Ground`

- Static/ Leakage Power Analysis
- Dynamic Power Analysis ![power analysis](docs/IC/50-数字EDA/attachments/Power%20Analysis/poweranalysis.JPG)
- With Shrinking technology Static leakage increases which results in more focus in Reducing leakage power for advanced technologies

------

#### **Static Power/ Leakage Power**

- It is the power consumed when the device is powered up but no signals are changing value (when the transistors are not switching)
- In CMOS devices, static power consumption is due to leakage
- Sub-threshold leakage occurs when a CMOS gate is not turned completely OFF

![static power, leakage power](820_IC/50-数字EDA/attachments/Power%20Analysis/eq.JPG)

where 
	**μ** - Carrier mobility 
	**COX** - Gate capacitance 
	**VT** - Threshold voltage 
	**VGS** - Gate-Source voltage 
	**W and L** - Dimensions of the transistor 
	**VTH** - Thermal voltage, kT/q = 25.9mV at room temperature 
	**n** - function of device fabrication process (ranges 1.0 -2.5) 

**Static Power Dissipation —** Leakage Power, is consumed when the transistors are not switching

- Dependent on the voltage, temperature and state of the transistors
- Leakage Power = V * Ileak

**Types of Static Leakages**

- Reverse biased diode leakage from the diffusion layers and the substrate
- Gate Induced Drain Leakage
- Gate Oxide Tunnelling
- Sub-threshold Leakage caused by reduced threshold voltages which prevents the Gate from completely turning OFF

**Static Power Reduction Techniques**

- Using Multi VT cell in the design and optimizing for leakage by replacing high VT cell for non timing critical paths
- Power Gating
  - Power Shut-off groups of logic which are not used
- Voltage Scaling
- Multi VDD and Voltage Island
- Multi-threshold CMOS (Back Biasing)

#### **Dynamic/ Switching Power**

- Dynamic power is the power consumed when the device is active, when signals are changing values (by switching logic states)

- Primary source of dynamic power consumption is switching power
     $P_{DYN}= A C V^2 F$
	where,
	A is activity factor, i.e., the fraction of the circuit that is switching
	C is Load capacitance
	V is supply voltage
	F is clock frequency

  

### **Dynamic Power Calculation depends on**

- Switching frequency

- Transition

- Output load

- Cell internal power **Dynamic Power Dissipation**
	- Dynamic power is dissipated any time the voltage on a net changesdue to some stimulus

**Types of Dynamic Power**

- Net Switching $Power = (Cint * V*V *f)$
- Internal $Power = (Cint * V*V *f) + (V * Isc)$

**Short Circuit :** = $(V*ISC)$ During switching both PMOS and NMOS becomes on which results in a short circuit current
**Internal Capacitance Loading Power** = $(Cint * V*V *f)$ is the power consumed while charging/discharging internal nets



#### **Dynamic Power Reduction Techniques**

![dynamic power reduction](docs/IC/50-数字EDA/attachments/Power%20Analysis/reduction.JPG)



#### **Dynamic Power Reduction Techniques**

**Clock Gating**

- Architectural Technique to reduce Dynamic Power along the Clock Path
- Clock gates should be placed at the Root of the Clock
- Results in small delay, more area and makes the design complex
- Clock Gating logic is generally in the form of "Integrated clock gating" (ICG)
- Sequential clock gating is the process of extracting/propagating the enable conditions to the upstream/downstream sequential elements, so that additional registers can be clock gated
- As the granularity on which you gate the clock of a synchronous circuit approaches zero, the power consumption of that circuit approaches that of an asynchronous circuit: the circuit only generates logic transitions when it is actively computing
  ![power analysis](docs/IC/50-数字EDA/attachments/Power%20Analysis/cgate.JPG)