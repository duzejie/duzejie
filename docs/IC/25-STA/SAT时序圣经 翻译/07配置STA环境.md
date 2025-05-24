---
title: 第七章：配置STA环境
author: Sunglow
top: false
cover: false
toc: false
mathjax: false
summary: 'null'
categories:
  - 集成电路
  - 时序分析
tags:
  - 集成电路
  - 时序分析
date: 2018-02-21 21:25:13
keywords:
---

<div>
<style>
img{
    width: 60%;
    //padding-left: 20%;
}
</style>
</div>


> 正确的约束对于分析STA结果很重要，只有准确指定设计环境，STA分析才能够识别出设计中的所有时序问题。STA的准备工作包括设置时钟、指定IO时序特性以及指定伪路径和多周期路径。在继续学习下一章的时序验证之前，请务必全面了解本章节。

## 7.1 什么是STA环境

大部分数字设计是同步的，从前一个时钟周期计算出的数据, 在时钟有效沿上被锁存在触发器中。请考虑图7-1所示的典型同步设计，假定**待分析设计（DUA）**会与其它同步设计交互。这意味着DUA从触发器接收数据，并将数据输出到DUA外部的另一个触发器。

![img](v2-aa744d6ffbef6109377a3eccda7e218d_1440w.jpg)图7-1

为了对这种设计执行STA，需要指定触发器的时钟、以及进入设计和退出设计的所有路径的时序约束。

图7-1中的例子假定只有一个时钟，并且C1、C2、C3、C4和C5代表组合逻辑块，其中C1和C5在待分析设计之外。

在典型的设计中，可能存在多个时钟，且许多路径都会从一个时钟域到另一个时钟域。以下各小节将介绍在这种情况下如何配置环境。

## 7.2 指定时钟

要定义时钟，我们需要提供以下信息：

● 时钟源（Clock source）：它可以是设计的端口，也可以是设计内部单元的引脚（通常是时钟生成逻辑的一部分）。

● 周期（Period）：时钟的周期。

● 占空比（Duty cycle）：高电平持续时间（正相位）和低电平持续时间（负相位）。

● 边沿时间（Edge times）：上升沿和下降沿的时刻。

基本定义如图7-2所示。通过定义时钟，所有内部时序路径（触发器到触发器路径）都将受到约束，这意味着可以仅使用时钟约束来分析所有内部路径。时钟约束指定触发器到触发器的路径必须占用一个周期，稍后我们将介绍如何放宽这一要求（一个周期时间）。

![img](v2-6e1ad5dbf33e4c8c136550a31b319aef_1440w.jpg)图7-2

以下是一个基本的时钟约束规范：

● **create_clock** **-name** SYSCLK **-period** 20 **-waveform** {0 5} [**get_ports** SCLK]

该时钟名为SYSCLK，并在端口SCLK上定义。SYSCLK的周期指定为20个单位，如果未指定，默认时间单位为纳秒（通常，时间单位会在技术库中进行指定）。**waveform**中的第一个自变量指定出现上升沿的时刻，第二个自变量指定出现下降沿的时刻。

**waveform**选项中可以指定任意数量的边沿。但是，所有边沿必须在一个周期内。边沿时刻从零时刻之后的第一个上升沿开始，然后是下降沿，然后再是上升沿，以此类推，这意味着**waveform**列表中的所有时刻值必须单调增加。

● **-waveform** {time_rise time_fall time_rise time_fall ...}

另外，必须指定偶数个边沿时刻。**waveform**选项将指定一个时钟周期内的波形，然后不断重复。

如果未指定任何**waveform**选项，则默认值为：

● **-waveform** {0，period/2}

以下是一个没有使用**waveform**选项的时钟约束示例（见图7-3）。

● **create_clock** **-period** 5 [**get_ports SCAN_CLK**]

在此约束中，由于未指定**-name**选项，因此时钟的名称与端口的名称相同，即SCAN_CLK。

![img](v2-dbbcd77f39398830ea5cf2fd2eb25bde_1440w.jpg)图7-3

以下是时钟约束的另一个示例，其中波形的边沿在一个周期的中间位置（见图7-4）。

![img](v2-d0c02747fa5844119df9c7e7195a1874_1440w.jpg)图7-4

