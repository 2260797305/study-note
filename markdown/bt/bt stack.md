# Bluetooth 介绍

## 蓝牙概述

蓝牙无线技术是一种短距离通信系统；蓝牙无线技术的关键特性是稳定，功耗低，成本低。 其中许多功能是
可配置的，允许产品差异化；名字来源由于一个统一了挪威和丹麦的国王，他因喜欢吃蓝莓而导致牙齿染成蓝色；

蓝牙的传输频段在未授权的 2.4Ghz 左右；工作在此频段的还有微波炉，ZigBee，WIFI 等等；

![img](bt%20stack.assets/20171106131110720.jpg)



## 蓝牙分类

 当前的蓝牙协议包括**BR/EDR(Basic Rate/Enhanced Data Rate)**、**AMP(Alternate MAC/PHYs)**、**LE(Low Energy)**三种技术。

蓝牙版本在2019年已经更新到了 5.1。第一个正式版本 1.0 是在1999 年推出的；2004 年的 2.0 版本推出了EDR，2019 月的 3.0 版本推出了 AMP，2010 的 4.1版本时推出了 BLE；

### BR/EDR

传统蓝牙，**Enhanced Data Rate (EDR)** 是可选的；BR 理论最大传输速率可以达到 721.2 kb/s， EDR 可以达到 2-3 Mb/s (bits)；

使用频段： 2.4 - 2.4835 GHz，79 个频段，每 1Mhz 一个区段；

| Regulatory Range | RF Channels            |
| ---------------- | ---------------------- |
| 2.400-2.4835 GHz | f=2402+k MHz, k=0,…,78 |

### LE

BLE，低功耗蓝牙，主要是用于低功耗场景，其相比 BR/EDR，功耗更底、复杂度更低、成本更低；

速率有 1 Mb/s 2 Mb/s 125 kb/s 500 kb/s 几种；

使用频段：2402 + k * 2 MHz, where k = 0, ..., 39. 

### AMP

高速蓝牙，速率可达 54 Mb/s ；它不能独立存在。他需要依附于双方的BR/EDR 先建立好连线。才能通过 AMP 来交互数据；当蓝牙设备双方通过 BR/EDR 建立好连线之后，检查到双方都支持 AMP，就可以将数据迁移到 AMP 进行通信；

蓝牙核心系统包括一个 **Host(BTS)** 和至少一个 **controller(BTC)**；两者之间交互的一层被称作 **HCI(Host Controller interface)**；

![image-20191116210444156](bt%20stack.assets/image-20191116210444156.png)



## 数据交互

### 拓扑结构

BR/EDR   controller 之间连接的拓扑结构称为微微网（**piconet**），每一个连接都包含两个设备，分别叫做 **master**  和 **slave**；

一个 piconet 中，有且仅有一个 master 设备，其他设备都是 slave 设备，且与 master 设备相连；

一个 piconet  中的 master，可以同时在其他 piconet  中做 slave；

一个设备不可能是多个 piconet 的 master，但可以是多个 piconet 的 slave；

一个piconet中，master 最多可以与 255 个slave 相连，但是至多只能有 7 个slave 处于活动状态；

一个 piconet 中，slave 只能与 master 通信，无法与其他 salve 通信；

如果多个微微网之中有共用的设备，那么便称这几个微微网组成了**散射网 (scatternet)**；散射网上每一个微微网都有自己的 master，有各自的跳频序。多个微微网的叠加，会导致干扰增加，效率降低；



![image-20191117231917178](C:/Users/miku/Desktop/Bluetooth%20doc/pic/%E5%BE%AE%E5%BE%AE%E7%BD%91.png)

### 时钟与地址

每一个蓝牙设备都有一个本地时钟，频率为 3.2 kHz ；最低能表达的时间是 312.5 us；一般称 625 us 为一个  shot；

同一个微微网中，设备都与master的时钟同步；不同的微微网中，他们的时钟是不用同步的；

每一个蓝牙设备都有一个 48 bits 的独一无二的蓝牙地址；这个地址需要向  **IEEE Registration Authority**  登记；

![image-20191118235006270](bt%20stack.assets/image-20191118235006270.png)

LAP 中 0x9E8B00-0x9E8B3F   这段 64 地址是用于做 inquiry 的；  0x9E8B33  这个设备地址是用作通用设备查询，而其他 63 个地址是用于查询特定类别的设备 ；设备的地址的LAP 段不可以是这个范围的值；

设备之间使用**跳频技术（  frequency hopping  ）**进行数据通信；传输数据时，设备双方收发器的必须在同一频段；所以双方必须有一个共同的跳频序列，跳频序列由地址的  UAP 和LAP  确定；



![image-20191124164133033](bt%20stack.assets/image-20191124164133033.png)



### inquiry

**inquiry** 即扫描（发现）周围设备的过程；

**inquiry scan** 即指该设备能够被周围设备扫描到（发现）；

