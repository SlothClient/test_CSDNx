# 从零到可用：在手机上用 Termux + Git + Obsidian 打造稳定同步环境（踩坑全记录）
> hey 这里是不做超级小白 喜欢我的内容的话请多多支持我~

> 这篇文章不是“标准教程”，而是我真实从踩坑到跑通的一整套过程总结。
> 适合：想在手机上用 Obsidian + Git 同步，但被各种问题折磨过的人。

---

# 🧠 一、为什么不用 Obsidian Git 插件？

我一开始也是直接用 Obsidian Git 插件，但很快就遇到：

* ❌ 拉不下来（网络问题）
* ❌ 卡死 / 超时
* ❌ 大仓库直接崩

👉 本质原因：

**手机环境 + Git 插件 ≈ 不稳定组合**

---

👉 解决思路：

```text
Obsidian 负责写
Termux 负责 Git
```

---

# 🚀 二、Termux 正确安装方式

## ❌ 不要用 Play 商店版

* 已停止更新
* 很多包会报错

---

## ✅ 正确方式

👉 用 GitHub 或 F-Droid

👉 github渠道下载不懂各种版本的盲选：

```text
universal.apk（通用版）
```

---

## 🧠 架构解释（顺便涨知识）

* arm64-v8a → 64位手机（主流）
* armeabi-v7a → 老设备
* v8a / v7a 中的 “a” = Application（不是 alpha）

---

# ⚙️ 三、初始化配置

## 1️⃣ 更新系统

```bash
pkg update && pkg upgrade -y
```

---

## 2️⃣ 处理配置文件提示（重点）

升级时会问你：

### 👉 openssl.cnf

```text
👉 选 N（保留）
```

---

### 👉 sources.list

```text
👉 选 Y（更新源）
```

---

### 👉 bash.bashrc

```text
👉 选 N（保留你的环境）
```

---

👉 记住这个规则：

```text
系统源 → Y
用户配置 → N
```

---

## 3️⃣ 换国内源（在国内的话）

```bash
termux-change-repo
```

推荐选择中国大陆的mirrors group

---

# 🔐 四、Git + SSH（关键步骤）

## 1️⃣ 生成 SSH key

```bash
ssh-keygen -t ed25519 -C "termux"
```

解释：

* `-t` → 算法类型
* `-C` → 备注（不影响安全）

跳出的各种选择可以一路回车，后续使用也很方便，若对安全要求高可以设置pass，不过就是要记住了，忘了还得重配

---

## 2️⃣ 添加到 GitHub

```bash
cat ~/.ssh/id_ed25519.pub
```

复制 → GitHub → SSH Keys

title就给iphone17_termux，记得改成自己的机型，这里只是举例

---

## 3️⃣ 首次连接

```bash
ssh -T git@github.com
```

会提示：

```text
Are you sure you want to continue connecting?
```

👉 输入：

```bash
yes
```

---

👉 这是 SSH 第一次“信任确认”，正常现象

---

## ⚠️ 端口问题（你可能会遇到）

```text
Connection closed by xxx port 22
```

👉 用：

```bash
ssh -T git@github.com -p 443
```

---

# 📦 五、修复 Obsidian Git 问题
一切都配置好之后，来到obsidian的part，首先说一下小白写的场景是你从前已经在使用手机版的obsidian配合git插件进行创作了，所以你在github上已经有了一个obsidian_notes的仓库，你遇到了插件pull和push失败的情况。随着上面termux part的完成，直接进入到自己的vault路径做pull（如果手机上有修改先add+commit
## ❗ 典型问题

```text
git pull 要输入账号密码
```

👉 原因：

```text
你用的是 HTTPS，不是 SSH
```
`git remote -v`看一下就知道，如果之前用的是https方式推荐改ssh，更稳

---

## ✅ 修复方法

```bash
git remote set-url origin git@github.com:用户名/仓库.git
```

---

👉 再执行：

```bash
git pull --rebase
```

👉 不再需要密码

---

# 📱 六、Termux 权限问题

执行：

```bash
termux-setup-storage
```

会请求：

👉 相机 / 文件 / 媒体权限

---

## 🧠 这意味着什么？

👉 Termux 可以访问：

```text
/sdcard/
```

包括：

* 照片
* 下载
* 社交软件文件

---

## ⚠️ 是否安全？

👉 结论：

* ✔ 正常用没问题
* ❗ 风险在你运行的脚本

---

👉 **核心原则：**

```text
不要运行不明脚本
不要玩不明破解软件
```

---

# ⚡ 七、为什么锁屏后我的pull中断了？

小白做pull的时候有600多个objects要receive，中途手机自动锁屏了，直接报错：
![锁屏后pull中断](从零到可用：在手机上用 Termux + Git + Obsidian 打造稳定同步环境（踩坑全记录）.assets/47b90e63de6a550c.png)


---

## 📱 原因

👉 手机锁屏后：

* 限制网络
* 降低 CPU
* 直接断连接

建议长按应用进入应用信息，找到省电策略改成无限制（不行再给一个termux-wake-lock



---

# 🧠 八、pull太久真正的性能瓶颈

👉 不是网络

👉 是：

```text
Obsidian 仓库里有大量图片
```

---

## ❗ 为什么图片是灾难？

因为 Git：

* 擅长文本 ✅
* 不擅长二进制 ❌

---

👉 结果：

* 仓库巨大
* pull 慢
* 手机容易断

---

# 🚀 九、三种优化方案

---

## 方案1：Git LFS

```bash
pkg install git-lfs
git lfs install
git lfs track "*.png" "*.jpg"
```

👉 图片走 LFS
👉 Git 只存指针

---

---

## 方案2：图床

👉 思路：

```text
图片不进 Git，用 URL
```

---

### 示例

```markdown
![img](https://xxx.com/a.png)
```

---

## 推荐组合：

```text
Obsidian + PicGo + GitHub 图床
```

---

👉 优点：

* 仓库极小
* 拉取飞快
* 手机体验最好

---

---

## 方案3：浅克隆

```bash
git clone --depth=1
```

👉 只拉最新版本

---

# 🎯 十、最终实现

👉 某小白的方案：

```text
Obsidian（写） + Termux（Git） + SSH + 图床
```

---

## 日常流程

```bash
git pull --rebase
写笔记
git add .
git commit -m "update"
git push
```

---

👉 或一键：

```bash
alias gsync="git add . && git commit -m 'update' && git pull --rebase && git push"
```

---

# 📌 总结

👉 你要记住三件事：

---

## 1️⃣ Termux 是稳定的 Git 环境

👉 比 Obsidian 插件靠谱

---

## 2️⃣ SSH 一次配置，永久省心

👉 不要再用 HTTPS

---

## 3️⃣ 图片是性能杀手

👉 用 LFS 或图床解决

---

# 🚀 最后的话

你现在其实已经完成了一个很难的过程：

```text
手机 → Linux → Git → SSH → 开发环境
```

👉 这套能力是“跨平台开发”的基础

---

如果你还想继续升级，可以走这些方向：

* 用手机跑 Linux（Ubuntu）
* VSCode 远程连接 Termux
* 自动化 Git 同步脚本

---

👉 搞机党可以继续折腾 😄

---

## Cover 图

![cover_1](从零到可用：在手机上用 Termux + Git + Obsidian 打造稳定同步环境（踩坑全记录）.assets/2f30d6398ff28a9a.png)
