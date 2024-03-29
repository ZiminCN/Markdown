# 								OTA固件更新客户操作手册

###### 									          Author: DirectDriveTech    		Last revision time: 2022.11.15

## 注意与声明

欢迎您使用刑天机器人OTA固件更新操作手册，执行机器人固件更新仅需要您了解一点基本的Linux操作系统知识，推荐您完全按照本操作手册依次执行更新步骤，以便您更好更快的完成一次机器人固件更新。

## Quick Start

1.准备工作：执行固件更新以前，请确保您的机器人拥有较充足的电量并在固件更新前使机器人处于匍匐状态；同时准备好购买刑天机器人时配套的地平线（或树莓派），本次更新将在地平线（或树莓派）上执行；更新时主要通过上位机连接主控板的40Pin的方式做固件更新。您的上位机系统版本参考建议是Ubuntu Linux20.04。

更新前，请您将主控板与地平线（或树莓派）以40Pin的方式连接在一起，正常安装在机器人上：

<img src="/home/zengjianming/Downloads/14260608921015082872.jpg" style="zoom: 50%;" />

2.连接上位机：您可以直接使用上位机的图形化界面或者SSH远程连接的方式控制操作您的上位机，这里我们仅对SSH远程连接方式做介绍。如果您了解SSH连接操作，则您可以略过本步骤继续执行下一步。

SSH远程连接可以快速便捷的帮助您使用个人PC远程控制机器人上位机。如果您的个人PC系统是Linxu，则您可以直接使用默认的终端输入ssh命令，如果您的系统是Windows，则您可以安装MobaXterm软件或者Putty软件做ssh连接。

**首先请您将地平线（或树莓派）连接至网络，并确保网络环境良好；**地平线（或树莓派）通用的连接方式是通过HDMI线将其与一块显示器相连接，在图形化界面中连接网络。

连接网络成功后，请确认您的上位机与个人PC均已开启了ssh服务，您可以向上位机图形化界面终端中输入`systemctl status ssh.service`来确认ssh服务是否处于`active (running)`状态；随后在上位机图形化界面终端中输入`ifconfig`,在打印信息中记住您的IP地址，再在您的个人PC终端上输入

```bash
ssh userid@ip
例如您上位机用户名为diablo，其IP地址为192.168.0.10
那么您应该输入ssh diablo@192.168.0.10
```

随后输入上位机密码，便可远程控制您的上位机。

刑天机器人配套的地平线默认用户名是`root`，密码为`root`；树莓派默认用户是`diablo`，密码为`diablo`。



3.开始固件更新：在您上述步骤都确认完成后，我们将给您发送一个可执行文件，请您将该文件放至地平线（或树莓派）Linux系统上，随后为其赋予可执行权限，在终端输入

```bash
sudo chmod +x 可执行文件名
```

随后执行该文件，在终端中输入

```bash
./可执行文件名
```

并等待固件更新开始

此时您将观察到机器人主控板上的灯由**不断闪烁的蓝灯**转变为**短暂闪烁的白灯**，不久后继续转变为**快速连闪的白灯**；当固件更新的执行完成后，主控板的灯将恢复为不断闪烁的蓝灯。

**<u>请注意：在您确认主控板灯效恢复正常后，先不要立刻关闭机器人电源，先使用遥控控制机器人做下基本动作控制，确定所有控制正常后，关闭机器人电源并重启，再次确认所有控制正常。</u>**

上述操作完成后，固件更新便完成了。





4.异常处理：固件更新过程中会因为您的网络环境、机器人状态等原因造成更新失败；如果不幸发生，甚至您会发现您的主控板灯效变为不断闪烁的白灯，与此同时机器人无法控制，此时您可以再次通过上位机执行可执行文件做固件更新，如果固件更新过程中主控板灯效一直没有变化，始终是不断闪烁的白灯，**请您按下主控板上的reset键**，此时固件更新将继续执行，主控板将恢复正常。

![](/home/zengjianming/Pictures/2022-12-13 09.jpg)