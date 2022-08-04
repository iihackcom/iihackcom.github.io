---
title: Command Line Fun
tags: oscp
---

# 命令

## export

设置变量，设置tip为靶机ip 192.168.2.1 ，ping 靶机

```bash
$ export tip=192.168.2.1   

$ ping $tip -c3
PING 192.168.2.1 (192.168.2.1) 56(84) bytes of data.
64 bytes from 192.168.2.1: icmp_seq=1 ttl=63 time=1.91 ms
64 bytes from 192.168.2.1: icmp_seq=2 ttl=63 time=1.36 ms
64 bytes from 192.168.2.1: icmp_seq=3 ttl=63 time=2.74 ms

--- 192.168.2.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 1.360/2.004/2.741/0.567 ms

```

## echo

```bash
$ echo 'Hello world'
Hello world
                                                                
$ echo $path
/usr/local/sbin /usr/sbin /sbin /usr/local/bin /usr/bin /bin /usr/local/games /usr/games
                                                                         
$ echo $HOME
/home/kali
```




## grep

```bash
$ ls -la /usr/bin | grep zip
-rwxr-xr-x  3 root   root       39144 Sep  5  2019 bunzip2
-rwxr-xr-x  3 root   root       39144 Sep  5  2019 bzip2
-rwxr-xr-x  1 root   root       18584 Sep  5  2019 bzip2recover
-rwxr-xr-x  1 root   root       26776 Aug 16  2019 funzip
-rwxr-xr-x  1 root   root        3516 Jul  5 00:20 gpg-zip
-rwxr-xr-x  2 root   root        2346 Apr  8 19:05 gunzip
-rwxr-xr-x  1 root   root       97496 Apr  8 19:05 gzip
-rwxr-xr-x  2 root   root      186664 Aug 16  2019 unzip
-rwxr-xr-x  1 root   root       84248 Aug 16  2019 unzipsfx
-rwxr-xr-x  1 root   root      216256 Apr 22  2017 zip
-rwxr-xr-x  1 root   root       93816 Apr 22  2017 zipcloak
-rwxr-xr-x  1 root   root       50718 Oct 19  2020 zipdetails
-rwxr-xr-x  1 root   root        2953 Aug 16  2019 zipgrep
-rwxr-xr-x  2 root   root      186664 Aug 16  2019 zipinfo
-rwxr-xr-x  1 root   root       89488 Apr 22  2017 zipnote
-rwxr-xr-x  1 root   root       93584 Apr 22  2017 zipsplit

$ grep ss /etc/passwd 
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
sshd:x:112:65534::/run/sshd:/usr/sbin/nologin
smmsp:x:116:122:Mail Submission Program,,,:/var/lib/sendmail:/usr/sbin/nologin

```

## sed

```bash
$ echo "I need to try hard" | sed 's/hard/harder/'
I need to try harder
```

## cut

```bash
$ echo "I hack binaries,web apps,mobile apps, and just about anything else"| cut -f 2 -d ","
web apps
```

```bash
$ cut -d ":" -f 1 /etc/passwd
root
daemon
bin
sys
sync
games
man
lp
mail
news
uucp
proxy
www-data
backup
list
irc
gnats
nobody
systemd-network
systemd-resolve
systemd-timesync
messagebus
syslog
_apt
tss
uuidd
tcpdump
landscape
pollinate
usbmux
sshd
systemd-coredump
kali
```

## awk

```bash
$ echo "hello::there::friend" | awk -F "::" '{print $1, $3}'
hello friend

```

## uniq

```bash
$ cat 2.txt 
a
b
b
c
c
c
d
d
d
d

$ cat 2.txt|uniq
a
b
c
d
```



## wc

## nano

## vi/vim

```bash
$ vim a

:set nu
```

## seq

```bash
$ seq 9
1
2
3
4
5
6
7
8
9
```



## comm

## diff

## vimdiff

## ping

## fg

## jobs

## kill

## wget

## curl

## axel

## alias

## mkdir

```bash
$ mkdir i
$ ls
2.txt  3.txt  i
```

## ls

- 按文件大小，由大到小排序

