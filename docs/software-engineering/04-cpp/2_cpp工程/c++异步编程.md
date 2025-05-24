---
title: c++异步编程
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

# C++ Task 的实现（lambda 是个好东西）

本文目的在于实现一个简单易用的 task 类。它的目的在于将将要执行的动作和上下文相关信息（如参数）保存起来，然后在想要执行的时候，发起这个动作。

## 1. 添加了对类成员函数的支持


完成品的实例如下：

```
struct A
{

    int b;
};
task t([](A a, A b, int c)->void
{
    a.a += b.b+c;
}, A(233), A(A_v), 4);

t();1234567891011
```

那么这个task 的应用场景在哪里呢？

举个例子。 
 在socket 的编程中，我们需要一个线程来处理接收到的buffer 信息。 
 然而，每个socket 开一个线程太昂贵了。 
 所以，我们可以开固定个线程，然后把需要处理的信息放进task里，再把task 丢进任务列表里面就行了。

## Lambda 函数的本质

C++ 11 开始支持匿名函数，也就是lambda 语法。 
 一个典型的lambda 例子如下：

```
auto func = [this](int a) -> void 
{
    //todo something
};
12345
```

lambda 函数和一般函数的区别在于，它多了个捕获列表，也就是`[]` 括起来的那些内容。捕获类型又分为按值捕获和按引用捕获。

那么，不妨思考一下，Lambda 函数的本质是什么。

假如你是编译期厂商，你拿到了C++ 委员会发布的C++11 标准，老板要求你支持lambda 函数的语法，你会怎么去实现呢。

我提一下自己的想法。 
 在我看来，一个Lambda 函数，在编译的时候，编译期会检测它捕获的上下文相关信息，然后生成一个匿名结构体。这个结构体有一个 `operator()` 函数，包含有捕获的上下文变量信息。

那还是举个例子吧：

```C++
int a, b;
auto func = [a, &b](int c)->int
{
    b += a + c;
    return b;   
};1234567
```

编译期看到上面的这些信息之后，就生成了下面这个结构体：

```c++
struct lambda_abcd
{
    lambda_abcd(int p1, int& p2) : p1(p1), p2(p2) {}
    int operator() (int c) 
    {
        p2 += p1 + c;
        return p2;
    }

    int p1, &p2;
};1234567891011
```

Lambda 函数最宝贵的特性，在于它能够由编译器自动生成我们需要的结构体类。 
 不妨思考一下，Lambda 函数的sizeof 到底是多少？捕获一个变量和捕获两个变量是一样的吗？可以试着验证一下。

## **task 类的需求**

前面铺垫了一下lambda 函数的特性，因为它是实现task 关键的技术。

下面说一下设计task 的思想。

我的目的是，一个简单易用的task 类，它除了能够保存参数的信息，将其延后调用之外，尽量看起来和正常的函数行为是一样的。

那我们就可以用lambda 函数，将参数的信息保存起来了。

首先，先设计一个接口，task 的执行单元：

```c++
struct task_unit
{
    virtual ~task_unit() {}
    virtual void operator() () {}
};
```

它主要是定义了 `operator ()` 操作符，使得它像函数一样产生作用。 
 （* 个人喜欢，可能do 或者work 的函数命名也是可以的）

接下来我们设计一个模板类，让它能够适配不同的lambda 函数。

```
template<class Func>
struct task_unit_impl : public task_unit
{
    task_unit_impl(Func in_func) : m_func(in_func) {}

    virtual void operator() ()
    {
        m_func();
    }


private:
    Func m_func;
};
```

当我们写这个的时候，应该时刻谨记着，Func 是能够接受任何可以调用 operator() 操作符的类型。 
 可以是函数指针，重载了operator() 的结构体，lambda 函数等。 
 而且，在**构造函数中，m_func是传值实例化**的。明白这点的话，就要清楚地知道，当你写代码的时候，它的代价是多少。

现在我们终于可以掏出一个面向客户的task 类了。 
 emm，先简单设计一下接口：

