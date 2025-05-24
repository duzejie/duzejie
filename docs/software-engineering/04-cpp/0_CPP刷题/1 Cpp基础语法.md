# C/C++基础

### 1. C++中的基本数据类型及派生类型
1）整型 int
2）浮点型 单精度float，双精度double
3）字符型 char
4）逻辑型 bool
5）控制型 void

基本类型的字长及其取值范围可以放大和缩小，改变后的类型就叫做基本类型的派生类型。派生类型声明符由基本类型关键字char、int、float、double前面加上类型修饰符组成。
类型修饰符包括：
>short 短类型，缩短字长
>long 长类型，加长字长
>signed 有符号类型，取值范围包括正负值
>unsigned 无符号类型，取值范围只包括正值

### 1. const知道吗？解释一下其作用

const修饰类的成员变量，表示常量不可能被修改
const修饰类的成员函数，表示该函数不会修改类中的数据成员，不会调用其他非const的成员函数
const函数只能调用const函数，非const函数可以调用const函数
```cpp
//const 使用

class A  
{  
private:    const int a;                // 常对象成员，只能在初始化列表赋值

public:    // 构造函数    
A() : a(0) { };    A(int x) : a(x) { };        // 初始化列表

// const可用于对重载函数的区分    
int getValue();             // 普通成员函数    
int getValue() const;       // 常成员函数，不得修改类中的任何数据成员的值  
};

void function()  
{    // 对象    
A b;                        // 普通对象，可以调用全部成员函数、更新常成员变量    
const A a;                  // 常对象，只能调用常成员函数    
const A *p = &a;            // 常指针    
const A &q = a;             // 常引用

// 指针    
char greeting[] = "Hello";    
char* p1 = greeting;                // 指针变量，指向字符数组变量    
const char* p2 = greeting;          // 指针变量，指向字符数组常量    
char* const p3 = greeting;          // 常指针，指向字符数组变量    
const char* const p4 = greeting;    // 常指针，指向字符数组常量  
}

// 函数  
void function1(const int Var);           // 传递过来的参数在函数内不可变  
void function2(const char* Var);         // 参数指针所指内容为常量  
void function3(char* const Var);         // 参数指针为常指针  
void function4(const int& Var);          // 引用参数在函数内为常量

// 函数返回值  
const int function5();      // 返回一个常数  
const int* function6();     // 返回一个指向常量的指针变量，使用：const int *p = function6();  
int* const function7();     // 返回一个指向变量的常指针，使用：int* const p = function7();
```


### 1. 定义和声明的区别

- 声明是告诉编译器变量的类型和名字，不会为变量分配空间
- 定义需要分配空间，同一个变量可以被声明多次，但是只能被定义一次

### 3. 结构体struct和共同体union（联合）的区别

结构体：将不同类型的数据组合成一个整体，是自定义类型
共同体：不同类型的几个变量共同占用一段内存
1）结构体中的每个成员都有自己独立的地址，它们是同时存在的；
共同体中的所有成员占用同一段内存，它们不能同时存在；
2）sizeof(struct)是内存对齐后所有成员长度的总和，sizeof(union)是内存对齐后最长数据成员的长度、

结构体为什么要内存对齐呢？
1.平台原因（移植原因）：不是所有的硬件平台都能访问任意地址上的任意数据，某些硬件平台只能在某些地址处取某些特定类型的数据，否则抛出硬件异常
2.硬件原因：经过内存对齐之后，CPU的内存访问速度大大提升。

### 1. define和const的区别

1）#define定义的常量没有类型，所给出的是一个立即数；const定义的常量有类型名字，存放在静态区域
2）处理阶段不同，#define定义的宏变量在预处理时进行替换，可能有多个拷贝，const所定义的变量在编译时确定其值，只有一个拷贝。
3）#define定义的常量是不可以用指针去指向，const定义的常量可以用指针去指向该常量的地址
4）#define可以定义简单的函数，const不可以定义函数

