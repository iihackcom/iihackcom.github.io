---
layer: article
title: Vulnhub靶机无法获得IP
categories: pentest vulnhub
---

## 一般情况

将[Kali Linux](https://www.iihack.com/pages/tools/Kali-Linux.html)和[Vulnhub](https://www.vulnhub.com/)靶机通过设置`nat网络`连接。

### 添加新NAT网络

以VirtualBox 为例，先在VirtualBox *全局设定-网络*  点击加号  *添加新NAT网络*

![](https://static.iihack.com/vulnhub/ip/1.png)

### 设置NAT网络

将Kali Linux和靶机网络分别设置为`NAT网络`，界面名称选刚才设置的`NatNetwork`

![](https://static.iihack.com/vulnhub/ip/3.png)

![](https://static.iihack.com/vulnhub/ip/2.png)

### 内网扫描

Kali Linux上执行命令`sudo arp-scan -l`进行C段arp扫描

正常情况下，靶机网卡信息对的上，且获得`nat`网络IP

![](https://static.iihack.com/vulnhub/ip/4.png)

## 异常情况

靶机无法获得IP，Kali Linux咋扫也扫不到靶机IP

### 原因分析

多数情况是因为网卡名称对不上导致

### 处理异常

1. 重启系统，鼠标不停点进虚拟机同时不停按下键，停留开机 grub 选择菜单那里，按上键到第一行按`e`。

   ![](https://static.iihack.com/vulnhub/ip/5.png)

2. 将光标移动到`linux`开头的那一行，将原来的“`ro`”改为“`rw`”，在后面追加`init=/bin/sh`，（若需更好的调试，可将`quiet`删去），改完后按 Ctrl+X或F10 启动系统。

   ![](https://static.iihack.com/vulnhub/ip/6.png)

   改成

   ![](https://static.iihack.com/vulnhub/ip/7.png)

3. 成功进入系统。

   看到了熟悉的 # 输入`ip a`

   ![](https://static.iihack.com/vulnhub/ip/8.png)

   注意看箭头部分的网卡名称`enp0s17`，记下来

4. 修改配置文件

   - Ubuntu系统新版本

   需修改的文件`/etc/netplan/*.yaml`

   ![](https://static.iihack.com/vulnhub/ip/9.png)

   修改倒数第二行网卡名称为自己的网卡名称

   ![](https://static.iihack.com/vulnhub/ip/10.png)

   - Debian系统 和Ubuntu系统旧版本

     需修改文件`/etc/network/interfaces` ，修改两处网卡名称为自己的网卡名称

5. 重启完成
