---
title: C++静态成员变量和静态成员函数
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
date: 2018-02-22 21:25:13
keywords:
---

# C++静态成员变量和静态成员函数   

[转][原文](http://blog.163.com/sunshine_linting/blog/static/448933232011810101848652/)
> 注意：  
类中静态变量在类外初始化格式：   
＜数据类型＞＜类名＞::＜静态数据成员名＞=＜值＞  
加上声明是为了在构造函数之前运行。
```
private: 
int a,b,c; 
static const int Sum[];//声明静态数据成员!!!!!!!!!!!!!!!  
}; 
const int Myclass::Sum={1,2,3,4,5,8};//定义并初始化静态数据成员!!!!!!!! 
```

数据成员可以分静态变量、非静态变量两种.   

*静态成员：* 静态类中的成员加入static修饰符,即是静态成员.可以直接使用类名::静态成员名访问此静态成员,因为静态成员存在于内存,非静态成员需要实例化才会分配内存,所以静态成员不能访问非静态的成员.因为静态成员存在于内存,所以非静态成员可以直接访问类中静态的成员.   

*非成静态员：* 所有没有加Static的成员都是非静态成员,当类被实例化之后,可以通过实例化的类名进行访问.非静态成员的生存期决定于该类的对象的生存期..而静态成员则不存在生存期的概念,因为静态成员始终驻留在内容中.. 

一个类中也可以包含静态成员和非静态成员,类中也包括静态构造函数和非静态构造函数.. 

分两个方面来总结，第一方面主要是相对于面向过程而言，即在这方面不涉及到类，第二方面相对于面向对象而言，主要说明static在类中的作用。 

## 一、在面向过程设计中的static关键字 
### 1、静态全局变量 

定义：在全局变量前，加上关键字 static 该变量就被定义成为了一个静态全局变量。 

特点：   
- 　　A、该变量在全局数据区分配内存。  
- 　　B、初始化：如果不显式初始化，那么将被隐式初始化为0(自动变量是随机的,除非显式地初始化)。
- 　　C、访变量只在本源文件可见，严格的讲应该为定义之处开始到本文件结束。
- 　　D、文件作用域下声明的const的常量默认为static存储类型。

　　例（摘于C++程序设计教程---钱能主编P103）： ```
```
　　//file1.cpp 
　　//Example 1 
    #include <iostream.h> 
    void fn(); 
    static int n; //定义静态全局变量 
    void main() 
    { 
        n=20; 
        cout < <n < <endl; 
        fn(); 
    } 

    void fn() 
    { 
        n++; 
        cout < <n < <endl; 
    } 
```


静态变量都在全局数据区分配内存，包括后面将要提到的静态局部变量。对于一个完整的程序，在内存中的分布情况如下： 

>代码区   
全局数据区   
堆区   
栈区   


　　一般程序的由new产生的动态数据存放在堆区，函数内部的自动变量存放在栈区。自动变量一般会随着函数的退出而释放空间，静态数据（即使是函数内部的静态局部变量）也存放在全局数据区。全局数据区的数据并不会因为函数的退出而释放空间。细心的读者可能会发现，Example 1中的代码中将 

                static int n; //定义静态全局变量 

改为: 

int n; //定义全局变量程序照样正常运行。的确，定义全局变量就可以实现变量在文件中的共享，但定义静态全局变量还有以下好处： 

静态全局变量不能被其它文件所用；(好像是区别extern的) 
其它文件中可以定义相同名字的变量，不会发生冲突； 
您可以将上述示例代码改为如下： 
```
//Example 2 
//File1 
#include <iostream.h> 
void fn(); 
static int n; //定义静态全局变量(只能在本文件中使用) 
void main() 
{ 
n=20; 
cout < <n < <endl; 
fn(); 
} 

//File2 
#include <iostream.h> 
extern int n;(可在别的文件中引用这个变量) 
void fn() 
{ 
n++; 
cout < <n < <endl; 
}
```


编译并运行Example2，您就会发现上述代码可以分别通过编译，但link时出现错误。

试着将 static int n; //定义静态全局变量 
改为 
int n; //定义全局变量 

再次编译运行程序，细心体会全局变量和静态全局变量的区别。

### 2、静态局部变量   
定义：在局部变量前加上static关键字时，就定义了静态局部变量。我们先举一个静态局部变量的例子，如下：//Example 3 
```
#include <iostream.h> 
void fn(); 
void main() 
{ 
fn(); 
fn(); 
fn(); 
} 
void fn() 
{ 
static n=10; 
cout < <n < <endl; 
n++; 
} 
```

　　通常，在函数体内定义了一个变量，每当程序运行到该语句时都会给该局部变量分配栈内存。但随着程序退出函数体，系统就会收回栈内存，局部变量也相应失效。    
　　但有时候我们需要在两次调用之间对变量的值进行保存。通常的想法是定义一个全局变量来实现。但这样一来，变量已经不再属于函数本身了，不再仅受函数的控制，给程序的维护带来不便。   
　　静态局部变量正好可以解决这个问题。静态局部变量保存在全局数据区，而不是保存在栈中，每次的值保持到下一次调用，直到下次赋新值。    

特点：    
- 　　A、该变量在全局数据区分配内存。   
- 　　B、初始化：如果不显式初始化，那么将被隐式初始化为0,以后的函数调用不再进行初始化。   
- 　　C、它始终驻留在全局数据区，直到程序运行结束。但其作用域为局部作用域，当定义它的函数或　语句块结束时，其作用域随之结束。  



### 3、静态函数（注意与类的静态成员函数区别）  
定义：在函数的返回类型前加上static关键字，函数即被定义成静态函数。    
特点：     　　
-  A、静态函数与普通函数不同，它只能在声明它的文件当中可见，不能被其它文件使用。    　　

静态函数的例子：  
```
//Example 4 
#include <iostream.h> 
static void fn();//声明静态函数 
void main() 
{ 
fn(); 
} 
void fn()//定义静态函数 
{ 
int n=10; 
cout < <n < <endl; 
}
```
定义静态函数的好处：静态函数不能被其它文件所用；   
其它文件中可以定义相同名字的函数，不会发生冲突；  

##  二、面向对象的static关键字（类中的static关键字）   

### 1、静态数据成员 

在类内数据成员的声明前加上关键字static，该数据成员就是类内的静态数据成员。    
先举一个静态数据成员的例子。 
```
//Example 5 
#include <iostream.h> 
class Myclass 
{ 
public: 
Myclass(int a,int b,int c); 
void GetSum(); 

private: 
int a,b,c; 
static int Sum;//声明静态数据成员!!!!!!!!!!!!!!!  
}; 
int Myclass::Sum=0;//定义并初始化静态数据成员!!!!!!!! 

Myclass::Myclass(int a,int b,int c) 
{ 
this->a=a; 
this->b=b; 
this->c=c; 
Sum+=a+b+c; 
} 

void Myclass::GetSum() 
{ 
cout < <"Sum=" < <Sum < <endl; 
} 

void main() 
{ 
Myclass M(1,2,3); 
M.GetSum(); 
Myclass N(4,5,6); 
N.GetSum(); 
M.GetSum(); 

}
```

可以看出，静态数据成员有以下特点：   

对于非静态数据成员，每个类对象都有自己的拷贝。而静态数据成员被当作是类的成员。无论这个类的对象被定义了多少个，静态数据成员在程序中也只有一份拷贝，由该类型的所有对象共享访问。也就是说，静态数据成员是该类的所有对象所共有的。对该类的多个对象来说，静态数据成员只分配一次内存，供所有对象共用。所以，静态数据成员的值对每个对象都是一样的，它的值可以更新；   
静态数据成员存储在全局数据区。静态数据成员定义时要分配空间，所以不能在类声明中定义。在Example 5中，语句 ```    int Myclass::Sum=0;     ```  是定义静态数据成员；   
静态数据成员和普通数据成员一样遵从public,protected,private访问规则；   
因为静态数据成员在全局数据区分配内存，属于本类的所有对象共享，所以，它不属于特定的类对象，在没有产生类对象时其作用域就可见，即在没有产生类的实例时，我们就可以操作它；   

静态数据成员初始化与一般数据成员初始化不同。  

> 静态数据成员初始化的格式为：   
==＜数据类型＞＜类名＞::＜静态数据成员名＞=＜值＞==   
类的静态数据成员有两种访问形式：   
==＜类对象名＞.＜静态数据成员名＞ 或＜类类型名＞::＜静态数据成员名＞==   

如果静态数据成员的访问权限允许的话（即public的成员），可在程序中，按上述格式来引用静态数据成员 ；   
静态数据成员主要用在各个对象都有相同的某项属性的时候。比如对于一个存款类，每个实例的利息都是相同的。所以，应该把利息设为存款类的静态数据成员。   
这有两个好处，第一，不管定义多少个存款类对象，利息数据成员都共享分配在全局数据区的内存，所以节省存储空间。
第二，一旦利息需要改变时，只要改变一次，则所有存款类对象的利息全改变过来了；   
同全局变量相比，使用静态数据成员有两个优势：  

静态数据成员没有进入程序的全局名字空间，因此不存在与程序中其它全局名字冲突的可能性；    
可以实现信息隐藏。静态数据成员可以是private成员，而全局变量不能；    

### 2、静态成员函数 

　　与静态数据成员一样，我们也可以创建一个静态成员函数，它为类的全部服务而不是为某一个类的具体对象服务。静态成员函数与静态数据成员一样，都是类的内部实现，属于类定义的一部分。普通的成员函数一般都隐含了一个this指针，this指针指向类的对象本身，因为普通成员函数总是具体的属于某个类的具体对象的。通常情况下，this是缺省的。如函数fn()实际上是this->fn()。
但是与普通函数相比，静态成员函数由于不是与任何的对象相联系，因此它不具有this指针。从这个意义上讲，它无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，它只能调用其余的静态成员函数。
下面举个静态成员函数的例子。 
```
//Example 6 
#include <iostream.h> 
class Myclass 
{ 
public: 
Myclass(int a,int b,int c); 
static void GetSum();//声明静态成员函数 
private: 
int a,b,c; 
static int Sum;//声明静态数据成员 
}; 
int Myclass::Sum=0;//定义并初始化静态数据成员 

Myclass::Myclass(int a,int b,int c) 
{ 
this->a=a; 
this->b=b; 
this->c=c; 
Sum+=a+b+c; //非静态成员函数可以访问静态数据成员 
} 

void Myclass::GetSum() //静态成员函数的实现 
{ 
// cout < <a < <endl; //错误代码，a是非静态数据成员 静态成员函数由于不是与任何的对象相联系，因此它不具有this指针。从这个意义上讲，它无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，它只能调用其余的静态成员函数。
cout < <"Sum=" < <Sum < <endl; 
} 

void main() 
{ 
Myclass M(1,2,3); 
M.GetSum(); 
Myclass N(4,5,6); 
N.GetSum(); 
Myclass::GetSum(); 
} 
```
关于静态成员函数，可以总结为以下几点：  


出现在类体外的函数定义不能指定关键字static；   
静态成员之间可以相互访问，包括静态成员函数访问静态数据成员和访问静态成员函数；     
非静态成员函数可以任意地访问静态成员函数和静态数据成员；   
静态成员函数不能访问非静态成员函数和非静态数据成员；    
由于没有this指针的额外开销，因此静态成员函数与类的全局函数相比速度上会有少许的增长；    
调用静态成员函数，可以用成员访问操作符(.)和(->)为一个类的对象或指向类对象的指针调用静态成员函数.