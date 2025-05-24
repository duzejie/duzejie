---
title: linux搭建异构计算开发环境
date: 2018-08-13 21:25:13
author: Sunglow
top: false
cover: false
toc: false
mathjax: false
summary: 以manjaro为例详解搭建CUDA开发环境
categories: 
   - 人工智能  
   - HPC
tags:
  - 异构计算
  - linux
  - manjaro
---





# Intel与Nvidia双显卡切换

## Manjaro配置Intel与Nvidia双显卡切换

[Bumblebee](https://github.com/Bumblebee-Project/Bumblebee)是一套Linux下双显卡切换的解决方案，通过它可以自由的切换集显与独立显卡，做到续航与性能的平衡。

## 安装

Manjaro 提供了强大的硬件检测模块`mhwd`,可以很方便的安装各种驱动。

- 安装依赖

  > `sudo pacman -S virtualgl lib32-virtualgl lib32-primus primus`

- 安装nvidia闭源驱动与intel驱动混合版bumblebee

  > `sudo mhwd -f -i pci video-hybrid-intel-nvidia-bumblebee`

- 开启自动启动bumblebeed服务

  > `sudo systemctl enable bumblebeed`

- 将用户添加到bumblee组

  > `sudo gpasswd -a $USER bumblebee`

如果一切顺利的话，重启后就可以在你想运行的程序名前面加`optirun`,好使用独立显卡驱动你的应用程序。

- 但很大可能是重启后发现无法进入图形化界面,你可以尝试在Grub菜单启动界面按[E]编辑，找到

  ```
  quite
  ```

  并在后面加入(注意空格):

  > acpi_osi=! acpi_osi=’Windows 2009’
  > 或者
  > acpi_osi=! acpi_osi=Linux acpi_osi=’Windows 2015’ pcie_port_pm=off
  > **(很多硬件厂商的BIOS驱动都对Linux不友好，无法顺利加载ACPI模块，而导致无法驱动独立显卡,acpi_osi=’Windows 2009’的意思是告诉ACPI模块，我是‘Windows 7’，别闹情绪了，赶紧工作吧。)**
  > 接着按[Ctrl]+[x]或[F10]保存更改并启动系统。

- 顺利进入系统后打开终端更改配置文件

  > sudo vim /etc/default/grub

- 给 “GRUB_CMLINE_LINUX_DEFAULT”添加你可以正常启动Linux的‘acpi_osi’参数，如图我用的是’Windows 2009’。
  ![Grub](manjaro_grub.png)

- 更新Grub文件,即可永久解决不能启动图形界面的问题

  > sudo update-grub

## 测试性能

- 安装测试软件

  > `sudo pacman -S mesa-demos`

- 集显性能

  > `glxgears -info`

- 独显性能

  > `optirun glxgears -info`

## 使用

- `optirun   [cmd]`

- NVIDIA面板无信息

  > `optirun -b none nvidia-settings -c :8`

- 不依赖Bumblebee来使用CUDA

  > `sudo tee /proc/acpi/bbswitch <<< ‘ON’`

- 使用完CUDA 停止NVIDIA显卡

  > `sudo rmmod nvidia_uvm nvidia && sudo tee /proc/acpi/bbswitch <<< OFF`