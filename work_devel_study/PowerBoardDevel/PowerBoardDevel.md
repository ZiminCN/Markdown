# 电源板&电池开发记录

***本文的主要目的是记录A2机器人电源板相关外设的开发情况和方案的记录与评估***

***本文部分内容会大量参考网上分享的相关文档和工程***

快充模块

使用USB-PD（**Power Delivery**）协议实现快充，该协议基于TypeC接口。

因此需要了解Type-C接口的一些基本信息，Type-C插头（公头）和母座（母头）每个均有24个引脚。如下图所示

![](pic/typeC.png)

具体的引脚定义如下:

| Pin  | 名称   | Function                       | Pin                              | 名称   | Function                       |
| :--: | ------ | ------------------------------ | -------------------------------- | ------ | ------------------------------ |
|  A1  | GND    | 接地                           | B12                              | GND    | 接地                           |
|  A2  | SSTXp1 | SuperSpeed差分信号#1，TX，正   | B11                              | SSRXp1 | SuperSpeed差分信号#1，RX，正   |
|  A3  | SSTXn1 | SuperSpeed差分信号#1，TX，负   | B10                              | SSRXn1 | SuperSpeed差分信号#1，RX，负   |
|  A4  | VBUS   | 总线电源                       | B9                               | VBUS   | 总线电源                       |
|  A5  | CC1    | Configuration channel          | B8                               | SBU2   | Sideband use                   |
|  A6  | Dp1    | USB2.0差分信号，position 1，正 | B7                               | Dn2    | USB2.0差分信号，position 2，负 |
|  A7  | Dn1    | USB2.0差分信号，position 1，负 | USB2.0差分信号，position 1，正B6 | Dp2    | USB2.0差分信号，position 2，正 |
|  A8  | SBU1   | Sideband use                   | B5                               | CC2    | Configuration channel          |
|  A9  | VBUS   | 总线电源                       | B4                               | VBUS   | 总线电源                       |
| A10  | SSRXn2 | SuperSpeed差分信号#2，RX，负   | B3                               | SSTXn2 | SuperSpeed差分信号#2，RX，负   |
| A11  | SSRXp2 | SuperSpeed差分信号#2，RX，正   | B2                               | SSTXp2 | SuperSpeed差分信号#2，RX，正   |
| A12  | GND    | 接地                           | B1                               | GND    | 接地                           |

简单来说，Type-C口有4对TX/RX分线，2对USBD+/D-，一对SBU，2个CC，另外还有4个VBUS和4个地线。