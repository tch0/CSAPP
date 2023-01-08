<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [第八章：异常控制流](#%E7%AC%AC%E5%85%AB%E7%AB%A0%E5%BC%82%E5%B8%B8%E6%8E%A7%E5%88%B6%E6%B5%81)
  - [8.1 异常](#81-%E5%BC%82%E5%B8%B8)
    - [异常处理](#%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
    - [异常类别](#%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%88%AB)
    - [Linux/x86-64系统中的异常](#linuxx86-64%E7%B3%BB%E7%BB%9F%E4%B8%AD%E7%9A%84%E5%BC%82%E5%B8%B8)
  - [8.2 进程](#82-%E8%BF%9B%E7%A8%8B)
    - [逻辑控制流](#%E9%80%BB%E8%BE%91%E6%8E%A7%E5%88%B6%E6%B5%81)
    - [并发流](#%E5%B9%B6%E5%8F%91%E6%B5%81)
    - [私有地址空间](#%E7%A7%81%E6%9C%89%E5%9C%B0%E5%9D%80%E7%A9%BA%E9%97%B4)
    - [用户模式和内核模式](#%E7%94%A8%E6%88%B7%E6%A8%A1%E5%BC%8F%E5%92%8C%E5%86%85%E6%A0%B8%E6%A8%A1%E5%BC%8F)
    - [上下文切换](#%E4%B8%8A%E4%B8%8B%E6%96%87%E5%88%87%E6%8D%A2)
  - [8.3 系统调用错误处理](#83-%E7%B3%BB%E7%BB%9F%E8%B0%83%E7%94%A8%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
  - [8.4 进程控制](#84-%E8%BF%9B%E7%A8%8B%E6%8E%A7%E5%88%B6)
    - [进程ID](#%E8%BF%9B%E7%A8%8Bid)
    - [创建终止进程](#%E5%88%9B%E5%BB%BA%E7%BB%88%E6%AD%A2%E8%BF%9B%E7%A8%8B)
    - [回收子进程](#%E5%9B%9E%E6%94%B6%E5%AD%90%E8%BF%9B%E7%A8%8B)
    - [让进程休眠](#%E8%AE%A9%E8%BF%9B%E7%A8%8B%E4%BC%91%E7%9C%A0)
    - [加载并运行程序](#%E5%8A%A0%E8%BD%BD%E5%B9%B6%E8%BF%90%E8%A1%8C%E7%A8%8B%E5%BA%8F)
    - [利用fork和execve运行程序](#%E5%88%A9%E7%94%A8fork%E5%92%8Cexecve%E8%BF%90%E8%A1%8C%E7%A8%8B%E5%BA%8F)
  - [8.5 信号](#85-%E4%BF%A1%E5%8F%B7)
    - [信号术语](#%E4%BF%A1%E5%8F%B7%E6%9C%AF%E8%AF%AD)
    - [发送信号](#%E5%8F%91%E9%80%81%E4%BF%A1%E5%8F%B7)
    - [接收信号](#%E6%8E%A5%E6%94%B6%E4%BF%A1%E5%8F%B7)
    - [阻塞和解除阻塞信号](#%E9%98%BB%E5%A1%9E%E5%92%8C%E8%A7%A3%E9%99%A4%E9%98%BB%E5%A1%9E%E4%BF%A1%E5%8F%B7)
    - [编写信号处理程序](#%E7%BC%96%E5%86%99%E4%BF%A1%E5%8F%B7%E5%A4%84%E7%90%86%E7%A8%8B%E5%BA%8F)
    - [同步流以避免讨厌的并发错误](#%E5%90%8C%E6%AD%A5%E6%B5%81%E4%BB%A5%E9%81%BF%E5%85%8D%E8%AE%A8%E5%8E%8C%E7%9A%84%E5%B9%B6%E5%8F%91%E9%94%99%E8%AF%AF)
    - [显式等待信号](#%E6%98%BE%E5%BC%8F%E7%AD%89%E5%BE%85%E4%BF%A1%E5%8F%B7)
  - [8.6 非本地跳转](#86-%E9%9D%9E%E6%9C%AC%E5%9C%B0%E8%B7%B3%E8%BD%AC)
  - [8.7 操作进程的工具](#87-%E6%93%8D%E4%BD%9C%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%B7%A5%E5%85%B7)
  - [补充资料](#%E8%A1%A5%E5%85%85%E8%B5%84%E6%96%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 第八章：异常控制流

通常一个系统中除了顺序的指令流之外还有跳转、函数调用返回这种突变流，这些控制流都是程序的内部变化。但是系统中应当还有其他类型的控制流，也就是程序响应外部环境发生变化的控制流：
- 这些控制流不是被内部程序变量捕获的，而且不一定要和程序执行相关。
- 比如：
    - 一个硬件定时器定期产生的信号，这个事件必须得到处理。
    - 包到达网络适配器后，必须被存放到主存中。
    - 程序向磁盘请求数据，然后休眠，数据就绪后收到的通知。
    - 子进程终止时，父进程得到通知。
- 现代系统通过使控制流发生突变来对这些情况作出反应，我们把这些突变称之为**异常控制流**（Exceptional Control Flow，ECF）。
- 异常控制流发生在系统的各个层次：
    - 硬件层：硬件检测到的事件触发控制转移到异常处理程序。
    - 操作系统层：内核通过上下文切换控制一个用户进程转移到另一个用户进程。
    - 应用层：一个进程发送信号到另一个进程，接受者会突然将控制转移到一个信号处理程序。
    - 一个程序可以通过回避通常的栈规则，并执行到其他函数中任意位置的非本地跳转来对错误做出反应。
- ECF非常重要：
    - ECF是操作系统用来实现I/O、进程和虚拟内存的基本机制。
    - 应用程序通过一个叫做陷阱（trap）或者系统调用（system call）的ECF形式，向操作系统请求服务。ECF是应用程序和内核交互的基本方式，比如磁盘写数据、从网络读数据、创建新进程、终止进程等。
    - 操作系统为应用程序提供了强大的ECF机制，了解这些机制才能编写如Unix Shell和Web服务器之类的程序。
    - ECF是计算机系统中实现并发的基本机制，理解ECF是理解并发的第一步。
    - ECF是实现软件异常的基本机制，理解ECF才能理解异常机制是如何实现的。
- 本章介绍的是应用如何和操作系统交互的东西，这些交互都是围绕ECF的。
- 本章内容：
    - 异常：位于硬件和操作系统的交界。
    - 系统调用：为应用程序提供到操作系统入口点的异常。
    - 进程和信号：应用和操作系统的交界。
    - 非本地跳转：ECF的一种应用层形式。

## 8.1 异常

异常是异常控制流的一种形式，一部分由硬件实现，一部分由操作系统实现。因为有一部分由硬件实现，所以具体系统随系统不同而不同。但基本思想是一致的。
- 异常就是控制流中的突变，用来响应处理器状态的某些变化。
- 状态的变化被称为事件，事件可能和当前指令的执行直接相关，比如缺页、算术溢出、除零等，也可能没有关系，比如系统定时器信号、一个I/O请求完成。
- 发生异常时会跳转到异常处理程序，处理完后再跳转回来继续执行。
```
    |
    |
    | Icurr ---------------->| 异常处理程序
event occurs here            |
    | Inext <----------------|
    |
    |
    v
```
- 任何情况下，当处理器检测到事件发生，它会通过一张叫做**异常表**（exception table）的跳转表，进行一个间接过程调用，跳转到专门设计用来处理这类事件的操作系统子程序（异常处理程序，exception handler）。异常处理程序处理完后，根据引起异常的事件的类型，会发生以下三种情况中的一种：
    - 处理程序将控制返回给当前指令Icurr，即发生事件时正在执行的指令。
    - 处理程序将事件返回给下一个指令Inext，即没有发生异常将会执行的下一条指令。
    - 处理程序终止被中断的程序。

注意：本章所述的异常是ECF中统称的异常，而编程语言中的异常则只是其中一种应用级ECF实现，大多数时候异常都是指前者，从上下文可以区分到底在说硬件级异常还是软件异常。

### 异常处理

异常是由硬件和软件配合实现的，需要搞清楚他们分别负责哪个部分：
- 系统中每种可能的异常都分配了一个唯一的非负整数的**异常号**（exception number）。
- 其中一些号码是由处理器设计者分配，其他号码是由操作系统内核的设计者分配。前者包括除零、缺页、内存访问违例、断点、算术运算溢出等，后者包括系统调用、外部I/O设备信号等。
- 系统启动时，操作系统会分配或者初始化一张称为异常表的跳转表。通过异常号k作为索引就可以从异常表中查找出对应的异常处理程序。
- 异常表的起始地址放在一个叫做**异常表基址寄存器**（exception table base register）的寄存器中，通过异常号乘以异常表条目大小就能得到异常处理程序地址。在x86-64架构中，通过一次比例变址寻址就能找到。
- 在运行时，处理器检测到发生了一个事件，并确定了异常号k之后。处理器会通过执行间接过程调用触发异常，通过异常表条目k，转到对应的处理程序。
- 异常类似于过程调用，但有一些不同之处：
    - 过程调用时，跳转到处理程序前，处理器将返回地址压入栈中。然而根据异常类型，返回地址要么是当前指令，要么是下一条指令。
    - 处理器还会将一些额外状态压入栈中，在异常处理返回后再恢复这些状态。
    - 如果控制从用户程序转移到内核，那么所有项目都被压入内核栈，而不是压入用户栈。
    - 异常处理程序运行在内核态下，也就是说他们对所有的系统资源都有完全的访问权限。
- 一旦硬件触发了异常，转入异常处理程序中后，剩下的工作都由异常处理程序完成。在处理结束后，通过一条特殊的“从中断返回”指令，可选地返回到被中断的程序。该指令将适当的状态弹出到处理器的控制和数据寄存器中。如果异常中断的是一个用户程序，就将状态恢复到用户态，然后将控制返回给被中断程序。

### 异常类别

异常分为四类：中断（interrupt）、陷阱（trap）、故障（fault）、终止（abort）。这些异常有不同的属性：

|类别|原因|异步发生/同步发生|返回行为|
|:-:|:-|:-:|:-|
|中断|来自I/O设备信号|异步|总是返回下一条指令
|陷阱|有意的异常|同步|总是返回到下一条指令
|故障|潜在可恢复的错误|同步|可能返回到当前指令
|终止|不可恢复的错误|同步|不会返回

**中断**：
- 中断是**异步发生**的，是来自于处理器外部的I/O设备信号的结果。也就是说它不是由任何一条专门的指令造成的，而是随机的来自外围设备的信号。
- 硬件中断的处理程序常称为中断处理程序。
- 发起中断的设备通过向处理器芯片的引脚发送信号，并将异常放到系统总线上，来触发中断，这个异常号标识了引起中断的设备。
- 当每一条指令执行完成后，处理器如果注意到中断引脚电压变高了，就会从系统总线上读取异常号，然后调用适当的中断处理程序。
- 当处理程序返回时，将控制返回给下一条指令，程序继续执行，好像没有中断过一样。
- 剩下的类型都是同步发生的，是执行当前指令的结果，将这些指令统称为故障指令（faulting instruction）。

**陷阱和系统调用**：
- 陷阱是有意的异常，是执行一条指令的结果，陷阱处理程序将返回到下一条指令。
- 陷阱最重要的用途是在用户程序和内核之间提供像过程调用一样的接口，称之为系统调用（system call）。
- 用户程序经常需要向系统请求服务，这些服务只能有系统调用在内核态下完成，所以需要一个陷阱，以从用户态切换到内核态。这些服务很多，比如读一个文件（read）、创建一个新线程（fork）、加载一个新程序（execve）、终止当前进程（exit）等。
- 为了支持这类服务，系统提供了一条特殊的`syscall n`指令，用户程序请求服务`n`时就可以执行这条指令，执行`syscall1`指令会将控制传递到陷阱处理程序，这个处理程序解析参数，并调用适当的内核程序。
- 普通用户程序运行在用户态，只有使用系统调用进入内核态才能调用一些特权指令和访问内核栈。操作系统通过系统调用的方式向用户提供一系列功能，用户要使用这些功能就必须调用系统调用（直接或者间接）。

**故障**：
- 故障由错误情况引起，可能能够被故障处理程序修正。
- 故障发生时，处理器将控制权转移给故障处理程序，如果故障处理程序能够修正这个情况，那么它就将控制返回到引起故障的指令，从而重新执行它。否则，处理程序调用内核中的`abort`例程，`abort`例程会终止引起故障的应用程序。
- 一个典型的故障例子是**缺页异常**，当指令引用一个虚拟地址，而这个虚拟地址对应的物理页面不再内存中，那么就必须将其从磁盘换出到内存中，就会发生故障。当缺页处理程序返回时，重新执行引起缺页的指令，对应物理页面已经在内存中，就不会再发生缺页了。

**终止**：
- 终止是不可恢复错误造成的结果，通常是一些硬件错误。
- 终止处理程序不会将控制返回给应用程序，而是将控制返回给一个`abort`例程，它将终止程序运行。

### Linux/x86-64系统中的异常

x86-64中系统中定义的异常，有256中异常类型，0~31为Intel架构师定义，对任何x86-64系统都一样，32~256对应操作系统定义的中断和陷阱：
- 例子：

|异常号|描述|异常类别|
|:-:|:-|:-
|0|除法错误|故障
|13|一般保护故障|故障
|14|缺页|故障
|18|机器检查|终止
|32~255|操作系统定义异常|中断或陷阱

- 除法错误：整数除零时会导致除零故障，Unix系统不会视图从除法错误中恢复，而是选择终止程序，Linux Shell通常会将除法错误报告为`Floating point exception`。
- 一般保护故障：很多原因可能会导致，比如访问一个未分配的虚拟内存区域，或者写一个只读页面中的内存，Linux不会尝试恢复这类故障，Linux Shell通常将这类保护故障报告为**段错误**（segmentation fault）。
- 段错误例子：
```C
int main()
{
    char* s = "hello"; // "hello" is in .rodata section
    s[0] = 'y'; // write read-only data
    return 0;
}
```
- 缺页：如前所述，第九章有具体细节。
- 机器检查：在导致故障的指令执行过程中检测到指明硬件错误时发生，机器检查处理程序不会返回控制给应用程序，而知直接终止。

Linux/X86-64系统调用：
- Linux系统提供几百种系统调用，当应用程序想要请求内核服务时可以使用，比如读写文件、创建新进程等。
- 每个系统调用都有一个唯一的整数号，对应到内核中一个跳转表的偏移量。（注意这个跳转表和异常表不一样，这个编号也不是异常号）。
- C程序用`syscall`函数可以直接调用任何系统调用，然而实际中几乎没有必要这么做，对于大多数系统调用，操作系统都提供了标准C函数封装。这些包装函数将参数打包到一起，以适当系统调用指令陷入内核，然后将系统调用的状态返回给调用程序。
- 系统调用的调用惯例，和普通函数不一样：
    - 寄存器`%rax`包含系统调用号。
    - 所有系统调用参数都使用寄存器传递。
    - 使用寄存器`%rdi %rsi %rdx %r10 %r8 %r9`传递最多6个参数。
    - 从系统调用返回时，`%rcx %r11`会被破坏，`%rax`包含返回值。
    - 返回值在`-4095 ~ -1`表明发生了错误，对应于付的errno。
- Linux系统常见系统调用示例：

|编号|名字|描述、
|:-:|:-:|:-
|0|read|读文件|
|1|write|写文件
|2|open|打开文件
|3|close|关闭文件
|4|stat|获得文件信息
|9|mmap|将内存映射到文件
|12|brk|重置堆顶
|32|dup2|复制文件描述符


使用示例：
- 用`syscall`直接进行系统调用：
```
SYSCALL(2)

NAME
       syscall - indirect system call

SYNOPSIS
       #include <unistd.h>
       #include <sys/syscall.h>   /* For SYS_xxx definitions */

       long syscall(long number, ...);
```
- `write`包装：
```
WRITE(2)

NAME
       write - write to a file descriptor

SYNOPSIS
       #include <unistd.h>

       ssize_t write(int fd, const void *buf, size_t count);
```
- `write`例子：
```C
#include <unistd.h>

char* s = "yes\n";

int main()
{
    syscall(1, STDOUT_FILENO, "hello\n", 6);
    write(STDOUT_FILENO, "world\n", 6);
    // GCC inline assembly
    __asm__(
        "movq $1, %rax\n\t" // syscall number: 1
        "movq $1, %rdi\n\t" // STDOUT_FILENO is 1
        "movq s, %rsi\n\t" // value of s
        "movq $4, %rdx\n\t" // size: 4
        "syscall"
    );
    write(STDOUT_FILENO, "no\n", 3);
    return 0;
}
```
- 输出结果：
```
hello
world
yes
no
```
- 使用`syscall`和调用C包装是一样的，最终都是通过`syscall`指令来陷入内核，进行系统调用。

## 8.2 进程

进程（process）是计算机科学中最深刻、最成功的概念之一，异常是实现进程概念的基本构造块：
- 进程给我们提供了一个假象：我们的程序好像是系统中当前唯一运行的程序一样，我们的程序好像是独占处理器和内存，处理器好像是无间断一条接一条执行程序中指令。
- 进程的经典定义是一个执行中程序的实例。系统中每个程序都运行在某个进程的上下文（context）中。上下文是由程序正确运行所需的状态组成的，这个状态包括存放在内存中的程序和数据、栈、通用目的寄存器内容、程序计数器、环境变量、打开文件描述符、页表等的集合。
- 在Shell中执行一个程序时，Shell就会创建一个新进程，然后在这个新进程的上下文中运行这个可执行目标文件。
- 进程提供给应用程序的关键抽象：
    - 一个独立的逻辑控制流：提供程序在独占使用处理器的假象。
    - 一个私有的地址空间：提供程序独占使用内存系统的假象。

### 逻辑控制流

每个程序的PC的序列称之为逻辑控制流，或者逻辑流，
- 操作系统向我们提供独占处理器的假象，实际上却是每个进程运行一段时间后挂起，轮到其他进程执行。
- 不过由于切换过程中并不改变程序状态，所以进程的切换对我们来说并不会有感知。逻辑流看起来是连续的。

### 并发流

- 一个逻辑流在时间上与另一个流重叠，称之为并发流（concurrent flow），这两个流被称为并发地运行，准确地说，他们互相并发。
- 多个流并发执行的现象称之为并发（concurrency）。
- 一个进程和其他进程轮流运行的概念称之为多任务（multitasking）。
- 一个进程执行它的控制流的一部分的每一时间段叫做时间片（time slice）。多任务也叫做时间分片（time slicing）。一个进程的流由多个时间片组成。
- 并发流的思想与流运行的处理器核心数量或者计算机数量无关，如果两个流在时间上重叠，它们就是并发的，即使是运行在同一处理器上。
- 如果在同一个时刻，两个流同时在运行，他们就称之为并行流（parallel stream），它们是并发流的真子集。并行流是实现并发流的其中一个手段。

### 私有地址空间

进程也为每个程序提供了它们在独占地址空间的假象，进程为每个程序提供他们自己的私有地址空间。
- 一般而言，一个进程的私有地址空间内存中的字节是不会被其他进程读取的。
- 这样的地址空间都有通用的结构，细节见第九章。
- 私有地址空间是通过虚拟内存实现的，细节见第九章。

### 用户模式和内核模式

为了更好地实现进程，操作系统应该提供一种机制，限制一个应用可以执行的指令以及它可以访问的地址空间范围：
- 现代处理器通常通过某个控制寄存器中的模式位来提供这种功能，这个寄存器描述了进程当前所拥有的特权。
- 模式位可以有两种状态：内核态和用户态。运行在内核态中的进程可以执行任何指令，并且可以访问系统中的任何内存位置，而运行在用户态的进程则只能访问自己的地址空间，并且只能执行特权指令。
- 普通程序都是运行在用户态的，用户态下不能执行特权指令，比如停止处理器、改变模式位、发起I/O操作等，也不允许进程直接修改地址空间中内核区的数据和代码。任何这种尝试都会导致故障发生。
- 操作系统通过系统调用提供这些功能，执行系统调用将进入内核态。
- 一般地来说，将控制从用户态转换为内核态的唯一方式是通过中断、故障或者陷阱这样的异常。
- 异常发生时，控制传递到异常处理程序，执行异常处理程序时，处理器模式将从用户态变为内核态。返回应用程序时，又从内核态切换回用户态。

Linux提供`/proc`文件系统，允许用户模式进程访问内核数据结构内容：
- `/proc`文件系统将许多内核数据结构的内容输出为一个用户程序可以读取的文本文件的层次结构。
- 比如可以通过`cat /proc/cpuinfo`查看CPU信息。

### 上下文切换

操作系统内核使用一种叫做**上下文切换**（context switch）的高级形式的异常控制流来实现多任务。
- 上下文切换机制建立在底层异常机制之上。
- 内核为每个进程维护一个上下文（context），上下文就是内核重新启动一个被抢占的进程所需的状态。包括：通用目的寄存器、浮点寄存器、程序计数器、用户栈、状态寄存器、内核栈和各种内核数据结构，比如页表、进程表、打开文件表等。
- 进行执行中，内核可以决定抢占当前线程，并重新开始一个先前被抢占的线程。这种决策叫做调度（scheduling），由内核中的**调度器**（scheduler）的代码处理。
- 内核通过上下文切换的方式调度进程，上下文切换的步骤：
    - 保存当前进程上下文。
    - 恢复某个先前被抢占的进程被保存的上下文。
    - 将控制传递给新恢复的进程。
- 当内核代表用户执行系统调用时，可能会发生上下文切换。如果一个进程发生阻塞（等待IO或者等待锁释放条件变量通知等）或者睡眠，那么内核会将其切换到其他进程。即使系统调用没有阻塞，分配的时间片执行完之后，系统也会选择切换上下文调度到其他线程。
- 中断也可能引发上下文切换，比如这个中断可能是由定时器发送表示当前进程时间片已经执行完了，或者磁盘DMA传输完成发送来的中断信号。

## 8.3 系统调用错误处理

UNIX系统级函数遇到错误时，通常会返回`-1`，并设置全局变量`errno`，可以使用`strerror`将`errno`的值转化为一个字符串。使用每个可能出错的系统级函数时，总是应该检查其返回值，可以将检查返回值的工作包装到一个函数中，以简化重复代码。

## 8.4 进程控制

UNIX提供了大量操作进程的系统调用，这一节描述这些函数并说明如何使用。

### 进程ID

每个进程都有一个唯一的正整数进程ID（PID）：
- `getid`获取调用进程的PID。
- `getppid`获取父进程PID。
```C
#include <sys/types.h>
#include <unistd.h>
pid_t getpid(void);
pid_t getppid(void);
```

### 创建终止进程

从程序员角度，可以说程序总是处于下面三种状态之一：
- 运行：要么在CPU上运行，要么在等待执行最终会被内核调度。
- 停止：进程被挂起，且不会被调度。当收到`SIGSTOP SIGTSTP STGTTIN STGTTOUT`信号时，进程被挂起，知道收到`SIGCONT`信号。信号是软中断的一种形式，下一节详述。
- 终止：运行结束。三种终止原因：收到终止信号，从主程序返回，调用`exit`函数。
```C
#include <stdlib.h>
void exit(int status);
```

相关UNIX系统调用接口：
- 父进程通过`fork`函数创建新的子进程：
```C
#include <sys/types.h>
#include <unistd.h>
pid_t fork(void);
```
- 新创建的子进程几乎但不完全和父进程完全相同，子进程得到与父进程用户级虚拟地址空间相同但独立的一份副本（会使用COW机制降低不必要的开销），父进程与子进程最大区别在于它们有不同的PID。
- `fork`会返回两次，子进程中返回0，父进程中返回子进程PID，以此区分子进程和父进程。
- 子进程和父进程是并发执行的独立进程。
- 子进程和父进程拥有相同但是独立的虚拟地址空间。
- 子进程父进程共享打开的文件，比如`stdout`标准输出。

### 回收子进程

- 当一个子进程由于某种原因终止时，内核并不是立刻将其从系统中清除。
- 相反，进程会保持在已终止的状态中，直到被它的父进程回收（reaped）。
- 当父进程回收已终止子进程时，内核将子进程状态传递给父进程，然后抛弃终止的进程，从此刻开始，该子进程就不存了。
- 一个终止了但是还未被回收的进程称为僵死进程（zombie）。
- 如果一个父进程终止了，内核会安排`init`进程称为它的孤儿进程的养父进程。
- `init`进程的PID为1，是内核启动时就创建的，不会终止。
- 如果父进程没有回收它的僵死子进程就终止了，内核会安排`init`进程回收这些僵死的子进程。
- 长时间运行的程序，总是应该回收它们的僵死子进程，即是僵死子进程没有运行，他们依然在消耗内存资源。

回收子进程：
- 一个进程可以通过使用`waitpid`函数等待它的子进程终止或者结束：
```C
#include <sys/types.h>
#include <sys/wait.h>
pid_t wait(int *wstatus);
pid_t waitpid(pid_t pid, int *wstatus, int options);
```
- 等待集合：
    - 如果`pid > 0`，那么就是等待该PID的单独子进程。
    - 如果`pid == -1`，那么等待集合就是父进程的所有子进程。
- `options`参数可以选择`WNOHANG WUNTRACED WCONTINUED`是在子进程仍在执行时直接返回还是挂起父进程。
- `wstatus`可以用来保存返回子进程的状态信息：是`exit return`正常返回，还是因为信号返回，可以通过这个参数得到进程终止的相关信息。
- 更多细节查看`man waitpid`。
- `wait`是`waitpid`的简化版本，等价于`waitpid(-1, wstatus, 0)`。

### 让进程休眠

`sleep`函数让进程挂起指定时间：
```C
#include <unistd.h>
unsigned int sleep(unsigned int seconds);
```
- 查看`man 3 sleep`。
- 可以被信号中断。

`pause`函数让进程休眠，直到进程受到一个信号：
```C
#include <unistd.h>
int pause(void);
```
- `man 2 pause`。

### 加载并运行程序

```C
#include <unistd.h>
int execve(const char *pathname, char *const argv[],
                  char *const envp[]);
```
- 执行文件`pathname`，如果找不到，立即返回。
- 如果找到，以参数列表`argv`，环境变量列表`envp`执行，此时从不返回。
- `argv`第一个参数是可执行文件名，环境变量为`name=value`形式的字符串列表，都以NULL作为列表末尾标志。
- 加载了文件之后，转到启动代码，设置栈，并将控制交给新程序主函数。
- Linux提供几个函数来操作环境变量：
```C
#include <stdlib.h>
char *getenv(const char *name);
int setenv(const char *name, const char *value, int overwrite);
int unsetenv(const char *name);
```

### 利用fork和execve运行程序

像Unix Shell和Web服务器这样的程序大量使用`fork`和`execve函数来执行程序：
- shell是一个交互型应用程序，代表用户运行其他程序。
- shell是一个Read-Eval-Loop，读取用户输入，执行，终止，并继续循环这个过程。
- 书上P526编写了一个简单的shell，实现了读取-求值循环，可以参考。

更多细节参考APUE。

## 8.5 信号

前面说了硬件和软件如何配合实现底层异常机制，这里将研究一种更高层的软件形式的异常，成为Linux信号，它允许进程和内核中断其他进程。
- 一个信号就是一条小消息，它通知进程系统中发生了一个某种类型的事件。
```
Linux supports the standard signals listed below.  The second column of the table indicates which standard (if any) specified the signal: "P1990" indicates that the signal is described
in the original POSIX.1-1990 standard; "P2001" indicates that the signal was added in SUSv2 and POSIX.1-2001.

Signal      Standard   Action   Comment
────────────────────────────────────────────────────────────────────────
SIGABRT      P1990      Core    Abort signal from abort(3)
SIGALRM      P1990      Term    Timer signal from alarm(2)
SIGBUS       P2001      Core    Bus error (bad memory access)
SIGCHLD      P1990      Ign     Child stopped or terminated
SIGCLD         -        Ign     A synonym for SIGCHLD
SIGCONT      P1990      Cont    Continue if stopped
SIGEMT         -        Term    Emulator trap
SIGFPE       P1990      Core    Floating-point exception
SIGHUP       P1990      Term    Hangup detected on controlling terminal
                                or death of controlling process
SIGILL       P1990      Core    Illegal Instruction
SIGINFO        -                A synonym for SIGPWR
SIGINT       P1990      Term    Interrupt from keyboard
SIGIO          -        Term    I/O now possible (4.2BSD)
SIGIOT         -        Core    IOT trap. A synonym for SIGABRT
SIGKILL      P1990      Term    Kill signal
SIGLOST        -        Term    File lock lost (unused)
SIGPIPE      P1990      Term    Broken pipe: write to pipe with no
                                readers; see pipe(7)
SIGPOLL      P2001      Term    Pollable event (Sys V).
                                Synonym for SIGIO
SIGPROF      P2001      Term    Profiling timer expired
SIGPWR         -        Term    Power failure (System V)
SIGQUIT      P1990      Core    Quit from keyboard
SIGSEGV      P1990      Core    Invalid memory reference
SIGSTKFLT      -        Term    Stack fault on coprocessor (unused)
SIGSTOP      P1990      Stop    Stop process
SIGTSTP      P1990      Stop    Stop typed at terminal
SIGSYS       P2001      Core    Bad system call (SVr4);
                                see also seccomp(2)
SIGTERM      P1990      Term    Termination signal
SIGTRAP      P2001      Core    Trace/breakpoint trap
SIGTTIN      P1990      Stop    Terminal input for background process

SIGTTOU      P1990      Stop    Terminal output for background process
SIGUNUSED      -        Core    Synonymous with SIGSYS
SIGURG       P2001      Ign     Urgent condition on socket (4.2BSD)
SIGUSR1      P1990      Term    User-defined signal 1
SIGUSR2      P1990      Term    User-defined signal 2
SIGVTALRM    P2001      Term    Virtual alarm clock (4.2BSD)
SIGXCPU      P2001      Core    CPU time limit exceeded (4.2BSD);
                                see setrlimit(2)
SIGXFSZ      P2001      Core    File size limit exceeded (4.2BSD);
                                see setrlimit(2)
SIGWINCH       -        Ign     Window resize signal (4.3BSD, Sun)
```
- 其中比较常见的有：`SIGSEGV`段错误、除零`SIGFPE`、键盘输入Ctrl+C则会触发`SIGINT`、执行非法指令`SIGILL`、一个进程可以向另一个进程发送`SIGKILL`来终止它、一个子进程终止或者停止时，内核会发送`SIGCHILD`给父进程。
- 行为一栏有几种：终止Term，终止并转储Core，停止Stop，忽略Ign。
- 每种信号类型都对应于某种系统事件，低层的硬件异常是由内核异常处理程序处理的。正常情况下，对用于进程是不可见的。信号提供了一种机制，通知用户进程发生了这些异常。

### 信号术语

发送信号给目的进程由两个步骤组成：
- 发送信号：内核通过更新目的进程的上下文中的某个状态，发送一个信号给目的进程。
    - 发送信号可能有两种原因：
    - 内核检测到一个系统事件，比如除零错或者子进程终止。
    - 一个进程调用了`kill`函数，显式要求内核发送一个信号个目的进程。
- 当目的进程被内核强迫以某种方式对信号的发送做出反应时，它就接收了信号，
    - 进程可以忽略这个信号，终止，或者通过执行一个称为信号处理程序的（signal handler）的用户层函数捕获这个信号。

### 发送信号

UNIX系统提供了大量向进程发送信号的机制，这些机制都基于进程组（process group）这个概念。

**进程组**：
- 每个进程只属于一个进程组，进程组由一个正整数进程组ID标识，`getpgrp`函数返回当前进程的进程组ID。
```
NAME
       setpgid, getpgid, setpgrp, getpgrp - set/get process group

SYNOPSIS
       #include <sys/types.h>
       #include <unistd.h>

       int setpgid(pid_t pid, pid_t pgid);
       pid_t getpgid(pid_t pid);

       pid_t getpgrp(void);                 /* POSIX.1 version */
       pid_t getpgrp(pid_t pid);            /* BSD version */

       int setpgrp(void);                   /* System V version */
       int setpgrp(pid_t pid, pid_t pgid);  /* BSD version */

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):
```
- 默认一个进程和它的父进程同属于一个进程组，可以通过`setpgrp`改变自己或其他进程进程组。

**用`/bin/kill`程序发送信号**：
- `/bin/kill`可以向另外程序发送任意信号：
```shell
/bin/kill -9 15213
```
- 将向进程`15213`发送信号`SIGKILL`（9）。
- 负的PID将导致向进程组中所有进程发送信号。

**从键盘发送信号**：
- Unix Shell使用作业（job）这个抽象概念表示对一条命令行求值而创建的进程。
- 在任何时刻，至多只有一个前台作业和0个或多个后台作业（键入命令后跟`&`将会后台运行）。
- Shell为每个作业创建一个独立进程组，进程组ID通常取自作业中父进程的那一个。
- 键盘输入Ctrl+C会导致内核发送`SIGINT`给前台进程组中每个进程，默认情况下，结果是终止前台作业。
- 类似地，输入Ctrl+Z，会发送`SIGTSTP`给前台进程组中每个进程，默认情况下，结果是挂起前台作业。

**用`kill`函数发送信号**：
```
KILL(2)

NAME
       kill - send signal to a process

SYNOPSIS
       #include <sys/types.h>
       #include <signal.h>

       int kill(pid_t pid, int sig);
```
- 如果`pid > 0`，那么发送信号给该进程。
- 如果`pid == 0`，那么发送给调用进程所在进程组所有进程。
- 如果`pid < 0`，那么发送给绝对值表示进程所在进程组所有进程。
- `man 2 kill`。

**用alarm函数发送信号**：
```
ALARM(2)

NAME
       alarm - set an alarm clock for delivery of a signal

SYNOPSIS
       #include <unistd.h>

       unsigned int alarm(unsigned int seconds);
```
- `man 2 alarm`。
- 内核安排在`seconds`秒后发送`SIGALRM`给调用进程。

### 接收信号

信号的接收：
- 当内核吧进程从内核态切换到用户态时，会检查进程上下文中未被阻塞的进程上下文信号结集合，如果为空，那么将控制转到下一条指令。如果不为空，则会选择一个信号进行处理（通常是最小的）。
- 每个信号都有一个预定义的默认行为，是下面一种：
    - 终止。
    - 终止并转储内存。
    - 挂起/停止直到被`SIGCONT`信号重启。
    - 忽略该信号。
- 进程可以通过`signal`函数修改和信号关联的默认行为：
```
SIGNAL(2)

NAME
       signal - ANSI C signal handling

SYNOPSIS
       #include <signal.h>

       typedef void (*sighandler_t)(int);

       sighandler_t signal(int signum, sighandler_t handler);

DESCRIPTION
       The  behavior  of  signal() varies across UNIX versions, and has also varied historically across different versions of Linux.  Avoid its use: use sigaction(2) instead.  See Portability
       below.

       signal() sets the disposition of the signal signum to handler, which is either SIG_IGN, SIG_DFL, or the address of a programmer-defined function (a "signal handler").

       If the signal signum is delivered to the process, then one of the following happens:

       *  If the disposition is set to SIG_IGN, then the signal is ignored.

       *  If the disposition is set to SIG_DFL, then the default action associated with the signal (see signal(7)) occurs.

       *  If the disposition is set to a function, then first either the disposition is reset to SIG_DFL, or the signal is blocked (see Portability below), and then handler is called with ar‐
          gument signum.  If invocation of the handler caused the signal to be blocked, then the signal is unblocked upon return from the handler.

       The signals SIGKILL and SIGSTOP cannot be caught or ignored.

RETURN VALUE
       signal() returns the previous value of the signal handler, or SIG_ERR on error.  In the event of an error, errno is set to indicate the cause.
```
- 设置的这个函数称为信号处理函数，调用信号处理程序称为捕获信号，执行信号处理程序称为处理信号。
- 信号处理程序有一个参数就是信号整数值，这个参数允许同一个函数处理不同类型的信号。
- 当信号处理程序执行`return`后，控制（通常）会传递回控制流中进程被信号中断位置处的指令。

### 阻塞和解除阻塞信号

Linux提供阻塞信号的机制：
- 隐式阻塞：内核默认阻塞任何当前处理程序正在处理信号类型的待处理信号。也就是说在处理信号s过程中，不会在被此时受到的信号s中断。
- 显式阻塞：应用程序可以用`sigprocmask`函数和它的辅助函数，明确地阻塞和解除阻塞选定的信号。
- 见`man 2 sigprocmask`。

### 编写信号处理程序

编写信号处理程序通常很棘手，有几个原因：
- 处理程序和主程序并发运行，共享同样的全局变量，因此可能会互相干扰。
- 如何以及何时接受信号常常未被直觉。
- 不同系统由不同的信号处理语义。

安全的信号处理：
- 处理程序尽可能简单。
- 在处理程序中只调用异步信号安全的函数，列表见`man 7 signal`，这些函数要么是可重入的，要么不能被信号处理程序中断。
    - 信号处理程序中产生输出的安全方式是`write`。
- 保存和恢复errno。
- 主程序中阻塞所有信号以保护对共享全局数据结构的访问。
- 用`volatile`声明全局变量，防止优化，并在访问全局变量时阻塞信号。
- 用`sig_atomic_t`声明全局标志以原子读写。
- 上面的要求并不是任何时刻都全部必须。

正确的信号处理：
- 信号的一个与直觉不符的方面是未处理信号是不排队的，如果有一个信号还未处理，那么第二个到达的会被直接丢弃。这基于一个思想：如果存在未处理信号表明至少一个信号到达了。
- 举个例子，我们可能希望父进程在等待子进程结束回收子进程前去做自己的事情，而不是等待子进程结束。那么可以用`SIGCHLD`信号处理函数来回收子进程，但是如果有多个子进程，在收到信号但还未处理时又收到同一个信号，信号处理的次数可能就会和僵死的子进程数量不一样，导致某些僵死子进程未被回收。那代码中就需要解决信号不会排队这个问题。
- **不可以用信号来对其他进程中发生的事件计数**。
- 我们可以通过在`SIGCHLD`处理程序中每次回收僵死的所有子进程解决这个问题。

可移植的信号处理：
- Unix信号处理存在一些缺陷：
    - signal函数在不同系统中语义各有不同。
    - 系统调用可以中断。某些慢速系统调用比如`read write`可以被信号中断，在这时，程序必须手动检测`errno`并在被中断时重启系统调用代码。
- Posix标准定义了`sigaction`函数，允许用户设置信号处理函数时，明确指定他们想要的语义：
```C
#include <signal.h>
int sigaction(int signum, const struct sigaction *act,
                     struct sigaction *oldact);
```

### 同步流以避免讨厌的并发错误

喜好处理是异步的，显式的同步操作是必要的，可能需要异步编程所需要的所有措施。

### 显式等待信号

使用循环等待显然不是一个好主意，合适的解决方式是使用`sugsyspend`：
```
SIGSUSPEND(2)

NAME
       sigsuspend, rt_sigsuspend - wait for a signal

SYNOPSIS
       #include <signal.h>

       int sigsuspend(const sigset_t *mask);

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

       sigsuspend(): _POSIX_C_SOURCE

DESCRIPTION
       sigsuspend()  temporarily replaces the signal mask of the calling thread with the mask given by mask and then suspends the thread until delivery of a signal whose action is to invoke a
       signal handler or to terminate a process.

       If the signal terminates the process, then sigsuspend() does not return.  If the signal is caught, then sigsuspend() returns after the signal handler returns, and the  signal  mask  is
       restored to the state before the call to sigsuspend().

       It is not possible to block SIGKILL or SIGSTOP; specifying these signals in mask, has no effect on the thread's signal mask.

RETURN VALUE
       sigsuspend() always returns -1, with errno set to indicate the error (normally, EINTR).
```

## 8.6 非本地跳转

C语言提供了一种用户级异常控制流形式，称之为**非本地跳转（nonlocal jump）**：
- 它将控制直接从一个函数转移到另一个当前正在执行的函数，而不需要经过正常的调用-返回序列。
- 非本地跳转通过`setjmp longjmp`实现：
```
SETJMP(3)

NAME
       setjmp, sigsetjmp, longjmp, siglongjmp  - performing a nonlocal goto

SYNOPSIS
       #include <setjmp.h>

       int setjmp(jmp_buf env);
       int sigsetjmp(sigjmp_buf env, int savesigs);

       void longjmp(jmp_buf env, int val);
       void siglongjmp(sigjmp_buf env, int val);

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

       setjmp(): see NOTES.

       sigsetjmp(): _POSIX_C_SOURCE

DESCRIPTION
       The  functions  described on this page are used for performing "nonlocal gotos": transferring execution from one function to a predetermined location in another function.  The setjmp()
       function dynamically establishes the target to which control will later be transferred, and longjmp() performs the transfer of execution.

       The setjmp() function saves various information about the calling environment (typically, the stack pointer, the instruction pointer, possibly the values of  other  registers  and  the
       signal mask) in the buffer env for later use by longjmp().  In this case, setjmp() returns 0.

       The longjmp() function uses the information saved in env to transfer control back to the point where setjmp() was called and to restore ("rewind") the stack to its state at the time of
       the setjmp() call.  In addition, and depending on the implementation (see NOTES), the values of some other registers and the process signal mask may be restored to their state  at  the
       time of the setjmp() call.

       Following  a  successful  longjmp(),  execution  continues as if setjmp() had returned for a second time.  This "fake" return can be distinguished from a true setjmp() call because the
       "fake" return returns the value provided in val.  If the programmer mistakenly passes the value 0 in val, the "fake" return will instead return 1.

   sigsetjmp() and siglongjmp()
       sigsetjmp() and siglongjmp() also perform nonlocal gotos, but provide predictable handling of the process signal mask.

       If, and only if, the savesigs argument provided to sigsetjmp() is nonzero, the process's current signal mask is saved in env and will be restored if a siglongjmp() is  later  performed
       with this env.

RETURN VALUE
       setjmp() and sigsetjmp() return 0 when called directly; on the "fake" return that occurs after longjmp() or siglongjmp(), the nonzero value specified in val is returned.

       The longjmp() or siglongjmp() functions do not return.
```
- `setjmp`函数在`env`缓冲区中保存当前调用环境，以供后面的`longjmp`使用，并返回0。调用环境包括程序计数器、栈指针、通用目的寄存器。`setjmp`返回值不能被赋给变量。
- `longjmp`函数从`env`缓冲区中恢复调用环境，然后触发一个从最近一次初始化`env`的`setjmp`的调用的返回。然后`setjmp`返回，并带有非零的返回值`retval`。
- 说明：
    - `setjmp`调用一次但返回多次，第一次调用保存调用环境，然后每次`longjmp`都将返回到`setjmp`中来。
    - `longjmp`调用一次，但从不返回。
- 一个重要应用就是从深层嵌套的函数调用中立即返回，通常是检测到某个错误引起，C++的异常就可以使用`setjmp longjmp`实现。
- `longjmp`允许跳过所有中间调用的特性可能产生意味后果，比如内存泄漏。
- 非本地跳转的另一个重要应用是使一个信号处理程序分支到一个特殊的代码位置，而不是返回被信号到达中断了的指令的位置。
- `sigsetjmp siglongjmp`是`setjmp longjmp`的可以被信号处理程序使用的版本。
- 使用`setjmp longjmp`实现的异常机制中，`catch`类似于`setjmp`，`throw`类似于`longjmp`。

## 8.7 操作进程的工具

- `strace`：打印一个正在运行的程序和它的子进程调用的每个系统调用轨迹。
- `ps`：列出系统中的进程，包括僵死进程。
- `top`：打印出关于当前进程资源使用的信息。
- `pmap`：显式进程的内存映射。
- `/proc`：一个虚拟文件系统，以ASCII形式输出大量内核数据结构的内容。用户程序可以读取这些内容，比如`cat /proc/cpuinfo`显式CPU信息。

## 补充资料

- 系统编程：[UNIX环境高级编程](https://book.douban.com/subject/25900403/)
- 操作系统书籍：
    - [操作系统概念](https://book.douban.com/subject/30297919/)
    - [现代操作系统](https://book.douban.com/subject/27096665/)
    - [深入理解Linux内核](https://book.douban.com/subject/1230516/)
