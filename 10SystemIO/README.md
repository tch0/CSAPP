<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [第十章：系统级I/O](#%E7%AC%AC%E5%8D%81%E7%AB%A0%E7%B3%BB%E7%BB%9F%E7%BA%A7io)
  - [10.1 Unix I/O](#101-unix-io)
  - [10.2 文件](#102-%E6%96%87%E4%BB%B6)
  - [10.3 打开和关闭文件](#103-%E6%89%93%E5%BC%80%E5%92%8C%E5%85%B3%E9%97%AD%E6%96%87%E4%BB%B6)
  - [10.4 读和写文件](#104-%E8%AF%BB%E5%92%8C%E5%86%99%E6%96%87%E4%BB%B6)
  - [10.5 用RIO包健壮地读写](#105-%E7%94%A8rio%E5%8C%85%E5%81%A5%E5%A3%AE%E5%9C%B0%E8%AF%BB%E5%86%99)
  - [10.6 读取文件元数据](#106-%E8%AF%BB%E5%8F%96%E6%96%87%E4%BB%B6%E5%85%83%E6%95%B0%E6%8D%AE)
  - [10.7 读取目录内容](#107-%E8%AF%BB%E5%8F%96%E7%9B%AE%E5%BD%95%E5%86%85%E5%AE%B9)
  - [10.8 共享文件](#108-%E5%85%B1%E4%BA%AB%E6%96%87%E4%BB%B6)
  - [10.9 I/O重定向](#109-io%E9%87%8D%E5%AE%9A%E5%90%91)
  - [10.10 标准I/O](#1010-%E6%A0%87%E5%87%86io)
  - [10.11 综合：我该使用哪些I/O函数？](#1011-%E7%BB%BC%E5%90%88%E6%88%91%E8%AF%A5%E4%BD%BF%E7%94%A8%E5%93%AA%E4%BA%9Bio%E5%87%BD%E6%95%B0)
  - [资料补充](#%E8%B5%84%E6%96%99%E8%A1%A5%E5%85%85)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 第十章：系统级I/O

输入输出（IO）是在主存和外部设备（磁盘驱动器、终端、网络）之间复制数据的过程。
- 输入操作是从IO设备到主存，输出操作是从主存到IO设备。
- 所有运行时系统都提供执行IO的较高级别的工具，例如ANSI C提供标准IO库，包含printf、scanf这样的带缓冲IO函数，C++语言IO库提供`<< >>`运算符用于输入输出。
- 在Linux系统中，可以通过内核提供的系统级Unix IO函数实现这些教高级别的输出输出功能的。
- 某些时候，无法选择或者高层IO接口不是一个好选择，除了使用底层IO接口别无选择。比如网络编程中、需要获取文件元数据等场景。

## 10.1 Unix I/O

一个Linux文件就是一个某长度字节的序列，所有的IO设备都被模型化为**文件**，而所有的输入输出都被当做对相应文件的读和写来执行。
- 这种将设备优雅地映射为文件的方式，允许Linux内核引出一个简单的、低级的应用接口，称之为Unix I/O。使得所有输入输出能够以统一方式来执行：
- 打开文件：应用程序通过要求内核打开相应文件，来宣告它想要访问一个I/O设备。内核返回一个小的非负整数，叫做描述符，后续对文件的所有操作都基于文件描述符。内核记录着打开文件的所有信息，应用程序只需要记住这个描述符。
- Linux Shell创建的每个进程开始时都有三个打开的文件，标准输入、标准输出、标准错误输出，文件描述符分别为0、1、2，使用常量`STDIN_FILENO STDOUT_FILENO STDERR_FILENO`来表示。
- 改变文件当前位置：对于每个打开文件，内核维护着一个当前位置k，初始为0，这个值是从文件开头开始的字节偏移量。通过`seek`操作可以改变当前位置。
- 读写文件：读操作就是从文件复制n字节到内存，从当前文件位置k开始增加到k+n，如果k+n大于文件大小，就会触发EOF（End-Of-File）条件，应用程序能够检测这个条件，文件尾并没有明确的EOF符号。类似地，写操作就是从内存复制n字节到文件，从当前文件k开始，然后更新k。
- 关闭文件：当应用完成对文件的访问之后，它会通知内核关闭这个文件，作为响应，内核会释放打开时创建的数据结构，并将文件描述符恢复到可用的描述符池中。无论一个进程因为何种原因终止，内核都会自动关闭它打开的文件，并释放它们的内存资源。

## 10.2 文件

每个Linux文件都有一个类型：
- 普通文件：包含任意数据，可能是文本文件或者二进制文件，对内核没有差别，只在应用程序处理中存在差别。文件文件是一个文本行序列，每一个文本行以`\n`即LF作为结尾。
- 目录（directory）：包含一组链接（link）的文件，其中每个链接都将一个文件名映射到一个文件，这个文件也可能是目录。其中至少包含两个文件，`. ..`映射到当前目录和上一级目录。
- 套接字（socket）：是和另一个进程进行跨进程通信的文件。
- 其他文件类型：命名管道、符号链接、字符和块设备。

Linux将所有文件组织成一个目录层次的树形结构（directory hierarchy），根目录是`/`。

作为上下文的一部分，每个进程都有一个当前工作目录（current working directory），来确定其目录层次结构中的当前位置，可以用`cd`命令修改shell中的当前工作目录。
- 目录层次中的位置用路径名（pathname）来指定。
- 路径名是一个字符串，可以是绝对路径或者相对路径。

## 10.3 打开和关闭文件

打开文件：
```C
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
int creat(const char *pathname, mode_t mode);
int openat(int dirfd, const char *pathname, int flags);
int openat(int dirfd, const char *pathname, int flags, mode_t mode);
```
- `flags`访问模式：
    - 读写权限：`O_RDONLY O_WRONLY R_RDWR`表示只读、只写、可读可写。
    - 可以或上更多掩码：`O_CREAT O_TRUNC O_APPEND`表示创建、截断、添加到末尾。
- `mode`参数给定新文件的访问权限：
    - 作为上下文一部分，每个进程有一个`umask`，可以通过调用`umask`函数设置：
    ```C
    #include <sys/types.h>
    #include <sys/stat.h>
    mode_t umask(mode_t mask);
    ```
    - `open creat`创建新文件时，文件权限位被设置为`mode & ~umask`。
    - 掩码或者访问权限位：
    - `S_IRUSR S_IWUSR S_IXUSR`：拥有者可以读、写、执行。
    - `S_IRGRP S_IWGRP S_IXGRP`：拥有者同组可以读、写、执行。
    - `S_IROTH S_IWOTH S_IXOTH`：其他人可以读、写、执行。

关闭文件：
```C
#include <unistd.h>
int close(int fd);
```

## 10.4 读和写文件

读写文件：
```C
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
ssize_t write(int fd, const void *buf, size_t count);
```
- `read`从文件中读取至多`count`字节到内存位置`buf`，返回实际读取的数量，`-1`表示错误，可能为0（如果直接遇到EOF）。
- `write`从`buf`中写入`count`字节到文件`fd`当前文件位置。

更改当前位置：
```C
#include <sys/types.h>
#include <unistd.h>
off_t lseek(int fd, off_t offset, int whence);
```

`sszie_t size_t`区别：
- 前者有符号，可以表示负数，后者无符号，通常用来表示长度、大小。

不足值：
- `read write`某些时候传送的字节比应用程序要求的少。
- 读磁盘文件除了`EOF`不会有，写磁盘文件也不会有。
- 从标准输入读取，一行缓冲一次，返回一行的内容。
- 读写网络套接字，可能因为缓冲和延迟导致`read write`实际读写数量不足。
- 管道调用`read write`也可能不足。
- 这些不是错误，而是程序行为，不应该仅依赖传入的字节数来保证，而应该检查返回值，适当重新调用。

## 10.5 用RIO包健壮地读写

可以使用`read write`实现会自动处理不足值的健壮IO函数，通过循环检查返回值在不足或者被中断时自动重新调用，直到所有字节传输完成再返回。实现略。

## 10.6 读取文件元数据

应用程序能够调用`stat fstat`函数，检索关于文件的信息：
```C
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
int stat(const char *pathname, struct stat *statbuf);
int fstat(int fd, struct stat *statbuf);
int lstat(const char *pathname, struct stat *statbuf);
#include <fcntl.h>           /* Definition of AT_* constants */
#include <sys/stat.h>
int fstatat(int dirfd, const char *pathname, struct stat *statbuf,
            int flags);
```
- `struct stat`数据结构文件的元数据：
```C
struct stat {
    dev_t     st_dev;         /* ID of device containing file */
    ino_t     st_ino;         /* Inode number */
    mode_t    st_mode;        /* File type and mode */
    nlink_t   st_nlink;       /* Number of hard links */
    uid_t     st_uid;         /* User ID of owner */
    gid_t     st_gid;         /* Group ID of owner */
    dev_t     st_rdev;        /* Device ID (if special file) */
    off_t     st_size;        /* Total size, in bytes */
    blksize_t st_blksize;     /* Block size for filesystem I/O */
    blkcnt_t  st_blocks;      /* Number of 512B blocks allocated */

    /* Since Linux 2.6, the kernel supports nanosecond
       precision for the following timestamp fields.
       For the details before Linux 2.6, see NOTES. */

    struct timespec st_atim;  /* Time of last access */
    struct timespec st_mtim;  /* Time of last modification */
    struct timespec st_ctim;  /* Time of last status change */

#define st_atime st_atim.tv_sec      /* Backward compatibility */
#define st_mtime st_mtim.tv_sec
#define st_ctime st_ctim.tv_sec
};
```
- `st_mode`编码了文件访问许可位和文件类型，`sys/stat.h`中定义了一些宏函数来确定`st_mode`中的文件类型：
    - `S_ISREG(m) S_ISDIR(m) S_ISSOCK(m)`表示是普通文件、目录文件、套接字。

## 10.7 读取目录内容

- 打开目录，得到一个指向目录流`DIR`的指针。
```C
#include <sys/types.h>
#include <dirent.h>
DIR *opendir(const char *name);
DIR *fdopendir(int fd);
```
- 读取目录：每次调用返回指向目录流中下一目录项的指针，如果没有更多目录项则返回NULL。
```C
#include <dirent.h>
struct dirent *readdir(DIR *dirp);

struct dirent {
    ino_t          d_ino;       /* Inode number */
    off_t          d_off;       /* Not an offset; see below */
    unsigned short d_reclen;    /* Length of this record */
    unsigned char  d_type;      /* Type of file; not supported
                                   by all filesystem types */
    char           d_name[256]; /* Null-terminated filename */
};
```
- `dirent`中只有`d_ino d_name`是标准的，前者是文件名，后者是文件位置。
- 出错会返回NULL，并设置errno。区分错误和流结束的方式只有检查errno前后是否修改过。
- 关闭目录：
```C
#include <sys/types.h>
#include <dirent.h>
int closedir(DIR *dirp);
```
- 关闭流并释放其所有资源。

## 10.8 共享文件

Linux有许多方式可以用来共享文件，需要弄清楚内核如何表示打开文件才能才能理解文件共享，内核用三个相关的数据结构来表示打开文件：
- **描述符表**（descriptor table）：每个进程都有独立的描述符表，表项使用文件描述符索引。每个打开的文件描述符表项指向文件表中的一个表项。
- **文件表**：打开文件的集合由一张文件表来表示，所有进程共享这张表。每个文件表表项包括当前文件位置、引用计数、指向v-node表中对应表项的指针。关闭一个描述符会递减引用计数，知道引用计数为零，内核才会删除这个表项。
- **v-node表**：所有进程共享，每个表项包含`stat`结构中的大多数信息，包括`st_mode st_size`成员。

文件共享：
- 可以通过多次调用`open`打开同一个文件来共享文件，此时得到的多个描述符不同，他们指向的文件表项也不同，文件表项中保存的文件位置也不同。不过最终指向的v-node表项是相同的。
- 父子进程则不同，子进程拥有父进程文件描述符表的副本，父子进程共享相同的文件表项，因此共享表项中相同的文件位置，但是不共享描述符表。

## 10.9 I/O重定向

Linux Shell提供I/O重定向操作符，允许用户将磁盘文件和标准输入输出练习起来，比如：
```shell
ls > foo.txt
```
- 使shell执行`ls`文件，并且将标准输出文件重定向到磁盘文件`foo.txt`。
- 那么IO重定向是如何工作的呢，一个典型手段是使用`dup`函数：
```C
#include <unistd.h>
int dup(int oldfd);
int dup2(int oldfd, int newfd);
#define _GNU_SOURCE             /* See feature_test_macros(7) */
#include <fcntl.h>              /* Obtain O_* constant definitions */
#include <unistd.h>
int dup3(int oldfd, int newfd, int flags);
```
- `dup`返回一个新的复制的文件描述符，`dup2`则是将就`oldfd`复制到`newfd`，如果`newfd`已经打开，那么会被静默关闭。
- 复制描述符的意思就是复制对应的描述符表项，复制得到的和原文件描述符表项将指向同一个文件表项，共享同一个文件位置，共享同一个文件表项中的引用计数。
- 出错返回-1，成功将返回新的文件描述符。
- 例子：使用`dup(5, STDIN_FILENO)`即可将标准输入重定位到描述符5对应文件，那么此时从标准输入读取内容时就会从改文件读取。

## 10.10 标准I/O

C语言定义了一组高级输入输出函数，称为标准I/O库，这个库提供了：
- 打开关闭文件：`fopen fclose`。
- 按字节读写文件：`fread fwrite`。
- 读写字符串函数：`fgets fputs`。
- 以及复杂的格式化函数：`fscanf fprintf`。
- 这些函数有些有标准输入输出版本，对应于使用标准输出输出流`stdin stdout stderr`。
- 标准输入输出流类型是`struct FILE`，是对文件描述符和流缓冲区的抽象。

## 10.11 综合：我该使用哪些I/O函数？

无论是自己实现的可靠的IO函数，还是C标准库提供的跨平台IO函数，在Unix平台都是基于底层Unix IO系统调用的。
- 在用户程序中，可以使用任意一者进行IO。
- 那么如何选择呢，基本指导原则是：
    - 只有有可能就使用标准IO，标准IO已经处理了缓冲区、格式化等问题，并且还跨平台。
    - 不要使用格式化IO函数来读取二进制文件，标准接口都是读取文本文件的。应该自己编写Unix IO的包装来做这件事情（如果要跨平台，那么最好先定义一套跨平台接口，而不是直接用Unix IO）。
    - 用于网络IO时，最好自己实现一套接口（虽然使用Unix IO也是可行的，但最好在此基础上实现一套更健壮的IO函数），不能使用标准输入输出。

## 资料补充

- APUE，TLPL。