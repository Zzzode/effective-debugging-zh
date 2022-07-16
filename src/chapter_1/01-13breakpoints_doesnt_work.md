# 断点不工作
如果程序不如预期那样在预先设置的断点停止，你可能想要检查下面的列表来保证断点被设置正确和准确。

- 源代码文件可能在二进制生成以后发生了改变。调试符号包含的源代码文件路径是在二进制构建的时候的。但是它没有源代码的实际内容。除非用户指定另外的源码搜索路径，调试器从调试符号里面的路径来加载源代码文件。调试器会发出一个警告信息如果源码文件的时间戳比二进制创建的时间戳更新。如果警告被忽略了，这并非是不常见由于信息被一行又一行的其他信息埋没了，调试器捡到的源代码行与调试符号里面的源代码行。当调试器被指定要在某个行设置断点时，他可能实际把陷入指令插入了不同的行。

- 如果一个断点将要设置在一个共享库，直到库被映射到被调试的进程的地址空间，调试器不能够插入陷入指令。如果你希望调试库的初始代码，这是困难的，因为当我们有机会设置断点的时候，通常有点晚了。比如，当函数调用`dlopen`或者`LoadLibrary`把库被动态加载以后，初始代码已经完成了执行。幸运的是，调试器如gdb可以延迟断点设置到当库文件被加载到进程。当debugee加载一个库和在任何库代码被执行的时候，内核会发送一个事件到调试器。这给了调试器一个机会去检查它的延迟断点和适当地设置它们。Windows Visual Studio在项目设置对话框的调试页面同样支持额外的DLLs，这允许用户设置断点到将被加载的DLLs。

•	Compiler may shuffle source code back and forth if optimization is enabled. As a result, the debugger may not be able to set the breakpoint at the source line exactly as the user wishes. In this case, it is better to set breakpoint at function or instruction level so that the breakpoint will be fired reliably (see Chapter 5 for more detail).
- 如果优化被打开，编译器可能来来回回调整源代码。结果是，调试器不能够在用户希望设置的源代码行设置断点。在这个情形下，在函数或者指令级别设置断点会更好，借此断点可以可靠地被设置（见第五章更多的细节）。