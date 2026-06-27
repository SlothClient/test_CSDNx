

>hey 这里是不做超级小白 喜欢我的内容的话请多多支持我~
# uni-app小程序体验版报错无法连接uniCloud本地调试，咋回事？

最近在vibe coding一个uni-app微信小程序，项目里用了uniCloud。开发阶段一切正常，本地运行、云函数调用、数据库操作都没问题。但上传微信体验版之后，uniCloud就开始异常：微信开发者工具预览和真机调试能用，体验版不能用。

现在这个时代，遇到问题问ai
小白先用`claude code + mimo`排查，它说是`manifest.json`没填`spaceId`导致的，说得有理有据
尽管如此，作为一个科班程序员，对于这种`xx-id`、`xx-secret`小白还是比较警惕
所以小白又去问了deepseek，uniapp小程序开发把`spaceid`复制写到`manifest.json`是否是正确做法（小程序太久没碰确实网络orz...），deepseek回答如下：
![ds的标准错误回复](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/e3/e37d6aae5a8981c6b83f3f67fcdd9440f463657401c235e38db29d987693334d.png)
尽管如此，作为一个科班程序员，对于这种`xx-id`、`xx-secret`小白还是非常非常警惕
是的，小白又去问了chatgpt，毕竟充了钱的，不用荒废了，chatgpt明确告诉小白不是`manifest.json`没填`spaceId`的问题，这是它给的回复：
![gpt的标准正确回复](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/1f/1f05ef9da24f1763982b0b38541dcfa256d2f15e12bdd6647936157aab8e2dd2.png)
<u>小白看到这句话的瞬间恍然大悟、醍醐灌顶，</u>
<u>为啥？</u>
<u>因为小白上班的时候犯过这个错！还好只是体验版，提审上线就是T0级别事故！！</u>
<u>这次vibe coding除了`git`小白基本都没管，却又一次翻了这个错误！！！</u>
<u>切换build包重新发布，手机重进小程序，一切正常！</u>

## 问题现象

项目在HBuilderX本地运行时正常：

```js
uniCloud.callFunction({
  name: 'xxx',
  data: {}
})
```

本地调试没有报错，云函数也能调用。

但是上传到微信体验版之后，uniCloud相关功能就失效了，如下：
![体验版报错无法连接unicloud本地调试](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/ad/ad1b187d8966aab7804b83322da52e50225915a1b907f6b719021c1291d7624d.png)

这些报错不一定每个人都一样，但本质都指向一个问题：体验版运行环境和本地开发环境不一样。

## 小白踩的坑：上传了dev包

uni-app编译微信小程序时，常见会生成两个目录：

```text
unpackage/dist/dev/mp-weixin
unpackage/dist/build/mp-weixin
```

这两个目录看起来都像微信小程序项目，都能被微信开发者工具打开，所以很容易搞错。

小白当时凌晨开发完也没管了，想快点休息，所以预览+真机调试后就直接上传了：

```text
unpackage/dist/dev/mp-weixin
```

这个目录是HBuilderX“运行”时生成的开发调试包，适合本地调试，不适合上传体验版或正式版。

正确应该上传的是：

```text
unpackage/dist/build/mp-weixin
```

这个目录是HBuilderX“发行”时生成的线上构建包，才应该用于微信体验版和正式版。

## dev包和build包的区别

简单理解：

```text
dev/mp-weixin：本地调试用
build/mp-weixin：体验版、正式版发布用
```

dev包是开发环境产物，主要服务于本地运行、调试、热更新、控制台排查等场景。它不一定包含完整的线上uniCloud配置，甚至可能保留本地调试相关地址。

build包是发行环境产物。HBuilderX会在发行构建过程中，根据项目关联的uniCloud服务空间，把线上运行需要的配置处理进最终产物里。
## 正确发布流程

正确流程如下：

```text
HBuilderX → 发行 → 小程序-微信
```

发行完成后，打开这个目录：

```text
unpackage/dist/build/mp-weixin
```

然后用微信开发者工具打开该目录，点击右上角“上传”。

上传完成后，再去微信公众平台后台，将该版本设为体验版。

不要上传：

```text
unpackage/dist/dev/mp-weixin
```

也不要手动修改：

```text
unpackage/dist/build/mp-weixin
```

构建产物里的配置应该由HBuilderX生成，不建议手动改。

## spaceId到底要不要写到manifest.json？

普通单服务空间项目，一般不需要手动把`spaceId`写进`manifest.json`。

正确做法是：在HBuilderX中对项目的`uniCloud`目录右键，选择“关联云服务空间”。

关联好服务空间后，本地开发和发行构建都会基于这个关联关系处理uniCloud配置。

只有一种比较特殊的情况需要手动写`spaceId`：一个应用要访问多个uniCloud服务空间。这时通常是在代码里使用`uniCloud.init()`：

```js
const cloud = uniCloud.init({
  provider: 'aliyun', // 或 tencent
  spaceId: '你的spaceId'
})
```

这属于多服务空间初始化，不是普通项目为了让体验版运行而去改`manifest.json`。

## 还需要检查的几个点

如果你已经确认上传的是build包，但体验版里uniCloud还是用不了，可以继续检查下面几项。

第一，项目是否正确关联了uniCloud服务空间。

在HBuilderX中，项目根目录下的`uniCloud`目录右键，检查是否已经关联正确的云服务空间。

第二，云函数或云对象是否已经上传到云端。

本地运行正常，不代表云端已经有对应云函数。需要把云函数、云对象、数据库schema等资源上传到服务空间。

第三，微信小程序后台是否合法域名配置完整。

体验版和正式版会校验request、uploadFile、downloadFile等合法域名。本地开发工具可能因为勾选了“不校验合法域名”而正常，但手机体验版不会绕过这些限制。

第四，微信开发者工具打开的目录是否真的是build包。

建议直接看路径，确认是：

```text
unpackage/dist/build/mp-weixin
```

而不是：

```text
unpackage/dist/dev/mp-weixin
```

## 总结

这次问题的根因不是`spaceId`没写进`manifest.json`，而是小白把开发调试包上传成了体验版。

最终结论：

```text
本地运行：使用 dev/mp-weixin
体验版/正式版：使用 build/mp-weixin
```

uniCloud项目发布微信小程序时，应该使用HBuilderX的“发行→小程序-微信”，然后上传`unpackage/dist/build/mp-weixin`。

不要把`dev/mp-weixin`上传到微信后台，也不要靠手动修改`manifest.json`或构建产物里的`spaceId`来解决问题。

这个坑很隐蔽，因为dev包是开发时在微信开发者工具打开默认的，也能上传，但它本质上不是给体验版和正式版用的。以后只要记住一句话：

```text
运行生成dev，发行生成build；体验版只上传build。
```

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/4e/4e9c5bae70620998be0ca8eb60bc2c8fd222e9005029ec3cedb58dee977d4fca.png)
