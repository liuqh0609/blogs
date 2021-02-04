---
title: CSS排版
urlname: hn2asq
date: '2020-12-21 07:28:21 +0800'
tags: []
categories: []
---

## 盒（Box)

> HTML 代码中可以书写开始标签，结束标签，和自封闭标签
> 一对起止标签，表示一个元素
> DOM 树中存储的是元素和其他类型的节点（Node)
> CSS 选择器选中的是元素
> CSS 选择器选中的元素，在排版时可能产生多个盒
> 排版和渲染的基本单位是盒

### 盒模型

![IMG_F9E013A8321B-1.jpeg](https://cdn.nlark.com/yuque/0/2020/jpeg/2705850/1608507917849-976f9a05-930c-4cd1-9ce0-3c9417f3bc20.jpeg#align=left&display=inline&height=316&margin=%5Bobject%20Object%5D&name=IMG_F9E013A8321B-1.jpeg&originHeight=962&originWidth=1195&size=1274496&status=done&style=none&width=393)
盒模型分为两种：

> 可以通过 box-sizing 来设置不同的盒模型
> box-sizing 默认值是 content-box

1. 怪异盒模型（border-box)： `box-width = content + padding + border`
1. 标准盒模型 (content-box)： `box-width = content`
   > 标准盒模型在增加 padding 和 border 的宽度时，会保持原有的 box-width 而去对应减少 content 的 width 的所占空间

## 正常流

正常流的排版：

1. 收集盒和文字进行计算
1. 计算盒和文字在行中的排布
1. 计算行的排布

IFC：行内级格式化上下文（从左到右排列）
BFC：块级格式化上下（从上到下排列）
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608591685793-2521eba0-5803-466b-89a6-763608883ac7.png#align=left&display=inline&height=960&margin=%5Bobject%20Object%5D&name=image.png&originHeight=960&originWidth=2226&size=303741&status=done&style=none&width=2226)

## 正常流的行级排布（IFC）

### Baseline

下图中黄色的线就是基线，文字是基于**基线**来对齐的。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608592017818-80fa1342-32b6-4329-8d26-f87285f9454b.png#align=left&display=inline&height=62&margin=%5Bobject%20Object%5D&name=image.png&originHeight=248&originWidth=738&size=149484&status=done&style=none&width=185)

### Text

底层软件定义的文字
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608592161826-fb417db4-a214-46c9-a569-a667cbf53176.png#align=left&display=inline&height=169&margin=%5Bobject%20Object%5D&name=image.png&originHeight=676&originWidth=1034&size=367041&status=done&style=none&width=259)

- origin：基线的原点
- advance：排版中文字占据的空间
- bearingX：文字之间的间距
- yMin：文字基线距离文字底部的距离

### 行模型

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608592669392-5b688eca-283f-4dcb-a9c6-e2d8b49d9a05.png#align=left&display=inline&height=210&margin=%5Bobject%20Object%5D&name=image.png&originHeight=838&originWidth=1276&size=241489&status=done&style=none&width=319)

- base-line：基线，文字默认对齐的线
- text-top、text-bottom：文字的上下边缘线

  > 只要字体的大小不变，text-top 和 text-bottom 就是不变的
  > 如果用了多种字体混排的话，那么这个文字的上下边缘就是有 font-size 最大的字体决定的。
  > 我们基本可以认为这两条线是固定不变的

- line-top、line-bottom：行的上下边缘线
  > 这两条线的产生是因为行高大于文字的高度

### 文字和盒混排时产生的问题

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608593084262-c9db1a4c-cc9d-4337-a842-b06660122bd6.png#align=left&display=inline&height=238&margin=%5Bobject%20Object%5D&name=image.png&originHeight=950&originWidth=1854&size=397540&status=done&style=none&width=464)
当一个行内盒是按照 text-bottom 来对齐的话，它就会撑开该行的高度，造成偏移的情况。

> 当没有蓝色的行内盒的时候，只有文字那么该盒是只有 text-top 和 text-bottom 的高度时，文字看起来排版就比较正常，但是当蓝色盒子出现的时候撑开了 line-top，导致该行整体高度都被撑开了，那么文字相较之前的就会看起来偏下了许多。

Eg：

```html
<div style="font-size: 50px; line-height: 100px; background-color: bisque">
  <span>Hello good 国</span>
  <div
    style="
          display: inline-block;
          line-height: 70px;
          width: 100px;
          height: 150px;
          background-color: cadetblue;
        "
  ></div>
</div>
```

