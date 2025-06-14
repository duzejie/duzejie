一颗芯片从无到有，从有需求到最终应用，经历的是一个漫长的过程，作为人类科技巅峰之一的芯片，凝聚了人们的智慧，而芯片产业链也是极其复杂的，在此，我大致把它归为四个部分（市场需求--芯片设计--芯片制造--测试封装）。

![[attachments/5b364be15160338fa3c641cc6b212c4d_MD5.jpg]]

## 市场需求
[[21-市场需求/芯片市场分析]]


## 芯片设计
芯片设计又可以分为两部分，芯片前端设计和芯片后端设计
- **芯片前端设计**
	前端设计也就是从输入需求到输出网表的过程：主要分为以下几个步骤：
	- **[[22-RTL设计/RTL设计]]**
	- **[[23-IC验证/IC验证]]**
	- **[[25-STA/静态时序分析]]**
	- **覆盖率**
	- **[[26-逻辑综合/ASIC逻辑综合]]**
	时序分析和验证时出现的错误可能需要反复重做前面几步才能解决，是一个多次迭代优化的过程。


- **芯片后端设计**
	后端设计也就是从输入网表到输出GDSII文件的过程：主要分为以下六个步骤：
	- [[820_IC/50-数字EDA/逻辑综合]]
	- [[27-形式验证/形式验证]]
	- [[28-物理实现/物理实现]]
	- [[29-时钟树/时钟树综合]]-CTS
	- [[30-寄生参数/寄生参数提取]]
	- [[31-物理验证/物理验证]]

最后进行封装和测试，就得到了我们实际看见的芯片。

![[attachments/29d54dcd7759fc4801ca770d0df911c1_MD5.jpg]]

芯片设计的流程是纷繁复杂的，从设计到流片耗时长（一年甚至更久），流片成本高，一旦发现问题还要迭代之前的某些过程。






