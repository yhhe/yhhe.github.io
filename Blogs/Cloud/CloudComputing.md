## 云计算（刘鹏）

### 第一章 大数据与云计算
- 云计算
    - 云计算是并行计算、分布式计算、和网格计算这些计算科学概念的商业实现
    - 云计算是虚拟化、效用计算、将基础设施作为服务(IaaS)、将平台作为服务(Paas)、和将软件作为服务(SaaS)等概念混合演进并跃升的结果。
    - 特点： 超大规模，虚拟化，高可靠性，通用型，高伸缩度，按需服务，极其廉价
    - Iaas 
        - 将硬件设备等基础资源封装成服务供用户使用
        - 相当于使用裸机和磁盘
    - Paas
        - 提供用于应用程序的运行环境
    - SaaS
        - 将某些特定应用软件封装成服务
- 云计算的实现机制
    - 物理资源层
    - 资源池层
    - 管理中间件层
    - SOA(Service-Oriented Architecture)构建层
```
SOA构建层   | 服务接口，服务注册，服务查找，服务访问，服务工作流
-----------------------------------------------------
          ｜ 用户管理
管理中间件层｜ 任务管理      安全管理
          ｜ 资源管理
-----------------------------------------------------
资源池层    |   计算资源池，存储资源池，网络资源池，数据资源池，软件资源池
-----------------------------------------------------
物理资源层  ｜  计算机，存储器，网络设施，数据库，软件

```


### 第二章 Google云计算原理与应用
- Google文件系统GFS
    - 系统架构
        - 分为Client，Master，和Chunk Server
        - 数据存储在Chunk Server上
        - 首先访问Master，获取Chunk Server信息，而后访问Chunk Server获取数据
        - 特点：中心服务器模式，不缓存数据，在用户态下实现，只提供专用接口
        ![F2-1 GFS的系统架构](./Resource/F2-1-GFS的系统架构.png)
    - 容错机制