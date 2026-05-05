# 🌐 告别离线！为Claude Code装上联网搜索的三种姿势（Tavily真香）

大家好，我是超级小白。最近一直在折腾Claude Code，这玩意儿写代码、读项目确实溜得飞起，但有个小遗憾——**默认没法联网搜索**。当你问它“2026年冬奥会最新赛况”或者“React 19新特性”时，如下：
```bash
web search
did 0 search
web search
did 0 search
...(ask u for search once more or give up)
```
 这就很憋屈了。

好在我们有MCP（模型上下文协议），可以像给电脑装插件一样，给Claude Code装上各种工具。今天我就把亲自踩坑、对比的三种**Web Search配置方案**全盘托出，尤其是最后一种，我已经深度沉迷了！
> MCP出来太久了，zhubo才刚玩上...

## 🤔 为什么需要自己配搜索？

Claude Code本身是个离线环境，它只认你项目里的代码和你喂给它的上下文。要让它能联网查资料，就得通过MCP服务器当“桥梁”。我试了三个方案：`synthetic-web-search-mcp`、`mcp-search-server`，还有最近大火的**Tavily**。

下面这张表是我花了一下午实测整理的，直接上干货：

| 对比维度 | **Tavily** | **synthetic-web-search-mcp** | **mcp-search-server** |
| :--- | :--- | :--- | :--- |
| **API 密钥** | 需要（注册很简单） | 需要（注册Synthetic） | **完全免费，无需密钥** |
| **免费额度** | **每月 1000 次搜索**（个人版） | 一次性 10 美元试用金 | 完全免费，无限制 |
| **搜索质量** | ⭐⭐⭐⭐⭐ 结构化数据，含摘要、相关内容 | ⭐⭐⭐⭐ 结果清晰 | ⭐⭐⭐ 常规DuckDuckGo结果 |
| **额外功能** | 支持内容提取、网站爬取、时间过滤 | 仅搜索 | 支持搜索+网页抓取 |
| **配置难度** | ⭐☆☆☆☆ 极简（有手就行） | ⭐☆☆☆☆ 极简 | ⭐☆☆☆☆ 极简 |

## 🥇 我的首选：Tavily（如果你已经注册，直接冲！）

如果你像我一样已经注册了Tavily（注册地址：app.tavily.com），那恭喜你，**Tavily是目前最适合Claude Code的搜索工具**。

### 为什么说它香？

1. **专为AI设计**：它返回的不是乱七八糟的网页源码，而是**结构化数据**——标题、链接、摘要，甚至还有“答案性内容”，Claude直接就能读懂，不用二次解析。
2. **功能拉满**：不仅能搜，还能**提取网页全文、爬取整个网站**，甚至支持按时间范围搜索（比如只搜近一周的新闻）。这些高级功能在其他两个方案里可没有。
3. **免费额度实在**：个人版每月1000次搜索，正常使用绝对够够的（除非你把它当爬虫用）。

### 🚀 手把手配置Tavily（三种姿势任选）

#### 姿势一：远程MCP服务器（最简单，推荐！）

打开终端，直接运行下面这条命令（记得把`<你的密钥>`换成你在Tavily后台复制的API Key）：

```bash
claude mcp add --transport http tavily https://mcp.tavily.com/mcp/?tavilyApiKey=<你的密钥>
```

如果你想让这台电脑上的所有项目都能用，加个 `--scope user` 就行。

**然后重启Claude Code**，输入 `/mcp`，看到 `tavily` 状态是 `connected`，就成了！

#### 姿势二：项目配置文件（更灵活）

如果你只想在某个项目里用，或者想自定义一些参数，可以编辑项目根目录下的 `.claude.json` 文件：

```json
{
  "mcpServers": {
    "tavily-search": {
      "command": "npx",
      "args": ["-y", "tavily-mcp@latest"],
      "env": {
        "TAVILY_API_KEY": "你的密钥放这里"
      }
    }
  }
}
```

保存，重启Claude Code，搞定！

#### 姿势三：官方Claude Code插件（适合深度玩家）

先全局配置API Key：

```bash
code ~/.claude/settings.json
```

添加 `"env": { "TAVILY_API_KEY": "你的密钥" }`。

然后**在Claude Code对话界面**里，依次输入：

```bash
/plugin marketplace add tavily-ai/skills
/plugin install tavily@skills
```

重启Claude Code，之后你就可以直接用 `/search` 快捷命令了，体验超棒。

### 🔍 怎么玩？高级玩法示例

配置好后，你就可以随意差遣Claude了：

- “帮我搜索最近一周关于AI芯片的新闻，并总结主要趋势。”（Tavily会自动过滤时间）
- “提取这篇文章的核心内容：[https://example.com/article]”（内容提取）
- “帮我爬取这个网站的前三页文章列表，生成一个表格。”（爬虫功能）

看看，是不是瞬间从“离线版”升级成了“全能版”？

## 💡 其他两个方案也值得提一嘴

- **mcp-search-server**：如果你不想注册任何服务，或者希望100%免费且无限制，它是最佳备胎。基于DuckDuckGo，无需密钥，配置简单。缺点是结果比较“原生”，Claude需要自己整理。
- **synthetic-web-search-mcp**：新用户送10美元试用金，相当于一次性体验卡。用完就得充值，适合轻度尝鲜。

## 🎉 写在最后

给Claude Code配上搜索，就像给跑车加了翅膀——它本来就是个强大的编码助手，现在又能联网获取最新信息，简直无敌了。我已经用这套组合拳查了不少技术资料、写周报时搜热点，效率翻倍。

如果你也动手配好了，记得回来评论区告诉我你的体验！如果遇到啥坑，也欢迎随时问我，大家一起填坑～

**现在，快去给你的Claude Code开个“天窗”吧！** 🌟

---

## Cover 图

![cover_1](告别离线！为Claude Code装上联网搜索的三种姿势（Tavily真香）.assets/c3d59c204c8d8be3.jpg)
