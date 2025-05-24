# 2 Introduction

- [What is SWIG?](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn2)
- [Why use SWIG?](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn3)
- [Target languages](https://www.swig.org/Doc4.2/Introduction.html#Introduction_target_languages)
    - [Supported status](https://www.swig.org/Doc4.2/Introduction.html#Introduction_supported_status)
    - [Experimental status](https://www.swig.org/Doc4.2/Introduction.html#Introduction_experimental_status)
- [A SWIG example](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn4)
    - [SWIG interface file](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn5)
    - [The swig command](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn6)
    - [Building a Perl5 module](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn7)
    - [Building a Python module](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn8)
    - [Shortcuts](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn9)
- [Supported C/C++ language features](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn10)
- [Non-intrusive interface building](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn11)
- [Incorporating SWIG into a build system](https://www.swig.org/Doc4.2/Introduction.html#Introduction_build_system)
- [Hands off code generation](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn12)
- [SWIG and freedom](https://www.swig.org/Doc4.2/Introduction.html#Introduction_nn13)

## 2.1 What is SWIG?

[SWIG](https://www.swig.org/) is a software development tool that simplifies the task of interfacing different languages to C and C++ programs. In a nutshell, SWIG is a compiler that takes C/C++ declarations and creates the wrappers needed to access those declarations from other languages including Perl, Python, Tcl, Ruby, Guile, and Java. SWIG normally requires no modifications to existing code and can often be used to build a usable interface in only a few minutes. Possible applications of SWIG include:

- Building interpreted interfaces to existing C programs.
- Rapid prototyping and application development.
- Interactive debugging.
- Reengineering or refactoring of legacy software into scripting language components.
- Making a graphical user interface (using Tk for example).
- Testing of C libraries and programs (using scripts).
- Building high performance C modules for scripting languages.
- Making C programming more enjoyable (or tolerable depending on your point of view).
- Impressing your friends.
- Obtaining vast sums of research funding (although obviously not applicable to the author).

SWIG was originally designed to make it extremely easy for scientists and engineers to build extensible scientific software without having to get a degree in software engineering. Because of this, the use of SWIG tends to be somewhat informal and ad-hoc (e.g., SWIG does not require users to provide formal interface specifications as you would find in a dedicated IDL compiler). Although this style of development isn't appropriate for every project, it is particularly well suited to software development in the small; especially the research and development work that is commonly found in scientific and engineering projects. However, nowadays SWIG is known to be used in many large open source and commercial projects.

## 2.2 Why use SWIG?

As stated in the previous section, the primary purpose of SWIG is to simplify the task of integrating C/C++ with other programming languages. However, why would anyone want to do that? To answer that question, it is useful to list a few strengths of C/C++ programming:

- Excellent support for writing programming libraries.
- High performance (number crunching, data processing, graphics, etc.).
- Systems programming and systems integration.
- Large user community and software base.

Next, let's list a few problems with C/C++ programming

- Writing a user interface is rather painful (i.e., consider programming with MFC, X11, GTK, or any number of other libraries).
- Testing is time consuming (the compile/debug cycle).
- Not easy to reconfigure or customize without recompilation.
- Modularization can be tricky.
- Security concerns (buffer overflows for instance).

To address these limitations, many programmers have arrived at the conclusion that it is much easier to use different programming languages for different tasks. For instance, writing a graphical user interface may be significantly easier in a scripting language like Python or Tcl (consider the reasons why millions of programmers have used languages like Visual Basic if you need more proof). An interactive interpreter might also serve as a useful debugging and testing tool. Other languages like Java might greatly simplify the task of writing distributed computing software. The key point is that different programming languages offer different strengths and weaknesses. Moreover, it is extremely unlikely that any programming is ever going to be perfect. Therefore, by combining languages together, you can utilize the best features of each language and greatly simplify certain aspects of software development.

From the standpoint of C/C++, a lot of people use SWIG because they want to break out of the traditional monolithic C programming model which usually results in programs that resemble this:

- A collection of functions and variables that do something useful.
- A main() program that starts everything.
- A horrible collection of hacks that form some kind of user interface (but which no-one really wants to touch).

Instead of going down that route, incorporating C/C++ into a higher level language often results in a more modular design, less code, better flexibility, and increased programmer productivity.

SWIG tries to make the problem of C/C++ integration as painless as possible. This allows you to focus on the underlying C program and using the high-level language interface, but not the tedious and complex chore of making the two languages talk to each other. At the same time, SWIG recognizes that all applications are different. Therefore, it provides a wide variety of customization features that let you change almost every aspect of the language bindings. This is the main reason why SWIG has such a large user manual ;-).