● **create_clock** **-name** BDYCLK **-period** 15 **-waveform** {5 12} [**get_ports GBLCLK**]

时钟的名称为BDYCLK，并且在端口GBLCLK上定义。实际上，最好将时钟名称与端口名称保持一致。

以下是另一些时钟约束示例：

![img](v2-516fe313698455d1d526b37d09993dc6_1440w.jpg)图7-5

上图7-5（a）中的时钟约束为：

● **create_clock** **-period** 10 **-waveform** {5 10} [**get_ports** FCLK]

上图7-5（b）中的时钟约束为：

● **create_clock** **-period** 125 **-waveform** {100 150} [**get_ports** ARMCLK]

![img](v2-6c5dab5ba4ba161baf81c50755916381_1440w.jpg)图7-6

上图7-6（a）中的时钟约束为：

● **create_clock** **-period** 1.0 **-waveform** {0.5 1.375} [**get_ports** MAIN_CLK]

上图7-6（b）中的时钟约束为：

● **create_clock** **-period** 1.2 **-waveform** {0.3 0.4 0.8 1.0} [**get_ports** JTAG_CLK]

还有一些时钟约束如下：

● **create_clock** **-period** 1.27 **-waveform** {0 0.635} [**get_ports** clk_core]

● **create_clock** **-name** TEST_CLK **-period** 17 **-waveform** {0 8.5} **-add** [**get_ports** ip_io_clk[0]]

除了上述属性外，还可以在时钟源处指定过渡时间（压摆）。在某些情况下，例如顶层的输入端口或某些PLL的输出端口，工具无法自动计算出过渡时间。在这种情况下，在时钟源处显式地指定过渡时间很有用，这可以使用**set_clock_transition**命令来指定。

● **set_clock_transition** **-rise** 0.1 [**get_clocks** CLK_CONFIG]

● **set_clock_transition** **-fall** 0.12 [**get_clocks** CLK_CONFIG]

这个约束仅适用于理想时钟，一旦构建了时钟树就将其忽略，因为此时将会使用时钟引脚上的实际过渡时间。如果在输入端口上定义了时钟，也可以使用**set_input_transition**命令（参见7.7节）来约束时钟的压摆。

## 7.2.1 时钟不确定度

可以使用**set_clock_uncertainty**约束来指定时钟周期的时序不确定度（uncertainty），该不确定度可用于对可能会减少有效时钟周期的各种因素进行建模。 这些因素可能是时钟抖动（jitter）以及可能需要在时序分析中考虑的任何其它悲观度。

● **set_clock_uncertainty** **-setup** 0.2 [**get_clocks** CLK_CONFIG]

● **set_clock_uncertainty** **-hold** 0.05 [**get_clocks** CLK_CONFIG]

注意，建立时间检查的时钟不确定度将减少可用的有效时钟周期，如图7-7所示。对于保持时间检查，时钟不确定度将用作需要满足的额外时序裕量。

![img](v2-f96ee253d5ab4344d609d25ead44dd52_1440w.jpg)图7-7

以下命令可用于指定跨时钟边界路径上的时钟不确定度，称为时钟间不确定度（inter-clock uncertainty）。

● **set_clock_uncertainty** **-from** VIRTUAL_SYS_CLK **-to** SYSCLK **-hold** 0.05

● **set_clock_uncertainty** **-from** VIRTUAL_SYS_CLK **-to** SYSCLK **-setup** 0.3

● **set_clock_uncertainty** **-from** SYS_CLK **-to** CFG_CLK **-hold** 0.05

● **set_clock_uncertainty** **-from** SYS_CLK **-to** CFG_CLK **-setup** 0.1

图7-8中为两个不同时钟域SYS_CLK和CFG_CLK之间的路径。根据上述时钟间不确定度的约束，将100ps用作建立时间检查的不确定度，将50ps用作保持时间检查的不确定度。

![img](v2-12998af38a47e4142c6e20f5e6b4afce_1440w.jpg)图7-8

## 7.2.2 时钟延迟

可以使用**set_clock_latency**命令指定时钟的延迟。

● **set_clock_latency** 1.8 **-rise** [**get_clocks** MAIN_CLK]

