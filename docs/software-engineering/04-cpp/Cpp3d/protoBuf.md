---
title: Google Protocol Buffer
author: Sunglow
top: false
cover: false
toc: false
mathjax: false
summary: 'null'
categories:
  - 软件工程
  - 工具
tags:
  - 编程
  - C++
  - 3dPart
date: 2018-03-23 21:25:13
keywords:
---

# 简介

什么是 Google Protocol Buffer？ 假如您在网上搜索，应该会得到类似这样的文字介绍：

Google Protocol Buffer( 简称 Protobuf) 是 Google 公司内部的混合语言数据标准，目前已经正在使用的有超过 48,162 种报文格式定义和超过 12,183 个 .proto 文件。他们用于 RPC 系统和持续数据存储系统。

Protocol Buffers 是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。目前提供了 C++、Java、Python 三种语言的 API。

或许您和我一样，在第一次看完这些介绍后还是不明白 Protobuf 究竟是什么，那么我想一个简单的例子应该比较有助于理解它。

# 一个简单的例子

## 安装 Google Protocol Buffer

- 编译安装

  在网站 http://code.google.com/p/protobuf/downloads/list上可以下载 Protobuf 的源代码。然后解压编译安装便可以使用它了。

  安装步骤如下所示：

  ```
  tar -xzf protobuf-2.1.0.tar.gz 
  cd protobuf-2.1.0 
  ./configure --prefix=$INSTALL_DIR 
  make 
  make check 
  make install
  ```

- Ubuntu下安装

  ```shell
  sudo apt install  protobuf-compiler
  sudo apt install libprotobuf-dev
  ```

  

## 关于简单例子的描述

我打算使用 Protobuf 和 C++ 开发一个十分简单的例子程序。

该程序由两部分组成。第一部分被称为 Writer，第二部分叫做 Reader。

Writer 负责将一些结构化的数据写入一个磁盘文件，Reader 则负责从该磁盘文件中读取结构化数据并打印到屏幕上。

准备用于演示的结构化数据是 HelloWorld，它包含两个基本数据：

- ID，为一个整数类型的数据
- Str，这是一个字符串

## 书写 .proto 文件

首先我们需要编写一个 proto 文件，定义我们程序中需要处理的结构化数据，在 protobuf 的术语中，结构化数据被称为 Message。proto 文件非常类似 java 或者 C 语言的数据定义。代码清单 1 显示了例子应用中的 proto 文件内容。

清单 1. proto 文件

```protobuf
package lm; 
message helloworld 
{ 
   required int32     id = 1;  // ID 
   required string    str = 2;  // str 
   optional int32     opt = 3;  //optional field 
}
```

一个比较好的习惯是认真对待 proto 文件的文件名。比如将命名规则定于如下：

```
packageName.MessageName.proto
```

在上例中，==package 名字叫做 lm==，定义了一个消息 helloworld，该消息有三个成员，类型为 int32 的 id，另一个为类型为 string 的成员 str。opt 是一个可选的成员，即消息中可以不包含该成员。

## 编译 .proto 文件

写好 proto 文件之后就可以用 Protobuf 编译器将该文件编译成目标语言了。本例中我们将使用 C++。

假设您的 proto 文件存放在 $SRC_DIR 下面，您也想把生成的文件放在同一个目录下，则可以使用如下命令：

```
protoc -I=$SRC_DIR --cpp_out=$DST_DIR $SRC_DIR/addressbook.proto
```

命令将生成两个文件：

lm.helloworld.pb.h ， 定义了 C++ 类的头文件

lm.helloworld.pb.cc ， C++ 类的实现文件

在生成的头文件中，定义了一个 C++ 类 helloworld，后面的 Writer 和 Reader 将使用这个类来对消息进行操作。诸如对消息的成员进行赋值，将消息序列化等等都有相应的方法。

## 编写 writer 和 Reader

如前所述，Writer 将把一个结构化数据写入磁盘，以便其他人来读取。假如我们不使用 Protobuf，其实也有许多的选择。一个可能的方法是将数据转换为字符串，然后将字符串写入磁盘。转换为字符串的方法可以使用 sprintf()，这非常简单。数字 123 可以变成字符串”123”。

这样做似乎没有什么不妥，但是仔细考虑一下就会发现，这样的做法对写 Reader 的那个人的要求比较高，Reader 的作者必须了 Writer 的细节。比如”123”可以是单个数字 123，但也可以是三个数字 1,2 和 3，等等。这么说来，我们还必须让 Writer 定义一种分隔符一样的字符，以便 Reader 可以正确读取。但分隔符也许还会引起其他的什么问题。最后我们发现一个简单的 Helloworld 也需要写许多处理消息格式的代码。

