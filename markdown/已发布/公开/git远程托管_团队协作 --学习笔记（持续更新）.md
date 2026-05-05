# git远程托管/团队协作

> 基础git操作见往期文章
> [git的安装和基本使用--学习笔记](https://mp.csdn.net/mp_blog/creation/editor/141297189)

## vscode开发

 1. 创建开发文件夹
 2. 使用vscode打开文件夹
 3. git管理初始化`git init`，文件夹自动创建.git文件夹
 

	> 注意一定要在当前文件夹的终端环境中，vscode中右击当前文件夹选择打开终端则自动进入当前文件夹终端
	![在这里插入图片描述](git远程托管_团队协作 --学习笔记（持续更新）.assets/c065cf6b99e06870.png)

 4. 创建说明文档readMe.md，简单描述工作内容后提交内容
	`git add .` `git commit -m "[remark]"`
	

	> 也可以使用vscode工具快速提交，点击提交后在中央搜索框中**填入修改说明**，**点击输入框右边√号**完成提交
	![在这里插入图片描述](git远程托管_团队协作 --学习笔记（持续更新）.assets/ec87aa875b3a021e.png)

 5. 随后进入我们的github，创建一个新的仓库，配置都选默认即可
 

	> 这里没有github账号的需要创建账号；进不去的多刷新几次；使用魔法

 6. 仓库创建完成的页面不要关闭，里面都是我们能直接copy的代码
 ![在这里插入图片描述](git远程托管_团队协作 --学习笔记（持续更新）.assets/efdddae670277249.png)

 7. 复制你的仓库链接，先执行这三行
 ![在这里插入图片描述](git远程托管_团队协作 --学习笔记（持续更新）.assets/0bdab3164f243e96.png)

 ```bash
git remote add origin [link_to_your_repository]
git branch -M main
git push -u origin main
```

 8. 进入浏览器完成身份绑定
 

	> 进不去多刷新/使用魔法
	

 9. 绑定后刷新之前的仓库说明页进入自己的仓库

##  协作开发

> fork团队成员的仓库，创建自己的分支，独立开发，push，merge...

### 设置主仓，成员fork

> *waiting for updating*
> 笔者暂时忙，可评论催更

### 公用仓库，邀请collaborators

> *waiting for updating*
> 笔者暂时忙，可评论催更

### 常用命令
```bash
git clone
git remote add
git remote -v
git fetch
git push( -u origin [branch])
git pull( -u origin [branch])
git branch -r
git checkout [local/remote branch]

---

## Cover 图

![cover_1](git远程托管_团队协作 --学习笔记（持续更新）.assets/efdddae670277249.png)
