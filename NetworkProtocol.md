# 网络
## 什么是网络？  
网络是把各种设备连接起来，按照一定的通讯协议，进行通讯的集合。

## 网络物理拓扑结构
- 总线拓扑
- 环状拓扑
- 星型拓扑
- 扩展星型拓扑
- 网状拓扑
- 双环拓扑

## OSI参考模型

1. 应用层  application
2. 表示层  persention
3. 会话层  session
4. 传输层  transport
5. 网络层  network
6. 数据链路层  data link
7. 物理层  physical

### 网络模型分层的意义
- 降低复杂性
- 标准化接口
- 简化模块化设计
- 确保技术的互操作性
- 加快发展速度
- 简化教学

### PDU Protocol Data Unit
协议数据单元是指对等层次之间传递的数据单位
- 物理层的PDU是数据位 bit
- 数据链路层的PDU是数据帧 frame
- 网络层的PDU是数据包 packet
- 传输层的PDU是数据段 segment
- 其他更高层次的PDU是消息 message

## 通讯模式
- 单播
- 广播
- 组播

## LAN 组成
- 计算机  
- - PCs
- - Servers
- 网络互联
- - NICs
- - Media
- 网络设备
- - Hubs
- - Switches
- - Routers
- Protocols
- - Ethernet
- - IP
- - ARP
- - DHCP