### 1. typdef和define区别

\#define是预处理命令，在预处理是执行简单的替换，不做正确性的检查
typedef是在编译时处理的，它是在自己的作用域内给已经存在的类型一个别名
```cpp
typedef (int*) pINT;
#define pINT2 int*
```

效果相同？实则不同！实践中见差别：pINT a,b;的效果同int \*a; int \*b;表示定义了两个整型指针变量。而pINT2 a,b;的效果同int \*a, b;表示定义了一个整型指针变量a和整型变量b。

### 1. C++的四种强制转换

类型转化机制可以分为隐式类型转换和显示类型转化（强制类型转换）
(new-type) expression
new-type (expression)

隐式类型转换比较常见，在混合类型表达式中经常发生；四种强制类型转换操作符：

`static_cast、dynamic_cast、const_cast、reinterpret_cast`

1）static_cast ：编译时期的静态类型检查
`static_cast < type-id > ( expression )`

该运算符把expression转换成type-id类型，在编译时使用类型信息执行转换，在转换时执行必要的检测（指针越界、类型检查），其操作数相对是安全的

- 用于基本数据类型之间的转换，例如 short 转 int、int 转 double、const 转非 const、向上转型等；

void 指针和具体类型指针之间的转换

static_cast只能在有相互联系的类型中进行相互转换,不一定包含虚函数

