

# 深入理解计算机操作系统 学习笔记

# 第1章  计算机系统漫游

## 1 编译系统

### 1.1 编译系统执行过程

包括预处理、编译、汇编、链接四个阶段。

这里面以一个hello word程序简单说明上述过程。

hello.c文件程序

```c
#include <stdio.h>

int main(){

    printf(” h e l l o , world ”);

    return 0;

}
```



预处理：替换掉以#开头的内容替换掉源程序，简单理解就是文本替换，hello.c 经过预处理变成hello.i文件。

编译    ：这一阶段主要是语法分析、语义分析、词法分析、中间代码生成及优化等，将程序翻译成汇编程序，

​               hello.i文件变成hello.s文件。

汇编    ：将汇编程序翻译成机器指令，hello.s 变成可重定位目标文件hello.o。

链接    ：将hello.o 和 printf.o链接合并得到可执行文件hello。

### 1.2 理解编译系统的好处

（1）优化程序性能

（2）理解链接过程出现的错误

（3）避免安全漏洞

## 2 系统硬件架构

系统硬件主要包括CPU、内存、总线、I/O设备。

![csapp_1_1](C:\Users\lenovo\Desktop\CSAPP\csapp_note\pictures\csapp_1_1.PNG)

### 2.1 CPU

CPU主要由PC（程序计数器）、寄存器堆、算术/逻辑单元（ALU）组成。

PC：是一个4字节或者8字节的存储空间，存放的是某一条指令的地址，处理器不断地执行PC指向的指令

寄存器：用于临时存放数据，例如计算a + b, 从内存读取a存放在寄存器X中，读取b存放与寄存器Y中。

ALU:  进行算数与逻辑的计算，计算机核心部分，计算速度极快。

### 2.2 内存

内存主要存放程序指令以及数据，可以看成一个从零开始的大数组，每个字节都有相应地址.

### 2.3 总线

总线负责系统各个部件之间的数据传递，比如处理器和内存、I/O设备和处理器、I/O设备和内存。

### 2.4 I/O设备

包括鼠标、键盘、显示器、磁盘等，通过控制器或者适配器与I/O总线相连。

## 3 解释内存中的指令

以hello程序为例，可执行目标文件**hello** 已经存放在系统的磁盘上，如何运行这个可执行文件呢？

1. 通过键盘输入”./hello” 的字符串，shell 程序会将输入的字符逐一读入寄存器，处理器会把**hello** 这个字符串放入内存中。


2. 按下回车键时，shell程序执行一系列的指令来来加载可执行文件**hello**。
3. 利用DMA（Direct Memory Access）技术，这些指令将**hello** 中的数据和代码从磁盘复制到内存。
4. CPU执行机器指令，将hello, world，从存中复制到寄存器文件中，再复制到显示器输出给用户。

## 4 存储设备

通常，存储设备容量越大，速度越慢，价格越便宜。存储容量对比如下：

寄存器： 100 ~ 1000B

内存： 1 ~ 100G

磁盘：1 ~ 1000TB

计算机系统的信息存储可以用一个层次结构来表示，通常而言，存储容量越小，速度越快，价格越高，上一层存储设备是下一层存储设备的高速缓存。

![csapp_1_2](C:\Users\lenovo\Desktop\CSAPP\csapp_note\pictures\csapp_1_2.PNG)



处理能力强的处理器一般有3级缓存，L1 缓存和寄存器的速度几乎一样快，大小为KB级，L2的缓存速度是L1的五分之一，大小为数十万到数百万字节，L3容量更大，但是速度更慢。

## 5 操作系统

### 5.1 操作系统的作用

操作系统作为应用程序和硬件间的桥梁，	应用程序不能直接操作硬件，只能通过操作系统。这样设计目的有两个：

（1）防止硬件被失控的应用程序滥用；

（2）操作系统提供统一的机制来控制这些复杂的底层硬件。

为了实现中间件的作用，操作系统引入了文件、虚拟内存、进程三个抽象概念。

