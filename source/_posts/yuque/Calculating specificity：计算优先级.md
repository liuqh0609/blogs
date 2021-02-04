---
title: Calculating specificity：计算优先级
urlname: xq2an5
date: '2020-12-20 08:34:25 +0800'
tags: []
categories: []
---

## 计算方法

为给定元素计算它的优先级，遵循以下原则：

- 计算 ID 选择器的值  A
- 计算类选择、属性选择器、伪类选择器的值  B
- 计算标签选择器和伪元素选择器的值  C
- 忽略通用选择器\*

1. :is()、:not()、:has(),这三个伪类选择器的计算值将被其中优先级最高的值所代替

   > eg:
   > :is(#id)   [1,0,0] 伪类的优先级值 B 被 id 的高优先值所代替

2. 类似的, :nth-child()、：nth-last-child()的计算值是一个伪类的值加上参数的计算值之和。

   > eg:
   > :nth-child(.item)  [0,2,0]   伪类的值加上参数.item 的值

3. :where() 伪类的计算值为 0
   > eg:
   > .item:where(em,#foo)  [0,1,0]  where 伪类的值为 0

【A，B，C】

> - /_ a=0 b=0 c=0 _/
>   LI                                      /_ a=0 b=0 c=1 _/
>   UL LI                                  /_ a=0 b=0 c=2 _/
>   UL OL+LI                           /_ a=0 b=0 c=3 _/
>   H1 + _[REL=up]                  /_ a=0 b=1 c=1 _/
>   UL OL LI.red                      /_ a=0 b=1 c=3 _/
>   LI.red.level                        /_ a=0 b=2 c=1 _/
>   #x34y                               /_ a=1 b=0 c=0 _/
>   #s12:not(FOO)                   /_ a=1 b=0 c=1 _/
>   .foo :is(.bar, #baz)             /_ a=1 b=1 c=0 \*/

官方文档是按照 3 位来计算优先级的，就是上图中的：
[A, B, C]
还有一种计算方式是按照四位来计算的，最高位表示是否为行内样式：
[ L, A, B, C]

## 练习

```css
div#a.b .c[id=x] 		[0, 1, 3, 1]
#a:not(#b) 					[0, 2, 0, 0]
*.a 								[0, 0, 1, 0]
div.a 							[0, 0, 1, 1]
```
