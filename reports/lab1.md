# 简答作业
## 1 进入U态后使用S态寄存器的报错，程序出错行为

答：分别是
[ERROR] [kernel] PageFault in application, core dumped.
[ERROR] [kernel] IllegalInstruction in application, core dumped.
[ERROR] [kernel] IllegalInstruction in application, core dumped.
也即是，bad address会导致page fault，而访问mode不合法的寄存器以及使用mode不合法的指令会触发illegal instruction错误

## 2 __alltraps 和 __restore 两个函数的作用
### （1）刚进入 __restore 时，a0代表了什么值，指出 __restore的两种使用情景

答：根据mv sp, a0可以看出，刚进入时a0是系统调用结束后内核栈顶的值。在执行完系统调用后使用这个函数返回U-mode，或通过这个函数来开始运行一个应用程序。

### （2）这几行汇编代码特殊处理了哪些寄存器？这些寄存器的的值对于进入用户态有何意义？请分别解释。

ld t0, 32*8(sp)
ld t1, 33*8(sp)
ld t2, 2*8(sp)
csrw sstatus, t0
csrw sepc, t1
csrw sscratch, t2

答：通过t0、t1、t2寄存器

### （3）L53-L59：为何跳过了 x2 和 x4？

ld x1, 1*8(sp)
ld x3, 3*8(sp)
.set n, 5
.rept 27
   LOAD_GP %n
   .set n, n+1
.endr

答：x4寄存器叫tp，除非手动出于一些特殊用途使用它，否则一般不会用到，因此无需保存；x2寄存器叫sp，是栈指针寄存器，由于用户栈指针其实已经通过交换保存在sscratch中，此时sp并不是原来的用户寄存器，所以在这里不用保存。

### （4）L63：该指令之后，sp 和 sscratch 中的值分别有什么意义？

csrrw sp, sscratch, sp

答：执行原子交换指令后，sp重新指向用户栈，sscratch指向内核栈

### （5） __restore：中发生状态切换在哪一条指令？为何该指令执行之后会进入用户态？

答：sret会从S态退出到U态，因为此时已经将用户态的寄存器值都恢复，sp也重新指向用户栈，所以切换机器模式后就可以正常回到用户态。

### （6）L13：该指令之后，sp 和 sscratch 中的值分别有什么意义？

csrrw sp, sscratch, sp

答：该指令之后，将用户栈的地址保存在sscratch中，而将内核栈的地址交换到sp中，因此可以后续在内核栈里存本次系统调用所需的上下文信息。

### （7）从 U 态进入 S 态是哪一条指令发生的？

答：这个是在syscall函数里发生的，用的是ecall这条汇编指令。


# lab1实现思路简要说明
改造了taskctrl的结构，增加了start_time记录开始时间，再对每个系统调用都计数（使用一个500大小的数组）。

对taskinfo的计时比较简单，直接用start_time-cur_time就可以；由于每一个任务在统计的时候都是running状态，所以直接设置状态为running即可