```
class task
{
public:
    template<class Func, class .... Args>
    task(Func func,Args&& ... args)
    {}

    void reset() // todo
    void operator () ()
    {
        (*m_task_unit)();
    }
private:
    task_unit* m_task_unit;
};123456789101112131415
```

到达这一步之后，核心就是如何实例化一个合适的`m_task_unit` ，让它可以胜任保存 函数体和参数的作用。

不过在此之前，我希望先解释一下可变参数和模板的推导规则。

注意到，task 的构造函数接受一个函数体（我称呼它为函数体，实际上它可以是任何接受operator() 操作符调用的类型），和可变参数。函数体是按值传递，而可变参数是右值引用的方式。 
 实际上，**右值引用是可变参数的通用表达形式**。

C++ 的模板推导规则中，符合以下条例：

- 左值 + && 变成左值引用
- 右值 + && 变成右值

（更多可查询【1】【2】），也就是说，不管传递的参数是什么，最后要么是左值引用（也就是一般的引用类型， &），要么是右值引用（&&）。

当我们传递参数的时候，有三种情况：

- 按值传参
- 按左值引用传参
- 按右值引用传参

由于使用可变参数模板，按值传参是无法实现的（实际上我们也可以选择按值传参，但是这样就没有办法按引用传参了，也许可以使用一些小技巧，但是这会违背我们一开始设计的初衷）。

万幸的是，按**右值传参可以实现按值传参**的效果！真棒！

解决了这个问题之后，我们继续思考，如何利用Lambda 函数来捕获参数信息呢？ 
 由于使用了可变参数模板，Lambda 函数捕获的时候，要么就按值捕获，要么就按引用捕获。这并不是我们想要的。

emmm，似乎遇到了一点麻烦，不过还不碍事。 
 我们可以通过设计一个模板类，它可以把引用类型的参数保存起来，然后让Lambda 函数按值去捕获它。

```
    template<class T>
    struct parameter_packer
    {
    };

    template<class T>
    struct parameter_packer<T&>
    {
        parameter_packer(T& value) : param(value) {}

        T& param;
    };

    template<class T>
    struct parameter_packer<T&&>
    {
        parameter_packer(T&& value) : param( move(value) ) {}

        parameter_packer(const parameter_packer& other) : param(move(other.param) ){}

        T&& param;
    };12345678910111213141516171819202122
```

这个模板类它会完美地将引用类型保存起来。

```
template<class T>
    struct parameter_packer<T&&>
123
```

这个版本为什么会额外需要一个copy constructor 呢。 
 因为它有一个右值引用的类成员。当按值传递 parameter_packer 的时候，需要正确的实现它。

到了这一步，我们终于把所有的内容都铺垫好了，接下来把函数内容补充完全。

```
template<class Func, class ... Args>
        task(Func func, Args&& ... args)
        {
            reset_impl(func, parameter_packer< decltype( std::forward<Args>(args) ) >
                (std::forward<Args>(args )  ) ...);
        }
    private:
        template<class Func, class ... Args>
        void reset_impl(Func func, Args ... args)
        {
            auto lambda_helper = [func, args ...]()->void
            {
                func( (args.param)...);
            };

            m_task_unit.reset( new task_unit_impl<decltype(lambda_helper) >(lambda_helper)  );
        }1234567891011121314151617
```

到这一步，就算是完成主要的内容了。

通过parameter_packer 的设计，完成函数参数的封装。

## **如何支持类成员函数**

在实际的使用过程中，我们经常会调用类的成员函数。 
 那么对类的成员函数有支持，将会极大丰富它的功能性。

但是，调用类的成员函数，是需要保存类的对象指针的。 
 所以，针对这个版本，我们可以实现如下：

```
template<class ReturnT, class ClassT, class ... ClassFuncArgs>
void reset(ReturnT(ClassT::* func)(ClassFuncArgs ...), ClassT * obj_ptr, ClassFuncArgs&& ... args)
{
    reset_class_func_impl(func, obj_ptr, parameter_packer<decltype(std::forward<ClassFuncArgs>(args)) >
        (std::forward<ClassFuncArgs>(args)) ...);
}

template<class Func, class T, class ... Args>
void reset_class_func_impl(Func func, T* obj_ptr, Args ... args)
{
    auto lambda_helper = [func, obj_ptr, args ...]()->void
    {
        (obj_ptr->*func)((args.param) ...);
    };

    m_task_unit.reset(new task_unit_impl<decltype(lambda_helper) >(lambda_helper));
}1234567891011121314151617
```

