---
aliases:
---

**ThreadSanitizer(TSan)** 是一个数据竞争检测器，可以用来分析线程竞态、死锁等线程相关问题。使用 `TSan`, 只需在编译时使用 `-fsanitize=thread` 即可，可以添加 `-O2` 要获得合理的性能，同时使用 `-g` 获取警告消息中的文件名和行号。

**运行开销**    竞争检测的成本因程序而异，但对于典型的程序，内存使用量可能会增加 `5-10` 倍，执行时间会增加 `2-20` 倍。


tsan中的事件可以分为两大类：_内存访问_（_memory access_）事件和_同步_（_synchronization_）事件。


[Popular Data Races ](https://github.com/google/sanitizers/wiki/ThreadSanitizerPopularDataRaces)
[ThreadSanitizer WiKi DetectableBugs](https://github.com/google/sanitizers/wiki/ThreadSanitizerDetectableBugs)

[Google ThreadSanitizer -- 排查多线程问题data race的大杀器 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/139000777)
[ThreadSanitizerCppManual ](https://github.com/google/sanitizers/wiki/ThreadSanitizerCppManual)

[ThreadSanitizer — Clang 12 documentation (llvm.org)](https://releases.llvm.org/12.0.1/tools/clang/docs/ThreadSanitizer.html)

Ref:
[sanitizers Wiki](https://github.com/google/sanitizers/wiki)
[ThreadSanitizer——跟data race说再见 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/38687826)


### 关键要点

1. 除了加-fsanitize=thread外，一定要加-fPIE -pie。
2. -g 是为了能显示文件名和行号。
3. 如果分生成obj(-c)和link两个步骤，每一步都加：thread -fPIE -pie -g，并且在link的时候加-ltsan
4. 只支持64位，最好指定编译64位(-m64)
5. 如果依赖其他静态库，其他静态库编译时必须指定-fPIC（如果不是请重编）

## 其它工具
[[docs/software-engineering/04-cpp/3_调试/cpp-debug工具]]





