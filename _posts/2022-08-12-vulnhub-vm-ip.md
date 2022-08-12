---
title: Vulnhub靶机无法获得IP
categories: pentest vulnhub
---

一般将Kali Linux和Vulnhub靶机通过设置`nat`网络连接。

先在VirtualBox *全局设定-网络* 点击  *添加新NAT网络*，将Kali Linux和靶机网络设置为`NAT网络`

![](https://static.iihack.top/vulnhub/0/1.PNG)

Kali Linux上执行命令`sudo arp-scan  -l`进行C段arp扫描

正常情况如下图，网卡信息对的上，且获得`nat`网络IP

![](https://static.iihack.top/vulnhub/0/2.PNG)

不正确的时候，靶机无法获得IP的情况，kali上咋扫也扫不到

多数情况是因为网卡名称对不上导致，修改一下即可：

1. 重启系统，鼠标点进虚拟机不停按下键，停留开机 grub 选择菜单那里，按上键到第一行按`e`。

2. 将光标移动到`linux`开头的那一行，将原来的“`ro`”改为“`rw`”，并在后面追加`init=/bin/sh`为了更好的调试，可以将quiet删去，改完后按 Ctrl+X或F10 启动系统。

   ![](https://static.iihack.top/vulnhub/0/3.PNG)

3. 成功进入单用户模式。看到了熟悉的 # 输入ip a

   ![](https://static.iihack.top/vulnhub/0/4.PNG)

这里我已经改好获得了IP，注意看enp0s3部分的网卡名称

4. 修改配置文件

- ubuntu系统新版本需修改文件`/etc/netplan/*.yaml` ，修改倒数第二行网卡名称为自己的网卡名称

    ![](https://static.iihack.top/vulnhub/0/5.PNG)

- debian系统 需修改文件/etc/network/interfaces ，修改倒数第二行网卡名称为自己的网卡名称