● **set_clock_latency** 2.1 **-fall** [**all_clocks**]

时钟延迟有两种类型：网络延迟（network latency）和源延迟（source latency）。网络延迟是指从时钟定义点（create_clock）到触发器时钟引脚的延迟。源延迟，也称为插入延迟（insertion delay），是指从时钟源到时钟定义点的延迟，源延迟可能代表片上或片外延迟，图7-9展示了这两种情况。触发器时钟引脚上的总时钟延迟是源延迟和网络延迟之和。

![img](v2-c7004cb35ecb6cc694c7a0fb8573f446_1440w.jpg)图7-9

以下是一些指定源延迟和网络延迟的命令示例：

● **set_clock_latency** 0.8 [**get_clocks** CLK_CONFIG]

● **set_clock_latency** 1.9 **-source** [**get_clocks** SYS_CLK]

● **set_clock_latency** 0.851 **-source** **-min** [**get_clocks** CFG_CLK]

● **set_clock_latency** 1.322 **-source** **-max** [**get_clocks** CFG_CLK]

源延迟和网络延迟之间的一个重要区别是：一旦为设计建立了时钟树，就可以忽略网络延迟（假设指定了**set_propagated_clock**命令）。但是，即使在建立时钟树之后，源延迟也会保留。网络延迟是在进行时钟树综合（Clock Tree Synthesis）之前对时钟树延迟的估计值。在时钟树综合完成后，从时钟源到触发器时钟引脚的总时钟延迟是源延迟加上时钟树从时钟定义点到触发器的实际延迟。

下一节将介绍衍生时钟（generated clocks），7.9节将介绍虚拟时钟（virtual clocks）。

## 7.3 衍生时钟

衍生时钟是从主时钟（master clock）派生而来的时钟，主时钟是指使用**create_clock**命令定义的时钟。

在基于主时钟的设计中生成一个新时钟时，可以将这个新时钟定义为衍生时钟。例如，如果有一个时钟的三分频电路，则将在该电路的输出处定义一个衍生时钟。由于STA不知道分频逻辑输出的时钟周期已更改，更重要的是新的时钟周期是多少，因此需要定义衍生时钟。图7-10给出了衍生时钟示例，该时钟是主时钟CLKP的2分频。

● **create_clock** **-name** CLKP 10 [**get_pins** UPLL0/CLKOUT]

● **create_generated_clock** **-name** CLKPDIV2 **-source** UPPL0/CLKOUT **-divide_by** 2 [**get_pins** UFF0/Q]

![img](v2-941547528f380cbca82879d15bf2b3d7_1440w.jpg)图7-10

可以在触发器的输出端口定义一个新时钟是主时钟，而非衍生时钟吗？答案是肯定的，这确实是可能的，但是它也有一些缺点。定义主时钟而不是衍生时钟会创建一个新的时钟域。通常这不是问题，除了在设置STA约束时需要处理更多的时钟域外。相反，将新时钟定义为衍生时钟不会创建新的时钟域，并且衍生时钟会被认为与其主时钟同相，衍生时钟不需要进行额外的约束。因此，尽量将内部新生成的时钟定义为衍生时钟，而不是将其声明为另一个主时钟。

主时钟和衍生时钟之间的另一个重要区别是时钟源的概念。在主时钟中，时钟源位于主时钟的定义点。而在衍生时钟中，时钟源是主时钟的源而不是衍生时钟的源。这意味着在时钟路径报告中，时钟路径的起点始终是主时钟的定义点。这样一来，与定义新的主时钟相比，衍生时钟具有很大优势，因为对于新的主时钟，是不会自动考虑源延迟的。

图7-11给出了一个在两个输入端都有时钟的多路复用器示例，在这种情况下，不必在多路复用器的输出端定义时钟。如果选择信号设置为常数，则多路复用器的输出会自动获取正确的时钟传播。而如果多路复用器的选择端不受约束，则出于STA的目的，两个时钟都将通过多路复用器传播。在这样的情况下，STA会报告出TCLK和TCLKDIV5之间的路径。注意，这样的路径是不可能存在的，因为选择信号只能选择一个多路复用器的时钟输入。在这种情况下，可能需要设置伪路径或指定这两个时钟之间的互斥（exclusive）关系，以避免报告出错误的路径。当然，这假定设计中其它部分的TCLK和TCLKDIV5之间没有路径。

