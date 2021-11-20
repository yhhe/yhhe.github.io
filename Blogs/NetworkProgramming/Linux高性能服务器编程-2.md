## Linux高性能服务器编程-2

**主要内容**
- Socket地址API
- 创建使用socket
- 数据读写API

### 5. Linux网络编程基础API
#### 5.1 Socket地址API
- **字节序**
    - 主机字节序和网络字节序（大端）
    - 大端（数据高位存储在内存低位）和小端（数据高位存储在内存高位）
```cpp
#include <netimet/in.h>

// host to net
unsigned long int htonl(unsigned long int hostlong);
unsigned short int htons(unsigned short int hostshort);

// net to host
unsigned long int ntohl(unsigned long int netlong);
unsigned short int ntohs(unsigned short int netshort);

```
- **通用socket地址**
    - 结构体sockaddr
    - 空间不够大时使用sockaddr_storage
```cpp
#include <bits/socket.h> // OR #include <sys/socket.h>

struct sockaddr{
    // 地址族类型
    /*
        协议族       地址族
        PF_UNIX     AF_UNIX     108字节
        PF_INET     AF_INET     端口号（2）和地址（4），共6字节
        PF_INET6    PF_INET6    端口号（2）流标志（4）地址（16）和范围id（4），共26字节
        (协议族和地址族值相同，可以混用)
    */
    sa_family_t sa_family;
    char sa_data[14];
};

struct sockaddr_storage{
    sa_family_t sa_family;
    // 内存对齐
    unsigned long int __ss_align;
    char __ss_padding[128-sizeof(__ss_align)];
};
```

- **专用socket地址**
```cpp
// UNIX 地址
#include <sys/un.h>

struct sockaddr_un{
    sa_family_t sin_family; // AF_UNIX
    char sun_path[108]; // 文件路径名
};

// TCP/IP
#include <netinet/in.h>

struct sockaddr_in{
    sa_family_t sin_family; // AF_INET
    u_int16_t sin_port; // 端口号，网络字节序
    struct in_addr sin_addr; // 地址结构体
};
struct in_addr{
    u_int32_t s_addr; // 地址，网络字节序
};

struct sockaddr_in6{
    sa_family_t sin6_family; // AF_INET6
    u_int16_t sin6_port; // 端口号，网络字节序
    u_int32_t sin6_flowinfo; // 流信息，应设置为0
    struct in6_addr sin6_addr; // 地址结构体
    u_int32_t sin6_scope_id; // scope id，实验阶段
};
struct in6_addr{
    unsigned char sa_addr[16]; // 地址，网络字节序
};
```

- **IP地址转换函数**
```cpp
#include <arpa/inet.h>

// 点分十进制转网络字节序整数表示
in_addr_t inet_addr(const char* strptr);

// 同上，将转化结果存入inp
// 成功返回1，失败返回0
int inet_aton(const char* cp, struct in_addr* inp);

// 将网络字节序整数转化为点分十进制
/*
    函数在内部用一个静态存储变量存储转化结果
    因此是不可重入的
*/
char* inet_ntoa(struct in_addr in);


// 同上，且同时适用于ipv4和ipv6
// 成功返回1，失败返回0，并设置errno
int inet_pton(int af, const char* src, void* dst);
// 成功返回目标地址，失败返回NULL并设置errno
const char* inet_ntop(int af, const void* src, char* dst, socklen_t cnt);
```

#### 5.2 创建、命名、监听socket，连接相关
- **创建**
```cpp
#include <sys/type.h>
#include <sys/socket.h>

/*
    domain: PF_INET, PF_INET6, PF_UNIX
    type: SOCK_STREAM, SOCK_DGRAM,
          SOCK_NONBLOCK(非阻塞), SOCK_CLOEXEC(在fork后的子进程中关闭该sock)
    protocol: 一般置为0
*/
int socket(int domain, int type, in protocol);

```

- **命名**
```cpp
#include <sys/type.h>
#include <sys/socket.h>

// 成功返回0，失败返回-1，设置errno
// EACCES 被绑定的地址是受保护的地址（普通用户绑定知名服务端口）
// EADDRINUSE 被绑定的地址正在使用中
int bind(int sockfd, const struct sockaddr* my_addr, sockelen_t addrlen);
```

- **监听**
```cpp
#include <sys/socket.h>

// backlog: 提示内核监听队列的最大长度，若超过，则不再受理
int listen(int sockfd, int backlog);
```

- **接受连接**

```cpp
#include <sys/type.h>
#include <sys/socket.h>

// 成功返回sock，失败返回-1，设置errno
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

- **发起、关闭连接**

```cpp
#include <sys/type.h>
#include <sys/socket.h>
#include <unistd.h>

// 成功返回sock，失败返回-1，设置errno
// ECONNREFUSED 目标端口不存在
// ETIMEDOUT 连接超时
int connect(int sockfd, const struct sockaddr* serv_addr, socklen_t addrlen);
// 引用次数减1
int close(int fd);
// 无论如何关闭连接
// howto：
//          SHUT_RD
//          SHUT_WR
//          SHUT_RDWR
// 成功返回0，失败返回-1并设置errno
int shutdown(int sockfd, int howto);
```

#### 5.3 数据读写
