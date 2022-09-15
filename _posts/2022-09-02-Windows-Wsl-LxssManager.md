---
layer: article
title: 关闭/重启 Windows 10 子系统（WSL) 
tags: tips
---

## `LxssManager`

关闭重启`LxssManager`服务

```powershell
net stop LxssManager
net start LxssManager
```

## `wsl`

直接使用`wsl`关闭

```powershell
wsl --shutdown
```

