
**1、RTL设计**

在设计之前我们先要确定芯片的工艺，比如是选择TSMC还是SMIC，是7nm，还是5nm，而工艺的选择也是受很多因素的制约（如下图），而芯片工艺的选择，就是对这些因素的权衡。

![[../attachments/ea572f8bcbad1fd75db140f373f936f5_MD5.jpg]]

IC设计的第一步就是制定Spec，这个步骤就像是在设计建筑前，要先画好图纸一样，在确定好所有的功能之后在进行设计，这样才不用再花额外的时间进行后续修改。IC 设计也需要经过类似的步骤，才能确保设计出来的芯片不会有任何差错。

![[../attachments/83e6cbe2b8c9af006f82c3f45f54f091_MD5.jpg]]

而用RTL实现的各种功能模块，来组成一个实现具体功能的IP，SOC芯片最终由SOC integration工程师把各个IP集成到一起。

IP又分为模拟IP和数字IP，大概可以做如下的分类：

![[../attachments/d0ac7ba7ecc04b7160c5f8d4af356f11_MD5.jpg]]

在芯片功能设计完备后，我们还要做可测性设计DFT（Design For Test）。

![[../attachments/b23f02bd300993670e6b82cdd1c0552b_MD5.jpg]]

RTL设计最后要做的就是代码的设计规则检查。

通过lint, Spyglass等工具，针对电路进行设计规则检查，包括代码编写风格，DFT，命名规则和电路综合相关规则等。
