---
layout: mypost
title: PIP 使用清华源
categories: [optimization]

---

因为国内复杂的网络环境，为了加速pip安装可使用清华源。

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple django
```

设置默认安装清华源

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

成功后会显示写入pip.ini的具体路径
