
[[docs/software-engineering/04-cpp/3_调试/AddressSanitizer]]



`Helgrind` 是 `Valgrind` 工具集中的一个检测 **数据争用** 的工具。功能更强大，性能查些。
`AddressSanitizer` 
1. [Dr.Memory](https://link.zhihu.com/?target=https%3A//drmemory.org/) 检测未初始化的内存访问、double free、use after free 等错误
2. [Mudflap](https://link.zhihu.com/?target=https%3A//gcc.gnu.org/wiki/Mudflap_Pointer_Debugging) 检测指针的解引用，**静态插桩**
3. [Insure++](https://link.zhihu.com/?target=https%3A//www.parasoft.com/products/parasoft-insure/) 检测内存泄漏
4. [Valgrind](https://link.zhihu.com/?target=https%3A//valgrind.org/) 可以检测非常多的内存错误

但是无一例外，Dr.Memory、Insure++ 和 Mudflap 虽然在运行时造成的额外损耗比较少，但是检测场景有限；Valgrind 虽然能够在许多场景的检测出错误，但是它实现了自己的一套 ISA 并在其之上运行目标程序，因此它会严重拖慢目标程序的速度。而 ASan 在设计时就综合考虑了检测场景、速度的影响因素，结合了 Mudflap 的静态插桩、Valgrind 的多场景检测。ASan 由两部分组成：一个是静态插桩模块，将内存访问判断的逻辑直接插入在了二进制中，保证了检测逻辑的执行速度；另一部分则是运行时库，提供部分功能的开启、报错函数和 `malloc`/`free`/`memcpy` 等函数的 asan 检测版本。




`sanitizers` 是一种集成于编译器中，用于调试 `C/C++` 代码的工具，通过在编译过程中插入检查代码来检查代码运行时出现的内存访问越界、未定义行为等错误。

它分为以下几种：

-   AddressSanitizer[^address-sanitizer]：检测对堆、栈、全局变量的越界访问，无效的释放内存、内存泄漏（实验性）。
-   ThreadSanitizer[^thread-sanitizer]：检测多线程的数据竞争。
-   MemorySanitizer[^memory-sanitizer]：检测对未初始化内存的读取。
-   UndefinedBehaviorSanitizer[^ub-san]：检测未定义行为。