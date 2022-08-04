---
title: 分割文件传输
tags: pentest
---

又遇到大文件传输失败了，这次直接是90KB限制。

`split.exe -b 80000 GoogleChrome.exe`

然后在靶机上`type x* >GoogleChrome.exe`
