作为一个Windows系统Docker用户，你是否也遇到过这样的场景：打开终端准备跑个容器，敲下`docker ps`却发现Docker Desktop还没启动？然后不得不手动去开始菜单找图标、点开、等待启动……这个流程重复多了，真的很烦。

于是我写了一个小工具，让这一切变得完全自动化：**执行docker命令时，自动检测并启动Docker Desktop，等待就绪后继续执行你的命令**。整个过程你完全无感，就像Docker一直开着一样。

## 问题背景

Docker Desktop是Windows上最常用的Docker开发环境，但它不会随系统自启动（考虑到资源占用，这也是合理的）。问题在于，我经常忘记这一点：

```bash
# 某个阳光明媚的早晨
$ docker-compose up -d
error during connect: This error may indicate that the docker daemon is not running.
# 😑 又忘了启动Docker Desktop...
```

每次都要：

1. Win键搜索"Docker"
2. 点击启动
3. 等待托盘图标变绿
4. 重新执行命令

步骤不算多，但架不住频率高。作为一个程序员，这种重复劳动必须消灭。

## 解决思路

我想到的方案是：**用脚本包装docker命令，在执行前检测Docker状态，未运行则自动启动并等待就绪**。

这个方案有几个关键点：

1. **透明代理**：用户依然输入`docker xxx`，不需要改变习惯
2. **静默启动**：不弹出Docker Desktop窗口干扰工作
3. **智能等待**：启动后要等到Docker API响应，才算真正就绪
4. **多Shell支持**：我日常混用Git Bash、PowerShell、CMD，都要覆盖

## 核心实现

### 检测Docker状态

最简单的检测方式是调用`docker info`命令，如果返回成功说明Docker已就绪：

```bash
_docker_ready() {
    command docker info >/dev/null 2>&1
}
```

这里`command`是Bash的内置命令，用于调用真实的docker可执行文件，避免递归调用我们包装的函数。

### 启动并等待就绪

检测到未运行后，启动Docker Desktop并轮询等待：

```bash
_start_docker() {
    echo "Starting Docker Desktop..."
    cmd.exe /c start "" "\"$DOCKER_DESKTOP\"" 2>/dev/null

    local elapsed=0
    while ! _docker_ready; do
        if [ $elapsed -ge $TIMEOUT ]; then
            echo "Error: Docker failed to start within ${TIMEOUT}s" >&2
            return 1
        fi
        sleep 2
        elapsed=$((elapsed + 2))
        echo "Waiting for Docker... ($elapsed/$TIMEOUT)"
    done
    echo "Docker is ready!"
}
```

设计要点：

- **超时机制**：防止无限等待，默认60秒
- **友好提示**：显示等待进度，用户心里有数
- **静默启动**：用`start ""`避免弹窗

### 函数包装

最后用Shell函数覆盖`docker`命令：

```bash
docker() {
    local DOCKER_DESKTOP="/c/Program Files/Docker/Docker/Docker Desktop.exe"
    local TIMEOUT=60

    _docker_ready || _start_docker || return $?
    command docker "$@"
}
```

`source`这个脚本后，输入`docker ps`会先检测、可能启动、然后执行真正的命令。

### 多平台适配

不同Shell的实现略有差异：

**PowerShell**用函数覆盖：

```powershell
function docker {
    $dockerDesktop = "C:\Program Files\Docker\Docker\Docker Desktop.exe"
    $timeout = 60

    function Test-DockerReady {
        $output = docker.exe info 2>&1
        return $LASTEXITCODE -eq 0
    }

    if (-not (Test-DockerReady)) {
        # 启动逻辑...
    }

    & docker.exe $args
}
```

**CMD**用批处理脚本：

```batch
@echo off
docker.exe info >nul 2>&1
if %errorlevel%==0 goto :run

echo Starting Docker Desktop...
start "" %DOCKER_DESKTOP%

:wait_loop
docker.exe info >nul 2>&1
if %errorlevel%==0 goto :ready
:: 超时检测...
goto :wait_loop

:ready
echo Docker is ready!
:run
docker.exe %*
```

## 一键安装

为了让安装更简单，我写了一个PowerShell安装脚本：

```powershell
# 安装到所有shell
.\install.ps1

# 或只安装到特定shell
.\install.ps1 -Shell bash
.\install.ps1 -Shell powershell
.\install.ps1 -Shell cmd
```

脚本会自动：

- 在`.bashrc`添加`source`语句
- 在PowerShell `$PROFILE`添加导入
- 提供CMD注册表AutoRun配置选项

## 踩坑记录

开发过程中遇到几个有意思的坑：

### 1. Git Bash中静默启动GUI程序

直接运行`Docker Desktop.exe`会阻塞终端，必须用`start`命令。但Git Bash中`start`不可用，需要调用`cmd.exe /c start`：

```bash
cmd.exe /c start "" "\"$DOCKER_DESKTOP\"" 2>/dev/null
```

### 2. PowerShell中避免递归调用

在`Test-DockerReady`函数中调用`docker.exe info`，如果直接写`docker info`，会无限递归调用我们自己定义的`docker`函数。必须用`docker.exe`明确调用可执行文件。

### 3. CMD的AutoRun注册表

CMD没有类似`.bashrc`的配置文件，但支持通过注册表设置启动命令：

```
HKCU\Software\Microsoft\Command Processor
AutoRun = doskey docker="path\to\docker-wrapper.bat" $*
```

## 总结

这个小工具只有几百行代码，但极大改善了我的日常开发体验。现在我可以：

1. 打开终端就敲`docker`命令，不用管Docker Desktop是否启动
2. 脚本自动处理一切，等待、提示、执行
3. 完全不改变使用习惯

项目已开源，支持Git Bash、WSL、PowerShell、CMD四种终端环境。如果你也有同样的痛点，欢迎试试。

如果你日常开发中也有类似的"小麻烦"，不妨想想能不能用脚本自动化解决。有时候，一个简单的工具就能带来意想不到的幸福感。

---

## Cover 图

![cover_1](执行docker命令时自动启动Docker Desktop？一个小工具让开发体验更丝滑.assets/b142aeb9af4cdac1.png)
