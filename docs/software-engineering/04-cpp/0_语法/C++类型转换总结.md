---
title: C++类型转换总结
author: Sunglow
top: false
cover: false
toc: false
mathjax: false
summary: >-
  类型转换就是将给定类型的表达式转换为另一种类型。C++中的转型可分为两种：隐式类型转换和显式类型转换。下面将详细介绍这两种转型操作，以及各自的适用场景，潜在问题，最终将总结使用类型转换操作应牢记的原则。 
categories:
  - 软件工程
  - Cpp
tags:
  - 编程
  - C++
date: 2018-03-21 21:25:13
keywords:
---

# C++类型转换

类型转换就是将给定类型的表达式转换为另一种类型。C++中的转型可分为两种：隐式类型转换和显式类型转换。下面将详细介绍这两种转型操作，以及各自的适用场景，潜在问题，最终将总结使用类型转换操作应牢记的原则。  

# 一、隐式类型转换
C语言中的类型转换属于旧式的类型转换，其使用比较简单，只要在待转换的变量前加上转换的类型即可，然后括号可以加在原变量上面，也可以加在类型名称上面。旧式类型转换在代码中不容易分辨（无论是对人还是对程序），并且没有做任何的安全检查，在C++中不推荐使用。  
```C
void test1() //C语言类型转换
{
	int a0 = 123;
	bool a1 = a0;			
	bool a2 = (bool)a0;
	bool a3 = bool(a0);
	cout << a0 << " " << a1 << " " << a2 << " " << a3 << endl;//123 1 1 1
}
```

讲到类型转换需要提下计算机存储数值的方式，主要分为**大端模式**（Big Endian）和**小端模式**（Little Endian）。

计算机中基本的存储单元一个字节，对应一个地址。一些数值需要占用多个字节，如4字节整数int，这时该数值的每个字节是怎么排列的呢？

在小端模式中，数字的低位字节放在低地址中，高位字节放在高地址中。大端模式则相反（实际中网络协议采用大端模式传输数据，因此也被称为网络字节序）。比如说0×12345678，0×78是该数的低位字节，0×12是该数的高位字节。那么在小端模式中，从低地址到高地址依次是：0×78 0×56 0×34 0×12，而在大端模式中则为：0×12 0×34 0×56 0×78。即大端模式中的数字跟我们习惯的数字顺序相符合，小端模式则相反。   

在类型转换时，有时涉及到一个变量的前若干个字节，这时跟大小端模式相关，需要根据不同系统进行判断。如下面代码所示：    
```
void test2() //大小端模式例子
{
	int tem = 0x12345678;
	char ch = (char)tem;
	if (ch == 0x12)		cout << "ch == 0x12" << endl;//大端模式
	else if(ch == 0x78) cout << "ch == 0x78" << endl;//小端模式
}
```

# 二、显式类型转换

C++扩展了C语言的类型转换，主要分为const_cast，static_cast，dynamic_cast，reinterpret_cast四种:    
const_cast，字面上理解就是去const属性。
static_cast，命名上理解是静态类型转换。如int转换成char。
dynamic_cast，命名上理解是动态类型转换。如子类和父类之间的多态类型转换。
reinterpret_cast，仅仅重新解释类型，但没有进行二进制的转换。
4种类型转换的格式，如：TYPE B = static_cast(TYPE)(a)。  

##  1. const_cast类型转换
const_cast类型转换可以去掉类型的const或volatile属性，把const类型的指针变为非const类型的指针，从而可以修改所指向元素的值。

取出const属性的类型转换只有const_cast可以完成，而非const变成const可以用其他的如static_cast完成。
```C
void test3() //测试const_cast
{
	const A a0;
	cout << a0.a << endl;//123
	A& a1 = const_cast<A &>(a0);
	a1.a = 321;
	cout << a0.a << endl;//321
}
```

下面再看一个例子，理论上应该有a=b=321，但实际上却出现了a和b地址一样，但内容不一样的情况。  

其实这涉及到常量折叠的概念：编译器进行语法分析的时候，将常量表达式计算求值，并用求得的值来替换表达式，放入常量表，可以算作一种编译优化。在编译器的优化的过程中，在预编译阶段会把用const修饰的变量以内容替换掉，类似于宏替换。在运行阶段时，它的内存里的内容是可以改变的。  
```C
void test4() //测试常量折叠
{
	const int a = 123;
	int &b = const_cast<int &>(a);
	b = 321;
	cout << "a：地址=" << &a << ", 值=" << a << endl;//a：地址=002EFA60, 值=123
	cout << "b：地址=" << &b << ", 值=" << b << endl;//b：地址=002EFA60, 值=321

	//如果a定义改为 volatile const int a = 123; 
	//则最终a和b的值一致，因为加了volatile之后每次都会去真实地址取值。
}

```

