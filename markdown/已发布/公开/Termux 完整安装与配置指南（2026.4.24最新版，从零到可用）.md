
# Termux 完整安装与配置指南（2026.4.24最新版，从零到可用）

> hey 这里是不做超级小白 喜欢我的内容的话请多多支持我~

> 说在前面：termux在Google Play已停更，国内应用商店大多没上架，termux官网提供了两种安装方式：
> ![termux官网推荐安装途径](Termux 完整安装与配置指南（2026.4.24最新版，从零到可用）.assets/50c6337c1c279c1f.png)
> 小白使用的是github，v8a的apk，不知道对自己是否适用请下载universal版本
> - [v8a版本点此下载](https://github.com/termux/termux-app/releases/download/v0.118.3/termux-app_v0.118.3+github-debug_arm64-v8a.apk)
> - [universal版本点此下载](https://github.com/termux/termux-app/releases/download/v0.118.3/termux-app_v0.118.3+github-debug_universal.apk)


---

## 📱 一、安装完成后的第一步

打开 Termux，先做最基础的更新：

```bash
pkg update && pkg upgrade -y
```

👉 作用：

* 更新软件源
* 修复依赖
* 避免后续安装报错

---

## 🌐 二、换国内源（强烈推荐）

默认源在国外，速度很慢，建议换源，输入以下命令：

```bash
termux-change-repo
```

跳出界面如下：

![换源界面](Termux 完整安装与配置指南（2026.4.24最新版，从零到可用）.assets/14b48f317b0ae252.png)
直接enter即可，进入下一个选择：
![group选择](Termux 完整安装与配置指南（2026.4.24最新版，从零到可用）.assets/2d1d0e641bdd9d65.png)
选择Mirrors in Chinese Mainland，也就是国内的镜像链接组，
配置完毕。

---

## 📦 三、安装基础工具（必备）

```bash
pkg install -y git curl wget vim nano unzip zip
```

推荐再装：

```bash
pkg install -y htop tree neofetch
```

👉 用途：

* git：拉代码
* curl/wget：下载资源
* vim/nano：编辑文件
* htop：看性能
* neofetch：装比（gpt说的，我个人不是很喜欢装比）

---

## 🐍 四、安装开发环境（按需选）

### 1️⃣ Python 环境

```bash
pkg install python -y
pip install --upgrade pip
```

测试：

```bash
python
```

---

### 2️⃣ Node.js 环境

```bash
pkg install nodejs -y
```

测试：

```bash
node -v
npm -v
```

---

### 3️⃣ C/C++ 编译环境

```bash
pkg install clang make cmake -y
```

---

### 4️⃣ Java（可选）

```bash
pkg install openjdk-17 -y
```

---

## 🧠 五、优化体验（强烈推荐）

### 1️⃣ 设置存储权限

```bash
termux-setup-storage
```
手机会跳出弹窗请求权限，同意即可，只要别随便运行未知脚本，保持手机不被黑，一般都没问题，小白这里就直接给了权限

👉 允许后可以访问：

```
/sdcard/
```

---

### 2️⃣ 安装 Oh My Zsh（终端美化）

```bash
pkg install zsh -y
chsh -s zsh
```

安装 oh-my-zsh：
>上面装的curl这里就用到了

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

### 3️⃣ 安装常用插件（推荐）

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ~/.zsh/zsh-syntax-highlighting
```

编辑配置：

```bash
nano ~/.zshrc
```

加入：

```bash
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

---

## 🔐 六、SSH（远程连接）

安装：

```bash
pkg install openssh -y
```

启动 SSH：

```bash
sshd
```

查看端口：

```bash
ss -tnlp | grep sshd
```

👉 默认端口通常是 **8022**

连接方式：

```bash
ssh -p 8022 用户名@手机IP
```

---

## 📁 七、常用目录说明

* Home：

```
~/ 
```

* 共享存储：

```
/sdcard/
```

* 下载目录：

```
~/storage/downloads
```

---

## ⚠️ 八、常见坑（一定要看）

### ❌ 1. 不要用 Play 商店版

已停止更新，会各种报错

---

### ❌ 2. 不要混用不同源

容易依赖冲突

---

### ❌ 3. Python pip 报错

解决：

```bash
pip install --upgrade pip setuptools wheel
```

---

### ❌ 4. 权限问题

重新执行：

```bash
termux-setup-storage
```

---

## 🚀 九、进阶玩法（爱折腾的可以试试）

* 安装 Linux（proot-distro）
* 搭建 Web 服务器（nginx / node）
* 跑 AI / 爬虫
* 搭建开发环境（VSCode Remote）

---

## 📌 十、一键初始化（懒人版）

如果你想一步搞定：

```bash
pkg update && pkg upgrade -y && \
pkg install -y git curl wget vim nano unzip zip python nodejs clang make cmake openssh && \
pip install --upgrade pip
```

---

## 🎯 总结

Termux 本质上就是：

👉 **一个运行在安卓上的 Linux 环境**

配置完成后你可以：

* 写代码（Python / Node / C++）
* 跑服务器
* 做自动化
* 学 Linux

---

## 🧩 推荐使用方式

* 📱 手机本地开发：轻量任务
* 💻 SSH + 电脑连接：更舒服
* ☁️ 配合 GitHub：完整开发流

---

如果你看到这里，说明你已经：

👉 从“刚装 Termux”进化成“能用 Termux 的人”了 😄

---

## Cover 图

![cover_1](Termux 完整安装与配置指南（2026.4.24最新版，从零到可用）.assets/3aa1d62075c16504.png)