![img](v2-c423248b15d63a8735f39e7c97cfdffc_1440w.jpg)图7-11

如果多路复用器选择信号不是静态不变的并且在运行期间会发生变化，这样会发生什么呢？ 在这种情况下，会对多路复用器输入端进行时钟门控（clock gating）检查，时钟门控检查将在第10章中介绍，这些检查可确保多路复用器输入端的时钟相对于多路复用器选择信号能够安全地切换。

图7-12给出了一个示例，其中时钟SYS_CLK由触发器的输出进行门控。由于触发器的输出可能不是恒定的，因此处理这种情况的一种方法是在与门单元的输出处定义一个衍生时钟，该时钟与输入时钟相同。

![img](v2-fd500c1fb9ceb87b41d91b8d99c0e497_1440w.jpg)图7-12

● **create_clock** 0.1 [**get_ports** SYS_CLK]

● **create_generated_clock** **-name** CORE_CLK **-divide_by** 1 **-source** SYS_CLK [**get_pins** UAND1/Z]

下一个示例是一个衍生时钟，其频率高于源时钟的频率。波形如图7-13所示：

![img](v2-7f44f7cf3f9f2e084177458eef817bd3_1440w.jpg)图7-13

● **create_clock** **-period** 10 **-waveform** {0 5} [**get_ports** PCLK]

● **create_generated_clock** **-name** PCLK×2 **-source** [**get_ports** PCLK] **-multiply_by** 2 [**get_pins** UCLKMULTREG/Q]

请注意，在主时钟定义中指定了主时钟周期，然后**-multiply_by**和**-divide_by**选项指定了衍生时钟的频率。

### 时钟门控单元输出端的主时钟示例

考虑图7-14中所示的时钟门控示例，两个时钟分别输入进一个与门单元中，问题是与门单元的输出是什么呢？如果与门单元的输入均为时钟，则可以安全地在与门单元的输出端定义一个新的主时钟，因为该单元的输出与任何一个输入时钟都没有相位关系的可能性很小。

![img](v2-7aae184dbb9001688b317a5b0cd297c9_1440w.jpg)图7-14

● **create_clock** **-name** SYS_CLK **-period** 4 **-waveform** {0 2} [**get_pins** UFFSYS/Q]

● **create_clock** **-name** CORE_CLK **-period** 12 **-waveform** {0 4} [**get_pins** UFFCORE/Q]

● **create_clock** **-name** MAIN_CLK **-period** 12 **-waveform** {0 2} [**get_pins** UAND2/Z]

在内部引脚上创建时钟的一个缺点是：它会影响路径延迟计算，并迫使设计人员手动计算源延迟。

### 使用Edge和Edge_shift选项生成时钟

图7-15给出了一个衍生时钟的示例，除两个不同相的时钟外，还会生成一个二分频时钟。各时钟的波形也显示在图中。

![img](v2-d16d08f6864a33be0a8c5c02fa846b54_1440w.jpg)图7-15

下面给出了该示例中所有时钟的定义。衍生时钟的定义使用了**-edges**选项，这是定义衍生时钟的另一种方法。该选项采用源主时钟{上升，下降，上升}的边沿列表，以形成新的衍生时钟。主时钟的第一个上升沿是沿1，第一个下降沿是沿2，下一个上升沿是沿3，依此类推。

● **create_clock** **-period** 2 [**get_ports** DCLK]

● **create_generated_clock** **-name** DCLKDIV2 **-edge** {2 4 6} **-source** DCLK [**get_pins** UBUF2/Z]

● **create_generated_clock** **-name** PH0CLK **-edges** {3 4 7} **-source** DCLK [**get_pins** UAND0/Z]

● **create_generated_clock** **-name** PH1CLK **-edges** {1 2 5} **-source** DCLK [**get_pins** UAND1/Z]

如果衍生时钟的第一个边沿是下降沿怎么办？考虑如图7-16所示的衍生时钟G3CLK。可以通过指定边沿5、7和10来定义这种衍生时钟，如以下时钟约束所示。注意，1ns时刻的下降沿将被自动推断出来。