如果使用 Protobuf，那么这些细节就可以不需要应用程序来考虑了。

使用 Protobuf，Writer 的工作很简单，需要处理的结构化数据由 .proto 文件描述，经过上一节中的编译过程后，该数据化结构对应了一个 C++ 的类，并定义在 lm.helloworld.pb.h 中。对于本例，类名为 lm::helloworld。

Writer 需要 include 该头文件，然后便可以使用这个类了。

现在，在 Writer 代码中，将要存入磁盘的结构化数据由一个 lm::helloworld 类的对象表示，它提供了一系列的 get/set 函数用来修改和读取结构化数据中的数据成员，或者叫 field。

当我们需要将该结构化数据保存到磁盘上时，类 lm::helloworld 已经提供相应的方法来把一个复杂的数据变成一个字节序列，我们可以将这个字节序列写入磁盘。

对于想要读取这个数据的程序来说，也只需要使用类 lm::helloworld 的相应反序列化方法来将这个字节序列重新转换会结构化数据。这同我们开始时那个“123”的想法类似，不过 Protobuf 想的远远比我们那个粗糙的字符串转换要全面，因此，我们不如放心将这类事情交给 Protobuf 吧。

程序清单 2 演示了 Writer 的主要代码，您一定会觉得很简单吧？

清单 2. Writer 的主要代码

```cpp
#include "lm.helloworld.pb.h"
#include <fstream>
using namespace std;

 int main(void) 
 { 
   
  lm::helloworld msg1; 
  msg1.set_id(101); 
  msg1.set_str(“hello”); 
     
  // Write the new address book back to disk. 
  fstream output("./log", ios::out | ios::trunc | ios::binary); 
         
  if (!msg1.SerializeToOstream(&output)) { 
      cerr << "Failed to write msg." << endl; 
      return -1; 
  }         
  return 0; 
 }
```

Msg1 是一个 helloworld 类的对象，set_id() 用来设置 id 的值。SerializeToOstream 将对象序列化后写入一个 fstream 流。

代码清单 3 列出了 reader 的主要代码。

清单 3. Reader

```cpp
#include "lm.helloworld.pb.h" 

#include <fstream>

using namespace std;
 void ListMsg(const lm::helloworld & msg) { 
  cout << msg.id() << endl; 
  cout << msg.str() << endl; 
 } 
  
 int main(int argc, char* argv[]) { 
 
  lm::helloworld msg1; 
  
  { 
    fstream input("./log", ios::in | ios::binary); 
    if (!msg1.ParseFromIstream(&input)) { 
      cerr << "Failed to parse address book." << endl; 
      return -1; 
    } 
  } 
  
  ListMsg(msg1); 
  
 }
```

同样，Reader 声明类 helloworld 的对象 msg1，然后利用 ParseFromIstream 从一个 fstream 流中读取信息并反序列化。此后，ListMsg 中采用 get 方法读取消息的内部信息，并进行打印输出操作。

## 运行结果

运行 Writer 和 Reader 的结果如下：

```
>writer 
>reader 
``101 
``Hello
```

Reader 读取文件 log 中的序列化信息并打印到屏幕上。本文中所有的例子代码都可以在附件中下载。您可以亲身体验一下。

这个例子本身并无意义，但只要您稍加修改就可以将它变成更加有用的程序。==比如将磁盘替换为网络 socket，那么就可以实现基于网络的数据交换任务。而存储和交换正是 Protobuf 最有效的应用领域==。

# 和其他类似技术的比较

看完这个简单的例子之后，希望您已经能理解 Protobuf 能做什么了，那么您可能会说，世上还有很多其他的类似技术啊，比如 XML，JSON，Thrift 等等。和他们相比，Protobuf 有什么不同呢？

简单说来 Protobuf 的主要优点就是：简单，快。

## Protobuf 的优点

Protobuf 有如 XML，不过它更小、更快、也更简单。你可以定义自己的数据结构，然后使用代码生成器生成的代码来读写这个数据结构。你甚至可以在无需重新部署程序的情况下更新数据结构。只需使用 Protobuf 对数据结构进行一次描述，即可利用各种不同语言或从各种不同数据流中对你的结构化数据轻松读写。

它有一个非常棒的特性，即“向后”兼容性好，人们不必破坏已部署的、依靠“老”数据格式的程序就可以对数据结构进行升级。这样您的程序就可以不必担心因为消息结构的改变而造成的大规模的代码重构或者迁移的问题。因为添加新的消息中的 field 并不会引起已经发布的程序的任何改变。

