


**第一阶段：数字 IC ASIC 之前端设计（RTL 设计）：**

用硬件描述语言HDL（Verilog、VHDL）来描述；描述硬件电路，抽象地表示电路的结构和行为（怎样组成，完成什么功能）；

> **HDL描述的两种方式：**  
> 结构描述：若干部件用信号线互连形成一个实体；  
> 行为描述：反映信号的变化、组合和传播行为，特点是信号的延迟和并行性；  
> **HDL的作用：**  
> 具有与具体硬件电路无关和与EDA工具平台无关的特性，简化了设计；  
> 支持从系统级到门和器件级的电路描述，并具有在不同设计层次上的仿真/验证机制；  
> 可作为综合工具的输入，支持电路描述由高层向低层的转换；



Upon agreement of a system design, RTL designers then implement the functional models in a hardware description language like [Verilog]( https://en.wikipedia.org/wiki/Verilog "Verilog"), [SystemVerilog]( https://en.wikipedia.org/wiki/SystemVerilog "SystemVerilog"), or [VHDL]( https://en.wikipedia.org/wiki/VHDL "VHDL"). Using digital design components like adders, shifters, and state machines as well as computer architecture concepts like pipelining, superscalar execution, and [branch prediction]( https://en.wikipedia.org/wiki/Branch_prediction "Branch prediction"), RTL designers will break a functional description into hardware models of components on the chip working together. Each of the simple statements described in the [[../22-系统设计/electronic system-level design]] can easily turn into thousands of lines of [RTL]( https://en.wikipedia.org/wiki/Register-transfer_level "Register-transfer level") code, which is why it is extremely difficult to verify that the RTL will do the right thing in all the possible cases that the user may throw at it.

To reduce the number of functionality bugs, a separate hardware verification group will take the RTL and design testbenches and systems to check that the RTL actually is performing the same steps under many different conditions, classified as the domain of [functional verification](https://en.wikipedia.org/wiki/Functional_verification "Functional verification"). Many techniques are used, none of them perfect but all of them useful – extensive [logic simulation](https://en.wikipedia.org/wiki/Logic_simulation "Logic simulation"), [formal methods](https://en.wikipedia.org/wiki/Formal_methods "Formal methods"), [hardware emulation](https://en.wikipedia.org/wiki/Hardware_emulation "Hardware emulation"), [lint](https://en.wikipedia.org/wiki/Lint_programming_tool "Lint programming tool")-like code checking, [code coverage](https://en.wikipedia.org/wiki/Code_coverage "Code coverage"), and so on.

A tiny error here can make the whole chip useless, or worse. The famous [Pentium FDIV bug]( https://en.wikipedia.org/wiki/Pentium_FDIV_bug "Pentium FDIV bug") caused the results of a division to be wrong by at most 61 parts per million, in cases that occurred very infrequently. No one even noticed it until the chip had been in production for months. Yet [Intel]( https://en.wikipedia.org/wiki/Intel "Intel") was forced to offer to replace, for free, every chip sold until they could fix the bug, at a cost of $475 million (US).






