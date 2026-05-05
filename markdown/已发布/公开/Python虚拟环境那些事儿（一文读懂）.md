### 虚拟环境详解

在 Python 中，使用虚拟环境（virtual environment）是一种管理项目依赖和隔离项目环境的最佳实践。`venv` 和 `virtualenv` 都是用于创建虚拟环境的工具，虽然它们有些相似，但 `venv` 是 Python 3.3 版本开始自带的标准库，而 `virtualenv` 是一个第三方工具，适用于 Python 2 和 3。

下面是关于 `venv` 和 `virtualenv` 的详细介绍，包括创建、激活、退出等操作：

#### 1. 使用 `venv` 创建虚拟环境

`venv` 是 Python 3.3 及以后的版本自带的模块，用于创建虚拟环境。它是官方推荐的创建虚拟环境的方式。

##### 1.1 创建虚拟环境

首先，确保你使用的 Python 版本 >= 3.3。

1. 打开命令行/终端。
2. 切换到你的项目目录，或者你希望存放虚拟环境的目录。
3. 使用以下命令创建虚拟环境：

```bash
python -m venv myenv
```

其中，`myenv` 是你想要创建的虚拟环境的目录名（你可以自定义名称）。该命令会创建一个新的目录 `myenv`，并在其中包含 Python 解释器和一个独立的包管理环境。

##### 1.2 激活虚拟环境

- **在 Windows 上**：

```bash
myenv\Scripts\activate
```

- **在 macOS 和 Linux 上**：

```bash
source myenv/bin/activate
```

激活后，你会看到命令行提示符前面加上了虚拟环境的名字（如 `(myenv)`），表示当前的环境已经切换到虚拟环境。

##### 1.3.1 安装依赖包

激活虚拟环境后，你可以使用 `pip` 来安装依赖包，而这些依赖包只会安装在当前虚拟环境中，而不会影响系统的全局 Python 环境。

```bash
pip install <package_name>
```

例如，安装 `requests` 库：

```bash
pip install requests
```
##### 1.3.2 使用虚拟环境中的解释器运行

 - 直接命令行中使用`python`命令运行脚本
 - vscode中<kbd>Ctrl + Shift + P</kbd>选择到目标虚拟解释器
 - pycharm中打开项目文件夹自动选中，或setting中设置

##### 1.4 退出虚拟环境

完成工作后，你可以通过以下命令退出虚拟环境：

```bash
deactivate
```

退出后，你的命令行会返回到系统的 Python 环境。

#### 2. 使用 `virtualenv` 创建虚拟环境

`virtualenv` 是一个第三方工具，适用于 Python 2 和 Python 3。它提供了一些额外的功能，比如创建独立的 Python 环境时支持不同的 Python 版本。

##### 2.1 安装 `virtualenv`

在使用 `virtualenv` 之前，你需要先安装它。你可以通过 `pip` 来安装：

```bash
pip install virtualenv
```

##### 2.2 创建虚拟环境

使用 `virtualenv` 创建虚拟环境与 `venv` 类似。首先进入到你希望存放虚拟环境的目录，然后运行：

```bash
virtualenv myenv
```

与 `venv` 类似，这将会在 `myenv` 目录中创建一个新的虚拟环境。你还可以指定 Python 版本（如果你有多个 Python 版本）：

```bash
virtualenv -p /usr/bin/python3.8 myenv
```

这将创建一个使用 Python 3.8 的虚拟环境。

##### 2.3 激活虚拟环境

- **在 Windows 上**：

```bash
myenv\Scripts\activate
```

- **在 macOS 和 Linux 上**：

```bash
source myenv/bin/activate
```

##### 2.4 安装依赖包

激活虚拟环境后，你可以使用 `pip` 安装依赖包。例如，安装 `requests` 库：

```bash
pip install requests
```

##### 2.5 退出虚拟环境

与 `venv` 一样，你可以通过以下命令退出虚拟环境：

```bash
deactivate
```

#### 3. 虚拟环境的目录结构

不论是使用 `venv` 还是 `virtualenv` 创建虚拟环境，虚拟环境的目录结构大体是相同的。

- **Windows**：

```
myenv/
    Scripts/
        activate
        pip.exe
        python.exe
    Lib/
        site-packages/
```

- **macOS/Linux**：

```
myenv/
    bin/
        activate
        pip
        python
    lib/
        python3.x/
            site-packages/
```

#### 4. 使用虚拟环境的优点

1. **隔离环境**：每个虚拟环境都有自己独立的 Python 解释器和安装的包，不会与其他项目冲突。
2. **简化依赖管理**：每个项目可以有不同的依赖版本，避免版本冲突。
3. **可复制性**：通过 `requirements.txt` 文件，团队成员可以确保每个人的开发环境一致。

#### 5. 使用 `requirements.txt` 管理依赖

为了确保你的项目在不同机器上可以复现，你可以将项目的依赖包写入一个 `requirements.txt` 文件。在虚拟环境中，你可以使用 `pip` 导出当前环境的依赖：

```bash
pip freeze > requirements.txt
```

当其他人获取到你的项目后，他们只需要运行以下命令来安装所有依赖：

```bash
pip install -r requirements.txt
```

#### 6. 删除虚拟环境

如果你不再需要某个虚拟环境，可以直接删除该虚拟环境的目录，例如：

```bash
rm -rf myenv
```

#### 总结

- **`venv`** 是 Python 3.3+ 自带的创建虚拟环境的工具，推荐用于 Python 3 项目。
- **`virtualenv`** 是一个第三方工具，支持 Python 2 和 Python 3，提供一些额外功能。
- 创建虚拟环境后，你可以使用 `pip` 安装和管理项目依赖，确保每个项目的依赖都被隔离。
- 通过 `deactivate` 可以退出虚拟环境，回到系统的 Python 环境。

虚拟环境是 Python 开发中的最佳实践，能够大大简化包管理和版本控制，避免不同项目之间的依赖冲突。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/5d/5dd286523d82954e10c49c5fbb6f6ccc4f6f8a54e08442c219f7a10721435f72.jpeg)