Protobuf 语义更清晰，无需类似 XML 解析器的东西（因为 Protobuf 编译器会将 .proto 文件编译生成对应的数据访问类以对 Protobuf 数据进行序列化、反序列化操作）。

使用 Protobuf 无需学习复杂的文档对象模型，Protobuf 的编程模式比较友好，简单易学，同时它拥有良好的文档和示例，对于喜欢简单事物的人们而言，Protobuf 比其他的技术更加有吸引力。

## Protobuf 的不足

Protbuf 与 XML 相比也有不足之处。它功能简单，无法用来表示复杂的概念。

XML 已经成为多种行业标准的编写工具，Protobuf 只是 Google 公司内部使用的工具，在通用性上还差很多。

由于文本并不适合用来描述数据结构，所以 Protobuf 也不适合用来对基于文本的标记文档（如 HTML）建模。另外，由于 XML 具有某种程度上的自解释性，它可以被人直接读取编辑，在这一点上 Protobuf 不行，它以二进制的方式存储，除非你有 .proto 定义，否则你没法直接读出 Protobuf 的任何内容。





# ProtoBuf教程



## ProtoBuf使用的一般步骤

知道了ProtoBuf的作用与支持的数据类型。我么需要知道ProtoBuf使用的一般步骤，下面以C++中使用ProtoBuf为例来描述使用的一般步骤。

**第一步：**定义proto文件，文件的内容就是定义我们需要存储或者传输的数据结构，也就是定义我们自己的数据存储或者传输的协议。

**第二步：**编译安装protocol buffer编译器来编译自定义的.proto文件，用于生成.pb.h文件（proto文件中自定义类的头文件）和 .pb.cc（proto文件中自定义类的实现文件）。

**第三步：** 使用protoco buffer的C++ API来读写消息。

下面将具体讲解每一步的实现。

## 定义proto文件

定义proto文件就是定义自己的数据存储或者传输的协议格式。我们以上面需要传输的Student对象为例。要想序列化Student对象进行网络传输，那么我们需要从编写一个.proto文件开始。.proto文件的定义是比较简单的：为每一个你需要序列化的数据结构添加一个消息（message），然后为消息（message）中的每一个字段（field）指定一个名字、类型和修饰符以及唯一标识（tag）。每一个消息对应到C++中就是一个类，嵌套消息对应的就是嵌套类，当然一个.proto文件中可以定义多个消息，就像一个头文件中可以定义多个类一样。下面就是一个自定义的嵌套消息的.proto文件student.proto。

```protobuf
package tutorial;
 
message Student{
    required uint64 id = 1;
    required string name =2;
    optional string email = 3;
 
    enum PhoneType {
        MOBILE = 0;
        HOME = 1;
    }
 
    message PhoneNumber { 
        required string number = 1;
        optional PhoneType type = 2 [default = HOME];
    }
    repeated PhoneNumber phone = 4;
}
```

正如你所看到的一样，该语法类似于C++或Java的语法。让我们依次来看看文件的每一部分的作用。

**关于package声明。** 
.proto文件以一个package声明开始。这个声明是为了防止不同项目之间的命名冲突。对应到C++中去，你用这个.proto文件生成的类将被放置在一个与package名相同的命名空间中。

**关于字段类型。** 
再往下看，就是若干消息（message）定义了。一个消息就是某些类型的字段的集合。许多标准的、简单的数据类型都可以用作字段类型，包括bool，int32，float，double以及string。你也可以使用其他的消息（message）类型来作为你的字段类型——在上面的例子中，消息PhoneNumber 就是一个被用作字段类型的例子。

**关于修饰符。** 
每一个字段都必须用以下之一的修饰符来修饰： 

- **required：** 必须提供字段值，否则对应的消息就会被认为是“未初始化的”。如果libprotobuf是以debug模式编译的，序列化一个未初始化的消息（message）将会导致一个断言错误。在优化过的编译情况下（译者注：例如release），该检查会被跳过，消息会被写入。然而，解析一个未初始化的消息仍然会失败（解析函数会返回false）。除此之外，一个required的字段与一个optional的字段就没有区别了。

