# 安装

最新版安装路径

加载不出多刷新几次/找国内镜像源/使用魔法

下载完成后找到执行文件双击进入安装流程，推荐一篇笔记，这位大哥写得很清楚：Git 详细安装教程（详解 Git 安装过程的每一个步骤）_git安装-CSDN博客

到安装成功那一步就可以回来了，开始基础学习。

# 设置用户名和邮箱

安装完成后，桌面右击显示更多选项：点击open git bash here，输入以下两句git命令完成对用户名和邮箱的设置：

git config --global user.name "[your_user_name]"
git config --global user.email "[your_email_address]"

完成后关闭窗口即可。 这个设置能有便于后续的管理。

#  测试基础功能

找一个自己喜欢的地方创建一个文件夹供前期git学习基本语句使用，笔者直接在D盘创建了一个gitTest文件夹。

进入到文件夹之后，鼠标右击显示更多选项：

<img alt="" height="324" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/c2/c250562dbe4b668bf9c0c94968eb7dfe18a47a4416cdc2d528666066814fb4c3.png" width="300" />

点击open git bash here

进入形如cmd命令行的git环境

初始化文件夹：git init   -- 文件夹中会自动生成一个.git文件夹，不需要改动

创建测试文件：鼠标右键手动创建一个文本文件用于测试，随手写上一行喜欢的内容，记得按保存！

 提交文件：①git add [你的文件名]（所有单词均可以tab补全）/git add .(表示当前文件夹下所有，为了偷懒，一个文件也这么写)②git commit

<img alt="" height="332" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/03/03daa2c1c2bb9ab94e25e42f2f6a5ee1b2fd0092598b57ad0a879b2f786d4e57.png" width="832" />

添加行为备注：当你执行完git commit后，如果你没有手贱在安装时改了默认编辑器，那么会跳入vim环境，英文提示你添加提交备注。①按i/a键进入编辑模式②添加备注后按esc键退出编辑模式③输入:wq(write quit)退出vim

再次对文件进行修改提交：编辑→保存→git add .→git commit -m "[做出的改动]"

git commit -m "[做出的改动]"省去了vim的繁琐操作，一般采用这种格式

查看日志信息： git log

查看详细日志信息（含文件改动）：git log --stat

<img alt="" height="875" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/63/633746053d227c6a7d9c21b5449c7c6527922e50fa629b2eb36a27a118eddd93.png" width="932" />

查看与历史版本的不同：git diff [commit_id]

<img alt="" height="327" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/6a/6a4ee9f86581b5750668b0f76d582916b60caf974efd9151c8384ed10d9008d8.png" width="767" />

重置到之前的版本：git reset --hard [commit_id]

<img alt="" height="91" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/c4/c4a69f1a025bfa3d23bd099107890f9ca5b67b7d4e59255e3740e43e7cca81f5.png" width="839" />

创建分支：

默认分支为master，一般放稳定版代码，所以尽量不对master进行直接修改，所以这里引入分支的概念，先在分支修改测试代码，敲定后再合并到master中

或者有时候协同开发时，每人做一个模块，也用到分支

查看当前分支 -- git branch 

创建新分支并进入 -- git checkout -b [新分支名称]

<img alt="" height="325" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/d7/d77c6b7d89643b11afa24b91fc9a3730143fcaeaed999626f5f4585c97f05a8b.png" width="710" />

接下来对文件进行的操作则是在新分支上执行的，并不会影响到主分支

分支上修改提交：①git add .②git commit -m "[remark]"③git log查看是否成功（忘记按保存就会发现本次提交无变动）

<img alt="" height="202" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ce/ce3b93d4718f022d1fd2600c1a9d81b5d59aa31244d1c9da4de8685c68bf5966.png" width="925" />

合并到主分支：①git checkout master②git merge [分支名]

<img alt="" height="278" src="https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/37/3779329cbb7bcba7a0cb66d0e4a1017cd92519a1033d7396da5f05cf984a340f.png" width="769" />

# 注意点（建议先看）

①vim不会操作可以直接忽略，提交改动时直接采用`git commit -m "[remark]"`即可，否则初学就遇到挫折会很难受。

②分支创建完全在git窗口执行，如果你发现你的文件夹内并没有产生分支文件夹，不要手动创建，不要手动创建，不要手动创建！！！分支只是一个概念，理解即可。

③代码改动与日志可以通过pycharm和vscode的可视化工具更直观地感受。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/03/03daa2c1c2bb9ab94e25e42f2f6a5ee1b2fd0092598b57ad0a879b2f786d4e57.png)
