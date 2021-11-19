## Linux高性能服务器编程-1

### 1 TCP/IP 协议族
TCP/IP协议族体系结构及主要协议
```
应用层              ping        telnet      OSPF        DNS
                    |           |           |          |
传输层               |          TCP          |          UDP
                    |               \       |        /
网络层              ICMP---------------------IP------
                                            |
数据链路层                ARP------------- Data Link ---- RARP

```