- **optional：** 字段值指定与否都可以。如果没有指定一个optional的字段值，它就会使用默认值。对简单类型来说，你可以指定你自己的默认值，就像我们在上面的例子中对phone number的type字段所做的一样。如果你不指定默认值，就会使用系统默认值：数据类型的默认值为0，string的默认值为空字符串，bool的默认值为false。对嵌套消息（message）来说，其默认值总是消息的“默认实例”或“原型”，即：没有任何一个字段是指定了值的。调用访问类来取一个未显式指定其值的optional（或者required）的字段的值，总是会返回字段的默认值。
- **repeated：** 字段会重复N次（N可以为0）。重复的值的顺序将被保存在protocol buffer中。你只要将重复的字段视为动态大小的数组就可以了。

>  **注意：** required是永久性的：在把一个字段标识为required的时候，你应该特别小心。如果在某些情况下你不想写入或者发送一个required的字段，那么将该字段更改为optional可能会遇到问题——旧版本的读者（译者注：即读取、解析消息的一方）会认为不含该字段的消息（message）是不完整的，从而有可能会拒绝解析。在这种情况下，你应该考虑编写特别针对于应用程序的、自定义的消息校验函数。Google的一些工程师得出了一个结论：使用required弊多于利；他们更愿意使用optional和repeated而不是required。当然，这个观点并不具有普遍性。

**关于标识。** 
在每一项后面的、类似于“= 1”，“= 2”的标志指出了该字段在二进制编码中使用的唯一“标识（tag）”。标识号1~15编码所需的字节数比更大的标识号使用的字节数要少1个，所以，如果你想寻求优化，可以为经常使用或者重复的项采用1~15的标识（tag），其他经常使用的optional项采用≥16的标识（tag）。在重复的字段中，每一项都要求重编码标识号（tag number），所以重复的字段特别适用于这种优化情况。

