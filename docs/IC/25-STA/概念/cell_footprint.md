# # timing lib

cell_footprint:

|   |
|---|
|footprint一般都在.lib文档中定义，每个cell都有一个footprint名对应；一个footprint代表一组cells。|

|   |
|---|
|例如： and2x1.and2x2.and2x4.他们具有相同的footprint and2，他们的功能相同，驱动能力不同|

footprint是指单个std cell的function/size/pin location,一般主要用于low power design flow里保持routing不变的前提下std cell的替换