@[TOC]
> 这是一个发生在真实仓库里的故事，主角是两个文件，它们名字一样，只是大小写不同。在Linux上它们相安无事，到了Windows上，战争开始了。

## 故事的开始

某天，我在Windows电脑上执行了`git pull`：

```
$ git pull
Fast-forward
 README.md | 1 +
 readme.md | 1 +
 2 files changed, 2 insertions(+)
 create mode 100644 README.md
 create mode 100644 readme.md
```

看起来很正常，拉取了两个文件。然后我看了看当前目录：

```
$ ls
readme.md
...
```

等等，不是拉了两个文件吗？怎么只有一个？

## 灵异事件调查

我决定深入调查：

```
$ git show HEAD:README.md
uppercase: just for test

$ git show HEAD:readme.md
lowercase: just for test
```

两个文件，内容不同，都存在于Git仓库中。

再看一眼Git状态：

```
$ git status
Changes not staged for commit:
    modified:   README.md
```

**破案了！** Git认为`README.md`被修改了，但我的工作目录里只有`readme.md`。

## 真相只有一个

事情的真相是这样的：

| 角色            | 行为                                          |
| --------------- | --------------------------------------------- |
| Linux文件系统   | `README.md` 和 `readme.md` 是两个不同的文件 ✓ |
| Windows文件系统 | `README.md` 和 `readme.md` 是同一个文件 ✗     |
| Git对象库       | 存了两个blob，内容都保存着 ✓                  |
| Git索引         | 记录了两个条目 ✓                              |
| Git checkout    | 按顺序写文件，后写的覆盖了前写的 ¯\\_(ツ)_/¯  |

是的，Git**没有报错**，它就这么默默地把两个文件写到了同一个路径上，后写的覆盖了前写的。

画个图：

```
.git/objects/          工作目录 (Windows)
┌─────────────────┐    ┌─────────────────┐
│ blob: "upper..."│    │                 │
│ blob: "lower..."│    │ readme.md       │ ← 最后写的
└─────────────────┘    └─────────────────┘
     ↑                        ↑
   Git都知道               Windows只认一个
```

## `core.ignorecase=true` 是什么？

你可能会问：不是说Git有`core.ignorecase=true`这个配置吗？

是的，这个配置告诉Git：**在比较文件名时忽略大小写**。

但问题是，这个配置**不会阻止你创建两个只有大小写不同的文件**。它只是让Git在检测文件变更时，把`README.md`和`readme.md`当成同一个文件来处理。

所以这个配置反而让情况更混乱：

- 远程仓库有两个文件（Linux用户创建的）
- Windows用户拉取时，Git不报错
- 工作目录只剩一个文件
- Git状态显示"modified"

## 如何避免这个坑

### 1. 检查是否有大小写冲突

```bash
git ls-files | sort -f | uniq -di
```

这个命令会列出所有只有大小写不同的文件名对。

### 2. 重命名冲突的文件

```bash
# 把readme.md重命名为README-lowercase.md
git mv readme.md README-lowercase.md
git checkout -- README.md
git add README-lowercase.md
git commit -m "Rename to avoid case conflict on Windows"
```

### 3. 团队约定

最佳实践：**永远不要创建只有大小写不同的文件名**。

这不是技术问题，是团队协作问题。在跨平台项目中，这就是一个定时炸弹。

## 总结

| 平台    | 大小写敏感 | 建议                        |
| ------- | ---------- | --------------------------- |
| Windows | 否         | 避免创建冲突文件名          |
| Linux   | 是         | 也别这么干，照顾Windows队友 |
| macOS   | 默认否     | 同Windows                   |

记住：**Git不会保护你免受文件系统的限制**。它只是忠实地记录你让它记录的一切，然后忠实地把一切写到文件系统允许它写的地方。

下次看到`git status`莫名其妙显示文件被修改，想想是不是大小写的问题。

---

*本文由一个真实的大小写冲突事件改编，该事件就发生在写文档的这个仓库里。*

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/15/155f0ba36997b9190d9169c3df10704b55b434f0ef21757bacbcf001b1947d1f.png)