你可以在[Language Guide (proto3)](https://developers.google.com/protocol-buffers/docs/proto3)一文中找到编写.proto文件的完整指南（包括所有可能的字段类型）。但是，不要想在里面找到与类继承相似的特性，因为protocol buffers不是拿来做这个的。

## 编译安装Protocol Buffers编译器来编译自定义的.proto文件

在得到一个.proto文件之后，下一步你就要生成可以读写Student消息（当然也就包括了PhoneNumber消息）的类了。此时你需要运行protocol buffer编译器来编译你的.proto文件。

### 编译安装Protocol Buffers

如果你还没有安装该编译器，[下载protobuf源码](https://developers.google.com/protocol-buffers/docs/downloads) ，或直接到[github上下载](https://github.com/google/protobuf)，详情请参照README.md文件中的说明来安装。

To build and install the C++ Protocol Buffer runtime and the Protocol Buffer compiler (protoc) execute the following: 
构建和安装C++ Protocol Buffer runtime和ProProtocol 
Buffer compiler (protoc) 需要执行如下命令：

```
    $ ./configure    
    $ make    
    $ make check    
    $ make install
```

果上面的命令没有出错，那么恭喜你，你就完成了对ProtoBuf源码的编译和安装的工作。下面我们就可以使用ProtoBuf的编译器protoc对我们的.proto文件啦。

我的make check的结果如下： 
![这里写图片描述](20160716210903664)

make install注意权限问题，最好使用sudo make install。安装成功之后，使用which protoc就可以查看protoc已经安装成功了。ProtoBuf默认安装的路径在/usr/local，当然我们可以在配置的时候改变安装路径，使用如下命令：

```
./configure --prefix=/usr
```

安装成功后，我们执行`protoc --version` 查看我们的Protocol Buffer的版本，我使用的版本是：libprotoc 2.6.1。

### 编译我们的.proto文件

有了Protocol Buffers的编译器protoc，我们就可以来编译我们自定义的.proto文件来产生对应的消息类，生成一个头文件 ( 定义.proto文件中的消息类 )，和一个源文件（实现.proto文件中的消息类）。

编译方法。指定源目录（即你的应用程序源代码所在的目录——如果不指定的话，就使用当前目录）、目标目录（即生成的代码放置的目录，通常与$SRC_DIR是一样的），以及你的.proto文件所在的目录。命令如下：

```
protoc -I=$SRC_DIR --cpp_out=$DST_DIR $SRC_DIR/addressbook.proto
```

因为需要生成的是C++类，所以使用了–cpp_out选项参数——protocol buffers也为其他支持的语言提供了类似的选项参数，如`--java_out=OUT_DIR`，指定java源文件生成目录。

以上面自定义的student.proto为例，来编译产生我们的student消息类。运行如下命令：

```
protoc student.proto --cpp_out=./
```

这样就可以在我指定的当前目录下生成如下文件：

```
student.pb.h：声明你生成的类的头文件。
student.pb.cc：你生成的类的实现文件。
```

![这里写图片描述](20160716215859386)

protoc的详细用法参见`protoc -h`。

## 了解Protocol Buffer API

让我们看一下生成的代码，了解一下编译器为你创建了什么样的类和函数。如果你看了编译器protoc为我们生成的student.pb.h文件，就会发现你得到了一个类，它对应于student.proto文件中写的每一个消息（message）。更深入一步，看看Student类：编译器为每一个字段生成了读写函数。例如，对name，id，email以及phone字段，分别有如下函数：

``` cpp
  // required uint64 id = 1;
  inline bool has_id() const;
  inline void clear_id();
  static const int kIdFieldNumber = 1;
  inline ::google::protobuf::uint64 id() const;
  inline void set_id(::google::protobuf::uint64 value);
 
  // required string name = 2;
  inline bool has_name() const;
  inline void clear_name();
  static const int kNameFieldNumber = 2;
  inline const ::std::string& name() const;
  inline void set_name(const ::std::string& value);
  inline void set_name(const char* value);
  inline void set_name(const char* value, size_t size);
  inline ::std::string* mutable_name();
  inline ::std::string* release_name();
  inline void set_allocated_name(::std::string* name);
 
  // optional string email = 3;
  inline bool has_email() const;
  inline void clear_email();
  static const int kEmailFieldNumber = 3;
  inline const ::std::string& email() const;
  inline void set_email(const ::std::string& value);
  inline void set_email(const char* value);
  inline void set_email(const char* value, size_t size);
  inline ::std::string* mutable_email();
  inline ::std::string* release_email();
  inline void set_allocated_email(::std::string* email);
 
  // repeated .tutorial.Student.PhoneNumber phone = 4;
  inline int phone_size() const;
  inline void clear_phone();
  static const int kPhoneFieldNumber = 4;
  inline const ::tutorial::Student_PhoneNumber& phone(int index) const;
  inline ::tutorial::Student_PhoneNumber* mutable_phone(int index);
  inline ::tutorial::Student_PhoneNumber* add_phone();
  inline const ::google::protobuf::RepeatedPtrField< ::tutorial::Student_PhoneNumber >& phone() const;
  inline ::google::protobuf::RepeatedPtrField< ::tutorial::Student_PhoneNumber >* mutable_phone();

```

正如你所看到的，getter函数具有与字段名一模一样的名字，并且是小写的，而setter函数都是以set\_前缀开头。此外，还有has\_前缀的函数，对每一个单一的（required或optional的）字段来说，如果字段被置（set）了值，该函数会返回true。最后，每一个字段还有一个clear_前缀的函数，用来将字段重置（un-set）到空状态（empty state）。

然而，数值类型的字段id就只有如上所述的基本读写函数，name和email字段则有一些额外的函数，因为它们是string——前缀为mutable\_的函数返回string的直接指针（direct pointer）。除此之外，还有一个额外的setter函数。注意：你甚至可以在email还没有被置（set）值的时候就调用mutable_email()，它会被自动初始化为一个空字符串。在此例中，如果有一个单一消息字段，那么它也会有一个mutable\_ 前缀的函数，但是没有一个set\_ 前缀的函数。

重复的字段也有一些特殊的函数——如果你看一下重复字段phone 的那些函数，就会发现你可以： 
		（1）得到重复字段的\_size（换句话说，这个Person关联了多少个电话号码）。

（2）通过索引（index）来获取一个指定的电话号码。

（3）通过指定的索引（index）来更新一个已经存在的电话号码。

（3）向消息（message）中添加另一个电话号码，然后你可以编辑它（重复的标量类型有一个add_前缀的函数，允许你传新值进去）。

关于编译器如何生成特殊字段的更多信息，请查看文章[C++ generated code reference](https://developers.google.com/protocol-buffers/docs/reference/cpp-generated)。

**关于枚举和嵌套类（Enums and Nested Classes）。** 
生成的代码中包含了一个PhoneType 枚举，它对应于.proto文件中的那个枚举。你可以把这个类型当作Student::PhoneType，其值为Student::MOBILE和Student::HOME（实现的细节稍微复杂了点，但是没关系，不理解它也不会影响你使用该枚举）。

编译器还生成了一个名为Student::PhoneNumber的嵌套类。如果你看看代码，就会发现“真实的”类实际上是叫做Student_PhoneNumber，只不过Student内部的一个typedef允许你像一个嵌套类一样来对待它。这一点所造成的唯一的一个区别就是：如果你想在另一个文件中对类进行前向声明（forward-declare）的话，你就不能在C++中对嵌套类型进行前向声明了，但是你可以对Student_PhoneNumber进行前向声明。

**关于标准消息函数（Standard Message Methods）。** 
每一个消息（message）还包含了其他一系列函数，用来检查或管理整个消息，包括：

```
bool IsInitialized() const; //检查是否全部的required字段都被置（set）了值。 
void CopyFrom(const Person& from); //用外部消息的值，覆写调用者消息内部的值。 
void Clear();   //将所有项复位到空状态（empty state）。 
int ByteSize() const;   //消息字节大小
```

**关于Debug的API。**

```
string DebugString() const; //将消息内容以可读的方式输出 
string ShortDebugString() const; //功能类似于，DebugString(),输出时会有较少的空白 
string Utf8DebugString() const; //Like DebugString(), but do not escape UTF-8 byte sequences. 
void PrintDebugString() const;  //Convenience function useful in GDB. Prints DebugString() to stdout.
```

这些函数以及后面章节将要提到的I/O函数实现了Message 的接口，它们被所有C++ protocol buffer类共享。更多信息，请查看文章[complete API documentation for Message](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.message#Message)。

**关于解析&序列化(Parsing and Serialization)。**

最后，每一个protocol buffer类都有读写你所选择的消息类型的函数。它们包括：

```cpp
bool SerializeToString(string* output) const; //将消息序列化并储存在指定的string中。注意里面的内容是二进制的，而不是文本；我们只是使用string作为一个很方便的容器。
 
bool ParseFromString(const string& data); //从给定的string解析消息。
 
bool SerializeToArray(void * data, int size) const  //将消息序列化至数组
 
bool ParseFromArray(const void * data, int size)    //从数组解析消息
 
bool SerializeToOstream(ostream* output) const; //将消息写入到给定的C++ ostream中。
 
bool ParseFromIstream(istream* input); //从给定的C++ istream解析消息。
```

这些函数只是用于解析和序列化的几个函数罢了。请再次参考[Message API reference](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.message#Message)以查看完整的函数列表。

**注意：** 
protocol buffers和面向对象的设计 protocol buffer类通常只是纯粹的数据存储器（就像C++中的结构体一样）；它们在对象模型中并不是一等公民。如果你想向生成的类中添加更丰富的行为，最好的方法就是在应用程序中对它进行封装。如果你无权控制.proto文件的设计的话，封装protocol buffers也是一个好主意（例如，你从另一个项目中重用一个.proto文件）。在那种情况下，你可以用封装类来设计接口，以更好地适应你的应用程序的特定环境：隐藏一些数据和方法，暴露一些便于使用的函数，等等。但是你绝对不要通过继承生成的类来添加行为。这样做的话，会破坏其内部机制，并且不是一个好的面向对象的实践。

## 使用Protocol Buffer来读写消息

下面让我们尝试使用protobuf为我们产生的消息类来进行序列化和反序列的操作。你想让你的Student程序完成的第一件事情就是向Student消息类对象进行 赋值，并且进行序列化操作。然后在从序列化结果进行反序列话操作，解析我们需要的字段信息。具体参考如下示例代码：

```cpp
//test.cpp
 
#include <iostream>
#include <string>
#include "student.pb.h"
using namespace std;
 
int main(int argc, char* argv[]){
    GOOGLE_PROTOBUF_VERIFY_VERSION;
 
    tutorial::Student student;
 
    //给消息类Student对象student赋值
    student.set_id(201421031059);
    *student.mutable_name()="dablelv";
    student.set_email("dablelv@tencent.com");
    //增加一个号码对象
    tutorial::Student::PhoneNumber* phone_number = student.add_phone();
    phone_number->set_number("15813354925");
    phone_number->set_type(tutorial::Student::MOBILE);
 
    //再增加一个号码对象
    tutorial::Student::PhoneNumber* phone_number1 = student.add_phone();
    phone_number1->set_number("0564-4762652");
    phone_number1->set_type(tutorial::Student::HOME);
 
    //对消息对象student序列化到string容器
    string serializedStr;
    student.SerializeToString(&serializedStr);
    cout<<"serialization result:"<<serializedStr<<endl; //序列化后的字符串内容是二进制内容，非可打印字符，预计输出乱码
    cout<<endl<<"debugString:"<<student.DebugString();
 
/*----------------上面是序列化，下面是反序列化-----------------------*/
    //解析序列化后的消息对象，即反序列化
    tutorial::Student deserializedStudent;
    if(!deserializedStudent.ParseFromString(serializedStr)){
      cerr << "Failed to parse student." << endl;
      return -1;
    }
 
    cout<<"-------------上面是序列化，下面是反序列化---------------"<<endl;
    //打印解析后的student消息对象 
    cout<<"deserializedStudent debugString:"<<deserializedStudent.DebugString();
    cout <<endl<<"Student ID: " << deserializedStudent.id() << endl;
    cout <<"Name: " << deserializedStudent.name() << endl;
    if (deserializedStudent.has_email()){
        cout << "E-mail address: " << deserializedStudent.email() << endl;
    }
    for (int j = 0; j < deserializedStudent.phone_size(); j++){
        const tutorial::Student::PhoneNumber& phone_number = deserializedStudent.phone(j);
 
        switch (phone_number.type()) {
            case tutorial::Student::MOBILE:
            cout << "Mobile phone #: ";
            break;
            case tutorial::Student::HOME:
                cout << "Home phone #: ";
            break;
        }
        cout <<phone_number.number()<<endl;
    }
 
    google::protobuf::ShutdownProtobufLibrary();
}
```

## 编译程序

编译上面的测试程序，可使用如下命令：


> g++ -o protobufTest.out `-lprotobuf` test.cpp student.pb.cc


编译成功后，运行protobufTest.out程序，可能会报如下错误：

```
error while loading shared libraries: libprotobuf.so.9: cannot open shared object file: No such file or directory
```

原因是protobuf的连接库默认安装路径是/usr/local/lib，而/usr/local/lib 不在常见Linux系统的LD_LIBRARY_PATH链接库路径这个环境变量里，所以就找不到该lib。LD_LIBRARY_PATH是Linux环境变量名，该环境变量主要用于指定查找共享库（动态链接库）。所以，解决办法就是修改环境变量LD_LIBRARY_PATH的值。 
**方法一：** 
使用export命令临时修改LD_LIBRARY_PATH，只对当前shell会话有效：

```
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
```

**方法二：** 
或者永久修改，在~/目录下打开.bash_profile文件，设置环境变量如下：

```
LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH  export LD_LIBRARY_PATH  
```

注意Linux下点号开头的文件都是隐藏文件，使用`ls -a` 可查看指定目录下的所有文件，包括隐藏文件。

**方法三：** 
永久有效的话，可以创建protobuf的动态连接库配置文件/etc/ld.so.conf.d/libprotobuf.conf并包含如下内容：

```
/usr/local/lib 
```

然后运行动态链接库的管理命令ldconfig。

```
sudo ldconfig
```

ldconfig通常在系统启动时运行,而当用户安装了一个新的动态链接库时,就需要手工运行这个命令。

测试程序输出结果： 
![这里写图片描述](20160717164452471)

## 扩展一个protocol buffer（Extending a Protocol Buffer）

无论或早或晚，在你放出你那使用protocol buffer的代码之后，你必定会想“改进“protocol buffer的定义，即我们自定义定义消息的proto文件。如果你想让你的新buffer向后兼容（backwards-compatible），并且旧的buffer能够向前兼容（forward-compatible），你一定希望如此，那么你在新的protocol buffer中就要遵守其他的一些规则了： 
（1）对已存在的任何字段，你都不能更改其标识（tag）号。

（2）你绝对不能添加或删除任何required的字段。

（3）你可以添加新的optional或repeated的字段，但是你必须使用新的标识（tag）号（例如，在这个protocol buffer中从未使用过的标识号——甚至于已经被删除过的字段使用过的标识号也不行）。

（有一些例外情况，但是它们很少使用。）

如果你遵守这些规则，老的代码将能很好地解析新的消息（message），并忽略掉任何新的字段。对老代码来说，已经被删除的optional字段将被赋予默认值，已被删除的repeated字段将是空的。新的代码也能够透明地读取旧的消息。但是，请牢记心中：新的optional字段将不会出现在旧的消息中，所以你要么需要显式地检查它们是否由has_前缀的函数置（set）了值，要么在你的.proto文件中，在标识（tag）号的后面用[default = value]提供一个合理的默认值。如果没有为一个optional项指定默认值，那么就会使用与特定类型相关的默认值：对string来说，默认值是空字符串。对boolean来说，默认值是false。对数值类型来说，默认值是0。还要注意：如果你添加了一个新的repeated字段，你的新代码将无法告诉你它是否被留空了（被新代码），或者是否从未被置（set）值（被旧代码），这是因为它没有has_标志。

## 优化小技巧（Optimization Tips）

Protocol Buffer 的C++库已经做了极度优化。但是，正确的使用方法仍然会提高很多性能。下面是一些小技巧，用来提升protocol buffer库的最后一丝速度能力： 
（1）如果有可能，重复利用消息（message）对象。即使被清除掉，消息（message）对象也会尽量保存所有被分配来重用的内存。这样的话，如果你正在处理很多类型相同的消息以及一系列相似的结构，有一个好办法就是重复使用同一个消息（message）对象，从而使内存分配的压力减小一些。然而，随着时间的流逝，对象占用的内存也有可能变得越来越大，尤其是当你的消息尺寸（译者注：各消息内容不同，有些消息内容多一些，有些消息内容少一些）不同的时候，或者你偶尔创建了一个比平常大很多的消息（message）的时候。你应该自己监测消息（message）对象的大小——通过调用SpaceUsed函数——并在它太大的时候删除它。

（2）在多线程中分配大量小对象的内存的时候，你的操作系统的内存分配器可能优化得不够好。在这种情况下，你可以尝试用一下Google’s tcmalloc。

## 高级使用（Advanced Usage）

Protocol Buffers的作用绝不仅仅是简单的数据存取以及序列化。请阅读[C++ API reference](https://developers.google.com/protocol-buffers/docs/reference/cpp/)全文来看看你还能用它来做什么。

protocol消息类所提供的一个关键特性就是反射。你不需要编写针对一个特殊的消息（message）类型的代码，就可以遍历一个消息的字段，并操纵它们的值，就像XML和JSON一样。“反射”的一个更高级的用法可能就是可以找出两个相同类型的消息之间的区别，或者开发某种“协议消息的正则表达式”，利用正则表达式，你可以对某种消息内容进行匹配。只要你发挥你的想像力，就有可能将Protocol Buffers应用到一个更广泛的、你可能一开始就期望解决的问题范围上。

“反射”是由Message::Reflection interface提供的。

### Import Message**

在一个 .proto 文件中，还可以用 Import 关键字引入在其他 .proto 文件中定义的消息，这可以称做 Import Message，或者 Dependency Message。

比如下例：

清单 5. 代码

```
import common.header; 
 
message youMsg{ 
 required common.info_header header = 1; 
 required string youPrivateData = 2; 
}

```

其中 ,`common.info_header`定义在`common.header`包内。

Import Message 的用处主要在于提供了方便的代码管理机制，类似 C 语言中的头文件。您可以将一些公用的 Message 定义在一个 package 中，然后在别的 .proto 文件中引入该 package，进而使用其中的消息定义。

Google Protocol Buffer 可以很好地支持嵌套 Message 和引入 Message，从而让定义复杂的数据结构的工作变得非常轻松愉快。



**嵌套 Message**

嵌套是一个神奇的概念，一旦拥有嵌套能力，消息的表达能力就会非常强大。

代码清单 4 给出一个嵌套 Message 的例子。

清单 4. 嵌套 Message 的例子

```
message Person { 
 required string name = 1; 
 required int32 id = 2;        // Unique ID number for this person. 
 optional string email = 3; 
 
 enum PhoneType { 
   MOBILE = 0; 
   HOME = 1; 
   WORK = 2; 
 } 
 
 message PhoneNumber { 
   required string number = 1; 
   optional PhoneType type = 2 [default = HOME]; 
 } 
 repeated PhoneNumber phone = 4; 
}
```

在 Message Person 中，定义了嵌套消息 PhoneNumber，并用来定义 Person 消息中的 phone 域。这使得人们可以定义更加复杂的数据结构。



# 在Python中使用



首先在say_hi.proto文件中定义一个需要在代码中传递的数据结构：

```
syntax = "proto2";

package hello_word;

message SayHi {
    required int32 id = 1;
    required string something = 2;
    optional string extra_info = 3;
}
```



然后使用命令

```
protoc -I . --python_out=. hello_world.say_hi.proto
```

在当前路径下面生成一个say_hi_pb2.py文件。

-I 是指定.proto文件所在路径。

--python_out 输出生成好的pb2.py文件所在路径。

后面参数指定使用哪个.proto文件。

 

之后我们就可以愉快的使用这个生成好的文件的类进行数据序列化反序列化了。使用例子如下：



```
# coding: utf-8
import say_hi_pb2

po = say_hi_pb2.SayHi()
po.id = 123
po.something = 'do_something'
po.extra_info = 'xiba'

bilibili = po.SerializeToString()

oo = say_hi_pb2.SayHi()
oo.ParseFromString(bilibili)
print oo.id
print oo.something
print oo.extra_info
输出：
123
do_something
xiba
```