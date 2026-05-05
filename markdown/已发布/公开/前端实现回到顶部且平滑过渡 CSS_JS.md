# 背景

> 最近同学在项目开发的时候问了我一个问题：小白，回到顶部该怎么做呀？我当时就愣住了，心想这不是很基础的一个功能吗，然后想到该同学没有系统学过网页三剑客，我就给他讲了该怎么实现这个虽然基础但在很多项目中都很实用的功能。

> 不过我还是笑了，为啥，因为我不允许还有人不会这个听起来貌似高大上的==回到顶部==，所以我选择更一篇。（大佬绕道 /plea手）
# 基本介绍
本文仅介绍回到**顶部功能的CSS做法**~~（毕竟这么简单没有特别的需求都能用）~~ 。
> 后续或许会出涉及JS的用法

## 什么是回到顶部按钮？
回到顶部按钮是一个浮动在页面右下角的小图标，用户点击后可以立即返回到页面的顶部。这种设计可以有效地提高网站的可用性，尤其是在移动设备上，用户只需轻轻一按就能回到开始阅读的位置。

## 代码实现
以下是实现回到顶部效果的 HTML 和 CSS 代码示例，功能以外的样式从简处理。
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>back-to-top-demo</title>
    <style>
        /* 通配 */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        /* 滚动条样式 */
        /* 定义滚动条宽度和背景颜色 */
        ::-webkit-scrollbar {
            width: 8px;
            background-color: #F5F5F5;
        }

        /* 定义滚动条轨道的阴影和圆角 */
        ::-webkit-scrollbar-track {
            -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            background-color: #F5F5F5;
        }

        /* 定义滑块的圆角和阴影 */
        ::-webkit-scrollbar-thumb {
            border-radius: 10px;
            -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
            background-color: #555;
        }
        html,
        body {
            /* height: 100%; */
            /* width: 100%; */
            background-color: rgba(153, 153, 255, .3);
            /* 平滑过渡效果 */
            scroll-behavior: smooth;
        }

        /* scoped样式 */
        #title {
            text-align: center;
            font-weight: 900;
            font-family: '宋体';
            padding: 10px;
        }

        #to_top_ball {
            display: block;
            text-align: center;
            line-height: 60px;
            /* display: flex;
            justify-content: center;
            align-items: center; */
            width: 60px;
            height: 60px;
            font-size: 50px;
            background-color: rgb(153, 204, 255);
            border-radius: 50%;
            text-decoration: none;
            color: rgb(255, 255, 255);
            box-shadow: 0 0 20px 0 rgba(0, 0, 255, .5);
            position: fixed;
            bottom: 20px;
            right: 20px;
            /* opacity: .6; */
            transition: all .6s;
        }

        #to_top_ball:hover {
            background-color: rgb(255, 102, 102);
            box-shadow: 0 0 30px 0 rgba(255, 0, 0, .8);
            transform: translate(0, -10px);
        }
    </style>
</head>

<body>
    <div id="index">
        <h1 id="title">我是标题</h1>
        <div id="content">
            <p id="a">我是内容</p>
            <p>我是内容</p>
            <p>我是内容</p>
            <p>我是内容</p>
            /* p{我是内容}*100；需要自己添加足够多能出现滚动条的内容 */
        </div>
        <a href="#index" id="to_top_ball">↑</a>
    </div>
</body>