inquiry 设备跳频序列由 LAP 段的确定；一般为指定一个在0x9E8B00-0x9E8B3F  中的值；蓝牙设过查询在其周围邻近的设备，inquiry 设备每隔 312.5（0.5 个 slot） 微秒选择一个新的频率来发送查询；inquiry  scan 设备每隔1.28s选择一次新的监听频率，两者一个快，一个慢，总有碰头的机会。

当 inquiry scan 的设备监听到 inquiry 的信息时，就会

inquiry page 的 response： bt  addr、bt name、device class、clock offset；



### page

**page**：即设备与指定的目标设备建立连接的过程；

**page scan**：即指设备能与其他人建立连接；

蓝牙设备通过page 其它的设备加入其所在的微微网，主动 page 的设备每隔312.5微秒选择一个新的频率来发送 page 消息； page scan 的设备每隔1.28s选择一个新的监听频率。寻呼和被寻呼设备使用被寻呼设备地址(BT_ADDR )的低28个比特,产生的寻呼跳变序列(paging –hopping  sequence)是一个定义明确的周期序列,它的各个频点均匀分布在2.4G的79个频率信道上. 

page 需要知道对端的设备地址，跳频序列由被配对的设备的地址确定；  跳频频率为  3200 / s

一般主动发起 page 的设备在连接建立之后会成为 master；当然，设备也可以请求发起 **role switch**，切换 master；

### 连接

![image-20191120231406942](bt%20stack.assets/image-20191120231406942.png)

master 只会在偶数 slot 时发起数据传输，slave 只会在奇数 slot 时发起传输；每次传输的时间与传输的 packet 有关，占1、3、5 个 slot；



#### ACL



#### SCO/ESCO



## 连接模式

### sniff mode

### active mode



## 安全管理



![2019-11-17_225944](bt%20stack.assets/2019-11-17_225944.png)

​	C-plane: 控制

​	U-plane：数据

## OPERATIONAL PROCEDURES AND MODES 





## 更新日志

| 版本       | 更新时间      | 更新内容                                                     |
| ---------- | ------------- | ------------------------------------------------------------ |
| 5.0        | Dec 06 2016   | •New features added in 5.0:<br/>  - CSA 5 features (Higher Output Power)<br/>  - Slot Availability Mask (SAM)<br/>- 2 Msym/s PHY for LE<br/>- LE Long Range<br/>- High Duty Cycle Non-Connectable Advertising<br/>- LE Advertising Extensions<br/>- LE Channel Selection Algorithm #2<br/>• Park State was deprecated and removed<br/>• Errata for v2.0 + EDR, v2.1 + EDR, v3.0 + HS + 4.0 + 4.1<br/>+ 4.2 (ESR09, ESR10 and ESR11) |
| 4.2        | Dec 02 2014   | •New features added in 4.2:<br/>- LE Data Packet Length Extension<br/>- LE Secure Connections<br/>- Link Layer Privacy<br/>- Link Layer Extended Scanner Filter Policies<br/>• Errata for v2.0 + EDR, v2.1 + EDR, v3.0 + HS + 4.0 + 4.1 |
| 4.1        | Dec 03 2013   | •New features added and changes made in 4.1:<br/>- CSA 2 features<br/>- CSA 3 features<br/>- CSA 4 features<br/>- Secure Connections<br/>- Train Nudging & Generalized Interlaced Scan<br/>- Low Duty Cycle Directed Advertising<br/>- 32-bit UUID Support in LE<br/>- LE Dual Mode Topology<br/>- Piconet Clock Adjustment<br/>- Removal of At Least One New Feature<br/>- LE L2CAP Connection Oriented Channel Support<br/>- LE Privacy v1.1<br/>- LE Link Layer Topology<br/>- LE Ping<br/>• Errata for v2.0 + EDR, v2.1 + EDR, v3.0 + HS + 4.0<br/>(ESR05, ESR06 and ESR07) |
| 4.0        | June 30 2010  | • New features added in 4.0:<br/>-Low Energy<br/>• Errata for v2.0 + EDR, v2.1 + EDR, v3.0 + HS |
| 3.0 + HS   | April 21 2009 | • New features added in 3.0 + HS:<br/>-AMP Manager Protocol (A2MP)<br/>-Enhancements to L2CAP for AMP<br/>-Enhancements to HCI for AMP<br/>-Enhancements to Security for AMP<br/>-802.11 Protocol Adaptation Layer<br/>• Enhanced Power Control<br/>-Unicast Connectionless Data<br/>-HCI Read Encryption Key Size command<br/>-Generic Test Methodology for AMP<br/>-Enhanced USB and SDIO HCI Transports<br/>• Errata for v 2.0 + EDR and v2.1 + EDR |
| v2.1 + EDR | July 26 2007  | • New features added in 2.1 + EDR:<br/>-Encryption Pause and Resume<br/>-Erroneous Data Reporting<br/>-Extended Inquiry Response<br/>-Link Supervision Timeout Changed Event<br/>-Non-Flushable Packet Boundary Flag<br/>-Secure Simple Pairing<br/>-Sniff Subrating<br/>-Security Mode 4<br/>• Updates to IEEE language in Volume 2, Part H, Security<br/>• Errata for v2.0 + EDR |
| v2.0 + EDR | Aug 01 2004   | This version of the specification is intended to be a separate <br/>Bluetooth Specification. This specification was created by adding<br/> EDR and the errata |
| 1.0a       | July 26 1999  | The first version of the Bluetooth Specification published on<br/>the public web site.<br/>Added part: Bluetooth Compliance Requirements |
| 0.7        | Oct 19 1998   | This first version only included Baseband and Link Manager Protocol |

