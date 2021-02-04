---
title: CSS结构
urlname: gpovl9
date: '2020-12-14 07:30:32 +0800'
tags: []
categories: []
---

因为找不到一个明确的语法线索，所以我们这里根据 2.1 版本的语法来进行学习。
现版本 css 多了很多 css3 的语法，但是不影响我们理解他的语法结构。

## CSS2.1 语法

- [css2.1Grammar 中文对照版](http://www.ayqy.net/doc/css2-1/grammar.html)

## 总体结构

一个样式文件从上到下的顺序结构：

1. @charset

   > `@charset 'UTF-8';` 
   > 声明 css 文件的字符编码标准。
   >
   > - 必须在样式表的第一行声明
   > - 如果有多个@charset 声明，那么只有第一个会生效
   > - 无法在 HTML 的 style 标签里使用该 at-rule 规则
   >
   > Tip:
   > 在样式表中声明字符编码有很多种方式，浏览器会按照以下顺序去尝试确定文件的编码方式（只要找到一种就会停止并得出结果）：
   >
   > 1. 文件开头的  [Unicode byte-order](http://en.wikipedia.org/wiki/Byte_order_mark)  字符值
   > 1. 由 Content-type 确定：HTTP 协议中的  charset 属性给出的值或用于提供样式表的协议中的等效值
   > 1. CSS @rule 规则：@charset
   > 1. 假设文档上是 UTF-8 的格式

2. @import
3. rules （这部分是我们日常最常用到的部分）
   1. @media
   1. @page
   1. rule

## CSS 规则的结构

```css
div /* selector */ {
  /* key */
  background-color: /* value */ red;
}
```

由上面的 css 代码可以明确 css 规则的结构分为以下两个部分：

1. Selector（选择器）
1. Declare（声明）
   1. key
   1. value

## 爬取 w3c 的 css 规则

```javascript
JSON.stringify(
  Array.prototype.slice
    .call(document.querySelector("#container").children)
    .filter((e) => e.getAttribute("data-tag").match(/css/))
    .map((e) => ({
      name: e.children[1].children[0].innerText,
      url: e.children[1].children[0].href,
    }))
);
```

## 思维导图

![](https://cdn.nlark.com/yuque/0/2020/jpeg/2705850/1608424173469-cf7b1df7-98e3-4d61-aa22-74c472f7e380.jpeg)
