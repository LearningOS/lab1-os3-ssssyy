# 简答作业
## 1 进入U态后使用S态寄存器的报错，程序出错行为

## 2 __alltraps 和 __restore 两个函数的作用
### （1）刚进入 __restore 时，a0代表了什么值，指出 __restore的两种使用情景


### （2）这几行汇编代码特殊处理了哪些寄存器？这些寄存器的的值对于进入用户态有何意义？请分别解释。

ld t0, 32*8(sp)
ld t1, 33*8(sp)
ld t2, 2*8(sp)
csrw sstatus, t0
csrw sepc, t1
csrw sscratch, t2

### （3）L53-L59：为何跳过了 x2 和 x4？

ld x1, 1*8(sp)
ld x3, 3*8(sp)
.set n, 5
.rept 27
   LOAD_GP %n
   .set n, n+1
.endr

### （4）L63：该指令之后，sp 和 sscratch 中的值分别有什么意义？

csrrw sp, sscratch, sp

### （5） __restore：中发生状态切换在哪一条指令？为何该指令执行之后会进入用户态？

### （6）L13：该指令之后，sp 和 sscratch 中的值分别有什么意义？

csrrw sp, sscratch, sp

### （7）从 U 态进入 S 态是哪一条指令发生的？