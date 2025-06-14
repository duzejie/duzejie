---
title: C++ paser命令行
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
date: 2018-02-21 21:25:13
keywords:
---

# [命令行选项解析函数(C语言)：getopt()和getopt_long()](https://www.cnblogs.com/chenliyang/p/6633739.html)



 上午在看源码项目 webbench 时，刚开始就被一个似乎挺陌生函数 getopt_long() 给卡住了，说实话这函数没怎么见过，自然不知道这哥们是干什么的。于是乎百度了一番，原来是处理命令行选项参数的，的确，正规点的大型程序一般第一步就是处理命令行参数的，接着才是主干程序。在百度和 man 的帮助下，找到了具体使用方法和解释，二话不说赶紧学习一下，并总结出文档记录一下。

​    平时在写程序时常常需要对命令行参数进行处理，因为参数少，自己解析就可以搞定；如果命令行个数比较多时，如果按照顺序一个一个定义参数含义很容易造成混乱，而且如果程序只按顺序处理参数的话，一些“可选参数”的功能将很难实现，这个问题在 linux 中用 getopt 等函数可以优雅地解决。

### **一、查询linux命令手册：**



```
#include<unistd.h>
#include<getopt.h>          /*所在头文件 */
int getopt(intargc, char * const argv[], const char *optstring);
int getopt_long(int argc, char * const argv[], const char *optstring,
                          const struct option *longopts, int*longindex);
int getopt_long_only(int argc, char * const argv[],const char *optstring,
                          const struct option *longopts, int*longindex);
extern char *optarg;         /*系统声明的全局变量 */
extern int optind, opterr, optopt;
```



先拿最简单的 getopt 函数开刀，getopt_long 只是前者的增强版，功能多点而已。

### **二、getopt函数**

**1、定义**：

```
int getopt(int argc, char * const argv[], const char *optstring);
```

**2、描述**：

```
getopt是用来解析命令行选项参数的，但是只能解析短选项: -d 100,不能解析长选项：--prefix
```

**3、参数**：

```
argc：main()函数传递过来的参数的个数
argv：main()函数传递过来的参数的字符串指针数组
optstring：选项字符串，告知 getopt()可以处理哪个选项以及哪个选项需要参数
```

**4、返回**：

如果选项成功找到，返回选项字母；如果所有命令行选项都解析完毕，返回 -1；如果遇到选项字符不在 optstring 中，返回字符 '?'；如果遇到丢失参数，那么返回值依赖于 optstring 中第一个字符，如果第一个字符是 ':' 则返回':'，否则返回'?'并提示出错误信息。

**5、下边重点举例说明optstring的格式意义：**

```
char*optstring = “ab:c::”;
单个字符a         表示选项a没有参数            格式：-a即可，不加参数
单字符加冒号b:     表示选项b有且必须加参数      格式：-b 100或-b100,但-b=100错
单字符加2冒号c::   表示选项c可以有，也可以无     格式：-c200，其它格式错误
```

上面这个 optstring 在传入之后，getopt 函数将依次检查命令行是否指定了 -a， -b， -c(这需要多次调用 getopt 函数，直到其返回-1)，当检查到上面某一个参数被指定时，函数会返回被指定的参数名称(即该字母)

```
optarg —— 指向当前选项参数(如果有)的指针。
optind —— 再次调用 getopt() 时的下一个 argv指针的索引。
optopt —— 最后一个未知选项。
opterr ­—— 如果不希望getopt()打印出错信息，则只要将全域变量opterr设为0即可。
```

以上描述的并不生动，下边结合实例来理解：

**6、实例：**



```
#include<stdio.h>
#include<unistd.h>
#include<getopt.h>
int main(intargc, char *argv[])
{
    int opt;
    char *string = "a::b:c:d";
    while ((opt = getopt(argc, argv, string))!= -1)
    {  
        printf("opt = %c\t\t", opt);
        printf("optarg = %s\t\t",optarg);
        printf("optind = %d\t\t",optind);
        printf("argv[optind] = %s\n",argv[optind]);
    }  
}
```





