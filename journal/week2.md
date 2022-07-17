1. 学习了riscv的ll/sc设计，c++中atomics的区别，了解了它和CAS的优劣
		- https://www.youtube.com/watch?v=w12tSZNwWuQ
		- https://www.youtube.com/watch?app=desktop&v=A8eCGOqgvH4
		- riscv spec A extension
2. 阅读了riscv特权指令集的ppt和spec，初步理解了priviledged的结构，但不能说完全理解，
TODO要读一些实际代码来加深理解
3. 为了完成ppt里的思考题，断点异常和单步跟踪是如何实现的，大致阅读了gdb的相关代码		
		- https://eli.thegreenplace.net/2011/01/23/how-debuggers-work-part-1		
介绍了呼叫ptrace systemcall实现断点调试，在gdb代码中也可看到`target_follow_fork()`和`riscv-linux-nat.c`
		- 继续看ptrace的实现，对linux内核代码简单grep 一下PTRACE_SINGLESTEP，发现/kernel/ptrace.c调用了arch specific的`user_enable_single_step()`，以为问题解决
		- 然而\~\~，发现riscv没有实现这个函数，继续调查发现最初的Sifive Unleash无法实现hardware breakpoint，而后发现这个https://pi.simark.ca/gdb-patches/CAFyWVabHRPBtLvxr4fQzkRhDHAoEcvfa80sM6cdLHrLqfGX4XA@mail.gmail.com/		
		 所以实际还是通过software实现的。其他ARCH除了流行的amd和部分arm，也都使用的software breakpoint
		- 具体原理大概是先在内存中insert一个ebreak，然后break触发后再删掉。想不到还有这种操作。。，当然必须注意不能在ll/sc之间插入ebreak
		- gdb的具体实现参考`gdbserver/mem-break.cc`，`gdbserver/linux-low.cc`，尤其是`handle_extended_wait`函数，包括了insert,delete,reinsert过程，从里面的`single_step`一路往下扒，就会发现`target_breakpoint_kind_from_pc`跳转到了riscv相关的代码中
		- 不过我对gdb源码的这部分理解不一定正确，之后继续研究
4. 完成os2和os3，感觉rust相对c还是写着舒服啊
