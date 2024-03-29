### 2019-08-06 使用Ollydb调试--断点分类

#### Software Breakpoint

软件断点指的是:当使用Ollydb设置断点时，指定断点处的指令的最后一个字节被替换为软件断点0xCC,也就是常说的**int3**断点指令。这个过程是Ollydb自动替换的。当CPU执行时遇到了0xCC指令，就出发中断异常，然后经过一些列的操作将控制权给到了调试器。

如果需要恢复中断，只需要将中断处被替换为0xCC的指令替换为原来的字节指令即可。


设置软件断点：通常我们可以指定了要设置断点的地址，然后双击或者使用**快捷键F2**或者在Ollydb的汇编代码窗口右键-->找到Breakpoint-->Trigger即可。


查看断点列表：查看断点列表可以在Ollydb的状态栏找到B或者Br即可看到设置的断点列表。

常用的调试手段。


#### Hardware Breakpoint
硬件断点：是指使用CPU的调试寄存器设置断点。Windows提供了R0-R78个寄存器，但是提供可用的只有四个寄存器。
设置方式：和设置软件断点一样，在想要设置断点的地址栏右键选择 **"Breakpoint-->Harware,onExecute"**进行设置
查看断点列表：查看列表通过顶部的菜单栏，Debug-->Hardware Breakpoints 即可看到设置的硬件断点。

常用：在一些**应用被加壳或者保护等机制时**，可使用这个断点进行设置断点调试。


#### Memory Breakpoint
内存断点：是指当需要在内存找寻某些值，例如字符串等时，可以通过在内存的窗口指定地址设置内存断点，如果程序访问了当前内存即可触发中断。
设置方式：
1、指令窗口设置，找到要设置的指令行，选择顶部的菜单栏Breakpoint-->Memory即可设置。
2、在memory dump窗口，选择16进制数据的一个字节或多个字节，然后使用1)的方式设置
3、在菜单的bar内找到M(memory窗口)，在弹出的memory的窗口内，选择要设置的地址，右键选择**Set Memory breakpoint on access**即可设置。




#### 总结
软件断点：
软件断点是通过指令的最后一个字节为0xCC来出发断点异常获取程序的控制权限，常用的调试断点；

硬件断点：
通过CPU内置的调试寄存设置断点，有些软件经过了加密保护等操作时，常用这个方式设置断点


内存断点：
通过设置内存访问的断点来达到目的，通常为了监控某个内存是否被访问或者修改等。