![img](v2-24ae3783cf838e134c2d2b78a476b625_1440w.jpg)图7-16

● **create_generated_clock** **-name** G3CLK **-edge** {5 7 10} **-source** DCLK [**get_pins** UAND0/Z]

**-edge_shift**选项可与**-edges**选项一起使用，以指定相应边沿的任何偏移以形成新的衍生波形。它指定边沿列表中每个边沿的偏移量（以时间单位）。以下是使用此选项的示例：

● **create_clock** **-period** 10 **-waveform** {0 5} [**get_ports** MIICLK]

● **create_generated_clock** **-name** MIICLKDIV2 **-source** MIICLK **-edges** {1 3 5} [**get_pins** UMIICLKREG/Q]

● **create_generated_clock** **-name** MIIDIV2 **-source** MIICLK **-edges** {1 1 5} **-edge_shift** {0 5 0} [**get_pins** UMIIDIV/Q]

边沿列表中的边沿序列必须以非降序排列，但是同一边沿可重复使用，以指示不同于源时钟占空比的时钟脉冲。上例中的**-edge_shift**选项通过将源时钟的边沿1移位0ns获得了衍生时钟的第一个边沿，通过将源时钟的边沿1偏移5ns获得了衍生时钟的第二个边沿，而通过将源时钟的边沿5移位0ns获得了衍生时钟的第三个边沿。下图7-17显示了上述波形：

![img](v2-67973f8c8d91e10d9977f44118aea458_1440w.jpg)图7-17

### 使用Invert选项生成时钟

这是衍生时钟的另一个示例，这个示例使用了**-invert**选项：

● **create_clock** **-period** 10 [**get_ports** CLK]

● **create_generated_clock** **-name** NCLKDIV2 **-divide_by** 2 **-invert** **-source** CLK [**get_pins** UINVQ/Z]

在所有其它衍生时钟选项都被使用后，**-invert**选项将会对衍生时钟进行反相。图7-18给出了产生这种反相时钟的原理图：

![img](v2-57fdab36496c9d5c1d365c97bb6b8a52_1440w.jpg)图7-18

### 衍生时钟的时钟延迟

也可以为衍生时钟指定时钟延迟，在衍生时钟上指定的源延迟还包括了从主时钟定义点到衍生时钟定义点的延迟。因此，由衍生时钟驱动的触发器的时钟引脚的总时钟延迟是主时钟源延迟、衍生时钟源延迟和衍生时钟网络延迟的总和。如下图7-19所示：

![img](v2-4df950c98fe52b60cfa72e2478a8b988_1440w.jpg)图7-19

衍生时钟可以将另一个衍生时钟作为其时钟源，即一个衍生时钟也可以具有衍生时钟，以此类推。但是，衍生时钟只能有一个主时钟。在后面的章节中将介绍更多衍生时钟的示例。

### 典型的时钟生成方案

图7-20给出了在典型ASIC设计中如何进行时钟分配（clock distribution）的情形。晶振（Oscillator）在芯片外部产生低频（典型值为10-50 MHz）时钟，片上PLL将其用作参考时钟，以生成高频低抖动时钟（典型值为200-800 MHz）。然后，该PLL的输出时钟被输入到时钟分频器逻辑中，该逻辑产生ASIC所需的时钟。

![img](v2-3db3a3ad843d1528c0fa9a02ca77d63d_1440w.jpg)图7-20

在时钟分配的某些分支上，可能有时钟门控（clock gates）用于在设计的无效部分关闭时钟，以在必要时节省功耗。在PLL的输出端也可以接一个多路复用器，以便在必要时可以绕过PLL。

在进入设计的芯片输入端口处为参考时钟定义了一个主时钟，在PLL的输出处定义了第二个主时钟。PLL的输出时钟与参考时钟没有任何相位关系。因此，PLL的输出时钟不应是参考时钟的衍生时钟。很有可能的是，由时钟分频器逻辑生成的所有时钟都将被指定为PLL输出处主时钟的衍生时钟。



## 7.4 约束输入路径