</html>
```
## 重点代码
### 平滑过渡
==**很多人会嫌CSS做的回到顶部太过于生涩，点一下它就直接跳到目标锚点了，然后纷纷选择使用JS，但事实的确如此吗？CSS真的做不了平滑过渡的拉动效果吗？当然不！**== 一行CSS样式设置让你对它刮目相看。
```css
html,
body {
    /* ...other codes... */
    scroll-behavior: smooth;/* 平滑过渡效果 */
}
```
### #to_top_ball
```css
#to_top_ball {
	/* 球内内容水平垂直居中法一 */
    display: block;
    text-align: center;
    line-height: 60px;
    /* 球内内容水平垂直居中法二 */
    /* display: flex;
    justify-content: center;
    align-items: center; */
    width: 60px;
    height: 60px;
    /* 控制箭头大小 */
    font-size: 50px;
    background-color: rgb(153, 204, 255);
    border-radius: 50%;
    text-decoration: none;
    color: rgb(255, 255, 255);
    /* 呈现立体效果 */
    box-shadow: 0 0 20px 0 rgba(0, 0, 255, .5);
    /* 固定定位，相对窗口 */
    position: fixed;
    bottom: 20px;
    right: 20px;
    /* 过渡效果，球hover后不生涩 */
    transition: all .6s;
}
/* 球hover后的效果 */
#to_top_ball:hover {
    background-color: rgb(255, 102, 102);
    box-shadow: 0 0 30px 0 rgba(255, 0, 0, .8);
    transform: translate(0, -10px);
}
```
### #to_top_ball的内容控制
```css
#to_top_ball {
	/* 球内内容水平垂直居中法一 */
    display: block;
    text-align: center;
    line-height: 60px;
    /* 球内内容水平垂直居中法二 */
    /* display: flex;
    justify-content: center;
    align-items: center; */
    /* ...other codes... */
}
```
## 主要知识点
主要利用了a标签的href属性与其他标签的id属性进行配合
### Q&A

> Ⅰ
> **Q：a标签的href属性与其他标签的class属性进行配合可以吗？**
> A：当然肯定必须不行呀，举个例子，你喝完孟婆汤之后被带到了一个分叉路口，前面四五个指示牌都是罗马，这你怎么走，一不小心选错就变牛马...

>Ⅱ
>**Q：a标签href属性的值我可以写#top吗？**
>A：当然肯定必须可以呀，只要想达到的效果是回到当前页面顶部就行，自己写带id的元素只是可以更灵活控制scroll到的位置。

# 总结
等一个课代表评论区总结，笑。

# 二编（JS版本）

> 依旧平滑，q弹动画
> es5语法，无节流

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>scrollToTop-js</title>
</head>
<style>
  html, body {
    height: 100%;
  }
  body {
    margin: 0;
    background-color: rgba(221, 160, 221,.3);
  }
  h1 {
    text-align: center;
    font-family: "宋体";
  }
  .toTopBall  {
    position: fixed;
    right: 20px;
    bottom: 40px;
    width: 70px;
    height: 70px;
    border-radius: 50%;
    background-color: #fff;
    line-height: 70px;
    text-align: center;
    color: purple;
    font-size: 40px;
    font-weight: 900;
    font-family:Cambria;
    user-select: none;
    cursor: pointer;
    opacity: 0;
    box-shadow: 0 0 30px 5px;
    transition: all .7s;
  }
  .toTopBall:hover {
    transform: scale(1.15);
    box-shadow: 0 0 30px 3px;
    opacity: .8;
  }

  .muffleBox {
    z-index: 3;
    position: fixed;
    right: 20px;
    bottom: 40px;
    width: 70px;
    height: 70px;
    background-color: rgba(255, 228, 196, 0);
  }
</style>
<body>
  <h1>回到顶部</h1>
  <div class="toTopBall">
    <div class="icon">↑</div>
  </div>
  <script>
    var paragraphEl = document.createElement("p")
    var ballEl = document.querySelector(".toTopBall")
    var muffleEl = document.createElement("div")

    // 创建遮罩层，防止不显示状态下误点击
    muffleEl.classList.add("muffleBox")
    ballEl.after(muffleEl)

    paragraphEl.textContent = "我是段落1"
    document.body.append(paragraphEl)
    for(var i = 2; i <= 100; i++) {
      var newParaEl = paragraphEl.cloneNode()
      newParaEl.textContent = `我是段落${i}`
      document.body.append(newParaEl)
    }

    ballEl.onclick = function() {
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      })
    }

    window.onscroll = function() {
      // console.log(window.scrollY)
      if(window.scrollY > 500) {
        ballEl.style.opacity = 0.5
        muffleEl.remove()
      } else {
        ballEl.style.opacity = 0
        // 创建遮罩层，防止不显示状态下误点击
        ballEl.after(muffleEl)
      }
      // // 验证只是移除了元素，并没有删除dom对象以及添加的属性
      // console.log(muffleEl)
    }
  </script>
</body>
</html>
```

---

## Cover 图

![cover_1](前端实现回到顶部且平滑过渡 CSS_JS.assets/0e68335220c35895.png)
