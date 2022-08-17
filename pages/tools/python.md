---
title: Python
tags: tools pentest
---

- 交互式shell

nc反弹shell后提升为交互式shell，最好先执行一下`whereis python`，确定是执行`python`还是`python3.X` 此处以`python`为例。

```bash
python -c 'import pty; pty.spawn("/bin/bash")' 
```

- 起http服务

测试中为了下载文件，可在808端口起web服务

```bash
python -m "http.server" 808
```
