@[TOC](windows中使用cmd/powershell查杀进程)

在 Windows 上，你可以用 **cmd** 或 **PowerShell** 来查找并杀掉进程，比如 `redis-server.exe` 这样的 Redis 服务进程。

---

### 1. 查看进程

#### CMD

```cmd
tasklist | findstr redis
```

会显示类似：

```
redis-server.exe         1234 Console    1     25,000 K
```

其中 `1234` 就是 PID。

#### PowerShell

```powershell
Get-Process redis
```

会显示进程名、PID、CPU、内存等。

---

### 2. 杀掉进程

#### CMD

* 按 **PID 杀**

```cmd
taskkill /PID 1234 /F
```

* 按 **进程名杀**

```cmd
taskkill /IM redis-server.exe /F
```

#### PowerShell

* 按 **进程名杀**

```powershell
Stop-Process -Name redis -Force
```
![在这里插入图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/fb/fb56927a445b13e29940b97ad78d261cdfb1a8b6297c04c78696c84ee8013735.png)


* 按 **PID 杀**

```powershell
Stop-Process -Id 1234 -Force
```

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/f0/f06dc2a6d83c3b3aa46713492274a285188fc674fbee9fc9fc9fe8be9ecaabc4.png)