注意到，对于类的成员函数指针，模板是能够标识出来的。通过这个，我们就可以编写针对类成员函数指针的reset 函数了。 
 第二个参数是调用这个对象的类指针。

使用示例如下：

```
class A
{
public:
    void dosome(int a)
    {
        int i = a;
        i += 4;

        cout << i << endl;
    }
};

A a;

xj::task t;
t.reset(&A::dosome, &a, 5);

t();123456789101112131415161718
```

## **补全代码**

接着把一下内容补全就行，包括：

- task 的copy 和 move 的constructor 和 assignment 函数
- 一个reset 函数
- 将`m_task_unit` 用智能指针封装起来，进行资源管理

emmm，看起来一切都很好。 
 不对，还有一个问题没有解决。

如果我们本意是希望按值传递参数的话，可能需要做点小小的改动。 
 看代码：

```
struct A {};

void somefunction(int i, A a) {}

int i = 3;
A value;
task t(somefunction, i, value); //错误的按值传递
task t2(somefunction, int(i), A(value) );  // int 没有按值传递，A 按值传递。因为该死的int()语法是强制转换语法
12345678910
```

**所以，事实上按值传递的参数将会被转发为右值引用，从而被parameter_packer 保存下来。**

所以，我们可以加一个函数：

```
    template<class T>
    inline T param_maker(T value)
    {
        return value;
    }123456
```

用法变成了如下：

```
task t(somefunction, param_maker(i), param_maker(value) ); 1
```

emm，把一些都处理好以后，整个文件的内容如下：

```
#pragma once

#include <utility>
#include <memory>

#include <iostream>
using namespace std;

namespace xj
{

    struct task_unit
    {
        virtual ~task_unit() {}
        virtual void operator() () {}
    };


    template<class Func>
    struct task_unit_impl : public task_unit
    {
        task_unit_impl(Func in_func) : m_func(in_func) {}

        virtual void operator() ()
        {
            m_func();
        }


    private:
        Func m_func;
    };

    template<class T>
    struct parameter_packer
    {
    };

    template<class T>
    struct parameter_packer<T&>
    {
        parameter_packer(T& value) : param(value) {}

        T& param;
    };

    template<class T>
    struct parameter_packer<T&&>
    {
        parameter_packer(T&& value) : param(move(value)) {}

        parameter_packer(const parameter_packer& other) : param(move(other.param)) {}

        T&& param;
    };

    template<class T>
    inline T param_maker(T value)
    {
        return value;
    }


    class task
    {
    public:

        task() : m_task_unit(nullptr)
        {

        }

        task(task& other)
        {
            m_task_unit.swap(other.m_task_unit);
        }

        task(task&& other) :m_task_unit(std::move(other.m_task_unit)) {}


        task& operator = (task& other)
        {
            if (&other != this)
            {
                m_task_unit.swap(other.m_task_unit);
            }
            return *this;
        }

        task& operator = (task&& other)
        {
            if (&other != this)
            {
                m_task_unit = std::move(other.m_task_unit);
            }
            return *this;
        }

        template<class Func, class ... Args>
        task(Func func, Args&& ... args)
        {
            reset_impl(func, parameter_packer< decltype(std::forward<Args>(args)) >
                (std::forward<Args>(args)) ...);
        }

        template<class Func>
        task(Func func)
        {
            m_task_unit.reset(new task_unit_impl<Func >(func));
        }

        template<class ReturnT, class ClassT, class ... ClassFuncArgs>
        task(ReturnT(ClassT::* func)(ClassFuncArgs ...), ClassT * obj_ptr, ClassFuncArgs&& ... args)
        {
            reset_class_func_impl(func, obj_ptr, parameter_packer<decltype(std::forward<ClassFuncArgs>(args)) >
                (std::forward<ClassFuncArgs>(args)) ...);
        }

        template<class Func, class ... Args>
        void reset(Func func, Args&& ... args)
        {
            reset_impl(func, parameter_packer<decltype(std::forward<Args>(args)) >
                (std::forward<Args>(args)) ...);
        }

        template<class ReturnT, class ClassT, class ... ClassFuncArgs>
        void reset(ReturnT(ClassT::* func)(ClassFuncArgs ...), ClassT * obj_ptr, ClassFuncArgs&& ... args)
        {
            reset_class_func_impl(func, obj_ptr, parameter_packer<decltype(std::forward<ClassFuncArgs>(args)) >
                (std::forward<ClassFuncArgs>(args)) ...);
        }

        template<class Func>
        void reset(Func func)
        {
            m_task_unit.reset(new task_unit_impl<Func >(func));
        }

        void operator() ()
        {
            if (m_task_unit)
            {
                (*m_task_unit)();
            }
        }

    private:
        template<class Func, class ... Args>
        void reset_impl(Func func, Args ... args)
        {
            auto lambda_helper = [func, args ...]()->void
            {
                func((args.param)...);
            };

            m_task_unit.reset(new task_unit_impl<decltype(lambda_helper) >(lambda_helper));
        }

        template<class Func, class T, class ... Args>
        void reset_class_func_impl(Func func, T* obj_ptr, Args ... args)
        {
            auto lambda_helper = [func, obj_ptr, args ...]()->void
            {
                (obj_ptr->*func)((args.param) ...);
            };

            m_task_unit.reset(new task_unit_impl<decltype(lambda_helper) >(lambda_helper));
        }




        std::unique_ptr<task_unit> m_task_unit;
    };

}//xj123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161162163164165166167168169170171172173174175176
```

