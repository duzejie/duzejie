## 时序弧
Timing arc，中文名时序弧。这是timing计算最基本的组成元素，lib库介绍中，大部分时序信息都以Timing arc呈现。*如果两个pin之间在timing上存在因果关系，我们就把这种时序关系称为Timing arc*，主要分为定义时序延迟，和定义时序检查两种。为啥叫它时序弧？因为时序图中经常用一条弧形线段来表示它。如下图所示：cell的timing arc定义在lib中，lib库中没有net之间的timing arc, 它的delay则有RC参数计算而出。
https://blog.csdn.net/u011075954/article/details/113661472