```bash
$ ls -lh --sort=size /usr/share/php7.4-common/common 
total 68K
-rw-r--r-- 1 root root 75 Jun 13 21:43 tokenizer.ini
-rw-r--r-- 1 root root 74 Jun 13 21:43 calendar.ini
-rw-r--r-- 1 root root 74 Jun 13 21:43 fileinfo.ini
-rw-r--r-- 1 root root 73 Jun 13 21:43 gettext.ini
-rw-r--r-- 1 root root 73 Jun 13 21:43 sockets.ini
-rw-r--r-- 1 root root 73 Jun 13 21:43 sysvmsg.ini
-rw-r--r-- 1 root root 73 Jun 13 21:43 sysvsem.ini
-rw-r--r-- 1 root root 73 Jun 13 21:43 sysvshm.ini
-rw-r--r-- 1 root root 71 Jun 13 21:43 ctype.ini
-rw-r--r-- 1 root root 71 Jun 13 21:43 iconv.ini
-rw-r--r-- 1 root root 71 Jun 13 21:43 posix.ini
-rw-r--r-- 1 root root 71 Jun 13 21:43 shmop.ini
-rw-r--r-- 1 root root 70 Jun 13 21:43 exif.ini
-rw-r--r-- 1 root root 70 Jun 13 21:43 phar.ini
-rw-r--r-- 1 root root 69 Jun 13 21:43 ffi.ini
-rw-r--r-- 1 root root 69 Jun 13 21:43 ftp.ini
-rw-r--r-- 1 root root 69 Jun 13 21:43 pdo.ini

```

- 按时间排序

```bash
$ ls -lh --sort=time /usr/share/                
total 712K
drwxr-xr-x    2 root root 4.0K Jul 16 06:20 binfmts
drwxr-xr-x    2 root root 4.0K Jul 16 06:20 applications
drwxr-xr-x    2 root root 4.0K Jul 16 06:20 pixmaps
drwxr-xr-x 1074 root root  36K Jul 14 06:56 doc
drwxr-xr-x    3 root root 4.0K Jul 14 06:56 gitweb
drwxr-xr-x   30 root root 4.0K Jul 14 06:56 perl5
drwxr-xr-x    2 root root 4.0K Jul  6 17:37 unattended-upgrades
drwxr-xr-x    3 root root 4.0K Jul  6 17:37 ModemManager
drwxr-xr-x    2 root root 4.0K Jul  6 17:37 keyrings
drwxr-xr-x    4 root root 4.0K Jul  6 17:37 update-notifier
drwxr-xr-x    2 root root 4.0K Jul  6 06:47 info
drwxr-xr-x    2 root root 4.0K Jul  6 06:47 gnupg
drwxr-xr-x    2 root root 4.0K Jun 30 06:49 aclocal
drwxr-xr-x    2 root root 4.0K Jun 19 10:23 openssh
drwxr-xr-x    2 root root 4.0K Jun 19 10:23 pkgconfig
drwxr-xr-x    2 root root 4.0K Jun 19 10:23 systemd
drwxr-xr-x    2 root root 4.0K Jun 19 10:23 pam-configs
drwxr-xr-x    2 root root 4.0K Jun 19 10:23 libc-bin
drwxr-xr-x  202 root root 4.0K Jun 19 10:23 locale
drwxr-xr-x    2 root root 4.0K Jun 19 10:23 locales
drwxr-xr-x    4 root root 4.0K Jun 19 10:23 i18n
drwxr-xr-x    2 root root 4.0K Jun  9 06:25 doc-base
drwxr-xr-x    2 root root 4.0K Jun  9 06:25 et
drwxr-xr-x   53 root root 4.0K May 28 06:22 bug
drwxr-xr-x    2 root root 4.0K May 28 06:22 dpkg
drwxr-xr-x    6 root root 4.0K May 20 06:52 apport
```

## pwd

```bash
$ pwd
/usr/share/recon-ng
```

## more

```bash
$ more /var/log/dmesg
[    0.000000] kernel: microcode: microcode updated early to revision 0xf0, date = 2021-11-12
[    0.000000] kernel: Linux version 5.4.0-121-generic (buildd@lcy02-amd64-013) (gcc version 9.4.0 (Ubuntu 9.4.0-1ubuntu1~20.04.1)) 
#137-Ubuntu SMP Wed Jun 15 13:33:07 UTC 2022 (Ubuntu 5.4.0-121.137-generic 5.4.189)
[    0.000000] kernel: Command line: BOOT_IMAGE=/vmlinuz-5.4.0-121-generic root=/dev/mapper/ubuntu--vg-ubuntu--lv ro
[    0.000000] kernel: KERNEL supported cpus:

```

## less

```bash
$ less /var/log/dmesg
```

## cat

打印文件内容（全部）