以下附上一些测试用例。 
 最后，欢迎一起交流想法。

```
#include <iostream>
using namespace std;

#include "xj_task.h"
using namespace xj;

struct A
{
    A(int in_b = 4) : b(in_b)
    {
        cout << "A constructor " << endl;
    }

    A(const A& other) : b(other.b)
    {
        cout << "A copy contructor " << endl;
    }

    A(A&& other)
    {
        b = other.b;
        cout << "A move" << endl;
    }

    ~A()
    {
        cout << "A destructor" << endl;
    }
    int b;
};

int somefunc(int a, A& b)
{
    cout << a + b.b << endl;

    return a + b.b;
}

int somefunc(int*a, A&b)
{
    cout << *a + b.b << endl;
    return *a + b.b;
}

using first_type = int(*)(int, A&);
using second_type = int(*)(int*, A&);


void empty_func()
{
    cout << "empty_func" << endl;
}


int main()
{

    task tt(empty_func);
    tt();

    if (false)
    {
        A A_v;
        A& A_ref = A_v;

        int int_v = 3;
        task t(first_type(somefunc), param_maker(int_v), A_v);
        task t2((first_type)somefunc, 3, A(A_v));
        task t3((first_type)somefunc, 3, A_ref);

        task t4((second_type)somefunc, &int_v, A_v);


        task t5([](A a, A b, int c)->void
        {
            cout << somefunc(c, a) + b.b << endl;
        }, A(233), A(A_v), 4);


        //t(), t2(), t3();


        ++int_v;
        t();
        t2();
        t3();

        ++int_v;
        t4();

        t5();

        task t6;
        t6.reset(first_type(somefunc), param_maker(int_v), A_v);
        t6();

        task t7(t5);

        t7();
        t5();

    }

    {
        task t((first_type)somefunc, 4, A(233) );
        task t2(t);
        t2();
        t();
    }
}

```

【参考资料】 
 【1】[模板实参推导](http://zh.cppreference.com/w/cpp/language/template_argument_deduction) 
 【2】[引用声明](http://zh.cppreference.com/w/cpp/language/reference#.E8.BD.AC.E5.8F.91.E5.BC.95.E7.94.A8)