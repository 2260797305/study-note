# LE audio

## 名称解释

### Published Audio Capability (PAC)  

服务发布？

### Unicast client and Unicast server

 renamed from the ASCP client and ASCP server as defined in the Basic Audio Profile (BAP) specification [15]. 

单播？

### Volume controller and Volume renderer

 Adopted from the Volume Control Service (VCS) [17] and Volume Control Profile (VCP) [16] specifications. 

音量控制

### Call control client and server:

 Adopted from the Call Control Service (CCS)  [19] and Call Control Profile (CCP) [18] specifications to allow control of a phone call or VoIP call. 

通话控制

### Media control client and server

 Adopted from the Media Control Service (MCS) [21] and Media Control Profile (MCP) [20] specifications. The media control client typically presents a UI t  

媒体控制

### Set coordinator and Set member:

 Adopted from the Coordinated Set Identification Profile (CSIP) [23] and Service (CSIS) [24] specifications. enables the discovery of devices that are part of a coordinated set.   

没看懂

### Broadcast scan delegator and assistant: 

The delegator depends on the assistant to scan for broadcast audio on the delegator’s behalf. The assistant is any device that can scan for broadcast announcements and share the received information with the delegators. 

### Broadcast Source and Sink are used as defined in BAP [15].

### The Generic Audio Framework (GAF)  







## Telephony and Media Audio Profile (TMAP)   

![image-20200916233929363](LE%20audio.assets/image-20200916233929363.png)

### role

**Call Gateway (CG)**   电话 app

**Call Terminal (CT** **)**   耳机 通话

![image-20200916235056095](LE%20audio.assets/image-20200916235056095.png)

**Unicast Media Sender (UMS)**   发送audio 数据到有限的sink(比如播放器)

**Unicast Media Receiver (UMR)**   接收非广播的流（如无线音响）

![image-20200916235115952](LE%20audio.assets/image-20200916235115952.png)

**Broadcast Media Sender (BMS)**   

**Broadcast Media Receiver (BMR)** 

**Broadcast Scan Assistant (BSA)**   扫描其他人？（比如手机，手表）

**Broadcast Scan Delegator (BSD)**     被扫描（比如耳机）

![image-20200916235202181](LE%20audio.assets/image-20200916235202181.png)



![image-20200916235614628](LE%20audio.assets/image-20200916235614628.png)