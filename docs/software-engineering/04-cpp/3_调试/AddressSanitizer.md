**AddressSanitizer(ASan)** 是一个快速内存检测器，可以检测出缓冲区溢出、使用已释放内存等问题。编译时带上参数 -fsanitize=address及-g。

[内存检测工具AddressSanitizer - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/37515148)
[工欲善其事必先利其器——AddressSanitizer - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/382994002)



Google sanitizers和其他内存工具的对比

|-|AddressSanitizer|Valgrind/Memcheck|Dr. Memory|Mudflap|Guard Page|gperftools|
|---|---|---|---|---|---|---|
|technology|CTI|DBI|DBI|CTI|Library|Library|
|ARCH|x86, ARM, PPC|x86, ARM, PPC, MIPS, S390X, TILEGX|x86|all(?)|all(?)|all(?)|
|OS|Linux, OS X, Windows, FreeBSD, Android, iOS Simulator|Linux, OS X, Solaris, Android|Windows, Linux|Linux, Mac(?)|All (1)|Linux, Windows|
|Slowdown|2x|20x|10x|2x-40x|?|?|
|Heap OOB|yes|yes|yes|yes|some|some|
|Stack OOB|yes|no|no|some|no|no|
|Global OOB|yes|no|no|?|no|no|
|UAF|yes|yes|yes|yes|yes|yes|
|UAR|yes|no|no|no|no|no |
|UMR|no(MemorySanitizer) |yes|yes|?|no|no|
|Leaks|yes|yes|yes|?|no|no |


DBI: dynamic binary instrumentation  
CTI: compile-time instrumentation  
UMR: uninitialized memory reads  
UAF: use-after-free (aka dangling pointer)  
UAR: use-after-return  
OOB: out-of-bounds  
x86: includes 32- and 64-bit.  
mudflap was removed in GCC 4.9, as it has been superseded by AddressSanitizer.  
Guard Page: a family of memory error detectors (Electric fence or DUMA on Linux, Page Heap on Windows, libgmalloc on OS X)  
gperftools: various performance tools/error detectors bundled with TCMalloc. Heap checker (leak detector) is only available on Linux. Debug allocator provides both guard pages and canary values for more precise detection of OOB writes, so it's better than guard page-only detectors.

# AddressSanitizer

AddressSanitizer（ASAN）可以检查的错误类型：

- Use after free（dangling pointer dereference）：内存释放后继续使用，悬挂指针问题。
- Heap buffer overflow：堆内存溢出
- Stack buffer overflow：栈内存溢出
- Global buffer overflow：全局内存溢出（如全局变量）
- Use after return：局部变量在函数返回后使用
- Use after scope：局部变量在作用范围外使用
- Initialization order bugs：初始化顺序问题
- Memory leaks：内存泄漏

ASAN是一个执行速度非常快的工具，典型的程序在加上ASAN后，执行时间只会增加1倍。  
ASAN工具由一个编译器插桩模块（当前实现为LLVM的一个pass）和一个运行库（替换malloc函数等）组成。

# 错误类型典型案例

## Use after free

```
// RUN: clang -O -g -fsanitize=address %t && ./a.out
int main(int argc, char **argv) {
  int *array = new int[100];
  delete [] array;
  return array[argc];  // BOOM
}
```

## Heap buffer overflow

```
// RUN: clang -O -g -fsanitize=address %t && ./a.out
int main(int argc, char **argv) {
  int *array = new int[100];
  array[0] = 0;
  int res = array[argc + 100];  // BOOM
  delete [] array;
  return res;
}
```

## Stack buffer overflow

```
// RUN: clang -O -g -fsanitize=address %t && ./a.out
int main(int argc, char **argv) {
  int stack_array[100];
  stack_array[1] = 0;
  return stack_array[argc + 100];  // BOOM
}
```

## Global buffer overflow

```
// RUN: clang -O -g -fsanitize=address %t && ./a.out
int global_array[100] = {-1};
int main(int argc, char **argv) {
  return global_array[argc + 100];  // BOOM
}
```

## Use after return

```
// RUN: clang -O -g -fsanitize=address %t && ./a.out
// By default, AddressSanitizer does not try to detect
// stack-use-after-return bugs.
// It may still find such bugs occasionally
// and report them as a hard-to-explain stack-buffer-overflow.

// You need to run the test with ASAN_OPTIONS=detect_stack_use_after_return=1

int *ptr;
__attribute__((noinline))
void FunctionThatEscapesLocalObject() {
  int local[100];
  ptr = &local[0];
}

int main(int argc, char **argv) {
  FunctionThatEscapesLocalObject();
  return ptr[argc];
}
```

## Use after scope

```
// RUN: clang -O -g -fsanitize=address -fsanitize-address-use-after-scope \
//    use-after-scope.cpp -o /tmp/use-after-scope
// RUN: /tmp/use-after-scope

// Check can be disabled in run-time:
// RUN: ASAN_OPTIONS=detect_stack_use_after_scope=0 /tmp/use-after-scope

volatile int *p = 0;

int main() {
  {
    int x = 0;
    p = &x;
  }
  *p = 5;
  return 0;
}
```

## Initialization order bugs

```
$ cat tmp/init-order/example/a.cc
#include <stdio.h>
extern int extern_global;
int __attribute__((noinline)) read_extern_global() {
  return extern_global;
}
int x = read_extern_global() + 1;
int main() {
  printf("%d\n", x);
  return 0;
}

$ cat tmp/init-order/example/b.cc
int foo() { return 42; }
int extern_global = foo();
```

由于x和x依赖的extern_global处于不同的编译单元，所以x的初值依赖编译单元的初始化执行顺序。

## Memory leaks

```
$ cat memory-leak.c 
#include <stdlib.h>

void *p;

int main() {
  p = malloc(7);
  p = 0; // The memory is leaked here.
  return 0;
}
$ clang -fsanitize=address -g memory-leak.c
$ ./a.out 
=================================================================
==7829==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 7 byte(s) in 1 object(s) allocated from:
    #0 0x42c0c5 in __interceptor_malloc /usr/home/hacker/llvm/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:74
    #1 0x43ef81 in main /usr/home/hacker/memory-leak.c:6
    #2 0x7fef044b876c in __libc_start_main /build/buildd/eglibc-2.15/csu/libc-start.c:226

SUMMARY: AddressSanitizer: 7 byte(s) leaked in 1 allocation(s).
```

