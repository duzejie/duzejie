---
title: C++研发优秀资源汇总
author: Sunglow
top: false
cover: false
toc: false
mathjax: false
summary: 'null'
categories:
  - 软件工程
  - Cpp
tags:
  - 编程
  - C++
  - 资源
date: 2018-02-21 21:25:13
keywords:
---

# Effective Modern C++

虽然上述代码风格指南包含了很多种语言，就让我们先讨论一下C++吧，Scott Meyers 的这本 《Effective Modern C++》书中给出了42个指导建议，是构建 **正确，高效，可维护和可移植** 的软件的向导。让我们先来看一下本书简介吧：

如果你是一个像我一样有经验的C++程序猿，当初次体验C++11时，“啊，就是他，我明白了，这就是C++”。但是自从你学习了更多的内容，你会惊讶于他的变化。`auto`类型声明，基于区间的`for`循环，lambda表达式和右值引用改变了C++的样貌，还有新的并发API。除此之外，还包括一些合服语言习惯的改动。0和`typedef`都已经过时，`nullptr`和别名声明（alias declarations）强势登场。`enum`需要被作用域限制。现在更加建议使用在内部实现的智能指针。移动对象要比拷贝一个对象代价更小。

更重要的是，要想有效的利用好这些特性需要学习很多的东西。如果你拥有了关于“现代”C++特性，知识储备的基础，但是希望得到一个关于如何正确驾驭这些特性来创造运行正确，高效，可维护和可移植的软件的向导，搜寻的过程是非常具有挑战性的。这就是这本书的目的。他不是来介绍C++11和C++14的新特新的，而是来介绍如何使用他们的高效做法。

好了，我先放上本书的英文版 [Effective.Modern.C++.en.pdf](https://confluence.deepglint.com/download/attachments/4917465/Effective.Modern.C%2B%2B.en.pdf?version=1&modificationDate=1548694750910&api=v2)，你可以直接下载来读读，开源社区维护的 [中文版](https://vivym.gitbooks.io/effective-modern-cpp-zh/) 并不完整，当然你可以先读为快，如果你像我一样喜欢中文的纸质书随处可以找得到 比如在[JD](https://item.jd.com/12348026.html)。

# C++ Core Guidelines

如果你觉得还不过瘾，这里还有 Bjarne Stroustrup 主笔的指南，你不会不知道他是谁吧，他老的个人[主页](http://www.stroustrup.com/)中用一句话介绍自己：“I designed and implemented [the C++ programming language](http://www.stroustrup.com/C++.html).”。这里有很多很多的条款，助你了解C++的方方面面，比如RAII。文档较长，建议先浏览一下，知道里面都提到了哪些主题，在需要用到某些特性，忧郁不决时，可以来查一下这份文档，我总能从中得到启发，相信你也可以。用得多了，所有这些指南都内化了，你就是大神了！

好了，文档在此：[C++ Core Guidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)，暂时没有中文版。

# C++ Resource

- 当你写代码时需要查一下手册：[C++ 参考手册](https://zh.cppreference.com/w/cpp)
- 当你想了解标准C++的一些新闻和讨论：[isocpp](https://isocpp.org/)
- 这里还有个社区 [CppCon](https://cppcon.org/) | The C++ Conference，YouTube上有他们的[频道](https://www.youtube.com/user/CppCon)（如果你去听过现场 请告诉我 我要崇拜一下），推荐看一下 herb sutter 的这个演讲：[Back to the basics Essentials of modern C++ Style](https://sec.ch9.ms/ch9/bb5e/39551fa0-5ae1-4eb5-a640-b11d94d6bb5e/CPPBasicsSutter_mid.mp4)。

嗯，C++ 相关的资源就这些了，如果有其他语言的资源欢迎补充。

# Git

代码的版本管理也是也是一项基本技能。

入门可以看这份：[史上最浅显易懂的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

官方手册在这：[Git-Book](https://git-scm.com/book/zh)

## 分支管理

推荐使用 [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) 这个实践，我们没有用 git flow 的服务，但是你理解了文档中的那些由 branch 组成的图，用基本的 Git 命令一样可以管理好分支。

## 版本管理

如何定义版本号，[语义化版本](https://semver.org/lang/zh-CN/) 给出版本定义的准则，照此执行即可。





## C++ 有用的网站

- [C++ Programming](http://en.wikibooks.org/wiki/C++_Programming) − 这本书涵盖了 C++ 语言编程、软件交互设计、C++ 语言的现实生活应用。
- [C++ FAQ](http://www.sunistudio.com/cppfaq/) − C++ 常见问题
- [Free Country](http://www.thefreecountry.com/sourcecode/cpp.shtml) − Free Country 提供了免费的 C++ 源代码和 C++ 库，这些源代码和库涵盖了压缩、存档、游戏编程、标准模板库和 GUI 编程等 C++ 编程领域。
- [C and C++ Users Group](http://www.hal9k.com/cug/) − C 和 C++ 的用户团体提供了免费的涵盖各种编程领域 C++ 项目的源代码，包括 AI、动画、编译器、数据库、调试、加密、游戏、图形、GUI、语言工具、系统编程等。

## C++ 有用的书籍

- [Essential C++ 中文版](https://s.click.taobao.com/t?e=m%3D2%26s%3DLi0UXGZauMYcQipKwQzePOeEDrYVVa64K7Vc7tFgwiHjf2vlNIV67mvQw%2F0oWRxq5ZnjZiNpIZZ0SY1KVGTulTasDm8dtc1PiCkt1wuc0S8%2Bah%2FIOB5zRCm8dRJ%2FXdaZlrfKbc84rldzLweBEW94KuMnnFiZU89RomfkDJRs%2BhU%3D&pvid=10_120.41.149.90_561_1523089225734)
- [C++ Primer Plus 第6版中文版](https://s.click.taobao.com/t?e=m%3D2%26s%3D%2BDvqYICl6aocQipKwQzePOeEDrYVVa64K7Vc7tFgwiHjf2vlNIV67q6E28vpGaYdLzKPa%2Ff2nu90SY1KVGTulTasDm8dtc1PiCkt1wuc0S8%2Bah%2FIOB5zRCm8dRJ%2FXdaZlrfKbc84rle6JxiC6MKPJOzhr6zettVqxiXvDf8DaRs%3D&pvid=10_120.41.149.90_1177_1523091524310)
- [C++ Primer中文版（第5版）](https://s.click.taobao.com/t?e=m%3D2%26s%3DfYVpIxJTkKEcQipKwQzePOeEDrYVVa64K7Vc7tFgwiHjf2vlNIV67kgvHK4EZ15Ylg6AtVBcXjx0SY1KVGTulTasDm8dtc1PiCkt1wuc0S8%2Bah%2FIOB5zRCm8dRJ%2FXdaZlrfKbc84rlecMDxhX1VqrwoUWrUV%2FQF3omfkDJRs%2BhU%3D&pvid=10_120.41.149.90_2020_1523090203718)