本节将介绍输入路径的约束。这里需要注意的一点是，STA无法检查不受约束的路径上的任何时序，因此需要约束所有路径以进行时序分析。在后面的章节中会介绍一些示例，其中一些示例可能并不关心某些逻辑，因而这些输入路径可能可以不用约束。例如，设计人员可能并不在乎一些输入控制信号的时序，因此可能并不需要进行本节中将要介绍的时序检查。但是，本节假定我们要约束全部的输入路径。

图7-21中为待分析设计（DUA）的输入路径。触发器UFF0在设计的外部，并向设计内部的触发器UFF1提供数据。数据通过输入端口INP1连接两个触发器。

![img](v2-f713ddf9c69f23bbe3ab321fb441dd5a_720w.jpg)图7-21

CLKA的时钟定义指定了时钟周期，这是两个触发器UFF0和UFF1之间可用的总时间。外部逻辑所需的时间为Tclk2q（数据发起触发器UFF0的CK至Q延迟）加上Tc1（通过外部组合逻辑的延迟），因此输入引脚INP1上的延迟定义指定了Tclk2q加上Tc1的外部延迟。并且这个外部延迟是相对于一个时钟指定的，在本示例中为时钟CLKA。

以下是输入延迟的约束：

● **set** Tclk2q 0.9

● **set** Tc1 0.6

● **set_input_delay** **-clock** CLKA **-max** [**expr** Tclk2q + Tc1] [**get_ports** INP1]

该约束指定输入端口INP1的外部延迟为1.5ns，且这是相对于时钟CLKA而言的。假设CLKA的时钟周期为2ns，则INP1引脚的逻辑只有500ps（= 2ns-1.5ns）可以在设计内部中传播。此输入延迟定义意味着输入约束为：Tc2加上触发器UFF1的Tsetup必须小于500ps，才可以确保可靠地捕获到触发器UFF0发起的数据。请注意，上述外部延迟值被指定为了最大值（max）。

![img](v2-1113d503c83279b8ab7675d415989a0e_720w.jpg)图7-22

让我们同时考虑最大和最小延迟情况，如图7-22所示。以下是此示例的约束：

● **create_clock** **-period** 15 **-waveform** {5 12} [**get_ports** CLKP]

● **set_input_delay** **-clock** CLKP **-max** 6.7 [**get_ports** INPA]

● **set_input_delay** **-clock** CLKP **-min** 3.0 [**get_ports** INPA]

INPA的最大和最小延迟是从CLKP到INPA的延迟中得出的，最大和最小延迟分别是最长和最短路径延迟，这些通常也可以对应于最坏情况（worst-case）下的慢速（最大时序工艺角）和最佳情况（best-case）下的快速（最小时序工艺角）。因此，最大延迟对应于最大时序工艺角下的最长路径延迟，最小延迟对应于最小时序工艺角下的最短路径延迟。在我们的示例中，1.1ns和0.8ns是Tck2q的最大和最小延迟值。组合逻辑路径延迟Tc1的最大延迟为5.6ns，最小延迟为2.2ns。INPA上的波形显示了数据到达设计输入端的时间窗口，以及预计达到稳定状态的时间。从CLKP到INPA的最大延迟为6.7ns（= 1.1ns + 5.6ns），最小延迟为3ns（= 0.8ns + 2.2ns），这些延迟是相对于时钟有效沿指定的。在给定外部输入延迟的情况下，设计内部的可用建立时间是慢角（slow corner）下的8.3ns（= 15ns-6.7ns）和快角（fast corner）下的12ns（= 15ns-3.0ns）中的最小值。因此，8.3ns是用来可靠地捕获DUA内部数据的可用时间。

以下是输入约束的更多示例：

● **set_input_delay** **-clock** clk_core 0.5 [**get_ports** bist_mode]

● **set_input_delay** **-clock** clk_core 0.5 [**get_ports** sad_state]

由于未指定**max**或**min**选项，因此500ps这个值将同时用于最大延迟和最小延迟。此外部输入延迟是相对于时钟clk_core的上升沿指定的（如果输入延迟是相对于时钟的下降沿指定的，则必须使用**-clock_fall**选项）。

## 7.5 约束输出路径

本节将借助下面的三个例子来介绍输出路径的约束。

### 例子A

