# 当Windows本地Redis与Docker的Redis容器共享6379端口：一场诡异的和平共处

## 一、问题背景

最近在本地开发时遇到一个挺“反直觉”的问题：

* Windows 11 本机已经启动了一个 Redis（默认端口 `6379`）
* 使用 Docker Desktop 再起一个 Redis 容器，同样映射端口：

  ```yaml
  ports:
    - "6379:6379"
  ```
* **docker-compose up 正常，没有任何端口冲突报错**

按常识来说，这应该直接报错：`port is already allocated`。但现实是——**两个 Redis 居然“共存”了**。

更奇怪的是：

* 用 `redis-cli` 连 `127.0.0.1:6379` → 命中的是**本机 Redis**
* 用 RESP.app 连 `6379` → 也是**本机 Redis**
* 只有把 Docker 改成 `6380:6379`，才能访问容器里的 Redis

这说明：**Docker 映射成功了，但流量没有打到容器里。**

---

## 二、现象复盘

### 1️⃣ docker-compose 配置

```yaml
redis:
  image: redis:7-alpine
  container_name: docker-redis
  restart: always
  ports:
    - "6379:6379"
```

---

### 2️⃣ 端口占用情况

```bash
netstat -ano | findstr :6379
```

输出：

```
TCP    0.0.0.0:6379     LISTENING       5168
TCP    127.0.0.1:6379   LISTENING       6388
TCP    [::]:6379        LISTENING       5168
TCP    [::1]:6379       LISTENING       26412
```

### 3️⃣ 进程归属

```bash
tasklist | findstr 6388
redis-server.exe 6388
```

```bash
tasklist | findstr 5168
com.docker.backend.exe 5168
```

👉 结论：

| 端口绑定           | 进程             |
| -------------- | -------------- |
| 127.0.0.1:6379 | 本机 Redis       |
| 0.0.0.0:6379   | Docker backend |

---

## 三、核心问题

> 为什么两个进程都能“监听”6379端口，而且不冲突？

---

## 四、底层原理分析

### 1️⃣ 关键点：绑定地址不同

| 绑定方式             | 含义    |
| ---------------- | ----- |
| `127.0.0.1:6379` | 仅本机回环 |
| `0.0.0.0:6379`   | 所有网卡  |

理论上：

> **0.0.0.0 应该包含 127.0.0.1，但在 Windows + Docker Desktop 场景下不是简单覆盖关系**

---

### 2️⃣ Docker Desktop 的特殊实现（重点）

在 Windows 上，Docker 并不是直接用 Linux 网络栈，而是：

* 通过 **Hyper-V / WSL2 虚拟网络**
* 使用 **用户态代理（userland proxy）**
* 由 `com.docker.backend.exe` 接管端口

👉 关键行为：

> Docker 并不是直接 bind 真实的 6379，而是做了一层“转发代理”

---

### 3️⃣ 请求流向（重点理解）

当你执行：

```bash
redis-cli -h 127.0.0.1 -p 6379
```

请求路径是：

```
客户端 → 127.0.0.1 → 命中本机 Redis
```

不会经过 Docker

---

如果访问：

```
<本机IP>:6379
```

可能才会走：

```
客户端 → 0.0.0.0 → Docker backend → 容器 Redis
```

---

### 4️⃣ 为什么不会冲突？

因为：

| 组件       | 行为               |
| -------- | ---------------- |
| 本机 Redis | 只监听 127.0.0.1    |
| Docker   | 监听 0.0.0.0（通过代理） |

Windows TCP/IP 栈允许这种“分裂绑定”。

---

## 五、验证方法

### ✅ 方法1：查看绑定地址

```bash
netstat -ano | findstr :6379
```

重点看：

* `127.0.0.1` → 本机服务
* `0.0.0.0` → Docker

---

### ✅ 方法2：测试不同入口

```bash
redis-cli -h 127.0.0.1 -p 6379 INFO server | findstr redis_version
```

👉 我本机redis版本

```bash
redis-cli -h 192.168.0.1 -p 6379 INFO server | findstr redis_version
```
报错`NOAUTH Authentication required.`，`-a`参数加上密码
```bash
redis-cli -h 192.168.0.1 -p 6379 -a test123456 INFO server | findstr redis_version
```
👉 我docker-compose.yml配置的redis版本

---

## 六、为什么 RESP.app 也连不到 Docker？

因为默认连接：

```
127.0.0.1:6379
```

👉 永远优先命中本机 Redis

---

## 七、解决方案

### ✅ 方案1（推荐）：改端口映射

```yaml
ports:
  - "6380:6379"
```

优点：

* 简单直接
* 无歧义

---

### ✅ 方案2：关闭本机 Redis

```bash
net stop redis
```

---

### ✅ 方案3：让本机 Redis 绑定 0.0.0.0

修改 redis.conf：

```conf
bind 0.0.0.0
```

⚠️ 风险：

* 端口冲突概率 ↑
* 安全性 ↓

---

### ✅ 方案4：用容器网络访问（推荐开发环境）

```bash
docker exec -it docker-redis redis-cli
```

或者：

```bash
redis-cli -h host.docker.internal -p 6379
```

---

## 八、经验总结（踩坑复盘）

这次问题本质是：

> **“端口看起来一样，但流量入口不同”**

几个关键认知：

1. **Windows + Docker Desktop ≠ Linux Docker**
2. **0.0.0.0 ≠ 覆盖 127.0.0.1（在该场景下）**
3. Docker 端口映射是“代理”，不是直接占用
4. 本机服务优先级更高（loopback 优先）

---

## 九、最佳实践建议

在日常开发中：

* ✅ 本机服务：用默认端口（6379）
* ✅ Docker 服务：统一 +1（6380、6381…）
* ✅ 或者统一走 Docker（推荐微服务环境）

---

## 结语

这个问题表面是“端口冲突”，本质是：

> **网络栈 + 虚拟化 + 代理机制的组合行为**

如果不了解 Docker Desktop 的实现，很容易被误导。

希望这篇能帮你少踩一个坑 👇
下次看到“端口没冲突但访问错服务”，第一时间就该想到——**流量路径问题**。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/fc/fc920e52dbce57e2fbed7f1e93d74107894ebc2ceaaf1f6b79562eb9e2c82997.png)