效果如下：
文字正常情况按照基线对齐：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608594613929-d627e469-a14d-43ac-8262-8bf03832cfc2.png#align=left&display=inline&height=121&margin=%5Bobject%20Object%5D&name=image.png&originHeight=242&originWidth=722&size=51691&status=done&style=none&width=361)
当我们添加一个行内盒的时候：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608594091726-0caea8c5-9407-453f-b4b4-e5c209ff6a65.png#align=left&display=inline&height=93&margin=%5Bobject%20Object%5D&name=image.png&originHeight=372&originWidth=1092&size=82717&status=done&style=none&width=273)
蓝色的线是 line-top，就被撑开了（该行的整体高度被撑开）。绿色的线是 text-top，行内盒不会影响 text 的边缘线高度。
当我们在行内盒添加上一个文字时：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608594403003-766b921d-5f40-4926-abfc-4ecfbc4da80c.png#align=left&display=inline&height=90&margin=%5Bobject%20Object%5D&name=image.png&originHeight=358&originWidth=1006&size=87363&status=done&style=none&width=252)
这种情况就相当于，当一个行内盒中有了文字之后，那么对齐的时候就会按照文字的基线去对齐
当我们呢在行内盒再添加一个换行和文字时：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608594527028-d79e9601-b55a-46f6-bf8c-42d524096579.png#align=left&display=inline&height=89&margin=%5Bobject%20Object%5D&name=image.png&originHeight=354&originWidth=964&size=94340&status=done&style=none&width=241)
会按照最下面文字的基线去进行对齐
上面的演示代码：

```html
<div style="font-size: 50px; line-height: 100px; background-color: bisque">
  <!-- line-top -->
  <div
    style="
          vertical-align: top;
          width: 1px;
          height: 1px;
          display: inline-block;
        "
  >
    <div style="width: 100vw; height: 1px; background-color: blue"></div>
  </div>
  <!-- text-top -->
  <div
    style="
          vertical-align: text-top;
          width: 1px;
          height: 1px;
          display: inline-block;
        "
  >
    <div style="width: 100vw; height: 1px; background-color: green"></div>
  </div>
  <!-- 基线 -->
  <div
    style="
          vertical-align: baseline;
          width: 1px;
          height: 1px;
          display: inline-block;
        "
  >
    <div style="width: 100vw; height: 1px; background-color: red"></div>
  </div>
  <!-- text-bottom -->
  <div
    style="
          vertical-align: text-bottom;
          width: 1px;
          height: 1px;
          display: inline-block;
        "
  >
    <div style="width: 100vw; height: 1px; background-color: green"></div>
  </div>
  <!-- line-bottom -->
  <div
    style="
          vertical-align: bottom;
          width: 1px;
          height: 1px;
          display: inline-block;
        "
  >
    <div style="width: 100vw; height: 1px; background-color: blue"></div>
  </div>
  <span>Hello good 国</span>
  <div
    style="
          display: inline-block;
          line-height: 70px;
          width: 100px;
          height: 150px;
          background-color: cadetblue;
        "
  >
    M N
  </div>
</div>
```

## 正常流的块级排布（BFC)

### float 和 clear

float：

- 严格来说是已经脱离文档流的，他在排列时可以完成图文环绕排的形式。
- 多个 float 元素会依次排列，不会占据对方的空间
- ![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608854033824-58eec322-5bd5-4d17-a5b4-c8d43b51202c.png#align=left&display=inline&height=66&margin=%5Bobject%20Object%5D&name=image.png&originHeight=264&originWidth=1400&size=15569&status=done&style=none&width=350)
- float 会造成重排的问题

clear：与其说是清除浮动，不如说是在某个方向上找个干净的地方去完成浮动的排列

- 与上面同样的代码，只要给第二个 float 的红色 div 设置 `clear: right`  就可以实现下面这种效果
- ![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608854110759-07d731d4-93c2-4726-97bf-81e708a9a0f3.png#align=left&display=inline&height=60&margin=%5Bobject%20Object%5D&name=image.png&originHeight=242&originWidth=1405&size=14317&status=done&style=none&width=351)
- 所以说 clear 更像是在右面找了一个干净的地方进行浮动排列

### margin 折叠

这种现象只会发生在正常流中，在正常流中只有 BFC 才会有！

上下两个块级元素都有 margin 的情况下，在排列的时候会选取一个 margin 的最大值作为其最终排版的结果
![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608854541877-e2831a64-ad81-4f32-a22f-9b377bd8495f.png#align=left&display=inline&height=188&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1212&originWidth=1650&size=172874&status=done&style=none&width=256)  =====> ![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1608854549792-733a8918-101f-4602-bcbf-2c216a5fe612.png#align=left&display=inline&height=188&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1098&originWidth=1714&size=173370&status=done&style=none&width=294)

### BFC 合并

- Block Container： 里面有 BFC
  - 能容纳正常流的盒，里面就有 BFC
- BLock-level Box： 外面有 BFC 的
- Block Box = Block container + Block-level Box：里外都有 BFC 的
