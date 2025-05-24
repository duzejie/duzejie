IC设计常用文件及格式介绍

|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| GDSII：               | 它是用来描述掩模几何图形的事实标准，是二进制格式，内容包括层和几何图形的基本组成。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| CIF：                 | （caltech intermediate format）,叫caltech中介格式，是另一种基本文本的掩模描述语言。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| LEF：                 | library exchange format）,叫库交换格式，它是描述库单元的物理属性，包括端口位置、层定义和通孔定义。它抽象了单元的底层几何细节，提供了足够的信息，以便允许布线器在不对内部单元约束来进行修订的基础上进行单元连接。<br><br>包含了工艺的[技术](http://aax1985.spaces.eepw.com.cn/articles/article/item/30504)信息，如布线的层数、最小的线宽、线与线之间的最小距离以及每个被选用cell，BLOCK，PAD的大小和pin的实际位置。cell，PAD的这些信息由厂家提供的LEF文件给出，自己定制的BLOCK的LEF文件描述经ABSTRACT后生成，只要把这两个LEF文件整合起来就可以了。                                                                                                                                                                                                                                                                                                                                                              |
| DEF：                 | （design exchange format），叫[设计](http://aax1985.spaces.eepw.com.cn/articles/article/item/30504)交换格式，它描述的是实际的设计，对库单元及它们的位置和连接关系进行了列表，使用DEF来在不同的设计系统间传递设计，同时又可以保持设计的内容不变。DEF与只传递几何信息的GDSII不一样。它还给出了器件的物理位置关系和时序限制等信息。<br><br>DEF files are ASCII files that contain information that represent the design at any point during the layout process.DEF files can pass both logical information to and physical information fro place-and-route tools.<br><br> * logical information includes internal connectivery(represented by a netlist),grouping information and physical constraints.<br><br> * physical information includes the floorplan,placement locations and orientations, and routing geometry data. |
| SDF：                 | (Standard delay format),叫标准延时格式，是IEEE标准，它描述设计中的时序信息，指明了模块管脚和管脚之间的延迟、时钟到数据的延迟和内部连接延迟。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| DSPF、RSPF、SBPF和SPEF： | DSPF（detailed standard parasitic format）,叫详细标准寄生格式，属于CADENCE公司的文件格式。<br><br>    RSPF（reduced standard parasitic format）,叫精简标准寄生格式，属于CADENCE公司的文件格式。<br><br>    SBPF（synopsys binary parasitic format）,叫新思科技二进制寄生格式，属于SYNOPSYS公司的文件格式。<br><br>    SPEF（standard parasitic exchange format）,叫标准寄生交换格式，属于IEEE国际标准文件格式。<br><br>    以上四种文件格式都是从网表中提取出来的表示RC值信息，是在提取工具与时序[验证](http://aax1985.spaces.eepw.com.cn/articles/article/item/30504)工具之间传递RC信息的文件格式。                                                                                                                                                                                                                                                       |
| ALF：                 | (Advanved library format),叫先进库格式，是一种用于描述基本库单元的格式。它包含电性能参数。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| PDEF：                | （physical design exchange format）叫物理设计交换格式。它是SYNOPSYS公司用在前端和后端工具之间传递信息的文件格式。描述了与单元层次分组相关的互连信息。这种文件格式只有在使用SYNOPSYS公司的Physical Compiler工具才会用到，而且.13以下工艺基本都会用到该工具。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| TLF                  | TLF文件是描述cell时序的文件，标准单元的rise time，hold time，fall time都在TLF内定义。时序分析时就调用TLF文件，根据cell的输入信号强度和cell的负载来计算cell的各种时序信息。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| GCF                  | GCF文件包括TLF/CTLF文件的路径，以及综合时序、面积等约束条件。在布局布线前，GCF文件将设计者对电路的时序要求提供给SE。这些信息将在时序驱动布局布线以及静态时序分析中被调用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

来自 <[https://blog.csdn.net/augusdi/article/details/44247129](https://blog.csdn.net/augusdi/article/details/44247129)>



### 逻辑库 Liberty Library Format（.lib)
布局布线工具的环境建立从库来看可分为三个：逻辑库、物理库、设计库：

1.逻辑库：用于描述单元的时序和功耗特性的文件格式，Liberty Library Format（.lib),是由Synopsys公司研发。根据工艺的复杂度以及设计要求，现阶段普遍应用三种模型

NLDM：非线性延时模型

CCSM：复合电流模型

ECSM：有效电流模型

CCSM和ECSM不仅包含了时序和功耗属性，还包含了噪声信息，所以与Spice模型的误差可以控制在2%~3%以内，而NLDM则一般与Spice模型的误差在7%左右，在相同工艺条件下描述相同电路结构，采用CCSM的Liberty文件大小一般是NLDM模型Liberty文件的8-10倍。

大多数情况下，半导体厂商在提供（.lib）文件的同时，也会提供二进制格式的（.db）文件。（.lib）文件和（.db）文件是一种文件的两种不同表达方式。ICC使用的逻辑库数据类型必须是（.db）格式的库文件。因此，若只有（.lib）文件，则需要使用Synopsys工具用命令将其转换成（.db）文件，DC、LC、PT、ICC均可

write_lib library_name -format db -output xxx.db


### 物理库 LEF
物理库：描述了工艺层次，工艺规则以及标准单元等IP的形状大小，端口位置信息，目前业内标准是（.lef）格式，但是Synopsys的ICC使用Milkway格式的数据库，其中的fram view就是物理库，其本质和lef一样，ICC2放弃了Milkyway，使用(.db）和（.lef）生成的ndm作为参考库创建ndb。通常情况下，IO单元和标准单元的Milkyway库由IP提供商提供，而宏单元（Macro）的Milkway需要后端设计人员用Synopsys专门的EDA工具Milkyway根据宏单元的GDS文件或lef文件生成。

对于每一个工艺的参考库，都有专门的技术文件（Technology File）来说明其工艺的参数信息，技术文件简称（.tf）文件，产生Milkyway物理库和ndb时需要用到此文件。

技术文件通常由工艺厂提供，文件主要包含了每层掩膜层的层号，连接层信息，EDA工具中显示的颜色与线条，最小宽度，最小面积等信息。ICC就是根基技术文件中的描述的金属层和通孔层的设计规则进行布局布线的。

ICC的mdb创建方式

|   |   |
|---|---|
|Code|set top topname<br><br>set sdc_phase        prects<br><br>#library_setup<br><br>source -e -v        /library/library_tech.tcl<br><br>#Extends Milkyway database layer number support from 255 to 4095 layers<br><br>extend_mw_layers<br><br>#$lib_tech(tech,mdb) is technology file<br><br>cerate_mw_lib -technology $lib_tech(tech,mdb) topname.mdb<br><br>set_mw_lib_reference -mw_reference_library Milkyway topname.mdb<br><br>open_mw_lib topname.mdb<br><br>source -e -v $lib_tech(tech,ant)<br><br>report_antenna_rules<br><br>set_mw_technology_file -alf $tech(tech,em) topname.mdb<br><br>close_mw_lib|

ICC2的ndb创建脚本

|     |                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     | set top topname<br><br>#library setup<br><br>source -e -v /library/library_tech.tcl<br><br>if {[file exist ${top}.ndb]} {sh rm -rf ${top}.ndb}<br><br>#$lib_tech(tech,mdb) is technology file<br><br>#$lib_tech(physical_lib.ndb) is ICC2 ndm file<br><br>create_lib ${top}.ndb -technology $lib_tech(tech,mdb) -ref_libs $lib_tech(physical_lib.ndb)<br><br>source -e -v $lib_tech(tech,ant)<br><br>report_antenna_rules<br><br>save_lib |

### 设计库
设计库：包含网表和约束文件，其中网表一般是Verilog格式，如果使用dc综合，推荐使用ddc格式的网表；约束一般是sdc格式，对整个设计的工作频率，时钟关系，输入输出延迟等进行了相关的约束。

来自 <[https://blog.csdn.net/ShowKiller123/article/details/110188034](https://blog.csdn.net/ShowKiller123/article/details/110188034)>