**编译上述程序并执行结果：**

**输入选项及参数正确的情况**

```
dzlab:~/test/test#./opt -a100 -b 200 -c 300 -d
opt = a         optarg = 100            optind = 2              argv[optind] = -b
opt = b         optarg = 200            optind = 4              argv[optind] = -c
opt = c         optarg = 300            optind = 6              argv[optind] = -d
opt = d         optarg = (null)         optind = 7              argv[optind] = (null)
```

**或者这样的选项格式(注意区别)：**

```
dzlab:~/test/test#./opt -a100 -b200 -c300 -d 
opt = a         optarg = 100            optind = 2              argv[optind] = -b200
opt = b         optarg = 200            optind = 3              argv[optind] = -c300
opt = c         optarg = 300            optind = 4              argv[optind] = -d
opt = d         optarg = (null)         optind = 5              argv[optind] = (null)
```

**选项a是可选参数，这里不带参数也是正确的**

```
dzlab:~/test/test#./opt -a -b 200 -c 300 -d   
opt = a         optarg = (null)         optind = 2              argv[optind] = -b
opt = b         optarg = 200            optind = 4              argv[optind] = -c
opt = c         optarg = 300            optind = 6              argv[optind] = -d
opt = d         optarg = (null)         optind = 7              argv[optind] = (null)
```

**输入选项参数错误的情况**

```
dzlab:~/test/test#./opt -a 100 -b 200 -c 300 -d
opt = a         optarg = (null)         optind = 2              argv[optind] = 100
opt = b         optarg = 200            optind = 5              argv[optind] = -c
opt = c         optarg = 300            optind = 7              argv[optind] = -d
opt = d         optarg = (null)         optind = 8              argv[optind] = (null)
```

导致解析错误，第一个 optarg = null，实际输入参数 100，由于格式不正确造成的(可选参数格式固定)

**参数丢失，也会导致错误，c选项是必须有参数的，不加参数提示错误如下：**

```
dzlab:~/test/test#./opt -a -b 200 -c      
opt = a         optarg = (null)         optind = 2              argv[optind] = -b
opt = b         optarg = 200            optind = 4              argv[optind] = -c
./opt: optionrequires an argument -- 'c'
opt = ?         optarg = (null)         optind = 5              argv[optind] = (null)
```

这种情况，optstring 中第一个字母不是':'，如果在 optstring 中第一个字母加':'，则最后丢失参数的那个选项 opt 返回的是':'，不是'?'，并且没有提示错误信息，这里不再列出。

**命令行选项未定义，-e选项未在optstring中定义，会报错：**

```
dzlab:~/test/test#./opt -a -b 200 -e
opt = a         optarg = (null)         optind = 2              argv[optind] = -b
opt = b         optarg = 200             optind = 4              argv[optind] = -e
./opt: invalidoption -- 'e'
opt = ?         optarg = (null)         optind = 5              argv[optind] = (null)
```

到这里应该已经把getopt函数的功能讲解清楚了吧，下边来说说 getopt_long 函数，getopt_long 函数包含了 getopt 函数的功能，并且还可以指定"长参数"(或者说长选项)，与 getopt 函数对比，getopt_long 比其多了两个参数：

### **三、getopt_long函数**

**1、定义**：

```
int getopt_long(int argc, char * const argv[], const char *optstring,

                                 const struct option *longopts,int *longindex);
```

**2、描述**：

```
包含 getopt 功能，增加了解析长选项的功能如：--prefix --help
```

**3、参数**：

```
longopts    指明了长参数的名称和属性
longindex   如果longindex非空，它指向的变量将记录当前找到参数符合longopts里的第几个元素的描述，即是longopts的下标值
```

**4、返回**：

```
对于短选项，返回值同getopt函数；对于长选项，如果flag是NULL，返回val，否则返回0；对于错误情况返回值同getopt函数
5、struct option
struct option {
const char  *name;       /* 参数名称 */
int          has_arg;    /* 指明是否带有参数 */
int          *flag;      /* flag=NULL时,返回value;不为空时,*flag=val,返回0 */
int          val;        /* 用于指定函数找到选项的返回值或flag非空时指定*flag的值 */
}; 
```