文件：对I/O设备的抽象

虚拟内存：对内存和磁盘I/O的抽象

进程：对处理器、内存、I/O设备的抽象

### 5.2 进程

几个重要概念：

上下文：进程运行所需要的所有状态信息，包括当前PC和寄存器的值，以及内存中的内容等等。

多线程：一个进程实际上由多个线程组成，每个线程都运行在进程的上下文中，共享代码和数据。

### 5.3 虚拟内存

操作系统为每个进程提供了一个假象，就是每个进程都在独自占用整个内存空间，每个进程看到的内存都是一样的，我们称之为虚拟地址空间。

下图为linux 虚拟地址空间：

![csapp_1_3](C:\Users\lenovo\Desktop\CSAPP\csapp_note\pictures\csapp_1_3.PNG)

主要包含程序代码和数据、堆、共享区域、用户栈、内核区。

### 5.4 文件

linux系统的哲学思想：一切皆文件。

所有的输入输出设备都可以看成文件，网络也可以视为一个I/O设备，系统的输入输出都是通过文件完成。

## 6 系统加速一些概念

### 6.1 基本概念

任务(task)：并行计算所处理的对象.

工作量(workload)：处理某任务的所需的各种开销的总和

处理器(processor)：并行计算中所使用的最基本的处理器单元

执行率(executionrat)：每个处理器单位时间能完成的工作量

执行时间(executiontime)：处理某任务所需的时间

加速比(scalability)：当处理器个数增多时，完成某固定工作量任务所需执行时间的减少倍数.

理想加速比(idealscalability)：处理器个数增多的比例

并行效率(parallelefficiency):加速比÷理想加速比×100%

### 6.2 三个重要定律

阿姆达尔定律

古斯塔法森定律

孙-倪定律

## 7 并发和并行

线程级并发：增加CPU的核心数，可以提高系统的性能，除此之外，通过超线程技术，例如一个CPU执行两个线程，当一个线程因为读取数据而进入等待状态时，CPU可以去执行另外一个线程，其中线程之间的切换只需要极少的时间代价。

指令级并行：现代处理器可以同时执行多条指令

单指令多数据并行：现代处理器拥有特殊的硬件部件，允许一条指令产生多个并行的操作，这种方式称为单指令多         数据（SingleInstructionMultipleData）



# 第2章 信息的处理和表示

## 知识点1 虚拟地址空间

​ 定义：程序将内存视为一个非常大的数组，数组每个元素是一个字节，由一个唯一数表示称为地址，所有地址集合称为虚拟地址空间。

范围：字长决定了虚拟地址空间范围，0 ~ 2w - 1，32位机器，w = 32, 64 位机器，w = 64。

## 知识点2 大端法和小端法存储

大端法：最高有效字节存储在最前面，也就是低地址处。

小端法：最低有效字节存储在在最前面.

注： 举例对于0x01234567来说，最高有效字节是0x01, 最低有效字节是0x67

厂商模式：大多数英特尔机器采用小端模式，IBM 和SUN采用大端模式，ios和安卓只能小端模式，很多新的机器支持双端模式，可以配置。

大小端一种测试方法：代码如下

```c
#include <stdio.h>
typedef unsigned char *byte_pointer;
void show_bytes(byte_pointer start , int len){
    int i;
    for(i = 0; i < len; i++){
    printf(” %.2x”, start[i]);
    }
  
    printf(”\n”);
}

void show_int(int x){
    show_bytes ((byte pointer) &x,sizeof (×));
}
```

## 知识点3 原码、反码、补码

原码：正数的符号位用“0”表示，负数的符号位用“1”表示，其余数位表示数值本身。

反码：正数的反码与其原码相同; 负数的反码是在原码的基础上保持符号位不变，其余
​           各位按位求反得到的。

补码：正数的补码与其原码相同; 负数的补码是在原码保持符号位不变，其它位取反加1。即，负数的补码是它的

​           反码加1。在计算机中，有符号整数常常用补码形式存储。

## 知识点4 整数的转化、扩展和截取

