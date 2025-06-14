---
title: Verilog教程
author: Sunglow
top: false
cover: false
toc: false
mathjax: false
summary: 'null'
categories:
  - 集成电路
  - verilog
tags:
  - verilog
date: 2018-02-21 21:25:13
keywords:
---



# 1. Verilog 

## 1.1 Verilog 教程



Verilog HDL（简称 Verilog ）是一种硬件描述语言，用于数字电路的系统设计。可对算法级、门级、开关级等多种抽象设计层次进行建模。

Verilog 继承了 C 语言的多种操作符和结构，与另一种硬件描述语言 VHDL 相比，语法不是很严格，代码更加简洁，更容易上手。

Verilog 不仅定义了语法，还对语法结构都定义了清晰的仿真语义。因此，Verilog 编写的数字模型就能够使用 Verilog 仿真器进行验证。

## 1.3 Verilog 环境搭建

[1.3 Verilog 环境搭建 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/verilog-install.html)

## 1.4 Verilog 设计方法

Verilog 的设计多采用自上而下的设计方法（top-down）。即先定义顶层模块功能，进而分析要构成顶层模块的必要子模块；然后进一步对各个模块进行分解、设计，直到到达无法进一步分解的底层功能块。这样，可以把一个较大的系统，细化成多个小系统，从时间、工作量上分配给更多的人员去设计，从而提高了设计速度，缩短了开发周期。

[1.4 Verilog 设计方法 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/verilog-design-method.html)

![img](Verilog基础教程/vlg-design-method-1.png)

### 设计流程

![img](Verilog基础教程/vlg-design-method-2.png)

Verilog 的设计流程，一般包括以下几个步骤：

**需求分析**

工作人员需要对用户提出的功能要求进行分析理解，做出电路系统的整体规划，形成详细的技术指标，确定初步方案。例如，要设计一个电子屏，需要考虑供电方式、工作频率、产品体积、成本、功耗等，电路实现采用 ASIC 还是选用 FPGA/CPLD 器件等。

**功能划分**

正确地分析了用户的电路需求后，就可以进行逻辑功能的总体设计，设计整个电路的功能、接口和总体结构，考虑功能模块的划分和设计思路，各子模块的接口和时序（包括接口时序和内部信号的时序）等，向项目组成员合理分配子模块设计任务。

**文本描述**

可以用任意的文本编辑器，也可以用专用的 HDL 编辑环境，对所需求的数字电路进行设计建模，保存为 **.v** 文件。

**功能仿真（前仿真）**

对建模文件进行编译，对模型电路进行功能上的仿真验证，查找设计的错误并修正。

此时的仿真验证并没有考虑到信号的延迟等一些 timing 因素，只是验证逻辑上的正确性。

**逻辑综合**

综合（synthesize），就是在标准单元库和特定的设计约束的基础上，将设计的高层次描述（Verilog 建模）转换为门级网表的过程。逻辑综合的目的是产生物理电路门级结构，并在逻辑、时序上进行一定程度的优化，寻求逻辑、面积、功耗的平衡，增强电路的可测试性。

但不是所有的 Verilog 语句都是可以综合成逻辑单元的，例如时延语句。

**布局布线**

根据逻辑综合出的网表与约束文件，利用厂家提供的各种基本标准单元库，对门级电路进行布局布线。至此，已经将 Verilog 设计的数字电路，设计成由标准单元库组成的数字电路。

**时序仿真（后仿真）**

布局布线后，电路模型中已经包含了时延信息。利用在布局布线中获得的精确参数，用仿真软件验证电路的时序。单元器件的不同、布局布线方案都会给电路的时序造成影响，严重时会出现错误。出错后可能就需要重新修改 RTL（寄存器传输级描述，即 Verilog 初版描述），重复后面的步骤。这样的过程可能反复多次，直至错误完全排除。

**FPGA/CPLD 下载或 ASIC 制造工艺生产**

完成上面所有步骤后，就可以通过开发工具将设计的数字电路目标文件下载到 FPGA/CPLD 芯片中，然后在电路板上进行调试、验证。

如果要在 ASIC 上实现，则需要制造芯片。一般芯片制造时，也需要先在 FPGA 板卡上进行逻辑功能的验证。

# 2 Verilog 语法

## 2.1 Verilog 基础语法

### 格式

- Verilog 是区分大小写的。

- 格式自由，可以在一行内编写，也可跨多行编写。

- 每个语句必须以分号为结束符。空白符（换行、制表、空格）都没有实际的意义，在编译阶段可忽略。例如下面两中编程方式都是等效的。



**实例1 不换行（不推荐）**

```verilog
wire [1:0] results ;assign results = (a == 1'b0) ? 2'b01 ： (b==1'b0) ? 2'b10 : 2'b11 ;
```

**实例2  换行（推荐）**

```verilog
wire [1:0]  results ;
assign      results = (a == 1'b0) ? 2'b01: (b==1'b0) ? 2'b10 :2'b11 ;
```

### 注释

Verilog 中有 2 种注释方式:

用 **//** 进行单行注释：

```verilog
reg [3:0] counter ;  // A definition of counter register
```

用 **/\*** 与 ***/** 进行跨行注释:

```verilog
wire [11:0]  addr ;
/* 
Next are notes with multiple lines.
Codes here cannot be compiled.
*/
assign   addr = 12'b0 ;
```

### 标识符与关键字

标识符（identifier）可以是任意一组字母、数字、**$** 符号和 **_**(下划线)符号的合，但标识符的第一个字符必须是字母或者下划线，不能以数字或者美元符开始。

另外，标识符是区分大小写的。

关键字是 Verilog 中预留的用于定义语言结构的特殊标识符。

Verilog 中关键字全部为小写。

```verilog

reg [3:0] counter ; //reg 为关键字， counter 为标识符*
input clk; //input 为关键字，clk 为标识符*
input CLK; //CLK 与 clk是 2 个不同的标识符*
```

## 2.2 Verilog 数值表示

### 数值种类

Verilog HDL 有下列四种基本的值来表示硬件电路中的电平逻辑：

- 0：逻辑 0 或 "假"
- 1：逻辑 1 或 "真"
- x 或 X：未知
- z 或 Z：高阻

**x** 意味着信号数值的不确定，即在实际电路里，信号可能为 1，也可能为 0。

**z** 意味着信号处于高阻状态，常见于信号（input, reg）没有驱动时的逻辑结果。例如一个 pad 的 input 呈现高阻状态时，其逻辑值和上下拉的状态有关系。上拉则逻辑值为 1，下拉则为 0 。

### 整数数值表示方法

数字声明时，合法的基数格式有 4 中，包括：十进制('d 或 'D)，十六进制('h 或 'H)，二进制（'b 或 'B），八进制（'o 或 'O）。数值可指明位宽，也可不指明位宽。

**指明位宽：**

```verilog
4'b1011     *// 4bit 数值*
32'h3022_c0de  *// 32bit 的数值*
```

其中，下划线 **_** 是为了增强代码的可读性。

**不指明位宽:**

一般直接写数字时，默认为十进制表示，例如下面的 3 种写法是等效的：

```verilog
counter = 'd100 ; *//一般会根据编译器自动分频位宽，常见的为32bit*
counter = 100 ;
counter = 32'h64 ;
```

**负数表示**

通常在表示位宽的数字前面加一个减号来表示负数。例如：

```verilog
-6'd15  
-15
```

-15 在 5 位二进制中的形式为 5'b10001, 在 6 位二进制中的形式为 6'b11_0001。

需要注意的是，减号放在基数和数字之间是非法的，例如下面的表示方法是错误的：

```verilog
4'd-2 //非法说明
```

### 实数表示方法

实数表示方法主要有两种方式：

**十进制：**

```verilog
30.123
6.0
3.0
0.001
```

**科学计数法：**

```verilog
1.2e4         //大小为12000
1_0001e4      //大小为100010000
1E-3          //大小为0.001
```

### 字符串表示方法

字符串是由双引号包起来的字符队列。字符串不能多行书写，即字符串中不能包含回车符。Verilog 将字符串当做一系列的单字节 ASCII 字符队列。例如，为存储字符串 "www.runoob.com", 需要 14*8bit 的存储单元。例如：

```verilog
reg [0: 14*8-1]       str ;
initial begin
    str = "www.runoob.com";
end  
```

## 2.3 Verilog 数据类型

Verilog 最常用的 2 种数据类型就是线网（wire）与寄存器（reg），其余类型可以理解为这两种数据类型的扩展或辅助。

### 线网（wire）

wire 类型表示硬件单元之间的物理连线，由其连接的器件输出端连续驱动。如果没有驱动元件连接到 wire 型变量，缺省值一般为 "Z"。举例如下：

```verilog
wire   interrupt ;
wire   flag1, flag2 ;
wire   gnd = 1'b0 ;  
```

线网型还有其他数据类型，包括 ***wand，wor，wri，triand，trior，trireg*** 等。这些数据类型用的频率不是很高，这里不做介绍。

### 寄存器（reg）

寄存器（reg）用来表示存储单元，它会保持数据原有的值，直到被改写。声明举例如下：

**reg**   clk_temp;
**reg**  flag1, flag2 ;

例如在 always 块中，寄存器可能被综合成边沿触发器，在组合逻辑中可能被综合成 wire 型变量。寄存器不需要驱动源，也不一定需要时钟信号。在仿真时，寄存器的值可在任意时刻通过赋值操作进行改写。例如：

```verilog
reg rstn ;
initial begin
    rstn = 1'b0 ;
    #100 ;
    rstn = 1'b1 ;
end
```



### 向量

当位宽大于 1 时，wire 或 reg 即可声明为向量的形式。例如：

```verilog
reg [3:0]      counter ;    //声明4bit位宽的寄存器counter
wire [32-1:0]  gpio_data;   //声明32bit位宽的线型变量gpio_data
wire [8:2]     addr ;       //声明7bit位宽的线型变量addr，位宽范围为8:2
reg [0:31]     data ;       //声明32bit位宽的寄存器变量data, 最高有效位为0
```

对于上面的向量，我们可以指定某一位或若干相邻位，作为其他逻辑使用。例如：

```verilog
wire [9:0]     data_low = data[0:9] ;
addr_temp[3:2] = addr[8:7] + 1'b1 ;
```

Verilog 支持可变的向量域选择，例如：

```verilog
reg [31:0]     data1 ;
reg [3:0]      byte1 [7:0];
integer j ;
always@* begin
    for (j=0; j<=3;j=j+1) begin
        byte1[j] = data1[(j+1)*4-1 : j*4];
        //把data1[7:0]…data1[31:24]依次赋值给byte1[0][7:0]…byte[3][7:0]
    end
end
```



**Verillog 还支持指定 bit 位后固定位宽的向量域选择访问。**

- **[bit+: width]** : 从起始 bit 位开始递增，位宽为 width。
- **[bit-: width]** : 从起始 bit 位开始递减，位宽为 width。

```verilog
//下面 2 种赋值是等效的*
A = data1[31-: 8] ;
A = data1[31:24] ;

