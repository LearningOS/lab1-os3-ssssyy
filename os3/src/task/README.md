# task
task文件是任务的管理模块，会将一个任务的信息记录在结构体内，供sys_task_info调用

使用文档给出的TaskInfo结构体

根据ref的提示，需要先在context中构造一个存储任务上下文的结构，类似于PSW，称为TaskContext

## 结构
参照ref中的代码，建立mod.rs（用于管理），switch.rs以及switch.S用于ctx switch的保存，task.rs用于管理task info，context.rs用于定义ctx以及初始化等工作的函数调用

## switch.S
这里是riscv的汇编代码，直接使用手册上给出的代码
