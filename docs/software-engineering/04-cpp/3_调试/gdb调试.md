常用命令：

[GDB调试 断点 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/508126446)

## GDB Dynamic Printf
`dprintf location,format string,arg1,arg2,...`   
dprintf命令和C语言中的printf的用法很相似，支持格式化打印。
```text
dprintf 6,"Hello, World!\n"
dprintf 11,"i = %d, a = %d, b = %d\n",i,a,b
dprintf 14,"Leaving! Bye bye!\n"
```


