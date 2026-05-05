# WSL中`git status`显示大量修改，而Win却clean？status中文乱码？
>hey 这里是不做超级小白 喜欢我的内容的话请多多支持我~

>前情概要：小白同时在win和wsl操作Obsidian_Notes的仓库，两边bash中`git log`一致但是`git status`大不相同，如下：
>![powershell](WSL中git status显示大量修改，而Win却clean？status中文乱码？.assets/6298431d8c2c02fe.png)
![wsl](WSL中git status显示大量修改，而Win却clean？status中文乱码？.assets/4ffc36bbe1f4a445.png)
>所以这篇文章诞生了。

## 一、快速解决方案

在cmd/powershell和wsl中执行下面三条命令即可：

```bash id="v6t7sj"
git config --global core.quotepath false
git config --global i18n.logoutputencoding utf-8
git config --global core.autocrlf input
```

> * `core.quotepath false`：关闭 Git 对非 ASCII 文件名的转义显示，让中文文件名、特殊字符直接可读；
> * `i18n.logoutputencoding utf-8`：保证 Git 输出日志（如提交信息）在终端显示正确 UTF-8 编码，不会乱码；
> * `core.autocrlf input`：在提交时自动将 CRLF 转为 LF，检出文件时保持原样，避免 WSL 显示大量 `modified`。

执行完后，在 WSL 里运行：

```bash id="2y8glq"
git status
```

> 此时应看到 WSL 和 Windows 的工作区状态一致，中文文件名也显示正常。

---

## 二、问题原因解析

### 1. 为什么 Windows clean，而 WSL 显示大量 `modified`

WSL 和 Windows Git 使用的是不同的环境：

* **Windows Git**：默认 CRLF 换行符，且 Windows 文件系统不区分权限位；
* **WSL Git**：使用 LF 换行符，且 Linux 权限位敏感。

所以同一份仓库：

```bash id="9h3pft"
git log
```

> 查看提交历史可能完全一致，但：

```bash id="t1j8ru"
git status
```

> 会因为换行符差异或权限位差异显示不同。

### 2. 中文文件名显示为 `\345\xxx`

默认 Git 对非 ASCII 文件名会转义显示，PowerShell 常出现 `\xxx` 形式，WSL bash 一般显示正常。

执行：

```bash id="3g8s5v"
git config --global core.quotepath false
```

> 关闭路径转义后，中文、日文、emoji 文件名可以正确显示。

### 3. Git 输出日志乱码

如果提交信息含中文，Git 在 Windows 终端或 WSL 可能显示乱码。

执行：

```bash id="p2v0lo"
git config --global i18n.logoutputencoding utf-8
```

> 让 Git 输出日志使用 UTF-8 编码，终端显示正确。

### 4. CRLF / LF 换行符差异

Windows Git 默认可能自动将 LF 转为 CRLF，这会导致 WSL 显示大量 `modified`：

```bash id="n9x6vq"
git diff --ignore-space-at-eol --stat
```

> 查看忽略行尾空格和换行符差异后的实际修改情况。
```bash id="aircyan"
git config --global core.autocrlf input
```
> `core.autocrlf input` 配置可以在提交时统一换行符，避免 Windows/WSL 差异。

### 5. 权限位差异

WSL/Linux 会检测文件权限位，Windows 文件系统不支持执行位，所以你可能会在linux中看到：

```diff id="xk2mp8"
old mode 100644
new mode 100755
```

```bash id="y7drq3"
git config core.filemode false
```

> 忽略文件权限位变化，防止显示大量无实际内容修改。

---

## 三、操作顺序建议

1. **确认仓库路径和提交**：

```bash id="q3g8hf"
git rev-parse --show-toplevel
git rev-parse HEAD
```

> 确保 Windows 和 WSL 访问的是同一份仓库。

2. **修复中文显示和日志编码**：

```bash id="8d4nkj"
git config --global core.quotepath false
git config --global i18n.logoutputencoding utf-8
```

3. **修复换行符差异**：

```bash id="2m5lxh"
git config --global core.autocrlf input
```

4. **刷新工作区状态**：

```bash id="s9j7fh"
git status --short
```

> 查看 WSL 和 Windows 是否一致。

---

## 四、总结

* `git log` 一样 ≠ `git status` 一样；
* 主要影响因素：**中文文件名编码、日志编码、换行符、文件权限、不同 Git 配置**；
* 最安全、快速的做法就是上面的三条命令，既能保证显示正常，又不会影响已有提交内容；
* 对于团队协作或跨平台仓库，建议使用 `.gitattributes` 统一换行符规则。

> 总结一句话：先修复显示和换行符差异，再排查权限或忽略规则，确保 Windows 和 WSL 状态一致，不要盲目提交或 reset。

---

## Cover 图

![cover_1](WSL中git status显示大量修改，而Win却clean？status中文乱码？.assets/163519a2fb0afeba.png)
