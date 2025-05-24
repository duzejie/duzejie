## 1.什么是SPEF
SPEF是Standard Parasitic Extraction Format的缩写，用于描述芯片在PR之后实际电路中的 R L C 的值。由于芯片的 current loops非常窄也比较短，所以一般不考虑芯片的电感，所以通常SPEF中包含的寄生参数为RC值。SPEF被后端StarRC工具抽取并用于之后的STA。
[ 详解SPEF](https://blog.csdn.net/zyn1347806/article/details/111804012)
## 2.SPEF的模型
SPEF支持一下三种net模型
- distribute net model        
- reduced net model
- lumpped capacitance model
例如，对于下图的连接线
![[spef1.png|600]]


三种模型分别抽象为

![|600](docs/IC/14-%E6%95%B0%E6%8D%AE%E5%8F%8A%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F/attachments/spef%E6%96%87%E4%BB%B6/93956dd9880005a8cc59449a32e1c709_MD5.png)

distribute net model每一段net都有自己独立的RC值
![|500](docs/IC/14-%E6%95%B0%E6%8D%AE%E5%8F%8A%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F/attachments/spef%E6%96%87%E4%BB%B6/168d9ecc95f4a14ef376f8c12376fbbb_MD5.png)

reduced net model  Load pin是一个简化的RC值，driven 拼端将RC模型简化为一个pie model
![reduce|500](docs/IC/14-%E6%95%B0%E6%8D%AE%E5%8F%8A%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F/attachments/spef%E6%96%87%E4%BB%B6/7b79379acf42bfa7e6203143bafa9053_MD5.png)


lumpped capacitance model  将所有net的cap简化为一个单一的cap值
![|500](docs/IC/14-%E6%95%B0%E6%8D%AE%E5%8F%8A%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F/attachments/spef%E6%96%87%E4%BB%B6/34890bf0cafec7fd6a04a3f0d909c1f7_MD5.png)


## 3 SPEF文件的内容
SPEF文件的总体格式如下。下面分别介绍这几部分的含义。
```
header_definition
[ name_map ]
[ power_definition ]
[ external_definition ]
[ define_definition ]
internal_definition
```

 

### 3.1 header_definition
一个典型的head definition如下所示，内容 基本上看一下就明白这里不过多介绍
```spef

*SPEF "IEEE 1481-1998"
*DESIGN "ddrphy"
*DATE "Thu Oct 21 00:49:32 2004"
*VENDOR "SGP Design Automation"
*PROGRAM "Galaxy-RCXT"
*VERSION "V2000.06 "
*DESIGN_FLOW "PIN_CAP NONE" "NAME_SCOPE
LOCAL"
*DIVIDER /
*DELIMITER :
*BUS_DELIMITER [ ]
*T_UNIT 1.00000 NS
*C_UNIT 1.00000 FF
*R_UNIT 1.00000 OHM
*L_UNIT 1.00000 HENRY
// A comment starts with the two characters “//”.
// TCAD_GRD_FILE /cad/13lv/galaxy-rcxt/
t013s6ml_fsg.nxtgrd
// TCAD_TIME_STAMP Tue May 14 22:19:36 2002
 ```

### 3.2 name  map
如下所示为name map的示例。name map可以大大减小SPEF的大小
```
*NAME_MAP
*1 memclk
*2 memclk_2x
*3 reset_
*4 refresh
*5 resync
*6 int_d_out[63]
*7 int_d_out[62]
*8 int_d_out[61]
*9 int_d_out[60]
*10 int_d_out[59]
*11 int_d_out[58]
*12 int_d_out[57]
. . .
*364 mcdll_write_data/write19/d_out_2x_reg_19
*366 mcdll_write_data/write20/d_out_2x_reg_20
*368 mcdll_write_data/write21/d_out_2x_reg_21
. . .
*5423 mcdll_read_data/read21/capture_data[53]
. . .
*5426 mcdll_read_data/read21/capture_pos_0[21]
. . .
*11172 Tie_VSSQ_assign_buf_318_N_1
. . .
*14954 test_se_15_S0
*14955 wr_sdly_course_enc[0]_L0
*14956 wr_sdly_course_enc[0]_L0_1
*14957 wr_sdly_course_enc[0]_S0
 ```

### 3.3 power definition
该部分定义了power的相关内容

>*POWER_NETS VDDQ
>*GROUND_NETS VSSQ
 
### 3.4 external definition
定义了设计中的逻辑和物理的关系。例如，对于port的逻辑定义格式如下
```
*PORTS
port_name direction { conn_attribute }
port_name direction { conn_attribute }
. . .
```
其中port name是一个正整数，direction为I/O/B分别代表输入、输出和输入输出。连接属性为可选参数，有如下几种

> *C number number : 表示port
> *L par_value : 表示port的cap
> *S par_value par_value : 定义port waveform的形状（不太懂）
> *D cell_type :定义驱动port的cell的类型

port的物理定义格式如下
```
*PHYSICAL_PORTS
pport_name direction { conn_attribute }
pport_name direction { conn_attribute }
. . .
```

### 3.5 define definition
该部分描述了当前SPEF中例化的instance的reference name，这些instance的SPEF信息由另外的SPEF文件给出。其定义的格式如下。DEFINE 定义的是相关instance的hierarchy内容。例如下面的例子表示关于core/u1ddrphy和core/u2ddrphy的两个instance的SPEF对于design名为 ddrphy

*DEFINE core/u1ddrphy core/u2ddrphy “ddrphy”
*PDEFINE定义instance的物理信息。

*DEFINE instance_name { instance_name } entity_name
*PDEFINE physical_instance entity_name
 

### 3.6  internal definition
该部分定义了design内部net的RC信息。根据SPEF支持的RC网络模型，SPEF 包含两种基本的格式D_NET表示 distributed net模型；R_NET表示 reduced net模型。两种模型的内容类似下面以distributed net模型为例进行讲解。例如，对于下面的例子。
```
*D_NET *5426 0.899466
*CONN
*I *14212:D I *C 21.7150 79.2300
*I *14214:Q O *C 21.4950 76.6000 *D DFFQX1
*CAP
1 *5426:10278 *5290:8775 0.217446
2 *5426:10278 *16:3754 0.0105401
3 *5426:10278 *5266:9481 0.0278254
4 *5426:10278 *5116:9922 0.113918
5 *5426:10278 0.529736
*RES
1 *5426:10278 *14212:D 0.340000
2 *5426:10278 *5426:10142 0.916273
3 *5426:10142 *14214:Q 0.340000
*END
```

其中，5426是net的名字，可以从前面的name map找到对应的net；0.899466表示net的所以的cap值。

CONN表示driver和load的连接关系，其中
```
I 表示internal pin
*14212:D 表示名为14212的instance的D pin
O 表示输出 C表示对应的坐标 D表示driver pin
CAP部分描述了对应net node的cap值，单位见header definition
其中第一行描述了两个net之间的耦合电容
其中第5行省略了第二个net表示是对地电容
```
 

RES部分定义了net之间的电阻，单位见header definition，含义和CAP部分类似。这里不再啰嗦了。

*RES
1 *5426:10278 *14212:D 0.340000
2 *5426:10278 *5426:10142 0.916273
3 *5426:10142 *14214:Q 0.340000

所以对于上面这样一个D_NET的描述信息可以得到如下的RC网络
![[spef2.png|800]]


以上就是SPEF内容的介绍，虽然在实际的工作过程中，我们很少会直接去人工分析SPEF进行相关的计算。但是对于工程人员还是要知其然的。
————————————————
版权声明：本文为CSDN博主「进击的芯片」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zyn1347806/article/details/111804012