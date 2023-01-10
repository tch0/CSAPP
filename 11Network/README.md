<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [第十一章：网络编程](#%E7%AC%AC%E5%8D%81%E4%B8%80%E7%AB%A0%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B)
  - [11.1 客户端-服务器编程模型](#111-%E5%AE%A2%E6%88%B7%E7%AB%AF-%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%BC%96%E7%A8%8B%E6%A8%A1%E5%9E%8B)
  - [11.2 网络](#112-%E7%BD%91%E7%BB%9C)
  - [11.3 全球IP因特网](#113-%E5%85%A8%E7%90%83ip%E5%9B%A0%E7%89%B9%E7%BD%91)
    - [IP地址](#ip%E5%9C%B0%E5%9D%80)
    - [因特网域名](#%E5%9B%A0%E7%89%B9%E7%BD%91%E5%9F%9F%E5%90%8D)
    - [因特网连接](#%E5%9B%A0%E7%89%B9%E7%BD%91%E8%BF%9E%E6%8E%A5)
  - [11.4 套接字接口](#114-%E5%A5%97%E6%8E%A5%E5%AD%97%E6%8E%A5%E5%8F%A3)
    - [套接字地址结构](#%E5%A5%97%E6%8E%A5%E5%AD%97%E5%9C%B0%E5%9D%80%E7%BB%93%E6%9E%84)
    - [socket函数](#socket%E5%87%BD%E6%95%B0)
    - [connect函数](#connect%E5%87%BD%E6%95%B0)
    - [bind函数](#bind%E5%87%BD%E6%95%B0)
    - [listen函数](#listen%E5%87%BD%E6%95%B0)
    - [accept函数](#accept%E5%87%BD%E6%95%B0)
    - [主机和服务的转换](#%E4%B8%BB%E6%9C%BA%E5%92%8C%E6%9C%8D%E5%8A%A1%E7%9A%84%E8%BD%AC%E6%8D%A2)
    - [套接字接口的辅助函数](#%E5%A5%97%E6%8E%A5%E5%AD%97%E6%8E%A5%E5%8F%A3%E7%9A%84%E8%BE%85%E5%8A%A9%E5%87%BD%E6%95%B0)
    - [echo客户端和服务器示例](#echo%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%92%8C%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%A4%BA%E4%BE%8B)
  - [11.5 Web服务器](#115-web%E6%9C%8D%E5%8A%A1%E5%99%A8)
  - [11.6 综合：TINY Web服务器](#116-%E7%BB%BC%E5%90%88tiny-web%E6%9C%8D%E5%8A%A1%E5%99%A8)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 第十一章：网络编程

网络应用依赖于很多概念：进程、信号、字节序、内存映射、动态内存分配等。

## 11.1 客户端-服务器编程模型

每个网络应用都基于**客户端-服务器模型**（Client-Server Model）：
- 采用这个模型，一个应用由一个服务端进程和多个客户端进程组成。
- 服务器管理某种资源，通过操作这种资源为客户端提供服务。
- 客户端-服务器模型中的基本操作是事务（transaction），一个客户端-服务器模型由四步组成：
    - 客户端需要服务，像服务器发送一个请求，发起一个事务。
    - 服务器收到请求，解释它，以适当方式操作它的资源。
    - 服务器向客户端发送一个响应，并等待下一个请求。
    - 客户端收到响应并处理它。
- 这里的客户与服务器并不是机器或者主机，而是进程。一台主机上可以同时运行许多不同的客户端和服务器，而且一个客户端和服务器的事务可以在同一台或是不同的主机上。
- 客户端-服务器事务不同于数据库事务，没有数据库事务的任何特性，比如原子性。在之类，客户和服务器端的事务仅仅是客户端和服务器执行的一系列步骤。

## 11.2 网络

网络是一个非常复杂的系统，这里只关注其提供的思维模型：
- 对主机而言，网络是一种IO设备，是数据源和数据接收方。
- 一个插到I/O总线扩展槽的适配器，提供了到网络的物理接口。从网络上接受的数据从适配器经过I/O和内存总线复制到内存，通常是通过DMA传送，类似地，数据也能从内存复制到网络适配器。
- 物理上来说，网络是一个按照地理远近组成的层次系统，最底层是LAN（Local Area Network，**局域网**）。现在最流行的以太网技术是**以太网**（Ethernet）。
- 一个以太网段包括一个**集线器**（hub）和一系列电缆连接起来的主机，每个以太网适配器都有一个全球唯一的48位的硬件（MAC）地址，存储在适配器上。
- 一台主机可以发送一段位（称为**帧**frame）到这个网段的其他任何主机。每个帧包括一些固定长度的**头部**（header）位，用来标识此帧的源和目的地以及此帧的长度，此后跟随的数据称为**有效载荷**（payload）。
- 使用电缆或者**网桥**（bridge），多个以太网可以连接成较大的局域网，成为桥接以太网（bridged Ethernet）。不同电缆可以拥有不同的带宽。
- 网桥会聪明地将帧发送到需要的端口，而不是发送到所有端口。
- 在层次的更高级别中，多个不兼容的局域网可以通过叫做路由器（router）的特殊计算机连接起来，组成一个internet（互联网络）。
- 每台路由器对于它所连接到的每个网络都有一个适配器（端口）。路由器也能连接到广域网（WAN，Wide-Area Network）。
- 互联网络的重要特征是：它能由采用不同和不兼容技术的各种局域网和广域网组成。
- 那么如何让某台源主机跨过不兼容的网络向另一台目的主机发送数据呢？解决方法是实现一层运行在每台主机和路由器上的**协议软件**，它消除了不同网络之间的差异。
- 这个软件就是协议，这种协议控制主机和路由器如何协同工作来实现数据传输，这种协议提供两种基本能力：
    - 命名机制：不同局域网技术有不同和不兼容的方式分配主机地址，互联网协议通过定义一种一致的主机地址格式消除这种差异。每台主机会被分配到至少一个互联网络地址（internet address），这个地址唯一标识一台主机。
    - 传送机制：不同的联网技术有不同不兼容的在电缆上编码位和将这些为封装成帧的方式。互联网协议通过定义一种把数据位捆扎成不连续的片（称为**包**）的统一方式，消除了这种差异。

## 11.3 全球IP因特网

- 全球IP因特网是最著名和成功的互联网实现，每台因特网主机都运行实现TCP/IP协议的软件，几乎每个现代计算机都支持这个协议。
- 因特网的客户端和服务端混合使用**套接字接口**函数和Unix IO函数进行通信。
- 通常套接字函数实现为系统调用，这些系统调用将会陷入内核，并调用内核模式的TCP/IP函数。
- TCP/IP是一个协议族，其中每一个都提供不同的功能。
    - 例如，IP协议提供基本的命名方法和递送机制，这种递送机制是从一台主机到另一台主机的。
    - UDP在其上稍微进行了扩展，这样，它在进程间而不是主机间传播。但和IP一样，依然是不可靠的。
    - TCP是一个构建在IP之上的复杂协议，提供了进程间可靠的全双工（双向）**连接**。
- 这里为了讨论方便，将不讨论太多细节，只讨论TCP和IP提供的某些基本功能，不讨论UDP。
- 从程序员角度，可以将互联网抽象为一个世界范围的主机集合：
    - 主机集合被映射到32位的IP地址。
    - 这些IP地址被映射为一组成为因特网域名的标识符。
    - 因特网主机的进程能够通过连接和任何其他因特网主机上的进程通信。

### IP地址

因为IPV6还未普及，这里仅基于IPV4，原理是一样。IP地址：
- IPV4地址原则上来说可以存在32位无符号整数中。
```C
struct in_addr {
    uint32_t s_addr; // big-endian
};
```
- 原则上来说放在32位无符号整数即可，但用一个结构保存是历史原因。
- TCP/IP规定任意整数数据项都是用大断序存放，因为因特网主机可能有不同字节序。
- IP地址结构中存放的总是大端序存放的，如果要将主机字节序和网络字节序相互转换可以用：
```C
#include <arpa/inet.h>
uint32_t htonl(uint32_t hostlong);
uint16_t htons(uint16_t hostshort);
uint32_t ntohl(uint32_t netlong);
uint16_t ntohs(uint16_t netshort);
```
- IP地址通常用点分十进制表示，`hostname -i`可以获取本机IP地址。
- 转换点分十进制IP和整数IP：
```C
#include <arpa/inet.h>
int inet_pton(int af, const char *src, void *dst);
const char *inet_ntop(int af, const void *src,
                      char *dst, socklen_t size);
```
- `af`可以是`AF_INET AF_INET6`表示IPV4或者IPV6地址。

### 因特网域名

互联网客户端和服务端相互通信使用IP地址，但是大整数很难记住，点分十进制也不是很好记，所以互联网定义了一组更为人性化的域名（domain name），以及一种将域名映射到IP地址的机制。
- 域名是一串用点号分隔的单词（字母、数字或破折号），比如`hello.cmu.edu`。
- 域名集合形成了一个树状的层次结构，每个域名编码了它在层次中的一个位置。从上到下称为顶级域名、二级域名、以此类推。
- 域名和IP的映射由分布世界范围内的数据库来维护（DNS）。
- 可以使用`nslookup`命令探究DNS映射的一些属性。
- 每台互联网主机都有本地域名`localhost`，总是映射到`127.0.0.1`：
```shell
tch@KillingBoat:~/CSAPP$ nslookup localhost
Server:         172.26.0.1
Address:        172.26.0.1#53

Non-authoritative answer:
Name:   localhost
Address: 127.0.0.1
Name:   localhost
Address: ::1
```
- 我们可以用这个命令查看某个网址的IP：
```shell
tch@KillingBoat:~/CSAPP$ nslookup google.com
Server:         172.26.0.1
Address:        172.26.0.1#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.160.110
```
- 某些域名可以映射到多个IP，某些合法域名则没有映射到任何IP。

### 因特网连接

互联网客户端和服务端通过在连接上发送和接收字节流来通信：
- 连接是点对点、全双工、可靠的。
- **套接字**（socket）是连接的端点，由一个IP地址和一个16位端口（port）构成。
- 客户端发起连接时，端口由内核临时分配，称为临时端口。而服务器端端口一般是固定的，比如HTTP固定用80端口。
- 一个连接由两端的套接字地址唯一确定，这对套接字地址叫做套接字对（socket pair）：由一个元组`(cliaddr : cliport, servaddr : servport)`唯一确定。

## 11.4 套接字接口

套接字接口（socket interface）是一组函数，和Unix IO函数结合起来，用以创建网络应用。大多数现代系统上都实现了套接字接口，包括所有类Unix、Windows、MacOS上。

### 套接字地址结构

从Linux内核来看，套接字就是通信的一个端点。从Linux程序员角度来看，套接字就是一个打开文件。

套接字结构：
```C
// IP socket address structure
struct sockaddr_in
{
    uint16_t        sin_family;  // protocal family, always AF_INET
    uint16_t        sin_port;    // port number
    struct in_addr  sin_addr;    // IP address
    unsigned char   sin_zero[8]; // Pad to sizeof(struct sockaddr)
};
// generic  socket address structure (for connect bind and accept)
struct sockaddr 
{
    uint16_t    sa_family;      // protocal family
    char        sa_data[14];    // address data
}
```
- `sockaddr`的存在是为了统一各种类型的套接字，实际的套接字比如`sockaddr_in`需要转化为`sockaddr`。对应的前者的后三个成员就存在了后者的`sa_data`数组中。

### socket函数

客户端和服务器使用`socket`函数来创建一个套接字描述符（socket descriptor）：
```C
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```
- 出错返回`-1`，成功返回非负描述符。
- 如果想使套接字成为连接的一个端点，那么就使用如下硬编码来调用：
```C
clientfd = socket(AF_INET, SOCK_STREAM, 0);
```
- `AF_INET`表明使用IPV4，`SOCK_STREAM`表明这个套接字是连接的一个端点。不过最好的方式是使用`getaddrinfo`来自动生成这些参数，这样代码就与协议无关了。
- 返回的描述符是仅部分打开的，还不能用于读写，如何完成接下来的工作，取决于是服务端还是客户端。

### connect函数

客户端通过`connect`来建立和服务器的连接：
```C
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int connect(int sockfd, const struct sockaddr *addr,
            socklen_t addrlen);
```
- `connect`函数试图与套接字地址为`addr`的服务器建立一个因特网连接，其中`addrlen`是`sizeof(sockaddr_in)`。
- `connect`函数会阻塞，一直到连接成功建立，或者发生错误，如果成功，`clientfd`现在就已经准备好可以读写了，得到的连接由套接字对`(x:y, addr.sin_addr:addr.sin_port)`标识。
- 它唯一确定了客户端主机上的客户端进程。对于`socket`，更好方法是用`getaddrinfo`为`connect`提供参数。

剩下的函数：`bind listen accept`是服务器用来和客户端建立连接的。

### bind函数

`bind`函数告诉内核将`addr`中的服务器套接字地址和套接字描述符`sockfd`联系起来。
```C
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int bind(int sockfd, const struct sockaddr *addr,
         socklen_t addrlen);
```
- `addrlen`是`sizeof(sockaddr_in)`。

### listen函数

客户端是发起连接请求的主动实体，服务器是等待来自客户端的连接请求的被动实体：
```C
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int listen(int sockfd, int backlog);
```
- 默认情况下，内核会认为`socket`创建的描述符应用于主动套接字（active socket），存在于一个连接的客户端。服务端调用`listen`函数告诉内核，描述符是被用于服务器而不是客户端使用的。
- `listen`将一个主动套接字转换为一个监听套接字（listening socket），该套接字可以接受来自客户端的连接请求。
- `backlog`参数暗示了内核在开始拒绝连接请求前，队列中要排队的未完成连接请求数量。确切含义这里不表，设为1024即可。

### accept函数

等待客户端的连接请求：
```C
#include <sys/types.h>          /* See NOTES */
#include <sys/socket.h>
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
#define _GNU_SOURCE             /* See feature_test_macros(7) */
#include <sys/socket.h>
int accept4(int sockfd, struct sockaddr *addr,
            socklen_t *addrlen, int flags);
```
- 服务器调用`accept`来等待来自客户端的连接请求到达监听描述符`sockfd`，然后在`addr`中填写客户端的套接字地址，并返回一个已连接描述符（connected descritor），失败则返回-1。
- 这个描述符可以被用来利用Unix IO函数与客户端通信。

监听描述符与已连接描述符：
- 监听描述符作为客户端请求的一个端点，通常被创建一次，并存在于服务器的整个生命周期。
- 已连接描述符是客户端和服务器之间已经建立起来的连接的一个端点，每次接受连接请求会创建一次，只存在于服务器为一个客户端服务的过程中。
- 区分两者可以使我们在同一个监听描述符上建立多个连接，用于并发服务器。比如每次一个连接请求到达监听描述符时，可以派生（fork）一个新进程，通过已连接描述符和客户端通信。并同时在其他进程中继续监听。

### 主机和服务的转换

Linux提供了一些函数实现二进制套接字地址结构、主机名、主机地址、服务名、端口号的字符串表示之间的相互转换：
```C
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
int getaddrinfo(const char *node, const char *service,
                const struct addrinfo *hints,
                struct addrinfo **res);
void freeaddrinfo(struct addrinfo *res);
const char *gai_strerror(int errcode);
```
- 和套接字接口一起使用，能够使我们编写独立于任何特定版本的IP协议的网络程序。
- `getaddrinfo`将主机名、主机地址、服务名、端口号的字符串转换为套接字地址结构。
- 与只相反的相反的函数是`getnameinfo`：
```C
#include <sys/socket.h>
#include <netdb.h>
int getnameinfo(const struct sockaddr *addr, socklen_t addrlen,
                char *host, socklen_t hostlen,
                char *serv, socklen_t servlen, int flags);
```

### 套接字接口的辅助函数

略。

### echo客户端和服务器示例

略，作为替代实现一个简单的程序作为替代：客户端输入内容，并不断传输到服务端显示。

## 11.5 Web服务器

HTTP协议，端口80，略。

## 11.6 综合：TINY Web服务器

略。
