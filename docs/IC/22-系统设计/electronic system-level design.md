
**Electronic system level ( #ESL ) design and verification** is an electronic design methodology, focused on higher abstraction level concerns.
Defines ESL as: _the utilization of appropriate abstractions in order to increase comprehension about a system, and to enhance the probability of a successful implementation of functionality in a cost-effective manner._


## **High-level synthesis** (**HLS**)
High-level synthesis ( #HLS ) sometimes referred to as **C synthesis**, **electronic system-level (ESL) synthesis**, **algorithmic synthesis**, or **behavioral synthesis**, is an automated design process that takes an abstract behavioral specification of a digital system and finds a register-transfer level structure that realizes the given behavior.

Synthesis begins with a high-level specification of the problem, where behavior is generally decoupled from low-level circuit mechanics such as clock-level timing. Early HLS explored a variety of input specification languages, although recent research and commercial applications generally accept synthesizable subsets of ANSI C/C++/SystemC/MATLAB. The code is analyzed, architecturally constrained, and scheduled to transcompile into a register-transfer level (RTL) design in a hardware description language (HDL), which is in turn commonly synthesized to the gate level by the use of a logic synthesis tool.

The goal of HLS is to let hardware designers efficiently build and verify hardware, by giving them better control over optimization of their design architecture, and through the nature of allowing the designer to describe the design at a higher level of abstraction while the tool does the RTL implementation. Verification of the RTL is an important part of the process.

Hardware can be designed at varying levels of abstraction. The commonly used levels of abstraction are [gate level](https://en.wikipedia.org/wiki/Digital_electronics "Digital electronics"), [register-transfer level](https://en.wikipedia.org/wiki/Register-transfer_level "Register-transfer level") (RTL), and [algorithmic](https://en.wikipedia.org/wiki/Algorithm "Algorithm") level.

While logic synthesis uses an RTL description of the design, high-level synthesis works at a higher level of abstraction, starting with an algorithmic description in a high-level language such as SystemC and ANSI C/C++. The designer typically develops the module functionality and the interconnect protocol. The high-level synthesis tools handle the micro-architecture and transform untimed or partially timed functional code into fully timed RTL implementations, automatically creating cycle-by-cycle detail for hardware implementation. The (RTL) implementations are then used directly in a conventional logic synthesis flow to create a gate-level implementation.


## Tools
There are various types of EDA tool used for ESL design. The key component is the Virtual Platform which is essentially a simulator. The Virtual Platform most commonly supports Transaction-level modeling ( #TLM ), where operations of one component on another are modelled with a simple method call between the objects modelling each component. This abstraction gives a considerable speed up over cycle-accurate modelling, since thousands of net-level events in the real system can be represented by simply passing a pointer.

Other tools support import and export or intercommunication with components modelled at other levels of abstraction. For instance, an RTL component be converted into a #SystemC model using VtoC or Verilator. And High Level Synthesis can be used to convert C models of a component into an RTL implementation.



---

[High-level synthesis - Wikipedia](https://en.wikipedia.org/wiki/High-level_synthesis)

Vendors: zotero{A Survey and Evaluation of FPGA High-Level Synthesis Tools}

[C to HDL - Wikipedia](https://en.wikipedia.org/wiki/C_to_HDL)
[High-level verification - Wikipedia](https://en.wikipedia.org/wiki/High-level_verification)