图7-23为一条通过待分析设计输出端口的路径示例，其中Tc1和Tc2是通过组合逻辑的延迟。

![img](v2-60b45c8d7e395dc89d5c52efad0d7717_720w.jpg)图7-23

时钟CLKQ的周期定义了触发器UFF0和UFF1之间的总可用时间。外部逻辑的总延迟为Tc2加上Tsetup，此总延迟Tc2 + Tsetup必须作为输出延迟约束的一部分来指定。注意，输出延迟是相对于捕获时钟指定的，数据必须及时到达外部触发器UFF1才能满足其建立时间要求。

● **set** Tc2 3.9

● **set** Tsetup 1.1

● **set_output_delay** **-clock** CLKQ **-max** [**expr** Tc2 + Tsetup] [**get_ports** OUTB]

这指定了相对于时钟边沿的最大外部延迟为Tc2加上Tsetup，即5ns的延迟。最小延迟可以类似地指定。

### 例子B

图7-24给出了同时具有最小和最大延迟的示例。最大路径延迟为7.4ns（=Tc2最大值加上Tsetup = 7 + 0.4），最小路径延迟为-0.2ns（=Tc2最小值减去Thold = 0-0.2）。因此，输出约束为：

![img](v2-5fbca91a1e5e88caff80703df074f522_720w.jpg)图7-24

● **create_clock** **-period** 20 **-waveform** {0 15} [**get_ports** CLKQ]

● **set_output_delay** **-clock** CLKQ **-min** -0.2 [**get_ports** OUTC]

● **set_output_delay** **-clock** CLKQ **-max** 7.4 [**get_ports** OUTC]

图7-24中的波形显示了OUTC必须保持稳定状态的时间，以确保外部触发器能够可靠地捕获它。这说明在所需的稳定区域开始之前，数据就必须在输出端口准备就绪，并且必须保持稳定，直到稳定区域结束为止。这同样反映了DUA内部对输出端口OUTC逻辑的时序要求。

### 例子C

这是另一个输入约束和输出约束的示例。该模块具有两个输入端口DATAIN和MCLK，以及一个输出端口DATAOUT。图7-25显示了预期的波形。

![img](v2-6d4f02368e4d12bbd76106cb750dfdc2_720w.jpg)图7-25

● **create_clock** **-period** 100 **-waveform** {5 55} [**get_ports** MCLK]

● **set_input_delay** 25 **-max** **-clock** MCLK [**get_ports** DATAIN]

● **set_input_delay** 5 **-min** **-clock** MCLK [**get_ports** DATAIN]

● **set_output_delay** 20 **-max** **-clock** MCLK [**get_ports** DATAOUT]

● **set_output_delay** -5 **-min** **-clock** MCLK [**get_ports** DATAOUT]

## 7.6 时序路径组

设计中的时序路径可以视为路径的集合，每个路径都有一个起点和一个终点。时序路径的示例如下图7-26所示：

![img](v2-e18974487168538c443b0ea0df357384_720w.jpg)图7-26

在STA中，时序路径是根据有效的起点和终点来划分的。有效的起点包括：输入端口或者同步器件（如触发器和存储器）的时钟引脚。有效的终点包括：输出端口或者同步器件的数据输入引脚。因此，有效的时序路径包括：

● 从输入端口到输出端口

● 从输入端口到触发器或存储器的数据输入引脚

● 从一个触发器或存储器的时钟引脚到另一个触发器或存储器的数据输入引脚

● 从一个触发器或存储器的时钟引脚到输出端口

图7-26中的有效时序路径包括：

● 输入端口A到输出端口Z

● 输入端口A到触发器UFFA的D引脚

● 触发器UFFA的CK引脚到触发器UFFB的D引脚

● 触发器UFFB的CK引脚到输出端口Z

时序路径可以根据与路径终点相关的时钟分类为不同时序路径组（path groups）。因此，每个时钟都有一组与之相关的时序路径。还有一个默认时序路径组，其中包括了所有非时钟（异步）路径。

![img](v2-36074bc598c2171aa333caaa1c264150_720w.jpg)图7-27

在图7-27的示例中，时序路径分组为：

● CLKA组：输入端口A到触发器UFFA的D引脚

