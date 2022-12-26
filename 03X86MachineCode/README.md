<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [第三章：程序的机器级表示](#%E7%AC%AC%E4%B8%89%E7%AB%A0%E7%A8%8B%E5%BA%8F%E7%9A%84%E6%9C%BA%E5%99%A8%E7%BA%A7%E8%A1%A8%E7%A4%BA)
  - [3.2 程序编码](#32-%E7%A8%8B%E5%BA%8F%E7%BC%96%E7%A0%81)
  - [3.3 数据格式](#33-%E6%95%B0%E6%8D%AE%E6%A0%BC%E5%BC%8F)
  - [3.4 访问信息](#34-%E8%AE%BF%E9%97%AE%E4%BF%A1%E6%81%AF)
    - [操作数](#%E6%93%8D%E4%BD%9C%E6%95%B0)
    - [数据传送指令](#%E6%95%B0%E6%8D%AE%E4%BC%A0%E9%80%81%E6%8C%87%E4%BB%A4)
    - [压入弹出栈数据](#%E5%8E%8B%E5%85%A5%E5%BC%B9%E5%87%BA%E6%A0%88%E6%95%B0%E6%8D%AE)
  - [3.5 算术与逻辑操作](#35-%E7%AE%97%E6%9C%AF%E4%B8%8E%E9%80%BB%E8%BE%91%E6%93%8D%E4%BD%9C)
    - [加载有效地址](#%E5%8A%A0%E8%BD%BD%E6%9C%89%E6%95%88%E5%9C%B0%E5%9D%80)
    - [一元与二元操作](#%E4%B8%80%E5%85%83%E4%B8%8E%E4%BA%8C%E5%85%83%E6%93%8D%E4%BD%9C)
    - [移位操作](#%E7%A7%BB%E4%BD%8D%E6%93%8D%E4%BD%9C)
    - [特殊算术操作](#%E7%89%B9%E6%AE%8A%E7%AE%97%E6%9C%AF%E6%93%8D%E4%BD%9C)
  - [3.6 控制流](#36-%E6%8E%A7%E5%88%B6%E6%B5%81)
    - [访问条件码](#%E8%AE%BF%E9%97%AE%E6%9D%A1%E4%BB%B6%E7%A0%81)
    - [跳转指令](#%E8%B7%B3%E8%BD%AC%E6%8C%87%E4%BB%A4)
    - [跳转指令如何编码](#%E8%B7%B3%E8%BD%AC%E6%8C%87%E4%BB%A4%E5%A6%82%E4%BD%95%E7%BC%96%E7%A0%81)
    - [用条件跳转实现条件分支](#%E7%94%A8%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC%E5%AE%9E%E7%8E%B0%E6%9D%A1%E4%BB%B6%E5%88%86%E6%94%AF)
    - [用条件传送实现条件分支](#%E7%94%A8%E6%9D%A1%E4%BB%B6%E4%BC%A0%E9%80%81%E5%AE%9E%E7%8E%B0%E6%9D%A1%E4%BB%B6%E5%88%86%E6%94%AF)
    - [循环](#%E5%BE%AA%E7%8E%AF)
  - [3.7 过程](#37-%E8%BF%87%E7%A8%8B)
    - [运行时栈](#%E8%BF%90%E8%A1%8C%E6%97%B6%E6%A0%88)
    - [转移控制](#%E8%BD%AC%E7%A7%BB%E6%8E%A7%E5%88%B6)
    - [数据传送](#%E6%95%B0%E6%8D%AE%E4%BC%A0%E9%80%81)
    - [栈上局部存储](#%E6%A0%88%E4%B8%8A%E5%B1%80%E9%83%A8%E5%AD%98%E5%82%A8)
    - [寄存器中的局部存储](#%E5%AF%84%E5%AD%98%E5%99%A8%E4%B8%AD%E7%9A%84%E5%B1%80%E9%83%A8%E5%AD%98%E5%82%A8)
  - [3.8 数组分配与访问](#38-%E6%95%B0%E7%BB%84%E5%88%86%E9%85%8D%E4%B8%8E%E8%AE%BF%E9%97%AE)
    - [多维数组](#%E5%A4%9A%E7%BB%B4%E6%95%B0%E7%BB%84)
  - [3.9 异质数据结构](#39-%E5%BC%82%E8%B4%A8%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
    - [结构](#%E7%BB%93%E6%9E%84)
    - [联合](#%E8%81%94%E5%90%88)
    - [数据对齐](#%E6%95%B0%E6%8D%AE%E5%AF%B9%E9%BD%90)
  - [3.10 在机器程序中将控制和数据结合起来](#310-%E5%9C%A8%E6%9C%BA%E5%99%A8%E7%A8%8B%E5%BA%8F%E4%B8%AD%E5%B0%86%E6%8E%A7%E5%88%B6%E5%92%8C%E6%95%B0%E6%8D%AE%E7%BB%93%E5%90%88%E8%B5%B7%E6%9D%A5)
    - [理解指针](#%E7%90%86%E8%A7%A3%E6%8C%87%E9%92%88)
    - [使用GDB调试](#%E4%BD%BF%E7%94%A8gdb%E8%B0%83%E8%AF%95)
    - [内存越界引用和缓冲区溢出](#%E5%86%85%E5%AD%98%E8%B6%8A%E7%95%8C%E5%BC%95%E7%94%A8%E5%92%8C%E7%BC%93%E5%86%B2%E5%8C%BA%E6%BA%A2%E5%87%BA)
    - [对抗缓冲区溢出攻击](#%E5%AF%B9%E6%8A%97%E7%BC%93%E5%86%B2%E5%8C%BA%E6%BA%A2%E5%87%BA%E6%94%BB%E5%87%BB)
    - [支持变长栈帧](#%E6%94%AF%E6%8C%81%E5%8F%98%E9%95%BF%E6%A0%88%E5%B8%A7)
  - [3.11 浮点代码](#311-%E6%B5%AE%E7%82%B9%E4%BB%A3%E7%A0%81)
    - [浮点寄存器](#%E6%B5%AE%E7%82%B9%E5%AF%84%E5%AD%98%E5%99%A8)
    - [浮点传送与转换操作](#%E6%B5%AE%E7%82%B9%E4%BC%A0%E9%80%81%E4%B8%8E%E8%BD%AC%E6%8D%A2%E6%93%8D%E4%BD%9C)
    - [过程中的浮点代码](#%E8%BF%87%E7%A8%8B%E4%B8%AD%E7%9A%84%E6%B5%AE%E7%82%B9%E4%BB%A3%E7%A0%81)
    - [浮点运算](#%E6%B5%AE%E7%82%B9%E8%BF%90%E7%AE%97)
    - [定义和使用浮点数常量](#%E5%AE%9A%E4%B9%89%E5%92%8C%E4%BD%BF%E7%94%A8%E6%B5%AE%E7%82%B9%E6%95%B0%E5%B8%B8%E9%87%8F)
    - [浮点数中的位级操作](#%E6%B5%AE%E7%82%B9%E6%95%B0%E4%B8%AD%E7%9A%84%E4%BD%8D%E7%BA%A7%E6%93%8D%E4%BD%9C)
    - [浮点比较操作](#%E6%B5%AE%E7%82%B9%E6%AF%94%E8%BE%83%E6%93%8D%E4%BD%9C)
  - [总结](#%E6%80%BB%E7%BB%93)
  - [补充：不同环境中编译生成的汇编对比](#%E8%A1%A5%E5%85%85%E4%B8%8D%E5%90%8C%E7%8E%AF%E5%A2%83%E4%B8%AD%E7%BC%96%E8%AF%91%E7%94%9F%E6%88%90%E7%9A%84%E6%B1%87%E7%BC%96%E5%AF%B9%E6%AF%94)
  - [补充：C语言内联汇编](#%E8%A1%A5%E5%85%85c%E8%AF%AD%E8%A8%80%E5%86%85%E8%81%94%E6%B1%87%E7%BC%96)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 第三章：程序的机器级表示

配置与环境：
- 环境：x86处理器，64位Linux，gcc编译器（使用gcc 12.1.0版本），`-Og`选项生成代码，汇编风格使用默认的AT&T风格。
- 与64位Windows环境产生的汇编可能会存在一些差异：
    - 64位Windows使用LLP64数据模型，`long`长度是32位，64位类Unix系统使用LLP64，`long`长度是64位。
    - 调用约定存在区别。
- 本章所有测试代码都将写在`test.c`中。
- 执行`make test.s`进行编译生成汇编源文件。
- 执行`make test.o`生成目标文件。
- 执行`make test`生成可执行目标文件。
- 执行`objdump -d test.o`对目标文件反汇编。
- 执行`make clean`清理所有生成文件。

## 3.2 程序编码

代码示例：
```C
long mult2(long, long);
void multstore(long x, long y, long* dest)
{
    long t = mult2(x, y);
    *dest = t;
}
```
- 使用C语言编译生成汇编：
```x86asm
encoding_multstore:
.LFB0:
	.cfi_startproc
	pushq	%rbx
	.cfi_def_cfa_offset 16
	.cfi_offset 3, -16
	movq	%rdx, %rbx
	call	encoding_mult2
	movq	%rax, (%rbx)
	popq	%rbx
	.cfi_def_cfa_offset 8
	ret
	.cfi_endproc
.LFE0:
	.size	encoding_multstore, .-encoding_multstore
	.globl	move_exchange
	.type	move_exchange, @function
```
- 精简后：
```x86asm
encoding_multstore:
	pushq	%rbx
	movq	%rdx, %rbx
	call	encoding_mult2
	movq	%rax, (%rbx)
	popq	%rbx
	ret
```
- 对目标文件反汇编 `objdump -d test.o` 可以得到一样的结果：
```
0000000000000000 <encoding_multstore>:
   0:   53                      push   %rbx
   1:   48 89 d3                mov    %rdx,%rbx
   4:   e8 00 00 00 00          callq  9 <encoding_multstore+0x9>
   9:   48 89 03                mov    %rax,(%rbx)
   c:   5b                      pop    %rbx
   d:   c3                      retq   
```
- X86指令是不定长指令，`.`开头的伪指令都是指导汇编器和链接器工作的微指令，不影响汇编代码的逻辑。
- 反汇编中的指令可能和汇编源文件中存在一些区别，比如指令末尾的`q`，这对指令含义并无影响。

## 3.3 数据格式

不同数据类型，使用同一指令的不同后缀形式，表示操作不同长度的数据：
- 操作字节（byte），后缀 `b`，数据长度1个字节。
- 操作字（word），后缀 `w`，数据长度2个字节。
- 操作双字（double words），后缀 `l`，数据长度8个字节。
- 操作四字（quad words），后缀 `q`，数据长度8个字节。
- 浮点单精度操作，后缀 `s`，数据长度4字节。
- 浮点双精度操作，后缀 `l`，数据长度8字节。
- 浮点操作和整数操作是不同指令，不会混淆。
- 例子：`movb movw movl movq`。

## 3.4 访问信息

16个存储64位值的通用寄存器：

|64位寄存器|32位寄存器|16位寄存器|8位寄存器|用途
|:-:|:-:|:-:|:-:|:-:|
|%rax|%eax|%ax|%al|返回值
|%rbx|%ebx|%bx|%bl|被调用者保存
|%rcx|%ecx|%cx|%cl|第4个参数
|%rdx|%edx|%dx|%dl|第3个参数
|%rsi|%esi|%si|%sil|第2个参数
|%rdi|%edi|%di|%dil|第1个参数
|%rbp|%ebp|%bp|%bpl|被调用者保存（base pointer）
|%rsp|%esp|%sp|%spl|栈指针（stack pointer）
|%r8 |%r8d |%r8w |%r8b |第5个参数
|%r9 |%r9d |%r9w |%r9b |第6个参数
|%r10|%r10d|%r10w|%r10b|调用者保存
|%r11|%r11d|%r11w|%r11b|调用者保存
|%r12|%r12d|%r12w|%r12b|被调用者保存
|%r13|%r13d|%r13w|%r13b|被调用者保存
|%r14|%r14d|%r14w|%r14b|被调用者保存
|%r15|%r15d|%r15w|%r15b|被调用者保存

- 前8个寄存器继承自16位处理器时代（8086），那时每个寄存器只有16位，在其后加上 `l` 表示低8位。
- 到32位时代（IA32），这8个寄存器扩展为32位，前面加上 `e` 表示。
- 到了64位时代（x86-64），这8个寄存器扩展为64位，前面加上 `r` 表示。并且新增了8个寄存器，命令为`r8 ~ r16`，访问低32位、低16位、低8位分别使用后缀`d w b`。
- 在操作寄存器的部分字节与寄存器整体时有两条规则：
    - 生成1字节和2字节数字的指令会保持剩下的字节不变。
    - 生成4字节数字的指令会将高位4个字节置0。
- 其中比较特殊的`%rsp`，保存栈指针，其他寄存器的用法更为灵活。少量指令会使用特定寄存器。
- 有一组标准的编程规范控制着如何使用寄存器管理栈、传递函数参数、从函数返回值、存储局部和临时数据。后续会详述这些惯例。

### 操作数

大多数的指令都有一个或者多个操作数：用于指示操作使用的源数据值或者放置结果的目的位置。操作数有三种类型：
- 立即数：表示这个数本身。使用`$`后跟一个整数，一般是`0x`开头的十六进制整数，也可以是十进制。
- 寄存器：表示寄存器中的内容，可以是寄存器的低位1、2、4字节或者整体8字节。
- 内存引用：表示一个内存地址处保存的值，根据计算内存地址的方式，有多种寻址模式。

操作数格式：
- 对于寄存器`r`，我们用`R[r]`来表示其中存储的值。
- 对于内存引用，我们用`M[Addr]`来表示地址`Addr`处存储的值，至于取几个字节的值，则由具体指令决定。
- 操作数的允许格式如下：

|类型|格式|操作数的值|名称|
|:-:|:-:|:-:|:-:
|立即数|`$Imm`|`Imm`|立即数寻址
|寄存器寻址|`r`|`R[r]`|寄存器寻址
|存储器|`Imm(rb, ri, s)`|`M[Imm + R[rb] + R[ri] * s`|比例变址寻址

- 在内存引用类型的操作数中，`Imm`、`rb`、`ri`和`s`、单独的`s`可以省略，就有了以下寻址方式：
    - `Imm`，表示`M[Imm]`，绝对寻址。
    - `(rb)`，表示`M[R[rb]]`，间接寻址。
    - `Imm(rb)`，表示`M[Imm + R[rb]]`，基址偏移量寻址。
    - `(rb, ri)`和`Imm(rb, ri)`，变址寻址。
    - `(, ri, s)`和`Imm(, ri, s)`和`(rb, ri, s)`，也是比例变址寻址。
    - 其中如果`s`省略则表示为1。
    - `rb`省略则空出来，后面的逗号也必须要写。

### 数据传送指令

我们将许多仅仅是源和目的的操作数类型、大小有不同的同类指令成为**指令类**。

数据传送指令是将数据从一个位置复制到另一个位置的指令，其中最简单的一类是MOV类指令。

**MOV类指令**：
- 简单的数据传送指令：`MOV S D`，将S数据传送到D。
- 源操作数可以是立即数、寄存器或者内存中的数，目的操作数只能是内存地址或者寄存器。
- x86-64加了限制，不能两个操作数都是内存位置。将一个操作数从内存位置复制到另一内存位置需要两条指令，需要先复制到寄存器。
- `movb movw movl movq`传送字节、字、双字、四字。
- `movabsq I R`传送绝对的四字，从I到R，I只能是64位立即数，R只能是寄存器。
- 前面说过`movl`会将高4字节置0。
- 常规的`movq`只能将表示为32位补码数字的立即数作为源操作数，然后符号扩展为64位后放到目的位置。

在将较小的源值复制到较大的目的位置时，则需要使用 **MOVZ和MOVS类指令**：
- 他们都从源复制到寄存器：`MOVZ/MOVS S R`。
- `MOVZ`类是零扩展数据传送指令：`movzbw movzbl movzwl movzbq movzwq`。
- `MOVS`类是符号扩展传送指令：`movsbw movsbl movswl movsbq movswq movslq cltq`。
- 从双字到四字则已经有了`movl`零扩展传送，所以没有`movzlq`。
- 其中`cltq`是将`%eax`符号扩展后`%rax`，仅用于这一个寄存器。效果与`movslq %eax, %rax`完全一致，不过编码更紧凑。

区别：
- 对于低位（1字节、2字节）传送而言，MOV类指令不修改高位，MOVZ类则会零扩展，高位会被修改为0，MOVS会符号扩展。

例子：
```C
long move_exchange(long* xp, long y)
{
    long x = *xp;
    *xp = y;
    return x;
}
```
汇编：
```x86asm
; xp in %rdi, y in %rci, return value in %rax
; long is 8 bytes
move_exchange:
	movq	(%rdi), %rax
	movq	%rsi, (%rdi)
	ret
```
- 其中临时变量被分配到了寄存器`%rax`中，并同时作为返回值。

### 压入弹出栈数据

程序栈：
- 栈指针保存在`%rsp`中，指向栈顶首个数据的首地址。
- 栈从高地址向低地址生长。
- `pushq S`压入数据，等价于：
```x86asm
subq %8, %rsp
movq S, (%rsp)
```
- `popq D`弹出数据，等价于：
```x86asm
movq (%rsp), D
addq $8, %rsp
```

## 3.5 算术与逻辑操作

整数与逻辑操作有很多类：
- 其中只有`leaq`没有其他大小变种，其他都有`b w l q`四个变种：

|指令|效果|描述
|:-:|:-:|:-:
|`leaq S D`| D = &S|加载有效地址，目标只能是寄存器
|`INC D`| D = D+1|自增
|`DEC D`| D = D-1|自减
|`NEG D`| D = -D|取负
|`NOT D`| D = ~D|取反
|`ADD S, D`|D = D+S|加
|`SUB S, D`|D = D-S|减
|`IMUL S, D`|D = D*S|乘
|`XOR S, D`|D = D^S|异或
|`OR S, D`|D = D\|S|或
|`AND S, D`|D = D&S|与
|`SAL k, D`|D = D << k|左移
|`SHL k, D`|D = D << k|左移(等同于SAL)
|`SAR k, D`|D = D >>(A) k|算术右移
|`SHR k, D`|D = D >>(L) k|逻辑右移

### 加载有效地址

- 目的操作数只能是寄存器。
- 很像MOV类指令，不过直接将源操作数的地址计算之后不去内存中取值，而是直接将值存入寄存器。
- 很多时候会被用来进行算术运算：可以用来执行加法和有限形式的乘法（乘以1,2,4,8）。
```C
long p129_leaq_scale(long x, long y, long z)
{
    // x in %rdi, y in %rsi, z in %rdx
    return x + 4*y + 12*z;
}
```
- 汇编：
```x86asm
p129_leaq_scale:
	leaq	(%rdi,%rsi,4), %rax ; x + 4*y
	leaq	(%rdx,%rdx,2), %rdx ; z + 2*z = 3*z
	leaq	(%rax,%rdx,4), %rax ; x + 4*y + 4*(3*z) = x + 4*y + 12*z
	ret
```

### 一元与二元操作

- 一元操作：只有一个操作数，即是源也是目的。很类似于C语言中的`++ -- - ~`运算符。
- 二元操作：第二个操作数是目的操作数，同时作为其中一个源。很类似于C语言中的复合赋值运算。
    - 第一个操作数可以是立即数、寄存器、内存位置，第二个操作数可以是寄存器或者内存位置。
- AT&T表示中，所有既有源又有目的的指令都是源在前，目的在后。

### 移位操作

- `SAL SHL`含义一致。
- `SAR`是算术右移，填充符号位。
- `SHR`是逻辑右移，填充0。
- 位移量可以是立即数，或者放在单字节寄存器`%cl`中（比较特殊只能是这个寄存器）。
- 目的操作数可以是寄存器或者内存位置。
- 例子：
```C
long p131_exercise_3_9_shift_left4_rightn(long x, long n)
{
    x <<= 4;
    x >>= n;
    return x;
}
```
- 汇编
```x86asm
p131_exercise_3_9_shift_left4_rightn:
	movq	%rdi, %rax ; get x
	salq	$4, %rax   ; x <<= 4
	movl	%esi, %ecx ; get n(in 4 bytes)
	sarq	%cl, %rax  ; x >>= n
	ret
```

### 特殊算术操作

64位有符号数或者无符号数相乘需要128位来保存结果才能保证不会溢出，x86-64对128位整数操作提供了有限支持。
- 128位整数成为8字（oct word）。

|指令|效果|描述
|:-:|:-:|:-:|
|`imulq S`|`R[%rdx] : R[%rax] = S * R[%rax]`|有符号乘法
|`mulq S`|`R[%rdx] : R[%rax] = S * R[%rax]`|无符号乘法
|`cqto`|`R[%rdx] : R[%rax] = signed-extend(R[%rax])`|4字转换为8字
|`idivq S`|`R[%rdx] = R[%rdx] : R[%rax] mod S`</p>`R[%rax] = R[%rdx] : R[rax] / S`|有符号除法
|`divq S`|`R[%rdx] = R[%rdx] : R[%rax] mod S`</p>`R[%rax] = R[%rdx] : R[rax] / S`|无符号除法

- 前面的双操作数的`imulq`指令是适用于有符号和无符号乘法，结果是64位。
- 此外，x86-64提供了两条单操作数，但是结果是128位的整数乘法，其中`imulq`是有符号乘法，`mulq`是无符号乘法，结果被保存到`R[%rdx]:R[%rax]`组合而成的128整数中。
- 这两条指令要求一个操作数给出，一个操作数位于`%rax`中，结果的低位存在`%rax`中，高位存在`%rdx`中。
- 当用到128位整数相关运算时，会使用到这些指令。标准C语言不提供128位整数类型，不过可以使用GCC扩展中的`__int128`类型，需要去掉`-pedantic-errors`选项以使用非标准的特性。
- 例子：
```C
// Note: GCC extension, non-standard type __int128
typedef unsigned __int128 uint128_t;
// section 3.5.5, P124, 128bit integer arithmetics
void p134_special_arith_inst_mul(uint128_t *dest, uint64_t x, uint64_t y)
{
    *dest = x * (uint128_t)y;
}
```
- 汇编：
```x86asm
p134_special_arith_inst_mul:
	movq	%rsi, %rax      ; save x to %rax
	mulq	%rdx            ; R[%rdx] : R[%rax] = y * R[%rax]
	movq	%rax, (%rdi)    ; save low 64 bits of result
	movq	%rdx, 8(%rdi)   ; save high 64 bits of result, little endian
	ret
```
- 64位整数的除法也是通过`idvq divq`实现的。
    - 如果是有符号数，则需要先通过`cqto`将64位整数符号扩展到128位。
    - 如果是无符号数，则直接零扩展，将高位(`%rdx`)设置为0。
- 有符号数例子：
```C
void P134_special_airth_inst_remdiv(long x, long y, long *qp, long *rp)
{
    long q = x / y;
    long r = x % y;
    *qp = q;
    *rp = r;
}
```
- 汇编：
```x86asm
;  x in %rdi, y in %rsi, qp in %rdx, %rp in %rcx
P134_special_airth_inst_remdiv:
	movq	%rdi, %rax      ; copy x to %rax
	movq	%rdx, %r8       ; copy qp to %r8
	cqto                    ; signed extend %rax(x) to %rdx:%rax
	idivq	%rsi            ; x/y to %rdx, x%y to %rax
	movq	%rax, (%r8)     ; *qp = x/y
	movq	%rdx, (%rcx)    ; *rp = x%y
	ret
```
- 无符号数：
```C
void p124_sepcial_arith_inst_remdiv_unsigned(unsigned long x, unsigned long y,
    unsigned long* qp, unsigned long* rp)
{
    unsigned long q = x / y;
    unsigned long r = x % y;
    *qp = q;
    *rp = r;
}
```
- 汇编：符号扩展改为零扩展，直接清零即可，有符号除法改为无符号除法。
```x86asm
; x in %rdi, y in %rsi, qp in %rdx, %rp in %rcx
p124_sepcial_arith_inst_remdiv_unsigned:
	movq	%rdi, %rax      ; copy x to %rax
	movq	%rdx, %r8       ; copy qp to %r8
	movl	$0, %edx        ; zero extend %rax(x) to %rdx:%rax / just clear %rdx
	divq	%rsi            ; x/y to %rdx, x%y to %rax
	movq	%rax, (%r8)     ; *qp = x/y
	movq	%rdx, (%rcx)    ; *rp = x%y
	ret
```

## 3.6 控制流

标志寄存器：
- `CF`：进位标志。
- `ZF`：零标志。
- `SF`：符号标志。负数则为1。
- `OF`：溢出标志，补码溢出，正溢出或者负溢出。
- 所有除了`leaq`之外的指令都会根据结果改变标志寄存器。
- 一般来说，一条指令只会设置一部分标志寄存器，而不是所有。比如位运算就不会设置溢出标志，因为不会溢出。

比较和测试指令：
- 比较指令`CMP S1, S2`：基于`S2-S1`，仅设置标志寄存器，行为类似于`SUB`，不过不更新目的寄存器。有`cmpb cmpw cmpl cmpq`不同版本。
- 测试指令`TEST S1, S2`：基于`S1 & S2`，仅设置标志寄存器，行为类似AND，不过不更新目的寄存器，有`testb testw testl testq`不同版本。

### 访问条件码

条件码的使用：
- 条件寄存器的值通常不会直接读取，而是使用它们的组合来表示各种不同的含义。
- 三种常用使用条件码的方法：
    - **根据条件码的某种组合设置一个字节为0或者1**。
    - **条件跳转到程序其他部分**。
    - **条件传送数据**。

条件设置指令：
- 目标必须是一字节寄存器。
- 他们的效果是去上面四个标志的组合来表示特定的含义，具体的组合则略（比如`setl`是`SF ^ OF`时设置）。

|指令|含义|
|:-:|:-:|
|`sete/setz D`|相等/零
|`setne/setnz D`|不等/非零
|`sets D`|负数
|`setns D`|非负数
|`setg/setnle D`|有符号>
|`setge/setnl D`|有符号>=
|`setl/setnge D`|有符号<
|`setle/setng D`|有符号<=
|`seta/setnbe D`|无符号>
|`setae/setnb D`|无符号>=
|`setb/setnae D`|无符号<
|`setbe/setna D`|无符号<=

- 其中对有符号数使用`less greater`的缩写，表示大于、小于。
- 对无符号数使用`above below`的缩写，表示超过、低于。
- 末尾的后缀不表示操作数长度，而是表示具体测试什么。
- 大多数情况下，机器代码对于有符号和无符号运算使用相同的指令，编译器根据源码中的数据类型选择是使用无符号还是有符号条件设置指令来得到不用含义的比较结果。
- 某些机器指令有多个名字，称之为同义名（synonym）。

- 例子：`a < b`。
```C
int p137_control_flow_less(long a, long b)
{
    return a < b;
}
```
- 汇编：
```x86asm
; a in %rdi, b in %rsi
p137_control_flow_less:
	cmpq	%rsi, %rdi ; compare a : b
	setl	%al        ; singed integer less
	movzbl	%al, %eax  ; return
	ret
```
- 有符号整数的情况：
```C
int p127_control_flow_less_unsinged(unsigned long a, unsigned long b)
{
    return a < b;
}
```
- 汇编：
```x86asm
p127_control_flow_less_unsinged:
	cmpq	%rsi, %rdi  ; compare a : b
	setb	%al         ; unsigned interger below
	movzbl	%al, %eax   ; return
	ret
```
- 需要注意的是比较指令的比较顺序，`cmpq`中`a`在`b`后面，但是其实是比较`a`和`b`。

### 跳转指令

不同于顺序执行时一条一条指令的执行过程，跳转指令可以跳转到一个特定位置开始执行：
- 在汇编中，跳转的目的地通常用一个标号指明（lebel，形式如`.LABEL`）。
- 在链接后这些标号会被替换为这个标号的虚拟地址。

无条件跳转：
- `jmp D`指令。
- 目标可以是一个标号：`.LABEL`。称之为直接跳转。
- 也可以是从寄存器或者内存位置中读出的地址，称之为间接跳转。
- 汇编中，直接跳转是使用`.`开始的标号，间接跳转是`*`后跟寄存器或者内存位置。
```x86asm
jmp .L1     ; jump to .L1
jmp *%rax   ; jump to R[%rax]
jmp *(%rax) ; jump to M[R[%rax]]
```

条件跳转：
- 只能是直接跳转。
- 与`SET`类指令的设置条件相匹配。

|指令|含义|
|:-:|:-:
|`je/jz Label`|相等/零
|`jne/jnz Label`|不等/非零
|`js Label`|负数
|`jns Label`|非负数
|`jg/jnle Label`|有符号>
|`jge/jnl Label`|有符号>=
|`jl/jnge Label`|有符号<
|`jle/jng Label`|有符号<=
|`ja/jnbe Label`|无符号>
|`jae/jnb Label`|无符号>=
|`jb/jnae Label`|无符号<
|`jbe/jna Label`|无符号<=

### 跳转指令如何编码

跳转指令的编码：
- 前面的指令的二进制编码对非编译期作者来说并不重要，他们几乎是和汇编一一对应的关系。
- 但是跳转指令则比较特别，跳转指令中的间接跳转也具有确定的编码，但是直接跳转是跳转到标号，而标号在最终的机器码中是不存在的。
- 所以如何表示标号就很重要：
    - 标号有几种不同的编码，其中最常用的是**PC相对编码**。也就是将标号的地址与紧跟在跳转指令后的那条指令地址（这是一种惯例，不是基于当前指令地址）之间的差（也就是偏移量）作为编码。这些地址偏移量可以编码为1、2、4个字节，可正可负。只需要在汇编时确定偏移量，链接时不需要进行修改。
    - 第二种编码是给出绝对地址，由四个字节直接指令目标。有汇编器和链接器选择适当的跳转目标。

例子：
```C++
int p141_encoding_of_jump_inst(int a)
{
    if (a > 0)
    {
        return a + 1;
    }
    return a - 1;
}

```
- 汇编：
```x86asm
p141_encoding_of_jump_inst:
	testl	%edi, %edi
	jg	.L16
	leal	-1(%rdi), %eax
	ret
.L16:
	leal	1(%rdi), %eax
	ret
```

- 目标文件反汇编：
```x86asm
0000000000000093 <p141_encoding_of_jump_inst>:
  93:   85 ff                   test   %edi,%edi
  95:   7f 04                   jg     9b <p141_encoding_of_jump_inst+0x8>
  97:   8d 47 ff                lea    -0x1(%rdi),%eax
  9a:   c3                      retq   
  9b:   8d 47 01                lea    0x1(%rdi),%eax
  9e:   c3                      retq 
```
- 反汇编中，跳转地址使用一个字节`04`的PC相对编码，也就是下一条指令其实地址`97`后移动四个字节，即`9b`。
- 偏移量可正可负，使用补码表示。
- 需要注意当`ret`语句作为跳转语句的目标时，处理器不能正确预测`ret`指令的目的，所以需要在前面加一条`rep`指令，看起来就像这样`rep; ret`，在反汇编中看起来可能像`retz retq`，他们是同义的。这里的`rep`类似于一个占位用的空指令，可以忽略。

### 用条件跳转实现条件分支

条件分支可以用条件跳转要可以用条件传送指令实现，通常的方法是条件跳转：
- 例子：
```C
long lt_cnt = 0;
long ge_cnt = 0;
long p142_absdiff_se(long x, long y)
{
    long result;
    if (x < y)
    {
        lt_cnt ++;
        result = y - x;
    }
    else
    {
        ge_cnt ++;
        result = x - y;
    }
    return result;
}
```
- 汇编：
```x86asm
p142_absdiff_se:
.LFB27:
	cmpq	%rsi, %rdi          ; compare x : y
	jge	.L18                    ; x >= y --> to .L18
	addq	$1, lt_cnt(%rip)    ; lt_cnt ++
	movq	%rsi, %rax
	subq	%rdi, %rax          ; result = y - x
	ret                         ; return result
.L18:
	addq	$1, ge_cnt(%rip)    ; ge_cnt ++
	movq	%rdi, %rax
	subq	%rsi, %rax          ; result = x - y
	ret                         ; return result
```
- 汇编代码几乎可以等价翻译为同样结构的`goto`风格代码：
```C
long p142_gotodiff_se(long x, long y)
{
    long result;
    if (x >= y)
    {
        goto x_ge_y;
    }
    lt_cnt++;
    result = y - x;
    return result;
x_ge_y:
    ge_cnt++;
    result = x - y;
    return result;
}
```
- 其汇编：
```x86asm
p142_gotodiff_se:
	cmpq	%rsi, %rdi
	jge	.L23
	addq	$1, lt_cnt(%rip)
	movq	%rsi, %rax
	subq	%rdi, %rax
	ret
.L23:
	addq	$1, ge_cnt(%rip)
	movq	%rdi, %rax
	subq	%rsi, %rax
	ret
```

对于通用的`if-else`语句：
```
if (test-expr)
    then-statement
else
    else-statement
```
- 可以将其等价翻译为：
```
    t = test-expr;
    if (!t)
    {
        goto false;
    }
    then-statement
    goto done;
false:
    else-statement
done:
```
- 然后将这种C语言结构翻译为相同结构的汇编。当然也可以是条件成立跳转，可以有另一种等等价结构。

### 用条件传送实现条件分支

使用条件跳转可以实现任何种类的分支指令，但是某些特定情况下，可以使用条件传送指令（conditional move）来代替条件跳转以获得更高的性能。

例子：
```C
long p145_absdiff(long x, long y)
{
    long result;
    if (x < y)
    {
        result = y - x;
    }
    else
    {
        result = x - y;
    }
    return result;
}
```
- 汇编（在`-Og`选项编译时不会使用条件选择指令）：
```x86asm
p145_absdiff:
	cmpq	%rsi, %rdi
	jge	.L25
	movq	%rsi, %rax
	subq	%rdi, %rax
	ret
.L25:
	movq	%rdi, %rax
	subq	%rsi, %rax
	ret
```
- 在开启`-O2`优化时的汇编：
```x86asm
p145_absdiff:
	movq	%rsi, %rdx  ; y
	movq	%rdi, %rax  ; x
	subq	%rdi, %rdx  ; y - x in %rdx
	subq	%rsi, %rax  ; x - y in %rax
	cmpq	%rsi, %rdi  ; compare x : y
	cmovl	%rdx, %rax  ; if x < y, move %rdx(y-x) to %rax
	ret                 ; return
```
- 其中的`cmovl`指令在`%rdi > %rsi`时才会将`%rdx`的内容传送`%rax`，否则什么事情也不做。

条件传送指令：

|指令|含义|
|:-:|:-:
|`cmove/cmovz S,R`|相等/零
|`cmovne/cmovnz S,R`|不等/非零
|`cmovs S,R`|负数
|`cmovns S,R`|非负数
|`cmovg/cmovnle S,R`|有符号>
|`cmovge/cmovnl S,R`|有符号>=
|`cmovl/cmovnge S,R`|有符号<
|`cmovle/cmovng S,R`|有符号<=
|`cmova/cmovnbe S,R`|无符号>
|`cmovae/cmovnb S,R`|无符号>=
|`cmovb/cmovnae S,R`|无符号<
|`cmovbe/cmovna S,R`|无符号<=

- 条件传送指令目的地只能是寄存器，可以是16位、32位、64位，汇编器会从目标寄存器名字推断出条件传送的操作数长度。对所有长度都可以使用同一个指令名。
- 通常编译器编译时生成的指令在条件传送前都要对条件进行测试。但是条件传送指令并不并不依赖于前面的测试指令才能工作。条件传送指令仅仅是读取值，然后检查条件码，然后要么更新目的寄存器，要么保持不变。

条件传送指令为什么性能更好：
- 因为现代处理器都通过流水线（pipeline）做到指令级并行，来获得高性能。
- 现代处理器都配备有精密的分支预测逻辑来猜测哪个分支更可能执行，然后在测试指令执行结束前，就已经将该分支的逻辑加载进流水线。如果预测成功则会不受影响地继续执行，如果预测失败则需要丢掉所有已经加载到流水线中的后续指令。从其他分支重新加载指令。
- 预测失败通常会导致15到30个时钟周期的浪费，导致程序性能下降。
- 虽然现代处理器的分支预测有高的准确率了（如果条件非常可预测，几乎所有情况都是某个分支，那么可能能够达到90%以上，如果条件本身不可预测，那么可能就五五开），但是并不是100%。
- 而条件传送指令的使用能够直接消除掉分支，消除分支预测失败的可能性，达到性能最大化。

使用条件传送指令实现分支：
- 考虑条件表达式：`v = test-expr ? then-expr : else-expr`。
- 用条件控制实现：
```C
    if (!test-expr)
        goto false;
    v = then-expr;
    goto done;
false:
    v = else-expr;
done:
```
- 如果使用条件传送实现：
```C
v = then-expr;
ve = else-expr;
t = test-expr;
if (!t) v = ve;
```
- 不是所有条件表达式都可以用条件传送编译的，因为我们可以看到条件传送实现时对两个表达式都进行了求值，但是C语言中条件表达式的语义是只有一个表达式会被求值。
- 如果这两个表达式存在副作用，那么行为就变了，所以只能在内置类型这种编译器能够确定无副作用的情况下才能使用。
- 比如`p ? *p : 0`这种表达式就是有副作用的，就必须用条件跳转来编译。
- 使用条件传送也并不一定都能有性能提升，如果多出来的整个表达式计算代价非常大，超过了分支预测失败的性能惩罚，那么最好还是使用条件转移。
- 总体来说，条件传送提供了一种非常受限的情况下的分支优化手段。

### 循环

首先**do-while循环**：
```C
do
    body-statement;
while(test-expr);
```
- 可以等价为：
```C
loop:
    body-statement;
    t = test-expr;
    if (t)
        goto loop;
```
- 例子：
```C
long p150_factorial_dowhile(long n)
{
    long result = 1;
    do
    {
        result *= n;
        n = n-1;
    } while (n > 1);
    return result;
}
```
- 汇编：
```x86asm
p150_factorial_dowhile:
	movl	$1, %eax    ; result = 1
.L30:                   ; loop:
	imulq	%rdi, %rax  ; result *= n
	subq	$1, %rdi    ; n--
	cmpq	$1, %rdi    ; compare n : 1
	jg	.L30
	ret
```

如何逆向汇编代码：
- 关键是找到程序值和寄存器之间的映射关系。
- 对于复杂的任务，编译器会尝试合并多个操作、对计算进行重组和变形、试图将多个变量映射到一个寄存器上，这都会给逆向工作带来难度。

**while循环**：
- 对于：
```C
while (test-expr)
    body-statement;
```
- 第一种编译策略可以是：
```C
    goto test;
loop:
    body-statement;
test:
    t = test-expr;
    if (t)
        goto loop;
```
- 第二种翻译策略：最开始做一次测试，如果不成立直接结束，然后就是一个do-while模式。这种策略成为`guarded-do`。
```C
t = test-expr
if (!t)
    goto done;
loop:
    body-statement;
    t = test-expr;
    if (t)
        goto loop;
done:
```
- 例子：
```C
long p154_factorial_while(long n)
{
    long result = 1;
    while (n > 1)
    {
        result *= n;
        n--;
    }
    return result;
}
```
- `-Og`选项生成汇编，使用第一种翻译策略：
```x86asm
p154_factorial_while:
	movl	$1, %eax    ; result = 1
	jmp	.L32            ; goto test
.L33:                   ; loop:
	imulq	%rdi, %rax  ; result *= n
	subq	$1, %rdi    ; n--
.L32:                   ; test:
	cmpq	$1, %rdi    ; compare n : 1
	jg	.L33            ; if n > 1, goto loop:
	ret
```
- `-O2`生成汇编，使用第二种翻译策略：
```x86asm
p154_factorial_while:
	movl	$1, %eax    ; result = 1
	cmpq	$1, %rdi    ; compare n : 1
	jle	.L35            ; if n <= 1, goto done
.L34:                   ; loop:
	imulq	%rdi, %rax  ; result *= n
	subq	$1, %rdi    ; n--
	cmpq	$1, %rdi    ; compare n : 1
	jne	.L34            ; if n != 1, goto loop
	ret                 ; return result
.L35:                   ; done:
	ret                 ; return result
```

- 可以看到当优化等级较高时，编译器会选择第二种。利用第二种策略，编译器可以优化初始时的测试，比如认为测试条件总是满足。
- 注意汇编中不一定与C源代码一一对应，某些地方可能是等价的，但是并不一一对应，比如`-O2`优化时有两个`ret`，但是源码中只有一次`return`。

**for循环**：
- 通用形式：
```C
for (init-expr; test-expr; update-expr)
    body-statement;
```
- 等价的`while`循环：
```C
init-expr;
while (test-expr)
{
    body-statement;
    update-expr;
}
```
- 对应的第一种翻译策略：
```C
    init-expr;
    goto test:
loop:
    body-statement;
    update-expr;
test:
    t = test-expr;
    if (t)
        goto loop;
```
- 第二种翻译策略：
```C
    init-expr;
    t = test-expr;
    if (!t)
        goto done;
loop:
    body-statement;
    update-expr;
    if (t)
        goto loop;
done:
```
- 例子：
```C
long p157_factorial_for(long n)
{
    long i;
    long result = 1;
    for (i = 2; i <= n; i++)
    {
        result *= n;
    }
    return result;
}
```
- 第一种翻译策略，`-Og`：
```x86asm
p157_factorial_for:
	movl	$1, %edx    ; result = 1
	movl	$2, %eax    ; i = 2
	jmp	.L35            ; goto test
.L36:                   ; loop:
	imulq	%rdi, %rdx  ; result *= n
	addq	$1, %rax    ; i++
.L35:                   ; test:
	cmpq	%rdi, %rax  ; compare i : n
	jle	.L36            ; if i <= n, goto loop:
	movq	%rdx, %rax
	ret                 ; return result
```
- 第二种翻译策略，`-O2`（某些转换确实令人迷惑）：
```x86asm
p157_factorial_for:
	cmpq	$1, %rdi        ; compare n : 1
	jle	.L40                ; if n <= 1, goto done
	leaq	1(%rdi), %rcx   ; tmp = n + 1
	movl	$1, %edx        ; result = 1
	movl	$2, %eax        ; i = 2
.L39:                       ; loop:
	addq	$1, %rax        ; i++
	imulq	%rdi, %rdx      ; result *= n
	cmpq	%rcx, %rax      ; compare i : tmp(n+1)
	jne	.L39                ; if i != tmp(n+1), goto loop
	movq	%rdx, %rax
	ret                     ; return result
.L40:                       ; done:
	movl	$1, %edx
	movq	%rdx, %rax      ; result = 1
	ret                     ; return result
```

**switch语句**：
- `switch`的翻译可以使用跳转表优化，当开关索引值较多且比较接近时，就可能被翻译为跳转表（jump table）。跳转表的执行速度与开关项的多少无关。
- 跳表的实现原理就是将多个跳转地址存在一个数组中，通过其开关值经过一种统一的处理后得到数组索引，从而取出地址。
- 跳转表存在程序的只读数据区域（`.rodata`，Read-Only Data）。
- 例子：
```C
void p160_switch_example(long x, long n, long* dest)
{
    long val = x;
    switch (n)
    {
    case 100:
        val *= 13;
        break;
    case 101:
        val += 10;
        // fall through
    case 102:
        val += 11;
        break;
    case 103:
    case 104:
        val *= val;
        break;
    default:
        val = 0;
    }
    *dest = val;
}
```
- `-Og`汇编，没有使用跳表优化：
```x86asm
p160_switch_example:
	cmpq	$102, %rsi
	je	.L38
	cmpq	$103, %rsi
	je	.L39
	cmpq	$100, %rsi
	je	.L42
	movl	$0, %edi
.L40:
	movq	%rdi, (%rdx)
	ret
.L42:
	leaq	(%rdi,%rdi,2), %rax
	leaq	(%rdi,%rax,4), %rdi
	jmp	.L40
.L38:
	addq	$10, %rdi
.L39:
	addq	$11, %rdi
	jmp	.L40
```
- 很遗憾，在这个版本，我尝试了`-O1 -O2 -O3 -Os`都没有优化成跳表。
- 但总体来说，优化的原理就是在只读区域创建一个保存标号对应地址的数组，然后在跳转时通过索引找到数组项后使用间接跳转。
- 最终只读数据区域的汇编会像是这样（其中不存在`case`标签的会保存默认标签的标号）：
```x86asm
.Lxxx
    .quad .L30
    .quad .LDef
    .quad .L32
    .quad .L33
    ...
```
- 然后`switch`语句的选择会像是这样：`jmp *.Lxxx(, %rsi, 8)`，其中`%rsi`中保存计算好的下标。
- 每一个`break`语句会生成一个`jmp .Label`的语句跳转到`switch`逻辑外。

## 3.7 过程

也就是函数，所有指令级都在机器指令级对过程提供支持：
- 为了支持过程，需要提供下列机制：
    - 传递控制：进入过程和从过程返回时恰当的PC修改。
    - 传递数据：向过程传递一个或者多个参数，从过程返回值。
    - 分配和释放内存：为过程内的局部变量分配空间，返回前释放这些空间。
- 如何传递数据和分配释放内存作为函数调用约定的一部分，在不同平台不同编译器间存在区别。
- x86-64的过程实现包括一组特殊的指令和一些对机器资源（寄存器和栈内存）使用的约定规则。

### 运行时栈

栈是一个LIFO的数据结构，每个进程拥有一个独立的栈：
- 通过`pushq popq`向栈中存入数据、弹出数据。
- 当x86-64过程需要的存储空间超过寄存器能够存放的大小时，就会在栈上分配空间，这部分被称为过程的栈帧（stack frame）。
- 当过程P调用Q时栈中有什么东西：
```
[高地址]
P的数据
...
返回地址
保存的寄存器
局部变量
参数构造区  <--- %rsp
[低地址]
```
- 返回地址算作P的栈帧，然后直到栈顶都算Q的栈帧。
- 通过寄存器可以最多传递6个参数（`%rdi %rsi %rdx %rsx %r8 %r9`），但是其他的参数需要通过栈来传递。

### 转移控制

控制转义指令：

|指令|功能|
|:-:|:-:|
|`call label`|过程调用
|`call *Operand`|过程调用
|`ret`|从过程返回

- `callq retq`在x86-64中是他们的同义词。
- 其中`call`负责将`call`指令的下一条指令的地址（也就是返回地址）压入栈中，然后将PC设置为过程的地址。和跳转一样，调用也可以是直接或者间接的。
- `ret`负责从栈中弹出返回地址，并把PC设置为这个返回地址。

### 数据传送

除了转移控制之外，过程调用时还需要进行数据的传递：
- 即参数的传递，和从过程返回一个值。
- x86-64中，大部分过程间的数据传送是通过寄存器实现的。
- 前六个参数的默认寄存器是：`%rdi %rsi %rdx %rcx %r8 %r9`，剩下的在栈中传递。
- 例子：
```C++
void p169_passing_parameter(long a1, long *a1p,
                            int a2, int *a2p,
                            short a3, short *a3p,
                            char a4, char * a4p)
{
    // a1 in %rdi, a1p in %rsi
    // a2 in %edx, a2p in %rcx
    // a3 in %r8w, a3p in %r9
    // a4 in (%rsp+8), a4p in (%rsp+16)
    *a1p += a1;
    *a2p += a2;
    *a3p += a3;
    *a4p += a4;
}
```
- 汇编：
```x86asm
p169_passing_parameter:
	movq	16(%rsp), %rax      ; a4p to %rax
	addq	%rdi, (%rsi)        ; *a1p += a1
	addl	%edx, (%rcx)        ; *a2p += a2
	addw	%r8w, (%r9)         ; *a3p += a3
	movl	8(%rsp), %edx       ; a4 to %edx
	addb	%dl, (%rax)         ; *a4p += a4
	ret
```
- 调用方：
```C
void p169_passing_parameter_caller()
{
    long a = 1;
    int b = 2;
    short c = 3;
    char d = 4;
    p169_passing_parameter(1, &a, 2, &b, 3, &c, 4, &d);
    a++;
}
```
- 汇编：
```x86asm
p169_passing_parameter_caller:
	subq	$16, %rsp           ; local storage allocation
	movq	$1, 8(%rsp)         ; a
	movl	$2, 4(%rsp)         ; b
	movw	$3, 2(%rsp)         ; c
	movb	$4, 1(%rsp)         ; d
	leaq	1(%rsp), %rax       ; &d
	pushq	%rax                ; push &d to stack
	pushq	$4                  ; push 4 to stack
	leaq	18(%rsp), %r9       ; &c
	movl	$3, %r8d            ; 3
	leaq	20(%rsp), %rcx      ; &b
	movl	$2, %edx            ; 2
	leaq	24(%rsp), %rsi      ; &a
	movl	$1, %edi            ; a
	call	p169_passing_parameter
	addq	$32, %rsp           ; release allocation of parameters(16 bytes) and local storage(16 bytes)
	ret
```
- 从上面例子可以看出：调用方在调用前需要先传参，如果参数超过6个则从后往前压倒栈中，前6个参数则存放到寄存器，然后才开始调用。执行完成后栈中的参数被释放（通过移动栈指针`addq %32, %rsp`），这里的释放是连同栈中的局部变量存储一起释放的。
- 局部作用域声明变量也是在栈中分配的。
- 通过栈传递参数时，所有的参数大小都向8对齐，即使参数长度小于8。多出来的部分是填充，不会被使用。

### 栈上局部存储

这从上面的例子已经可以看出来了，局部变量会在栈上分配，也会进行字节对齐：
- 不是所有时候局部变量都会在栈上分配，会在栈上分配的常见情况包括：
    - 寄存器不足够存放所有局部变量。
    - 对一个局部变量进行了取地址运算，因此必须要能够产生一个地址（寄存器没有地址）。
    - 局部变量是数据或者结构，此时不能在寄存器中分配。后续讨论这个问题。
- 通过减少栈指针来分配空间，分配的结果将作为当前过程的栈帧的一部分。
- 例子：
```C
long p171_swap_add(long *xp, long *yp)
{
    long x = *xp;
    long y = *yp;
    *xp = y;
    *yp = x;
    return x + y;
}
long p171_caller()
{
    long arg1 = 534;
    long arg2 = 1057;
    long sum = p171_swap_add(&arg1, &arg2);
    long diff = arg1 - arg2;
    return sum * diff;
}
```
- 汇编：
```x86asm
p171_swap_add:
	movq	(%rdi), %rax    ; x in %rax
	movq	(%rsi), %rdx    ; y in %rdx
	movq	%rdx, (%rdi)
	movq	%rax, (%rsi)
	addq	%rdx, %rax
	ret
p171_caller:
	subq	$16, %rsp       ; local storage for arg1 and arg2
	movq	$534, 8(%rsp)   ; arg1 = 534
	movq	$1057, (%rsp)   ; arg2 = 1057
	movq	%rsp, %rsi      ; &arg2
	leaq	8(%rsp), %rdi   ; &arg1
	call	p171_swap_add
	movq	8(%rsp), %rdx
	subq	(%rsp), %rdx    ; diff = arg1 - arg2
	imulq	%rdx, %rax      ; sum * diff
	addq	$16, %rsp       ; release local storage
	ret
```
- 栈上的多个变量相互之间的顺序可能会被重排以提高空间利用率，但初始化顺序不会变。

### 寄存器中的局部存储

寄存器组是所有过程共享的资源，给定时刻只有一个过程是活动的，但我们仍然需要取保当一个过程调用另一个过程时，被调不会覆盖主调正在使用的寄存器值：
- 为此，x86-64采用一组统一的寄存器使用惯例：
    - `%rbx %rbp`和`%r12 ~ %r15`划分为**被调用者保存寄存器**（callee-saved registers）。
    - 也就是说当过程P调用过程Q时，被调Q必须保证他们的值在调用时和返回时是一样的。
    - 如果被调要使用这些寄存器，那么需要将他们先压栈，并在返回前将其原样弹出。
    - 所有其他寄存器，除了栈指针`%rsp`，都分类为**调用者保存寄存器**（caller-saved registers）。
    - 也就是说如果使用了这些寄存器，那么他们需要由调用者在调用其他函数前保存，调用任何函数都有可能修改这些寄存器中的值。被调会假定这些寄存器都已经被妥善保存，可以向其写入数据覆盖。
- 例子：
```C
long p173_Q(long x)
{
    return x;
}
long p173_P(long x, long y)
{
    long u = p173_Q(y);
    long v = p173_Q(x);
    return u + v;
}
```
- 汇编：
```x86asm
p173_P:
	pushq	%rbp        ; save %rbp
	pushq	%rbx        ; save %rbx
	movq	%rdi, %rbp  ; save x
	movq	%rsi, %rdi  ; move y to first argument
	call	p173_Q
	movq	%rax, %rbx  ; save result
	movq	%rbp, %rdi  ; move x to first argument
	call	p173_Q
	addq	%rbx, %rax  ; add saved Q(y) to Q(x)
	popq	%rbx        ; restore %rbx
	popq	%rbp        ; restore %rbp
	ret
```
- 函数中用到了被调用者保存寄存器`%rbx %rbp`，所以需要由调用者保存和恢复。
- 而如果要用进行函数，但用于传参的寄存器却被占用，也需要先保存到其他寄存器。
- 例子2：
```C
void p174_inner();
long p174_outter(long a, long b, long c, long d, long e, long f)
{
    p174_inner(1, 2);
    return a + b + c + d + e + f;
}
```
- 汇编：
```x86asm
p174_outter:
	pushq	%r13        ; save registers
	pushq	%r12
	pushq	%rbp
	pushq	%rbx
	subq	$8, %rsp
	movq	%rdi, %rbx  ; save parameters a, b, c, d
	movq	%rsi, %r13
	movq	%rdx, %r12
	movq	%rcx, %rbp
	movl	$2, %esi
	movl	$1, %edi
	movl	$0, %eax
	call	p174_inner
	leaq	(%rbx,%r13), %rax
	addq	%r12, %rax
	addq	%rbp, %rax
	addq	$8, %rsp
	popq	%rbx
	popq	%rbp
	popq	%r12
	popq	%r13
	ret
```
- 从上面的例子看出，6个保存参数的寄存器同样是调用者保存寄存器，如果函数中调用了函数，那么这个函数就可能发生任何事，必须假定函数中会修改所有调用者保存寄存器。这些调用者保存寄存器就必须在调用前得到妥善保存。
- 而为了保存这些调用者保存寄存器，最好的方式就是将其保存到被调用者保存寄存器中。而保存到被调用者保存寄存器中前，当前函数同样作为被调，所以需要先将被调用者保存寄存器先妥善保存，然后将调用者保存寄存器保存到这些被调用者保存寄存器中。
- 参数寄存器保存后，在函数调用完成后是没有必要将其还原到原来的参数寄存器中的，无论在那个寄存器中，都可以参与运算。
- 如果某个参数在函数调用后没有被使用，那么就可以不用保存（`p174_outter`的`e f`参数）。

总结：
- 一个函数调用另一个函数前，需要保存所有在函数调用后还会使用到的调用者保存寄存器（包括参数寄存器）。通常方式是将这些寄存器保存到被调用者保存寄存器中（但不一定都能存下，也可能保存到栈中），在函数调用返回后一般不需要恢复，值直接被移动了。
- 一个函数中如果用到了被调用者保存寄存器，那么进入函数后需要先将其保存，从函数返回前需要将其恢复。保存方式通常来说都是压入栈中。
- 调用者保存寄存器`%rax %rcx %rdx %rsi %rdi %r8 %r9 %r10 %r11`，共9个。

## 3.8 数组分配与访问

- 对于数组：`T A[N]`。
- x86-64的内存引用指令可以用来简化数组访问，伸缩引子1、2、4、8覆盖了所有简单数据类型的大小：
- 访问元素`A[i]`可以使用`movq (A, i, sizeof(T)), target`实现。
- 取元素`A[i]`地址则可以用`leaq (A, i, sizeof(T)), target`实现。

### 多维数组

对于数组：`T D[R][C]`。
- 元素`D[i][j]`的地址是`D + sizeof(T)*(C * i + j)`。
- 通过计算这个式子即可得到元素地址。


## 3.9 异质数据结构

C语言支持两种将不同类型对象组合到一起的方法：结构（struct）和联合（union）。

### 结构

结构通过结构首地址，也就是第一个成员地址，加上该字段偏移即可得到成员地址，然后正常手法取值即可。
- 例子：
```C
struct rec
{
    int i;      // offset 0
    int j;      // offset 4
    int a[2];   // offset 8
    int *p;     // offset 16
};
void p185_test_struct(struct rec* r)
{
    r->p = &r->a[r->i + r->j];
}
```
- 汇编：
```x86asm
; r in %rdi
p185_test_struct:
	movl	4(%rdi), %eax           ; r->j
	addl	(%rdi), %eax            ; r->i + r->j
	cltq                            ; signed extend %eax to 8 bytes(%rax)
	leaq	8(%rdi,%rax,4), %rax    ; &r->a[r->i + r->j]
	movq	%rax, 16(%rdi)          ; save to r->p
	ret
```

### 联合

联合使用不同字段引用相同内存块，所有字段的地址都等于首地址：
- 联合在某些地方会很有用，它绕过了类型系统的检查。
- 但大多数时候使用联合都不是一个好选择。

### 数据对齐

大多数计算机系统都对基本数据类型的合法地址做了限制，要求必须是类型的整数倍：
- 这给硬件接口的实现带来了好处。
- x86-64无论是否对齐都能工作，但还是推荐对齐，对齐数据可以获得更高的性能。
- 对齐原则就是任何基本数据类型地址都必须是其大小的整数倍。
- 对于包含结构的代码，或者多个不同大小数据放一起（比如放在栈中的局部多个局部变量），编译器会在其中加入必要的填充（padding）以保证每个数据的对齐。通过少量的空间损失获得更好的性能。
- 汇编指令`.align 8`可以指定对齐要求。
- MSVC编译器中允许使用`#pragma pack()`指定对齐大小。
- C11开始可以使用关键字`_Alignas`显示指定对齐大小，C++中有`alignas`关键字。
- x86-64的某些超标量指令扩展（比如AVX、SSE）对对齐有强制要求，需要对齐才能正确计算。
- 结构的对齐规则是每个成员都按照其对齐大小对齐，并且结构大小能够整除最大的成员对齐大小，结构的对齐大小就是最大的成员对齐大小。

## 3.10 在机器程序中将控制和数据结合起来

### 理解指针

就是地址，不用过多介绍。

### 使用GDB调试

使用`-g`编译，`gdb prog`开始调试。略，现在我们都使用图形化调试工具。

### 内存越界引用和缓冲区溢出

例子：
```C
void p195_do_something(char* buf);
void p195_buffer_overflow()
{
    char buf[8];
    p195_do_something(buf);
}
```
- 汇编：
```x86asm
p195_buffer_overflow:
	subq	$24, %rsp           ; allocate memory
	leaq	8(%rsp), %rdi       ; buf is at 8%(rsp)
	call	p195_do_something
	addq	$24, %rsp           ; release
	ret
```
- C语言中并没有内建的数组下标越界检查，所有检查都需要程序员来做。
- 如果数组在栈上分配，比如上面的例子，那么越界写入就会导致栈破坏，轻则程序宕掉，重则数据破坏。
- 这一点还可能被黑客利用，将恶意代码编码为字符串，覆盖掉函数返回地址，函数返回时就会跳转到攻击者想要执行的位置。然后攻击者执行完指令后再返回到原来的调用者，看起来无事发生，其实已经受到了攻击。
- 任何外部接口都应该避免可能的缓冲区溢出攻击。

### 对抗缓冲区溢出攻击

有一些手段可以避免通过缓冲区溢出的攻击：

栈随机化：
- 为了避免攻击者制作出对多台机器都有效的病毒。可以将栈随机化，而不是所有机器的栈都拥有相同的地址。
- 栈随机化的思想是使栈的位置在程序每次运行期间都有变化。让攻击者找不到真正的栈在什么位置，需要在什么位置进行攻击。
- 典型实现手段可以是在栈上分配一个随机数量的空间，但不使用这个空间，而是跳过。
- Linux系统中，栈随机化已经是标准行为。
- 但执着的攻击者可能会使用暴力克服随机化，即反复对不同的地址进行尝试攻击。

栈破坏检测：
- 可以通过在任何局部缓冲区和其他数据之间存储一个特殊的金丝雀（canary）值作为哨兵，在函数调用返回前进行检查，如果这个值被更改，说明缓冲区溢出了。
- 这个值是程序每个运行时随机产生的，攻击者无法获取。
- GCC`-fstack-protector`选项即可插入栈保护代码，上面的`p195_buffer_overflow`加入栈保护后的结果：
```x86asm
p195_buffer_overflow:
	pushq	%rbp
	movq	%rsp, %rbp
	subq	$16, %rsp
	movq	%fs:40, %rax        ; read a value from memory
	movq	%rax, -8(%rbp)      ; and store it to stack
	xorl	%eax, %eax
	leaq	-16(%rbp), %rax
	movq	%rax, %rdi
	call	p195_do_something
	nop
	movq	-8(%rbp), %rax
	subq	%fs:40, %rax
	je	.L69
	call	__stack_chk_fail    ; check the value stored in stack
.L69:
	leave
	ret
```

限制可执行代码区域：
- 通过限制哪些内存区域才能够存放可执行代码，可以消除攻击者向系统中插入可执行代码的能力。
- 比如内存分页的标志（读、写、执行等）。
- 通过同时使用多种方式，可以显著降低被攻击的可能。
- 更好的方式还是在所有对外接口中对可写入缓冲区的数据大小做限制。

### 支持变长栈帧

C99引入变长数组VLA，数组长度需要在执行时才能知道，此时就需要有支持变长栈帧的能力：
- 前面的栈内存分配都是固定大小，大小都是硬编码到机器码中作为立即数的。
- 为了支持变长栈帧，就需要将大小存到一个寄存器中，分配时通过访问寄存器值来确定栈指针偏移多少，释放时也是同样。

## 3.11 浮点代码

处理器的浮点体系结构包含多个方面：
- 如果存储和访问浮点数，通常是通过某种寄存器。
- 浮点数据操作指令。
- 向函数传递浮点参数以及从函数返回浮点数的规则。
- 函数调用过程中保存寄存器的规则，例如调用者和被调用者保存寄存器。

Intel x86-64的浮点指令集发展历史：
- 最早是1980年发布的[8087](https://zh.wikipedia.org/wiki/Intel_8087)浮点协处理器，用来协助8086处理器（只支持整数计算）处理浮点计算。由此建立了x86系列的浮点模型，称之为x87。后续还有80287、80387等。
- 直到1993年[奔腾](https://zh.wikipedia.org/wiki/%E5%A5%94%E9%A8%B0)时代，浮点协处理器被集成到了CPU内部。
- 1996年后，奔腾处理器中增加了一类处理整数和浮点的向量指令，称之为[MMX](https://zh.wikipedia.org/wiki/MMX)，MultiMedia eXtensions，多媒体扩展。
- 1999年奔腾-III处理器上推出[SSE](https://zh.wikipedia.org/wiki/SSE)指令，Streaming SIMD Extensions，流式单指令多数据扩展。还有后来的SSE2等升级，直到SSE5。
- 2008年，Intel提出[AVX](https://zh.wikipedia.org/wiki/AVX%E6%8C%87%E4%BB%A4%E9%9B%86)指令集，高级向量扩展指令集，Advanced Vector Extensions。经过AVX1.1，AVX2，现已发展到了AVX-512。

这里讨论x86-64指令集，现代的x64位处理上几乎都支持AVX2，所以这里将基于AVX-2：
- 并且主要是标量指令。
- 如果需要了解浮点向量指令或者其他浮点指令扩展，可以自行查询资料。
- GCC编译器中编译时加入选项`-mavx2`可以生成AVX-2的代码。

### 浮点寄存器

SSE寄存器`%xmm0 ~ %xmm15`是128位的，AVX寄存器`%ymm0 ~ %ymm15`是256位的，其中其中就是后者的低位。

|256位AVX寄存器|128位SSE寄存器|用途
|:-:|:-:|:-:
|`%ymm0`|`%xmm0`|第1个浮点参数
|`%ymm1`|`%xmm1`|第2个浮点参数
|`%ymm2`|`%xmm2`|第3个浮点参数
|`%ymm3`|`%xmm3`|第4个浮点参数
|`%ymm4`|`%xmm4`|第5个浮点参数
|`%ymm5`|`%xmm5`|第6个浮点参数
|`%ymm6`|`%xmm6`|第7个浮点参数
|`%ymm7`|`%xmm7`|第8个浮点参数
|`%ymm8`|`%xmm8`|调用者保存
|`%ymm9`|`%xmm9`|调用者保存
|`%ymm10`|`%xmm10`|调用者保存
|`%ymm11`|`%xmm11`|调用者保存
|`%ymm12`|`%xmm12`|调用者保存
|`%ymm13`|`%xmm13`|调用者保存
|`%ymm14`|`%xmm14`|调用者保存
|`%ymm15`|`%xmm15`|调用者保存

如果只使用低32位或者64位，可以使用`%xmm0 ~ %xmm15`来访问，具体数据大小则通过指令区分。

### 浮点传送与转换操作

浮点传送指令：

|指令|源|目的|描述
|:-:|:-:|:-:|:-
|`vmovss`|M32|X|传送单精度浮点数
|`vmovss`|X|M32|传送单精度浮点数
|`vmovsd`|M64|X|传送双精度浮点数
|`vmovsd`|X|M64|传送双精度浮点数
|`vmovaps`|X|X|传送对齐的封装好的单精度浮点数
|`vmovapd`|X|X|传送对齐的封装好的双精度浮点数

- 无论对齐与否，这些指令都能工作，但是建议32位数据按照4字节对齐，64位数据按照8字节对齐，能够避免性能损失。
- `a`表示aligned对齐的意思。
- 例子：
```C
float p206_floating_point_passing(float v1, float *src, float *dest)
{
    float v2 = *src;
    *dest = v1;
    return v2;
}
```
- 汇编：
```x86asm
; v1 in %xmm0, src in %rdi, dest in %rsi
p206_floating_point_passing:
	vmovaps	%xmm0, %xmm1        ; copy v1
	vmovss	(%rdi), %xmm0       ; read v2 from src
	vmovss	%xmm1, (%rsi)       ; write v1 to dest
	ret                         ; return v2 in %xmm0
```

浮点数与整数间的转换指令：

- 双操作数浮点转换：浮点数到整数。

|指令|源|目的|描述
|:-:|:-:|:-:|:-
|`vcvttss2si`|X/M32|R32|用截断的方法把单精度浮点数转换为32位整数
|`vcvttsd2si`|X/M64|R32|用截断的方法把双精度浮点数转换为32位整数
|`vcvttss2siq`|X/M32|R64|用截断的方法把单精度浮点数转换为64位（四字）整数
|`vcvttsd2siq`|X/M64|R64|用截断的方法把双精度浮点数转换为64位（四字）整数

- 三操作数浮点转换：整数到浮点数。

|指令|源1|源2|目的|描述
|:-:|:-:|:-:|:-:|:-
|`vcvtsi2ss`|M32/R32|X|X|32位整数转单精度浮点数
|`vcvtsi2sd`|M32/R32|X|X|32位整数转双精度浮点数
|`vcvtsi2ssq`|M64/R64|X|X|64位（四字）整数转单精度浮点数
|`vcvtsi2sdq`|M64/R64|X|X|64位（四字）整数转双精度浮点数

- 其中整数转浮点数是不太常见的三操作数格式，有两个源和一个目的，第一个操作数读自一个通用寄存器或者内存，第二个操作数只会影响高位字节，这里可以忽略。
- 在最常见的使用场景中，第二个源和目的是一样的：
```x86asm
vcvtsi2sdq %rax, %xmm1, %xmm1
```
- 例子：
```C
void p207_floating_integer_conversion(int a, long b, float *fp, double *dp, int *ip, long *lp)
{
    *fp = a; // int to float
    *fp = b; // long to float
    *dp = a; // int to double
    *dp = b; // long to double
    *ip = *fp; // float to int
    *lp = *fp; // float to long
    *ip = *dp; // double to int
    *lp = *dp; // double to long
}
```
- 汇编：
```x86asm
; a int %edi, b in %rsi, fp in %rdx, dp in %rcx, ip in %r8, lp in %r9
p207_floating_integer_conversion:
	vxorps	%xmm0, %xmm0, %xmm0
	movq	%rcx, %rax              ; dp in %rax
	vcvtsi2ssl	%edi, %xmm0, %xmm1  ; a to float in %xmm1
	vmovss	%xmm1, (%rdx)           ; *fp = a
	vcvtsi2ssq	%rsi, %xmm0, %xmm1  ; b to float in %xmm1
	vmovss	%xmm1, (%rdx)           ; *fp = b
	vcvtsi2sdl	%edi, %xmm0, %xmm1  ; a to double in %xmm1
	vmovsd	%xmm1, (%rcx)           ; *dp = a
	vcvtsi2sdq	%rsi, %xmm0, %xmm0  ; b to double in %xmm0
	vmovsd	%xmm0, (%rcx)           ; *dp = b
	vcvttss2sil	(%rdx), %ecx        ; *fp to int in %ecx
	movl	%ecx, (%r8)             ; *ip = *fp
	vcvttss2siq	(%rdx), %rdx        ; *fp to long in %rdx
	movq	%rdx, (%r9)             ; *lp = *fp
	vcvttsd2sil	(%rax), %edx        ; *dp to int in %edx
	movl	%edx, (%r8)             ; *ip = *dp
	vcvttsd2siq	(%rax), %rax        ; *dp to long in %rax
	movq	%rax, (%r9)             ; *lp = *dp
	ret
```

浮点类型之间的转换指令：
- 同理其实还有三操作数的`vcvtss2sd vcvtsd2ss`来进行单精度和双精度浮点数之间的转换。
- 例子：
```C
double p207_float_double_conversion(float *fp, double *dp)
{
    *fp = *dp;
    *dp = *fp;
    return *dp;
}
```
- 汇编：
```x86asm
; fp in %rdi, dp in %rsi
p207_float_double_conversion:
	vxorps	%xmm0, %xmm0, %xmm0
	vcvtsd2ss	(%rsi), %xmm0, %xmm0    ; *dp to float in %xmm0
	vmovss	%xmm0, (%rdi)               ; *fp = *dp
	vcvtss2sd	%xmm0, %xmm0, %xmm0     ; *fp to double in %xmm0
	vmovsd	%xmm0, (%rsi)               ; *dp = *fp
	ret                                 ; return value is in %xmm0
```

### 过程中的浮点代码

XMM寄存器可以用来向函数传递浮点参数，以及从函数返回浮点值：
- XMM寄存器可以通过`%xmm0 ~ %xmm7`最多传递8个寄存器，按照参数列出顺序使用这些寄存器。超过8个后，会使用栈来传递。
- 使用寄存器`%xmm0`来返回浮点数。
- 所有XMM寄存器都是调用者保存的，被调用者不用保存覆盖这些寄存器就可以使用。
- 所有参数中除去浮点数之外的参数仍旧按照整数参数的参数传递顺序使用寄存器或者栈传递。

例子：
```C
void p210_passing_floating_parameters(float a, double b, float c, long d, double *p,
    float e, double f, float g, double h, double i, double j, double k);
void p210_passing_floating_caller()
{
    p210_passing_floating_parameters(1.0f, 2.0, 3.1f, 10L, 0,
        4.2f, 5.3, 6.4f, 7.5, 8.6, 9.7, 10.8);
}
```
- 汇编：
```x86asm
; d in %rdi, p in %rsi
; a - i in %xmm0 ~ %xmm7
; j, k in stack
p210_passing_floating_caller:
	subq	$8, %rsp
	pushq	$-1717986918
	movl	$1076205977, 4(%rsp)
	pushq	$1717986918
	movl	$1076061798, 4(%rsp)
	vmovsd	.LC0(%rip), %xmm7
	vmovsd	.LC1(%rip), %xmm6
	vmovss	.LC6(%rip), %xmm5
	vmovsd	.LC2(%rip), %xmm4
	vmovss	.LC7(%rip), %xmm3
	movl	$0, %esi
	movl	$10, %edi
	vmovss	.LC8(%rip), %xmm2
	vmovsd	.LC3(%rip), %xmm1
	vmovss	.LC9(%rip), %xmm0
	call	p210_passing_floating_parameters
	addq	$24, %rsp
	ret
.LC0:
	.long	858993459
	.long	1075917619
	.align 8
.LC1:
	.long	0
	.long	1075707904
	.align 8
.LC2:
	.long	858993459
	.long	1075131187
	.align 8
.LC3:
	.long	0
	.long	1073741824
	.section	.rodata.cst4,"aM",@progbits,4
	.align 4
.LC6:
	.long	1087163597
	.align 4
.LC7:
	.long	1082549862
	.align 4
.LC8:
	.long	1078355558
	.align 4
.LC9:
	.long	1065353216
	.ident	"GCC: (GNU) 12.1.0"
	.section	.note.GNU-stack,"",@progbits
```
- 编译时，浮点数常量被编译为了对应二进制位对应的整数，以加载到栈中或者寄存器中。看汇编时会比较难以理解。

### 浮点运算

浮点运算指令有一个或两个操作数，一个目的：

|单精度|双精度|效果|描述
|:-:|:-:|:-:|:-
|`vaddss`|`vaddsd`|`D = S1 + S2`|浮点数加法
|`vsubss`|`vsubsd`|`D = S1 - S2`|浮点数减法
|`vmulss`|`vmulsd`|`D = S1 * S2`|浮点数乘法
|`vdivss`|`vdivsd`|`D = S1 / S2`|浮点数除法
|`vmaxss`|`vmaxsd`|`D = max(S1, S2)`|浮点数最大值
|`vminss`|`vminsd`|`D = min(S1, S2)`|浮点数最小值
|`sqrtss`|`sqrtsd`|`D = sqrt(S1)`|浮点数平方根

- 其中第一个源操作数可以是XMM寄存器或者内存位置，第二个操作数和目的操作数都只能是XMM寄存器。
- 例子：
```C
double p210_floating_point_airthmetics(double a, float x, double b, int i)
{
    return a*x - b/i;
}
```
- 汇编：
```x86asm
; a in %xmm0, x in %xmm1, b in %xmm2, i in %edi
p210_floating_point_airthmetics:
	vcvtss2sd	%xmm1, %xmm1, %xmm1     ; x to double
	vmulsd	%xmm0, %xmm1, %xmm1         ; a*x to %xmm1
	vxorps	%xmm0, %xmm0, %xmm0
	vcvtsi2sdl	%edi, %xmm0, %xmm0      ; i to double in %xmm0
	vdivsd	%xmm0, %xmm2, %xmm2         ; b/i in %xmm2
	vsubsd	%xmm2, %xmm1, %xmm0         ; a*x - b/i to %xmm0
	ret
```

### 定义和使用浮点数常量

与整数运算不同，没有浮点立即数这种东西，浮点数常量会按照其二进制位编译为等价的整数，并按照浮点数来解释二进制位：
- 并且浮点数常量会被放到常量区，而不是直接编码到指令中。
- 例子：
```C
double p210_floating_point_constant(double temp)
{
    return 1.8 * temp + 32.1;
}
```
- 汇编：
```x86asm
p210_floating_point_constant:
	vmulsd	.LC10(%rip), %xmm0, %xmm0
	vaddsd	.LC11(%rip), %xmm0, %xmm0
	ret
	.align 8
.LC10:  ; 1.8, little endian
	.long	-858993459  ; low order 4 bytes of 1.8
	.long	1073532108  ; high order 4 bytes of 1.8
	.align 8
.LC11:  ; 32.1
	.long	-858993459
    .long	1077939404
```

### 浮点数中的位级操作

浮点数也可以进行按位运算，虽然可能没有什么意义，不过前面也出现过，可能会用来对一个寄存器进行清零操作等：

|单精度|双精度|效果|描述
|:-:|:-:|:-:|:-
|`vxorps`|`xorpd`|`D = S2 ^ S1`|按位异或（XOR）
|`vandps`|`andpd`|`D = S2 & S1`|按位与（AND）

### 浮点比较操作

AVX2提供了两条浮点数比较指令：

|指令|基于|描述
|:-:|:-:|:-
|`vucomiss S1, S2`|`S2 - S1`|单精度浮点数比较
|`vucomisd S1, S2`|`S2 - S1`|双精度浮点数比较

浮点数比较会设置三个条件码：零标志ZF，进位标志CF，奇偶标志PF。

|顺序S2:S1|CF|ZF|PF
|:-:|:-:|:-:|:-:
|无序（其中一个是NaN）|1|1|1
|`S2 < S1`|1|0|0
|`S2 = S1`|0|1|0
|`S2 > S1`|0|0|0

除了无序结果以外，比较结果的标志和无符号数一致，所以需要使用无符号数的条件跳转指令：`ja je jb`，无需结果在C语言中只能通过`else`得到：
- 例子：
```C
int p214_floating_point_comparison(double x)
{
    int result;
    if (x > 0)
    {
        result = 0;
    }
    else if (x == 0)
    {
        result = 1;
    }
    else if (x < 0)
    {
        result = 2;
    }
    else
    {
        result = 3;
    }
    return result;
}
```
- 汇编：
```x86asm
p214_floating_point_comparison:
	vxorpd	%xmm1, %xmm1, %xmm1
	vcomisd	%xmm1, %xmm0
	jbe	.L74
	movl	$0, %eax
	ret
.L74:
	vucomisd	%xmm1, %xmm0
	jp	.L71
	jne	.L71
	movl	$1, %eax
	ret
.L71:
	vxorpd	%xmm1, %xmm1, %xmm1
	vcomisd	%xmm0, %xmm1
	ja	.L75
	movl	$3, %eax
	ret
.L75:
	movl	$2, %eax
	ret
```

## 总结

- C语言到汇编的映射相对来说是简单直接的。
- 但在高优化等级下，生成的汇编可能非常不易读，因为进行了很多代码转换和优化。
- C++的编译也是同理，不过C++中会有名称修饰，ABI会比较不一样，会复杂很多。比如对象的拷贝是通过调用拷贝构造函数，而不是直接能用一条指令完成，并且编译器会自动插入非平凡析构类型对象的析构调用，还有许多C++特性会增加汇编的复杂性，比如虚表等。
- 一般来说没有比较了解太深，除非要写编译器。

## 补充：不同环境中编译生成的汇编对比

环境：64位Windows环境下的MinGW-W64（GCC 12.1.0）以及64位Linux环境下的原生GCC 12.1.0。
- 分析调用约定：
    - 各种调用约定下，谁负责清理堆栈。
    - 参数放在哪些寄存器，参数顺序之类。
    - 数据长度差异。
- 例子：
```C
void test_of_parameters(char a, short b, int c, long d,
    char* pa, short *pb, int *pc, long* pd)
{
    *pa = a;
    *pb = b;
    *pc = c;
    *pd = d;
}
```
- Linux汇编：
```x86asm
; a in %dil, b in %si, c in %edx, d in %rcx
; pa in %r8, pb in %r9, pc in 8(%rsp), pd in 16(%rsp)
; note: (%rsp) is return address
test_of_parameters:
	movb	%dil, (%r8)
	movw	%si, (%r9)
	movq	8(%rsp), %rax
	movl	%edx, (%rax)
	movq	16(%rsp), %rax
	movq	%rcx, (%rax)
	ret
```
- Windows汇编：
```x86asm
; a in %cl, b in %dx, c in %r8d, d in %r9d
; pa in 40(%rsp), pb in 48(%rsp), pc in 56(%rsp), pd in 64(%rsp)
test_of_parameters:
	movq	40(%rsp), %rax
	movb	%cl, (%rax)
	movq	48(%rsp), %rax
	movw	%dx, (%rax)
	movq	56(%rsp), %rax
	movl	%r8d, (%rax)
	movq	64(%rsp), %rax
	movl	%r9d, (%rax)
	ret
```

结论：
- 不同系统，不同编译器上调用约定不同，怎样传参的约定也就不一样，本章的所有东西都是基于Linux的，Windows上会有区别，但是道理是相同的。
- 至于具体的调用约定则有需要再探讨。

## 补充：C语言内联汇编

C语言中可以通过`asm`关键字引入内联汇编，在C源程序中嵌入内联汇编，不过内联汇编没有固定标准，在C中被当做扩展，可以不支持：
- GCC中可以通过`__asm__`关键字引入，而不是`asm`。而MSVC的关键字是`__asm`。
- 例子：应当使用在64位Linux环境中的GCC编译。
```C
int inline_assembly_test(int x, int y)
{
    // 64bit Linux: x in %edi, y in %esi
    __asm__(
        "movl %esi, %eax\n\t"
        "imull %edi, %eax\n\t"
        "ret"
    );
    return 0;
}
```
- 内联汇编仅能运行在特定的架构上，是和调用约定、ABI高度绑定的，通常不具有跨指令集、操作系统、编译器的可移植性。
- 一般来说很少使用，可以用在必须用汇编才能实现的场景中。当然这种场景其实还是添加汇编源文件编译为目标文件后链接到程序中来更好一些，耦合也更松，跨平台则直接编写多份即可。
