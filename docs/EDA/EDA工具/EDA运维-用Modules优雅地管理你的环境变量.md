# 用Modules优雅地管理你的环境变量

原创 李德荣 EDA运维

_2023年08月31日 19:11_ _上海_

在实际开发中，经常会碰到同一台服务器上需要使用不同版本的EDA软件，Modules（module-environment）可以很方便的帮我们完成软件各个版本的切换。

1.module简介

在高性能计算（HPC）平台上为方便使用，集群软件环境通过modules工具管理环境变量。Environment module”(环境模块)是一组环境变量设置的集合。module可以被加载(load)、卸载(unload)、切换(switch)，这些操作会改变相应的环境变量设置，从而让用户方便地在不同环境间切换。


查看已加载模块

```
module list
```

加载模块

```
module load
```

卸载模块

```
module unload
```

切换模块

```
module switch
```

卸载全部模块

```
module purge
```

显示模块内容

```
module show
```