来自 <[https://www.cnblogs.com/Allen-rg/p/6999360.html](https://www.cnblogs.com/Allen-rg/p/6999360.html)>

2）dynamic_cast：运行时的检查

用于在集成体系中进行安全的向下转换downcast，即基类指针/引用->派生类指针/引用
dynamic_cast是4个转换中唯一的RTTI操作符，提供运行时类型检查。
dynamic_cast如果不能转换返回NULL
dynamic_cast转为引用类型的时候转型失败会抛bad_cast
源类中必须要有虚函数，保证多态，才能使用dynamic_cast<source>(expression)

3）const_cast
去除const常量属性，使其可以修改 ; volatile属性的转换

4）reinterpret_cast
主要有三种强制转换用途：改变指针或引用的类型、将指针或引用转换为一个足够长度的整形、将整型转换为指针或引用类型。
不安全！reinterpret_cast强制转换过程仅仅只是比特位的拷贝，因此在使用过程中需要特别谨慎！

### 1. C++中指针和引用的区别

1）指针是一个新的变量，存储了另一个变量的地址，我们可以通过访问这个地址来修改另一个变量；
引用只是一个别名，还是变量本身，对引用的任何操作就是对变量本身进行操作，以达到修改变量的目的
2）引用只有一级，而指针可以有多级
3）指针传参的时候，还是值传递，指针本身的值不可以修改，需要通过解引用才能对指向的对象进行操作
引用传参的时候，传进来的就是变量本身，因此变量可以被修改

### 1. new、delete、malloc、free之间的关系
new/delete,malloc/free都是动态分配内存的方式
1）malloc对开辟的空间大小严格指定，而new只需要对象名
2）new为对象分配空间时，调用对象的构造函数，delete调用对象的析构函数
既然有了malloc/free，C++中为什么还需要new/delete呢？

运算符是语言自身的特性，有固定的语义，编译器知道意味着什么，由编译器解释语义，生成相应的代码。
库函数是依赖于库的，一定程度上独立于语言的。编译器不关心库函数的作用，只保证编译，调用函数参数和返回值符合语法，生成call函数的代码。

malloc/free是库函数，new/delete是C++运算符。对于非内部数据类型而言，光用malloc/free无法满足动态对象都要求。new/delete是运算符，编译器保证调用构造和析构函数对对象进行初始化/析构。但是库函数malloc/free是库函数，不会执行构造/析构。

### 1. delete和delete[]的区别

delete只会调用一次析构函数，而delete[]会调用每个成员的析构函数
用new分配的内存用delete释放，用new[]分配的内存用delete[]释放

### 1. inline 内联函数

- 相当于把内联函数里面的内容写在调用内联函数处；
- 相当于不用执行进入函数的步骤，直接执行函数体；
- 相当于宏，却比宏多了类型检查，真正具有函数特性；
- 编译器一般不内联包含循环、递归、switch 等复杂操作的内联函数；
- 在类声明中定义的函数，除了虚函数的其他函数都会自动隐式地当成内联函数。

inline 使用

```cpp

// 声明1（加 inline，建议使用）  
inline int functionName(int first, int second,...);

// 声明2（不加 inline）  
int functionName(int first, int second,...);

// 定义  
inline int functionName(int first, int second,...) {/****/};

// 类内定义，隐式内联  
class A {    int doA() { return 0; }         // 隐式内联  
}

// 类外定义，需要显式内联  
class A {    int doA();  
}  
inline int A::doA() { return 0; }   // 需要显式内联
```


编译器对 inline 函数的处理步骤

1. 将 inline 函数体复制到 inline 函数调用点处；
2. 为所用 inline 函数中的局部变量分配内存空间；
3. 将 inline 函数的的输入参数和返回值映射到调用方法的局部变量空间中；
4. 如果 inline 函数有多个返回点，将其转变为 inline 函数代码块末尾的分支（使用 GOTO）。

优点

1. 内联函数同宏函数一样将在被调用处进行代码展开，省去了参数压栈、栈帧开辟与回收，结果返回等，从而提高程序运行速度。
2. 内联函数相比宏函数来说，在代码展开时，会做安全检查或自动类型转换（同普通函数），而宏定义则不会。
3. 在类中声明同时定义的成员函数，自动转化为内联函数，因此内联函数可以访问类的成员变量，宏定义则不能。
4. 内联函数在运行时可调试，而宏定义不可以。

缺点

1. 代码膨胀。内联是以代码膨胀（复制）为代价，消除函数调用带来的开销。如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。
2. inline 函数无法随着函数库升级而升级。inline函数的改变需要重新编译，不像 non-inline 可以直接链接。
3. 是否内联，程序员不可控。内联函数只是对编译器的建议，是否对函数内联，决定权在于编译器。

虚函数（virtual）可以是内联函数（inline）吗？

[Are "inline virtual" member functions ever actually "inlined"?](http://www.cs.technion.ac.il/users/yechiel/c++-faq/inline-virtuals.html)

- 虚函数可以是内联函数，内联是可以修饰虚函数的，但是当虚函数表现多态性的时候不能内联。
- 内联是在编译器建议编译器内联，而虚函数的多态性在运行期，编译器无法知道运行期调用哪个代码，因此虚函数表现为多态性时（运行期）不可以内联。
- inline virtual 唯一可以内联的时候是：编译器知道所调用的对象是哪个类（如 Base::who()），这只有在编译器具有实际对象而不是对象的指针或引用时才会发生。

虚函数内联使用

```cpp

#include <iostream> using namespace std;  
class Base  
{  
public:        
inline virtual void who()  {  
        cout << "I am Base\n";  
       }        
        virtual ~Base() {}  
};  
class Derived : public Base  
{  
public:
inline void who() {  // 不写inline时隐式内联        
	 cout << "I am Derived\n";  
   }  
};

int main()  
{        // 此处的虚函数 who()，是通过类（Base）的具体对象（b）来调用的，编译期间就能确定了，所以它可以是内联的，但最终是否内联取决于编译器。        
		Base b;  
        b.who();
// 此处的虚函数是通过指针调用的，呈现多态性，需要在运行时期间才能确定，所以不能为内联。  
		Base *ptr = new Derived();  
        ptr->who();

// 因为Base有虚析构函数（virtual ~Base() {}），所以 delete 时，会先调用派生类（Derived）析构函数，再调用基类（Base）析构函数，防止内存泄漏。        
		delete ptr;  
        ptr = nullptr;

       system("pause");
       return 0;  
}

```