**6、参数说明：**

```
has_arg  指明是否带参数值，其数值可选：
no_argument         表明长选项不带参数，如：--name, --help
required_argument  表明长选项必须带参数，如：--prefix /root或 --prefix=/root
optional_argument  表明长选项的参数是可选的，如：--help或 –prefix=/root，其它都是错误
```

接着看一下实例操作会更加深刻地理解：

**7、实例：**

```
int main(intargc, char *argv[])
{
    int opt;
    int digit_optind = 0;
    int option_index = 0;
    char *string = "a::b:c:d";
    static struct option long_options[] =
    {  
        {"reqarg", required_argument,NULL, 'r'},
        {"optarg", optional_argument,NULL, 'o'},
        {"noarg",  no_argument,         NULL,'n'},
        {NULL,     0,                      NULL, 0},
    }; 
    while((opt =getopt_long_only(argc,argv,string,long_options,&option_index))!= -1)
    {  
        printf("opt = %c\t\t", opt);
        printf("optarg = %s\t\t",optarg);
        printf("optind = %d\t\t",optind);
        printf("argv[optind] =%s\t\t", argv[optind]);
        printf("option_index = %d\n",option_index);
    }  
}
```



编译上述程序并执行结果：

**正确输入长选项的情况**

```

dzlab:~/test/test#./long --reqarg 100 --optarg=200 --noarg
opt = r optarg =100     optind = 3   argv[optind] = --optarg=200  option_index = 0
opt = o optarg =200     optind = 4   argv[optind] = --noarg        option_index = 1
opt = n optarg =(null) optind = 5    argv[optind] =(null)          option_index = 2
```

**或者这种方式：**

```
dzlab:~/test/test#./long –reqarg=100 --optarg=200 --noarg
opt = r optarg =100     optind = 2   argv[optind] = --optarg=200  option_index = 0
opt = o optarg =200     optind = 3   argv[optind] = --noarg        option_index = 1
opt = n optarg =(null) optind = 4    argv[optind] =(null)          option_index = 2
```

**可选选项可以不给参数**

```
dzlab:~/test/test#./long --reqarg 100 --optarg --noarg   
opt = r optarg =100     optind = 3     argv[optind] = --optarg option_index = 0
opt = o optarg =(null) optind = 4      argv[optind] =--noarg   option_index = 1
opt = n optarg =(null) optind = 5      argv[optind] =(null)     option_index = 2
```

**输入长选项错误的情况**

```
dzlab:~/test/test#./long --reqarg 100 --optarg 200 --noarg 
opt = r optarg =100     optind = 3     argv[optind] = --optarg  option_index= 0
opt = o optarg =(null) optind = 4      argv[optind] =200        option_index = 1
opt = n optarg =(null) optind = 6      argv[optind] =(null)     option_index = 2
```

这时，虽然没有报错，但是第二项中 optarg 参数没有正确解析出来(格式应该是 —optarg=200)

**必须指定参数的选项，如果不给参数，同样解析错误如下：**

```
dzlab:~/test/test#./long --reqarg --optarg=200 --noarg    
opt = r optarg =--optarg=200  optind = 3 argv[optind] =--noarg  option_index = 0
opt = n optarg =(null)         optind = 4 argv[optind] =(null)    option_index = 2
```

长选项的举例说明暂且就这么多吧，其它如选项错误、缺参数、格式不正确的情况自己再试验一下。

### 四、getopt_long_only函数

getopt_long_only 函数与 getopt_long 函数使用相同的参数表，在功能上基本一致，只是 getopt_long 只将 --name 当作长参数，但 getopt_long_only 会将 --name 和 -name 两种选项都当作长参数来匹配。getopt_long_only 如果选项 -name 不能在 longopts 中匹配，但能匹配一个短选项，它就会解析为短选项。