## 2. static_cast类型转换
static_cast（静态类型转换），类似于C风格的强制转换，无条件转换。主要用于：  

基本数据类型(int等)转换，不能进行无关类型（如非父类和子类）指针之间的转换。  
基类和子类之间转换：其中子类指针转换成父类指针是安全的；但父类指针转换成子类指针是不安全的。(基类和子类之间的动态类型转换建议用dynamic_cast)  
把空指针转换成目标类型的空指针。  
把任何类型的表达式转换成void类型。  
static_cast不能去掉类型的const、volitale属性(用const_cast)。
```C
void test5() //测试static_cast
{
	int n = 123;
	double m = static_cast<int>(n); //基本类型转换
	int *pn = &n;
	void *p = static_cast<void *>(pn); //转成void指针

	A *pa = new A();
	B *pb = new B();
	A *p1 = static_cast<A *>(pb); //子类转父类，安全
	cout << p1->a << endl; //123
	B *p2 = static_cast<B *>(pa); //父类转子类，不安全
	cout << p2->a << endl; //123
	cout << p2->b << endl; //无意义数字
}

```

## 3. dynamic_cast类型转换

有条件转换，动态类型转换，运行时类型安全检查(转换失败返回NULL)：  
1. 安全的基类和子类之间转换。  
2. 必须要有虚函数。  
3. 相同基类不同子类之间的交叉转换。但结果是NULL。  
4. 

dynamic_cast（动态类型转换），用于运行时检查该转换是否类型安全。主要用于类层次间的上行转换和下行转换，以及类之间的交叉转换，其中上行转换可以正常进行，下行转换和交叉转换会进行安全处理。   

该操作符只在多态类型时合法，该类至少具有一个虚函数。因为dynamic_cast需要c++的RTTI（runtime type identification,运行时类型识别）的支持。而运行时类型检查需要运行时类型信息，这个信息存储在类的虚函数表中，只有定义了虚函数的多态类才有虚函数表。 

对指针进行dynamic_cast，失败返回null，成功返回正常cast后的对象指针；   
对引用进行dynamic_cast，失败抛出一个异常，成功返回正常cast后的对象引用。  
```C
void test6() //测试dynamic_cast
{
	A *pa = new A();
	B *pb = new B();

	A *p1 = dynamic_cast<A *>(pb);	//子类转父类，安全
	if (p1 == NULL) cout << "p1 == NULL" << endl;
	else			cout << p1->a << endl; //123

	B *p2 = dynamic_cast<B *>(pa);	//父类转子类，安全， 指针结果是NULL
	if (p2 == NULL) cout << "p2 == NULL" << endl; //p2 == NULL
	else cout << p2->a << " " << p2->b << endl;;

	A a;
	try
	{
		B &b = dynamic_cast<B &>(a); //父类转子类，安全， 引用结果抛出异常
	}
	catch (exception ex)
	{
		cout << ex.what() << endl; //Bad dynamic_cast!
	}
}
```

## 4. reinterpret_cast类型转换  
仅仅重新解释类型，但没有进行二进制的转换：
1. 转换的类型必须是一个指针、引用、算术类型、函数指针或者成员指针。  
2. 在比特位级别上进行转换。它可以把一个指针转换成一个整数，也可以把一个整数转换成一个指针（先把一个指针转换成一个整数，在把该整数转换成原类型的指针，还可以得到原先的指针值）。但不能将非32bit的实例转成指针。  
3. 最普通的用途就是在函数指针类型之间进行转换。  
4. 很难保证移植性。  


reinterpret_cast执行低级转型，实际动作结果可能取决于编译器，一般情况下是不可移植的。  

该转换修改了操作数类型,但仅仅是**重新解释了给出的对象的比特模型而没有进行二进制转换**  。可以将一种类型的指针转换为另一种类型的指针，或者将整数转换成指针，将指针转换成整数。

其使用比较灵活，但危险度较高，实际中比较少用。
```C
void test7() //测试reinterpret_cast
{
	int a = 123;
	//double *pb = &a;				//编译错误，不能将int*转成double*
	double *pb = reinterpret_cast<double *>(&a); //int*转成double*
	int *p = reinterpret_cast<int *>(a);//int转成int*
	int c = reinterpret_cast<int>(p);	//int*转成int
	cout << "a = " << a << endl;		//a = 123
	cout << "p = " << p << endl;		//p = 0000007B（123的16进制表示）
	cout << "c = " << c << endl;		//c = 123
}

```

参考文献

[1]文章来自NoAlGo博客 http://noalgo.info/461.html
[2]http://www.cnblogs.com/goodhacker/archive/2011/07/20/2111996.html