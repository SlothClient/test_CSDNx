>hey 我是不做超级小白。前两天在推和小红都有刷到LongCat，是美团自研AI模型，基本都是嘲讽为主，不过也有搬排名截图的，貌似还不低，小白怀着“shit也要尝尝咸淡”的心理，bing了一下，第一条就是：
![bing首条](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/42/42666129e826fd3cedc81f0f657e026c2736efa08dae8eeef24a26973e91668c.png)
点进去界面如下，也挺像模像样的
![LongCat主页面](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/20/202e4d32c00bb6b901af83bde370ac29c899c5214f63e3f6494c08cf179325a3.png)

下面是正文，从获取api key到claude code跑项目全流程


# 获取api key
1. 点击主页面右上角API开放平台
2. 若没注册则先行注册
3. [api key管理界面](https://longcat.chat/platform/api_keys)
4. 点击右上角**创建API Key**按钮，取一个好记的名，我是按照使用场景取的cc
>**注意**：**最多可创建十个api key，最多三个api key是启用状态**

# 我可以调用啥模型？
平台用量界面明确显示以下五个模型：
- Sphynx（最新的agentic模型）
- LongCat-Flash-Lite（轻量版，相当于手机的青春版） **默认50w免费token**
- LongCat-Flash-Chat（通用模型） **三个模型共享50w免费token**
  >cc实测可读图
- LongCat-Flash-Thinking-2601（思考模型） **三个模型共享50w免费token**
- LongCat-Flash-Omni-2603（多模态模型） **三个模型共享50w免费token**
除了Sphynx模型需要内测名额，其余四个均有免费额度供使用

# 加大剂量<a id="dosage"></a>
进入[用量信息页面](https://longcat.chat/platform/usage)，拉到最底下
!![申请更多额度](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/5b/5bb41afcfb40b11bae86e4c233065a4b93d9041329f77cad0f0ff4b6ab7cdc02.png)

按情况填写申请，三小时内处理完申请，三小时后重新进页面开盲盒即可

# 打通claude code
## cc switch
使用cc switch工具（懒可私），选择添加
![cc switch添加供应商](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/51/51dff86a6dc9b204dc9ceecf702fb05a15d555dc13e8cfdeb39f9f515a2e7a75.png)

复制你创建的api key，填入，模型映射全填LongCat-Flash-Chat，如下
![配置供应商](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/76/7601ea7edc41a2c22ae4838640f655a4acf6dc29542b472811700d2c6ae1c8f8.png)

选择保存后选择cc switch自带的测试按钮看看配成功没
![测试模型](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/25/25f7494cd55c80b3787bf3e3507967581c9749cef5a8eff44b5aa1f4ee171723.png)

弹出成功后选择启用该供应商，随后可启动claude code，如下
![cc界面](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/c2/c2a3b99887e8a1357457e47147b1756ab51d20bffb12e9bc4ece40b8f6ec5179.png)

>强烈推荐使用cc switch管理，有附加多种功能，小白最常用的是聊天记录管理，不需要的随时删，不占c盘空间。下面贴上官配教程。
## 手改配置文件
创建或编辑 Claude Code 配置文件：.claude/settings.json

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_longcat_api_key",
    "ANTHROPIC_BASE_URL": "https://api.longcat.chat/anthropic",
    "ANTHROPIC_MODEL": "LongCat-Flash-Chat",
    "ANTHROPIC_SMALL_FAST_MODEL": "LongCat-Flash-Chat",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "LongCat-Flash-Chat",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "LongCat-Flash-Chat",
    "CLAUDE_CODE_MAX_OUTPUT_TOKENS": "6000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1
  },
  "permissions": {
    "allow": [],
    "deny": []
  }
}
```
随后可启动claude code，如下
![cc界面](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/79/790c0ceadf29e5273bdcbd46d482b9e7c7906f0bd89857733b9e0e5bb75cada3.png)


# more多more多
目前还属于公测阶段，类似之前xiaomi-mimo的免费两周活动，没有充值渠道，所以众生平等，大家都只能用免费额度，不过可以e-mail联系longcat-team@meituan.com并附带上一个自己在用的api key，有可能获得更多额度。

# 不白看
实测体验：
和小白之前用过的阿里云百炼coding plan(lite)表现差不多，优于火山coding plan(lite)；

响应速度比百炼慢，比火山快；

模型能力不错，记忆强，提过一嘴的能提炼进工作流程，比如提醒git提交一次，之后每次任务测试完都会做提交，之前用百炼glm5这种小事经常记不住，需要写入claude.md

通用模型可以读图，<kbd>Alt</kbd>+<kbd>V</kbd>粘图到cli

<a href="#dosage">加大剂量</a>之后，每天100M的免费token，随便玩玩够用

一日体验小白主要用来收尾了一个利用环境变量PATH在win电脑资源管理器地址栏输入命令快速打开软件的vibe coding项目，感兴趣的可以看看，https://github.com/SlothClient/quick-launcher

# 遇到问题了？
- 可以看看官方的[常见问题](https://longcat.chat/platform/docs/zh/FAQ.html)
- 评论区交流
- 私信交流

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/91/9178944105cf6436d05cba929da076e999b174f27ff26f859e65490ee2b27770.png)
