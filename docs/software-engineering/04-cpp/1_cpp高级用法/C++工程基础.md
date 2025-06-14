---
title: C++ 工程基础
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
  - 工程
date: 2018-02-21 21:25:13
keywords:
---

# C++ 工程基础

## 语言版本

C++语言有不同的语法版本，包括 C++98 ， C++11 ， C++14 ， C++17 ， C++20 等，后面的数字表示草案通过的年份。权衡功能和兼容性，并参考 Google 目前（2019年1月）版本的编码规范 [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)， [Google 开源项目风格指南 (中文版)](../../00-standard/Google开源代码风格指南_cpp.md )，
我们**要求使用** C++11 版本的语言，并会在以后恰当的时间随之升级。

「要求使用」的含义是：如果新的语法可以提升代码效率，应当立即使用新语法；如果在保证代码效率不降低的前提下，新语法可以增加代码的可读性、简洁性，也应当立即使用新语法。但不要为了使用新语法，而使用代码的效率、可读性、简洁性降低。每个学习使用现代 C++ 的程序员都应该阅读[《 Effective Modern C++ 》](https://book.douban.com/subject/30178902/)。

## 标准库

C++标准库的版本是随语言版本同步更新的，事实上标准库已成为现代 C++ 语言的一部分，没有任何理由拒绝使用，并应该充分利用标准库，避免重复工作。
（待完成）

## 代码文件

C++工程的源代码通常由源文件和头文件构成，为了统一起见，我们要求源文件的扩展名定为「 *.cpp 」，头文件的扩展名定为「 *.hpp 」。罕见的，如果有内联文件，扩展名定为 「 *.inl 」。源代码的文件名是该代码功能的英语概述，我们采用如下命名规则：

- 为保证跨平台兼容性，只能由小写字母和下划线（-）组成，不得使用其它字符；
-  必须由英语单词构成，不可使用拼音，单词和单词之间由由下划线分隔；
-  为保证可读性，总长度强烈建议不要超过 22 个字符或4个单词。

关于代码规范性问题参考[ Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) ，下文统一使用这套约定。



## 宏声明

- #pragma once 

是一个比较常用的C/C++杂注，只要在头文件的最开始加入这条杂注，就能够保证头文件只被编译一次
是编译器相关的，有的编译器支持，有的编译器不支持，现在大部分编译器都有这个杂注了。

- #ifndef，#define，#endif

是C/C++语言中的宏定义，通过宏定义避免文件多次编译。所以在所有支持C++语言的编译器上都是有效的，如果写的程序要跨平台，最好使用这种方式。



## 二进制代码

我们通常所说的「二进制代码」一般是指机器码，就是一系列直接送给 CPU 执行运算的机器指令。由于不同硬件平台的 CPU 指令集不同，因此使用的编译器也不同。参见 [Intel® 64 and IA-32 Software Developer's Manual](https://www.intel.com/content/www/us/en/architecture-and-technology/64-ia-32-architectures-software-developers-manual.html) ，以及 [ARM developer documents](https://developer.arm.com/docs) 。

在绝大多数操作系统上，用户应用程序都必须通过操作系统来执行 CPU 指令。操作系统会对于一些可能会危及系统安全的指令进行保护，并对（硬件）中断访问相关的指令进行转发，还会提供I/O和内存管理接口，参考 [Protection ring](https://en.wikipedia.org/wiki/Protection_ring)。由于不同操作系统的权限、内存、硬件管理模式都有很大差别，因此不同的操作系统所使用的编译器也不同。

由 A （软/硬件）系统的编译器生成 B （软/硬件）系统的机器码的过程称为[交叉编译 (cross compilation)](https://en.wikipedia.org/wiki/Cross_compiler) 。

## 生成过程

C++ 程序的生成是指将源代码转换为「可执行程序」或「程序库」的过程，分为「预处理」、「编译」、「汇编」和「链接」四个阶段，参见[The C++ compilation process](http://faculty.cs.niu.edu/~mcmahon/CS241/Notes/compile.html)  。在大多数系统的实践中，汇编操作已被整合进「编译」过程，所以本文不再讨论汇编过程。

### 编译

广义上的编译包含了预处理的过程，即编译器直接由输入的源文件生成对应的「目标文件」的过程。目标文件也叫作「中间文件」，通常是二进制格式，里面存有符号表与机器指令。在Linux操作系统中，目标文件通常以「 *.o 」或「 *.obj 」为扩展名。

一个 C++ 程序由一分或多份源文件构成，要生成最后的程序，每个源文件均要执行一次单独的且独占的编译过程，最终再将多个目标文件链接在一起。请注意，**实际的编译器仅接受一个源代码文件的输入**，只不过有些整合编译器（如 Linux 内置的 g++ ）会在内部自动多次调用编译器来应对输入的多个源代码文件，并会根据参数进行链接。 g++ 会根据文件的扩展名来决定所其内部使用的编译器类型，比如 c 或 c++，参见 [g++: Linux man page](https://linux.die.net/man/1/g++) 。

编译器在得到源文件后要先进行预处理，即根据源代码中的预编译指令来生成真正的源代码，然后再对处理好的源代码进行编译。预编译指令包括常见的「 #include 」，「 #define 」，「 #pragma 」等。其中「 #include 」指令会引导编译器在当前目录（当前是指与源文件相同）或系统目录中找到被包含的头文件，然后**以一种类似于复制粘贴的方式在该行处展开**，嵌套包含的头文件也会被递归式的展开。

「#pragma」指令通常是用来配置编译器的参数，或在当前行生成特定代码（用户不可见），由编译器内部支持。不同的编译器或不同的版本所支持的 #pragma 指令都不相同，参见 [Implementation defined behavior control](https://en.cppreference.com/w/cpp/preprocessor/impl) 。

在生成了真正可编译的源代码后，编译器将开始执行一系列的编译算法，包括词法分析、语法分析、语义分析、代码生成、代码优化、符号表生成等，最终生成机器相关的目标文件。对此感兴趣的同学可参见一份短而精的介绍 [How C++ Works: Understanding Compilation](https://www.toptal.com/c-plus-plus/c-plus-plus-understanding-compilation)。另可参考号称「龙书」的[《编译原理》](https://book.douban.com/subject/3296317/) 。

注意，**头文件或内联文件不要直接送入编译器**，而应由源文件的预编译指令输入编译器。

### 链接

要理解链接的过程，必须先理解**符号表**。编译器会为每个目标文件生成一个**导出符号表**，它将目标文件中的各个项映射为链接器可以理解的名称，这些项包括函数、静态变量、字符串常量等等。另有一份**导入符号表**，如果你在代码中某处调用了外部库的一个函数或变量，编译器此时并不会将它在外部库的地址（地址包括文件相对/绝对路径和在文件中的偏移量）填入该处，而是在此处放置一个占位符（根据一定规则从被调用的函数名称和类型转译为符号），并在导入符号表中添加一项，之后链接器会为符号表中的每个符号去查找它们所在的真正地址。简而言知，导出符号表就是这个文件有哪些符号是给别人调用的，地址（偏移量）是什么。导入符号表就是有什么符号是引用别人的，在链接前并没有付给对应的地址。

我们输入给链接器的主要信息包括：一组编译好的目标文件，所需要引用的外部库文件，输出文件，链接选项。然后，链接器会做如下事：

1.  在默认路径和指定路径中查找外部程序库文件。
2. 将输入的所有程序库（包括静态和动态）的导出符号表解析出来，生成一张导出总表。重复的程序库文件会被忽略，如果相同的符号存在于不同的程序库中就会报错：符号重定义。
3.  把输入的每个目标文件的机器指令抽取出来，合并到输出文件的代码段。
4.  扫描当前输出文件中的所有内容，遇到占位符就去导出总表中查找地址：如果符号位于动态库，就将动态库中的地址存入导入符号表；如果符号位于静态库，则用静态库地址处的整段代码替换掉占位符；如果没找到就报错：未定义的符号。
5. 如果输出的是可执行文件，会将程序入口定为内部 main 函数的地址，如果找不到会报错：未定义符号。
6.  生成属于该输出文件的导出符号表，并存入输出文件中。

具体过程参见一本很棒的书： [Linkers and Loaders](<https://www.iecc.com/linker/>) 。

### 运行

操作系统加载进程时会首先将整个文件载入内存，然后扫描可执行文件导入符号表，如果该符号的地址是绝对路径，则直接查找该符号所在的程序库文件；如果该符号的地址是相对路径（往往只有文件名），会在系统默认路径、当前路径和环境变量 LD_LIBRARY_PATH 中指令的路径去查找。如果找到，就会将该程序库也载入内存，并将导入符号表中的地址替换为内存地址，找不到就会报错：找不到库文件。接下来会递归式的对载入的程序库执行上述操作。所有外部库都扫描结束后，根据指令的程序入口开始执行指令。

### 常用生成工具简介

#### g++ 整合编译器

编译及链接的整合工具。常用参数列举如下：

- `-o` ：指令输出的文件名，如：`g++ main.cpp -o main，`如不指令则会输a.out
- `-c` ：单独编译一个源文件而不执行链接，如：`g++ -c main.cpp -o main.o`
- `-std=c++11` ：使用C++11的语法，如：`g++ -std=c++11 -c main.cpp`
- `-I` ：指定头文件查找路径
- `-L` ：指定链接库查找路径，如果要使用绝对路径
- `-l` ：指定链接库，链接库会给名字自动加入前缀 lib 和后缀 .so
- `-fPIC -shared` ：生成动态库必须指写，并且要指定输出文件的扩展名为 .o

#### ld/ar 链接器

ld ：用于链接生成动态库，一般无需手动调用，用g++可实现同样功能。

ar ：用于链接生成静态库 .a，功能很强大，即可以将多个目标文件 .o 整合为一个静态库 .a ，还可以将一个静态库 .a 分解为多个目标文件 .o。

#### ldd/lddtree 链接查看工具

ldd 用来查看一个可执行程序文件或一个程序库的导入符号表。lddtree是ldd的升级版，可以以树形结构递归的显示自身以及导入库的导入符号表。

###