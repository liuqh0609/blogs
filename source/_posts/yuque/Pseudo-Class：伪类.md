---
title: Pseudo-Class：伪类
urlname: ct385n
date: '2020-12-20 08:33:41 +0800'
tags: []
categories: []
---

## 链接/行为

之前这些伪类的实现都是为了超链接设计的，但是现在有很多伪类都可以用到其他元素上了。

- :any-link 任何超链接
- :link 没有访问过的超链接
- :visited     已经访问过的超链接

  > any-link 可以看做是 link 和 visited 的结合
  > Tips：
  > 一旦使用了 link 或者 visited 伪类之后，就没有办法去修改除了颜色以外的 css 属性了。
  > 这样做是为了浏览器安全考虑的。因为一旦你更改了像 visited 超链接的大小的时候，就可以让别人明显感受到你访问过哪些网站，这对于用户来说是不太容易接受的，就像泄漏了隐私一样，也不符合浏览器安全的相关策略。

- :hover
- :active
- :focus
- :target

## 树结构

- :empty
- :nth-child()     从前让后数
- :nth-last-child() 从后往前数
- :first-child
- :last-child
- :only-child

## 逻辑型

- :not 伪类
- :where
- :has

在 css 书写过程中，我们不应该写过于复杂的选择器，这样一是对性能不好，二是可能自己 HTML 结构设计的有问题。