```bash
$ cat 2.txt 
a
b
c
d
1
2
3
4
```

## tac

反向输出

```bash
$ tac 2.txt 
4
3
2
1
d
c
b
a
```

## tail

默认10行

```
tail /var/log/dmesg

```

打印25行

```bash
tail -25 /var/log/dmesg

```

## watch

每隔三秒查看文件`/var/log/dmesg`更新

```bash
$ watch -n 3 tail /var/log/dmesg
Every 3.0s: tail /var/log/dmesg                                                                         host: Sat Jul 23 09:11:05 2022

```

## cp

进入文件夹1，复制2.txt到3.txt

```bash
cd 1
cp 2.txt 3.txt
```

复制文件夹1到文件夹`/tmp`目录文件夹名称4

```bash
cp -r 1 /tmp/4
```

## rm

删除文件`/tmp/4/3.txt`

```bash
rm /tmp/4/3.txt 

```

删除目录`/tmp/4`

```bash
rm -r /tmp/4
```

## top

直接输入`top`查看系统信息

`k`杀进程

## free

查看内存信息

```bash
$ free
              total        used        free      shared  buff/cache   available
Mem:       16275796     2456204     7067012       38860     6752580    13512576
Swap:       4194300           0     4194300


$ free -m
              total        used        free      shared  buff/cache   available
Mem:          15894        2398        6901          37        6594       13195
Swap:          4095           0        4095

```

## ps

```bash
$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 Jul06 ?        00:02:19 /sbin/init
root           2       0  0 Jul06 ?        00:00:00 [kthreadd]
root           3       2  0 Jul06 ?        00:00:00 [rcu_gp]
root           4       2  0 Jul06 ?        00:00:00 [rcu_par_gp]
root           6       2  0 Jul06 ?        00:00:00 [kworker/0:0H-kblockd]
```

```bash
$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 169740 13092 ?        Ss   Jul06   2:19 /sbin/init
root           2  0.0  0.0      0     0 ?        S    Jul06   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   Jul06   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   Jul06   0:00 [rcu_par_gp]
root           6  0.0  0.0      0     0 ?        I<   Jul06   0:00 [kworker/0:0H-kblockd]
root           9  0.0  0.0      0     0 ?        I<   Jul06   0:00 [mm_percpu_wq]
```

## ifconfig

linux系统查看网络信息`ifconfig`

关闭 开启网卡`eth0`

```bash
ifconfig eth0 down

ifconfig eth0 up
```

## netstat

查看网络连接信息

```bash
$ netstat -pantu
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:13443           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:5700            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:8388            0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:587           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:8888            0.0.0.0:*               LISTEN      -    
```

## sort

```bash
$ cat 3.txt 
a
b
b
c
c
c
a
b
c
a

$ cat 3.txt|sort
a
a
a
b
b
b
c
c
c
c
```



## route

## mount

```bash
$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=475788k,nr_inodes=118947,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=99836k,mode=755,inode64)
/dev/vda1 on / type ext4 (rw,relatime,errors=remount-ro)
```



## dmesg

```bash
$ sudo dmesg
[    0.000000] Linux version 5.18.0-kali5-amd64 (devel@kali.org) (gcc-11 (Debian 11.3.0-3) 11.3.0, GNU ld (GNU Binutils for Debian) 2.38) #1 SMP PREEMPT_DYNAMIC Debian 5.18.5-1kali6 (2022-07-07)
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.18.0-kali5-amd64 root=UUID=49d8d790-3180-49db-94f3-c3ab0985e17e ro quiet
[    0.000000] x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
[    0.000000] x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
[    0.000000] x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
```

## find

```bash
$ find . -name a.txt 
```

不区分大小写
```bash
$ find . -iname a.txt 
```
## whereis

```bash
$ whereis python
python: /usr/bin/python /usr/share/python /usr/share/man/man1/python.1.gz

$ whereis -b python
python: /usr/bin/python /usr/share/python
```



## which

```bash
$ which ping
/usr/bin/ping
```



## cd

```bash
$ pwd
/usr/share/php

$ cd .
$ pwd  
/usr/share/php

$ cd ..
$ pwd
/usr/share

$ cd /usr/share/php7.4-common/common
$ pwd
/usr/share/php7.4-common/common

$ cd .....
$ pwd
/

```

## gzip

解压.gz 文件

```bash
$ gzip rockyou.txt.gz -d
```



------

# 管道

-------

# Shell
