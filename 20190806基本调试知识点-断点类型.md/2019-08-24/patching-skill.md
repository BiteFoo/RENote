## Windows 逆向分析--常用的四个patched程序的方法

### LAME
LAME : Localized Assembly Manipulation and Enhancing 
按照语义：手动修改汇编指令完成patched程序。
在分析一些程序的时候会遇到反调试，检测输入是否满足条件等指令，如果不满足，则进入失败流程，可以通过NOP指令或者修改跳转指令来完成程序的patched.
例如，在遇到了程序在调用比较指令后，根据状态标志寄存器ELFAGS的BF或者ZF等判断是否进入错误流程。

```
cmp eax,data;
jnb failed; 
call succ;
```
其中failed实现跳转，需要BF=0；此时我们可以手动修改指令为jb failed,也就是如果BF=1才会执行,此时程序会进入到call succ。

这种方式需要在每个调用了check的地方来手动patched，如果有100个，确实很需要时间和精力。


### NOOB

NOOB:Not Only Obvious Breakpoints
语义：不只是在断点处做patch ，深入断点内的check分析，在核心位置patch是最好的方式，算是比LAME高级一点。
有时候逆向分析的时候发现check部分存在多个不同模块调用，但是如果手动去一个一个patch调用，显得比较繁琐也会很累，因此在关键点下断点，分析输入和比较的值，在check内部做patch掉，那么其他调用了check的位置就能正常绕过一些检测。

**注** 有时候check可能是其他函数调用检测值得，因此简单的在内部执行patch函数check也不一定是最好的方式。

### SKILLED

SKILLED: Some Knowledge In Lower Level Enginneered Data.

语义：关于逆向低级语言的数据知识。例如Byte,word double-word quarad-word等。
这个需要在熟练使用了NOOB的情况下，进一步分析程序的内存数据，因为有时候程序的只是分析了程序的汇编语言而不动使用的数据，内存，栈等，对于逆向来说不会有很好的提高，因此，在熟练掌握汇编语言之后，也要熟练掌握汇编语言使用的数据类型，内存，栈，寄存器的使用。
在逆向分析程序是，要话很多时间分析汇编指令，程序使用的内存，寄存器，栈的方式，**需要时间来慢慢掌握**

- `在逆向分析时，更多时候在理解了汇编指令的语义之后，更多的需要关注内存，栈，寄存器之间的使用，这块是重点，同时也是难点，需要花很多时间去做。`


### SK1LL$

额外的逆向技巧：有时候我们在分析到了关键的表指令，想通过patched内部的指令完成破解，但是，程序去不能单独分离出来使用。此时就要求我们自己写一个注册机。写一个注册机就要求我们能真正明白程序的流程，包括加解密过程，比较值的过程等，这样我们写出来的注册机才会对软件有效果。