● CLKB组：触发器UFFA的CK引脚到触发器UFFB的D引脚

● 默认组：输入端口A到输出端口Z、触发器UFFB的CK引脚到输出端口Z

静态时序分析和报告通常分别在每个时序路径组中单独执行。

## 7.7 外部属性建模

尽管**create_clock**、**set_input_delay**和**set_output_delay**足以约束设计中用于执行时序分析的所有路径，但这些并不足以获取该模块IO引脚上的准确时序。为了准确地对设计环境进行建模，还需要以下属性。对于输入，需要在输入端口处指定压摆。可以使用以下方式提供此信息：

● **set_drive**

● **set_driving_cell**

● **set_input_transition**

对于输出，需要指定输出引脚的负载电容。可以使用以下命令来指定：

● **set_load**

## 7.7.1 驱动强度建模

**set_drive**和**set_driving_cell**约束用于对驱动模块输入端口的外部单元的驱动强度进行建模。在没有这些约束的默认情况下，假定所有输入都具有无限的驱动强度，即输入引脚的过渡时间为0。

**set_drive**明确指定了DUA输入引脚上的驱动电阻值，该电阻值越小，驱动强度越高，电阻值为0表示无限的驱动强度。

![img](v2-a753d1df32090d6b3274a51125074ccb_720w.jpg)图7-28

● **set_drive** 100 UCLK

● **set_drive** **-rise** 3 [**all_inputs**]

● **set_drive** **-fall** 2 [**all_inputs**]

输入端口的驱动强度用于计算第一个单元的过渡时间。指定的驱动强度还可用于计算在任何RC互连情况下从输入端口到第一个单元的延迟值，计算公式如下：

● 延迟值 = （驱动强度 * 网络负载） + 互连线延迟

**set_driving_cell**约束提供了一种更方便、更准确的方法来描述端口的驱动能力。**set_driving_cell**可用于指定驱动输入端口的单元类型。

![img](v2-cb03063702ee294420e80a17f10bc305_720w.jpg)图7-29

● **set_driving_cell** **-lib_cell** INV3 **-library** slow [**get_ports** INPB]

● **set_driving_cell** **-lib_cell** INV2 **-library** tech13g [**all_inputs**]

● **set_driving_cell** **-lib_cell** BUFFD4 **-library** tech90gwc [**get_ports** {testmode[3]}]

与**set_drive**约束一样，**set_driving_cell**也可用于计算第一个单元的过渡时间，并在任何互连情况下计算从输入端口到第一个单元的延迟值。

使用**set_driving_cell**约束的一个注意点是：由于输入端口上的电容性负载而导致驱动单元的增量延迟被视作为输入上的附加延迟被包括在内。

作为上述方法的替代方法，**set_input_transition**约束提供了一种在输入端口表示过渡时间的便捷方法，并且可以指定参考时钟。以下是图7-30中示例的约束以及其它约束示例：

![img](v2-8a101572b7131c2329492842e143c39d_720w.jpg)图7-30

● **set_input_transition** 0.85 [**get_ports** INPC]

● **set_input_transition** 0.6 [**all_inputs**]

● **set_input_transition** 0.25 [**get_ports** SD_DIN*]

总之，设计人员需要指定输入端的压摆值来确定输入路径中第一个单元的延迟。在没有该约束的情况下，将假设为理想过渡值0，这显然是不现实的。

## 7.7.2 电容负载建模

**set_load**约束在输出端口上设置了电容性负载，以模拟由输出端口驱动的外部负载。默认情况下，端口上的电容性负载为0。可以将负载显式地指定为电容值或某个单元的输入引脚电容。

![img](v2-4733861a936ea7709cd63288f7f2fad6_720w.jpg)图7-31

● **set_load** 5 [**get_ports** OUTX]

● **set_load** 25 [**all_outputs**]

● **set_load** **-pin_load** 0.007 [**get_ports** {shift_write[31]}]

指定输出上的负载很重要，因为该值会影响驱动输出的单元的延迟。在没有该约束的情况下，将假定负载为0，这显然是不现实的。

**set_load**约束还可用于在设计中指定内部网络上的负载，以下是一个例子：

● **set_load** 0.25 [**get_nets** UCNT5/NET6]