## Ethernet Frame 结构
![image](https://user-images.githubusercontent.com/43201545/47615726-cad71e00-daed-11e8-9d55-4c73c1eca4be.png)

Ethernet 
- preamble 8Byte
- Destination Address 6Byte
- Source Address 6Byte
- Type 2Byte
- Data 46-1500 Byte
- Fcs 4Byte

IEEE 802.3
- Preamble 7Byte
- SOF 1Byte
- Destination Address 6Byte
- Source Address 6Byte
- Type 2Byte
- Data 46-1500 Byte
- Fcs 4Byte

## HUB集线器
    Hub：多端口中继器  属于物理层设备，它检查帧不是自己的就被抛弃
    Hub的特点：共享带宽，半双工     
    Hub 在网络中对每个端口都转发，flood 泛洪
    在Hub上同一时间只有一个信号在传输
    Hub 既不隔断冲突域也不隔断广播域

冲突域：当一个主机向网络中发送报文，另一个主机也发送报文，产生冲突，两个主机在一个冲突域中
广播域：一个主机发送广播，另一个主机收到，两个主机在一个广播域

## 交换机 以太网桥
任何一个数据报文到达交换机都可以转发，可以转发多路信号    
交换机能够分析帧结构，属于数据链路层设备
    多路信号同时可以传
    内置表端口满了以后就广播转发
    
    交换机表中记录的是数据报文的源地址，而广播地址只能作为目标地址，永远不能成为源地址，不可能成为源地址，不能存在MAC地址表中，所以只能泛洪
    广播地址表现为 FF:FF:FF:FF:FF:FF
    广播地址不会记录在数据表，所以只能泛洪
    只能隔断冲突域，不能隔断广播域

## 路由器
路由器属于网络层

    多播，广播都被隔断，隔断广播域
    可以做安全策略
    选择最佳路径，存放路由表，路由策略，路由算法
    连接广域网

路由：把一个数据包从一个设备发送到不同网络里的另一个设备上去。这些工作依靠路由器来完成。路由器只关心网络的状态和决定网络中的最佳路径。路由的实现依靠路由器中的路由表来完成

## TCP/IP 协议栈
Transmission Control Protocol/Internet Protocol
传输控制协议/因特网互联协议

TCP/IP是一个Protocol Stack，包括TCP、IP、UDP、ICMP、RIP、TELNET、FTP、SMTP、ARP等许多协议

共定义了四层和ISO参考模型的分层有对应关系

1. 应用层
2. 传输层
3. Internet层
4. 网络访问层

### TCP 特点
- 工作在传输层
- 面向连接协议
- 全双工协议
- 半关闭
- 错误检查
- 将数据打包成段，排序
- 确认机制
- 数据恢复，重传
- 流量控制，滑动窗口
- 拥塞控制，慢启动和拥塞避免算法

### TCP 包头
固定长度 20 Byte  
（单位 bit）

|源端口 |目的端口 |序号 |确认号 |数据偏移 |保留 |URG |ACK |PSH |PST |SYN |FIN |窗口 |校验和 |紧急指针 |
|-------|--------|-----|------|--------|-----|----|----|----|----|----|----|-----|------|--------|
|16     |16      |32   |32    |4       |6    |1   |1   |1   |1   |1   |1   |16   |16    |16      |

### TCP 端口号
用来标识每个应用程序的地址   0-65535

它是基于 C/S 结构 client/server 

常见端口号

    ssh C/S tcp 22
    http:tcp 80
    QQ:udp 8000
    https:tcp 443
    dhcp:tcp 67,68
    snmp:tcp 161
    mysql:tcp 3306
    oracle:tcp 1521
    sql:server:tcp 1433
    kerberos:tcp 88
    smtp:tcp 25
    pop3:tcp 110
    imap:tcp 143
    smb:tcp 445

有限状态机 FSM:Finite State Machine

状态：描述
- CLOSED：无连接是活动 的或正在进行
- LISTEN：服务器在等待进入呼叫
- SYN_RECV：一个连接请求已经到达，等待确认
- SYN_SENT：应用已经开始，打开一个连接
- ESTABLISHED：正常数据传输状态
- FIN_WAIT1：应用说它已经完成
- FIN_WAIT2：另一边已同意释放
- ITMED_WAIT：等待所有分组死掉
- CLOSING：两边同时尝试关闭
- TIME_WAIT：另一边已初始化一个释放
- LAST_ACK：等待所有分组死掉

### TCP 三次握手
TCP 建立连接需要同步，确认三次，俗称三次握手
![image](https://user-images.githubusercontent.com/43201545/47616883-56a47680-dafd-11e8-9273-203e98c8723e.png)

### TCP 四次挥手
TCP 断开连接需要四次请求，确认，四次挥手
![image](https://user-images.githubusercontent.com/43201545/47617203-dfbcad00-daff-11e8-91ab-d8ba27e75bcd.png)

### sync半连接和accept全连接队列
![image](https://user-images.githubusercontent.com/43201545/47617252-37f3af00-db00-11e8-896a-559e929b82e7.png)

### UDP 特性
- 工作在传输层
- 提供不可靠的网络访问
- 非面向连接协议
- 有限的错误检查
- 传输性能高
- 无数据恢复特性

### UDP包头
（单位 bit）

|源地址 |目标地址 |UDP 长度 |UDP 校验和 |数据 |
|-------|--------|---------|----------|-----|
|16     |16      |16       |16        |     |

### InternetProtocol 特性
- 运行于OSI网络层
- 面向无连接的协议
- 独立处理数据包
- 分层编址
- 尽力而为传输
- 无数据恢复功能

### IP 协议报头
固定部分 20 Byte  
(单位 bit)

|版本 |首部长度 |区分服务 |总长度 |标识 |标志 |片偏移 |生存时间 |协议 |首部检验和 |源地址 |目的地址 |
|-----|--------|--------|-------|----|-----|-------|--------|-----|---------|-------|--------|
|4    |4       |8       |16     |16  |3    |13     |8       |8    |16       |32     |32      |

## 路由    
路由器是单向设备
    路由表 决定了如何选择一条路径，把收到的数据报文转发到目标主机的所在地

        route -n （Linux命令）
        rout print （Windows命令）

    目标网络ID：目标IP所在网络ID
    接口：本设备要发送数据包到目标，从那个接口发送出来，才能到达
    网关：到达目标网络，需要将数据交给下一个路口哪个接口的对应的IP

    主机路由 192.168.32.100/32  优先级高于网络路由
    网络路由 192.168.64.0/19  优先级次之
    默认路由 0,0,0,0/0  优先级最低

    添加默认网关的目的就是为了生成默认路由

动态路由

    RIP OSPF BGP ISIS路由协议
    RIP 经过的路由器数量越少，就认为最优
    OSPF 会考虑带宽
