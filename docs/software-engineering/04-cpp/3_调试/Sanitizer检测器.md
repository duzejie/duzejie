

C/C++ 提供了直接操作内存的强大能力，然而使用不当也会招致许多问题，下面是 Google chromium 团队的一组统计数据: 

>"Around 70% of our high severity security bugs are memory unsafety problems (that is, mistakes with C/C++ pointers). Half of those are use-after-free bugs."


 Sanitizer 工具来帮助工程师快速排查与定位内存问题，提高生产力，它包括了[[AddressSanitizer]]、[[MemorySanitizer]]、[[ThreadSanitizer]]、[[LeakSanitizer]]等多种工具。这些工具最初是LLVM项目的一部分，后来也被GNU的GCC编译器支持。从GCC的4.8版本开始，就已经支持AddressSanitizer和ThreadSanitizer，而4.9版本则开始支持LeakSanitizer。
链接：https://github.com/google/sanitizers/wiki/




Google的sanitizers一共有5种：

- [[AddressSanitizer]] (检查寻址问题) ：包含LeakSanitizer (检查内存泄漏问题)
- ThreadSanitizer：检查数据竞争和死锁问题（支持C++和Go）
- MemorySanitizer：检查使用未初始化的内存问题
- HWASAN（Hardware-assisted AddressSanitizer）：AddressSanitizer的变种，相比AddressSanitizer消耗的内存更少。
- UBSan：检查使用语言的UndefinedBehavior问题。


# [GCC Instrumentation Options ](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html)
GCC supports a number of command-line options that control adding run-time instrumentation to the code it normally generates. For example, one purpose of instrumentation is collect profiling statistics for use in finding program hot spots, code coverage analysis, or profile-guided optimizations. Another class of program instrumentation is adding run-time checking to detect programming errors like invalid pointer dereferences or out-of-bounds array accesses, as well as deliberately hostile attacks such as stack smashing or C++ vtable hijacking. There is also a general hook which can be used to implement other forms of tracing or function-level instrumentation for debug or program analysis purposes.

### GCC编译器的检测功能

