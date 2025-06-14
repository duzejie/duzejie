# 外观模式(Facade Pattern)——提供统一的入口

说明：设计模式系列文章是读`刘伟`所著`《设计模式的艺术之道(软件开发人员内功修炼之道)》`一书的阅读笔记。个人感觉这本书讲的不错，有兴趣推荐读一读。详细内容也可以看看此书作者的博客`https://blog.csdn.net/LoveLion/article/details/17517213`

## 模式概述

绝大多数`B/S`系统都有一个首页或者导航页面，大部分`C/S`系统都提供了菜单或者工具栏，在这里，首页和导航页面就充当了B/S系统的外观角色，而菜单和工具栏充当了C/S系统的外观角色，通过它们用户可以快速访问子系统，增强了软件的易用性。

在软件开发中，有时候为了完成一项较为复杂的功能，一个客户类需要和多个业务类交互，而这些需要交互的业务类经常会作为一个整体出现，由于涉及到的类比较多，导致使用时代码较为复杂，此时，特别需要一个类似服务员一样的角色，由它来负责和多个业务类进行交互，而客户类只需与该类交互。`外观模式`通过引入一个外观角色(`Facade`)来简化客户端与子系统(`Subsystem`)之间的交互，为复杂的子系统调用提供一个统一的入口，降低子系统与客户端的耦合度，使得客户端调用非常方便。

### 模式定义

外观模式中，一个子系统的外部与其内部的通信通过一个统一的外观类进行，外观类将客户类与子系统的内部复杂性分隔开，使得客户类只需要与外观角色打交道，而不需要与子系统内部的很多对象打交道。

> 外观模式(`Facade Pattern`)：为子系统中的一组接口提供一个统一的入口。外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用

外观模式又称为门面模式，它是一种对象结构型模式。外观模式是`迪米特法则`的一种具体实现，通过引入一个新的外观角色可以降低原有系统的复杂度，同时降低客户类与子系统的耦合度。

### 模式结构图

外观模式没有一个一般化的类图描述，下图所示的类图也可以作为描述外观模式的结构图：

![外观模式结构图](attachments/facade-pattern/0601bf597049eed4ce378ab264b87924_MD5.png)

外观模式包含如下两个角色：

- Facade（外观角色）：在客户端可以调用它的方法，在外观角色中可以知道相关的（一个或者多个）子系统的功能和责任；在正常情况下，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理。

- SubSystem（子系统角色）：在软件系统中可以有一个或者多个子系统角色，每一个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能；每一个子系统都可以被客户端直接调用，或者被外观角色调用，它处理由外观类传过来的请求；子系统并不知道外观的存在，对于子系统而言，外观角色仅仅是另外一个客户端而已。

### 模式伪代码

外观模式中所指的`子系统`是一个广义的概念，它可以是一个类、一个功能模块、系统的一个组成部分或者一个完整的系统。子系统类通常是一些业务类，实现了一些具体的、独立的业务功能，其典型代码如下：

```java
public class SubSystemA {

    public void methodA() {
        //业务实现代码
    }
}

public class SubSystemB {

    public void methodB() {
        //业务实现代码
    }
}

public class SubSystemC {

    public void methodC() {
        //业务实现代码
    }
}
```

引入外观类，与子系统业务类之间的交互统一由外观类来完成
```java
public class Facade {
    private SubSystemA obj1 = new SubSystemA();
    private SubSystemB obj2 = new SubSystemB();
    private SubSystemC obj3 = new SubSystemC();

    public void method() {
        obj1.methodA();
        obj2.methodB();
        obj3.methodC();
    }
}
```

由于在外观类中维持了对子系统对象的引用，客户端可以通过外观类来间接调用子系统对象的业务方法，而无须与子系统对象直接交互。引入外观类后，客户端代码变得非常简单，典型代码如下：
```java
public static void main(String[] args) {
    Facade facade = new Facade();
    facade.method();
}
```

## 模式改进

在标准的外观模式中，如果需要增加、删除或更换与外观类交互的子系统类，必须修改外观类或客户端的源代码，这将违背开闭原则，因此可以通过引入抽象外观类来对系统进行改进，在一定程度上可以解决该问题。在引入抽象外观类之后，客户端可以针对抽象外观类进行编程，对于新的业务需求，不需要修改原有外观类，而对应增加一个新的具体外观类，由新的具体外观类来关联新的子系统对象。

定义抽象外观类
```java
public abstract class AbstractFacade {
    public abstract void method();
}
```
根据具体的场景，实现具体的外观类
```java
public class Facade1 extends AbstractFacade {

    private SubSystemA obj1 = new SubSystemA();
    private SubSystemB obj2 = new SubSystemB();

    @Override
    public void method() {
        obj1.methodA();
        obj2.methodB();
    }
}

public class Facade2 extends AbstractFacade {

    private SubSystemB obj1 = new SubSystemB();
    private SubSystemC obj2 = new SubSystemC();

    @Override
    public void method() {
        obj1.methodB();
        obj2.methodC();
    }
}
```

客户端针对抽象外观类进行编程，代码片段如下：
```java
public static void main(String[] args) {
    AbstractFacade facade = new Facade1();
    // facade = new Facade2();
    facade.method();
}
```

## 模式应用

个人认为外观模式某些情况下可以看成是对既有系统的再次封装，所以各种类库、工具库(比如`hutool`)、框架基本都有外观模式的影子。外观模式让调用方更加简洁，不用关心内部的实现，与此同时，也让越来越多的程序猿多了个`调包侠`的昵称(当然了这其中也包括笔者●´ω｀●行无际)。

所以，你可能在很多开源代码中看到类似`XxxBootstrap`、`XxxContext`、`XxxMain`等类似的`Class`，再追进去看一眼，你可能发现里面关联了一大堆的复杂的对象，这些对象对于外层调用者来说几乎是透明的。

例子太多，以致于不知道举啥例子(实际是偷懒的借口O(∩_∩)O哈哈~)。

## 模式总结

外观模式并不给系统增加任何新功能，它仅仅是简化调用接口。在几乎所有的软件中都能够找到外观模式的应用。所有涉及到与多个业务对象交互的场景都可以考虑使用外观模式进行重构。

### 主要优点

(1) 它对客户端屏蔽了子系统组件，减少了客户端所需处理的对象数目，并使得子系统使用起来更加容易。通过引入外观模式，客户端代码将变得很简单，与之关联的对象也很少。

(2) 它实现了子系统与客户端之间的松耦合关系，这使得子系统的变化不会影响到调用它的客户端，只需要调整外观类即可。

(3) 一个子系统的修改对其他子系统没有任何影响，而且子系统内部变化也不会影响到外观对象。

### 适用场景

(1) 当要为访问一系列复杂的子系统提供一个简单入口时可以使用外观模式。

(2) 客户端程序与多个子系统之间存在很大的依赖性。引入外观类可以将子系统与客户端解耦，从而提高子系统的独立性和可移植性。

(3) 在层次化结构中，可以使用外观模式定义系统中每一层的入口，层与层之间不直接产生联系，而通过外观类建立联系，降低层之间的耦合度。