
# 1.git init 初始化git项目

实操如下：

![请添加图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/74/74080cdd45a610cd7f630563bccfaa33a5913645c165f7dcb1ffad14aecf3bc0.png)


接着我创建了一个test.txt文件，下面进行提交

# 2.git add . 暂存文件

> `git add` 命令用于将文件的改动添加到**暂存区**（staging area），为下一次提交做好准备。简单来说，它标记了哪些文件或改动会被纳入下次 `git commit` 中。以下是 `git add` 的作用和使用场景：
>
> ### 1. **作用**
>
>    - `git add` 将指定文件或文件夹的修改从工作区（working directory）放入暂存区，但不会立即提交到 Git 仓库。
>    - 此过程让用户在提交之前选择性地添加文件，确保提交时只包含需要的改动。
>
> ### 2. **基本用法**
>
>    - **添加单个文件**：
>
>      ```bash
>      git add filename
>      ```
>
>      这样会将 `filename` 文件添加到暂存区。
>
>    - **添加所有改动文件**：
>
>      ```bash
>      git add .
>      ```
>
>      或者
>
>      ```bash
>      git add --all
>      ```
>
>      这会将所有已修改、删除和新建的文件添加到暂存区。
>
>    - **添加特定目录**：
>
>      ```bash
>      git add directory_name/
>      ```
>
>      这样会将指定目录及其子文件中的改动全部添加到暂存区。
>
> ### 3. **典型应用场景**
>
>    - **选择性提交**：在一次代码修改中可能涉及多个文件，使用 `git add` 可以选择性地添加需要提交的文件，以便分次提交，保持提交记录的清晰。
>    - **确认改动**：在添加文件到暂存区后，可以使用 `git status` 查看哪些文件已被暂存，以确认准备好的改动是否符合预期。
>
> `git add` 是 Git 工作流的一个关键步骤，因为只有在文件被添加到暂存区后，`git commit` 才会将其纳入提交。

实操如下：

![请添加图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/73/737d4c56de7afaeaadec6a74ba884df2c36e9f6d6b5467239f1ff34d22b74710.png)


> 这里是暂存失败了，原因是dubious ownership（类似于一般代码中报错的ambiguous declaration），也就是仓库拥有者不明确，因为我这是U盘下的项目
>
> 这个错误是由于 Git 检测到在当前目录下的文件系统不记录文件的“拥有者”信息，Git 出于安全原因将其视为“可疑”。为了解决这个问题，可以将这个目录标记为“安全”目录，让 Git 识别它的信任状态。
>
> 执行以下命令，添加信任配置：
>
> ```bash
> git config --global --add safe.directory 'E:/中转站/git项目开发使用基础教程'
> ```
>
> ### 解释
>
> - **--global**：表示将该目录的信任设置为全局有效（适用于所有 Git 项目），可以在任何地方信任这个目录。
> - **safe.directory**：指定安全目录。Git 在检测到“可疑”目录时会检查这个配置，以决定是否允许在该目录下进行操作。
>
> 此设置仅在当前环境中生效，不会影响其他 Git 仓库的安全性。

![请添加图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/cd/cdb8151a04458d001aaa64bbfe8b3003ba90d562748d66bf7fef9bab88e4a03e.png)


# 3.git commit -m "[comment]" 提交更改并添加提交留言

实操如下：

![请添加图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/5e/5e8ca05fe4144298039a5979b0350867443570500788a5f30466844389fd9207.png)


# 4.git log & git log --stat 查看提交日志&详细提交日志

实操如下：

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

> 上面的那个框起来的add file不是因为我执行了添加文件的操作，而是因为我提交的时候的留言是add file
>
> 但是我为什么偏偏写这个留言呢，写其他的不行吗？
>
> 这就涉及到了提交留言的规范，一般项目开发中都存在一定的提交规范，下面截取GitHub中的开源项目查看：
>
> ![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)
>
> ### **规范的提交类型**
>
> 使用类型化的提交前缀，让提交历史更易于理解和管理。常见的前缀包括：
>
>    - **feat**：新功能（feature）
>    - **fix**：修复 bug
>    - **docs**：仅文档更改
>    - **style**：代码格式调整（不涉及功能变化）
>    - **refactor**：代码重构（既不新增功能也不修复 bug）
>    - **test**：添加或修改测试
>    - **chore**：构建过程或辅助工具的变动
>
>    示例：
>
>    ```bash
> git commit -m "feat: add search functionality to user profile page"
>    ```

# 5.git diff [commit_id] 查看与[commit_id]提交时的不同

我先在test.txt中写一些内容：

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

暂存并提交，`git log`复制上一次提交的id并执行git diff：

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

# 6.git reset --hard [commit_id] 重置为commit_id时的状态

实操如下：

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

查看文件，已经为空

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

> 文字内容没清空是因为我没保存，但是插入的图片是自动保存的

# 7.git checkout -b [new_branch]创建新分支并进入

创建新文件→提交→切换回master分支查看区别

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)

# 8.git merge [another_branch] 合并其他分支

master合并develop之后：

![外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ac/acfdc98e46d5acc3bba7e6629db5a6371401faa615be88f5a349dc749eee6215.png)