`-fsanitize=address`[](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html#index-fsanitize_003daddress)

Enable AddressSanitizer, a fast memory error detector. Memory access instructions are instrumented to detect out-of-bounds and use-after-free bugs. The option enables -fsanitize-address-use-after-scope. See [https://github.com/google/sanitizers/wiki/AddressSanitizer](https://github.com/google/sanitizers/wiki/AddressSanitizer) for more details. The run-time behavior can be influenced using the `ASAN_OPTIONS` environment variable. When set to `help=1`, the available options are shown at startup of the instrumented program. See [https://github.com/google/sanitizers/wiki/AddressSanitizerFlags#run-time-flags](https://github.com/google/sanitizers/wiki/AddressSanitizerFlags#run-time-flags) for a list of supported options. The option cannot be combined with -fsanitize=thread or -fsanitize=hwaddress. Note that the only target -fsanitize=hwaddress is currently supported on is AArch64.

To get more accurate stack traces, it is possible to use options such as -O0, -O1, or -Og (which, for instance, prevent most function inlining), -fno-optimize-sibling-calls (which prevents optimizing sibling and tail recursive calls; this option is implicit for -O0, -O1, or -Og), or -fno-ipa-icf (which disables Identical Code Folding for functions). Since multiple runs of the program may yield backtraces with different addresses due to ASLR (Address Space Layout Randomization), it may be desirable to turn ASLR off. On Linux, this can be achieved with ‘setarch `uname -m` -R ./prog’.


`-fsanitize=hwaddress`[](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html#index-fsanitize_003dhwaddress)

Enable Hardware-assisted AddressSanitizer, which uses a hardware ability to ignore the top byte of a pointer to allow the detection of memory errors with a low memory overhead. Memory access instructions are instrumented to detect out-of-bounds and use-after-free bugs. The option enables -fsanitize-address-use-after-scope. See [https://clang.llvm.org/docs/HardwareAssistedAddressSanitizerDesign.html](https://clang.llvm.org/docs/HardwareAssistedAddressSanitizerDesign.html) for more details. The run-time behavior can be influenced using the `HWASAN_OPTIONS` environment variable. When set to `help=1`, the available options are shown at startup of the instrumented program. The option cannot be combined with -fsanitize=thread or -fsanitize=address, and is currently only available on AArch64.


`-fsanitize=shadow-call-stack`[](https://gcc.gnu.org/onlinedocs/gcc/Instrumentation-Options.html#index-fsanitize_003dshadow-call-stack)

Enable ShadowCallStack, a security enhancement mechanism used to protect programs against return address overwrites (e.g. stack buffer overflows.) It works by saving a function’s return address to a separately allocated shadow call stack in the function prologue and restoring the return address from the shadow call stack in the function epilogue. Instrumentation only occurs in functions that need to save the return address to the stack.

Currently it only supports the aarch64 platform. It is specifically designed for linux kernels that enable the CONFIG_SHADOW_CALL_STACK option. For the user space programs, runtime support is not currently provided in libc and libgcc. Users who want to use this feature in user space need to provide their own support for the runtime. It should be noted that this may cause the ABI rules to be broken.

On aarch64, the instrumentation makes use of the platform register `x18`. This generally means that any code that may run on the same thread as code compiled with ShadowCallStack must be compiled with the flag -ffixed-x18, otherwise functions compiled without -ffixed-x18 might clobber `x18` and so corrupt the shadow stack pointer.

Also, because there is no userspace runtime support, code compiled with ShadowCallStack cannot use exception handling. Use -fno-exceptions to turn off exceptions.

See [https://clang.llvm.org/docs/ShadowCallStack.html](https://clang.llvm.org/docs/ShadowCallStack.html) for more details.

`-fsanitize=thread`

Enable ThreadSanitizer, a fast data race detector. Memory access instructions are instrumented to detect data race bugs. See [https://github.com/google/sanitizers/wiki#threadsanitizer](https://github.com/google/sanitizers/wiki#threadsanitizer) for more details. The run-time behavior can be influenced using the `TSAN_OPTIONS` environment variable; see [https://github.com/google/sanitizers/wiki/ThreadSanitizerFlags](https://github.com/google/sanitizers/wiki/ThreadSanitizerFlags) for a list of supported options. The option cannot be combined with -fsanitize=address, -fsanitize=leak.

Note that sanitized atomic builtins cannot throw exceptions when operating on invalid memory addresses with non-call exceptions (-fnon-call-exceptions).

`-fsanitize=leak`

Enable LeakSanitizer, a memory leak detector. This option only matters for linking of executables. The executable is linked against a library that overrides `malloc` and other allocator functions. See [https://github.com/google/sanitizers/wiki/AddressSanitizerLeakSanitizer](https://github.com/google/sanitizers/wiki/AddressSanitizerLeakSanitizer) for more details. The run-time behavior can be influenced using the `LSAN_OPTIONS` environment variable. The option cannot be combined with -fsanitize=thread.

`-fsanitize=undefined`

Enable UndefinedBehaviorSanitizer, a fast undefined behavior detector. Various computations are instrumented to detect undefined behavior at runtime. See [https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html) for more details. The run-time behavior can be influenced using the `UBSAN_OPTIONS` environment variable. 
`-fno-sanitize=all`

This option disables all previously enabled sanitizers. -fsanitize=all is not allowed, as some sanitizers cannot be used together.


`-fsanitize-recover[=opts]`

-fsanitize-recover= controls error recovery mode for sanitizers mentioned in comma-separated list of opts. Enabling this option for a sanitizer component causes it to attempt to continue running the program as if no error happened. This means multiple runtime errors can be reported in a single program run, and the exit code of the program may indicate success even when errors have been reported. The -fno-sanitize-recover= option can be used to alter this behavior: only the first detected error is reported and program then exits with a non-zero exit code.

Currently this feature only works for -fsanitize=undefined (and its suboptions except for -fsanitize=unreachable and -fsanitize=return), -fsanitize=float-cast-overflow, -fsanitize=float-divide-by-zero, -fsanitize=bounds-strict, -fsanitize=kernel-address and -fsanitize=address. For these sanitizers error recovery is turned on by default, except -fsanitize=address, for which this feature is experimental. -fsanitize-recover=all and -fno-sanitize-recover=all is also accepted, the former enables recovery for all sanitizers that support it, the latter disables recovery for all sanitizers that support it.

Even if a recovery mode is turned on the compiler side, it needs to be also enabled on the runtime library side, otherwise the failures are still fatal. The runtime library defaults to `halt_on_error=0` for ThreadSanitizer and UndefinedBehaviorSanitizer, while default value for AddressSanitizer is `halt_on_error=1`. This can be overridden through setting the `halt_on_error` flag in the corresponding environment variable.

Syntax without an explicit opts parameter is deprecated. It is equivalent to specifying an opts list of:

undefined,float-cast-overflow,float-divide-by-zero,bounds-strict

`-fsanitize-address-use-after-scope`

Enable sanitization of local variables to detect use-after-scope bugs. The option sets -fstack-reuse to ‘none’.

`-fsanitize-trap[=opts]`

The -fsanitize-trap= option instructs the compiler to report for sanitizers mentioned in comma-separated list of opts undefined behavior using `__builtin_trap` rather than a `libubsan` library routine. If this option is enabled for certain sanitizer, it takes precedence over the -fsanitizer-recover= for that sanitizer, `__builtin_trap` will be emitted and be fatal regardless of whether recovery is enabled or disabled using -fsanitize-recover=.

The advantage of this is that the `libubsan` library is not needed and is not linked in, so this is usable even in freestanding environments.

Currently this feature works with -fsanitize=undefined (and its suboptions except for -fsanitize=vptr), -fsanitize=float-cast-overflow, -fsanitize=float-divide-by-zero and -fsanitize=bounds-strict. `-fsanitize-trap=all` can be also specified, which enables it for `undefined` suboptions, -fsanitize=float-cast-overflow, -fsanitize=float-divide-by-zero and -fsanitize=bounds-strict. If `-fsanitize-trap=undefined` or `-fsanitize-trap=all` is used and `-fsanitize=vptr` is enabled on the command line, the instrumentation is silently ignored as the instrumentation always needs `libubsan` support, -fsanitize-trap=vptr is not allowed.

`-fsanitize-undefined-trap-on-error`

The -fsanitize-undefined-trap-on-error option is deprecated equivalent of -fsanitize-trap=all.


