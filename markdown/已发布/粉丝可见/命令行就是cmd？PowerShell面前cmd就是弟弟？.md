# 一、先搞懂“命令行”到底指什么
**日常语境中的“命令行”：**

 - 广义指通过输入文本指令操作计算机的工具（如Windows的`cmd/PowerShell`、Linux/macOS的`Terminal`）。
 - 狭义常特指Windows的`cmd.exe`（尤其对习惯早期系统的用户）。 容易混淆的场景：

当教程说“用命令行执行”却未明确工具时，**可能导致**命令在cmd有效但在PowerShell报错（反之亦然）。
例如 ping 、 ipconfig 等基础命令两者通用，但涉及脚本或高级功能时差异显著。
 
# 二、cmd vs PowerShell：应用层高频问题
## 1. 命令兼容性：看似一样，实则暗坑
表面兼容：PowerShell允许直接运行多数cmd命令（如 dir 、 copy ），但实际是模拟执行，并非原生支持。
典型差异：
 
### cmd中可用，PowerShell会报错
```bash
taskkill /im notepad.exe /f  # PowerShell需写为：Stop-Process -Name notepad -Force
```
 

 
### 路径分隔符：cmd用`\`，PowerShell同时支持`\`和`/`
```bash
cd C:/Users  # PowerShell有效，cmd无效
```
 
## 2. 权限与执行策略
`cmd`：默认以当前用户权限运行，无脚本执行限制（`.bat`双击直接执行）。
`PowerShell`：
默认禁止运行.ps1脚本（防止恶意代码），需手动设置：
 
```bash
Set-ExecutionPolicy RemoteSigned  # 允许本地脚本运行
```
 
管理员权限：两者均需右键选择“以管理员身份运行”，但`PowerShell`可通过命令快速提权：
 
```bash
Start-Process powershell -Verb RunAs  # 直接唤起管理员窗口
```
 
## 3. 数据处理：文本 vs 对象
`cmd`管道传递的是纯文本，需用 findstr 等工具二次处理：
 
```bash
dir | findstr ".txt"  # 筛选txt文件
```
 
`PowerShell`管道传递的是对象，可直接操作属性：
 
```bash
Get-ChildItem | Where-Object { $_.Extension -eq ".txt" }  # 按扩展名过滤
Get-Process | Sort-Object CPU -Descending  # 按CPU排序进程
```
 
*注：PowerShell的 Select-Object 、 Format-Table 等命令可精细化控制输出。*
  
 
# 三、快速打开命令行的5种方法
**全局快捷键：**
 `Win + R`  → 输入 `cmd` 或 `powershell`  → 回车
隐藏技巧：在资源管理器地址栏直接输入 `cmd` 或 `powershell` ，回车即在该路径下启动。
**右键菜单增强：**
按住 `Shift` 右键文件夹空白处 → 选择“在此处打开 `PowerShell` 窗口”（Win10+默认支持）。
**搜索直达：**
**按 `Win + S`  → 输入“`cmd`”或“`PowerShell`” → 选择结果后 `Ctrl + Shift + Enter` 以管理员身份运行。**
**PowerShell内部切换：**
在`PowerShell`中输入 `cmd` 可直接进入`cmd`环境，输入 `exit` 返回`PowerShell`。
**任务管理器：**
 `Ctrl + Shift + Esc`  → 文件 → 运行新任务 → 输入工具名称。
 
**关键总结**

 - 能用PowerShell尽量用：功能更强大，尤其涉及系统管理或复杂数据处理时。
 - 临时需求用cmd：执行简单命令或兼容旧脚本时更轻量。

***如有错误，望不令赐教，谢谢。***

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/66/660a0d26bb4874abc4d10b5c17de222c4681447a3fbeacecb345e9d07d1beba5.jpeg)
