<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [第十二章：并发编程](#%E7%AC%AC%E5%8D%81%E4%BA%8C%E7%AB%A0%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B)
  - [12.1 基于进程的并发编程](#121-%E5%9F%BA%E4%BA%8E%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B)
  - [12.2 基于IO多路复用的并发编程](#122-%E5%9F%BA%E4%BA%8Eio%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8%E7%9A%84%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B)
  - [12.3 基于线程的并发编程](#123-%E5%9F%BA%E4%BA%8E%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B)
  - [12.4 多线程程序中的共享变量](#124-%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%A8%8B%E5%BA%8F%E4%B8%AD%E7%9A%84%E5%85%B1%E4%BA%AB%E5%8F%98%E9%87%8F)
  - [12.5 用信号量同步线程](#125-%E7%94%A8%E4%BF%A1%E5%8F%B7%E9%87%8F%E5%90%8C%E6%AD%A5%E7%BA%BF%E7%A8%8B)
  - [12.6 使用线程提高并行性](#126-%E4%BD%BF%E7%94%A8%E7%BA%BF%E7%A8%8B%E6%8F%90%E9%AB%98%E5%B9%B6%E8%A1%8C%E6%80%A7)
  - [12.7 其他并发问题](#127-%E5%85%B6%E4%BB%96%E5%B9%B6%E5%8F%91%E9%97%AE%E9%A2%98)
    - [可重入性](#%E5%8F%AF%E9%87%8D%E5%85%A5%E6%80%A7)
    - [在多线程程序中使用库函数](#%E5%9C%A8%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%A8%8B%E5%BA%8F%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%BA%93%E5%87%BD%E6%95%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 第十二章：并发编程

前面的例子中：硬件异常处理程序、进程、Linux信号处理都是并发程序的例子。应用级并发在以下情况会很有用：
- 访问慢速IO设备。
- 与人交互。
- 推迟工作降低延迟。
- 服务多个网络客户端。
- 在多核机器上进行并发计算。

现代操作系统中构造并发程序的方式：
- 进程：通过进程间通信与其他进程交互。
- IO多路复用：显式调度自己的控制流。
- 线程。

## 12.1 基于进程的并发编程

并发编程的最简单方式，使用`fork exec waitpid`等接口。
- 进程间共享文件表，但是不共享用户地址空间。
- 独立的地址空间让进程共享状态信息变得困难。为了共享信息，需要使用显式的IPC机制。
- 进程往往比较慢，进程控制和IPC开销很高。
- IPC（进程间通信）：
    - 最基本机制：比如waitpid、信号。
    - 套接字是另一个形式。
    - 还有其他形式：管道，FIFO、共享内存、信号量。

## 12.2 基于IO多路复用的并发编程

即是异步IO，在等待IO时要求内核挂起进程，在一个或者多个事件发生后才返回。

## 12.3 基于线程的并发编程

Linux下标准线程库是pthread：
- 创建线程：`pthread_create`。
- 获取自己的线程ID：`pthread_self`。
- 终止线程：`pthread_exit`。
- 取消线程：`pthread_cancel`。
- 等待线程终止：`pthread_join`。
- 分离线程：`pthread_detach`。

## 12.4 多线程程序中的共享变量

## 12.5 用信号量同步线程

## 12.6 使用线程提高并行性

## 12.7 其他并发问题

### 可重入性

可重入函数是线程安全函数的真子集：可重入函数不会引用任何共享数据。

### 在多线程程序中使用库函数

可以参考文档查看一个函数是否是线程安全的：
- 大多数Linux函数是线程安全的，小部分例外。
- Linux系统提供大多数线程不安全函数的可重入版本，这些函数以后缀`_r`结尾。比如`rand`是线程不安全的，它的可重入版本是`rand_r`。