#### Framed data traffic 

![1571847292027](E:\markdown\bt stack.assets\1571847292027.png)

即 acl -> l2cap 的数据

#### Unframed data traffic 

即 sco， esco； ble 不支持 Unframed data traffic 

#### Reliability of traffic bearers 

The baseband packet header uses forward error correcting (FEC) coding to allow error correction by the receiver and a header error check (HEC) to detect errors remaining after correction. Certain Baseband packet types include FEC for the payload. Furthermore, some Baseband packet types include a cyclic redundancy error check (CRC) 

 自动重传请求(Automatic Re Transmission Query， ARQ)



![1571848115312](E:\markdown\bt stack.assets\1571848115312.png)

- All packets include the channel access code. This is used to identify  communications on a particular physical channel, and to exclude or ignore packets on a different physical channel that happens to be using the same RF carrier in physical proximity 
- There is no direct field within the BR/EDR packet structure that represents or contains information relating to physical links. This information is implied by the combination of the logical transport address (LT_ADDR) carried in the packet header and the channel access code (CAC) 

## BR/EDR physical channels 

配对的两个设备，同一时刻使用同一频率；跳频序列有master  地址确定，为伪随机数；

### Basic piconet channel 

is used for communication between connected devices during normal operation 

On the basic piconet channel the master controls access to the channel. The master starts its transmission in even-numbered time slots only 

The basic piconet channel supports a number of physical links, logical transports, logical links and L2CAP channels used for general purpose Adapted piconet channel communications. 



five distinct security features: pairing,
bonding, device authentication, encryption and message integrity.
• Pairing: the process for creating one or more shared secret keys
• Bonding: the act of storing the keys created during pairing for use in
subsequent connections in order to form a trusted device pair
• Device authentication: verification that the two devices have the same keys
• Encryption: message confidentiality
• Message integrity: protects against message forgeries 

## packet

| LOGICAL TRANSPORTS | packet | slot | 说明                                        |
| ------------------ | ------ | ---- | ------------------------------------------- |
| common             | ID     | 1    | 固定68 bits                                 |
| common             | NULL   | 1    | 126bits，用于返回上一次传输的信息           |
| common             | POLL   | 1    |                                             |
| common             | FHS    | 1    |                                             |
| common             | DM1    | 1    | be regarded as an ACL packet                |
| sco                | HV1    | 1    | payload length: 240bits, no payload header  |
| sco                | HV2    | 1    | payload length: 240bits, no payload header  |
| sco                | HV3    | 1    | payload length: 240bits, no payload header  |
| sco                | DV     | 1    | combined data - voice packet. 150 + 80 bits |



## BLE

The LE system operates in the 2.4 GHz ISM band at 2400-2483.5 MHz. The LE system uses 40 RF channels. These RF channels have center frequencies 2402 + k * 2 MHz, where k = 0, ..., 39. 

The operation of the Link Layer can be described in terms of a state machine
with the following states:

- Standby State
- Advertising State
- Scanning State
- Initiating State
- Connection State
- Synchronization State 

![1571154447991](E:\markdown\bt stack.assets\1571154447991.png)



As specified in [Vol 6] Part A, Section 2, 40 RF channels are defined in the 2.4GHz ISM band. These RF channels are allocated into three LE physical channels: advertising, periodic, and data.

The advertising physical channel uses all 40 RF channels for discovering devices, initiating a connection and broadcasting data.

### LE physical channels 

#### LE piconet physical channel  

used for communication between connected devices and is associated with a specific piconet 

#### LE advertising physical channel 

used for broadcasting advertisements to LE devices. These advertisements can be used to discover, connect, or send user data to scanner or initiator devices 

#### periodic physical channel  

used to send user data to scanner devices in periodic advertisements at a specified interval. 

## 术语

- Adaptive Frequency Hopping (AFH)  自适应跳频
- device access code (DAC)
- inquiry access code (IAC)
- Segmentation and Reassembly (SAR)
- L2CAP Flow & Error Control (FEC)
- Basic information frame (B-frame)
- Information frame (I-frame)
- Supervisory frame (S-frame)：used in Enhanced Retransmission Mode, Retransmission Mode, and Flow Control Mode. It contains protocol information only, encapsulated by a Basic L2CAP header, and no SDU data.    used to  acknowledge I-frames and request retransmission of I-frames  
- Control frame (C-frame)
- Group frame (G-frame)  单纯用于无连接的 l2cap，用于le？
- Credit-based frame (K-frame)
- Data Link Connection Identifier  （  DLCI ）