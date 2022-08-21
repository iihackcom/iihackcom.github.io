---
layout: article
title: cewl
tags: tools
---

抓取页面文字存为字典文件pass


```bash
$ cewl http://www.midwest.htb/ -w pass
$ cewl -d 5 -m 5 http://www.midwest.htb/ -w pass
```

配合John生成增强字典pass2

```bash
$ john --wordlist=pass --rules=best64 --stdout >pass2
```

















-------

靶机推荐：

<https://www.vulnhub.com/entry/loly-1,538/>

<https://www.vulnhub.com/entry/midwest-101,692/>