转化：按照原码去求对应转化的值

扩展：

（1）无符号：只需要在扩展的数位进行补0 即可

（2）有符号：最高位是0，此时扩展的数位进行补零即可；当有符号数表示负数时，最高位是1，此时扩展的数位

​                         需要进行补1。

转换定理：当有符号数从一个较小的数据类型转换成较大类型时，进行符号位扩展，可以保持数值不变。

截取：将一个w 位的无符号数，截断成k 位时，丢弃最高的w-k 位，然后求值。

## 知识点5 整数加法、乘法、除法运算

## 知识点6 浮点数表示

（1）浮点数数值 分类

三类：由阶码的值决定分类，第一类是规格化的值，第二类是非规格化的值，第三类是特殊值。

（2）浮点数的IEEE表示

首先理解含有小数值得二进制数：如下图，结果为每个位数的值与二进制幂数相乘。

![csapp_2_1](C:\Users\lenovo\Desktop\CSAPP\csapp_note\pictures\csapp_2_1.PNG)

以上方法缺点是并不能表示很大的数。

IEEE 的浮点表示方法如下：

![2_1](C:\Users\lenovo\Desktop\CSAPP\csapp_note\pictures\2_1.PNG)

浮点数表示由3部分组成：符号s、阶码E 和尾数M。

例如C语言32 位的float, 最高位31 位表示符号位，23到30 位的8个二进制位为阶码E, 剩余的23位是和尾数相关的。64 位的双精度，阶码字段的长度为11位，小数字段的长度为52 位。

### 规格化的值

定义：阶码字段的二进制位不全为0，且不全为1。单精度最小值1，最大值254。

阶码的值   E = e − bias，  e 为8个二进制表示的值， bias 为与阶码位数相关的一个偏置量，单精度8位阶码位127，即为2 的7次方减1。结合e 的范围[1,254] 对于单精度浮点数，阶码E 的取值范围是[-126,127]。

尾数M 被定义为1+f。

### 非规格化的值

定义：当阶码字段的二进制位全为0

E = 1 - bias,   M = F;

非规格化的值有两个用途： 提供了表示0 的方法；可以表示非常接近0的数。

### 特殊值

定义：阶码字段的二进制位全为1, 分为无穷大和NaN("不是一个数")。NaN举例：无穷减无穷，-1 开根号

​              无穷大：当阶码字段全为1，且小数字段全为0 时，表示无穷大的数。无穷大也分为两种，
​                             正无穷大和负无穷大。如果符号位s 等于0 时，表示正无穷大；符号位s 等于1，
​                             表示负无穷大。

​              NaN: 当阶码字段全为1，且小数字段不为0 时, 可以表示NaN (Not a Number)。

### 浮点数表示举例

下图以8位浮点格式为例，阶码数K = 4, 小数位n = 3 。

![2_2](C:\Users\lenovo\Desktop\CSAPP\csapp_note\pictures\2_2.PNG)

### 整型转单精度型举例

### 舍入

对于值x，可能无法用浮点形式来精确的表示，因此我们希望可以找到“最接近的值x’来代替x，这就是舍入操作的任务。

IEEE 浮点格式定义了四种不同的舍入方式，分别是：向偶数舍入、向零舍入、向下舍入以及向上舍入。

### 浮点数运算

（1）浮点数的加法，乘法都不具结合性，另外，浮点乘法在加法上不具备分配性；

（2）当int 类型转换成float 类型时，数字不会发生溢出，但是可能会被舍入。这是由于单精度浮点数的小数字段   是23 位，可能会出现无法保留精度的情况；

（3）从double 类型转换成float 类型，由于float 类型所表示数值的范围更小，所以可能会发生溢出；

（4）此外，float 类型的精度相对于double 较小，转换后还可能被舍入；

（5）将float 类型或者double 类型的浮点数转换成int 类型，一种可能的情况是值会向零舍入，例如1.9 将被转换成1，-1.9 将被转换成-1；另外一种可能的情况是发生溢出。