//下面 2 种赋值是等效的*
B = data1[0+ : 8] ;
B = data1[0:7] ;
```

**对信号重新进行组合成新的向量时，需要借助大括号。例如：**

```verilog
wire [31:0]    temp1, temp2 ;
assign temp1 = {byte1[0][7:0], data1[31:8]};  //数据拼接
assign temp2 = {32{1'b0}};  //赋值32位的数值0  
```



### 整数，实数，时间寄存器变量

整数，实数，时间等数据类型实际也属于寄存器类型。

**整数（integer）**

整数类型用关键字 integer 来声明。声明时不用指明位宽，位宽和编译器有关，一般为32 bit。reg 型变量为无符号数，而` integer 型变量为有符号数`。例如：

```verilog
reg [31:0]      data1 ;
reg [3:0]       byte1 [7:0]; //数组变量，后续介绍
integer j ;  //整型变量，用来辅助生成数字电路
always@* begin
    for (j=0; j<=3;j=j+1) begin
        byte1[j] = data1[(j+1)*4-1 : j*4];
        //把data1[7:0]…data1[31:24]依次赋值给byte1[0][7:0]…byte[3][7:0]
        end
end
```



此例中，integer 信号 j 作为辅助信号，将 data1 的数据依次赋值给数组 byte1。综合后实际电路里并没有 j 这个信号，j 只是辅助生成相应的硬件电路。

**实数（real）**

实数用关键字 real 来声明，可用十进制或科学计数法来表示。实数声明不能带有范围，默认值为 0。如果将一个实数赋值给一个整数，则只有实数的整数部分会赋值给整数。例如：

```verilog
real        data1 ;
integer     temp ;
initial begin
    data1 = 2e3 ;
    data1 = 3.75 ;
end
 
initial begin
    temp = data1 ; //temp 值的大小为3
end
```



**时间（time）**

Verilog 使用特殊的时间寄存器 time 型变量，对仿真时间进行保存。其宽度一般为 64 bit，通过调用系统函数 $time 获取当前仿真时间。例如：

```verilog
time       current_time ;
initial begin
       #100 ;
       current_time = $time ; //current_time 的大小为 100
end
```



### 数组

在 Verilog 中允许声明 reg, wire, integer, time, real 及其向量类型的数组。

数组维数没有限制。线网数组也可以用于连接实例模块的端口。数组中的每个元素都可以作为一个标量或者向量，以同样的方式来使用，形如：**<数组名>[<下标>]**。对于多维数组来讲，用户需要说明其每一维的索引。例如：

```verilog
integer          flag [7:0] ; //8个整数组成的数组
reg  [3:0]       counter [3:0] ; //由4个4bit计数器组成的数组
wire [7:0]       addr_bus [3:0] ; //由4个8bit wire型变量组成的数组
wire             data_bit[7:0][5:0] ; //声明1bit wire型变量的二维数组
reg [31:0]       data_4d[11:0][3:0][3:0][255:0] ; //声明4维的32bit数据变量数组
```



下面显示了对数组元素的赋值操作：

```verilog
flag [1]   = 32'd0 ; //将flag数组中第二个元素赋值为32bit的0值
counter[3] = 4'hF ;  //将数组counter中第4个元素的值赋值为4bit 十六进制数F，等效于counter[3][3:0] = 4'hF，即可省略宽度;
assign addr_bus[0]        = 8'b0 ; //将数组addr_bus中第一个元素的值赋值为0
assign data_bit[0][1]     = 1'b1;  //将数组data_bit的第1行第2列的元素赋值为1，这里不能省略第二个访问标号，即 assign data_bit[0] = 1'b1; 是非法的。
data_4d[0][0][0][0][15:0] = 15'd3 ;  //将数组data_4d中标号为[0][0][0][0]的寄存器单元的15~0bit赋值为3
```



虽然数组与向量的访问方式在一定程度上类似，但不要将向量和数组混淆。向量是一个单独的元件，位宽为 n；数组由多个元件组成，其中每个元件的位宽为 n 或 1。它们在结构的定义上就有所区别。

### 存储器

存储器变量就是一种寄存器数组，可用来描述 RAM 或 ROM 的行为。例如：

```verilog
reg               membit[0:255] ;  //256bit的1bit存储器
reg  [7:0]        mem[0:1023] ;    //1Kbyte存储器，位宽8bit
mem[511] = 8'b0 ;                  //令第512个8bit的存储单元值为0
```



### 参数

参数用来表示常量，用关键字 parameter 声明，只能赋值一次。例如：

```verilog
parameter      data_width = 10'd32 ;
parameter      i=1, j=2, k=3 ;
parameter      mem_size = data_width * 10 ;
```

但是，通过实例化的方式，可以更改参数在模块中的值。此部分以后会介绍。

局部参数用 localparam 来声明，其作用和用法与 parameter 相同，区别在于它的值不能被改变。所以当参数只在本模块中调用时，可用 localparam 来说明。

### 字符串

字符串保存在 reg 类型的变量中，每个字符占用一个字节（8bit）。因此寄存器变量的宽度应该足够大，以保证不会溢出。

字符串不能多行书写，即字符串中不能包含回车符。如果寄存器变量的宽度大于字符串的大小，则使用 0 来填充左边的空余位；如果寄存器变量的宽度小于字符串大小，则会截去字符串左边多余的数据。例如，为存储字符串 "run.runoob.com", 需要 14*8bit 的存储单元：

```verilog
reg [0: 14*8-1]       str ;
initial begin
    str = "run.runoob.com";
end  
```

**特殊字符**

有一些特殊字符在显示字符串中有特殊意义，例如换行符，制表符等。如果需要在字符串中显示这些特殊的字符，则需要在前面加前缀转义字符 **\** 。例如下表所示：

| 转义字符 | 显示字符            |
| :------- | :------------------ |
| \n       | 换行                |
| \t       | 制表符              |
| %%       | %                   |
| \        | \                   |
| \"       | "                   |
| \ooo     | 1到3个8进制数字字符 |

其实，在 SystemVerilog（主要用于 Verilog 仿真的编程语言）语言中，已经可以直接用关键字 string 来表示字符串变量类型，这为 Verilog 的仿真带来了极大的便利。有兴趣的学者可以简单学习下 SystemVerilog。



## 2.4 Verilog 表达式

### 表达式

表达式由操作符和操作数构成，其目的是根据操作符的意义得到一个计算结果。表达式可以在出现数值的任何地方使用。例如：

```verilog
a^b ;          //a与b进行异或操作
address[9:0] + 10'b1 ;  //地址累加
flag1 && flag2 ;  //逻辑与操作
```

### 操作数

操作数可以是任意的数据类型，只是某些特定的语法结构要求使用特定类型的操作数。

操作数可以为常数，整数，实数，线网，寄存器，时间，位选，域选，存储器及函数调用等。

```verilog
module test;

//实数
real a, b, c;
c = a + b ;

//寄存器
reg  [3:0]       cprmu_1, cprmu_2 ;
always @(posedge clk) begin
        cprmu_2 = cprmu_1 ^ cprmu_2 ;
end
         
//函数
reg  flag1 ;
flag = calculate_result(A, B);
 
//非法操作数
reg [3:0]         res;
wire [3:0]        temp;
always@ （*）begin
    res    = cprmu_2 – cprmu_1 ;
    //temp = cprmu_2 – cprmu_1 ; //不合法，always块里赋值对象不能是wire型
end

endmodule
```

### 操作符

Verilog 中提供了大约 9 种操作符，分别是算术、关系、等价、逻辑、按位、归约、移位、拼接、条件操作符。

大部分操作符与 C 语言中类似。同类型操作符之间，除条件操作符从右往左关联，其余操作符都是自左向右关联。圆括号内表达式优先执行。例如下面每组的 2 种写法都是等价的。

```verilog
A+B-C ;
(A+B）-C ;

A ? B : C ? D : F ;
A ? B : (C ? D : F) ;

(A ? B : C) ? D : F ;
A ? B : C ? D : F ;
```

不同操作符之间，优先级是不同的。下表列出了操作符优先级从高至低的排列顺序。当没有圆括号时，Verilog 会根据操作符优先级对表达式进行计算。为了避免由操作符优先级导致的计算混乱，在不确定优先级时，建议用圆括号将表达式区分开来。

| 操作符       | 操作符号         | 优先级 |
| :----------- | :--------------- | :----- |
| 单目运算     | + - ! ~          | 最高   |
| 乘、除、取模 | * / %            |        |
| 加减         | + -              |        |
| 移位         | <<  >>           |        |
| 关系         | < <= > >=        |        |
| 等价         | == !=  \=== !=== |        |
| 归约         | & ~&             |        |
|              | ^ ~^             |        |
|              | \| ~\|           |        |
| 逻辑         | &&               |        |
|              | \|\|             |        |
| 条件         | ?:               | 最低   |

### 算术操作符

算术操作符包括单目操作符和双目操作符。

双目操作符对 2 个操作数进行算术运算，包括乘（*）、除（/）、加（+）、减（-）、求幂（**）、取模（%）。

```verilog
reg [3:0]  a, b;
reg [4:0]  c ;
a = 4'b0010 ;
b = 4'b1001 ;
c = a+b;        //结果为c=b'b1011
c = a/b;          //结果为c=4，取整
```

如果操作数某一位为 X，则计算结果也会全部出现 X。例如：

```verilog
b = 4'b100x ;
c = a+b ;       //结果为c=4'bxxxx
```

对变量进行声明时，要根据变量的操作符对变量的位宽进行合理声明，不要让结果溢出。上述例子中，相加的 2 个变量位宽为 4bit，那么结果寄存器变量位宽最少为 5bit。否则，高位将被截断，导致结果高位丢失。无符号数乘法时，结果变量位宽应该为 2 个操作数位宽之和。

```verilog
reg [3:0]        mula ;
reg [1:0]        mulb;
reg [5:0]        res ;
mula = 4'he   ;
mulb = 2'h3   ;
res  = mula * mulb ; //结果为res=6'h2a, 数据结果没有丢失位数
```



\+ 和 - 也可以作为单目操作符来使用，表示操作数的正负性。此类操作符优先级最高。

```
-4  //表示负4
+3  //表示正3
```

负数表示时，可以直接在十进制数字前面增加一个减号 **-**，也可以指定位宽。因为负数使用二进制补码来表示，不指定位宽来表示负数，编译器在转换时，会自动分配位宽，从而导致意想不到的结果。例如：

```verilog
mula = -4'd4 ;
mulb = 2 ;
res = mula * mulb ;      //计算结果为res=-6'd8, 即res=6'h38，正常
res = mula * (-'d4) ;    //(4的32次幂-4) * 2, 结果异常
```

### 关系操作符

关系操作符有大于（>），小于（<），大于等于（>=），小于等于（<=）。

关系操作符的正常结果有 2 种，真（1）或假（0）。

如果操作数中有一位为 x 或 z，则关系表达式的结果为 x。

```verilog
A = 4 ;
B = 3 ;
X = 3'b1xx ;
   
A > B     //为真
A <= B    //为假
A >= Z    //为X，不确定
```



### 等价操作符

等价操作符包括逻辑相等（\==），逻辑不等（!=），全等（\=\=\=），非全等（!\==）。

等价操作符的正常结果有 2 种：为真（1）或假（0）。

逻辑相等/不等操作符不能比较 x 或 z，当操作数包含一个 x 或 z，则结果为 x。

`全等比较时，如果按位比较有相同的 x 或 z，返回结果也可以为 1，即全等比较可比较 x 或 z。`所以，全等比较的结果一定不包含 x。举例如下：

```verilog
A = 4 ;
B = 8'h04 ;
C = 4'bxxxx ;
D = 4'hx ;
A == B        //为真
A == (B + 1)  //为假
A == C        //为X，不确定
A === C       //为假，返回值为0
C === D       //为真，返回值为1
```

### 逻辑操作符

逻辑操作符主要有 3 个：&&（逻辑与）, ||（逻辑或），!（逻辑非）。

逻辑操作符的计算结果是一个 1bit 的值，0 表示假，1 表示真，x 表示不确定。

如果一个操作数不为 0，它等价于逻辑 1；如果一个操作数等于 0，它等价于逻辑 0。`如果它任意一位为 x 或 z，它等价于 x`。

`如果任意一个操作数包含 x，逻辑操作符运算结果不一定为 x。`

逻辑操作符的操作数可以为变量，也可以为表达式。例如：

```verilog
A = 3;
B = 0;
C = 2'b1x ;
   
A && B    //     为假
A || B    //     为真
! A       //     为假
! B       //     为真
A && C    //     为X，不确定
A || C    //     为真，因为A为真
(A==2) && (! B)  //为false，此时第一个操作数为表达式
```

### 按位操作符

按位操作符包括：取反（\~），与（&），或（|），异或（\^），同或（\~\^）。

按位操作符对 2 个操作数的每 1bit 数据进行按位操作。

如果 2 个操作数位宽不相等，则用 0 向左扩展补充较短的操作数。

取反操作符只有一个操作数，它对操作数的每 1bit 数据进行取反操作。

下图给出了按位操作符的逻辑规则。

| &(与） | 0    | 1    | x    |      | \|(或) | 0    | 1    | x    |
| :----- | :--- | :--- | :--- | :--- | :----- | :--- | :--- | :--- |
| 0      | 0    | 0    | 0    |      | 0      | 0    | 1    | x    |
| 1      | 0    | 1    | x    |      | 1      | 1    | 1    | 1    |
| x      | 0    | x    | x    |      | x      | x    | 1    | x    |

| ^(异或) | 0    | 1    | x    |      | ~^(同或) | 0    | 1    | x    |
| :------ | :--- | :--- | :--- | :--- | :------- | :--- | :--- | :--- |
| 0       | 0    | 1    | x    |      | 0        | 1    | 0    | x    |
| 1       | 1    | 0    | x    |      | 1        | 0    | 1    | x    |
| x       | x    | x    | x    |      | x        | x    | x    | x    |

```verilog
A = 4'b0101 ;
B = 4'b1001 ;
C = 4'bx010 ;
   
~A        //4'b1010
A & B     //4'b0001
A | B     //4'b1101
A^B       //4'b1100
A ~^ B    //4'b0011
B | C     //4'b1011
B&C       //4'bx000
```

### 归约操作符

归约操作符包括：归约与（&），归约与非（\~&），归约或（|），归约或非（\~|），归约异或（\^），归约同或（\~\^）。

归约操作符只有一个操作数，它对这个向量操作数逐位进行操作，最终产生一个 1bit 结果。

逻辑操作符、按位操作符和归约操作符都使用相同的符号表示，因此有时候容易混淆。区分这些操作符的关键是分清操作数的数目，和计算结果的规则。

```verilog
A = 4'b1010 ;
&A ;      //结果为 1 & 0 & 1 & 0 = 1'b0，可用来判断变量A是否全1
~|A ;     //结果为 ~(1 | 0 | 1 | 0) = 1'b0, 可用来判断变量A是否为全0
^A ;      //结果为 1 ^ 0 ^ 1 ^ 0 = 1'b0
```

### 移位操作符

移位操作符包括左移（<<），右移（>>），算术左移（<<<），算术右移（>>>）。

移位操作符是双目操作符，两个操作数分别表示要进行移位的向量信号（操作符左侧）与移动的位数（操作符右侧）。

算术左移和逻辑左移时，右边低位会补 0。

逻辑右移时，左边高位会补 0；而算术右移时，左边高位会补充符号位，以保证数据缩小后值的正确性。

```verilog
A = 4'b1100 ;
B = 4'b0010 ;
A = A >> 2 ;        //结果为 4'b0011
A = A << 1;         //结果为 4'b1000
A = A <<< 1 ;       //结果为 4'b1000
C = B + (A>>>2);    //结果为 2 + (-4/4) = 1, 4'b0001
```

### 拼接操作符

拼接操作符用大括号 **{，}** 来表示，用于将多个操作数（向量）拼接成新的操作数（向量），信号间用逗号隔开。

拼接符操作数必须指定位宽，常数的话也需要指定位宽。例如：

```verilog
A = 4'b1010 ;
B = 1'b1 ;
Y1 = {B, A[3:2], A[0], 4'h3 };  //结果为Y1='b1100_0011
Y2 = {4{B}, 3'd4};  //结果为 Y2=7'b111_1100
Y3 = {32{1'b0}};  //结果为 Y3=32h0，常用作寄存器初始化时匹配位宽的赋初值
```

### 条件操作符

条件表达式有 3 个操作符，结构描述如下：

```verilog
condition_expression ? true_expression : false_expression
```

计算时，如果 condition_expression 为真（逻辑值为 1），则运算结果为 true_expression；如果 condition_expression 为假（逻辑值为 0），则计算结果为 false_expression。

```verilog
assign hsel    = (addr[9:8] == 2'b0) ? hsel_p1 : hsel_p2 ;
//当信号 addr 高 2bit 为 0 时，hsel 赋值为 hsel_p1; 否则，将 hsel_p2 赋值给 hsel。
```

其实，条件表达式类似于 2 路（或多路）选择器，其描述方式完全可以用 if-else 语句代替。

当然条件操作符也能进行嵌套，完成一个多次选择的逻辑。例如：

```verilog
assign   hsel = (addr[9:8] == 2'b00) ? hsel_p1 :
                (addr[9:8] == 2'b01) ? hsel_p2 :
                (addr[9:8] == 2'b10) ? hsel_p3 :
                (addr[9:8] == 2'b11) ? hsel_p4 ;
```



## 2.5 Verilog 编译指令

### `define

以反引号 **\`** 开始的某些标识符是 Verilog 系统编译指令。

编译指令为 Verilog 代码的撰写、编译、调试等提供了极大的便利。

下面介绍下完整的 8 种编译指令，其中前 4 种使用频率较高。

```verilog
`define, `undef
```

在编译阶段，**`define** 用于文本替换，类似于 C 语言中的 **#define**。

一旦 **`define** 指令被编译，其在整个编译过程中都会有效。例如，在一个文件中定义：

```verilog
`define    DATA_DW     32
```

则在另一个文件中也可以直接使用 DATA_DW。

```verilog
`define    S     $stop;   
//用`S来代替系统函数$stop; (包括分号)
`define    WORD_DEF   reg [31:0]       
//可以用`WORD_DEF来声明32bit寄存器变量
```

**`undef** 用来取消之前的宏定义，例如：

```verilog
`define    DATA_DW     32
……
reg  [DATA_DW-1:0]    data_in   ;
……
`undef DATA_DW

`ifdef, `ifndef, `elsif, `else, `endif
```

这些属于条件编译指令。例如下面的例子中，如果定义了 MCU51，则使用第一种参数说明；如果没有定义 MCU、定义了 WINDOW，则使用第二种参数说明；如果 2 个都没有定义，则使用第三种参数说明。

```verilog
`ifdef       MCU51
    parameter DATA_DW = 8   ;
`elsif       WINDOW
    parameter DATA_DW = 64  ;
`else
    parameter DATA_DW = 32  ;
`endif
```



当然，也可用 **`ifndef** 来设置条件编译，表示如果没有相关的宏定义，则执行相关语句。

下面例子中，如果定义了 WINDOW，则使用第二种参数说明。如果没有定义 WINDOW，则使用第一种参数说明。

```verilog
`ifndef     WINDOW
    parameter DATA_DW = 32 ;  
 `else
    parameter DATA_DW = 64 ;
 `endif
```



### `include

使用 **`include** 可以在编译时将一个 Verilog 文件内嵌到另一个 Verilog 文件中，作用类似于 C 语言中的 #include 结构。该指令通常用于将全局或公用的头文件包含在设计文件里。

文件路径既可以使用相对路径，也可以使用绝对路径。

```verilog
`include         "../../param.v"
`include         "header.v"
```

### `timescale

在 Verilog 模型中，时延有具体的单位时间表述，并用 **`timescale** 编译指令将时间单位与实际时间相关联。

该指令用于定义时延、仿真的单位和精度，格式为：

```verilog
`timescale      time_unit / time_precision
```

time_unit 表示时间单位，time_precision 表示时间精度，它们均是由数字以及单位 s（秒），ms（毫秒），us（微妙），ns（纳秒），ps（皮秒）和 fs（飞秒）组成。时间精度可以和时间单位一样，但是时间精度大小不能超过时间单位大小，例如下面例子中，输出端 Z 会延迟 5.21ns 输出 A&B 的结果。

```verilog
`timescale 1ns/100ps    //时间单位为1ns，精度为100ps，合法
//`timescale 100ps/1ns  //不合法
module AndFunc(Z, A, B);
    output Z;
    input A, B ;
    assign #5.207 Z = A & B
endmodule
```

在编译过程中，==**timescale** 指令会影响后面所有模块中的时延值==，直至遇到另一个 **timescale** 指令或 **resetall** 指令。

由于在 Verilog 中没有默认的 **timescale**，如果没有指定 **timescale**，Verilog 模块就有会继承前面编译模块的 **timescale** 参数。有可能导致设计出错。

如果一个设计中的多个模块都带有 **`timescale** 时，模拟器总是定位在所有模块的最小时延精度上，并且所有时延都相应地换算为最小时延精度，时延单位并不受影响。例如:

```verilog
`timescale 10ns/1ns      
module test;
    reg        A, B ;
    wire       OUTZ ;
 
    initial begin
        A     = 1;
        B     = 0;
        # 1.28    B = 1;
        # 3.1     A = 0;
    end
 
    AndFunc        u_and(OUTZ, A, B) ;
endmodule
```

在模块 AndFunc 中，5.207 对应 5.21ns。

在模块 test 中，1.28 对应 13ns，3.1 对应 31ns。

但是，当仿真 test 时，由于 AndFunc 中的最小精度为 100ps，因此 test 中的时延精度将进行重新调整。13ns 将对应 130\*100ps，31ns 将对应 310\*100ps。仿真时，时延精度也会使用 100ps。仿真时间单位大小没有影响。

如果有并行子模块，子模块间的 `timescale 并不会相互影响。

例如在模块 test 中再例化一个子模块 OrFunc。仿真 test 时，OrFunc 中的 #5.207 延时依然对应 52ns。

```verilog
//子模块：
`timescale 10ns/1ns      //时间单位为1ns，精度为100ps，合法
module OrFunc(Z, A, B);
    output Z;
    input A, B ;
    assign #5.207 Z = A | B
endmodule
 
//顶层模块：
`timescale 10ns/1ns      
module test;
    reg        A, B ;
    wire       OUTZ ;
    wire       OUTX ;
 
    initial begin
        A     = 1;
        B     = 0;
        # 1.28    B = 1;
        # 3.1     A = 0;
    end
 
    AndFunc        u_and(OUTZ, A, B) ;
    OrFunc         u_and(OUTX, A, B) ;
 
endmodule
```

此例中，仿真 test 时，OrFunc 中的 #5.207 延时依然对应 52ns。

**`timescale** 的时间精度设置是会影响仿真时间的。时间精度越小，仿真时占用内存越多，实际使用的仿真时间就越长。所以如果没有必要，应尽量将时间精度设置的大一些。

### `default_nettype

该指令用于为隐式的线网变量指定为线网类型，即将没有被声明的连线定义为线网类型。

```verilog
`default_nettype wand 
```

该实例定义的缺省的线网为线与类型。因此，如果在此指令后面的任何模块中的连线没有说明，那么该线网被假定为线与类型。

```verilog
`default_nettype none
```

该实例定义后，将不再自动产生 wire 型变量。

例如下面第一种写法编译时不会报 Error，第二种写法编译将不会通过。

```verilog
//Z1 无定义就使用，系统默认Z1为wire型变量，有 Warning 无 Error
module test_and(
        input      A,
        input      B,
        output     Z);
    assign Z1 = A & B ;  
endmodule
```



```verilog
//Z1无定义就使用，由于编译指令的存在，系统会报Error，从而检查出书写错误
`default_nettype none
module test_and(
        input      A,
        input      B,
        output     Z);
    assign Z1 = A & B ;  
endmodule
```



### `resetall

该编译器指令将所有的编译指令重新设置为缺省值。

**`resetall** 可以使得缺省连线类型为线网类型。

当 **resetall** 加到模块最后时，可以将当前的 **timescale** 取消防止进一步传递，只保证当前的 **timescale** 在局部有效，避免 `timescale 的错误继承。

### celldefine, \`endcelldefine

这两个程序指令用于将模块标记为单元模块，他们包含模块的定义。例如一些与、或、非门，一些 PLL 单元，PAD 模型，以及一些 Analog IP 等。

```verilog
`celldefine
module (
    input      clk,
    input      rst,
    output     clk_pll,
    output     flag);
        ……
endmodule
`endcelldefine
```



### unconnected_drive, `nounconnected_drive

在模块实例化中，出现在这两个编译指令间的任何未连接的输入端口，为正偏电路状态或者为反偏电路状态。

```verilog
`unconnected_drive pull1
. . .
 / *在这两个程序指令间的所有未连接的输入端口为正偏电路状态（连接到高电平） * /
`nounconnected_drive
`unconnected_drive pull0
. . .
 / *在这两个程序指令间的所有未连接的输入端口为反偏电路状态（连接到低电平） * /
`nounconnected_drive 
 
```

# 3. Verilog赋值与时延 

## 3.1 Verilog 连续赋值

**关键词：assign， 全加器**

连续赋值语句是 Verilog 数据流建模的基本语句，用于对 `wire` 型变量进行赋值。：

格式如下

```verilog
assign     LHS_target = RHS_expression  ；
```

LHS（left hand side） 指赋值操作的左侧，RHS（right hand side）指赋值操作的右侧。

assign 为关键词，任何已经声明 wire 变量的连续赋值语句都是以 assign 开头，例如：

```verilog
wire      Cout, A, B ;
assign    Cout  = A & B ;     //实现计算A与B的功能
```

需要说明的是：

- ==LHS_target 必须是一个标量或者线型向量，而不能是寄存器类型。==
- RHS_expression 的类型没有要求，可以是标量或线型或存器向量，也可以是函数调用。
- 只要 RHS_expression 表达式的操作数有事件发生（值的变化）时，RHS_expression 就会立刻重新计算，同时赋值给 LHS_target。

Verilog 还提供了另一种对 wire 型赋值的简单方法，即在 wire 型变量声明的时候同时对其赋值。wire 型变量只能被赋值一次，因此该种连续赋值方式也只能有一次。例如下面赋值方式和上面的赋值例子的赋值方式，效果都是一致的。

```verilog
wire      A, B ;
wire      Cout = A & B ;
```

### 全加器

下面采用数据流描述方式，来设计一个 1bit 全加器。

设 Ai，Bi，Ci 分别为被加数、加数和相邻低位的进位数，So, Co 分别为本位和与向相邻高位的进位数。

全加器的表达式为：

```
So = Ai ⊕ Bi ⊕ Ci ;
Co = AiBi + Ci(Ai+Bi)
```

rtl 代码（full_adder1.v）如下：

```verilog
module full_adder1(
    input    Ai, Bi, Ci,
    output   So, Co);
 
    assign So = Ai ^ Bi ^ Ci ;
    assign Co = (Ai & Bi) | (Ci & (Ai | Bi));
endmodule
```

当然，更为贴近加法器的代码描述可以为：

```verilog
module full_adder1(
    input    Ai, Bi, Ci
    output   So, Co);
 
    assign {Co, So} = Ai + Bi + Ci ;
endmodule
```

testbench（test.sv）参考如下：

```verilog
`timescale 1ns/1ns
 
module test ;
    reg Ai, Bi, Ci ;
    wire So, Co ;
 
    initial begin
        {Ai, Bi, Ci}      = 3'b0;
        forever begin
            #10 ;
            {Ai, Bi, Ci}      = {Ai, Bi, Ci} + 1'b1;
        end
    end
 
    full_adder1  u_adder(
        .Ai      (Ai),
        .Bi      (Bi),
        .Ci      (Ci),
        .So      (So),
        .Co      (Co));
 
    initial begin
        forever begin
            #100;
            //$display("---gyc---%d", $time);
            if ($time >= 1000) begin
            $finish ;
            end
        end
    end
 
 endmodule
```

## 3.2 Verilog 时延

**关键词：时延， 惯性时延**

> 连续赋值延时语句中的延时，用于控制任意操作数发生变化到语句左端赋予新值之间的时间延时。
>
> `类似于 右侧表达式结果，在传递到左侧wire耗费的时间`

时延一般是不可综合的。

寄存器的时延也是可以控制的，这部分在时序控制里加以说明。

连续赋值时延一般可分为普通赋值时延、隐式时延、声明时延。

下面 3 个例子实现的功能是等效的，分别对应 3 种不同连续赋值时延的写法。

*//普通时延，A&B计算结果延时10个时间单位赋值给Z*
**wire** Z, A, B ;
**assign** #10   Z = A & B ; 
*//隐式时延，声明一个wire型变量时对其进行包含一定时延的连续赋值。*
**wire** A, B;
**wire** #10     Z = A & B;

*//声明时延，声明一个wire型变量是指定一个时延。因此对该变量所有的连续赋值都会被推迟到指定的时间。除非门级建模中，一般不推荐使用此类方法建模。*
**wire** A, B;
**wire** #10 Z ;
**assign**      Z =A & B

```verilog
// Testbench
module test;
  reg A,B;
  wire Z;
  assign #5   Z = A & B ; 
  
  initial begin
    A=0;
    B=0;
    $display("0 A,B,Z \n",A,,B,,Z);
    #4;
    A=1;
    $display("4 A,B,Z \n",A,,B,,Z);
    #5;
    B=1;
    $display("9 A,B,Z \n",A,,B,,Z);
  end
endmodule
```

> \# KERNEL: 0 A,B,Z 
> \# KERNEL: 0 0 x
> \# KERNEL: 4 A,B,Z 
> \# KERNEL: 1 0 x
> \# KERNEL: 9 A,B,Z 
> \# KERNEL: 1 1 0

### 惯性时延

在上述例子中，A 或 B 任意一个变量发生变化，那么在 Z 得到新的值之前，会有 10 个时间单位的时延。所以称之为惯性时延，==即信号脉冲宽度小于时延时，对输出没有影响==。

> 因此仿真时，时延一定要合理设置，防止某些信号不能进行有效的延迟。

对一个有延迟的与门逻辑进行时延仿真。

```verilog
module time_delay_module(
    input   ai, bi,
    output  so_lose, so_get, so_normal);
 
    assign #20      so_lose      = ai & bi ;
    assign  #5      so_get       = ai & bi ;
    assign          so_normal    = ai & bi ;
endmodule
```

testbench 参考如下:

```verilog
`timescale 1ns/1ns

module test ;
    reg  ai, bi ;
    wire so_lose, so_get, so_normal ;
 
    initial begin
        ai        = 0 ;
        #25 ;      ai        = 1 ;
        #35 ;      ai        = 0 ;        //60ns
        #40 ;      ai        = 1 ;        //100ns
        #10 ;      ai        = 0 ;        //110ns
    end
 
    initial begin
        bi        = 1 ;
        #70 ;      bi        = 0 ;
        #20 ;      bi        = 1 ;
    end
 
    time_delay_module  u_wire_delay(
        .ai              (ai),
        .bi              (bi),
        .so_lose         (so_lose),
        .so_get          (so_get),
        .so_normal       (so_normal));
 
    initial begin
        forever begin
            #100;
            //$display("---gyc---%d", $time);
            if ($time >= 1000) begin
                $finish ;
            end
        end
    end
 
endmodule
```

仿真结果如下:

信号 so_normal 为正常的与逻辑。

由于所有的时延均大于 5ns，所以信号 so_get 的结果为与操作后再延迟 5ns 的结果。

信号 so_lose 前一段是与操作后再延迟 20ns 的结果。

由于信号 ai 第二个高电平持续时间小于 20ns，so_lose 信号会因惯性时延而漏掉对这个脉冲的延时检测，所以后半段 so_lose 信号仍然为 0。

# 4. Verilog 过程结构

## 4.1 Verilog 过程结构

关键词：initial， always

过程结构语句有 2 种，initial 与 always 语句。它们是行为级建模的 2 种基本语句。

- 一个模块中可以包含多个 initial 和 always 语句，但 2 种语句不能嵌套使用。

- 这些语句在模块间并行执行，与其在模块的前后顺序没有关系。

- 但是 initial 语句或 always 语句内部可以理解为是顺序执行的（非阻塞赋值除外）。

- 每个 initial 语句或 always 语句都会产生一个独立的控制流，执行时间都是从 0 时刻开始。


### initial语句

initial 语句从 0 时刻开始执行，只执行一次，多个 initial 块之间是相互独立的。

- 如果 initial 块内包含多个语句，需要使用关键字 begin 和 end 组成一个块语句。

- 如果 initial 块内只要一条语句，关键字 begin 和 end 可使用也可不使用。


`initial 理论上来讲是不可综合的，多用于初始化、信号检测等。`

对上一节代码稍作修改，进行仿真，代码如下。

```verilog
`timescale 1ns/1ns
 
module test ;
    reg  ai, bi ;
 
    initial begin
        ai         = 0 ;
        #25 ;      ai        = 1 ;
        #35 ;      ai        = 0 ;        //absolute 60ns
        #40 ;      ai        = 1 ;        //absolute 100ns
        #10 ;      ai        = 0 ;        //absolute 110ns
    end
 
    initial begin
        bi         = 1 ;
        #70 ;      bi        = 0 ;        //absolute 70ns
        #20 ;      bi        = 1 ;        //absolute 90ns
    end
 
    //at proper time stop the simulation
    initial begin
        forever begin
            #100;
            //$display("---gyc---%d", $time);
            if ($time >= 1000) begin
                $finish ;
            end
        end
   end
 
endmodule
```

仿真结果如下:

可以看出，2 个 initial 进程语句分别给信号 ai，bi 赋值时，相互间并没有影响。

信号 ai，bi 的值按照赋值顺序依次改变，所以 initial 内部语句也可以看做是顺序执行。

### always 语句

与 initial 语句相反，always 语句是重复执行的。always 语句块从 0 时刻开始执行其中的行为语句；当执行完最后一条语句后，便再次执行语句块中的第一条语句，如此循环反复。

由于循环执行的特点，always 语句多用于仿真时钟的产生，信号行为的检测等。

下面用 always 产生一个 100MHz 时钟源，并在 1010ns 时停止仿真代码如下。

代码如下:

```verilog
`timescale 1ns/1ns
 
module test ;
 
    parameter CLK_FREQ   = 100 ; //100MHz
    parameter CLK_CYCLE  = 1e9 / (CLK_FREQ * 1e6) ;   //switch to ns
 
    reg  clk ;
    initial      clk = 1'b0 ;      //clk is initialized to "0"
    always     # (CLK_CYCLE/2) clk = ~clk ;       //generating a real clock by reversing
 
    always begin
        #10;
        if ($time >= 1000) begin
            $finish ;
        end
    end
 
endmodule
```

## 4.2 Verilog 过程赋值

关键词：阻塞赋值，非阻塞赋值，并行

过程性赋值是在 initial 或 always 语句块里的赋值，==赋值对象是寄存器、整数、实数等类型。==

这些变量在被赋值后，其值将保持不变，直到重新被赋予新值。

连续性赋值总是处于激活状态，任何操作数的改变都会影响表达式的结果；过程赋值只有在语句执行的时候，才会起作用。这是连续性赋值与过程性赋值的区别。

Verilog 过程赋值包括 2 种语句：阻塞赋值与非阻塞赋值。

### 阻塞赋值

阻塞赋值属于顺序执行，即下一条语句执行前，当前语句一定会执行完毕。

阻塞赋值语句使用等号 **=** 作为赋值符。

前面的仿真中，initial 里面的赋值语句都是用的阻塞赋值。

### 非阻塞赋值

非阻塞赋值属于并行执行语句，即下一条语句的执行和当前语句的执行是同时进行的，它不会阻塞位于同一个语句块中后面语句的执行。

非阻塞赋值语句使用小于等于号 **<=** 作为赋值符。

利用下面代码，对阻塞、非阻塞赋值进行仿真，来说明 2 种过程赋值的区别。

```verilog
`timescale 1ns/1ns
 
module test ;
    reg [3:0]   ai, bi ;
    reg [3:0]   ai2, bi2 ;
    reg [3:0]   value_blk ;
    reg [3:0]   value_non ;
    reg [3:0]   value_non2 ;
 
    initial begin
        ai            = 4'd1 ;   //(1)
        bi            = 4'd2 ;   //(2)
        ai2           = 4'd7 ;   //(3)
        bi2           = 4'd8 ;   //(4)
        #20 ;                    //(5)
 
        //non-block-assigment with block-assignment
        ai            = 4'd3 ;     //(6)
        bi            = 4'd4 ;     //(7)
        value_blk     = ai + bi ;  //(8)
        value_non     <= ai + bi ; //(9)
 
        //non-block-assigment itself
        ai2           <= 4'd5 ;           //(10)
        bi2           <= 4'd6 ;           //(11)
        value_non2    <= ai2 + bi2 ;      //(12)
    end
 
   //stop the simulation
    always begin
        #10 ;
        if ($time >= 1000) $finish ;
    end
 
endmodule
```

仿真结果如下：

语句（1）-（8）都是阻塞赋值，按照顺序执行。

20ns 之前，信号 ai，bi 值改变。由于过程赋值的特点，value_blk = ai + bi 并没有执行到，所以 20ns 之前，value_blk 值为 X（不确定状态）。

20ns 之后，信号 ai，bi 值再次改变。执行到 value_blk = ai + bi，信号 value_blk 利用信号 ai，bi 的新值得到计算结果 7。

语句（9）-（12）都是非阻塞赋值，并行执行。

首先，（9）-（12）虽然都是并发执行，但是执行顺序也是在（8）之后，所以信号 value_non = ai + bi 计算是也会使用信号 ai，bi 的新值，结果为 7。

其次，（10）-（12）是并发执行，所以 value_non2 = ai2 + bi2 计算时，并不关心信号 ai2，bi2 的最新非阻塞赋值结果。即 value_non2 计算时使用的是信号 ai2，bi2 的旧值，结果为 4'hF。



### 使用非阻塞赋值避免竞争冒险

上述仿真代码只是为了让读者更好的理解阻塞赋值与非阻塞赋值的区别。实际 Verilog 代码设计时，切记不要在一个过程结构中混合使用阻塞赋值与非阻塞赋值。两种赋值方式混用时，时序不容易控制，很容易得到意外的结果。

更多时候，在设计电路时，==always 时序逻辑块中多用非阻塞赋值，always 组合逻辑块中多用阻塞赋值==；在仿真电路时，initial 块中一般多用阻塞赋值。

如下所示，为实现在时钟上升沿交换 2 个寄存器值的功能，在 2 个 always 块中使用阻塞赋值。

因为 2 个 always 块中的语句是同时进行的，但是 a=b 与 b=a 是无法判定执行顺序的，这就造成了竞争的局面。

但不管哪个先执行（和编译器等有关系），不考虑 timing 问题时，他们执行顺序总有先后，最后 a 与 b 的值总是相等的。没有达到交换 2 个寄存器值的效果。

```
always @(posedge clk) begin
    a = b ;
end
 
always @(posedge clk) begin
    b = a;
end
```

但是，如果在 always 块中使用非阻塞赋值，则可以避免上述竞争冒险的情况。

如下所示，2 个 always 块中语句并行执行，赋值操作右端操作数使用的是上一个时钟周期的旧值，此时 a<=b 与 b<=a 就可以相互不干扰的执行，达到交换寄存器值的目的。

```
always @(posedge clk) begin
    a <= b ;
end
 
always @(posedge clk) begin
    b <= a;
end
```



当然，利用下面代码也可以实现交换寄存器值的功能，但是显然不如在 always 块中直接用非阻塞赋值简单直观。

```
always @(posedge clk) begin
    temp    = a ;
    a       = b ;
    b       = temp ;
end
```

## 4.3 Verilog 时序控制

关键词：时延控制，事件触发，边沿触发，电平触发

Verilog 提供了 2 大类时序控制方法：时延控制和事件控制。事件控制主要分为边沿触发事件控制与电平敏感事件控制。

### 时延控制

基于时延的时序控制出现在表达式中，它指定了语句从开始执行到执行完毕之间的时间间隔。

时延可以是数字、标识符或者表达式。

根据在表达式中的位置差异，时延控制又可以分为常规时延与内嵌时延。

**常规时延**

遇到常规延时时，`该语句需要等待一定时间`，然后将计算结果赋值给目标信号。

格式为：#delay procedural_statement，例如：

```verilog
reg  value_test ;
reg  value_general ;
#10  value_general    = value_test ;
```

该时延方式的另一种写法是直接将井号 **#** 独立成一个时延执行语句，例如：

```verilog
#10 ;
value_ single         = value_test ;
```

**内嵌时延**

遇到内嵌延时时，该语句`先将计算结果保存`，然后等待一定的时间后赋值给目标信号。

内嵌时延控制加在赋值号之后。例如：

```verilog
reg  value_test ;
reg  value_embed ;
value_embed        = #10 value_test ;
```

需要说明的是，这 2 种时延控制方式的效果是有所不同的。

当延时语句的赋值符号右端是常量时，2 种时延控制都能达到相同的延时赋值效果。

当延时语句的赋值符号右端是变量时，2 种时延控制可能会产生不同的延时赋值效果。

例如下面仿真代码：

```verilog
`timescale 1ns/1ns
 
module test ;
    reg  value_test ;
    reg  value_general, value_embed, value_single ;
 
    //signal source
    initial begin
        value_test        = 0 ;
        #25 ;      value_test        = 1 ;
        #35 ;      value_test        = 0 ;        //absolute 60ns
        #40 ;      value_test        = 1 ;        //absolute 100ns
        #10 ;      value_test        = 0 ;        //absolute 110ns
    end
 
    //(1)general delay control
    initial begin
        value_general     = 1;
        #10 value_general  = value_test ; //10ns, value_test=0
        #45 value_general  = value_test ; //55ns, value_test=1
        #30 value_general  = value_test ; //85ns, value_test=0
        #20 value_general  = value_test ; //105ns, value_test=1
    end
 
    //(2)embedded delay control
    initial begin
        value_embed       = 1;
        value_embed  = #10 value_test ; //0ns, value_test=0
        value_embed  = #45 value_test ; //10ns, value_test=0
        value_embed  = #30 value_test ; //55ns, value_test=1
        value_embed  = #20 value_test ; //85ns, value_test=0
    end
 
    //(3)single delay control
    initial begin
        value_single      = 1;
        #10 ;
        value_single = value_test ; //10ns, value_test=0
        #45 ;
        value_single = value_test ; //55ns, value_test=1
        #30 ;
        value_single = value_test ; //85ns, value_test=0
        #20 ;
        value_single = value_test ; //105ns, value_test=1
    end
 
    always begin
        #10;
        if ($time >= 150) begin
            $finish ;
        end
    end
 
endmodule

```



- （1）一般延时的两种表达方式执行的结果都是一致的。
- （2）一般时延赋值方式：遇到延迟语句后先延迟一定的时间，然后将当前操作数赋值给目标信号，并没有"惯性延迟"的特点，不会漏掉相对较窄的脉冲。
- （3）内嵌时延赋值方式：遇到延迟语句后，先计算出表达式右端的结果，然后再延迟一定的时间，赋值给目标信号。



### 边沿触发事件控制

在 Verilog 中，事件是指某一个 reg 或 wire 型变量发生了值的变化。

基于事件触发的时序控制又主要分为以下几种。

**一般事件控制**

事件控制用符号 **@** 表示。

语句执行的条件是信号的值发生特定的变化。

关键字 posedge 指信号发生边沿正向跳变，negedge 指信号发生负向边沿跳变，未指明跳变方向时，则 2 种情况的边沿变化都会触发相关事件。例如：

```verilog
//信号clk只要发生变化，就执行q<=d，双边沿D触发器模型
always @(clk) q <= d ;                
//在信号clk上升沿时刻，执行q<=d，正边沿D触发器模型
always @(posedge clk) q <= d ;  
//在信号clk下降沿时刻，执行q<=d，负边沿D触发器模型
always @(negedge clk) q <= d ;
//立刻计算d的值，并在clk上升沿时刻赋值给q，不推荐这种写法
q = @(posedge clk) d ;      
```

**命名事件控制**

用户可以声明 event（事件）类型的变量，并触发该变量来识别该事件是否发生。命名事件用关键字 event 来声明，触发信号用 **->** 表示。例如：

```verilog
event     start_receiving ;
always @( posedge clk_samp) begin
        -> start_receiving ;       //采样时钟上升沿作为时间触发时刻
end
 
always @(start_receiving) begin
    data_buf = {data_if[0], data_if[1]} ; //触发时刻，对多维数据整合
end
```

**敏感列表**

当多个信号或事件中任意一个发生变化都能够触发语句的执行时，Verilog 中使用"或"表达式来描述这种情况，用关键字 **or** 连接多个事件或信号。这些事件或信号组成的列表称为"敏感列表"。当然，or 也可以用逗号 **,** 来代替。例如：

```verilog
//带有低有效复位端的D触发器模型
always @(posedge clk or negedge rstn)    begin      
//always @(posedge clk , negedge rstn)    begin      
//也可以使用逗号陈列多个事件触发
    if（! rstn）begin
        q <= 1'b ;      
    end
    else begin
        q <= d ;
    end
end
```

当组合逻辑输入变量很多时，那么编写敏感列表会很繁琐。此时，更为简洁的写法是 **@\*** 或 **@(\*)**，表示对语句块中的所有输入变量的变化都是敏感的。例如：

```verilog
always @(*) begin
//always @(a, b, c, d, e, f, g, h, i, j, k, l, m) begin
//两种写法等价
    assign s = a? b+c : d ? e+f : g ? h+i : j ? k+l : m ;
end
```

### 电平敏感事件控制

前面所讨论的事件控制都是需要等待信号值的变化或事件的触发，使用 **@+敏感列表** 的方式来表示的。

Verilog 中还支持使用电平作为敏感信号来控制时序，即后面语句的执行需要等待某个条件为真。Verilog 中使用关键字 wait 来表示这种电平敏感情况。例如：



```verilog
initial begin
    wait (start_enable) ;      //等待 start 信号
    forever begin
        //start信号使能后，在clk_samp上升沿，对数据进行整合
        @(posedge clk_samp)  ;
        data_buf = {data_if[0], data_if[1]} ;      
    end
end
```

## 4.4 Verilog 语句块

关键词：顺序块，并行块，嵌套块，命名块，disable

Verilog 语句块提供了将两条或更多条语句组成语法结构上相当于一条一句的机制。主要包括两种类型：顺序块和并行块。

### 顺序块

顺序块用关键字 begin 和 end 来表示。

顺序块中的语句是一条条执行的。当然，非阻塞赋值除外。

顺序块中每条语句的时延总是与其前面语句执行的时间相关。

在本节之前的仿真中，initial 块中的阻塞赋值，都是顺序块的实例。

### 并行块

并行块有关键字 fork 和 join 来表示。

并行块中的语句是并行执行的，即便是阻塞形式的赋值。

并行块中每条语句的时延都是与块语句开始执行的时间相关。

顺序块与并行块的区别显而易见，下面用仿真说明。

仿真代码如下:

```verilog
`timescale 1ns/1ns
 
module test ;
    reg [3:0]   ai_sequen, bi_sequen ;
    reg [3:0]   ai_paral,  bi_paral ;
    reg [3:0]   ai_nonblk, bi_nonblk ;
 
 //============================================================//
    //(1)Sequence block
    initial begin
        #5 ai_sequen         = 4'd5 ;    //at 5ns
        #5 bi_sequen         = 4'd8 ;    //at 10ns
    end
    //(2)fork block
    initial fork
        #5 ai_paral          = 4'd5 ;    //at 5ns
        #5 bi_paral          = 4'd8 ;    //at 5ns
    join
    //(3)non-block block
    initial fork
        #5 ai_nonblk         <= 4'd5 ;    //at 5ns
        #5 bi_nonblk         <= 4'd8 ;    //at 5ns
    join
 
endmodule
```

仿真结果如下: 顺序块顺序执行，第 10ns 时，信号 bi_sequen 才赋值为 8。 而并行块，ai_paral 与 bi_paral 的赋值是同时执行的，所以均在 5ns 时被赋值。而非阻塞赋值，也能达到和并行块同等的赋值效果。

### 嵌套块

顺序块和并行块还可以嵌套使用。

仿真代码如下:

```
`timescale      1ns/1ns
 
module test ;
 
    reg [3:0]   ai_sequen2, bi_sequen2 ;
    reg [3:0]   ai_paral2,  bi_paral2 ;
    initial begin
        ai_sequen2         = 4'd5 ;    //at 0ns
        fork
            #10 ai_paral2          = 4'd5 ;    //at 10ns
            #15 bi_paral2          = 4'd8 ;    //at 15ns
        join
        #20 bi_sequen2      = 4'd8 ;    //at 35ns
    end
 
endmodule
```

仿真结果如下:

并行块语句块内是并行执行，所以信号 ai_paral2 和信号 bi_paral2 分别在 10ns, 15ns 时被赋值。而并行块中最长的执行时间为 15ns，所以顺序块中的信号 bi_sequen2 在 35ns 时被赋值。

### 命名块

我们可以给块语句结构命名。

命名的块中可以声明局部变量，通过层次名引用的方法对变量进行访问。

仿真代码如下:

```verilog
`timescale 1ns/1ns
 
module test;
 
    initial begin: runoob   //命名模块名字为runoob，分号不能少
        integer    i ;       //此变量可以通过test.runoob.i 被其他模块使用
        i = 0 ;
        forever begin
            #10 i = i + 10 ;      
        end
    end
 
    reg stop_flag ;
    initial stop_flag = 1'b0 ;
    always begin : detect_stop
        if ( test.runoob.i == 100) begin //i累加10次，即100ns时停止仿真
            $display("Now you can stop the simulation!!!");
            stop_flag = 1'b1 ;
        end
        #10 ;
    end
 
endmodule
```

命名的块也可以被禁用，用关键字 disable 来表示。

disable 可以终止命名块的执行，可以用来从循环中退出、处理错误等。

与 C 语言中 break 类似，但是 break 只能退出当前所在循环，而 disable 可以禁用设计中任何一个命名的块。

仿真代码如下:

```verilog
`timescale 1ns/1ns
 
module test;
 
    initial begin: runoob_d //命名模块名字为runoob_d
        integer    i_d ;
        i_d = 0 ;
        while(i_d<=100) begin: runoob_d2
            # 10 ;
            if (i_d >= 50) begin       //累加5次停止累加
                disable runoob_d3.clk_gen ;//stop 外部block: clk_gen
                disable runoob_d2 ;       //stop 当前block: runoob_d2
            end
            i_d = i_d + 10 ;
        end
    end
 
    reg clk ;
    initial begin: runoob_d3
        while (1) begin: clk_gen  //时钟产生模块
            clk=1 ;      #10 ;
            clk=0 ;      #10 ;
        end
    end
 
endmodule
```

仿真结果如下:

由图可知，信号 i_d 累加到 50 以后，便不再累加，以后 clk 时钟也不再产生。

可见，disable 退出了当前的 while 块。

需要说明的是，disable 在 always 或 forever 块中使用时只能退出当前回合，下一次语句还是会在 always 或 forever 中执行。因为 always 块和 forever 块是一直执行的，此时的 disable 有点类似 C 语言中的 continue 功能。

## 4.5 Verilog 条件语句

关键词：if，选择器

### 条件语句

条件（if）语句用于控制执行语句要根据条件判断来确定是否执行。

条件语句用关键字 if 和 else 来声明，条件表达式必须在圆括号中。

条件语句使用结构说明如下：

```
if (condition1)       true_statement1 ;
else if (condition2)        true_statement2 ;
else if (condition3)        true_statement3 ;
else                      default_statement ;
```

- if 语句执行时，如果 condition1 为真，则执行 true_statement1 ；如果 condition1 为假，condition2 为真，则执行 true_statement2；依次类推。
- else if 与 else 结构可以省略，即可以只有一个 if 条件判断和一组执行语句 ture_statement1 就可以构成一个执行过程。
- else if 可以叠加多个，不仅限于 1 或 2 个。
- ture_statement1 等执行语句可以是一条语句，也可以是多条。如果是多条执行语句，则需要用 begin 与 end 关键字进行说明。

下面代码实现了一个 4 路选择器的功能。

```verilog
module mux4to1(
    input [1:0]     sel ,
    input [1:0]     p0 ,
    input [1:0]     p1 ,
    input [1:0]     p2 ,
    input [1:0]     p3 ,
    output [1:0]    sout);

    reg [1:0]     sout_t ;

    always @(*) begin
        if (sel == 2'b00)
            sout_t = p0 ;
        else if (sel == 2'b01)
            sout_t = p1 ;
        else if (sel == 2'b10)
            sout_t = p2 ;
        else
            sout_t = p3 ;
    end
    assign sout = sout_t ;
 
endmodule
```

testbench 代码如下：

```verilog
`timescale 1ns/1ns

module test ;
    reg [1:0]    sel ;
    wire [1:0]   sout ;

    initial begin
        sel       = 0 ;
        #10 sel   = 3 ;
        #10 sel   = 1 ;
        #10 sel   = 0 ;
        #10 sel   = 2 ;
    end

    mux4to1 u_mux4to1 (
        .sel    (sel),
        .p0     (2'b00),        //path0 are assigned to 0
        .p1     (2'b01),        //path1 are assigned to 1
        .p2     (2'b10),        //path2 are assigned to 2
        .p3     (2'b11),        //path3 are assigned to 3
        .sout   (sout));
   //finish the simulation
    always begin
        #100;
        if ($time >= 1000) $finish ;
    end
endmodule
```

事例中 if 条件每次执行的语句只有一条，没有使用 begin 与 end 关键字。但如果是 if-if-else 的形式，即便执行语句只有一条，不使用 begin 与 end 关键字也会引起歧义。

例如下面代码，虽然格式上加以区分，但是 else 对应哪一个 if 条件，是有歧义的。

```verilog
if(en)
    if(sel == 2'b1)
        sout = p1s ;
    else
        sout = p0 ;
```

当然，编译器一般按照就近原则，使 else 与最近的一个 if（例子中第二个 if）相对应。

但显然这样的写法是不规范且不安全的。

所以条件语句中加入 begin 与 and 关键字就是一个很好的习惯。

例如上述代码稍作修改，就不会再有书写上的歧义。

```verilog
if(en) begin
    if(sel == 2'b1) begin
        sout = p1s ;
    end
    else begin
        sout = p0 ;
    end
end
```

## 4.6 Verilog 多路分支语句

关键词：case，选择器

case 语句是一种多路条件分支的形式，可以解决 if 语句中有多个条件选项时使用不方便的问题。

### case 语句

case 语句格式如下：

```
case(case_expr)
    condition1     :             true_statement1 ;
    condition2     :             true_statement2 ;
    ……
    default        :             default_statement ;
endcase
```

case 语句执行时，如果 condition1 为真，则执行 true_statement1 ; 如果 condition1 为假，condition2 为真，则执行 true_statement2；依次类推。如果各个 condition 都不为真，则执行 default_statement 语句。

default 语句是可选的，且在一个 case 语句中不能有多个 default 语句。

条件选项可以有多个，不仅限于 condition1、condition2 等，而且这些条件选项不要求互斥。虽然这些条件选项是并发比较的，但执行效果是谁在前且条件为真谁被执行。

ture_statement1 等执行语句可以是一条语句，也可以是多条。如果是多条执行语句，则需要用 begin 与 end 关键字进行说明。

**case 语句支持嵌套使用。**

下面用 case 语句代替 if 语句实现了一个 4 路选择器的功能。仿真结果与 testbench 可参考[条件语句](https://www.runoob.com/w3cnote/verilog-condition-statement.html)一章，两者完全一致。

```verilog
module mux4to1(
    input [1:0]     sel ,
    input [1:0]     p0 ,
    input [1:0]     p1 ,
    input [1:0]     p2 ,
    input [1:0]     p3 ,
    output [1:0]    sout);
 
    reg [1:0]     sout_t ;
    always @(*)
        case(sel)
            2'b00:   begin      
                    sout_t = p0 ;
                end
            2'b01:       sout_t = p1 ;
            2'b10:       sout_t = p2 ;
            default:     sout_t = p3 ;
        endcase
    assign sout = sout_t ;
 
endmodule
```

case 语句中的条件选项表单式不必都是常量，也可以是 x 值或 z 值。

当多个条件选项下需要执行相同的语句时，多个条件选项可以用逗号分开，放在同一个语句块的候选项中。

但是 case 语句中的 x 或 z 的比较逻辑是不可综合的，所以一般不建议在 case 语句中使用 x 或 z 作为比较值。

例如，对 4 路选择器的 case 语句进行扩展，举例如下：

```verilog
case(sel)
    2'b00:   sout_t = p0 ;
    2'b01:   sout_t = p1 ;
    2'b10:   sout_t = p2 ;
    2'b11:     sout_t = p3 ;
    2'bx0, 2'bx1, 2'bxz, 2'bxx, 2'b0x, 2'b1x, 2'bzx :
        sout_t = 2'bxx ;
    2'bz0, 2'bz1, 2'bzz, 2'b0z, 2'b1z :
        sout_t = 2'bzz ;
    default:  $display("Unexpected input control!!!");
endcase
```

### casex/casez 语句

casex、 casez 语句是 case 语句的变形，用来表示条件选项中的无关项。

casex 用 "x" 来表示无关值，casez 用问号 "?" 来表示无关值。

两者的实现的功能是完全一致的，语法与 case 语句也完全一致。

但是 casex、casez 一般是不可综合的，多用于仿真。

例如用 casez 语句来实现一个 4bit 控制端的 4 路选择选择器。

```verilog
module mux4to1(
    input [3:0]     sel ,
    input [1:0]     p0 ,
    input [1:0]     p1 ,
    input [1:0]     p2 ,
    input [1:0]     p3 ,
    output [1:0]    sout);
 
    reg [1:0]     sout_t ;
    always @(*)
        casez(sel)
            4'b???1:     sout_t = p0 ;
            4'b??1?:     sout_t = p1 ;
            4'b?1??:     sout_t = p2 ;
            4'b1???:     sout_t = p3 ;  
        default:         sout_t = 2'b0 ;
    endcase
    assign      sout = sout_t ;
 
endmodule
```

## 4.7 Verilog 循环语句

关键词：while, for, repeat, forever

Verilog 循环语句有 4 种类型，分别是 while，for，repeat，和 forever 循环。循环语句只能在 always 或 initial 块中使用，但可以包含延迟表达式。

### while 循环

while 循环语法格式如下：

```
while (condition) begin
    …
end
```

while 循环中止条件为 condition 为假。

如果开始执行到 while 循环时 condition 已经为假，那么循环语句一次也不会执行。

当然，执行语句只有一条时，关键字 begin 与 end 可以省略。

下面代码执行时，counter 执行了 11 次。

```verilog
`timescale 1ns/1ns
 
module test ;
 
    reg [3:0]    counter ;
    initial begin
        counter = 'b0 ;
        while (counter<=10) begin
            #10 ;
            counter = counter + 1'b1 ;
        end
    end
 
   //stop the simulation
    always begin
        #10 ;  if ($time >= 1000) $finish ;
    end
 
endmodule
```

### for 循环

for 循环语法格式如下：

```
for(initial_assignment; condition ; step_assignment)  begin
    …
end
```

initial_assignment 为初始条件。

condition 为终止条件，condition 为假时，立即跳出循环。

step_assignment 为改变控制变量的过程赋值语句，通常为增加或减少循环变量计数。

一般来说，因为初始条件和自加操作等过程都已经包含在 for 循环中，所以 for 循环写法比 while 更为紧凑，但也不是所有的情况下都能使用 for 循环来代替 while 循环。

下面 for 循环的例子，实现了与 while 循环中例子一样的效果。需要注意的是，i = i + 1 不能像 C 语言那样写成 i++ 的形式，i = i -1 也不能写成 i -- 的形式。

```verilog
// for 循环语句
integer      i ;
reg [3:0]    counter2 ;
initial begin
    counter2 = 'b0 ;
    for (i=0; i<=10; i=i+1) begin
        #10 ;
        counter2 = counter2 + 1'b1 ;
    end
end
```

### repeat 循环

repeat 循环语法格式如下：

```
repeat (loop_times) begin
    …
end
```

repeat 的功能是执行固定次数的循环，它不能像 while 循环那样用一个逻辑表达式来确定循环是否继续执行。repeat 循环的次数必须是一个常量、变量或信号。如果循环次数是变量信号，则循环次数是开始执行 repeat 循环时变量信号的值。即便执行期间，循环次数代表的变量信号值发生了变化，repeat 执行次数也不会改变。

下面 repeat 循环例子，实现了与 while 循环中的例子一样的效果。

```verilog
// repeat 循环语句
reg [3:0]    counter3 ;
initial begin
    counter3 = 'b0 ;
    repeat (11) begin  //重复11次
        #10 ;
        counter3 = counter3 + 1'b1 ;
    end
end
```

下面 repeat 循环例子，实现了连续存储 8 个数据的功能:

```verilog
always @(posedge clk or negedge rstn) begin
    j = 0  ;
    if (!rstn) begin
        repeat (8) begin
            buffer[j]   <= 'b0 ;      //没有延迟的赋值，即同时赋值为0
            j = j + 1 ;
        end
    end
    else if (enable) begin
        repeat (8) begin
            @(posedge clk) buffer[j]    <= counter3 ;       //在下一个clk的上升沿赋值
            j = j + 1 ;
        end
     end
end
```

### forever 循环

forever 循环语法格式如下：

```
forever begin
    …
end
```

forever 语句表示永久循环，不包含任何条件表达式，一旦执行便无限的执行下去，系统函数 $finish 可退出 forever。

forever 相当于 while(1) 。

通常，forever 循环是和时序控制结构配合使用的。

例如，使用 forever 语句产生一个时钟：

```verilog
reg          clk ;
initial begin
    clk       = 0 ;
    forever begin
        clk = ~clk ;
        #5 ;
    end
end
```

例如，使用 forever 语句实现一个时钟边沿控制的寄存器间数据传输功能：

```verilog
reg    clk ;
reg    data_in, data_temp ;
initial begin
    forever @(posedge clk)      data_temp = data_in ;
end
```

## 4.8 Verilog 过程连续赋值

关键词：deassign，force，release

过程连续赋值是过程赋值的一种。这种赋值语句能够替换其他所有 wire 或 reg 的赋值，改写了 wire 或 reg 型变量的当前值。

与过程赋值不同的是，过程连续赋值的表达式能被连续的驱动到 wire 或 reg 型变量中，即`过程连续赋值发生作用时，右端表达式中任意操作数的变化都会引起过程连续赋值语句的重新执行`。

过程连续性赋值主要有 2 种，assign-deassign 和 force-release。

### assign, deassign

assign（过程赋值操作）与 deassign （取消过程赋值操作）表示第一类过程连续赋值语句。赋值对象只能是寄存器或寄存器组，而不能是 wire 型变量。

赋值过程中对寄存器连续赋值，寄存器中的值被保留直到被重新赋值。

例如，一个带复位端的 D 触发器可以用下面代码描述：

```verilog
module dff_normal(
    input       rstn,
    input       clk,
    input       D,
    output reg  Q
 );

    always @(posedge clk or negedge rstn) begin
        if(!rstn) begin   //Q = 0 after reset effective
            Q <= 1'b0 ;
        end
        else begin
            Q <= D ;       //Q = D at posedge of clock
        end
    end

endmodule  
```

下面，用 assign 与 deassign 改写，完成相同的功能。

即在复位信号为 0 时，Q 端被 assign 语句赋值，始终输出为 0。

复位信号为 1 时，Q 端被 deassign 语句取消赋值，在时钟上升沿被重新赋值。

```verilog
module dff_assign(
    input       rstn,
    input       clk,
    input       D,
    output reg  Q
 );
 
    always @(posedge clk) begin
        Q <= D ;       //Q = D at posedge of clock
    end
 
    always @(negedge rstn) begin
        if(!rstn) begin
            assign Q = 1'b0 ; //change Q value when reset effective
        end
        else begin        //cancel the Q value overlay,
            deassign Q ;  //and Q remains 0-value until the coming of clock posedge
        end
    end
 
endmodule
```

### force, release

force （强制赋值操作）与 release（取消强制赋值）表示第二类过程连续赋值语句。

使用方法和效果，和 assign 与 deassign 类似，但赋值对象可以是 reg 型变量，也可以是 wire 型变量。

因为是无条件强制赋值，一般多用于交互式调试过程，不要在设计模块中使用。

当 force 作用在寄存器上时，寄存器当前值被覆盖；release 时该寄存器值将继续保留强制赋值时的值。之后，该寄存器的值可以被原有的过程赋值语句改变。

当 force 作用在线网上时，线网值也会被强制赋值。但是，一旦 release 该线网型变量，其值马上变为原有的驱动值。

为直观的观察两种类型变量强制赋值的区别，利用第一节中的计数器 counter10 作为设计模块，testbench 设计如下。

```verilog
`timescale 1ns/1ns
 
module test ;
    reg          rstn ;
    reg          clk ;
    reg [3:0]    cnt ;
    wire         cout ;
 
    counter10     u_counter (
        .rstn    (rstn),
        .clk     (clk),
        .cnt     (cnt),
        .cout    (cout));
 
    initial begin
        clk       = 0 ;
        rstn      = 0 ;
        #10 ;
        rstn      = 1'b1 ;
        wait (test.u_counter.cnt_temp == 4'd4) ;
        @(negedge clk) ;
        force     test.u_counter.cnt_temp = 4'd6 ;
        force     test.u_counter.cout     = 1'b1 ;
        #40 ;
        @(negedge clk) ;
        release   test.u_counter.cnt_temp ;
        release   test.u_counter.cout ;
    end
 
    initial begin
        clk = 0 ;
        forever #10 clk = ~ clk ;
    end
 
    //finish the simulation
    always begin
        #1000;
        if ($time >= 1000) $finish ;
    end
 
endmodule // test
```

仿真结果如下。

在 cnt_temp 等于 4 时（80ns）, cnt_temp 被强制赋值为 6，cout 被强制赋值为 1。

release 时（120ns）, cnt_temp 为寄存器类型，仍然保持原有值不变，直到时钟上升沿对其进行加法赋值操作，值才变为 7 。

而 120ns 时，由于 cout 是线网型变量，其值不能保存。原码 counter10 模型中存在驱动语句： **assign cout = (cnt_temp==4'd9)** ，所以 cout 值变为 0 。

## 4.9 区别：连续赋值、过程赋值和过程性连续赋值

### 连续赋值语句（Continuous Assignments）

连续赋值的主要特点：
1）语法上，有关键词“assign”来标识；
2）连续赋值语句不能出现在过程块中（initial/always）；
3）连续赋值语句主要用来对组合逻辑进行建模以及线网数据间进行描述；
4）左侧被赋值的数据类型必须是线网型数据（wire）
连续赋值语句的左值可以是一下类型之一：
①标量线网
②向量线网
③矩阵中的一个元素（该矩阵可以是标量线网类型的，也可以是向量线网类型的）
④向量线网的某一位
⑤向量线网的部分位
以及上述各种类型的拼接体
但是，不能是向量或向量寄存器。
5）连续赋值语句产生作用后，赋值表达式中信号的任何变化都将立即被反映到赋值线网型数据的取值上；
连续赋值语句总是处于激活状态。只要任意一个操作数发生变化，表达式就会被立即重新计算，并且将结果赋给等号左边的线网。
操作数可以是标量或向量的线网或寄存器，也可以是函数的调用。
赋值延迟用于控制对线网赋予新值的时间，根据仿真时间单位进行说明。赋值延迟类似于门延迟，对于描述实际电路中的时序是非常重要的。

### 过程赋值语句（Procedural Assignments）

过程赋值的主要特点：
1）语法上，没有关键词assign；
2）过程赋值语句的左值可以是以下类型之一：
①reg、整形数、实型数、时间寄存器变量或存储器单元
②上述各种类型的位选（例如：addr[3]）
③上述各种类型的域选（例如：addr[31:16]）
④上面三种类型的拼接
3）过程赋值语句只能在initial或always语句内进行赋值，只能对变量数据类型赋值，同时initial和always中只能使用过程赋值语句。
4）在过程赋值语句的情况下，只有在过程赋值语句被执行时才执行赋值操作，语句执行完后被赋值变量的取值不再受到赋值表达式的影响， 这些类型的变量在被赋值后，其值将保持不变，直到被其他过程赋值语句赋予新值。过程赋值语句只有在执行到的时候才会起作用。
5）过程性赋值语句包括两种类型的赋值语句：阻塞赋值（=）和非阻塞赋值（<=）。

### 过程连续赋值语句（Procedural Continuous Assignments）

过程连续性赋值语句的主要特点：
1）过程连续赋值是在过程块内对变量或线网型数据进行连续赋值，是一种过程性赋值，换言之，过程性连续赋值语句是一种能够在always或initial语句块中出现的语句。
2）这种赋值可以改写（Override）所有其他语句对线网或者变量的赋值。这种赋值允许赋值表达式被连续的驱动进入到变量或线网中去；过程性连续赋值语句比普通的过程赋值语句有更高的优先级。
3）过程连续赋值语句有两种类型：
force语句的优先级高于assign 。
①assign和deassign过程性语句：
只能用于对寄存器型变量的连续赋值操作，而不能用来对线网型数据进行连续赋值操作；
deassign 撤销对某一个寄存器型变量的连续赋值后，该寄存器变量仍然保持deassign操作之前的取值。
②force和release过程性语句：它不仅能对寄存器型变量产生作用，也对线网型数据产生作用。

3.1 assign和deassign语句
assign和deassign语句构成了一类过程性连续赋值语句，只能用于对寄存器类型变量的连续赋值操作，不能用来对线网类型数据进行连续赋值操作。
①assign语句
语法：assign <寄存器类型变量> = <赋值表达式>
assign在执行时，寄存器类型变量将由赋值表达式进行连续驱动，即进入连续赋值状态。如果此时有普通的过程赋值语句对该寄存器变量进行过程赋值操作，由于过程连续赋值语句assign的优先级高于普通过程赋值语句，所以出于连续赋值状态的寄存器变量将忽略普通过程赋值语句对它的过程赋值操作，其逻辑状态仍然由过程连续赋值语句内的赋值表达式所决定。
如果先后有两条assign语句对同一寄存器变量进行了过程连续赋值操作，那么第二条assign的执行将覆盖第一条assign的执行效果。
②deassign语句
语法：deassign <寄存器类型变量>
deassign语句是一条撤销连续赋值语句，用来结束对变量的连续赋值操作。当deassign语句执行后，原来由assign语句对该变量进行的连续赋值操作将失效，寄存器变量被连续赋值的状态将得到解除，该变量又可以由普通过程赋值语句进行赋值操作了。这里需要注意一点，当执行该语句撤销对某寄存器变量的连续赋值后，该寄存器变量仍将保持使用该语句之前的原有值。

```verilog
reg [3:0] out;
initial begin
	out = 4'b0000;			//s0:过程赋值语句
	#10;
	assign out = a & b;		//s1:第一条过程连续赋值语句
	#10;
	assign out = c & d;		//s2:第二条过程连续赋值语句
	assign out = e & f;		//s3:第三条过程连续赋值语句
	deassign out;			//s4:撤销连续赋值语句
end
```

上述语句执行过程如下：
s0：在0时刻，out被赋值为0，并且保持这个取值；
s1：在10时刻，s1开始执行，实现了对变量out的连续赋值操作，因此从10时刻开始，out将处于连续赋值状态；
s2：在20时刻，s2开始执行，将覆盖s1产生的作用，所以从20时刻开始，out将由c & d连续驱动；
s3：s3操作覆盖掉s2操作；
s4：当deassign语句得到执行，变量out连续赋值状态被解除，其取值将保持最后一次assign语句赋予的值，即“e & f”；

3.2 force和release语句
force和release语句与assign和deassign语句类似，也是一种过程连续赋值语句。这组赋值语句不仅能对寄存器类型变量产生作用，还能对线网类型数据进行连续赋值操作。
①force语句
语法：force <寄存器变量或者线网数据> = <赋值表达式>>；
force语句应用于寄存器类型变量时，则在force语句执行后，该寄存器变量将强制由<赋值表达式>进行连续驱动，进入被连续赋值的状态，此时将忽略其他较低优先级的赋值语句对该寄存器变量的赋值操作，直到执行一条release语句来释放对该寄存器变量的连续赋值为止。
force语句应用于线网数据时，则force语句执行后，对应的线网数据将得到<赋值表达式>的连续驱动，此时将忽略该线网数据上较低优先级的驱动，直到有一条release语句执行为止。
②release语句
语法：release <寄存器变量或者线网数据>
release语句执行后，原先由force语句对变量或者线网施加的过程连续赋值将失效，变量将解除被被连续赋值的状态，较低优先级的赋值语句的赋值操作将有效。

```verilog
`timescale 1ns/1ps
module test();
	reg [2:0] reg1;
	reg [2:0] reg2;
	initial begin
		reg1 = 3'b000;			//s0:过程赋值语句
		assign reg2 = 3'b001;	//s1过程连续赋值语句
		#10;
		force reg1 = 3'b100;	//s2:过程连续赋值语句
		force reg2 = 3'b100;	//s3:过程连续赋值语句
		#10;
		release reg1;			//s4:撤销连续赋值语句
		release reg2;			//s5:撤销连续赋值语句
	end
endmodule
```

s0：实现对变量reg1的过程赋值操作，即reg1被赋值为3’b000；
s1：执行assign过程连续赋值语句，用来实现对变量reg2的连续赋值，从而reg2将被连续赋值为3’b001；
s2：在执行本条语句时，reg1未被assign语句进行过连续赋值操作，因此reg1被force连续赋值为3’b100；
s3：执行本条语句后，var_reg2被force连续赋值为3’b100；
s4：执行本条语句时，因为变量reg1将退出连续赋值的状态，因为reg1未曾被assign语句进行过连续赋值操作，故reg1取值保持不变，即保持force状态时的值3’b100；
s5：执行本语句时，因为reg2在执行s3之前已经由s1实现了连续赋值，所以在本条语句s5执行后，变量reg2将恢复到由assign语句s1确定的连续赋值状态，即3’b001;

### 赋值语句的区别

4.1 连续赋值语句和过程赋值语句之间的区别
连续赋值语句由assign来标示，而过程赋值语句不能包含这个关键词；
连续赋值语句中左侧的数据类型必须是线网数据类型，而过程赋值语句中的被赋值数据类型则必须是寄存器类型的变量；
连续赋值语句不能出现在过程块（initial或always）中，而过程赋值语句可以；
连续赋值语句主要用来对组合逻辑电路进行建模以及对线网数据间的连接进行描述，而过程赋值语句主要用来对时序逻辑电路进行行为描述；
连续赋值语句对被赋值线网型数据的赋值是“连续”的（即赋值表达式的任何变化都会在立刻反应在线网数据的取值上），而过程性赋值语句，只有在过程赋值语句被执行时才执行赋值操作，语句执行完后被赋值变量的取值不再受到赋值表达式的影响（注意这里的一次是指：在initial块中，过程性赋值只顺序执行一次，而在always块中，每一次满足always的条件时，都要顺序执行一次该always块中的语句。）。

4.2 过程连续赋值语句和连续赋值语句之间的区别
过程连续赋值语句只能用在过程块（initial过程快或always过程块）内，而连续赋值语句不能出现在过程块中。
过程连续赋值语句可以对寄存器类型变量进行连续赋值（其中force-release语句还可以对线网进行连续赋值），但是其赋值目标不能是变量或线网的某一位或某几位，而连续赋值语句只能对线网数据进行赋值，赋值目标可以是线网型数据的某一位或某几位。
————————————————
版权声明：本文为CSDN博主「Fulai.HOU」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Michael177/article/details/120810650



# 5. 模块与端口

## 5.1 Verilog 模块与端口

关键词：模块，端口，双向端口，PAD

结构建模方式有 3 类描述语句： Gate（门级）例化语句，UDP (用户定义原语)例化语句和 module (模块) 例化语句。本次主要讲述使用最多的模块级例化语句。

### 模块

模块是 Verilog 中基本单元的定义形式，是与外界交互的接口。

模块格式定义如下：

```verilog
module module_name #(parameter_list)(port_list) ;              
  Declarations_and_Statements ;
endmodule
```

模块定义必须以关键字 module 开始，以关键字 endmodule 结束。

模块名，端口信号，端口声明和可选的参数声明等，出现在设计使用的 Verilog 语句（图中 Declarations_and_Statements）之前。

模块内部有可选的 5 部分组成，分别是变量声明，数据流语句，行为级语句，低层模块例化及任务和函数，如下图表示。这 5 部分出现顺序、出现位置都是任意的。但是，各种变量都应在使用之前声明。变量具体声明的位置不要求，但必须保证在使用之前的位置。

![2021071304581415181660](Verilog基础教程/2021071304581415181660.png)

前面大多数仿真代码都会用到 module 声明，大家可以自行参考，这里不再做具体举例。下面介绍端口时，再做详细的仿真。

### 端口

端口是模块与外界交互的接口。对于外部环境来说，模块内部是不可见的，对模块的调用只能通过端口连接进行。

**端口列表**

模块的定义中包含一个可选的端口列表，一般将不带类型、不带位宽的信号变量罗列在模块声明里。下面是一个 PAD 模型的端口列表：

```verilog
module pad(    DIN, OEN, PULL,    DOUT, PAD);
```

一个模块如果和外部环境没有交互，则可以不用声明端口列表。例如之前我们仿真时 test.sv 文件中的 test 模块都没有声明具体端口。

```verilog
module test ;  //直接分号结束    ......     //数据流或行为级描述endmodule
```

**端口声明**

(1) 端口信号在端口列表中罗列出来以后，就可以在模块实体中进行声明了。

根据端口的方向，端口类型有 3 种： 输入（input），输出（output）和双向端口（inout）。

>  input、inout 类型不能声明为 reg 数据类型，因为 reg 类型是用于保存数值的，而输入端口只能反映与其相连的外部信号的变化，不能保存这些信号的值。
>
> output 可以声明为 wire 或 reg 数据类型。

上述例子中 pad 模块的端口声明，在 module 实体中就可以表示如下：

```verilog
//端口类型声明
input        DIN, OEN ;
input [1:0]  PULL ;  //(00,01-dispull, 11-pullup, 10-pulldown)
inout        PAD ;   //pad value
output       DOUT ;  //pad load when pad configured as input

//端口数据类型声明
wire         DIN, OEN ;
wire  [1:0]  PULL ;
wire         PAD ;
reg          DOUT ;
```

(2) 在 Verilog 中，端口隐式的声明为 wire 型变量，即当端口具有 wire 属性时，不用再次声明端口类型为 wire 型。但是，当端口有 reg 属性时，则 reg 声明不可省略。

上述例子中的端口声明，则可以简化为：

```verilog
//端口类型声明
input        DIN, OEN ;
input [1:0]  PULL ;     
inout        PAD ;     
output       DOUT ;    
reg          DOUT ;
```

(3) 当然，信号 DOUT 的声明完全可以合并成一句：

```verilog
output reg      DOUT ;
```

(4) 还有一种更简洁且常用的方法来声明端口，即在 module 声明时就陈列出端口及其类型。reg 型端口要么在 module 声明时声明，要么在 module 实体中声明，例如以下 2 种写法是等效的。

实例

```verilog
module pad(
    input        DIN, OEN ,
    input [1:0]  PULL ,
    inout        PAD ,
    output reg   DOUT
    );
 
module pad(
    input        DIN, OEN ,
    input [1:0]  PULL ,
    inout        PAD ,
    output       DOUT
    );
 
    reg        DOUT ;
```

### inout 端口仿真

对包含有 inout 端口类型的 pad 模型进行仿真。pad 模型完整代码如下：

实例

```verilog
module pad(
    //DIN, pad driver when pad configured as output
    //OEN, pad direction(1-input, o-output)
    input        DIN, OEN ,
    //pull function (00,01-dispull, 10-pullup, 11-pulldown)
    input [1:0]  PULL ,
    inout        PAD ,
    //pad load when pad configured as input
    output reg   DOUT
    );
 
    //input:(not effect pad external input logic), output: DIN->PAD
    assign       PAD = OEN? 'bz : DIN ;
 
    //input:(PAD->DOUT)
    always @(*) begin
        if (OEN == 1) begin //input
            DOUT   = PAD ;
        end
        else begin
            DOUT   = 'bz ;
        end
    end
 
    //use tristate gate in Verilog to realize pull up/down function
    bufif1  puller(PAD, PULL[0], PULL[1]);
 
endmodule
```

testbench代码如下：

实例

```verilog
`timescale 1ns/1ns
 
module test ;
    reg          DIN, OEN ;
    reg [1:0]    PULL ;
    wire         PAD ;
    wire         DOUT ;
 
    reg          PAD_REG ;
    assign       PAD = OEN ? PAD_REG : 1'bz ; //
 
    initial begin
        PAD_REG   = 1'bz ;        //pad with no dirve at first
        OEN       = 1'b1 ;        //input simulation
        #0 ;      PULL      = 2'b10 ;   //pull down
        #20 ;     PULL      = 2'b11 ;   //pull up
        #20 ;     PULL      = 2'b00 ;   //dispull
        #20 ;     PAD_REG   = 1'b0 ;
        #20 ;     PAD_REG   = 1'b1 ;
 
        #30 ;     OEN       = 1'b0 ;    //output simulation
                  DIN       = 1'bz ;
        #15 ;     DIN       = 1'b0 ;
        #15 ;     DIN       = 1'b1 ;
    end
 
    pad     u_pad(
        .DIN     (DIN) ,
        .OEN     (OEN) ,
        .PULL    (PULL) ,
        .PAD     (PAD) ,
        .DOUT    (DOUT)
    );
 
    initial begin
        forever begin
            #100;
            if ($time >= 1000)  $finish ;
        end
    end
 
endmodule // test
```

仿真结果如下：

![2021071304581518119591](Verilog基础教程/2021071304581518119591.png)

**仿真结果分析如下：**

当 PAD 方向为 input 且没有驱动时，pull 功能能通过 PAD 的值而体现。

前 60ns 内，PAD 的驱动端 PAD_REG 为 z, 可认为没有驱动，所以开始时 PULL=2, 下拉，PAD值为 0； 20ns 时，PULL=3，上拉，PAD 值为 1；

40ns 时，PULL=0，没有 pull 功能，PAD 值输入为 z。

60ns~100ns 后，PAD 的驱动端 PAD_REG 开始正常驱动。此时相当于 PAD 直接与 PAD_REG 相连，所以 PAD 值与其驱动值保持一致。

以上分析，PAD 方向都是 input，所有输出端 DOUT 与 PAD 值保持一致。

当 PAD 方向为 output 时，即 120ns 时 OEN= 0，PAD 值与输入端 DIN 值保持一致。

## 5.2 Verilog 模块例化

关键字：例化，generate，全加器，层次访问

在一个模块中引用另一个模块，对其端口进行相关连接，叫做`模块例化`。模块例化建立了描述的层次。信号端口可以通过位置或名称关联，端口连接也必须遵循一些规则。

### 命名端口连接

这种方法将需要例化的模块端口与外部信号按照其名字进行连接，端口顺序随意，可以与引用 module 的声明端口顺序不一致，只要保证端口名字与外部信号匹配即可。

下面是例化一次 1bit 全加器的例子：

```verilog
full_adder1  u_adder0(
    .Ai     (a[0]),
    .Bi     (b[0]),
    .Ci     (c==1'b1 ? 1'b0 : 1'b1),
    .So     (so_bit0),
    .Co     (co_temp[0]));
```

如果某些输出端口并不需要在外部连接，例化时 可以悬空不连接，甚至删除。一般来说，input 端口在例化时不能删除，否则编译报错，output 端口在例化时可以删除。例如：

```verilog
//output 端口 Co 悬空
full_adder1  u_adder0(
    .Ai     (a[0]),
    .Bi     (b[0]),
    .Ci     (c==1'b1 ? 1'b0 : 1'b1),
    .So     (so_bit0),
    .Co     ());
 
//output 端口 Co 删除
full_adder1  u_adder0(
    .Ai     (a[0]),
    .Bi     (b[0]),
    .Ci     (c==1'b1 ? 1'b0 : 1'b1),
    .So     (so_bit0));
```

### 顺序端口连接

这种方法将需要例化的模块端口按照模块声明时端口的顺序与外部信号进行匹配连接，位置要严格保持一致。例如例化一次 1bit 全加器的代码可以改为：

```verilog
full_adder1  u_adder1(    a[1], b[1], co_temp[0], so_bit1, co_temp[1]);
```

虽然代码从书写上可能会占用相对较少的空间，但代码可读性降低，也不易于调试。有时候在大型的设计中可能会有很多个端口，端口信号的顺序时不时的可能也会有所改动，此时再利用顺序端口连接进行模块例化，显然是不方便的。所以平时，建议采用命名端口方式对模块进行例化。

### 端口连接规则

**输入端口**

模块例化时，从模块外部来讲， input 端口可以连接 wire 或 reg 型变量。这与模块声明是不同的，从模块内部来讲，input 端口必须是 wire 型变量。

**输出端口**

模块例化时，`从模块外部来讲，output 端口必须连接 wire 型变量`。这与模块声明是不同的，从模块内部来讲，output 端口可以是 wire 或 reg 型变量。

**输入输出端口**

模块例化时，从模块外部来讲，inout 端口必须连接 wire 型变量。这与模块声明是相同的。

**悬空端口**

模块例化时，如果某些信号不需要与外部信号进行连接交互，我们可以将其悬空，即端口例化处保留空白即可，上述例子中有提及。

output 端口正常悬空时，我们甚至可以在例化时将其删除。

input 端口正常悬空时，悬空信号的逻辑功能表现为高阻状态（逻辑值为 z）。但是，例化时一般不能将悬空的 input 端口删除，否则编译会报错，例如：

```verilog
//下述代码编译会报Warning
full_adder4  u_adder4(
    .a      (a),
    .b      (b),
    .c      (),
    .so     (so),
    .co     (co));
```



```verilog
//如果模块full_adder4有input端口c，则下述代码编译是会报Error
full_adder4  u_adder4(
    .a      (a),
    .b      (b),
    .so     (so),
    .co     (co));
```

一般来说，建议 input 端口不要做悬空处理，无其他外部连接时赋值其常量，例如：



```verilog
full_adder4  u_adder4(
    .a      (a),
    .b      (b),
    .c      (1'b0),
    .so     (so),
    .co     (co));
```

**位宽匹配**

当例化端口与连续信号位宽不匹配时，端口会通过无符号数的右对齐或截断方式进行匹配。

假如在模块 full_adder4 中，端口 a 和端口 b 的位宽都为 4bit，则下面代码的例化结果会导致：u_adder4.a = {2'bzz, a[1:0]}, u_adder4.b = b[3:0] 。

```verilog
full_adder4  u_adder4(
    .a      (a[1:0]),      //input a[3:0]
    .b      (b[5:0]),      //input b[3:0]
    .c      (1'b0),
    .so     (so),
    .co     (co));
```

**端口连续信号类型**

连接端口的信号类型可以是，1）标识符，2）位选择，3）部分选择，4）上述类型的合并，5）用于输入端口的表达式。

当然，信号名字可以与端口名字一样，但他们的意义是不一样的，分别代表的是 2 个模块内的信号。

### 用 generate 进行模块例化

当例化多个相同的模块时，一个一个的手动例化会比较繁琐。用 generate 语句进行多个模块的重复例化，可大大简化程序的编写过程。

重复例化 4 个 1bit 全加器组成一个 4bit 全加器的代码如下：



```verilog
module full_adder4(
    input [3:0]   a ,   //adder1
    input [3:0]   b ,   //adder2
    input         c ,   //input carry bit
 
    output [3:0]  so ,  //adding result
    output        co    //output carry bit
    );
 
    wire [3:0]    co_temp ; 
    //第一个例化模块一般格式有所差异，需要单独例化
    full_adder1  u_adder0(
        .Ai     (a[0]),
        .Bi     (b[0]),
        .Ci     (c==1'b1 ? 1'b1 : 1'b0),
        .So     (so[0]),
        .Co     (co_temp[0]));
 
    genvar        i ;
    generate
        for(i=1; i<=3; i=i+1) begin: adder_gen
        full_adder1  u_adder(
            .Ai     (a[i]),
            .Bi     (b[i]),
            .Ci     (co_temp[i-1]), //上一个全加器的溢位是下一个的进位
            .So     (so[i]),
            .Co     (co_temp[i]));
        end
    endgenerate
 
    assign co    = co_temp[3] ;
 
endmodule
```

testbench 如下：



```verilog
`timescale 1ns/1ns
 
module test ;
    reg  [3:0]   a ;
    reg  [3:0]   b ;
    //reg          c ;
    wire [3:0]   so ;
    wire         co ;
 
    //简单驱动
    initial begin
        a = 4'd5 ;
        b = 4'd2 ;
        #10 ;
        a = 4'd10 ;
        b = 4'd8 ;
    end
 
    full_adder4  u_adder4(
               .a      (a),
               .b      (b),
               .c      (1'b0),   //端口可以连接常量
               .so     (so),
               .co     (co));
 
    initial begin
        forever begin
            #100;
            if ($time >= 1000)  $finish ;
        end
    end
 
endmodule // test
```

仿真结果如下，可知 4bit 全加器工作正常：

![2021071304581718671370](Verilog基础教程/2021071304581718671370.png)

### 层次访问

每一个例化模块的名字，每个模块的信号变量等，都使用一个特定的标识符进行定义。在整个层次设计中，每个标识符都具有唯一的位置与名字。

Verilog 中，通过使用一连串的 . 符号对各个模块的标识符进行层次分隔连接，就可以在任何地方通过指定完整的层次名对整个设计中的标识符进行访问。

层次访问多见于仿真中。

例如，有以下层次设计，则叶单元、子模块和顶层模块间的信号就可以相互访问。

```
//u_n1模块中访问u_n3模块信号: 
a = top.u_m2.u_n3.c ;

//u_n1模块中访问top模块信号
if (top.p == 'b0) a = 1'b1 ; 

//top模块中访问u_n4模块信号
assign p = top.u_m2.u_n4.d ;
```

![2021071304581812162351](Verilog基础教程/2021071304581812162351.png)

前面章节的仿真中，或多或少的也进行过相关的层次访问。例如[《过程连续赋值》](https://zhishitu.com/)一节中，在顶层仿真激励 test 模块中使用了如下语句：

```verilog
wait (test.u_counter.cnt_temp == 4'd4) ;
```

## 5.3 Verilog 带参数例化

关键词： defparam，参数，例化，ram

当一个模块被另一个模块引用例化时，高层模块可以对低层模块的参数值进行改写。这样就允许在编译时将不同的参数传递给多个相同名字的模块，而不用单独为只有参数不同的多个模块再新建文件。

参数覆盖有 2 种方式：1）使用关键字 defparam，2）带参数值模块例化。

### defparam 语句

可以用关键字 defparam 通过模块层次调用的方法，来改写低层次模块的参数值。

例如对一个单口地址线和数据线都是 4bit 宽度的 ram 模块的 MASK 参数进行改写：

实例

```verilog
//instantiation
defparam     u_ram_4x4.MASK = 7 ;
ram_4x4    u_ram_4x4
    (
        .CLK    (clk),
        .A      (a[4-1:0]),
        .D      (d),
        .EN     (en),
        .WR     (wr),    //1 for write and 0 for read
        .Q      (q)    );
```

ram_4x4 的模型如下：



```verilog
module  ram_4x4
    (
     input               CLK ,
     input [4-1:0]       A ,
     input [4-1:0]       D ,
     input               EN ,
     input               WR ,    //1 for write and 0 for read
     output reg [4-1:0]  Q    );
 
    parameter        MASK = 3 ;
 
    reg [4-1:0]     mem [0:(1<<4)-1] ;
    always @(posedge CLK) begin
        if (EN && WR) begin
            mem[A]  <= D & MASK;
        end
        else if (EN && !WR) begin
            Q       <= mem[A] & MASK;
        end
    end
 
endmodule
```

对此进行一个简单的仿真，testbench 编写如下：



```verilog
`timescale 1ns/1ns
 
module test ;
    parameter    AW = 4 ;
    parameter    DW = 4 ;
 
    reg                  clk ;
    reg [AW:0]           a ;
    reg [DW-1:0]         d ;
    reg                  en ;
    reg                  wr ;
    wire [DW-1:0]        q ;
 
    //clock generating
    always begin
        #15 ;     clk = 0 ;
        #15 ;     clk = 1 ;
    end
 
    initial begin
        a         = 10 ;
        d         = 2 ;
        en        = 'b0 ;
        wr        = 'b0 ;
        repeat(10) begin
            @(negedge clk) ;
            en     = 1'b1;
            a      = a + 1 ;
            wr     = 1'b1 ;  //write command
            d      = d + 1 ;
        end
        a         = 10 ;
        repeat(10) begin
            @(negedge clk) ;
            a      = a + 1 ;
            wr     = 1'b0 ;  //read command
        end
    end // initial begin
 
    //instantiation
    defparam     u_ram_4x4.MASK = 7 ;
    ram_4x4    u_ram_4x4
    (
        .CLK    (clk),
        .A      (a[AW-1:0]),
        .D      (d),
        .EN     (en),
        .WR     (wr),    //1 for write and 0 for read
        .Q      (q)
     );
 
    //stop simulation
    initial begin
        forever begin
            #100;
            if ($time >= 1000)  $finish ;
        end
    end
 
endmodule // test
```

仿真结果如下：

图中黄色部分，当地址第一次为 c 时写入数据 4， 当第二次地址为 c 时读出数据为 4；可知此时 ram 行为正确，且 MASK 不为 3。 因为 ram 的 Q 端 bit2 没有被屏蔽。

当第一次地址为 1 时写入数据为 9，第二次地址为 1 时读出的数据却是 1，因为此时 MASK 为 7，ram 的 Q 端信号 bit3 被屏蔽。由此可知，MASK 参数被正确改写。

[![2021071304582110648610](Verilog基础教程/2021071304582110648610.png)](https://zhishitu.com/)

### 带参数模块例化

第二种方法就是例化模块时，将新的参数值写入模块例化语句，以此来改写原有 module 的参数值。

例如对一个地址和数据位宽都可变的 ram 模块进行带参数的模块例化：

```verilog
ram #(.AW(4), .DW(4))
    u_ram
    (
        .CLK    (clk),
        .A      (a[AW-1:0]),
        .D      (d),
        .EN     (en),
        .WR     (wr),    //1 for write and 0 for read
        .Q      (q)
     );
```

ram 模型如下：

```verilog
module  ram
    #(  parameter       AW = 2 ,
        parameter       DW = 3 )
    (
        input                   CLK ,
        input [AW-1:0]          A ,
        input [DW-1:0]          D ,
        input                   EN ,
        input                   WR ,    //1 for write and 0 for read
        output reg [DW-1:0]     Q
     );
 
    reg [DW-1:0]         mem [0:(1<<AW)-1] ;
    always @(posedge CLK) begin
        if (EN && WR) begin
            mem[A]  <= D ;
        end
        else if (EN && !WR) begin
            Q       <= mem[A] ;
        end
    end
 
endmodule
```

仿真时，只需在上一例的 testbench 中，将本次例化的模块 u_ram 覆盖掉 u_ram_4x4, 或重新添加之即可。

仿真结果如下。由图可知，ram 模块的参数 AW 与 DW 均被改写为 4， 且 ram 行为正确。

[![2021071304582119975841](Verilog基础教程/2021071304582119975841.png)](https://zhishitu.com/)

### 区别与建议

(1) 和模块端口实例化一样，带参数例化时，也可以不指定原有参数名字，按顺序进行参数例化，例如 u_ram 的例化可以描述为：

```verilog
ram #(4, 4)   u_ram (......) ;
```

(2) 当然，利用 defparam 也可以改写模块在端口声明时声明的参数，利用带参数例化也可以改写模块实体中声明的参数。例如 u_ram 和 u_ram_4x4 的例化分别可以描述为：

```verilog
defparam     u_ram.AW = 4 ;
defparam     u_ram.DW = 4 ;
ram   u_ram(......);
ram_4x4  #(.MASK(7))    u_ram_4x4(......);
```

(3) 那能不能混合使用这两种模块参数改写的方式呢？当然能！前提是所有参数都是模块在端口声明时声明的参数或参数都是模块实体中声明的参数，例如 u_ram 的声明还可以表示为（模块实体中参数可自行实验验证）：

```verilog
defparam     u_ram.AW = 4 ;
ram #(.DW(4)) u_ram (......);  //也只有我这么无聊才会实验这种写法
```

(4) 那如果一个模块中既有在模块在端口声明时声明的参数，又有在模块实体中声明的参数，那这两种参数还能同时改写么？例如在 ram 模块中加入 MASK 参数，模型如下：

```verilog
module  ram
    #(  parameter       AW = 2 ,
        parameter       DW = 3 )
    (
        input                   CLK ,
        input [AW-1:0]          A ,
        input [DW-1:0]          D ,
        input                   EN ,
        input                   WR ,    //1 for write and 0 for read
        output reg [DW-1:0]     Q    );
 
    parameter            MASK = 3 ;
 
    reg [DW-1:0]         mem [0:(1<<AW)-1] ;
    always @(posedge CLK) begin
        if (EN && WR) begin
            mem[A]  <= D ;
        end
        else if (EN && !WR) begin
            Q       <= mem[A] ;
        end
    end
 
endmodule
```

此时再用 defparam 改写参数 MASK 值时，编译报 Error：



```verilog
//都采用defparam时会报Error
defparam     u_ram.AW = 4 ;
defparam     u_ram.DW = 4 ;
defparam     u_ram.MASK = 7 ;
ram   u_ram  (......);
 
//模块实体中parameter用defparam改写也会报Error
defparam     u_ram.MASK = 7 ;
ram #(.AW(4), .DW(4))   u_ram (......);
```

重点来了！！！如果你用带参数模块例化的方法去改写参数 MASK 的值，编译不会报错，MASK 也将被成功改写！

```verilog
ram #(.AW(4), .DW(4), .MASK(7)) u_ram (......);
```

可能的解释为，在编译器看来，如果有模块在端口声明时的参数，那么实体中的参数将视为 localparam 类型，使用 defparam 将不能改写模块实体中声明的参数。

也可能和编译器有关系，大家也可以在其他编译器上实验。

（5）建议，对已有模块进行例化并将其相关参数进行改写时，不要采用 defparam 的方法。除了上述缺点外，defparam 一般也不可综合。

（6）而且建议，模块在编写时，如果预知将被例化且有需要改写的参数，都将这些参数写入到模块端口声明之前的地方（用关键字井号 # 表示）。这样的代码格式不仅有很好的可读性，而且方便调试。

### 阶段总结

其实，介绍到这里，大家完全可以用前面学习到的 Verilog 语言知识，去搭建硬件电路的小茅草屋。对，是小茅草屋。因为硬件语言对应实际硬件电路的这种特殊性，在用 Verilog 建立各种模型时必须考虑实际生成的电路是什么样子的，是否符合实际要求。有时候 rtl 仿真能通过，但是最后生成的实际电路可能会工作异常。

所以，要为你的小茅草屋添砖盖瓦，还需要再学习下进阶部分。当然，进阶部分也只能让你的小茅草屋变成硬朗的砖瓦房，能抵挡风雪交加，可能遇到地震还是会垮塌。

如果你想巩固下你的砖瓦房，去建一套别墅，那你需要再学习下 Verilog 高级篇知识，例如 PLI（编程语言接口）、UDP（用户自定义原语），时序约束和时序分析等，还需要多参与项目工程积累经验，特别注意一些设计技巧，例如低功耗设计、异步设计等。当然学会用 SystemVerilog 去全面验证，又会让你的建筑增加一层防护盾。

但是如果你想把数字电路、Verilog 所有的知识学完，去筑一套防炮弹的总统府，那真的是爱莫能助。因为，学海无涯，回头没岸哪。

限于篇幅，这里只介绍下进阶篇。有机会，高级篇，技巧篇，也一并补上。



# 6.Verilog 函数

## 6.1 Verilog 函数

关键词：函数，大小端转换，数码管译码

在 Verilog 中，可以利用任务（关键字为 task）或函数（关键字为 function），将重复性的行为级设计进行提取，并在多个地方调用，来避免重复代码的多次编写，使代码更加的简洁、易懂。

### 函数

函数只能在模块中定义，位置任意，并在模块的任何地方引用，作用范围也局限于此模块。函数主要有以下几个特点：

- 1）不含有任何延迟、时序或时序控制逻辑
- 2）至少有一个输入变量
- 3）只有一个返回值，且没有输出
- 4）不含有非阻塞赋值语句
- 5）函数可以调用其他函数，但是不能调用任务

Verilog 函数声明格式如下：

```verilog
function [range-1:0]     function_id ;
  input_declaration ; 
  other_declaration ;
  procedural_statement ;
endfunction
```

`函数在声明时，会隐式的声明一个宽度为 range、 名字为 function_id 的寄存器变量，函数的返回值通过这个变量进行传递`。当该寄存器变量没有指定位宽时，`默认位宽为 1`。

函数通过指明函数名与输入变量进行调用。函数结束时，返回值被传递到调用处。

函数调用格式如下：

```verilog
function_id(input1, input2, …);
```

下面用函数实现一个数据大小端转换的功能。

当输入为 4'b0011 时，输出可为 4'b1100。例如：



```verilog
module endian_rvs
    #(parameter N = 4)
        (
            input             en,     //enable control
            input [N-1:0]     a ,
            output [N-1:0]    b
    );
         
        reg [N-1:0]          b_temp ;
        always @(*) begin
        if (en) begin
                b_temp =  data_rvs(a);
            end
            else begin
                b_temp = 0 ;
            end
    end
        assign b = b_temp ;
         
    //function entity
        function [N-1:0]     data_rvs ;
            input     [N-1:0] data_in ;
            parameter         MASK = 32'h3 ; 
            integer           k ;
            begin
                for(k=0; k<N; k=k+1) begin
                    data_rvs[N-k-1]  = data_in[k] ;  
                end
            end
    endfunction
         
endmodule        
```

函数里的参数也可以改写，例如：

```verilog
defparam data_rvs.MASK = 32'd7 ;
```

但是仿真时发现，此种写法编译可以通过，仿真结果中，函数里的参数 MASK 实际并没有改写成功，仍然为 32'h3。这可能和编译器有关，有兴趣的学者可以用其他 Verilog 编译器进行下实验。

函数在声明时，也可以在函数名后面加一个括号，将 input 声明包起来。

例如上述大小端声明函数可以表示为：

```verilog
function [N-1:0]     data_rvs（input     [N-1:0] data_in     ......    ） ;
```

### 常数函数

常数函数是指在仿真开始之前，在编译期间就计算出结果为常数的函数。常数函数不允许访问全局变量或者调用系统函数，但是可以调用另一个常数函数。

这种函数能够用来引用复杂的值，因此可用来代替常量。

例如下面一个常量函数，可以来计算模块中地址总线的宽度：

```verilog
parameter    MEM_DEPTH = 256 ;
reg  [logb2(MEM_DEPTH)-1: 0] addr ; //可得addr的宽度为8bit
 
    function integer     logb2;
    input integer     depth ;
        //256为9bit，我们最终数据应该是8，所以需depth=2时提前停止循环
    for(logb2=0; depth>1; logb2=logb2+1) begin 
        depth = depth >> 1 ;
    end
endfunction
```

### automatic 函数

> 在 Verilog 中，一般函数的局部变量是静态的，即函数的每次调用，函数的局部变量都会使用同一个存储空间。若某个函数在两个不同的地方同时并发的调用，那么两个函数调用行为同时对同一块地址进行操作，会导致不确定的函数结果。

Verilog 用关键字` automatic 来对函数进行说明，此类函数在调用时是可以自动分配新的内存空间的`，也可以理解为是可递归的。因此，automatic 函数中声明的局部变量不能通过层次命名进行访问，但是 automatic 函数本身可以通过层次名进行调用。

下面用 automatic 函数，实现阶乘计算：



```verilog
wire [31:0]          results3 = factorial(4);
function automatic   integer         factorial ;
    input integer     data ;
    integer           i ;
    begin
        factorial = (data>=2)? data * factorial(data-1) : 1 ;
    end
endfunction // factorial
```

下面是加关键字 automatic 和不加关键字 automatic 的仿真结果。

由图可知，信号 results3 得到了我们想要的结果，即 4 的阶乘。

而信号 results_noauto 值为 1，不是可预知的正常结果，这里不再做无用分析。

![2021071304582319792950](Verilog基础教程/2021071304582319792950.png)

### 数码管译码

上述中涉及的相关函数知识似乎并没有体现出函数的优越性。下面设计一个 4 位 10 进制的数码管译码器，来说明函数可以简化代码的优点。

下图是一个数码管的实物图，可以用来显示 4 位十进制的数字。在比赛计分、时间计时等方面有着相当广泛的应用。

![2021071304582413786071](Verilog基础教程/2021071304582413786071.jpg)

每位数码显示端有 8 个光亮控制端（如图中 a-g 所示），可以用来控制显示数字 0-9 。

而数码管有 4 个片选（如图中 1-4），用来控制此时哪一位数码显示端应该选通，即应该发光。倘若在很短的时间内，依次对 4 个数码显示端进行片选发光，同时在不同片选下给予不同的光亮控制（各对应 4 位十进制数字），那么在肉眼不能分辨的情况下，就达到了同时显示 4 位十进制数字的效果。

![2021071304582513892572](Verilog基础教程/2021071304582513892572.jpg)

下面，我们用信号 abcdefg 来控制光亮控制端，用信号 csn 来控制片选，4 位 10 进制的数字个十百千位分别用 4 个 4bit 信号 single_digit, ten_digit, hundred_digit, kilo_digit 来表示，则一个数码管的显示设计可以描述如下：

实例

```verilog
module digital_tube
     (
      input             clk ,
      input             rstn ,
      input             en ,
 
      input [3:0]       single_digit ,
      input [3:0]       ten_digit ,
      input [3:0]       hundred_digit ,
      input [3:0]       kilo_digit ,
 
      output reg [3:0]  csn , //chip select, low-available
      output reg [6:0]  abcdefg        //light control
      );
 
    reg [1:0]            scan_r ;  //scan_ctrl
    always @ (posedge clk or negedge rstn) begin
        if(!rstn)begin
            csn            <= 4'b1111;
            abcdefg        <= 'd0;
            scan_r         <= 3'd0;
        end
        else if (en) begin
            case(scan_r)
            2'd0:begin
                scan_r    <= 3'd1;
                csn       <= 4'b0111;     //select single digit
                abcdefg   <= dt_translate(single_digit);
            end
            2'd1:begin
                scan_r    <= 3'd2;
                csn       <= 4'b1011;     //select ten digit
                abcdefg   <= dt_translate(ten_digit);
            end
            2'd2:begin
                scan_r    <= 3'd3;
                csn       <= 4'b1101;     //select hundred digit
                abcdefg   <= dt_translate(hundred_digit);
            end
            2'd3:begin
                scan_r    <= 3'd0;
                csn       <= 4'b1110;     //select kilo digit
                abcdefg   <= dt_translate(kilo_digit);
            end
            endcase
        end
    end
 
    /*------------ translate function -------*/
    function [6:0] dt_translate;
        input [3:0]   data;
        begin
        case(data)
            4'd0: dt_translate = 7'b1111110;     //number 0 -> 0x7e
            4'd1: dt_translate = 7'b0110000;     //number 1 -> 0x30
            4'd2: dt_translate = 7'b1101101;     //number 2 -> 0x6d
            4'd3: dt_translate = 7'b1111001;     //number 3 -> 0x79
            4'd4: dt_translate = 7'b0110011;     //number 4 -> 0x33
            4'd5: dt_translate = 7'b1011011;     //number 5 -> 0x5b
            4'd6: dt_translate = 7'b1011111;     //number 6 -> 0x5f
            4'd7: dt_translate = 7'b1110000;     //number 7 -> 0x70
            4'd8: dt_translate = 7'b1111111;     //number 8 -> 0x7f
            4'd9: dt_translate = 7'b1111011;     //number 9 -> 0x7b
        endcase
        end
    endfunction
 
endmodule
```

仿真结果如下。

由图可知，片选、译码等信号，均符合设计。实际中，4 位数字应当在一定的时间内保持不变，而片选信号不停的循环扫描，数码管才能给肉眼呈现一种静态显示的效果。

![2021071304582611721323](Verilog基础教程/2021071304582611721323.png)

### 小结

如果译码器设计没有使用函数 dt_translate，则在每个 case 选项里对信号 abcdefg 进行赋值时，还需要对 single_digit，ten_digit, hundred_digit, kilo_digit 进行判断。这些判断语句又会重复 4 次。虽然最后综合出的实际硬件电路可能是一样的，但显然使用函数后的代码更加的简洁、易读。



## 6.2 Verilog 任务

关键词：任务

### 任务与函数的区别

和函数一样，任务（task）可以用来描述共同的代码段，并在模块内任意位置被调用，让代码更加的直观易读。函数一般用于组合逻辑的各种转换和计算，而任务更像一个过程，不仅能完成函数的功能，还可以包含时序控制逻辑。下面对任务与函数的区别进行概括：

| 比较点   | 函数                                                 | 任务                                                         |
| :------- | :--------------------------------------------------- | :----------------------------------------------------------- |
| 输入     | 函数至少有一个输入，端口声明不能包含 inout 型        | 任务可以没有或者有多个输入，且端口声明可以为 inout 型        |
| 输出     | 函数没有输出                                         | 任务可以没有或者有多个输出                                   |
| 返回值   | 函数至少有一个返回值                                 | 任务没有返回值                                               |
| 仿真时刻 | 函数总在零时刻就开始执行                             | 任务可以在非零时刻执行                                       |
| 时序逻辑 | 函数不能包含任何时序控制逻辑                         | 任务不能出现 always 语句，但可以包含其他时序控制，如延时语句 |
| 调用     | 函数只能调用函数，不能调用任务                       | 任务可以调用函数和任务                                       |
| 书写规范 | 函数不能单独作为一条语句出现，只能放在赋值语言的右端 | 任务可以作为一条单独的语句出现语句块中                       |

### 任务

**任务声明**

任务在模块中任意位置定义，并在模块内任意位置引用，作用范围也局限于此模块。

模块内子程序出现下面任意一个条件时，则必须使用任务而不能使用函数。

- 1）子程序中包含时序控制逻辑，例如延迟，事件控制等
- 2）没有输入变量
- 3）没有输出或输出端的数量大于 1

Verilog 任务声明格式如下：

```verilog
task       task_id ;    
port_declaration ;    
procedural_statement ;
endtask
```

任务中使用关键字 input、output 和 inout 对端口进行声明。input 、inout 型端口将变量从任务外部传递到内部，output、inout 型端口将任务执行完毕时的结果传回到外部。

进行任务的逻辑设计时，可以把 input 声明的端口变量看做 wire 型，把 output 声明的端口变量看做 reg 型。但是不需要用 reg 对 output 端口再次说明。

对 output 信号赋值时也不要用关键字 assign。为避免时序错乱，建议 output 信号采用阻塞赋值。

例如，一个带延时的异或功能 task 描述如下：

```verilog
task xor_oper_iner;
    input [N-1:0]   numa;
    input [N-1:0]   numb;
    output [N-1:0]  numco ;
    //output reg [N-1:0]  numco ; //无需再注明 reg 类型，虽然注明也可能没错
    #3  numco = numa ^ numb ;
    //assign #3 numco = numa ^ numb ; //不用assign，因为输出默认是reg
endtask
```

任务在声明时，也可以在任务名后面加一个括号，将端口声明包起来。

上述设计可以更改为：

```verilog
task xor_oper_iner（
    input [N-1:0]   numa,
    input [N-1:0]   numb,
    output [N-1:0]  numco  ） ; 
    #3  numco       = numa ^ numb ;
endtask
```

**任务调用**

任务可单独作为一条语句出现在 initial 或 always 块中，调用格式如下：

```verilog
task_id(input1, input2, …,outpu1, output2, …);
```

任务调用时，端口必须按顺序对应。

输入端连接的模块内信号可以是 wire 型，也可以是 reg 型。==输出端连接的模块内信号要求一定是 reg 型==，这点需要注意。

对上述异或功能的 task 进行一个调用，完成对异或结果的缓存。

```verilog
module xor_oper
    #(parameter         N = 4)
     (
      input             clk ,
      input             rstn ,
      input [N-1:0]     a ,
      input [N-1:0]     b ,
      output [N-1:0]    co  );
 
    reg [N-1:0]          co_t ;
    always @(*) begin          //任务调用
        xor_oper_iner(a, b, co_t);
    end
 
    reg [N-1:0]          co_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            co_r   <= 'b0 ;
        end
        else begin
            co_r   <= co_t ;         //数据缓存
        end
    end
    assign       co = co_r ;
 
   /*------------ task -------*/
    task xor_oper_iner;
        input [N-1:0]   numa;
        input [N-1:0]   numb;
        output [N-1:0]  numco ;
        #3  numco       = numa ^ numb ;   //阻塞赋值，易于控制时序
    endtask
 
endmodule
```

**对上述异或功能设计进行简单的仿真，testbench 描述如下。**

激励部分我们使用简单的 task 进行描述，激励看起来就更加的清晰简洁。

其实，task 最多的应用场景还是应用于 testbench 中进行仿真。task 在一些编译器中也不支持综合。



```verilog
`timescale 1ns/1ns
 
module test ;
    reg          clk, rstn ;
 
    initial begin
        rstn      = 0 ;
        #8 rstn   = 1 ;
        forever begin
            clk = 0 ; # 5;
            clk = 1 ; # 5;
        end
    end
 
    reg  [3:0]   a, b;
    wire [3:0]   co ;
    initial begin
        a         = 0 ;
        b         = 0 ;
        sig_input(4'b1111, 4'b1001, a, b);
        sig_input(4'b0110, 4'b1001, a, b);
        sig_input(4'b1000, 4'b1001, a, b);
    end
 
    task sig_input ;
        input [3:0]       a ;
        input [3:0]       b ;
        output [3:0]      ao ;
        output [3:0]      bo ;
        @(posedge clk) ;
        ao = a ;
        bo = b ;
    endtask ; // sig_input
 
    xor_oper         u_xor_oper
    (
      .clk              (clk  ),
      .rstn             (rstn ),
      .a                (a    ),
      .b                (b    ),
      .co               (co   ));
 
    initial begin
        forever begin
            #100;
            if ($time >= 1000)  $finish ;
        end
    end
 
endmodule // test
```

**仿真结果如下。**

由图可知，异或输出逻辑结果正确，相对于输入有 3ns 的延迟。

且连接信号 a，b，co_t 与任务内部定义的信号 numa，numb，numco 状态也保持一致。

![2021071304582911173270](Verilog基础教程/2021071304582911173270.png)

### 任务操作全局变量

因为任务可以看做是过程性赋值，所以任务的 output 端信号返回时间是在任务中所有语句执行完毕之后。

任务内部变量也只有在任务中可见，如果想具体观察任务中对变量的操作过程，需要将观察的变量声明在模块之内、任务之外，可谓之"全局变量"。

例如有以下 2 种尝试利用 task 产生时钟的描述方式。



```verilog
//way1 to decirbe clk generating, not work
task clk_rvs_iner ;
        output    clk_no_rvs ;
        # 5 ;     clk_no_rvs = 0 ;
        # 5 ;     clk_no_rvs = 1 ;
endtask 
reg          clk_test1 ;
always clk_rvs_iner(clk_test1);

//way2: use task to operate global varialbes to generating clk
reg          clk_test2 ;
task clk_rvs_global ;
        # 5 ;     clk_test2 = 0 ;
        # 5 ;     clk_test2 = 1 ;
endtask // clk_rvs_iner
always clk_rvs_global;
```

**仿真结果如下。**

第一种描述方式，虽然任务内部变量会有赋值 0 和赋值 1 的过程操作，但中间变化过程并不可见，最后输出的结果只能是任务内所有语句执行完毕后输出端信号的最终值。所以信号 clk_test1 值恒为 1，此种方式产生不了时钟。

第二种描述方式，虽然没有端口信号，但是直接对"全局变量"进行过程操作，因为该全局变量对模块是可见的，所以任务内信号翻转的过程会在信号 clk_test2 中体现出来。

[![2021071304582918461311](Verilog基础教程/2021071304582918461311.png)](https://zhishitu.com/)

### automatic 任务

和函数一样，Verilog 中任务调用时的局部变量都是静态的。可以用关键字 automatic 来对任务进行声明，那么任务调用时各存储空间就可以动态分配，每个调用的任务都各自独立的对自己独有的地址空间进行操作，而不影响多个相同任务调用时的并发执行。

如果一任务代码段被 2 处及以上调用，一定要用关键字 automatic 声明。

当没有使用 automatic 声明任务时，任务被 2 次调用，可能出现信号间干扰，例如下面代码描述：

```verilog
task test_flag ;
        input [3:0]       cnti ;
        input             en ;
        output [3:0]      cnto ;
        if (en) cnto = cnti ;
endtask

reg          en_cnt ;
reg [3:0]    cnt_temp ;
initial begin
        en_cnt    = 1 ;
        cnt_temp  = 0 ;
        #25 ;     en_cnt = 0 ;
end
always #10 cnt_temp = cnt_temp + 1 ;

reg [3:0]             cnt1, cnt2 ;
always @(posedge clk) test_flag(2, en_cnt, cnt1);       //task(1)
always @(posedge clk) test_flag(cnt_temp, !en_cnt, cnt2);//task(2)
```

**仿真结果如下。**

en_cnt 为高时，任务 (1) 中信号 en 有效， cnt1 能输出正确的逻辑值；

此时任务 (2) 中信号 en 是不使能的，所以 cnt2 的值被任务 (1) 驱动的共用变量 cnt_temp 覆盖。

en_cnt 为低时，任务 (2) 中信号 en 有效，所以任务 (2) 中的信号 cnt2 能输出正确的逻辑值；而此时信号 cnt1 的值在时钟的驱动下，一次次被任务 (2) 驱动的共用变量 cnt_temp 覆盖。

可见，任务在两次并发调用中，共用存储空间，导致信号相互间产生了影响。

[![2021071304583016118492](Verilog基础教程/2021071304583016118492.png)](https://zhishitu.com/)

**其他描述不变，只在上述 task 声明时加入关键字 automatic，如下所以。**

```
task automatic test_flag ;
```

此时仿真结果如下。

- en_cnt 为高时，任务 (1) 中信号 cnt1 能输出正确的逻辑值，任务 (2) 中信号 cnt2 的值为 X；
- en_cnt 为低时，任务 (2) 中信号 cnt2 能输出正确的逻辑值，任务 (1) 中信号 cnt1 的值为 X；

可见，任务在两次并发调用中，因为存储空间相互独立，信号间并没有产生影响。

![2021071304583113381113](Verilog基础教程/2021071304583113381113.png)



## 6.3 Verilog 状态机

关键词：状态机，售卖机

有限状态机（Finite-State Machine，FSM），简称状态机，是表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型。状态机不仅是一种电路的描述工具，而且也是一种思想方法，在电路设计的系统级和 RTL 级有着广泛的应用。

### 状态机类型

Verilog 中状态机主要用于同步时序逻辑的设计，能够在有限个状态之间按一定要求和规律切换时序电路的状态。状态的切换方向不但取决于各个输入值，还取决于当前所在状态。状态机可分为 2 类：Moore 状态机和 Mealy 状态机。

**Moore 型状态机**

Moore 型状态机的输出只与当前状态有关，与当前输入无关。

输出会在一个完整的时钟周期内保持稳定，即使此时输入信号有变化，输出也不会变化。输入对输出的影响要到下一个时钟周期才能反映出来。这也是 Moore 型状态机的一个重要特点：输入与输出是隔离开来的。

![2021071304583315585810](Verilog基础教程/2021071304583315585810.png)

**Mealy 型状态机**

Mealy 型状态机的输出，不仅与当前状态有关，还取决于当前的输入信号。

Mealy 型状态机的输出是在输入信号变化以后立刻发生变化，且输入变化可能出现在任何状态的时钟周期内。因此，同种逻辑下，Mealy 型状态机输出对输入的响应会比 Moore 型状态机早一个时钟周期。

![2021071304583415983951](Verilog基础教程/2021071304583415983951.png)

**状态机设计流程**

根据设计需求画出状态转移图，确定使用状态机类型，并标注出各种输入输出信号，更有助于编程。一般使用最多的是 Mealy 型 3 段式状态机，下面用通过设计一个自动售卖机的具体实例来说明状态机的设计过程。

### 自动售卖机

**自动售卖机的功能描述如下：**

饮料单价 2 元，该售卖机只能接受 0.5 元、1 元的硬币。考虑找零和出货。投币和出货过程都是一次一次的进行，不会出现一次性投入多币或一次性出货多瓶饮料的现象。每一轮售卖机接受投币、出货、找零完成后，才能进入到新的自动售卖状态。

**该售卖机的工作状态转移图如下所示，包含了输入、输出信号状态。**

其中，coin = 1 代表投入了 0.5 元硬币，coin = 2 代表投入了 1 元硬币。

![2021071304583516795782](Verilog基础教程/2021071304583516795782.png)

### 状态机设计：3 段式（推荐）

**状态机设计如下：**

- (0) 首先，根据状态机的个数确定状态机编码。利用编码给状态寄存器赋值，代码可读性更好。
- (1) 状态机第一段，时序逻辑，非阻塞赋值，传递寄存器的状态。
- (2) 状态机第二段，组合逻辑，阻塞赋值，根据当前状态和当前输入，确定下一个状态机的状态。
- (3) 状态机第三代，时序逻辑，非阻塞赋值，因为是 Mealy 型状态机，根据当前状态和当前输入，确定输出信号。

### 实例

```verilog
// vending-machine
// 2 yuan for a bottle of drink
// only 2 coins supported: 5 jiao and 1 yuan
// finish the function of selling and changing

module  vending_machine_p3  (
    input           clk ,
    input           rstn ,
    input [1:0]     coin ,     //01 for 0.5 jiao, 10 for 1 yuan

    output [1:0]    change ,
    output          sell    //output the drink
    );

    //machine state decode
    parameter            IDLE   = 3'd0 ;
    parameter            GET05  = 3'd1 ;
    parameter            GET10  = 3'd2 ;
    parameter            GET15  = 3'd3 ;

    //machine variable
    reg [2:0]            st_next ;
    reg [2:0]            st_cur ;

    //(1) state transfer
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            st_cur      <= 'b0 ;
        end
        else begin
            st_cur      <= st_next ;
        end
    end

    //(2) state switch, using block assignment for combination-logic
    //all case items need to be displayed completely    
    always @(*) begin 
        //st_next = st_cur ;//如果条件选项考虑不全，可以赋初值消除latch
        case(st_cur)
            IDLE:
                case (coin)
                    2'b01:     st_next = GET05 ;
                    2'b10:     st_next = GET10 ;
                    default:   st_next = IDLE ;
                endcase
            GET05:
                case (coin)
                    2'b01:     st_next = GET10 ;
                    2'b10:     st_next = GET15 ;
                    default:   st_next = GET05 ;
                endcase

            GET10:
                case (coin)
                    2'b01:     st_next = GET15 ;
                    2'b10:     st_next = IDLE ;
                    default:   st_next = GET10 ;
                endcase
            GET15:
                case (coin)
                    2'b01,2'b10:
                               st_next = IDLE ;
                    default:   st_next = GET15 ;
                endcase
            default:    st_next = IDLE ;
        endcase
    end

    //(3) output logic, using non-block assignment
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
        else if ((st_cur == GET15 && coin ==2'h1)
               || (st_cur == GET10 && coin ==2'd2)) begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b1 ;
        end
        else if (st_cur == GET15 && coin == 2'h2) begin
            change_r       <= 2'b1 ;
            sell_r         <= 1'b1 ;
        end
        else begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
    end
    assign       sell    = sell_r ;
    assign       change  = change_r ;

endmodule
```

**testbench 设计如下。仿真中模拟了 4 种情景，分别是：**

case1 对应连续输入 4 个 5 角硬币；case2 对应 1 元 - 5 角 - 1 元的投币顺序；case3 对应 5 角 - 1 元 - 5 角的投币顺序；case4 对应连续 3 个 5 角然后一个 1 元的投币顺序。

实例

```verilog
`timescale 1ns/1ps

module test ;
    reg          clk;
    reg          rstn ;
    reg [1:0]    coin ;
    wire [1:0]   change ;
    wire         sell ;

    //clock generating
    parameter    CYCLE_200MHz = 10 ; //
    always begin
        clk = 0 ; #(CYCLE_200MHz/2) ;
        clk = 1 ; #(CYCLE_200MHz/2) ;
    end

    //motivation generating
    reg [9:0]    buy_oper ; //store state of the buy operation
    initial begin
        buy_oper  = 'h0 ;
        coin      = 2'h0 ;
        rstn      = 1'b0 ;
        #8 rstn   = 1'b1 ;
        @(negedge clk) ;

        //case(1) 0.5 -> 0.5 -> 0.5 -> 0.5
        #16 ;
        buy_oper  = 10'b00_0101_0101 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end

        //case(2) 1 -> 0.5 -> 1, taking change
        #16 ;
        buy_oper  = 10'b00_0010_0110 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end

        //case(3) 0.5 -> 1 -> 0.5
        #16 ;
        buy_oper  = 10'b00_0001_1001 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end

        //case(4) 0.5 -> 0.5 -> 0.5 -> 1, taking change
        #16 ;
        buy_oper  = 10'b00_1001_0101 ;
        repeat(5) begin
            @(negedge clk) ;
            coin      = buy_oper[1:0] ;
            buy_oper  = buy_oper >> 2 ;
        end
    end

   //(1) mealy state with 3-stage
    vending_machine_p3    u_mealy_p3     (
        .clk              (clk),
        .rstn             (rstn),
        .coin             (coin),
        .change           (change),
        .sell             (sell)
        );

   //simulation finish
   always begin
      #100;
      if ($time >= 10000)  $finish ;
   end

endmodule // test
```

仿真结果如下:

由图可知，代表出货动作的信号 sell 都能在投币完毕后正常的拉高，而代表找零动作的信号 change 也都能根据输入的硬币场景输出正确的是否找零信号。

[![2021071304583618531043](Verilog基础教程/2021071304583618531043.png)](https://zhishitu.com/)

### 状态机修改：2 段式

**将 3 段式状态机 2、3 段描述合并，其他部分保持不变，状态机就变成了 2 段式描述。**

修改部分如下：

实例

```verilog
//(2) state switch, and output logic
//all using block assignment for combination-logic
reg  [1:0]   change_r ;
reg          sell_r ;
always @(*) begin //all case items need to be displayed completely
    case(st_cur)
        IDLE: begin
            change_r     = 2'b0 ;
            sell_r       = 1'b0 ;
            case (coin)
                2'b01:     st_next = GET05 ;
                2'b10:     st_next = GET10 ;
                default:   st_next = IDLE ;
            endcase // case (coin)
        end
        GET05: begin
            change_r     = 2'b0 ;
            sell_r       = 1'b0 ;
            case (coin)
                2'b01:     st_next = GET10 ;
                2'b10:     st_next = GET15 ;
                default:   st_next = GET05 ;
            endcase // case (coin)
        end

        GET10:
            case (coin)
                2'b01:     begin
                    st_next      = GET15 ;
                    change_r     = 2'b0 ;
                    sell_r       = 1'b0 ;
                end
                2'b10:     begin
                    st_next      = IDLE ;
                    change_r     = 2'b0 ;
                    sell_r       = 1'b1 ;
                end
                default:   begin
                    st_next      = GET10 ;
                    change_r     = 2'b0 ;
                    sell_r       = 1'b0 ;
                end
            endcase // case (coin)

        GET15:
            case (coin)
                2'b01: begin
                    st_next     = IDLE ;
                    change_r    = 2'b0 ;
                    sell_r      = 1'b1 ;
                end
                2'b10:     begin
                    st_next     = IDLE ;
                    change_r    = 2'b1 ;
                    sell_r      = 1'b1 ;
                end
                default:   begin
                    st_next     = GET15 ;
                    change_r    = 2'b0 ;
                    sell_r      = 1'b0 ;
                end
            endcase
        default:  begin
            st_next     = IDLE ;
            change_r    = 2'b0 ;
            sell_r      = 1'b0 ;
        end

    endcase
end
```

**将上述修改的新模块例化到 3 段式的 testbench 中即可进行仿真，结果如下:**

由图可知，出货信号 sell 和 找零信号 change 相对于 3 段式状态机输出提前了一个时钟周期，这是因为输出信号都是阻塞赋值导致的。

如图中红色圆圈部分，输出信号都出现了干扰脉冲，这是因为输入信号都是异步的，而且输出信号是组合逻辑输出，没有时钟驱动。

实际中，如果输入信号都是与时钟同步的，这种干扰脉冲是不会出现的。如果是异步输入信号，首先应当对信号进行同步。

[![2021071304583715497804](Verilog基础教程/2021071304583715497804.png)](https://zhishitu.com/)

### 状态机修改：1 段式（慎用）

**将 3 段式状态机 1、 2、3 段描述合并，状态机就变成了 1 段式描述。**

修改部分如下：

实例

```verilog
    //(1) using one state-variable do describe
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            st_cur     <= 'b0 ;
            change_r   <= 2'b0 ;
            sell_r     <= 1'b0 ;
        end
        else begin
            case(st_cur)

            IDLE: begin
                change_r  <= 2'b0 ;
                sell_r    <= 1'b0 ;
                case (coin)
                    2'b01:     st_cur <= GET05 ;
                    2'b10:     st_cur <= GET10 ;
                endcase
            end
            GET05: begin
                case (coin)
                    2'b01:     st_cur <= GET10 ;
                    2'b10:     st_cur <= GET15 ;
                endcase
            end

            GET10:
                case (coin)
                    2'b01:     st_cur   <=  GET15 ;
                    2'b10:     begin
                        st_cur   <= IDLE ;
                        sell_r   <= 1'b1 ;
                    end
                endcase

            GET15:
                case (coin)
                    2'b01:     begin
                        st_cur   <= IDLE ;
                        sell_r   <= 1'b1 ;
                    end
                    2'b10:     begin
                        st_cur   <= IDLE ;
                        change_r <= 2'b1 ;
                        sell_r   <= 1'b1 ;
                    end
                endcase

            default:  begin
                  st_cur    <= IDLE ;
            end

            endcase // case (st_cur)
        end // else: !if(!rstn)
    end
```

**将上述修改的新模块例化到 3 段式的 testbench 中即可进行仿真，结果如下:**

由图可知，输出信号与 3 段式状态机完全一致。

1 段式状态机的缺点就是许多种逻辑糅合在一起，不易后期的维护。当状态机和输出信号较少时，可以尝试此种描述方式。

[![2021071304583812864965](Verilog基础教程/2021071304583812864965.png)](https://zhishitu.com/)

### 状态机修改：Moore 型

如果使用 Moore 型状态机描述售卖机的工作流程，那么还需要再增加 2 个状态编码，用以描述 Mealy 状态机输出时的输入信号和状态机状态。

3 段式 Moore 型状态机描述的自动售卖机 Verilog 代码如下：

实例

```verilog
module  vending_machine_moore    (
    input           clk ,
    input           rstn ,
    input [1:0]     coin ,     //01 for 0.5 jiao, 10 for 1 yuan

    output [1:0]    change ,
    output          sell    //output the drink
    );

    //machine state decode
    parameter            IDLE   = 3'd0 ;
    parameter            GET05  = 3'd1 ;
    parameter            GET10  = 3'd2 ;
    parameter            GET15  = 3'd3 ;
    // new state for moore state-machine
    parameter            GET20  = 3'd4 ;
    parameter            GET25  = 3'd5 ;

    //machine variable
    reg [2:0]            st_next ;
    reg [2:0]            st_cur ;

    //(1) state transfer
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            st_cur      <= 'b0 ;
        end
        else begin
            st_cur      <= st_next ;
        end
    end

    //(2) state switch, using block assignment for combination-logic
    always @(*) begin //all case items need to be displayed completely
        case(st_cur)
            IDLE:
                case (coin)
                    2'b01:     st_next = GET05 ;
                    2'b10:     st_next = GET10 ;
                    default:   st_next = IDLE ;
                endcase
            GET05:
                case (coin)
                    2'b01:     st_next = GET10 ;
                    2'b10:     st_next = GET15 ;
                    default:   st_next = GET05 ;
                endcase

            GET10:
                case (coin)
                    2'b01:     st_next = GET15 ;
                    2'b10:     st_next = GET20 ;
                    default:   st_next = GET10 ;
                endcase
            GET15:
                case (coin)
                    2'b01:     st_next = GET20 ;
                    2'b10:     st_next = GET25 ;
                    default:   st_next = GET15 ;
                endcase
            GET20:         st_next = IDLE ;
            GET25:         st_next = IDLE ;
            default:       st_next = IDLE ;
        endcase // case (st_cur)
    end // always @ (*)

   // (3) output logic,
   // one cycle delayed when using non-block assignment
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
        else if (st_cur == GET20 ) begin
            sell_r         <= 1'b1 ;
        end
        else if (st_cur == GET25) begin
            change_r       <= 2'b1 ;
            sell_r         <= 1'b1 ;
        end
        else begin
            change_r       <= 2'b0 ;
            sell_r         <= 1'b0 ;
        end
    end
    assign       sell    = sell_r ;
    assign       change  = change_r ;

endmodule
```

**将上述修改的 Moore 状态机例化到 3 段式的 testbench 中即可进行仿真，结果如下:**

由图可知，输出信号与 Mealy 型 3 段式状态机相比延迟了一个时钟周期，这是因为进入到新增加的编码状态机时需要一个时钟周期的时延。此时，输出再用非阻塞赋值就会导致最终的输出信号延迟一个时钟周期。这也属于 Moore 型状态机的特点。

[![2021071304583819635326](Verilog基础教程/2021071304583819635326.png)](https://zhishitu.com/)

**输出信号赋值时，用阻塞赋值，则可以提前一个时钟周期。**

输出逻辑修改如下。

实例

```verilog
    // (3.2) output logic, using block assignment
    reg  [1:0]   change_r ;
    reg          sell_r ;
    always @(*) begin
        change_r  = 'b0 ;
        sell_r    = 'b0 ; //not list all condition, initializing them
        if (st_cur == GET20 ) begin
            sell_r         = 1'b1 ;
        end
        else if (st_cur == GET25) begin
            change_r       = 2'b1 ;
            sell_r         = 1'b1 ;
        end
    end
```

**输出信号阻塞赋值的仿真结果如下:**

由图可知，输出信号已经和 3 段式 Mealy 型状态机一致。

[![2021071304583917106757](Verilog基础教程/2021071304583917106757.png)](https://zhishitu.com/)

### 

## 6.4 Verilog 竞争与冒险

关键字：竞争，冒险，书写规范

### 产生原因

数字电路中，信号传输与状态变换时都会有一定的延时。

- 在组合逻辑电路中，不同路径的输入信号变化传输到同一点门级电路时，在时间上有先有后，这种先后所形成的时间差称为`竞争（Competition）`。
- 由于竞争的存在，<u>输出信号需要经过一段时间才能达到期望状态，过渡时间内可能产生瞬间的错误输出，例如尖峰脉冲</u>。这种现象被称为`冒险（Hazard）`。
- 竞争不一定有冒险，但冒险一定会有竞争。

例如，对于给定逻辑    F = A & A'   ，电路如左下图所示。

由于反相器电路的存在，信号 A' 传递到与门输入端的时间相对于信号 A 会滞后，这就可能导致与门最后的输出结果 F 会出现干扰脉冲。如右下图所示。

![2021071304584117742710](Verilog基础教程/2021071304584117742710.png)

![2021071304584311042851](Verilog基础教程/2021071304584311042851.png)

其实实际硬件电路中，只要门电路各个输入端延时不同，就有可能产生竞争与冒险。

例如一个简单的与门，输入信号源不一定是同一个信号变换所来，由于硬件工艺、其他延迟电路的存在，也可能产生竞争与冒险，如下图所示。

![2021071304584318568612](Verilog基础教程/2021071304584318568612.png)

### 判断方法

**代数法**

在逻辑表达式，保持一个变量固定不动，将剩余其他变量用 0 或 1 代替，如果最后逻辑表达式能化简成

```
Y = A + A'
```

或

```
Y = A · A'
```

的形式，则可判定此逻辑存在竞争与冒险。

例如逻辑表达式 Y = AB + A'C，在 B=C=1 的情况下，可化简为 Y = A + A'。显然，A 状态的改变，势必会造成电路存在竞争冒险。

**卡诺图法**

有两个相切的卡诺圈，并且相切处没有其他卡诺圈包围，可能会出现竞争与冒险现象。

例如左下图所存在竞争与冒险，右下图则没有。

![2021071304584415974713](Verilog基础教程/2021071304584415974713.png)

其实，卡诺图本质上还是对逻辑表达式的一个分析，只是可以进行直观的判断。

例如，左上图逻辑表达式可以简化为 Y = A'B' + AC，当 B=0 且 C=1 时，此逻辑表达式又可以表示为 Y = A' + A。所以肯定会存在竞争与冒险。

右上图逻辑表达式可以简化为 Y = A'B' + AB，显然 **B** 无论等于 **1** 还是 **0**，此式都不会化简成 Y = A' + A。所以此逻辑不存在竞争与冒险。

需要注意的是，卡诺图是首尾相临的。如下图所示，虽然看起来两个卡诺圈并没有相切，但实际上，m6 与 m4 也是相邻的，所以下面卡诺图所代表的数字逻辑也会产生竞争与冒险。

![2021071304584513478164](Verilog基础教程/2021071304584513478164.png)

**其他较为复杂的情况，可能需要采用 "计算机辅助分析 + 实验" 的方法。**

### 消除方法

对数字电路来说，常见的避免竞争与冒险的方法主要有 4 种。

### 1）增加滤波电容，滤除窄脉冲

此种方法需要在输出端并联一个小电容，将尖峰脉冲的幅度削弱至门电路阈值以下。

此方法虽然简单，但是会增加输出电压的翻转时间，易破坏波形。

### 2）修改逻辑，增加冗余项

利用卡诺图，在两个相切的圆之间，增加一个卡诺圈，并加在逻辑表达式之中。

如下图所示，对数字逻辑 Y = A'B' + AC 增加冗余项 B'C，则此电路逻辑可以表示为 Y = A'B' + AC + B'C。此时电路就不会再存在竞争与冒险。

![2021071304584610085385](Verilog基础教程/2021071304584610085385.png)

### 3）使用时钟同步电路，利用触发器进行打拍延迟

同步电路信号的变化都发生在时钟边沿。对于触发器的 D 输入端，只要毛刺不出现在时钟的上升沿并且不满足数据的建立和保持时间，就不会对系统造成危害，因此可认为 D 触发器的 D 输入端对毛刺不敏感。利用此特性，在时钟边沿驱动下，对一个组合逻辑信号进行延迟打拍，可消除竞争冒险。

延迟一拍时钟时，会一定概率的减少竞争冒险的出现。实验表明，最安全的打拍延迟周期是 3 拍，可有效减少竞争冒险的出现。

当然，最终还是需要根据自己的设计需求，对信号进行合理的打拍延迟。

**为说明对信号进行打拍延迟可以消除竞争冒险，我们建立下面的代码模型。**

实例

```verilog
module competition_hazard
    (
      input             clk ,
      input             rstn ,
      input             en ,
      input             din_rvs ,
      output reg        flag
    );

    wire    condition = din_rvs & en ;  //combination logic
    always @(posedge clk or negedge !rstn) begin
        if (!rstn) begin
            flag   <= 1'b0 ;
        end
        else begin
            flag   <= condition ;
        end
    end 

endmodule
```

**testbench 描述如下：**

### 实例

```verilog
`timescale 1ns/1ns

module test ;
    reg          clk, rstn ;
    reg          en ;
    reg          din_rvs ;
    wire         flag_safe, flag_dgs ;

    //clock and rstn generating 
    initial begin
        rstn              = 1'b0 ;
        clk               = 1'b0 ;
        #5 rstn           = 1'b1 ;
        forever begin
            #5 clk = ~clk ;
        end
    end

    initial begin
        en        = 1'b0 ;
        din_rvs   = 1'b1 ;
        #19 ;      en        = 1'b1 ;
        #1 ;       din_rvs   = 1'b0 ;
    end

    competition_hazard         u_dgs
     (
      .clk              (clk           ),
      .rstn             (rstn          ),
      .en               (en            ),
      .din_rvs          (din_rvs       ),
      .flag             (flag_dgs      ));

    initial begin
        forever begin
            #100;
            if ($time >= 1000)  $finish ;
        end
    end

endmodule // test
```

**仿真结果如下:**

由图可知，信号 condition 出现了一个尖峰脉冲，这是由于信号 din_rvs 与信号 en 相对于模块内部时钟都是异步的，所以到达内部门电路时的延时是不同的，就有可能造成竞争冒险。

虽然最后的仿真结果 flag 一直为 0，似乎是我们想要的结果。但是实际电路中，这个尖峰脉冲在时间上非常靠近时钟边沿，就有可能被时钟采集到而产生错误结果。

![2021071304584618341786](Verilog基础教程/2021071304584618341786.png)

下面我们对模型进行改进，增加打拍延时的逻辑，如下：

实例

```verilog
module clap_delay
    (
      input             clk ,
      input             rstn ,
      input             en ,
      input             din_rvs ,
      output reg        flag
    );

    reg                  din_rvs_r ;
    reg                  en_r ;
    always @(posedge clk or !rstn) begin
        if (!rstn) begin
            din_rvs_r      <= 1'b0 ;
            en_r           <= 1'b0 ;
        end
        else begin
            din_rvs_r      <= din_rvs ;
            en_r           <= en ;
        end
    end

    wire                 condition = din_rvs_r & en_r ;
    always @(posedge clk or negedge !rstn) begin
        if (!rstn) begin
            flag   <= 1'b0 ;
        end
        else begin

            flag   <= condition ;
        end
    end // always @ (posedge clk or negedge !rstn)

endmodule
```

**将此模块例化到上述 testbench 中，得到如下仿真结果。**

由图可知，信号 condition 没有尖峰脉冲的干扰了，仿真结果中 flag 为 0 也如预期。

其实，输入信号与时钟边沿非常接近的情况下，时钟对输入信号的采样也存在不确定性，但是不会出现尖峰脉冲的现象。对输入信号多打 2 拍，是更好的处理方式，对竞争与冒险有更好的抑制作用。

![2021071304584716666957](Verilog基础教程/2021071304584716666957.png)

### 4）采用格雷码计数器

递加的多 bit 位计数器，计数值有时候会发生多个 bit 位的跳变。

例如计数器变量 counter 从 5 计数到 6 时， 对应二进制数字为 4'b101 到 4'b110 的转换。因为各 bit 数据位的延时，counter 的变换过程可能是： 4'b101 -> 4'b111 -> 4'b110。如果有以下逻辑描述，则信号 cout 可能出现短暂的尖峰脉冲，这显然是与设计相悖的。

```
cout = counter[3:0] == 4'd7 ; 
```

而格雷码计数器，计数时相邻的数之间只有一个数据 bit 发生了变化，所以能有效的避免竞争冒险。

好在 Verilog 设计时，计数器大多都是同步设计。即便计数时存在多个 bit 同时翻转的可能性，但在时钟驱动的触发器作用下，只要信号间满足时序要求，就能消除掉 100% 的竞争与冒险。

**小结**

- 一般来说，为消除竞争冒险，增加滤波电容和逻辑冗余，都不是 Verilog 设计所考虑的。
- 计数采用格雷码计数器，大多数也是应用在高速时钟下减少信号翻转率来降低功耗的场合。
- 利用触发器在时钟同步电路下对异步信号进行打拍延时，是 Verilog 设计中经常用到的方法。

除此之外，为消除竞争冒险，Verilog 编码时还需要注意一些问题，详见下一小节。

### Verilog 书写规范

在编程时多注意以下几点，也可以避免大多数的竞争与冒险问题。

- 1）时序电路建模时，用非阻塞赋值。
- 2）组合逻辑建模时，用阻塞赋值。
- 3）在同一个 always 块中建立时序和组合逻辑模型时，用非阻塞赋值。
- 4）在同一个 always 块中不要既使用阻塞赋值又使用非阻塞赋值。
- 5）不要在多个 always 块中为同一个变量赋值。
- 6）避免 latch 产生。

下面，对以上注意事项逐条分析。

### 1）时序电路建模时，用非阻塞赋值

前面讲述非阻塞赋值时就陈述过，时序电路中非阻塞赋值可以消除竞争冒险。

例如下面代码描述，由于无法确定 a 与 b 阻塞赋值的操作顺序，就有可能带来竞争冒险。

```verilog
always @(posedge clk) begin    a = b ;    b = a ;end
```

而使用非阻塞赋值时，赋值操作是同时进行的，所以就不会带来竞争冒险，如以下代码描述。

```verilog
always @(posedge clk) begin    a <= b ;    b <= a ;end
```

### 2）组合逻辑建模时，用阻塞赋值

例如，我们想实现 C = A&B, F=C&D 的组合逻辑功能，用非阻塞赋值语句如下。

两条赋值语句同时赋值，F <= C & D 中使用的是信号 C 的旧值，所以导致此时的逻辑是错误的，F 的逻辑值不等于 A&B&D。

而且，此时要求信号 C 具有存储功能，但不是时钟驱动，所以 C 可能会被综合成锁存器（latch），导致竞争冒险。

```verilog
always @(*) begin    C <= A & B ;    F <= C & D ;end
```

对代码进行如下修改，F = C & D 的操作一定是在 C = A & B 之后，此时 F 的逻辑值等于 A&B&D，符合设计。

```verilog
always @(*) begin    C = A & B ;    F = C & D ;end
```

### 3）在同一个 always 块中建立时序和组合逻辑模型时，用非阻塞赋值

虽然时序电路中可能涉及组合逻辑，但是如果赋值操作使用非阻塞赋值，仍然会导致如规范 1 中所涉及的类似问题。

例如在时钟驱动下完成一个与门的逻辑功能，代码参考如下。

实例

```verilog
always @(posedge clk or negedge rst_n)
    if (!rst_n) begin
        q <= 1'b0;
    end
    else begin
        q <= a & b;  //即便有组合逻辑，也不要写成：q = a & b 
     end
end
```

### 4）在同一个 always 块中不要既使用阻塞赋值又使用非阻塞赋值

always 涉及的组合逻辑中，既有阻塞赋值又有非阻塞赋值时，会导致意外的结果，例如下面代码描述。

此时信号 C 阻塞赋值完毕以后，信号 F 才会被非阻塞赋值，仿真结果可能正确。

但如果 F 信号有其他的负载，F 的最新值并不能马上传递出去，数据有效时间还是在下一个触发时刻。此时要求 F 具有存储功能，可能会被综合成 latch，导致竞争冒险。

```verilog
always @(*) begin    C = A & B ;    F <= C & D ;end
```

如下代码描述，仿真角度看，信号 C 被非阻塞赋值，下一个触发时刻才会有效。而 F = C & D 虽然是阻塞赋值，但是信号 C 不是阻塞赋值，所以 F 逻辑中使用的还是 C 的旧值。

```verilog
always @(*) begin    C <= A & B ;    F = C & D ;end
```

下面分析假如在时序电路里既有阻塞赋值，又有非阻塞赋值会怎样，代码如下。

假如复位端与时钟同步，那么由于复位导致的信号 q 为 0，是在下一个时钟周期才有效。

而如果是信号 a 或 b 导致的 q 为 0，则在当期时钟周期内有效。

如果 q 还有其他负载，就会导致 q 的时序特别混乱，显然不符合设计需求。

实例

```verilog
always @(posedge clk or negedge rst_n)
    if (!rst_n) begin  //假设复位与时钟同步
        q <= 1'b0;
    end
    else begin
        q = a & b;   
    end
end
```

需要说明的是，很多编译器都支持这么写，上述的分析也都是建立在仿真角度上。实际中如果阻塞赋值和非阻塞赋值混合编写，综合后的电路时序将是错乱的，不利于分析调试。

### 5）不要在多个 always 块中为同一个变量赋值

与 C 语言有所不同，`Verilog 中不允许在多个 always 块中为同一个变量赋值。此时信号拥有多驱动端（Multiple Driver），是禁止的`。当然，也不允许 assign 语句为同一个变量进行多次连线赋值。从信号角度来讲，多驱动时，同一个信号变量在很短的时间内进行多次不同的赋值结果，就有可能产生竞争冒险。

从语法来讲，很多编译器检测到多驱动时，也会报 Error。

### 6）避免 latch 产生

具体分析见下一章：[《避免 Latch》](https://zhishitu.com/)。



## 6.5 Verilog 避免 Latch

关键词：触发器，锁存器

### Latch 的含义

锁存器（Latch），是电平触发的存储单元，数据存储的动作取决于输入时钟（或者使能）信号的电平值。仅当锁存器处于使能状态时，输出才会随着数据输入发生变化。

当电平信号无效时，输出信号随输入信号变化，就像通过了缓冲器；当电平有效时，输出信号被锁存。激励信号的任何变化，都将直接引起锁存器输出状态的改变，很有可能会因为瞬态特性不稳定而产生振荡现象。

锁存器示意图如下：

![2021071304585011120040]( Verilog基础教程/2021071304585011120040.png)

触发器（flip-flop），是边沿敏感的存储单元，数据存储的动作（状态转换）由某一信号的上升沿或者下降沿进行同步的（限制存储单元状态转换在一个很短的时间内）。

触发器示意图如下：

![2021071304585018689261](Verilog基础教程/2021071304585018689261.png)

寄存器（register），在 Verilog 中用来暂时存放参与运算的数据和运算结果的变量。一个变量声明为寄存器时，它既可以被综合成触发器，也可能被综合成 Latch，甚至是 wire 型变量。但是大多数情况下我们希望它被综合成触发器，但是有时候由于代码书写问题，它会被综合成不期望的 Latch 结构。

Latch 的主要危害有：

- 1）输入状态可能多次变化，容易产生毛刺，增加了下一级电路的不确定性；
- 2）在大部分 FPGA 的资源中，可能需要比触发器更多的资源去实现 Latch 结构；
- 3）锁存器的出现使得静态时序分析变得更加复杂。

Latch 多用于门控时钟（clock gating）的控制，一般设计时，我们应当避免 Latch 的产生。

### if 结构不完整

组合逻辑中，不完整的 if - else 结构，会产生 latch。

例如下面的模型，if 语句中缺少 else 结构，系统默认 else 的分支下寄存器 q 的值保持不变，即具有存储数据的功能，所以寄存器 q 会被综合成 latch 结构。

实例

```verilog
module module1_latch1(
    input       data, 
    input       en ,
    output reg  q) ;
    
    always @(*) begin
        if (en) q = data ;
    end

endmodule
```

避免此类 latch 的方法主要有 2 种，一种是补全 if-else 结构，或者对信号赋初值。

例如，上面模型中的always语句，可以改为以下两种形式：

实例

```verilog
    // 补全条件分支结构    
    always @(*) begin
        if (en)  q = data ;
        else     q = 1'b0 ;
    end

    //赋初值
    always @(*) begin
        q = 1'b0 ;
        if (en) q = data ; //如果en有效，改写q的值，否则q会保持为0
    end
```

但是在时序逻辑中，不完整的 if - else 结构，不会产生 latch，例如下面模型。

这是因为，q 寄存器具有存储功能，且其值在时钟的边沿下才会改变，这正是触发器的特性。

实例

```verilog
module module1_ff(
    input       clk ,
    input       data, 
    input       en ,
    output reg  q) ;
    
    always @(posedge clk) begin
        if (en) q <= data ;
    end

endmodule
```

在组合逻辑中，当条件语句中有很多条赋值语句时，每个分支条件下赋值语句的不完整也是会产生 latch。

其实对每个信号的逻辑拆分来看，这也相当于是 if-else 结构不完整，相关寄存器信号缺少在其他条件下的赋值行为。例如：

实例

```verilog
module module1_latch11(
    input       data1, 
    input       data2, 
    input       en ,
    output reg  q1 ,
    output reg  q2) ;
    
    always @(*) begin
        if (en)   q1 = data1 ;
        else      q2 = data2 ;
    end

endmodule
```

这种情况也可以通过补充完整赋值语句或赋初值来避免 latch。例如：

实例

```verilog
    always @(*) begin
        //q1 = 0; q2 = 0 ; //或在这里对 q1/q2 赋初值
        if (en)  begin
            q1 = data1 ;
            q2 = 1'b0 ;
        end
        else begin
            q1 = 1'b0 ;
            q2 = data2 ;
        end
    end
```

### case 结构不完整

case 语句产生 Latch 的原理几乎和 if 语句一致。在组合逻辑中，当 case 选项列表不全且没有加 default 关键字，或有多个赋值语句不完整时，也会产生 Latch。例如：

实例

```verilog
module module1_latch2(
    input       data1, 
    input       data2, 
    input [1:0] sel ,
    output reg  q ) ;
    
    always @(*) begin
        case(sel)
            2'b00:  q = data1 ;
            2'b01:  q = data2 ;
        endcase
    end

endmodule
```

当然，消除此种 latch 的方法也是 2 种，将 case 选项列表补充完整，或对信号赋初值。

补充完整 case 选项列表时，可以罗列所有的选项结果，也可以用 default 关键字来代替其他选项结果。

例如，上述 always 语句有以下 2 种修改方式。

实例

```verilog
    always @(*) begin
        case(sel)
            2'b00:    q = data1 ;
            2'b01:    q = data2 ;
            default:  q = 1'b0 ;
        endcase
    end

    always @(*) begin
        case(sel)
            2'b00:  q = data1 ;
            2'b01:  q = data2 ;
            2'b10, 2'b11 :  
                    q = 1'b0 ;
        endcase
    end
```

### 原信号赋值或判断

在组合逻辑中，如果一个信号的赋值源头有其信号本身，或者判断条件中有其信号本身的逻辑，则也会产生 latch。因为此时信号也需要具有存储功能，但是没有时钟驱动。此类问题在 if 语句、case 语句、问号表达式中都可能出现，例如：

实例

```verilog
    //signal itself as a part of condition
    reg a, b ;
    always @(*) begin
        if (a & b)  a = 1'b1 ;   //a -> latch
        else a = 1'b0 ;
    end
    
    //signal itself are the assigment source 
    reg        c;
    wire [1:0] sel ;
    always @(*) begin
        case(sel)
            2'b00:    c = c ;    //c -> latch
            2'b01:    c = 1'b1 ;
            default:  c = 1'b0 ;
        endcase
    end

    //signal itself as a part of condition in "? expression"
    wire      d, sel2; 
    assign    d =  (sel2 && d) ? 1'b0 : 1'b1 ;  //d -> latch
```

避免此类 Latch 的方法，就只有一种，即在组合逻辑中避免这种写法，信号不要给信号自己赋值，且不要用赋值信号本身参与判断条件逻辑。

例如，如果不要求立刻输出，可以将信号进行一个时钟周期的延时再进行相关逻辑的组合。上述第一个产生 Latch 的代码可以描述为：

实例

```verilog
    reg   a, b ;
    reg   a_r ;
    
    always (@posedge clk)
        a_r  <= a ;
        
    always @(*) begin
        if (a_r & b)  a = 1'b1 ;   //there is no latch
        else a = 1'b0 ;
    end
```

### 敏感信号列表不完整

如果组合逻辑中 always@() 块内敏感列表没有列全，该触发的时候没有触发，那么相关寄存器还是会保存之前的输出结果，因而会生成锁存器。

这种情况，把敏感信号补全或者直接用 always@(*) 即可消除 latch。

小结

总之，为避免 latch 的产生，在组合逻辑中，需要注意以下几点：

- 1）if-else 或 case 语句，结构一定要完整
- 2）不要将赋值信号放在赋值源头，或条件判断中
- 3）敏感信号列表建议多用 always@(*)

## 6.6 Verilog 仿真激励

关键词：testbench，仿真，文件读写

Verilog 代码设计完成后，还需要进行重要的步骤，即逻辑功能仿真。仿真激励文件称之为 testbench，放在各设计模块的顶层，以便对模块进行系统性的例化调用进行仿真。

毫不夸张的说，对于稍微复杂的 Verilog 设计，如果不进行仿真，即便是经验丰富的老手，99.9999% 以上的设计都不会正常的工作。不能说仿真比设计更加的重要，但是一般来说，仿真花费的时间会比设计花费的时间要多。有时候，考虑到各种应用场景，testbench 的编写也会比 Verilog 设计更加的复杂。所以，数字电路行业会具体划分设计工程师和验证工程师。

下面，对 testbench 做一个简单的学习。

### testbench 结构划分

testbench 一般结构如下:

![2021071304585213168330](Verilog基础教程/2021071304585213168330.png)

其实 testbench 最基本的结构包括信号声明、激励和模块例化。

根据设计的复杂度，需要引入时钟和复位部分。当然更为复杂的设计，激励部分也会更加复杂。根据自己的验证需求，选择是否需要自校验和停止仿真部分。

当然，复位和时钟产生部分，也可以看做激励，所以它们都可以在一个语句块中实现。也可以拿自校验的结果，作为结束仿真的条件。

实际仿真时，可以根据自己的个人习惯来编写 testbench，这里只是做一份个人的总结。

### testbench 仿真举例

前面的章节中，已经写过很多的 testbench。其实它们的结构也都大致相同。

下面，我们举一个数据拼接的简单例子，对 testbench 再做一个具体的分析。

一个 2bit 数据拼接成 8bit 数据的功能模块描述如下:

实例

```verilog
module  data_consolidation
    (
        input           clk ,
        input           rstn ,
        input [1:0]     din ,          //data in
        input           din_en ,
        output [7:0]    dout ,
        output          dout_en        //data out
     );

   // data shift and counter
    reg [7:0]            data_r ;
    reg [1:0]            state_cnt ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            state_cnt     <= 'b0 ;
            data_r        <= 'b0 ;
        end
        else if (din_en) begin
            state_cnt     <= state_cnt + 1'b1 ;    //数据计数
            data_r        <= {data_r[5:0], din} ;  //数据拼接
        end
        else begin
            state_cnt <= 'b0 ;
        end
    end
    assign dout          = data_r ;

    // data output en
    reg                  dout_en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            dout_en_r       <= 'b0 ;
        end
        //计数为 3 且第 4 个数据输入时，同步输出数据输出使能信号
        else if (state_cnt == 2'd3 & din_en) begin  
            dout_en_r       <= 1'b1 ;
        end
        else begin
            dout_en_r       <= 1'b0 ;
        end
    end
    //这里不直接声明dout_en为reg变量，而是用相关寄存器对其进行assign赋值
    assign dout_en       = dout_en_r;

endmodule
```

对应的 testbench 描述如下，增加了文件读写的语句:

实例

```verilog
`timescale 1ns/1ps

   //============== (1) ==================
   //signals declaration
module test ;
    reg          clk;
    reg          rstn ;
    reg [1:0]    din ;
    reg          din_en ;
    wire [7:0]   dout ;
    wire         dout_en ;

    //============== (2) ==================
    //clock generating
    real         CYCLE_200MHz = 5 ; //
    always begin
        clk = 0 ; #(CYCLE_200MHz/2) ;
        clk = 1 ; #(CYCLE_200MHz/2) ;
    end

    //============== (3) ==================
    //reset generating
    initial begin
        rstn      = 1'b0 ;
        #8 rstn      = 1'b1 ;
    end

    //============== (4) ==================
    //motivation
    int          fd_rd ;
    reg [7:0]    data_in_temp ;  //for self check
    reg [15:0]   read_temp ;     //8bit ascii data, 8bit \n
    initial begin
        din_en    = 1'b0 ;        //(4.1)
        din       = 'b0 ;
        open_file("../tb/data_in.dat", "r", fd_rd); //(4.2)
        wait (rstn) ;    //(4.3)
        # CYCLE_200MHz ;

        //read data from file
        while (! $feof(fd_rd) ) begin  //(4.4)
            @(negedge clk) ;
            $fread(read_temp, fd_rd);
            din    = read_temp[9:8] ;
            data_in_temp = {data_in_temp[5:0], din} ;
            din_en = 1'b1 ;
        end

        //stop data
        @(posedge clk) ;  //(4.5)
        #2 din_en = 1'b0 ;
    end

    //open task
    task open_file;
        input string      file_dir_name ;
        input string      rw ;
        output int        fd ;

        fd = $fopen(file_dir_name, rw);
        if (! fd) begin
            $display("--- iii --- Failed to open file: %s", file_dir_name);
        end
        else begin
            $display("--- iii --- %s has been opened successfully.", file_dir_name);
        end
    endtask

    //============== (5) ==================
    //module instantiation
    data_consolidation    u_data_process
    (
      .clk              (clk),
      .rstn             (rstn),
      .din              (din),
      .din_en           (din_en),
      .dout             (dout),
      .dout_en          (dout_en)
     );

    //============== (6) ==================
    //auto check
    reg  [7:0]           err_cnt ;
    int                  fd_wr ;

    initial begin
        err_cnt   = 'b0 ;
        open_file("../tb/data_out.dat", "w", fd_wr);
        forever begin
            @(negedge clk) ;
            if (dout_en) begin
                $fdisplay(fd_wr, "%h", dout);
            end
        end
    end

    always @(posedge clk) begin
        #1 ;
        if (dout_en) begin
            if (data_in_temp != dout) begin
                err_cnt = err_cnt + 1'b1 ;
            end
        end
    end

    //============== (7) ==================
    //simulation finish
    always begin
        #100;
        if ($time >= 10000)  begin
            if (!err_cnt) begin
                $display("-------------------------------------");
                $display("Data process is OK!!!");
                $display("-------------------------------------");
            end
            else begin
                $display("-------------------------------------");
                $display("Error occurs in data process!!!");
                $display("-------------------------------------");
            end
            #1 ;
            $finish ;
        end
    end

endmodule // test
```

仿真结果如下。由图可知，数据整合功能的设计符合要求:

[![2021071304585311474831](Verilog基础教程/2021071304585311474831.png)](https://zhishitu.com/)

### testbench 具体分析

**1）信号声明**

testbench 模块声明时，一般不需要声明端口。因为激励信号一般都在 testbench 模块内部，没有外部信号。

声明的变量应该能全部对应被测试模块的端口。当然，变量不一定要与被测试模块端口名字一样。但是被测试模块输入端对应的变量应该声明为 reg 型，如 clk，rstn 等，输出端对应的变量应该声明为 wire 型，如 dout，dout_en。

**2）时钟生成**

生成时钟的方式有很多种，例如以下两种生成方式也可以借鉴。

实例

```verilog
initial clk = 0 ;
always #(CYCLE_200MHz/2) clk = ~clk;

initial begin
    clk = 0 ;
    forever begin
        #(CYCLE_200MHz/2) clk = ~clk;
    end
end       
```

需要注意的是，利用取反方法产生时钟时，一定要给 clk 寄存器赋初值。

利用参数的方法去指定时间延迟时，如果延时参数为浮点数，该参数不要声明为 parameter 类型。例如实例中变量 CYCLE_200MHz 的值为 2.5。如果其变量类型为 parameter，最后生成的时钟周期很可能就是 4ns。当然，timescale 的精度也需要提高，单位和精度不能一样，否则小数部分的时间延迟赋值也将不起作用。

**3）复位生成**

复位逻辑比较简单，一般赋初值为 0，再经过一段小延迟后，复位为 1 即可。

这里大多数的仿真都是用的低有效复位。

**4）激励部分**

激励部分该产生怎样的输入信号，是根据被测模块的需要来设计的。

本次实例中:

- (4.1) 对被测模块的输入信号进行一个初始化，防止不确定值 X 的出现。激励数据的产生，我们需要从数据文件内读入。
- (4.2) 处利用一个 task 去打开一个文件，只要指定文件存在，就可以得到一个不为 0 的句柄信号 fp_rd。fp_rd 指定了文件数据的起始地址。
- (4.3) 的操作是为了等待复位后，系统有一个安全稳定的可测试状态。
- (4.4) 开始循环读数据、给激励。在时钟下降沿送出数据，是为了被测试模块能更好的在上升沿采样数据。

利用系统任务 $fread ，通过句柄信号 fd_rd 将读取的 16bit 数据变量送入到 read_temp 缓存。

输入数据文件前几个数据截图如下。因为 $fread 只能读取 2 进制文件，所以输入文件的第一行对应的 ASCII 码应该是 330a，所以我们想要得到文件里的数据 3，应该取变量 read_temp 的第 9 到第 8bit 位的数据。

![2021071304585319733392](Verilog基础教程/2021071304585319733392.png)

信号 data_in_temp 是对输入数据信号的一个紧随的整合，后面校验模块会以此为参考，来判断仿真是否正常，模块设计是否正确。

- (4.5) 选择在时钟上升沿延迟 2 个周期后停止输入数据，是为了被测试模块能够正常的采样到最后一个数据使能信号，并对数据进行正常的整合。

当数据量相对较少时，可以利用 Verilog 中的系统任务 $readmemh 来按行直接读取 16 进制数据。保持文件 data_in.dat 内数据和格式不变，则该激励部分可以描述为：

实例

```verilog
    reg [1:0]    data_mem [39:0] ;
    reg [7:0]    data_in_temp ;  //for self check
    integer      k1 ;
    initial begin
        din_en    = 1'b0 ;
        din       = 'b0 ;
        $readmemh("../tb/data_in.dat", data_mem);
        wait (rstn) ;
        # CYCLE_200MHz ;

        //read data from file
        for(k1=0; k1<40; k1=k1+1)  begin
            @(negedge clk) ;
            din    = data_mem[k1] ;
            data_in_temp = {data_in_temp[5:0], din} ;
            din_en = 1'b1 ;
        end

        //stop data
        @(posedge clk) ;
        #2 din_en = 1'b0 ;
     end
```

**5）模块例化**

这里利用 testbench 开始声明的信号变量，对被测试模块进行例化连接。

**6）自校验**

如果设计比较简单，完全可以通过输入、输出信号的波形来确定设计是否正确，此部分完全可以删除。如果数据很多，有时候拿肉眼观察并不能对设计的正确性进行一个有效判定。此时加入一个自校验模块，会大大增加仿真的效率。

实例中，我们会在数据输出使能 dout_en 有效时，对输出数据 dout 与参考数据 read_temp（激励部分产生）做一个对比，并将对比结果置于信号 err_cnt 中。最后就可以通过观察 err_cnt 信号是否为 0 来直观的对设计的正确性进行判断。

当然如实例中所示，我们也可以将数据写入到对应文件中，利用其他方式做对比。

**7）结束仿真**

如果我们不加入结束仿真部分，仿真就会无限制的运行下去，波形太长有时候并不方便分析。Verilog 中提供了系统任务 $finish 来停止仿真。

停止仿真之前，可以将自校验的结果，通过系统任务 $display 在终端进行显示。

### 文件读写选项

用于打开文件的系统任务 $fopen 格式如下：

```
fd = $fopen("<name_of_file>", "mode")
```

和 C 语言类似，打开方式的选项 "mode" 意义如下：

| r    | 只读打开一个文本文件，只允许读数据。                         |
| :--- | :----------------------------------------------------------- |
| w    | 只写打开一个文本文件，只允许写数据。如果文件存在，则原文件内容会被删除。如果文件不存在，则创建新文件。 |
| a    | 追加打开一个文本文件，并在文件末尾写数据。如果文件如果文件不存在，则创建新文件。 |
| rb   | 只读打开一个二进制文件，只允许读数据。                       |
| wb   | 只写打开或建立一个二进制文件，只允许写数据。                 |
| ab   | 追加打开一个二进制文件，并在文件末尾写数据。                 |
| r+   | 读写打开一个文本文件，允许读和写                             |
| w+   | 读写打开或建立一个文本文件，允许读写。如果文件存在，则原文件内容会被删除。如果文件不存在，则创建新文件。 |
| a+   | 读写打开一个文本文件，允许读和写。如果文件不存在，则创建新文件。读取文件会从文件起始地址的开始，写入只能是追加模式。 |
| rb+  | 读写打开一个二进制文本文件，功能与 "r+" 类似。               |
| wb+  | 读写打开或建立一个二进制文本文件，功能与 "w+" 类似。         |
| ab+  | 读写打开一个二进制文本文件，功能与 "a+" 类似。               |



## 6.7 Verilog 流水线

关键词：流水线，乘法器

硬件描述语言的一个突出优点就是指令执行的并行性。多条语句能够在相同时钟周期内并行处理多个信号数据。

但是当数据串行输入时，指令执行的并行性并不能体现出其优势。而且很多时候有些计算并不能在一个或两个时钟周期内执行完毕，如果每次输入的串行数据都需要等待上一次计算执行完毕后才能开启下一次的计算，那效率是相当低的。`流水线就是解决多周期下串行数据计算效率低的问题`。

### 流水线

流水线的基本思想是：把一个重复的过程分解为若干个子过程，每个子过程由专门的功能部件来实现。将多个处理过程在时间上错开，依次通过各功能段，这样每个子过程就可以与其他子过程并行进行。

假如一个洗衣店内洗衣服的过程分为 4 个阶段：取衣、洗衣、烘干、装柜。每个阶段都需要半小时来完成，则洗一次衣服需要 2 小时。

考虑最差情况，洗衣店内只有一台洗衣机、一台烘干机、一个衣柜。如果每半小时送来一批要洗的衣服，每次等待上一批衣服洗完需要 2 小时，那么洗完 4 批衣服需要的时间就是 8 小时。

图示如下：

[![2021071304585615530540](Verilog基础教程/2021071304585615530540.png)](https://zhishitu.com/)

对这个洗衣店的装备进行升级，一共引进 4 套洗衣服的装备，工作人员也增加到 4 个，每个人负责一个洗衣阶段。所以每批次的衣服，都能够及时的被相同的人放入到不同的洗衣机内。由于时间上是错开的，每批次的衣服都能被相同的人在不同的设备与时间段（半小时）内洗衣、烘干和装柜。图示如下。

[![2021071304585616353601](Verilog基础教程/2021071304585616353601.jpg)](https://zhishitu.com/)

可以看出，洗完 4 批衣服只需要 3 个半小时，效率明显提高。

其实，在 2 小时后第一套洗衣装备已经完成洗衣过程而处于空闲状态，如果此时还有第 5 批衣服的送入，那么第一套设备又可以开始工作。依次类推，只要衣服批次不停的输入，4 台洗衣设备即可不间断的完成对所有衣服的清洗过程。且除了第一批次洗衣时间需要 2 小时，后面每半小时都会有一批次衣服清洗完成。

衣服批次越多，节省的时间就越明显。假如有 N 批次衣服，需要的时间为 (4+N) 个半小时。

当然，升级后洗衣流程也有缺点。设备和工作人员的增加导致了投入的成本增加，洗衣店内剩余空间也被缩小，工作状态看起来比较繁忙。

和洗衣服过程类似，数据的处理路径也可以看作是一条生产线，路径上的每个数字处理单元都可以看作是一个阶段，会产生延时。

流水线设计就是将路径系统的分割成一个个数字处理单元（阶段），并在各个处理单元之间插入寄存器来暂存中间阶段的数据。被分割的单元能够按阶段并行的执行，相互间没有影响。所以最后流水线设计能够提高数据的吞吐率，即提高数据的处理速度。

`流水线设计的缺点就是，各个处理阶段都需要增加寄存器保存中间计算状态，而且多条指令并行执行，势必会导致功耗增加`。

下面，设计一个乘法器，并对是否采用流水线设计进行对比。

### 一般乘法器设计

**前言**

也许有人会问，直接用乘号 * 来完成 2 个数的相乘不是更快更简单吗？

如果你有这个疑问，说明你对硬件描述语言的认知还有所不足。就像之前所说，Verilog 描述的是硬件电路，直接用乘号完成相乘过程，编译器在编译的时候也会把这个乘法表达式映射成默认的乘法器，但其构造不得而知。

例如，在 FPGA 设计中，可以直接调用 IP 核来生成一个高性能的乘法器。在位宽较小的时候，一个周期内就可以输出结果，位宽较大时也可以流水输出。在能满足要求的前提下，可以谨慎的用 * 或直接调用 IP 来完成乘法运算。

但乘法器 IP 也有很多的缺陷，例如位宽的限制，未知的时序等。尤其使用乘号，会为数字设计的不确定性埋下很大的隐瞒。

很多时候，常数的乘法都会用移位相加的形式实现，例如：

实例

```
A = A<<1 ;       //完成A * 2
A = (A<<1) + A ;   //对应A * 3
A = (A<<3) + (A<<2) + (A<<1) + A ; //对应A * 15
```

用一个移位寄存器和一个加法器就能完成乘以 3 的操作。但是乘以 15 时就需要 3 个移位寄存器和 3 个加法器（当然乘以 15 可以用移位相减的方式）。

有时候数字电路在一个周期内并不能够完成多个变量同时相加的操作。所以数字设计中，最保险的加法操作是同一时刻只对 2 个数据进行加法运算，最差设计是同一时刻对 4 个及以上的数据进行加法运算。

如果设计中有同时对 4 个数据进行加法运算的操作设计，那么此部分设计就会有危险，可能导致时序不满足。

此时，设计参数可配、时序可控的流水线式乘法器就显得有必要了。

**设计原理**

和十进制乘法类似，计算 13 与 5 的相乘过程如下所示：

![2021071304585713530912](Verilog基础教程/2021071304585713530912.png)

由此可知，被乘数按照乘数对应 bit 位进行移位累加，便可完成相乘的过程。

假设每个周期只能完成一次累加，那么一次乘法计算时间最少的时钟数恰好是乘数的位宽。所以建议，将位宽窄的数当做乘数，此时计算周期短。

**乘法器设计**

考虑每次乘法运算只能输出一个结果（非流水线设计），设计代码如下。

实例

```verilog
module    mult_low
    #(parameter N=4,
      parameter M=4)
     (
      input                     clk,
      input                     rstn,
      input                     data_rdy ,  //数据输入使能
      input [N-1:0]             mult1,      //被乘数
      input [M-1:0]             mult2,      //乘数

      output                    res_rdy ,   //数据输出使能
      output [N+M-1:0]          res         //乘法结果
      );

    //calculate counter
    reg [31:0]           cnt ;
    //乘法周期计数器
    wire [31:0]          cnt_temp = (cnt == M)? 'b0 : cnt + 1'b1 ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            cnt    <= 'b0 ;
        end
        else if (data_rdy) begin    //数据使能时开始计数
            cnt    <= cnt_temp ;
        end
        else if (cnt != 0 ) begin  //防止输入使能端持续时间过短
            cnt    <= cnt_temp ;
        end
        else begin
            cnt    <= 'b0 ;
        end
    end

    //multiply
    reg [M-1:0]          mult2_shift ;
    reg [M+N-1:0]        mult1_shift ;
    reg [M+N-1:0]        mult1_acc ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            mult2_shift    <= 'b0 ;
            mult2_shift    <= 'b0 ;
            mult1_acc      <= 'b0 ;
        end
        else if (data_rdy && cnt=='b0) begin  //初始化
            mult1_shift    <= {{(N){1'b0}}, mult1} << 1 ;  
            mult2_shift    <= mult2 >> 1 ;  
            mult1_acc      <= mult2[0] ? {{(N){1'b0}}, mult1} : 'b0 ;
        end
        else if (cnt != M) begin
            mult1_shift    <= mult1_shift << 1 ;  //被乘数乘2
            mult2_shift    <= mult2_shift >> 1 ;  //乘数右移，方便判断
            //判断乘数对应为是否为1，为1则累加
            mult1_acc      <= mult2_shift[0] ? mult1_acc + mult1_shift : mult1_acc ;
        end
        else begin
            mult2_shift    <= 'b0 ;
            mult2_shift    <= 'b0 ;
            mult1_acc      <= 'b0 ;
        end
    end

    //results
    reg [M+N-1:0]        res_r ;
    reg                  res_rdy_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            res_r          <= 'b0 ;
            res_rdy_r      <= 'b0 ;
        end  
        else if (cnt == M) begin
            res_r          <= mult1_acc ;  //乘法周期结束时输出结果
            res_rdy_r      <= 1'b1 ;
        end
        else begin
            res_r          <= 'b0 ;
            res_rdy_r      <= 'b0 ;
        end
    end

    assign res_rdy       = res_rdy_r;
    assign res           = res_r;

endmodule
```

**testbench**

实例

```verilog
`timescale 1ns/1ns

module test ;
    parameter    N = 8 ;
    parameter    M = 4 ;
    reg          clk, rstn;
 
   //clock
    always begin
        clk = 0 ; #5 ;
        clk = 1 ; #5 ;
    end

   //reset
    initial begin
        rstn      = 1'b0 ;
        #8 ;      rstn      = 1'b1 ;
    end

    =============================================//
    //no pipeline
    reg                  data_rdy_low ;
    reg [N-1:0]          mult1_low ;
    reg [M-1:0]          mult2_low ;
    wire [M+N-1:0]       res_low ;
    wire                 res_rdy_low ;

    //使用任务周期激励
    task mult_data_in ;  
        input [M+N-1:0]   mult1_task, mult2_task ;
        wait(!test.u_mult_low.res_rdy) ;  //not output state
        @(negedge clk ) ;
        data_rdy_low = 1'b1 ;
        mult1_low = mult1_task ;
        mult2_low = mult2_task ;
        @(negedge clk ) ;
        data_rdy_low = 1'b0 ;
        wait(test.u_mult_low.res_rdy) ; //test the output state
    endtask

    //driver
    initial begin
        #55 ;
        mult_data_in(25, 5 ) ;
        mult_data_in(16, 10 ) ;
        mult_data_in(10, 4 ) ;
        mult_data_in(15, 7) ;
        mult_data_in(215, 9) ;
    end 

    mult_low  #(.N(N), .M(M))
    u_mult_low
    (
      .clk              (clk),
      .rstn             (rstn),
      .data_rdy         (data_rdy_low),
      .mult1            (mult1_low),
      .mult2            (mult2_low),
      .res_rdy          (res_rdy_low),
      .res              (res_low));

   //simulation finish
   initial begin
      forever begin
         #100;
         if ($time >= 10000)  $finish ;
      end
   end

endmodule // test
```

仿真结果如下。

由图可知，输入的 2 个数据在延迟 4 个周期后，得到了正确的相乘结果。算上中间送入数据的延迟时间，计算 4 次乘法大约需要 20 个时钟周期。

[![2021071304585810791033](Verilog基础教程/2021071304585810791033.png)](https://zhishitu.com/)

### 流水线乘法器设计

下面对乘法执行过程的中间状态进行保存，以便流水工作，设计代码如下。

单次累加计算过程的代码文件如下（mult_cell.v ）：

实例

```verilog
module    mult_cell
    #(parameter N=4,
      parameter M=4)
    (
      input                     clk,
      input                     rstn,
      input                     en,
      input [M+N-1:0]           mult1,      //被乘数
      input [M-1:0]             mult2,      //乘数
      input [M+N-1:0]           mult1_acci, //上次累加结果

      output reg [M+N-1:0]      mult1_o,     //被乘数移位后保存值
      output reg [M-1:0]        mult2_shift, //乘数移位后保存值
      output reg [N+M-1:0]      mult1_acco,  //当前累加结果
      output reg                rdy );

    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            rdy            <= 'b0 ;
            mult1_o        <= 'b0 ;
            mult1_acco     <= 'b0 ;
            mult2_shift    <= 'b0 ;
        end
        else if (en) begin
            rdy            <= 1'b1 ;
            mult2_shift    <= mult2 >> 1 ;
            mult1_o        <= mult1 << 1 ;
            if (mult2[0]) begin
                //乘数对应位为1则累加
                mult1_acco  <= mult1_acci + mult1 ;  
            end
            else begin
                mult1_acco  <= mult1_acci ; //乘数对应位为1则保持
            end
        end
        else begin
            rdy            <= 'b0 ;
            mult1_o        <= 'b0 ;
            mult1_acco     <= 'b0 ;
            mult2_shift    <= 'b0 ;
        end
    end 

endmodule
```

**顶层例化**

多次模块例化完成多次累加，代码文件如下（mult_man.v ）：

实例

```verilog
module    mult_man
    #(parameter N=4,
      parameter M=4)
    (
      input                     clk,
      input                     rstn,
      input                     data_rdy ,
      input [N-1:0]             mult1,
      input [M-1:0]             mult2,

      output                    res_rdy ,
      output [N+M-1:0]          res );

    wire [N+M-1:0]       mult1_t [M-1:0] ;
    wire [M-1:0]         mult2_t [M-1:0] ;
    wire [N+M-1:0]       mult1_acc_t [M-1:0] ;
    wire [M-1:0]         rdy_t ;

    //第一次例化相当于初始化，不能用 generate 语句
    mult_cell      #(.N(N), .M(M))
    u_mult_step0
    (
      .clk              (clk),
      .rstn             (rstn),
      .en               (data_rdy),
      .mult1            ({{(M){1'b0}}, mult1}),
      .mult2            (mult2),
      .mult1_acci       ({(N+M){1'b0}}),
      //output
      .mult1_acco       (mult1_acc_t[0]),
      .mult2_shift      (mult2_t[0]),
      .mult1_o          (mult1_t[0]),
      .rdy              (rdy_t[0]) );

    //多次模块例化，用 generate 语句
    genvar               i ;
    generate
        for(i=1; i<=M-1; i=i+1) begin: mult_stepx
            mult_cell      #(.N(N), .M(M))
            u_mult_step
            (
              .clk              (clk),
              .rstn             (rstn),
              .en               (rdy_t[i-1]),
              .mult1            (mult1_t[i-1]),
              .mult2            (mult2_t[i-1]),
              //上一次累加结果作为下一次累加输入
              .mult1_acci       (mult1_acc_t[i-1]),
              //output
              .mult1_acco       (mult1_acc_t[i]),                                       
              .mult1_o          (mult1_t[i]),  //被乘数移位状态传递
              .mult2_shift      (mult2_t[i]),  //乘数移位状态传递
              .rdy              (rdy_t[i]) );
        end 
    endgenerate

    assign res_rdy       = rdy_t[M-1];
    assign res           = mult1_acc_t[M-1];

endmodule
```

**testbench**

将下述仿真描述添加到非流水乘法器设计例子的 testbench 中，即可得到流水式乘法运算的仿真结果。

2 路数据为不间断串行输入，且带有自校验模块，可自动判断乘法运算结果的正确性。

实例

```verilog
    reg          data_rdy ;
    reg [N-1:0]  mult1 ;
    reg [M-1:0]  mult2 ;
    wire                 res_rdy ;
    wire [N+M-1:0]       res ;

    //driver
    initial begin
        #55 ;
        @(negedge clk ) ;
        data_rdy  = 1'b1 ;
        mult1  = 25;      mult2      = 5;
        #10 ;      mult1  = 16;      mult2      = 10;
        #10 ;      mult1  = 10;      mult2      = 4;
        #10 ;      mult1  = 15;      mult2      = 7;
        mult2      = 7;   repeat(32)    #10   mult1   = mult1 + 1 ;
        mult2      = 1;   repeat(32)    #10   mult1   = mult1 + 1 ;
        mult2      = 15;  repeat(32)    #10   mult1   = mult1 + 1 ;
        mult2      = 3;   repeat(32)    #10   mult1   = mult1 + 1 ;
        mult2      = 11;  repeat(32)    #10   mult1   = mult1 + 1 ;
        mult2      = 4;   repeat(32)    #10   mult1   = mult1 + 1 ;
        mult2      = 9;   repeat(32)    #10   mult1   = mult1 + 1 ;
    end 

    //对输入数据进行移位，方便后续校验
    reg  [N-1:0]   mult1_ref [M-1:0];
    reg  [M-1:0]   mult2_ref [M-1:0];
    always @(posedge clk) begin
        mult1_ref[0] <= mult1 ;
        mult2_ref[0] <= mult2 ;
    end

    genvar         i ;
    generate
        for(i=1; i<=M-1; i=i+1) begin
            always @(posedge clk) begin
            mult1_ref[i] <= mult1_ref[i-1];
            mult2_ref[i] <= mult2_ref[i-1];
            end
        end
    endgenerate
    
    //自校验
    reg  error_flag ;
    always @(posedge clk) begin
        # 1 ;
        if (mult1_ref[M-1] * mult2_ref[M-1] != res && res_rdy) begin
            error_flag <= 1'b1 ;
        end
        else begin
            error_flag <= 1'b0 ;
        end
    end

    //module instantiation
    mult_man  #(.N(N), .M(M))
     u_mult
     (
      .clk              (clk),
      .rstn             (rstn),
      .data_rdy         (data_rdy),
      .mult1            (mult1),
      .mult2            (mult2),
      .res_rdy          (res_rdy),
      .res              (res));
```

**仿真结果**

前几十个时钟周期的仿真结果如下。

由图可知，仿真结果判断信号 error_flag 一直为 0，表示乘法设计正确。

数据在时钟驱动下不断串行输入，乘法输出结果延迟了 4 个时钟周期后，也源源不断的在每个时钟下无延时输出，完成了流水线式的工作。

[![2021071304585811689714](Verilog基础教程/2021071304585811689714.png)](https://zhishitu.com/)

相对于一般不采用流水线的乘法器，乘法计算效率有了很大的改善。

但是，流水线式乘法器使用的寄存器资源也大约是之前不采用流水线式的 4 倍。

所以，一个数字设计，是否采用流水线设计，需要从资源和效率两方面进行权衡。

# 7.verilog建模实例

## 7.1 Verilog 除法器设计

### 除法器原理（定点）

和十进制除法类似，计算 27 除以 5 的过程如下所示：

![2021071304585917491710](Verilog基础教程/2021071304585917491710.png)

除法运算过程如下：

- (1) 取被除数的高几位数据，位宽和除数相同（实例中是 3bit 数据）。
- (2) 将被除数高位数据与除数作比较，如果前者不小于后者，则可得到对应位的商为 1，两者做差得到第一步的余数；否则得到对应的商为 0，将前者直接作为余数。
- (3) 将上一步中的余数与被除数剩余最高位 1bit 数据拼接成新的数据，然后再和除数做比较。可以得到新的商和余数。
- (4) 重复过程 (3)，直到被除数最低位数据也参与计算。

需要说明的是，商的位宽应该与被除数保持一致，因为除数有可能为1。**所以上述手动计算除法的实例中，第一步做比较时，应该取数字 27 最高位 1 (3'b001) 与 3'b101 做比较。**根据此计算过程，设计位宽可配置的流水线式除法器，流水延迟周期个数与被除数位宽一致。

### 除法器设计

**单步运算设计**

单步除法计算时，单步被除数位宽（信号 dividend）需比原始除数（信号 divisor）位宽多 1bit 才不至于溢出。

为了便于流水，输出端需要有寄存器来存储原始的除数（信号 divisor 和 divisor_kp）和被除数信息（信号 dividend_ci 和 dividend_kp）。

单步的运算结果就是得到新的 1bit 商数据（信号 merchant）和余数（信号 remainder）。

为了得到最后的除法结果，新的 1bit 商数据（信号 merchant）还需要与上一周期的商结果（merchant_ci）进行移位累加。

单步运算单元设计如下（文件名 divider_cell.v）：

### 实例

```
// parameter M means the actual width of divisor
module    divider_cell
    #(parameter N=5,
      parameter M=3)
    (
      input                     clk,
      input                     rstn,
      input                     en,

      input [M:0]               dividend,
      input [M-1:0]             divisor,
      input [N-M:0]             merchant_ci , //上一级输出的商
      input [N-M-1:0]           dividend_ci , //原始除数

      output reg [N-M-1:0]      dividend_kp,  //原始被除数信息
      output reg [M-1:0]        divisor_kp,   //原始除数信息
      output reg                rdy ,
      output reg [N-M:0]        merchant ,  //运算单元输出商
      output reg [M-1:0]        remainder   //运算单元输出余数
    );

    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            rdy            <= 'b0 ;
            merchant       <= 'b0 ;
            remainder      <= 'b0 ;
            divisor_kp     <= 'b0 ;
            dividend_kp    <= 'b0 ;
        end
        else if (en) begin
            rdy            <= 1'b1 ;
            divisor_kp     <= divisor ;  //原始除数保持不变
            dividend_kp    <= dividend_ci ;  //原始被除数传递
            if (dividend >= {1'b0, divisor}) begin
                merchant    <= (merchant_ci<<1) + 1'b1 ; //商为1
                remainder   <= dividend - {1'b0, divisor} ; //求余
            end
            else begin
                merchant    <= merchant_ci<<1 ;  //商为0
                remainder   <= dividend ;        //余数不变
            end
        end // if (en)
        else begin
            rdy            <= 'b0 ;
            merchant       <= 'b0 ;
            remainder      <= 'b0 ;
            divisor_kp     <= 'b0 ;
            dividend_kp    <= 'b0 ;
        end
    end 

endmodule
```

**流水级例化**



将单步计算的余数（信号 remainder）和原始被除数（信号 dividend）对应位的 1bit 数据重新拼接，作为新的单步被除数输入到下一级单步除法计算单元。

其中，被除数、除数、及商的数据信息也要在下一级运算单元中传递。

流水级模块例化完成除法的设计如下（文件名 divider_man.v）:

实例

```
//parameter N means the actual width of dividend
//using 29/5=5...4
module    divider_man
    #(parameter N=5,
      parameter M=3,
      parameter N_ACT = M+N-1)
    (
      input                     clk,
      input                     rstn,

      input                     data_rdy ,  //数据使能
      input [N-1:0]             dividend,   //被除数
      input [M-1:0]             divisor,    //除数

      output                    res_rdy ,
      output [N_ACT-M:0]        merchant ,  //商位宽：N
      output [M-1:0]            remainder ); //最终余数

    wire [N_ACT-M-1:0]   dividend_t [N_ACT-M:0] ;
    wire [M-1:0]         divisor_t [N_ACT-M:0] ;
    wire [M-1:0]         remainder_t [N_ACT-M:0];
    wire [N_ACT-M:0]     rdy_t ;
    wire [N_ACT-M:0]     merchant_t [N_ACT-M:0] ;

    //初始化首个运算单元
    divider_cell      #(.N(N_ACT), .M(M))
       u_divider_step0
    ( .clk              (clk),
      .rstn             (rstn),
      .en               (data_rdy),
      //用被除数最高位 1bit 数据做第一次单步运算的被除数，高位补0
      .dividend         ({{(M){1'b0}}, dividend[N-1]}), 
      .divisor          (divisor),                  
      .merchant_ci      ({(N_ACT-M+1){1'b0}}),   //商初始为0
      .dividend_ci      (dividend[N_ACT-M-1:0]), //原始被除数
      //output
      .dividend_kp      (dividend_t[N_ACT-M]),   //原始被除数信息传递
      .divisor_kp       (divisor_t[N_ACT-M]),    //原始除数信息传递
      .rdy              (rdy_t[N_ACT-M]),
      .merchant         (merchant_t[N_ACT-M]),   //第一次商结果
      .remainder        (remainder_t[N_ACT-M])   //第一次余数
      );

    genvar               i ;
    generate
        for(i=1; i<=N_ACT-M; i=i+1) begin: sqrt_stepx
            divider_cell      #(.N(N_ACT), .M(M))
              u_divider_step
              (.clk              (clk),
               .rstn             (rstn),
               .en               (rdy_t[N_ACT-M-i+1]),
               .dividend         ({remainder_t[N_ACT-M-i+1], dividend_t[N_ACT-M-i+1][N_ACT-M-i]}),   //余数与原始被除数单bit数据拼接
               .divisor          (divisor_t[N_ACT-M-i+1]),
               .merchant_ci      (merchant_t[N_ACT-M-i+1]), 
               .dividend_ci      (dividend_t[N_ACT-M-i+1]), 
               //output
               .divisor_kp       (divisor_t[N_ACT-M-i]),
               .dividend_kp      (dividend_t[N_ACT-M-i]),
               .rdy              (rdy_t[N_ACT-M-i]),
               .merchant         (merchant_t[N_ACT-M-i]),
               .remainder        (remainder_t[N_ACT-M-i])
              );
        end // block: sqrt_stepx
    endgenerate

    assign res_rdy       = rdy_t[0];
    assign merchant      = merchant_t[0];  //最后一次商结果作为最终的商
    assign remainder     = remainder_t[0]; //最后一次余数作为最终的余数

endmodule
```

**testbench**

取被除数位宽为 5，除数位宽为 3，testbench 中加入自校验，描述如下：

### 实例

```
`timescale 1ns/1ns

module test ;
    parameter    N = 5 ;
    parameter    M = 3 ;
    reg          clk;
    reg          rstn ;
    reg          data_rdy ;
    reg [N-1:0]  dividend ;
    reg [M-1:0]  divisor ;

    wire         res_rdy ;
    wire [N-1:0] merchant ;
    wire [M-1:0] remainder ;

    //clock
    always begin
        clk = 0 ; #5 ;
        clk = 1 ; #5 ;
    end

    //driver
    initial begin
        rstn      = 1'b0 ;
        #8 ;
        rstn      = 1'b1 ;

        #55 ;
        @(negedge clk ) ;
        data_rdy  = 1'b1 ;
                dividend  = 25;      divisor      = 5;
        #10 ;   dividend  = 16;      divisor      = 3;
        #10 ;   dividend  = 10;      divisor      = 4;
        #10 ;   dividend  = 15;      divisor      = 1;
        repeat(32)    #10   dividend   = dividend + 1 ;
        divisor      = 7;
        repeat(32)    #10   dividend   = dividend + 1 ;
        divisor      = 5;
        repeat(32)    #10   dividend   = dividend + 1 ;
        divisor      = 4;
        repeat(32)    #10   dividend   = dividend + 1 ;
        divisor      = 6;
        repeat(32)    #10   dividend   = dividend + 1 ;
    end

    //对输入延迟，便于数据结果同周期对比，完成自校验
    reg  [N-1:0]   dividend_ref [N-1:0];
    reg  [M-1:0]   divisor_ref [N-1:0];
    always @(posedge clk) begin
        dividend_ref[0] <= dividend ;
        divisor_ref[0]  <= divisor ;
    end

    genvar         i ;
    generate
        for(i=1; i<=N-1; i=i+1) begin
            always @(posedge clk) begin
                dividend_ref[i] <= dividend_ref[i-1];
                divisor_ref[i]  <= divisor_ref[i-1];
            end
        end
    endgenerate

    //自校验
    reg  error_flag ;
    always @(posedge clk) begin
    # 1 ;
        if (merchant * divisor_ref[N-1] + remainder != dividend_ref[N-1] && res_rdy) beginb      //testbench 中可直接用乘号而不考虑运算周期
            error_flag <= 1'b1 ;
        end
        else begin
            error_flag <= 1'b0 ;
        end
    end

    //module instantiation
    divider_man  #(.N(N), .M(M))
    u_divider
     (
      .clk              (clk),
      .rstn             (rstn),
      .data_rdy         (data_rdy),
      .dividend         (dividend),
      .divisor          (divisor),
      .res_rdy          (res_rdy),
      .merchant         (merchant),
      .remainder        (remainder));

   //simulation finish
   initial begin
      forever begin
         #100;
         if ($time >= 10000)  $finish ;
      end
   end

endmodule // test
```

**仿真结果**

由图可知，2 个输入数据在延迟了和被除数相同位宽的周期数以后，输出了正确的除法结果。而且可流水式无延迟输出，符合设计。

[![2021071304585918257581](Verilog基础教程/2021071304585918257581.png)](https://zhishitu.com/)

### 源码下载

## 7.2 Verilog 并行 FIR 滤波器设计

- 

  分类：[计算机](https://xiaotua.com/jisuanji.html)

   最后更新: 2021年7月13日

FIR（Finite Impulse Response）滤波器是一种有限长单位冲激响应滤波器，又称为非递归型滤波器。

FIR 滤波器具有严格的线性相频特性，同时其单位响应是有限长的，因而是稳定的系统，在数字通信、图像处理等领域都有着广泛的应用。

### FIR 滤波器原理

FIR 滤波器是有限长单位冲击响应滤波器。直接型结构如下：

![2021071304590113804770](Verilog基础教程/2021071304590113804770.png)

FIR 滤波器本质上就是输入信号与单位冲击响应函数的卷积，表达式如下：

![2021071304590114419121](Verilog基础教程/2021071304590114419121.png)

FIR 滤波器有如下几个特性：

- (1) 响应是有限长序列。
- (2) 系统函数在 |z| > 0 处收敛，极点全部在 z=0 处，属于因果系统。
- (3) 结构上是非递归的，没有输出到输入的反馈。
- (4) 输入信号相位响应是线性的，因为响应函数 h(n) 系数是对称的。
- (5) 输入信号的各频率之间，相对相位差也是固定不变的。
- (6) 时域卷积等于频域相乘，因此该卷积相当于筛选频谱中各频率分量的增益倍数。某些频率分量保留，某些频率分量衰减，从而实现滤波的效果。

### 并行 FIR 滤波器设计

**设计说明**

输入频率为 7.5 MHz 和 250 KHz 的正弦波混合信号，经过 FIR 滤波器后，高频信号 7.5MHz 被滤除，只保留 250KHz 的信号。设计参数如下：

```
输入频率：    7.5MHz 和 250KHz采样频率：    50MHz阻带：           1MHz ~ 6MHz阶数：           15（N-1=15）
```

由 FIR 滤波器结构可知，阶数为 15 时，FIR 的实现需要 16 个乘法器，15 个加法器和 15 组延时寄存器。为了稳定第一拍的数据，可以再多用一组延时寄存器，即共用 16 组延时寄存器。由于 FIR 滤波器系数的对称性，乘法器可以少用一半，即共使用 8 个乘法器。

并行设计，就是在一个时钟周期内对 16 个延时数据同时进行乘法、加法运算，然后在时钟驱动下输出滤波值。这种方法的优点是滤波延时短，但是对时序要求比较高。

**并行设计**

设计中使用到的乘法器模块代码，可参考之前流水线式设计的乘法器。

为方便快速仿真，也可以直接使用乘号 * 完成乘法运算，设计中加入宏定义 SAFE_DESIGN 来选择使用哪种乘法器。

FIR 滤波器系数可由 matlab 生成，具体见附录。

### 实例

```
/***********************************************************
>> V201001 : Fs：50Mhz, fstop：1Mhz-6Mhz, order： 15
************************************************************/
`define SAFE_DESIGN
 
module fir_guide    (
    input                rstn,  //复位，低有效
    input                clk,   //工作频率，即采样频率
    input                en,    //输入数据有效信号
    input        [11:0]  xin,   //输入混合频率的信号数据
    output               valid, //输出数据有效信号
    output       [28:0]  yout   //输出数据，低频信号，即250KHz
    );
 
    //data en delay 
    reg [3:0]            en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            en_r[3:0]      <= 'b0 ;
        end
        else begin
            en_r[3:0]      <= {en_r[2:0], en} ;
        end
    end
 
   //(1) 16 组移位寄存器
    reg        [11:0]    xin_reg[15:0];
    reg [3:0]            i, j ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            for (i=0; i<15; i=i+1) begin
                xin_reg[i]  <= 12'b0;
            end
        end
        else if (en) begin
            xin_reg[0] <= xin ;
            for (j=0; j<15; j=j+1) begin
                xin_reg[j+1] <= xin_reg[j] ; //周期性移位操作
            end
        end
    end
 
   //Only 8 multipliers needed because of the symmetry of FIR filter coefficient
   //(2) 系数对称，16个移位寄存器数据进行首位相加
    reg        [12:0]    add_reg[7:0];
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            for (i=0; i<8; i=i+1) begin
                add_reg[i] <= 13'd0 ;
            end
        end
        else if (en_r[0]) begin
            for (i=0; i<8; i=i+1) begin
                add_reg[i] <= xin_reg[i] + xin_reg[15-i] ;
            end
        end
    end
 
    //(3) 8个乘法器
    // 滤波器系数，已经过一定倍数的放大
    wire        [11:0]   coe[7:0] ;
    assign coe[0]        = 12'd11 ;
    assign coe[1]        = 12'd31 ;
    assign coe[2]        = 12'd63 ;
    assign coe[3]        = 12'd104 ;
    assign coe[4]        = 12'd152 ;
    assign coe[5]        = 12'd198 ;
    assign coe[6]        = 12'd235 ;
    assign coe[7]        = 12'd255 ;
    reg        [24:0]   mout[7:0]; 
 
`ifdef SAFE_DESIGN
    //流水线式乘法器
    wire [7:0]          valid_mult ;
    genvar              k ;
    generate
        for (k=0; k<8; k=k+1) begin
            mult_man #(13, 12)
            u_mult_paral          (
              .clk        (clk),
              .rstn       (rstn),
              .data_rdy   (en_r[1]),
              .mult1      (add_reg[k]),
              .mult2      (coe[k]),
              .res_rdy    (valid_mult[k]), //所有输出使能完全一致  
              .res        (mout[k])
            );
        end
    endgenerate
    wire valid_mult7     = valid_mult[7] ;
 
`else
    //如果对时序要求不高，可以直接用乘号
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            for (i=0 ; i<8; i=i+1) begin
                mout[i]     <= 25'b0 ;
            end
        end
        else if (en_r[1]) begin
            for (i=0 ; i<8; i=i+1) begin
                mout[i]     <= coe[i] * add_reg[i] ;
            end
        end
    end
    wire valid_mult7 = en_r[2];
`endif
 
    //(4) 积分累加，8组25bit数据 -> 1组 29bit 数据
    //数据有效延时
    reg [3:0]            valid_mult_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            valid_mult_r[3:0]  <= 'b0 ;
        end
        else begin
            valid_mult_r[3:0]  <= {valid_mult_r[2:0], valid_mult7} ;
        end
    end

`ifdef SAFE_DESIGN
    //加法运算时，分多个周期进行流水，优化时序
    reg        [28:0]    sum1 ;
    reg        [28:0]    sum2 ;
    reg        [28:0]    yout_t ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            sum1   <= 29'd0 ;
            sum2   <= 29'd0 ;
            yout_t <= 29'd0 ;
        end
        else if(valid_mult7) begin
            sum1   <= mout[0] + mout[1] + mout[2] + mout[3] ;
            sum2   <= mout[4] + mout[5] + mout[6] + mout[7] ;
            yout_t <= sum1 + sum2 ;
        end
    end
 
`else 
    //一步计算累加结果，但是实际中时序非常危险
    reg signed [28:0]    sum ;
    reg signed [28:0]    yout_t ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            sum    <= 29'd0 ;
            yout_t <= 29'd0 ;
        end
        else if (valid_mult7) begin
            sum    <= mout[0] + mout[1] + mout[2] + mout[3] + mout[4] + mout[5] + mout[6] + mout[7];
            yout_t <= sum ;
        end
    end 
`endif
    assign yout  = yout_t ;
    assign valid = valid_mult_r[0];

endmodule 
```

**testbench**

testbench 编写如下，主要功能就是不间断连续的输入 250KHz 与 7.5MHz 的正弦波混合信号数据。输入的混合信号数据也可由 matlab 生成，具体见附录。

### 实例

```
`timescale 1ps/1ps
 
module test ;
   //input
    reg          clk ;
    reg          rst_n ;
    reg          en ;
    reg [11:0]   xin ;
    //output
    wire         valid ;
    wire [28:0]  yout ;
 
    parameter    SIMU_CYCLE   = 64'd2000 ;  //50MHz 采样频率
    parameter    SIN_DATA_NUM = 200 ;      //仿真周期

//=====================================
// 50MHz clk generating
    localparam   TCLK_HALF     = 10_000;
    initial begin
        clk = 1'b0 ;
        forever begin
            # TCLK_HALF ;
            clk = ~clk ;
        end
    end
 
//============================
//  reset and finish
    initial begin
        rst_n = 1'b0 ;
        # 30   rst_n = 1'b1 ;
        # (TCLK_HALF * 2 * SIMU_CYCLE) ;
        $finish ;
    end
 
//=======================================
// read signal data into register
    reg          [11:0] stimulus [0: SIN_DATA_NUM-1] ;
    integer      i ;
    initial begin
        $readmemh("../tb/cosx0p25m7p5m12bit.txt", stimulus) ;
        i = 0 ;
        en = 0 ;
        xin = 0 ;
        # 200 ;
        forever begin
            @(negedge clk) begin
                en          = 1'b1 ;
                xin         = stimulus[i] ;
                if (i == SIN_DATA_NUM-1) begin  //周期送入数据控制
                    i = 0 ;
                end
                else begin
                    i = i + 1 ;
                end
            end
        end 
    end 
 
    fir_guide u_fir_paral (
      .xin         (xin),
      .clk         (clk),
      .en          (en),
      .rstn        (rst_n),
      .valid       (valid),
      .yout        (yout));
 
endmodule
```

**仿真结果**

由下图仿真结果可知，经过 FIR 滤波器后的信号只有一种低频率信号（250KHz），高频信号（7.5MHz）被滤除了。而且输出波形是连续的，能够持续输出。

但是，如红圈所示，波形起始部分呈不规则状态，对此进行放大。

![2021071304590215446052](Verilog基础教程/2021071304590215446052.png)

波形起始端放大后如下图所示，可见不规则波形的时间段，即两根竖线之间的时间间隔是 16 个时钟周期。

因为数据是串行输入，设计中使用了 16 组延时寄存器，所以滤波后的第一个正常点应该较第一个滤波数据输出时刻延迟 16 个时钟周期。即数据输出有效信号 valid 应该再延迟 16 个时钟周期，则会使输出波形更加完美。

![2021071304590216553483](Verilog基础教程/2021071304590216553483.png)

### 附录：matlab 使用

**生成 FIR 滤波器系数**

打开 matlab，在命令窗口输入命令： fdatool。

然后会打开如下窗口，按照 FIR 滤波器参数进行设置。

这里选择的 FIR 实现方法是最小二乘法（Least-squares），不同的实现方式滤波效果也不同。

![2021071304590219410974](Verilog基础教程/2021071304590219410974.png)

点击 File -> Export

将滤波器参数输出，存到变量 coef 中，如下图所示。

![2021071304590310491225](Verilog基础教程/2021071304590310491225.png)

此时 coef 变量应该是浮点型数据。对其进行一定倍数的相乘扩大，然后取其近似的定点型数据作为设计中的 FIR 滤波器参数。这里取扩大倍数为 2048，结果如下所示。

![2021071304590311240376](Verilog基础教程/2021071304590311240376.png)

**生成输入的混合信号**

利用 matlab 生成混合的输入信号参考代码如下。

信号为无符号定点型数据，位宽宽度为 12bit，存于文件 **cosx0p25m7p5m12bit.txt**。

### 实例

```
clear all;close all;clc;
%=======================================================
% generating a cos wave data with txt hex format
%=======================================================

fc          = 0.25e6 ;      % 中心频率
fn          = 7.5e6 ;       % 杂波频率
Fs          = 50e6 ;        % 采样频率
T           = 1/fc ;        % 信号周期
Num         = Fs * T ;      % 周期内信号采样点数
t           = (0:Num-1)/Fs ;      % 离散时间
cosx        = cos(2*pi*fc*t) ;    % 中心频率正弦信号
cosn        = cos(2*pi*fn*t) ;    % 杂波信号
cosy        = mapminmax(cosx + cosn) ;     %幅值扩展到（-1,1） 之间
cosy_dig    = floor((2^11-1) * cosy + 2^11) ;     %幅值扩展到 0~4095
fid         = fopen('cosx0p25m7p5m12bit.txt', 'wt') ;  %写数据文件
fprintf(fid, '%x\n', cosy_dig) ;
fclose(fid) ;
 
%时域波形
figure(1);
subplot(121);plot(t,cosx);hold on ;
plot(t,cosn) ;
subplot(122);plot(t,cosy_dig) ;
 
%频域波形
fft_cosy    = fftshift(fft(cosy, Num)) ;
f_axis      = (-Num/2 : Num/2 - 1) * (Fs/Num) ;
figure(5) ;
plot(f_axis, abs(fft_cosy)) ;
```

### 源码下载



## 7.3 Verilog 串行 FIR 滤波器设计

- 

  分类：[计算机](https://xiaotua.com/jisuanji.html)

   最后更新: 2021年7月13日

### 串行 FIR 滤波器设计

**设计说明**

设计参数不变，与并行 FIR 滤波器参数一致。即，输入频率为 7.5 MHz 和 250 KHz 的正弦波混合信号，经过 FIR 滤波器后，高频信号 7.5MHz 被滤除，只保留 250KMHz 的信号。

```
输入频率：    7.5MHz 和 250KHz采样频率：    50MHz阻带：           1MHz-6MHz阶数：           15 （N=15）
```

串行设计，就是在 16 个时钟周期内对 16 个延时数据分时依次进行乘法、加法运算，然后在时钟驱动下输出滤波值。考虑到 FIR 滤波器系数的对称性，计算一个滤波输出值的周期可以减少到 8 个。串行设计时每个周期只进行一次乘法运算，所以设计中只需一个乘法器即可。此时数据需要每 8 个时钟周期有效输入一次，但是为了保证输出信号频率的正确性，工作时钟需要为采样频率的 8 倍，即 400MHz。这种方法的优点是资源耗费少，但是工作频率要求高，数据不能持续输出。

**串行设计**

设计中使用到的乘法器模块代码，可参考之前流水线式设计的乘法器。

为方便快速仿真，也可以直接使用乘号 * 完成乘法运算，设计中加入宏定义 SAFE_DESIGN 来选择使用哪种乘法器。

FIR 滤波器系数可由 matlab 生成，具体见附录。

### 实例

```
/**********************************************************
>> Description : fir study with serial tech
>> V190403     : Fs:50Mhz, fstop:1-6Mhz, order:16, sys clk:400MHz
***********************************************************/
`define SAFE_DESIGN
 
module fir_serial_low(
    input                rstn,
    input                clk,   // 系统工作时钟，400MHz
    input                en ,   // 输入数据有效信号
    input        [11:0]  xin,   // 输入混合频率的信号数据
    output               valid, // 输出数据有效信号
    output       [28:0]  yout   // 输出数据
    );
 
   //delay of input data enable
    reg [11:0]            en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            en_r[11:0]      <= 'b0 ;
        end
        else begin
            en_r[11:0]      <= {en_r[10:0], en} ;
        end
    end
 
    //fir coeficient
    wire        [11:0]   coe[7:0] ;
    assign coe[0]        = 12'd11 ;
    assign coe[1]        = 12'd31 ;
    assign coe[2]        = 12'd63 ;
    assign coe[3]        = 12'd104 ;
    assign coe[4]        = 12'd152 ;
    assign coe[5]        = 12'd198 ;
    assign coe[6]        = 12'd235 ;
    assign coe[7]        = 12'd255 ;
  
    //(1) 输入数据移位部分 
    reg [2:0]            cnt ;
    integer              i, j ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            cnt <= 3'b0 ;
        end
        else if (en || cnt != 0) begin
            cnt <= cnt + 1'b1 ;    //8个周期计数
        end
    end
 
    reg [11:0]           xin_reg[15:0];
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            for (i=0; i<16; i=i+1) begin
                xin_reg[i]  <= 12'b0;
            end
        end
        else if (cnt == 3'd0 && en) begin    //每8个周期读入一次有效数据
            xin_reg[0] <= xin ;
            for (j=0; j<15; j=j+1) begin
                xin_reg[j+1] <= xin_reg[j] ; // 数据移位
            end
        end
    end
  
    //(2) 系数对称，16个移位寄存器数据进行首位相加
    reg  [11:0]          add_a, add_b ;
    reg  [11:0]          coe_s ;
    wire [12:0]          add_s ;
    wire [2:0]           xin_index = cnt>=1 ? cnt-1 : 3'd7 ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            add_a  <= 13'b0 ;
            add_b  <= 13'b0 ;
            coe_s  <= 12'b0 ;
        end
        else if (en_r[xin_index]) begin //from en_r[1]
            add_a  <= xin_reg[xin_index] ;
            add_b  <= xin_reg[15-xin_index] ;
            coe_s  <= coe[xin_index] ;
        end
    end
    assign add_s = {add_a} + {add_b} ;  
 
    //(3) 乘法运算，只用一个乘法
    reg        [24:0]    mout ;
`ifdef SAFE_DESIGN
    wire                 en_mult ;
    wire [3:0]           index_mult = cnt>=2 ? cnt-1 : 4'd7 + cnt[0] ;
    mult_man #(13, 12)   u_mult_single    //例化自己设计的流水线乘法器
        (.clk        (clk),
         .rstn       (rstn),
         .data_rdy   (en_r[index_mult]),  //注意数据时序对应
         .mult1      (add_s),
         .mult2      (coe_s),
         .res_rdy    (en_mult),   
         .res        (mout)
        );
 
`else
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            mout   <= 25'b0 ;
        end
        else if (|en_r[8:1]) begin
            mout   <= coe_s * add_s ;  //直接乘
        end
    end
    wire                 en_mult = en_r[2];
`endif
 
    //(4) 积分累加，8组25bit数据 -> 1组 29bit 数据
    reg        [28:0]    sum ;
    reg                  valid_r ;
    //mult output en counter
    reg [4:0]            cnt_acc_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            cnt_acc_r <= 'b0 ;
        end
        else if (cnt_acc_r == 5'd7) begin  //计时8个周期
            cnt_acc_r <= 'b0 ;
        end
        else if (en_mult || cnt_acc_r != 0) begin //只要en有效，计时不停
            cnt_acc_r <= cnt_acc_r + 1'b1 ;
        end
    end
 
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            sum      <= 29'd0 ;
            valid_r  <= 1'b0 ;
        end
        else if (cnt_acc_r == 5'd7) begin //在第8个累加周期输出滤波值
            sum      <= sum + mout;
            valid_r  <= 1'b1 ;
        end
        else if (en_mult && cnt_acc_r == 0) begin //初始化
            sum      <= mout ;
            valid_r  <= 1'b0 ;
        end
        else if (cnt_acc_r != 0) begin //acculating between cycles
            sum      <= sum + mout ;
            valid_r  <= 1'b0 ;
        end
    end
 
    //时钟锁存有效的输出数据，为了让输出信号不是那么频繁的变化
    reg [28:0]           yout_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            yout_r <= 'b0 ;
        end
        else if (valid_r) begin
            yout_r <= sum ;
        end
    end
    assign yout = yout_r ;
 
    //(5) 输出数据有效延迟，即滤波数据丢掉前15个滤波值
    reg [4:0]    cnt_valid ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            cnt_valid      <= 'b0 ;
        end
        else if (valid_r && cnt_valid != 5'd16) begin
            cnt_valid      <= cnt_valid + 1'b1 ;
        end
    end
    assign valid = (cnt_valid == 5'd16) & valid_r ;

endmodule
```

**testbench**

testbench 编写如下，主要功能就是不间断连续的输入 250KHz 与 7.5MHz 的正弦波混合信号数据。输入的混合信号数据也可由 matlab 生成，具体见附录。

其中，工作频率为 400MHz，但输入数据和输入数据有效信号应当都保持 50MHz 的频率输入。

### 实例

```
module test ;
    //input
    reg          clk ;
    reg          rst_n ;
    reg          en ;
    reg  [11:0]  xin ;
    //output
    wire [28:0]  yout ;
    wire         valid ;

    parameter    SIMU_CYCLE   = 64'd1000 ;
    parameter    SIN_DATA_NUM = 200 ;

//=====================================
// 8*50MHz clk generating
    localparam   TCLK_HALF     = (10_000 >>3);
    initial begin
        clk = 1'b0 ;
        forever begin
            # TCLK_HALF clk = ~clk ;
        end
     end
 
//============================
//  reset and finish
    initial begin
        rst_n = 1'b0 ;
        # 30        rst_n = 1'b1 ;
        # (TCLK_HALF * 2 * 8  * SIMU_CYCLE) ;
        $finish ;
    end
 
//=======================================
// read cos data into register
    reg          [11:0] stimulus [0: SIN_DATA_NUM-1] ;
    integer      i ;
    initial begin
        $readmemh("../tb/cosx0p25m7p5m12bit.txt", stimulus) ;
        en = 0 ;
        i = 0 ;
        xin = 0 ;
        # 200 ;
        forever begin
            repeat(7)  @(negedge clk) ; //空置7个周期，第8个周期给数据
            en          = 1 ;
            xin         = stimulus[i] ;
            @(negedge clk) ;
            en          = 0 ;         //输入数据有效信号只持续一个周期即可
            if (i == SIN_DATA_NUM-1)  i = 0 ;
            else  i = i + 1 ;
        end 
    end 
 
    fir_serial_low       u_fir_serial (
        .clk         (clk),
        .rstn        (rst_n),
        .en          (en),
        .xin         (xin),
        .valid       (valid),
        .yout        (yout));

endmodule 
```

**仿真结果**

由下图仿真结果可知，经过 FIR 滤波器后的信号只有一种低频率信号（250KHz），高频信号（7.5MHz）被滤除了。为了波形更加的美观，取 16 个之后的滤波数据作为有效输出。

![2021071304590416073830](Verilog基础教程/2021071304590416073830.png)

波形局部放大后如下图所示，此时输入数据有效信号 en 与输出数据有效信号 valid 是周期（50MHz）相同的脉冲信号，不是持续有效的。但工作时钟为 400MHz，所以输出也会呈现为 50MHz 采样频率下的 250KHz 频率的正弦波信号。

![2021071304590516396761](Verilog基础教程/2021071304590516396761.png)

### 附录：matlab 使用（与[《并行 FIR 滤波器设计》](https://zhishitu.com/)一致）

**生成 FIR 滤波器系数**

打开 matlab，在命令窗口输入命令： fdatool。

然后会打开如下窗口，按照 FIR 滤波器参数进行设置，如下所示。

这里选择的 FIR 实现方法是最小二乘法（Least-squares），不同的实现方式滤波效果也不同。

![2021071304590614439122](Verilog基础教程/2021071304590614439122.png)

点击 File -> Export

将滤波器参数输出，存到变量 coef 中，如下图所示。

![2021071304590712366593](Verilog基础教程/2021071304590712366593.png)

此时 coef 变量应该是浮点型数据。对其进行一定倍数的相乘扩大，然后取其近似的定点型数据作为设计中的 FIR 滤波器参数。这里取扩大倍数为 2048，结果如下所示。

![2021071304590714287964](Verilog基础教程/2021071304590714287964.png)

**生成输入的混合信号**

利用 matlab 生成混合的输入信号参考代码如下。

信号为无符号定点型数据，位宽宽度为 12bit，存于文件 **cosx0p25m7p5m12bit.txt** 。

### 实例

```
clear all;close all;clc;
%=======================================================
% generating a cos wave data with txt hex format
%=======================================================

fc          = 0.25e6 ;      % 中心频率
fn          = 7.5e6 ;       % 杂波频率
Fs          = 50e6 ;        % 采样频率
T           = 1/fc ;        % 信号周期
Num         = Fs * T ;      % 周期内信号采样点数
t           = (0:Num-1)/Fs ;      % 离散时间
cosx        = cos(2*pi*fc*t) ;    % 中心频率正弦信号
cosn        = cos(2*pi*fn*t) ;    % 杂波信号
cosy        = mapminmax(cosx + cosn) ;     %幅值扩展到（-1,1） 之间
cosy_dig    = floor((2^11-1) * cosy + 2^11) ;     %幅值扩展到 0~4095
fid         = fopen('cosx0p25m7p5m12bit.txt', 'wt') ;  %写数据文件
fprintf(fid, '%x\n', cosy_dig) ;
fclose(fid) ;
 
%时域波形
figure(1);
subplot(121);plot(t,cosx);hold on ;
plot(t,cosn) ;
subplot(122);plot(t,cosy_dig) ;
 
%频域波形
fft_cosy    = fftshift(fft(cosy, Num)) ;
f_axis      = (-Num/2 : Num/2 - 1) * (Fs/Num) ;
figure(5) ;
plot(f_axis, abs(fft_cosy)) ;
```

### 源码下载

## 7.4 Verilog CIC 滤波器设计

- 

  分类：[计算机](https://xiaotua.com/jisuanji.html)

   最后更新: 2021年7月13日

积分梳状滤波器（CIC，Cascaded Integrator Comb），一般用于数字下变频（DDC）和数字上变频（DUC）系统。CIC 滤波器结构简单，没有乘法器，只有加法器、积分器和寄存器，资源消耗少，运算速率高，可实现高速滤波，常用在输入采样率最高的第一级，在多速率信号处理系统中具有着广泛应用。

### DDC 原理

**DDC 工作原理**

DDC 主要由本地振荡器（NCO） 、混频器、滤波器等组成，如下图所示。

![2021071304590819364370](Verilog基础教程/2021071304590819364370.png)

DDC 将中频信号与振荡器产生的载波信号进行混频，信号中心频率被搬移，再经过抽取滤波，恢复原始信号，实现了下变频功能。

中频数据采样时，需要很高的采样频率来确保 ADC（模数转换器）采集到信号的信噪比。经过数字下变频后，得到的基带信号采样频率仍然是 ADC 采样频率，所以数据率很高。此时基带信号的有效带宽往往已经远小于采样频率，所以利用抽取、滤波进行数据速率的转换，使采样率降低，避免资源的浪费和设计的困难，就成为 DDC 不可缺少的一部分。

而采用 CIC 滤波器进行数据处理，是 DDC 抽取滤波部分最常用的方法。

**带通采样定理**

在 DDC 系统中，输入的中频载波信号会根据载波频率进行频移，得到一个带通信号。如果此时仍然采用奈奎斯特采样定理，即采样频率为带通信号最高频率的两倍，那么此时所需的采样频率将会很高，设计会变的复杂。此时可按照带通采样定理来确定抽样频率。

带通采样定理：一个频带限制在![2021071304590915989271](Verilog基础教程/2021071304590915989271.png)的连续带通信号，带宽为![2021071304591013033382](Verilog基础教程/2021071304591013033382.png)。令![2021071304591019691843](Verilog基础教程/2021071304591019691843.png) ，其中 N 为不大于 ![2021071304591117006904](Verilog基础教程/2021071304591117006904.png)的最大正整数，如果采样频率满足条件：

![2021071304591215746465](Verilog基础教程/2021071304591215746465.png)

则该信号完全可以由其采样值无失真的重建。

当 m=1 时，带通采样定理便是奈奎斯特采样定理。

带通采样定理的另一种描述方式为：若信号最高频率为信号带宽的整数倍，采样频率只需大于信号带宽的两倍即可，此时不会发生频谱混叠。

所以，可以认为采样频率的一半是 CIC 滤波器的截止频率。

**DDC 频谱搬移**

例如一个带宽信号中心频率为 60MHz，带宽为 8MHz, 则频率范围为 56MHz ~ 64MHz，m 的可取值范围为 0 ~ 7。取 m=1, 则采样频率范围为 64MHz ~ 112MHz。

取采样频率为 80MHz，设 NCO 中心频率为 20 MHz，下面讨论复信号频谱搬移示意图。

（1）考虑频谱的对称性，输入复信号的频谱示意图如下：

![2021071304591317771846](Verilog基础教程/2021071304591317771846.png)

（2）80MHz 采样频率采样后，56~64MHz 的频带被搬移到了 -24~ -16MHz 与 136 ~ 144MHz（高于采样频率被滤除）的频带处，-64~ -56MHz 的频带被搬移到 -144~ -136MHz（高于采样频率被滤除）与 16~24MHz 的频带处。

采样后频带分布如下：

![2021071304591415934277](Verilog基础教程/2021071304591415934277.png)

（3）信号经过 20MHz NCO 的正交电路后， -24~ -16MHz 的频带被搬移到 -4~4MHz 与 -44~ -36MHz 的频带处，16~24MHz 的频带被搬移到 -4~4MHz 与 36~44MHz 的频带处，如下所示。

![2021071304591512937858](Verilog基础教程/2021071304591512937858.png)

（4）此时中频输入的信号已经被搬移到零中频基带处。

-44~ -36MHz 和 36~44MHz 的带宽信号是不需要的，可以滤除；-4~4MHz 的零中频信号数据速率仍然是 80MHz，可以进行抽取降低数据速率。而 CIC 滤波，就是要完成这个过程。

上述复习了很多数字信号处理的内容，权当抛 DDC 的砖，引 CIC 的玉。

### CIC 滤波器原理

**单级 CIC 滤波器**

设滤波器抽取倍数为 D，则单级滤波器的冲激响应为：

![2021071304591610720159](Verilog基础教程/2021071304591610720159.png)

对其进行 z 变换，可得单级 CIC 滤波器的系统函数为：

![20210713045916173368110](Verilog基础教程/20210713045916173368110.png)

令

![20210713045916179585011](Verilog基础教程/20210713045916179585011.png)

可以看出，单级 CIC 滤波器包括两个基本组成部分：积分部分和梳状部分，结构图如下：

![20210713045917157751212](Verilog基础教程/20210713045917157751212.png)

**积分器**

积分器是一个单级点的 IIR（Infinite Impulse Response，无限长脉冲冲激响应）滤波器，且反馈系数为 1，其状态方程和系统函数分别为：

![20210713045918130578013](Verilog基础教程/20210713045918130578013.png)

![20210713045919169344914](Verilog基础教程/20210713045919169344914.png)

**梳状器**

梳状器是一个 FIR 滤波器，其状态方程和系统函数分别为：

![20210713045920147580915](Verilog基础教程/20210713045920147580915.png)

![20210713045921115067216](Verilog基础教程/20210713045921115067216.png)

**抽取器**

在积分器之后，还有一个抽取器，抽取倍数与梳状器的延时参数是一致的。利用 z 变换的性质进行恒等变换，将抽取器移动到积分器与梳状器之间，可得到单级 CIC 滤波器结构，如下所示。

![20210713045921184578517](Verilog基础教程/20210713045921184578517.png)

**参数说明**

CIC 滤波器结构变换之前的参数 D 可以理解为梳状滤波器的延时或阶数；变换之后，D 的含义 变为抽取倍数，而此时梳状滤波器的延时为 1，即阶数为 1。

很多学者会引入一个变量 M，表示梳状器每一级的延时，此时梳妆部分的延时就不为 1 了。那么梳状器的系统函数就变为：

![20210713045923124137818](Verilog基础教程/20210713045923124137818.png)

其实把 DM 整体理解为单级滤波器延时，或者抽取倍数，也都是可以的。可能实现的方式或结构不同，但是最后的结果都是一样的。本次设计中，单级滤波器延时都为 M=1，即抽取倍数与滤波延时相同。

**多级 CIC 滤波器**

单级 CIC 滤波器的阻带衰减较差，为了提高滤波效果，抽取滤波时往往会采用多级 CIC 滤波器级联的结构。

实现多级直接级联的 CIC 滤波器在设计和资源上并不是最优的方式，需要对其结构进行调整。如下所示，将积分器和梳状滤波器分别移至一组，并将抽取器移到梳状滤波器之前。先抽取再进行滤波，可以减少数据处理的长度，节约硬件资源。

![20210713045923192369019](Verilog基础教程/20210713045923192369019.jpg)

当然，级联数越大，旁瓣抑制越好，但是通带内的平坦度也会变差。所以级联数不宜过多，一般最多 5 级。

### CIC 滤波器设计

**设计说明**

CIC 滤波器本质上就是一个简单的低通滤波器，截止频率为采样频率除以抽取倍数后的一半。输入数据信号仍然是 7.5MHz 和 250KHz，采样频率 50MHz。抽取倍数设置为 5，则截止频率为 5MHz，小于 7.5MHz，可以滤除 7.5MHz 的频率成分。设计参数如下：

```
输入频率：    7.5MHz 和 250KHz采样频率：    50MHz阻带：           5MHz 阶数：           1（M=1）级数：           3（N=3） 
```

关于积分时中间数据信号的位宽，很多地方给出了不同的计算方式，计算结果也大相径庭。这里总结一下使用最多的计算方式：

![20210713045924162637720](Verilog基础教程/20210713045924162637720.png)

其中，D 为抽取倍数，M 为滤波器阶数，N 为滤波器级数。抽取倍数为 5，滤波器阶数为 1，滤波器级联数为 3，取输入信号数据位宽为 12bit，对数部分向上取整，则积分后数据不溢出的中间信号位宽为 21bit。

为了更加宽裕的设计，滤波器阶数如果理解为未变换结构前的多级 CIC 滤波器直接型结构，则滤波器阶数可以认为是 5，此时中间信号最大位宽为 27bit。

**积分器设计**

根据输入数据的有效信号的控制，积分器做一个简单的累加即可，注意数据位宽。

### 实例

```
//3 stages integrator
module integrator
    #(parameter NIN     = 12,
      parameter NOUT    = 21)
    (
      input               clk ,
      input               rstn ,
      input               en ,
      input [NIN-1:0]     din ,
      output              valid ,
      output [NOUT-1:0]   dout) ;

    reg [NOUT-1:0]         int_d0  ;
    reg [NOUT-1:0]         int_d1  ;
    reg [NOUT-1:0]         int_d2  ;
    wire [NOUT-1:0]        sxtx = {{(NOUT-NIN){1'b0}}, din} ;

    //data input enable delay
    reg [2:0]              en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            en_r   <= 'b0 ;
        end
        else begin
            en_r   <= {en_r[1:0], en};
        end
    end

    //integrator
    //stage1
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            int_d0        <= 'b0 ;
        end
        else if (en) begin
            int_d0        <= int_d0 + sxtx ;
        end
    end

    //stage2
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            int_d1        <= 'b0 ;
        end
        else if (en_r[0]) begin
            int_d1        <= int_d1 + int_d0 ;
        end
    end

   //stage3
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            int_d2        <= 'b0 ;
        end
        else if (en_r[1]) begin
            int_d2        <= int_d2 + int_d1 ;
        end
    end
    assign dout  = int_d2 ;
    assign valid = en_r[2];

endmodule
```

**抽取器设计**

抽取器设计时，对积分器输出的数据进行计数，然后间隔 5 个数据进行抽取即可。

### 实例

```
module  decimation
    #(parameter NDEC = 21)
    (
     input                clk,
     input                rstn,
     input                en,
     input [NDEC-1:0]     din,
     output               valid,
     output [NDEC-1:0]    dout);

    reg                  valid_r ;
    reg [2:0]            cnt ;
    reg [NDEC-1:0]       dout_r ;

    //counter
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            cnt <= 3'b0;
        end
        else if (en) begin
            if (cnt==4) begin
                cnt <= 'b0 ;
            end
            else begin
                cnt <= cnt + 1'b1 ;
            end
        end
    end

    //data, valid
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            valid_r        <= 1'b0 ;
            dout_r         <= 'b0 ;
        end
        else if (en) begin
            if (cnt==4) begin
                valid_r     <= 1'b1 ;
                dout_r      <= din;
            end
            else begin
                valid_r     <= 1'b0 ;
            end
        end
    end
    assign dout          = dout_r ;
    assign valid         = valid_r ;

endmodule
```

**梳状器设计**

梳状滤波器就是简单的一阶 FIR 滤波器，每一级的 FIR 滤波器对数据进行一个时钟延时，然后做相减即可。因为系数为 ±1，所以不需要乘法器。

### 实例

```
module comb
    #(parameter NIN  = 21,
      parameter NOUT = 17)
    (
     input               clk,
     input               rstn,
     input               en,
     input [NIN-1:0]     din,
     input               valid,
     output [NOUT-1:0]   dout);

    //en delay
    reg [5:0]                 en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            en_r <= 'b0 ;
        end
        else if (en) begin
            en_r <= {en_r[5:0], en} ;
        end
    end
 
    reg [NOUT-1:0]            d1, d1_d, d2, d2_d, d3, d3_d ;
    //stage 1, as fir filter, shift and add(sub), 
    //no need for multiplier
    always @(posedge clk or negedge rstn) begin
        if (!rstn)        d1     <= 'b0 ;
        else if (en)      d1     <= din ;
    end
    always @(posedge clk or negedge rstn) begin
        if (!rstn)        d1_d   <= 'b0 ;
        else if (en)      d1_d   <= d1 ;
    end
    wire [NOUT-1:0]      s1_out = d1 - d1_d ;

    //stage 2
    always @(posedge clk or negedge rstn) begin
        if (!rstn)        d2     <= 'b0 ;
        else if (en)      d2     <= s1_out ;
    end
    always @(posedge clk or negedge rstn) begin
        if (!rstn)        d2_d   <= 'b0 ;
        else if (en)      d2_d   <= d2 ;
    end
    wire [NOUT-1:0]      s2_out = d2 - d2_d ;

    //stage 3
    always @(posedge clk or negedge rstn) begin
        if (!rstn)        d3     <= 'b0 ;
        else if (en)      d3     <= s2_out ;
    end
    always @(posedge clk or negedge rstn) begin
        if (!rstn)        d3_d   <= 'b0 ;
        else if (en)      d3_d   <= d3 ;
    end
    wire [NOUT-1:0]      s3_out = d3 - d3_d ;

    //tap the output data for better display
    reg [NOUT-1:0]       dout_r ;
    reg                  valid_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            dout_r         <= 'b0 ;
            valid_r        <= 'b0 ;
        end
        else if (en) begin
            dout_r         <= s3_out ;
            valid_r        <= 1'b1 ;
        end
        else begin
            valid_r        <= 1'b0 ;
        end
    end
    assign       dout    = dout_r ;
    assign       valid   = valid_r ;

endmodule
```

**顶层例化**

按信号的流向将积分器、抽取器、梳状器分别例化，即可组成最后的 CIC 滤波器模块。

梳状滤波器的最终输出位宽一般会比输入信号小一些，这里取 17bit。当然输出位宽完全可以与输入数据的位宽一致。

### 实例

```
module cic
    #(parameter NIN  = 12,
      parameter NMAX = 21,
      parameter NOUT = 17)
    (
     input               clk,
     input               rstn,
     input               en,
     input [NIN-1:0]     din,
     input               valid,
     output [NOUT-1:0]   dout);

    wire [NMAX-1:0]      itg_out ;
    wire [NMAX-1:0]      dec_out ;
    wire [1:0]           en_r ;

    integrator   #(.NIN(NIN), .NOUT(NMAX))
    u_integrator (
       .clk         (clk),
       .rstn        (rstn),
       .en          (en),
       .din         (din),
       .valid       (en_r[0]),
       .dout        (itg_out));

    decimation   #(.NDEC(NMAX))
    u_decimator (
       .clk         (clk),
       .rstn        (rstn),
       .en          (en_r[0]),
       .din         (itg_out),
       .dout        (dec_out),
       .valid       (en_r[1]));

    comb         #(.NIN(NMAX), .NOUT(NOUT))
    u_comb (
       .clk         (clk),
       .rstn        (rstn),
       .en          (en_r[1]),
       .din         (dec_out),
       .valid       (valid),
       .dout        (dout));

endmodule
```

**testbench**

testbench 编写如下，主要功能就是不间断连续的输入 250KHz 与 7.5MHz 的正弦波混合信号数据。输入的混合信号数据也可由 matlab 生成，具体过程参考[《并行 FIR 滤波器设计》](https://zhishitu.com/)一节。

### 实例

```
module test ;
    parameter    NIN  = 12 ;
    parameter    NMAX = 21 ;
    parameter    NOUT = NMAX ;

    reg                  clk ;
    reg                  rstn ;
    reg                  en ;
    reg  [NIN-1:0]       din ;
    wire                 valid ;
    wire [NOUT-1:0]      dout ;

    //=====================================
    // 50MHz clk generating
    localparam   T50M_HALF    = 10000;
    initial begin
        clk = 1'b0 ;
        forever begin
            # T50M_HALF clk = ~clk ;
        end
    end

    //============================
    //  reset and finish
    initial begin
        rstn = 1'b0 ;
        # 30 ;
        rstn = 1'b1 ;
        # (T50M_HALF * 2 * 2000) ;
        $finish ;
    end

    //=======================================
    // read cos data into register
    parameter    SIN_DATA_NUM = 200 ;
    reg          [NIN-1:0] stimulus [0: SIN_DATA_NUM-1] ;
    integer      i ;
    initial begin
        $readmemh("../tb/cosx0p25m7p5m12bit.txt", stimulus) ;
        i         = 0 ;
        en        = 0 ;
        din       = 0 ;
        # 200 ;
        forever begin
            @(negedge clk) begin
                en          = 1 ;
                din         = stimulus[i] ;
                if (i == SIN_DATA_NUM-1) begin
                    i = 0 ;
                end
                else begin
                    i = i + 1 ;
                end
            end
        end
    end

    cic #(.NIN(NIN), .NMAX(NMAX), .NOUT(NOUT))
    u_cic (
     .clk         (clk),
     .rstn        (rstn),
     .en          (en),
     .din         (din),
     .valid       (valid),
     .dout        (dout));

endmodule // test
```

**仿真结果**

由下图仿真结果可知，经过 CIC 滤波器后的信号只有一种低频率信号（250KHz），高频信号（7.5MHz）被滤除了。

但是波形不是非常完美，这与设计的截止频率、数据不是持续输出等有一定关系。

此时发现，积分器输出的数据信号也非常的不规则，这与其位宽有关系。

![20210713045925131519321](Verilog基础教程/20210713045925131519321.png)

为了更好的观察积分器输出的数据，将其位宽由 21bit 改为 34bit，仿真结果如下。

此时发现，CIC 滤波器的数据输出并没有实质性的变化，但是积分器输出的数据信号呈现锯齿状，也称之为梳状。这也是梳状滤波器名字的由来。

![20210713045926101975722](Verilog基础教程/20210713045926101975722.png)

### 源码下载

## 7.5 Verilog FFT 设计

- 

  分类：[计算机](https://xiaotua.com/jisuanji.html)

   最后更新: 2021年7月13日

FFT（Fast Fourier Transform），快速傅立叶变换，是一种 DFT（离散傅里叶变换）的高效算法。在以时频变换分析为基础的数字处理方法中，有着不可替代的作用。

### FFT 原理

**公式推导**

DFT 的运算公式为：

![2021071304592718049960](Verilog基础教程/2021071304592718049960.png)

其中，

![2021071304592718650801](Verilog基础教程/2021071304592718650801.png)

将离散傅里叶变换公式拆分成奇偶项，则前 N/2 个点可以表示为：

![2021071304592719270792](Verilog基础教程/2021071304592719270792.png)

同理，后 N/2 个点可以表示为：

![2021071304592811213163](Verilog基础教程/2021071304592811213163.png)

由此可知，后 N/2 个点的值完全可以通过计算前 N/2 个点时的中间过程值确定。对 A[k] 与 B[k] 继续进行奇偶分解，直至变成 2 点的 DFT，这样就可以避免很多的重复计算，实现了快速离散傅里叶变换（FFT）的过程。

**算法结构**

8 点 FFT 计算的结构示意图如下。

由图可知，只需要简单的计算几次乘法和加法，便可完成离散傅里叶变换过程，而不是对每个数据进行繁琐的相乘和累加。

![2021071304592911359294](Verilog基础教程/2021071304592911359294.jpg)

**重要特性**

(1) 级的概念

每分割一次，称为一级运算。

设 FFT 运算点数为 N，共有 M 级运算，则它们满足：

![2021071304592912010835](Verilog基础教程/2021071304592912010835.png)

每一级运算的标识为 m = 0, 1, 2, ..., M-1。

为了便于分割计算，FFT 点数 N 的取值经常为 2 的整数次幂。

(2) 蝶形单元

FFT 计算结构由若干个蝶形运算单元组成，每个运算单元示意图如下：

![2021071304592912688216](Verilog基础教程/2021071304592912688216.gif)

蝶形单元的输入输出满足：

![2021071304592913480297](Verilog基础教程/2021071304592913480297.jpg)

其中， ![2021071304592914080198](Verilog基础教程/2021071304592914080198.png)

每一个蝶形单元运算时，进行了一次乘法和两次加法。

每一级中，均有 N/2 个蝶形单元。

故完成一次 FFT 所需要的乘法次数和加法次数分别为：

![2021071304592914714049](Verilog基础教程/2021071304592914714049.png)

(3) 组的概念

每一级 N/2 个蝶形单元可分为若干组，每一组有着相同的结构与![20210713045929153456510](Verilog基础教程/20210713045929153456510.png)因子分布。

例如 m=0 时，可以分为 N/2=4 组。

m=1 时，可以分为 N/4=2 组。

m=M-1 时，此时只能分为 1 组。

(4) ![20210713045929153456510](Verilog基础教程/20210713045929153456510.png)因子分布![20210713045929166275712](Verilog基础教程/20210713045929166275712.png)因子存在于 m 级，其中 ![20210713045929172942513](Verilog基础教程/20210713045929172942513.png)。

在 8 点 FFT 第二级运算中，即 m=1 ，蝶形运算因子可以化简为：

![20210713045929178842614](Verilog基础教程/20210713045929178842614.png)

(5) 码位倒置

对于 N=8 点的 FFT 计算，X(0) ~ X(7) 位置对应的 2 进制码为：

```
X(000), X(001), X(010), X(011), X(100), X(101), X(110), X(111)
```

将其位置的 2 进制码进行翻转：

```
X(000), X(100), X(010), X(110), X(001), X(101), X(011), X(111)
```

此时位置对应的 10 进制为：

```
X(0), X(4), X(2), X(6), X(1), X(5), X(3), X(7)
```

恰好对应 FFT 第一级输入数据的顺序。

该特性有利于 FFT 的编程实现。

### FFT 设计

**设计说明**

为了利用仿真简单的说明 FFT 的变换过程，数据点数取较小的值 8。

如果数据是串行输入，需要先进行缓存，所以设计时数据输入方式为并行。

数据输入分为实部和虚部共 2 部分，所以计算结果也分为实部和虚部。

设计采用流水结构，暂不考虑资源消耗的问题。

为了使设计结构更加简单，这里做一步妥协，乘法计算直接使用乘号。如果 FFT 设计应用于实际，一定要将乘法结构换成可以流水的乘法器，或使用官方提供的效率较高的乘法器 IP。

**蝶形单元设计**

蝶形单元为定点运算，需要对旋转因子进行定点量化。

借助 matlab 将旋转因子扩大 8192 倍（左移 13 位），可参考附录。

为了防止蝶形运算中的乘法和加法导致位宽逐级增大，每一级运算完成后，要对输出数据进行固定位宽的截位，也可去掉旋转因子倍数增大而带来的影响。代码如下：

### 实例

```
`timescale 1ns/100ps
/**************** butter unit *************************
Xm(p) ------------------------> Xm+1(p)
           -        ->
             -    -
                -
              -   -
            -        ->
Xm(q) ------------------------> Xm+1(q)
      Wn          -1
*//////////////////////////////////////////////////////
module butterfly
    (
     input                       clk,
     input                       rstn,
     input                       en,
     input signed [23:0]         xp_real, // Xm(p)
     input signed [23:0]         xp_imag,
     input signed [23:0]         xq_real, // Xm(q)
     input signed [23:0]         xq_imag,
     input signed [15:0]         factor_real, // Wnr
     input signed [15:0]         factor_imag,

     output                      valid,
     output signed [23:0]        yp_real, //Xm+1(p)
     output signed [23:0]        yp_imag,
     output signed [23:0]        yq_real, //Xm+1(q)
     output signed [23:0]        yq_imag);

    reg [4:0]                    en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            en_r   <= 'b0 ;
        end
        else begin
            en_r   <= {en_r[3:0], en} ;
        end
    end

    //=====================================================//
    //(1.0) Xm(q) mutiply and Xm(p) delay
    reg signed [39:0] xq_wnr_real0;
    reg signed [39:0] xq_wnr_real1;
    reg signed [39:0] xq_wnr_imag0;
    reg signed [39:0] xq_wnr_imag1;
    reg signed [39:0] xp_real_d;
    reg signed [39:0] xp_imag_d;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            xp_real_d    <= 'b0;
            xp_imag_d    <= 'b0;
            xq_wnr_real0 <= 'b0;
            xq_wnr_real1 <= 'b0;
            xq_wnr_imag0 <= 'b0;
            xq_wnr_imag1 <= 'b0;
        end
        else if (en) begin
            xq_wnr_real0 <= xq_real * factor_real;
            xq_wnr_real1 <= xq_imag * factor_imag;
            xq_wnr_imag0 <= xq_real * factor_imag;
            xq_wnr_imag1 <= xq_imag * factor_real;
            //expanding 8192 times as Wnr
            xp_real_d    <= {{4{xp_real[23]}}, xp_real[22:0], 13'b0}; 
            xp_imag_d    <= {{4{xp_imag[23]}}, xp_imag[22:0], 13'b0};
        end
    end

    //(1.1) get Xm(q) mutiplied-results and Xm(p) delay again
    reg signed [39:0] xp_real_d1;
    reg signed [39:0] xp_imag_d1;
    reg signed [39:0] xq_wnr_real;
    reg signed [39:0] xq_wnr_imag;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            xp_real_d1     <= 'b0;
            xp_imag_d1     <= 'b0;
            xq_wnr_real    <= 'b0 ;
            xq_wnr_imag    <= 'b0 ;
        end
        else if (en_r[0]) begin
            xp_real_d1     <= xp_real_d;
            xp_imag_d1     <= xp_imag_d;
            //提前设置好位宽余量，防止数据溢出
            xq_wnr_real    <= xq_wnr_real0 - xq_wnr_real1 ; 
            xq_wnr_imag    <= xq_wnr_imag0 + xq_wnr_imag1 ;
      end
    end

   //======================================================//
   //(2.0) butter results
    reg signed [39:0] yp_real_r;
    reg signed [39:0] yp_imag_r;
    reg signed [39:0] yq_real_r;
    reg signed [39:0] yq_imag_r;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            yp_real_r      <= 'b0;
            yp_imag_r      <= 'b0;
            yq_real_r      <= 'b0;
            yq_imag_r      <= 'b0;
        end
        else if (en_r[1]) begin
            yp_real_r      <= xp_real_d1 + xq_wnr_real;
            yp_imag_r      <= xp_imag_d1 + xq_wnr_imag;
            yq_real_r      <= xp_real_d1 - xq_wnr_real;
            yq_imag_r      <= xp_imag_d1 - xq_wnr_imag;
        end
    end

    //(3) discard the low 13bits because of Wnr
    assign yp_real = {yp_real_r[39], yp_real_r[13+23:13]};
    assign yp_imag = {yp_imag_r[39], yp_imag_r[13+23:13]};
    assign yq_real = {yq_real_r[39], yq_real_r[13+23:13]};
    assign yq_imag = {yq_imag_r[39], yq_imag_r[13+23:13]};
    assign valid   = en_r[2];

endmodule
```

**顶层例化**

根据 FFT 算法结构示意图，将蝶形单元例化，完成最后的 FFT 功能。

可根据每一级蝶形单元的输入输出对应关系，依次手动例化 12 次，也可利用 generate 进行例化，此时就需要非常熟悉 FFT 中"组"和"级"的特点：

- (1) 8 点 FFT 设计，需要 3 级运算，每一级有 4 个蝶形单元，每一级的组数目分别是 4、2、1。
- (2) 每一级的组内一个蝶形单元中两个输入端口的距离恒为 ![20210713045929185357815](Verilog基础教程/20210713045929185357815.png)（m 为级标号，对应左移运算 1<<m），组内两个蝶形单元的第一个输入端口间的距离为 1。
- (3) 每一级相邻组间的第一个蝶形单元的第一个输入端口的距离为 ![20210713045929191717516](Verilog基础教程/20210713045929191717516.png)（对应左移运算 2<<m）。

例化代码如下。

其中，矩阵信号 xm_real（xm_imag）的一维、二维地址是代表级和组的标识。

在判断信号端口之间的连接关系时，使用了看似复杂的判断逻辑，而且还带有乘号，其实最终生成的电路和手动编写代码例化 12 个蝶形单元的方式是完全相同的。因为 generate 中的变量只是辅助生成实际的电路，相关值的计算判断都已经在编译时完成。这些变量更不会生成实际的电路，只是为更快速的模块例化提供了一种方法。

### 实例

```
timescale 1ns/100ps
module fft8 (
    input                    clk,
    input                    rstn,
    input                    en,

    input signed [23:0]      x0_real,
    input signed [23:0]      x0_imag,
    input signed [23:0]      x1_real,
    input signed [23:0]      x1_imag,
    input signed [23:0]      x2_real,
    input signed [23:0]      x2_imag,
    input signed [23:0]      x3_real,
    input signed [23:0]      x3_imag,
    input signed [23:0]      x4_real,
    input signed [23:0]      x4_imag,
    input signed [23:0]      x5_real,
    input signed [23:0]      x5_imag,
    input signed [23:0]      x6_real,
    input signed [23:0]      x6_imag,
    input signed [23:0]      x7_real,
    input signed [23:0]      x7_imag,

    output                   valid,
    output signed [23:0]     y0_real,
    output signed [23:0]     y0_imag,
    output signed [23:0]     y1_real,
    output signed [23:0]     y1_imag,
    output signed [23:0]     y2_real,
    output signed [23:0]     y2_imag,
    output signed [23:0]     y3_real,
    output signed [23:0]     y3_imag,
    output signed [23:0]     y4_real,
    output signed [23:0]     y4_imag,
    output signed [23:0]     y5_real,
    output signed [23:0]     y5_imag,
    output signed [23:0]     y6_real,
    output signed [23:0]     y6_imag,
    output signed [23:0]     y7_real,
    output signed [23:0]     y7_imag
    );

    //operating data
    wire signed [23:0]             xm_real [3:0] [7:0];
    wire signed [23:0]             xm_imag [3:0] [7:0];
    wire                           en_connect [15:0] ;
    assign                         en_connect[0] = en;
    assign                         en_connect[1] = en;
    assign                         en_connect[2] = en;
    assign                         en_connect[3] = en;

    //factor, multiplied by 0x2000
    wire signed [15:0]             factor_real [3:0] ;
    wire signed [15:0]             factor_imag [3:0];
    assign factor_real[0]        = 16'h2000; //1
    assign factor_imag[0]        = 16'h0000; //0
    assign factor_real[1]        = 16'h16a0; //sqrt(2)/2
    assign factor_imag[1]        = 16'he95f; //-sqrt(2)/2
    assign factor_real[2]        = 16'h0000; //0
    assign factor_imag[2]        = 16'he000; //-1
    assign factor_real[3]        = 16'he95f; //-sqrt(2)/2
    assign factor_imag[3]        = 16'he95f; //-sqrt(2)/2

    //输入初始化，和码位有关倒置
    assign xm_real[0][0] = x0_real;
    assign xm_real[0][1] = x4_real;
    assign xm_real[0][2] = x2_real;
    assign xm_real[0][3] = x6_real;
    assign xm_real[0][4] = x1_real;
    assign xm_real[0][5] = x5_real;
    assign xm_real[0][6] = x3_real;
    assign xm_real[0][7] = x7_real;
    assign xm_imag[0][0] = x0_imag;
    assign xm_imag[0][1] = x4_imag;
    assign xm_imag[0][2] = x2_imag;
    assign xm_imag[0][3] = x6_imag;
    assign xm_imag[0][4] = x1_imag;
    assign xm_imag[0][5] = x5_imag;
    assign xm_imag[0][6] = x3_imag;
    assign xm_imag[0][7] = x7_imag;

    //butter instantiaiton
    //integer              index[11:0] ;
    genvar               m, k;
    generate
    //3 stage
    for(m=0; m<=2; m=m+1) begin: stage
        for (k=0; k<=3; k=k+1) begin: unit

            butterfly           u_butter(
               .clk        (clk                 ) ,
               .rstn       (rstn                ) ,
               .en         (en_connect[m*4 + k] ) ,
                       //是否再组内？组编号+组内编号：下组编号+新组内编号
               .xp_real    (xm_real[ m ] [k[m:0] < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))] ),
               .xp_imag    (xm_imag[ m ] [k[m:0] < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))] ),
               .xq_real    (xm_real[ m ] [(k[m:0] < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))) + (1<<m) ]),                 //增加蝶形单元两个输入端口间距离
               .xq_imag    (xm_imag[ m ] [(k[m:0] < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))) + (1<<m) ]),

               .factor_real(factor_real[k[m:0]<(1<<m)? 
                            k[m:0] : k[m:0]-(1<<m) ]),
               .factor_imag(factor_imag[k[m:0]<(1<<m)? 
                            k[m:0] : k[m:0]-(1<<m) ]),

               //output data
               .valid      (en_connect[ (m+1)*4 + k ]  ),
               .yp_real    (xm_real[ m+1 ][k[m:0] < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))] ),
               .yp_imag    (xm_imag[ m+1 ][(k[m:0]) < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))] ),
               .yq_real    (xm_real[ m+1 ][(k[m:0] < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))) + (1<<m) ]),
               .yq_imag    (xm_imag[ m+1 ][((k[m:0]) < (1<<m) ?
                           (k[3:m] << (m+1)) + k[m:0] :
                           (k[3:m] << (m+1)) + (k[m:0]-(1<<m))) + (1<<m) ])
               );
            end
        end
    endgenerate

    assign     valid = en_connect[12];
    assign     y0_real = xm_real[3][0] ;
    assign     y0_imag = xm_imag[3][0] ;
    assign     y1_real = xm_real[3][1] ;
    assign     y1_imag = xm_imag[3][1] ;
    assign     y2_real = xm_real[3][2] ;
    assign     y2_imag = xm_imag[3][2] ;
    assign     y3_real = xm_real[3][3] ;
    assign     y3_imag = xm_imag[3][3] ;
    assign     y4_real = xm_real[3][4] ;
    assign     y4_imag = xm_imag[3][4] ;
    assign     y5_real = xm_real[3][5] ;
    assign     y5_imag = xm_imag[3][5] ;
    assign     y6_real = xm_real[3][6] ;
    assign     y6_imag = xm_imag[3][6] ;
    assign     y7_real = xm_real[3][7] ;
    assign     y7_imag = xm_imag[3][7] ;

endmodule
```

**testbench**

testbench 编写如下，主要用于 16 路实、复数据的连续输入。因为每次 FFT 只有 8 点数据，所以送入的数据比较随意，并不是正弦波等规则的数据。

### 实例

```
`timescale 1ns/100ps
module test ;
    reg          clk;
    reg          rstn;
    reg          en ;

    reg signed   [23:0]   x0_real;
    reg signed   [23:0]   x0_imag;
    reg signed   [23:0]   x1_real;
    reg signed   [23:0]   x1_imag;
    reg signed   [23:0]   x2_real;
    reg signed   [23:0]   x2_imag;
    reg signed   [23:0]   x3_real;
    reg signed   [23:0]   x3_imag;
    reg signed   [23:0]   x4_real;
    reg signed   [23:0]   x4_imag;
    reg signed   [23:0]   x5_real;
    reg signed   [23:0]   x5_imag;
    reg signed   [23:0]   x6_real;
    reg signed   [23:0]   x6_imag;
    reg signed   [23:0]   x7_real;
    reg signed   [23:0]   x7_imag;

    wire                  valid;
    wire signed  [23:0]   y0_real;
    wire signed  [23:0]   y0_imag;
    wire signed  [23:0]   y1_real;
    wire signed  [23:0]   y1_imag;
    wire signed  [23:0]   y2_real;
    wire signed  [23:0]   y2_imag;
    wire signed  [23:0]   y3_real;
    wire signed  [23:0]   y3_imag;
    wire signed  [23:0]   y4_real;
    wire signed  [23:0]   y4_imag;
    wire signed  [23:0]   y5_real;
    wire signed  [23:0]   y5_imag;
    wire signed  [23:0]   y6_real;
    wire signed  [23:0]   y6_imag;
    wire signed  [23:0]   y7_real;
    wire signed  [23:0]   y7_imag;

    initial begin
        clk = 0; //50MHz
        rstn = 0 ;
        #10 rstn = 1;
        forever begin
            #10 clk = ~clk; //50MHz
        end
    end

    fft8 u_fft (
      .clk        (clk    ),
      .rstn       (rstn    ),
      .en         (en     ),
      .x0_real    (x0_real),
      .x0_imag    (x0_imag),
      .x1_real    (x1_real),
      .x1_imag    (x1_imag),
      .x2_real    (x2_real),
      .x2_imag    (x2_imag),
      .x3_real    (x3_real),
      .x3_imag    (x3_imag),
      .x4_real    (x4_real),
      .x4_imag    (x4_imag),
      .x5_real    (x5_real),
      .x5_imag    (x5_imag),
      .x6_real    (x6_real),
      .x6_imag    (x6_imag),
      .x7_real    (x7_real),
      .x7_imag    (x7_imag),

      .valid      (valid),
      .y0_real    (y0_real),
      .y0_imag    (y0_imag),
      .y1_real    (y1_real),
      .y1_imag    (y1_imag),
      .y2_real    (y2_real),
      .y2_imag    (y2_imag),
      .y3_real    (y3_real),
      .y3_imag    (y3_imag),
      .y4_real    (y4_real),
      .y4_imag    (y4_imag),
      .y5_real    (y5_real),
      .y5_imag    (y5_imag),
      .y6_real    (y6_real),
      .y6_imag    (y6_imag),
      .y7_real    (y7_real),
      .y7_imag    (y7_imag));

    //data input
    initial      begin
        en = 0 ;
        x0_real = 24'd10;
        x1_real = 24'd20;
        x2_real = 24'd30;
        x3_real = 24'd40;
        x4_real = 24'd10;
        x5_real = 24'd20;
        x6_real = 24'd30;
        x7_real = 24'd40;

        x0_imag = 24'd0;
        x1_imag = 24'd0;
        x2_imag = 24'd0;
        x3_imag = 24'd0;
        x4_imag = 24'd0;
        x5_imag = 24'd0;
        x6_imag = 24'd0;
        x7_imag = 24'd0;
        @(negedge clk) ;
        en = 1 ;
        forever begin
            @(negedge clk) ;
            x0_real = (x0_real > 22'h3F_ffff) ? 'b0 : x0_real + 1 ;
            x1_real = (x1_real > 22'h3F_ffff) ? 'b0 : x1_real + 1 ;
            x2_real = (x2_real > 22'h3F_ffff) ? 'b0 : x2_real + 31 ;
            x3_real = (x3_real > 22'h3F_ffff) ? 'b0 : x3_real + 1 ;
            x4_real = (x4_real > 22'h3F_ffff) ? 'b0 : x4_real + 23 ;
            x5_real = (x5_real > 22'h3F_ffff) ? 'b0 : x5_real + 1 ;
            x6_real = (x6_real > 22'h3F_ffff) ? 'b0 : x6_real + 6 ;
            x7_real = (x7_real > 22'h3F_ffff) ? 'b0 : x7_real + 1 ;

            x0_imag = (x0_imag > 22'h3F_ffff) ? 'b0 : x0_imag + 2 ;
            x1_imag = (x1_imag > 22'h3F_ffff) ? 'b0 : x1_imag + 5 ;
            x2_imag = (x2_imag > 22'h3F_ffff) ? 'b0 : x2_imag + 3 ;
            x3_imag = (x3_imag > 22'h3F_ffff) ? 'b0 : x3_imag + 6 ;
            x4_imag = (x4_imag > 22'h3F_ffff) ? 'b0 : x4_imag + 4 ;
            x5_imag = (x5_imag > 22'h3F_ffff) ? 'b0 : x5_imag + 8 ;
            x6_imag = (x6_imag > 22'h3F_ffff) ? 'b0 : x6_imag + 11 ;
            x7_imag = (x7_imag > 22'h3F_ffff) ? 'b0 : x7_imag + 7 ;
        end
    end

   //finish simulation
   initial #1000       $finish ;
endmodule
```

**仿真结果**

大致可以看出，FFT 结果可以流水输出。

![20210713045930102080217](Verilog基础教程/20210713045930102080217.png)

用 matlab 自带的 FFT 函数对相同数据进行运算，前 2 组数据 FFT 结果如下。

可以看出，第一次输入的数据信号只有实部有效时，FFT 结果是完全一样的。

但是第二次输入的数据复部也有信号，此时两者之间的结果开始有误差，有时误差还很大。

![20210713045930109553018](Verilog基础教程/20210713045930109553018.png)

用 matlab 对 Verilog 实现的 FFT 过程进行模拟，发现此过程的 FFT 结果和 Verilog 实现的 FFT 结果基本一致。

将有误差的两种 FFT 结果取绝对值进行比较，图示如下。

可以看出，FFT 结果的趋势大致相同，但在个别点有肉眼可见的误差。

![20210713045930116162819](Verilog基础教程/20210713045930116162819.png)

**设计总结：**

就如设计蝶形单元时所说，旋转因子量化时，位宽的选择就会引入误差。

而且每个蝶形单元的运算结果都会进行截取，也会引入误差。

matlab 计算 FFT 时不用考虑精度问题，以其最高精度对数据进行 FFT 计算。

以上所述，都会导致最后两种 FFT 计算方式结果的差异。

感兴趣的学者，可以将旋转因子和输入数据位宽再进行一定的增加，FFT 点数也可以增加，然后再进行仿真对比，相对误差应该会减小。

### 附录：matlab 使用

**生成旋转因子**

8 点 FFT 只需要用到 4 个旋转因子。旋转因子扩大倍数为 8192。

### 实例

```
clear all;close all;clc;
%=======================================================
% Wnr calcuting
%=======================================================
for r = 0:3 
    Wnr_factor  = cos(pi/4*r) - j*sin(pi/4*r) ;
    Wnr_integer = floor(Wnr_factor * 2^13) ;
    
    if (real(Wnr_integer)<0) 
        Wnr_real    = real(Wnr_integer) + 2^16 ; %负数的补码
    else
        Wnr_real    = real(Wnr_integer) ;
    end
    if (imag(Wnr_integer)<0) 
        Wnr_imag    = imag(Wnr_integer) + 2^16 ; 
    else
        Wnr_imag    = imag(Wnr_integer);
    end
    
    Wnr(2*r+1,:)  =  dec2hex(Wnr_real)   %实部
    Wnr(2*r+2,:)  =  dec2hex(Wnr_imag)   %虚部
end
```

**FFT 结果对比**

matlab 模拟 Verilog 实现 FFT 的过程如下，也包括 2 种 FFT 结果的对比。

### 实例

```
clear all;close all;clc;
%=======================================================
% 8dots fft
%=======================================================
for r=0:3
    Wnr(r+1)  = cos(pi/4*r) - j*sin(pi/4*r) ;
end
x       = [10, 20, 30, 40, 10, 20 ,30 ,40];
step    = [1+2j, 1+5j, 31+3j, 1+6j, 23+4j, 1+8j, 6+11j, 1+7j];
x2      = x + step;
xm0     = [x2(0+1), x2(4+1), x2(2+1), x2(6+1), x2(1+1), x2(5+1),         x2(3+1), x2(7+1)] ;

%% stage1 
xm1(1) = xm0(1) + xm0(2)*Wnr(1) ;
xm1(2) = xm0(1) - xm0(2)*Wnr(1) ;
xm1(3) = xm0(3) + xm0(4)*Wnr(1) ;
xm1(4) = xm0(3) - xm0(4)*Wnr(1) ;
xm1(5) = xm0(5) + xm0(6)*Wnr(1) ;
xm1(6) = xm0(5) - xm0(6)*Wnr(1) ;
xm1(7) = xm0(7) + xm0(8)*Wnr(1) ;
xm1(8) = xm0(7) - xm0(8)*Wnr(1) ;
floor(xm1(:))

%% stage2
xm2(1) = xm1(1) + xm1(3)*Wnr(1) ;
xm2(3) = xm1(1) - xm1(3)*Wnr(1) ;
xm2(2) = xm1(2) + xm1(4)*Wnr(2) ;
xm2(4) = xm1(2) - xm1(4)*Wnr(2) ;
xm2(5) = xm1(5) + xm1(7)*Wnr(1) ;
xm2(7) = xm1(5) - xm1(7)*Wnr(1) ;
xm2(6) = xm1(6) + xm1(8)*Wnr(2) ;
xm2(8) = xm1(6) - xm1(8)*Wnr(2) ;
floor(xm2(:))

%% stage3
xm3(1) = xm2(1) + xm2(5)*Wnr(1) ;
xm3(5) = xm2(1) - xm2(5)*Wnr(1) ;
xm3(2) = xm2(2) + xm2(6)*Wnr(2) ;
xm3(6) = xm2(2) - xm2(6)*Wnr(2) ;
xm3(3) = xm2(3) + xm2(7)*Wnr(3) ;
xm3(7) = xm2(3) - xm2(7)*Wnr(3) ;
xm3(4) = xm2(4) + xm2(8)*Wnr(4) ;
xm3(8) = xm2(4) - xm2(8)*Wnr(4) ;
floor(xm3(:))

%% fft
fft1 = fft(x)
fft2 = fft(x2)
plot(1:8, abs(fft2))
hold on
plot(1:8, abs(xm3), 'r')
```

### 源码下载

## 7.6 Verilog DDS 设计

- 

  分类：[计算机](https://xiaotua.com/jisuanji.html)

   最后更新: 2021年7月13日

### DDS 原理

DDS（直接频率合成）技术是根据奈奎斯特抽样定理及数字处理技术，把一系列的模拟信号进行不失真的抽样，将得到的数字信号存储在存储器中，并在时钟的控制下，通过数模转换，将数字量变成模拟信号的方法。

DDS 模块主要由相位累加器、查找表、DAC 转换器和低通滤波器组成，基本结构如下。

![2021071304593116045630](Verilog基础教程/2021071304593116045630.png)

相位累加器，是 DDS 的核心组成部分，用于实现相位的累加，并输出相应的幅值。相位累加器由 ***M\*** 位宽加法器和 ***M\*** 位宽寄存器组成，通过时钟控制，将上一次累加结果反馈到加法器输入端实现累加功能，从而使每个时钟周期内的相位递增数为 ***K\***，并取相位累加结果作为地址输出给 ROM 查找表部分。

幅值查找表，存储着每个相位对应的二进制数字幅度。在每个时钟周期内，查找表对相位累加器输出的相位地址信息进行寻址，然后输出对应的二进制幅度数字离散值。假设查找表地址为 ***M\*** 位，输出数据为 ***N\*** 位，则查找表的容量大小为 ![2021071304593116653011](Verilog基础教程/2021071304593116653011.png)。不难看出，输出信号的相位分辨率为：

![2021071304593117252312](Verilog基础教程/2021071304593117252312.png)

DAC 转换器，将数字信号转换为模拟信号。实际上，DAC 输出的信号并不是连续的，而是根据每位代码的权重，将每一位输入的数字量进行求和，然后以其分辨率为单位进行模拟的输出。实际输出的信号是阶梯状的模拟线型信号，所以要对其进行平滑处理，一般使用滤波器滤波。

低通滤波器，由于 DAC 转换器输出的模拟信号存在阶梯状的缺陷，所以要对其进行平滑处理，滤除掉大部分的杂散信号，使输出信号变为比较理想的模拟信号。

DDS 工作时，频率控制字 ***K\*** 与 ***M\*** 比特位的相位累加器相加，得到的结果作为相位值。在每一个时钟周期内以二进制数的形式送给 ROM 查找表，将相位信息转化为数字化的正弦幅度值，再经过数模转换转化为阶梯形状的模拟信号。待信号经过系统滤波滤除大部分的杂散信号后，就可以得到一个比较纯正的正弦波。

从频率分解的角度讲，ROM 查找表将输入频率![2021071304593117866083](Verilog基础教程/2021071304593117866083.png)分解成了![2021071304593118566574](Verilog基础教程/2021071304593118566574.png)份，输出频率![2021071304593119183855](Verilog基础教程/2021071304593119183855.png)占用的份数正是步进频率控制字 ***K\***。 所以 DDS 输出频率可以表示为：

![2021071304593119835546](Verilog基础教程/2021071304593119835546.png)

从相位角度讲，在时间![2021071304593210431767](Verilog基础教程/2021071304593210431767.png)内由频率控制字 ***K\*** 控制输出的相位增量为：

![2021071304593211113168](Verilog基础教程/2021071304593211113168.png)

考虑此时输出频率的角速度![2021071304593212336729](Verilog基础教程/2021071304593212336729.png)，时间![20210713045932145913210](Verilog基础教程/20210713045932145913210.png)内输出频率的相位增量还可以表示为：

![20210713045932152282811](Verilog基础教程/20210713045932152282811.png)

由上述两式也可以推导出 DDS 输出频率与输入频率之间的关系。

### DDS 设计

**设计说明**

下面只对 DAC 之前的 DDS 电路进行设计。

设计的 DDS 特性有：

- 1）频率可控；
- 2）起始相位可控；
- 3）幅值可控；
- 4）正弦波、三角波和方波可选择输出；
- 5）资源优化：波形存储文件只采用了四分之一的正弦波数据。

**生成 ROM**

ROM 模块最好使用定制的 ip 核，时序和面积都会有更好的优化。定制的 ROM 还需要指定数据文件，例如 ISE 的 ROM 数据文件后缀为 .coe，Quartus II 的 ROM 数据文件后缀为 .mif。

为了方便仿真，这里用代码编写 ROM 模块，地址宽度为 8bit，数据宽度 10bit。

为了节省空间，只存四分之一的正弦波形，然后根据对称性进行平移，即可得到一个完整周期正弦波数据波形。

为实现 DDS 模式多样化，还加入了三角波、方波的 ROM 程序。

实现代码如下（全都包含在文件 mem.v 中）。

### 实例

```
module mem(
    input           clk,            //reference clock
    input           rstn ,          //resetn, low effective
    input           en ,            //start to generating waves
    input [1:0]     sel ,           //waves selection

    input [7:0]     addr ,
    output          dout_en ,
    output [9:0]    dout);          //data out, 10bit width

    //data out fROM ROMs
    wire [9:0]           q_tri ;
    wire [9:0]           q_square ;
    wire [9:0]           q_cos ;

    //ROM addr
    reg [1:0]            en_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            en_r   <= 2'b0 ;
        end
        else begin
            en_r   <= {en_r[0], en} ;         //delay one cycle for en
        end
    end
    assign dout      = en_r[1] ? (q_tri | q_square | q_cos) : 10'b0 ;
    assign dout_en   = en_r[1] ;

    //ROM instiation
    cos_ROM      u_cos_ROM (
       .clk     (clk),
       .en      (en_r[0] & (sel == 2'b0)),  //sel = 0, cos wave
       .addr    (addr[7:0]),
       .q       (q_cos[9:0]));

    square_ROM   u_square_ROM (
       .clk     (clk),
       .en      (en_r[0] & sel == 2'b01),  //sel = 1, square wave
       .addr    (addr[7:0]),
       .q       (q_square[9:0]));

    tri_ROM      u_tri_ROM (
       .clk     (clk),
       .en      (en_r[0] & sel == 2'b10), //sel = 2, triangle wave
       .addr    (addr[7:0]),
       .q       (q_tri[9:0]));

endmodule

//square waves ROM
module square_ROM (
    input               clk,
    input               en,
    input [7:0]         addr,
    output reg [9:0]     q);
    
    //1 in first half cycle, and 0 in second half cycle
    always @(posedge clk) begin
        if (en) begin
            q <= { 10{(addr < 128)} };     
        end
        else begin
            q <= 'b0 ;
        end
    end
endmodule

 //triangle waves ROM
module tri_ROM (
    input               clk,
    input               en,
    input [7:0]         addr,
    output reg [9:0]     q);
    //rising edge, addr -> 0x0, 0x3f
    always @(posedge clk) begin
        if (en) begin
            if (addr < 128) begin
                q <= {addr[6:0], 3'b0};   //rising edge  
            end
            else begin //falling edge
                q <= 10'h3ff - {addr[6:0], 3'b0} ;
            end
        end
        else begin
            q <= 'b0 ;
        end
    end
endmodule

//Better use mem ip.
//This format is easy for simulation
module cos_ROM (
    input               clk,
    input               en,
    input [7:0]         addr,
    output reg [9:0]     q);

   wire [8:0]           ROM_t [0 : 64] ;
   //as the symmetry of cos function, just store 1/4 data of one cycle
   assign ROM_t[0:64] = {
               511, 510, 510, 509, 508, 507, 505, 503,
               501, 498, 495, 492, 488, 485, 481, 476,
               472, 467, 461, 456, 450, 444, 438, 431,
               424, 417, 410, 402, 395, 386, 378, 370,
               361, 352, 343, 333, 324, 314, 304, 294,
               283, 273, 262, 251, 240, 229, 218, 207,
               195, 183, 172, 160, 148, 136, 124, 111,
               99 , 87 , 74 , 62 , 50 , 37 , 25 , 12 ,
               0 } ;

    always @(posedge clk) begin
        if (en) begin
            if (addr[7:6] == 2'b00 ) begin  //quadrant 1, addr[0, 63]
                q <= ROM_t[addr[5:0]] + 10'd512 ; //上移
            end
            else if (addr[7:6] == 2'b01 ) begin //2nd, addr[64, 127]
                q <= 10'd512 - ROM_t[64-addr[5:0]] ; //两次翻转
            end
            else if (addr[7:6] == 2'b10 ) begin //3rd, addr[128, 192]
                q <= 10'd512 - ROM_t[addr[5:0]]; //翻转右移
            end
            else begin     //4th quadrant, addr [193, 256]
                q <= 10'd512 + ROM_t[64-addr[5:0]]; //翻转上移
            end
        end
        else begin
            q <= 'b0 ;
        end
    end
endmodule
```

**DDS 控制模块**

### 实例

```
module dds(
    input           clk,            //reference clock
    input           rstn ,          //resetn, low effective
    input           wave_en ,       //start to generating waves

    input [1:0]     wave_sel ,      //waves selection
    input [1:0]     wave_amp ,      //waves amplitude control
    input [7:0]     phase_init,     //initial phase
    input [7:0]     f_word ,        //frequency control word

    output [9:0]    dout,           //data out, 10bit width
    output          dout_en);

    //phase acculator
    reg [7:0]            phase_acc_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            phase_acc_r    <= 'b0 ;
        end
        else if (wave_en) begin
            phase_acc_r    <= phase_acc_r + f_word ;
        end
        else begin
            phase_acc_r    <= 'b0 ;
        end
    end

    //ROM addr
    reg [7:0]            mem_addr_r ;
    always @(posedge clk or negedge rstn) begin
        if (!rstn) begin
            mem_addr_r     <= 'b0 ;
        end
        else if (wave_en) begin
            mem_addr_r     <= phase_acc_r + phase_init ;
        end
        else begin
            mem_addr_r     <= 'b0 ;
        end
    end

    //ROM instiation
    wire [9:0]   dout_temp ;
    mem  u_mem_wave(
        .clk     (clk),                 //reference clock
        .rstn    (rstn),                //resetn, low effective
        .en      (wave_en),             //start to generating waves
        .sel     (wave_sel[1:0]),       //waves selection
        .addr    (mem_addr_r[7:0]),
        .dout_en (dout_en),
        .dout    (dout_temp[9:0]));     //data out, 10bit width

    //amplitude
    //0 -> dout/1   //1 -> dout/2   //2 -> dout/4   //3 -> dout/8
    assign       dout = dout_temp >> wave_amp ;
endmodule
```

**testbench**

### 实例

```
`timescale 1ns/1ns

module test ;
    reg          clk ;
    reg          rstn ;
    reg          wave_en ;
    reg [1:0]    wave_sel ;
    reg [1:0]    wave_amp ;
    reg [7:0]    phase_init ;
    reg [7:0]    f_word ;
    wire [9:0]   dout ;
    wire         dout_en ;

    //(1)clk, reset and other constant regs
    initial begin
        clk           = 1'b0 ;
        rstn          = 1'b0 ;
        #100 ;
        rstn          = 1'b1 ;
        #10 ;
        forever begin
            #5 ;      clk = ~clk ;   //system clock, 100MHz
        end
    end

    //(2)signal setup ;
    parameter    clk_freq    = 100000000 ; //100MHz
    integer      freq_dst    = 2000000 ;   //2MHz
    integer      phase_coe   = 2;          //1/4 cycle, that is pi/2

    initial begin
        wave_en           = 1'b0 ;
        //(a)cos wave, pi/2 phase
        wave_amp          = 2'd1 ;
        wave_sel          = 2'd0 ;
        phase_init        = 256/phase_coe ;   //pi/8 initialing-phase
        f_word            = (1<<8) * freq_dst / clk_freq; //get the frequency control word
        #500 ;
        @ (negedge clk) ;
        wave_en           = 1'b1 ;        //start generating waves
        # 2000 ;
        //(b)triangle wave, pi/4 initialing-phase
        wave_en           = 1'b0 ;
        wave_sel          = 2'd2 ;
        phase_init        = 256/4 ;
        wave_amp          = 2'd2 ;
        # 50 ;
        wave_en           = 1'b1 ;
    end

    //(3) module instantiaion
    dds u_dds(
        .clk            (clk),
        .rstn           (rstn),
        .wave_en        (wave_en),
        .wave_sel       (wave_sel[1:0]),
        .wave_amp       (wave_amp[1:0]),
        .phase_init     (phase_init[7:0]),
        .f_word         (f_word[7:0]),
        .dout           (dout[9:0]),
        .dout_en        (dout_en));

    //(4) finish the simulation
    always begin
        #100;
        if ($time >= 100000) $finish ;
    end
endmodule 
```

**仿真结果**

如下图所示，将输出信号调整为模拟显示。

- 1）可见正弦波频率为 2MHz，与频率控制字对应；
- 2）正弦波初始相位为 1/2 周期，三角波初始相位为 1/4 周期，符合设置；
- 3）三角波赋值为正弦波的一半，幅值也可控制；
- 4）输出波形为正弦波和三角波，可以正常切换
- 5）正弦波波形没有异常，只用 1/4 周期的正弦波数据就完成了完整正弦波的输出。

限于篇幅，仿真只测试了部分特性。读者可以修改参数测试下其他特性，例如其他频率，方波的输出等。

[![20210713045932160824912](Verilog基础教程/20210713045932160824912.png)](https://zhishitu.com/)

### 附录：matlab使用

**1/4 周期正弦波数据生成**

使用 matlab 生成 1/4 周期正弦波数据描述如下，并对拼接完整正弦波的过程做了仿真。

### 实例

```
clear all;close all;clc;
%=======================================================
% generating 1/4 cos wave data with txt hex format
%=======================================================

N   = 64 ;                    %共256个数据，取1/4
n   = 0:N ;
w   = n/N *pi/2 ;             %量化到pi/2内
st  = (2^10 /2 -1)*cos(w) ;   %正弦波数据取10bit
st  = floor(st) ;

%% 第一象限拼接
st1  = st+512 ;
figure(5) ;plot(n, st1) ;
hold on ;

%% 第二象限拼接
n2  = 64 + n ;
st2 = 512 - st(64-n+1);
plot(n2, st2);
hold on

%% 第三象限拼接
n3  = 128 + n ;
st3 = 512 - st ;
plot(n3, st3) ;
hold on ;

%% 第四象限拼接
n4 = 192 + n ;
st4 = 512 + st(64-n+1) ;
plot(n4, st4) ;
hold on ;
```

### 源码下载

# 8.1 Verilog 数值转换

本节主要对有符号数的十进制与二进制表示以及一些数值变换进行简单的总结。

定义一个宽度为 DW 的二进制补码格式的数据 dbin ，其表示的有符号十进制数字为 ddec 。

```
reg [DW-1:0]     dbin ;
```

### 1. 十进制有符号数转二进制补码

正数的补码为原码。

假如十进制数 ddec 为负数，则计算其对应的二进制补码的方法主要有 2 种：

**将ddec 最高位符号位改写为 1，剩余数值部分取反加一**

例如，4bit 数字 -6 的数值部分为 4'b0110，取反加一后为 4'b0010，高位改写后为 4'b1010。

```
dbin = {1'b1, ~3'b110 + 3'b1} ;    //4'b1010
```

**将负数 ddec 直接与其代表的最大数值范围数相加（有人称之为模数）**

例如，4bit 数字 -6 与 16（2 的 4 次幂）的和为 10， 即对应 4'b1010。

```
dbin = ddec + (1<<4) ;        //4'b1010
```

### 2. 二级制补码转十进制有符号数

当 dbin 最高位为 0 时，其数值大小即为其表示的十进制正数。

当 dbin 最高位为 1 时，计算其表示的十进制有符号数方法主要有 2 种：

**将 dbin 取反加一，并增加符号位**

例如，4bit 数字 -6 的补码为 4'b1010，取反加一后为 4'b0110，增加符号位后为 -6。

```
ddec = -(~4'b1010 + 1'b1) ;  //-6
```

**将 dbin 代表的无符号数值与其代表的最大数值范围数直接相减**

例如，4bit 数字 -6 的补码为 4'b1010，即无符号数值为 10，10 减 16 便可得到 -6 。

```
ddec = dbin - (1<<4) ;  //-6
```

### 3. 绝对值

求 dbin 的绝对值逻辑如下：

```
dbin_abs = (dbin[DW-1]? ~dbin : dbin) + 1'b1 ;
```

例如，4bit 数字 -6 的补码为 4'b1010，取反加 1 后的值为 4'b0110（6），即为 -6 的绝对值。

但如果 dbin 为正数，加 1 后的值比其真正的绝对值要大 1，此步操作只是为了让正数部分的绝对值数量与负数部分一致。因为一定位宽下，由于 0 值的存在，有符号数表示的负数数量会比正数多 1 个。

### 4. 有符号数转无符号数

将有符号数扩展成为无符号数的逻辑如下：

```
dbin_unsigned = {!dbin[DW-1], dbin[DW-2:0]) ;
```

例如：

```
4'b1010 (-6) -> 4'b0010 (2)，4'b0010 (2) -> 4'b1010 (10)
```

其实转换原则是将数据代表的数值范围移动到 0 以上，有符号数转换成无符号数之后，数据相对间的差并没有改变。

### 5. 扩展符号位

计算时有时会根据需要对有符号数位宽进行扩展。假设位宽增量为 W，扩展逻辑如下：

```
dbin_extend = {{(W){dbin[DW-1]}}, dbin} ;
```

扩展原则就是将信号代表符号位的最高位，填充至扩展的高位数据位中。

例如 4'b1010 (-6) 扩展到 8bit 为 8'b11111010，计算其对应的负数仍然是 -6。







