### 如何使typora图片不居中留白？

> 驻波使用`typora`记笔记的时候，好几次插入图片太大选择缩小都会发现图片仍然滞留在中间，居中显示，但我本人觉得并不好看，所以我决定改一下，于是有了这篇博客

#### 检查看原理
软件内鼠标右击选择`检查元素`
![检查看原理](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/90/90902cda75fe4ea785a1ab80d412fd122c2b373181838d0a06d1d4ce0182594d.png)
单击图片定位元素
查看右侧styles
![检查看原理](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/1f/1fc63aa6eca8a43ceb2b21f3ef1c33cf5c9a7408b10af464306a5b64cfa118de.png)
找到对应样式，`margin:auto`这的确是常用来做居中的，咱们反勾选试试看找对了没
![检查看原理](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/0d/0d94b48992a9f7f8350a77884ddee99b4a96d50ab01b7d885e554112adfaabb3.png)
诶，对了！**但这只是临时调试，退出后不会保留，和在浏览器中调试一样，接下来改全局**
![检查看原理](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/62/62f6f4c6773f1369105dbc60dd9a2c60f7184928d02894adb9e9bf85baa7e267.png)
#### 更改全局样式
进入默认全局样式文件
![更改全局样式](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/54/54e63adc0f882b632752b18f286dc52b363c42b842d3c26fa6b47cb364260f18.png)
`vscode`打开（文本编辑器也行），**`ctrl+f`查找之前定位的选择器**
![更改全局样式](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/7b/7bd5ecf2dca167ea67d443e30ea701eceebd9450c98104d643767c3c3cb1cc8a.png)
删去`margin:auto`，`ctrl+s`保存修改退出
![更改全局样式](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/16/16e1fd201fdfd836700ac198af9ed64226b724532d70037d12449f5420038fe6.png)
#### 重启`typora`生效
![重启typora生效](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/2b/2ba9275582004f228ed6d49f90bc9be3c48e7b2e7bd1ce67b35fe79e715aad64.png)
![重启typora生效](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/bd/bdf02485eed9f1ec1f68c626a7bdeaa4d25f1b966655cdcc5cdcb0d503fc8fa0.png)
##### “小”功告成！🥳

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/67/676d7cc7fc03a891f155c5a1fcabcc66e8e340a88d6c93cb71cd27626587013a.png)
