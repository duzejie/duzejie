
使用gcc编译代码时，有时候由于种种原因，必需要使用一个函数名，而该函数名又在依赖的库中进行了定义，一般有如下两种情况，也对应了不同的解决办法：

- 如果该函数在库中被修饰为弱函数，我们可以直接定义一个参数和返回值都相同的函数，重写该函数即可，这种情况不会报重复定义的错误（比如fputc、\_write函数等）。
- 如果该函数不是弱函数，则需要使用`-Wl,--wrap`参数编译，并定义一个包装函数，来达到这一目的。

网上对包装函数的解释如下:

> 对symbol使用包装函数(wrapper function)，任何对symbol未定义的引用(undefined reference)会被解析成__wrap_symbol，而任何对__real_symbol未定义的引用会被解析成symbol。即当一个名为symbol符号使用wrap功能时，工程中任何用到symbol符号的地方实际使用的是__wrap_symbol符号，任何用到__real_symbol的地方实际使用的是真正的symbol。
> 
> 注意：**当__wrap_symbol是使用C++实现时，一定要加上extern “C”**，否则将会出现”undefined reference to __wrap_symbol”。


GCC的--wrap是一个链接器选项，假如我们要编译的源文件为main.c，编译命令如下：

`gcc main.c -Wl,--wrap=memset -o main`


- `-Wl`：此选项用于将后面的参数传递给链接器。GCC 作为一个编译套件，涵盖了预处理器、编译器、汇编器和链接器等多个工具，`-Wl` 能让你在使用 GCC 时向链接器传递特定参数。
- `--wrap=memset`：这是传递给链接器的参数，它的作用是让链接器对 `memset` 函数进行包装（wrap）。具体来说，当代码里调用 `memset` 函数时，实际上会调用名为 `__wrap_memset` 的函数；而当 `__wrap_memset` 函数里调用 `__real_memset` 时，才会调用真正的 `memset` 函数。

如在适配内存管理函数，采用包装函数时使用的的编译参数，在此做个备忘：

```c
"-Wl,--wrap,_free_r",
"-Wl,--wrap,_malloc_usable_size_r",
"-Wl,--wrap,_malloc_r",
"-Wl,--wrap,_memalign_r",
"-Wl,--wrap,_realloc_r",
"-Wl,--wrap,_calloc_r",
```
(当要替换libc功能时，建议放在libc前，其它lib后)


