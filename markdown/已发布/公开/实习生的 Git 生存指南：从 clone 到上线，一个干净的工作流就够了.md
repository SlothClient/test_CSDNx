# 实习生的 Git 生存指南：从 clone 到上线，一个干净的工作流就够了

> 接手项目不慌，提交通畅不踩坑

对于刚进入开发实习的同学来说，最怕的不是代码难写，而是**不知道 Git 该怎么用**。尤其是面对一个已经跑了好几年的成熟项目，分支多、提交多、同事多，一个不小心就可能把仓库搞得乱七八糟。

本文根据一位过来人的真实踩坑经验，总结了一套从 `git clone` 到最终上线的**干净工作流**。照着做，至少不会因为 Git 操作被组长点名批评。

---

## 第一步：接手项目，先摸清“家底”

当你拿到项目仓库地址，第一件事不是急着写代码，而是先把项目“看一遍”。

### 1. 克隆项目

```bash
git clone <项目地址>
```

如果项目历史很庞大，可以考虑浅克隆加速：

```bash
git clone --depth 1 <项目地址>
```

### 2. 查看所有分支，了解命名习惯

```bash
git branch -a
```

- `-a` 会显示本地分支 + 远程分支（`remotes/origin/xxx`）。
- 仔细观察远程分支的命名：是 `feature/xxx`、`fix/xxx`，还是 `release/xxx`？了解团队的分支命名规范，以后自己建分支才不会出错。

### 3. 熟悉代码与业务流程

光看分支名不够，还要把项目跑起来，看看主要的业务代码在哪里。**不懂就问**，实习生的特权就是可以光明正大地问问题。不要闷头看三天，问一嘴可能十分钟就清楚了。

---

## 第二步：准备开发，先同步再拉分支

当你被分配了一个任务（比如加一个新功能或修一个 bug），不要上来就写代码。先确保本地代码是最新的。

```bash
git status                # 看看当前有没有未提交的改动
git pull --rebase         # 拉取最新代码，用 rebase 保持提交历史干净
git log --oneline -n 5    # 看看最近 5 次提交，了解最近项目发生了什么变化
```

然后基于主分支（通常是 `main` 或 `develop`）创建自己的开发分支：

```bash
git checkout -b feature/your-task-name
```

分支名要符合团队规范，比如 `feature/add-login-cache` 或 `fix/header-style`。

---

## 第三步：开发中的小插曲 – 用 stash 优雅暂停

开发到一半，突然被告知要紧急切到另一个分支修 bug。但当前代码还没写完，不想提交一个半成品，怎么办？
>没有强迫症和代码洁癖的也可以先做一个temp commit，事后再restore

**用 `git stash` 暂存当前改动。**

```bash
git stash save "正在开发 xxx，未完成"
```

然后放心切分支、处理紧急任务。处理完后切回原来的分支：

```bash
git stash list            # 查看 stash 列表
git stash apply           # 恢复最近一次 stash（不删除记录）
git stash pop             # 恢复并删除记录
```

**提醒：** 用完的 stash 要及时清理，不要让 stash list 堆出一大串没用的东西。

```bash
git stash drop stash@{0}  # 删除指定 stash
git stash clear           # 慎用，会清空所有 stash
```

---

## 第四步：日常开发 – 小步提交，既是规范也是“功劳簿”

有的实习生喜欢攒一天的代码，最后来一个 `git commit -m "update"`。这是大忌。

**推荐做法：按逻辑功能，小单位提交。**

每完成一个小的功能点或修复，就提交一次。好处：

1. **更规范** – 同事 code review 时能清楚看到每一步改动。
2. **更安全** – 出错了容易回滚到上一个稳定点。
3. **看着干活多** – 别笑，这真的有用。一天 5~6 个有意义的提交，比你憋一个大提交更显工作量。

### 单次提交流程（标准版）

```bash
git status                # 1. 确认改动了哪些文件，有没有不该提交的
git diff                  # 2. 仔细看改动内容，避免把调试代码或密码提交上去
git add .                 # 3. 添加所有改动（也可以按文件添加）
git commit -m "feat: 增加用户登录验证"   # 4. 写好规范的提交信息
git pull --rebase         # 5. 拉取最新代码，避免产生合并提交（重要！）
git push                  # 6. 推送到远程
```

其中 **`git diff` 和 `push 前的 pull --rebase` 最重要**：

- `git diff` 让你在提交前再审视一遍代码，往往能发现“我什么时候加了个 `console.log`？”或者“这行调试代码忘删了”。
- `git pull --rebase` 避免产生无意义的 merge 节点，保持提交历史是一条直线。如果出现冲突，解决后 `git rebase --continue` 即可。

---

## 第五步：开发完毕，合并回主分支

功能开发完成、测试通过后，需要把代码合并到主分支。

### 先切回主分支，同步最新代码

```bash
git checkout main                 # 或 develop
git status
git pull --rebase
git log --oneline -n 5            # 看看别人有没有新提交
```

### 合并你的开发分支

```bash
git merge feature/your-task-name
```

**如果有冲突：**

1. Git 会提示哪些文件冲突。
2. 手动打开文件，解决 `<<<<<<<`、`=======`、`>>>>>>>` 标记。
3. 解决后 `git add .`，然后 `git commit`（如果使用 rebase 则是 `git rebase --continue`）。
4. 最后 `git push`。

### 通知组长

合并并推送后，不要默默结束。**主动在群里或私聊提醒组长 review 代码**，确认没问题后安排上线。这是责任心，也是刷存在感的好机会。

---

## 总结：一张图记住完整流程

```text
git clone
git branch -a
↓
git checkout -b feature/xxx
↓
开发 → 小步提交（status → diff → add → commit → pull --rebase → push）
↓
遇到中断 → git stash → 处理 → git stash pop
↓
开发完成 → 切回主分支 → pull --rebase → merge → push
↓
提醒组长 review → 上线
```

---

## 最后给实习生的几点建议

- **不懂就问**，比瞎操作后让别人帮你修仓库强一万倍。
- **多用 `git status`**，随时知道自己在哪里、有什么改动。
- **少用 `git push --force`**，除非你完全清楚后果。
- **提交信息写好前缀**：`feat:` / `fix:` / `docs:` / `refactor:`，方便后续生成 changelog。
- **每天上班第一件事：`git pull --rebase`**，保持本地代码最新。

这套工作流程不一定是最“极客”的，但一定是最稳的。跟着做，至少你不会因为 Git 而被组长在代码评审会上点名。

祝你实习顺利，代码无冲突，上线不加班！

---

## Cover 图

![cover_1](实习生的 Git 生存指南：从 clone 到上线，一个干净的工作流就够了.assets/035844a6cab51bd2.png)
