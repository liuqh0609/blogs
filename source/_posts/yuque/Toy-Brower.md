---
title: Toy-Brower
urlname: csr8da
date: '2020-12-02 21:38:57 +0800'
tags: []
categories: []
---

为了更加了解浏览器的工作原理，我们来自己用代码搞一个浏览器玩一下。

## 基本功能描述

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1607396985290-ee9fbd15-7e2e-4ab0-9f71-7adf34a29f88.png#align=left&display=inline&height=178&margin=%5Bobject%20Object%5D&name=image.png&originHeight=178&originWidth=1506&size=163962&status=done&style=none&width=1506)

## 功能实现

### HTTP 实现

为了实现浏览器发送网络请求的这一部分，需要梳理一下这部分所需要的功能。

1. 构建请求信息（请求方法、请求行、请求头、请求体）
1. 建立网络连接（net.createServer()）
1. 接收响应结果
1. 处理响应结果

   1. 处理响应头
   1. 处理响应体 - 利用单独的子类进行 body text 的处理

1. 构造一个发送请求的类 `Request`

   > 思考：这个类都需要什么东西？
   >
   > - 首先需要一些发送请求时需要配置的基本配置项（options）
   >   - url    请求路径
   >   - port     请求端口
   >   - method     请求方法
   >   - headers     请求头
   >   - body     请求体
   > - 其次需要一个发送该请求的方法（send）
   >   - 该方法返回一个 promise 对象，该对象是请求回来的结果
   >   - 该方法的功能：
   >     - 构造请求并发送，发送前需要拼接上请求行、请求头、换行、请求体等信息。
   >     - 接受请求结果

1. 构造一个解析响应的类 `ResponseParse`

   > - response 返回的结果需要分段来进行处理，所以我们需要一个 ResponseParse 来进行装配
   > - ResponseParse 来分段处理返回的结果，我们用有限状态机来记录完成这一个过程
   > - 针对 response body 中的内容采用另一个方法进行分段解析

1. 构造一个类 `TrunkedBodyParser`
   > - response body 针对不通的 Content-type 类型会有不同的处理方法
   > - 我们这里针对 chunk 类型进行解析

---

### HTML 解析的实现

1. 接收响应体进行解析

![image.png](https://cdn.nlark.com/yuque/0/2020/png/2705850/1607643059789-38ccd590-7e2b-42a1-8852-b7b75fa9d219.png#align=left&display=inline&height=64&margin=%5Bobject%20Object%5D&name=image.png&originHeight=128&originWidth=1108&size=79721&status=done&style=none&width=554)

2. 构造状态机进行标签解析
   1. 开始标签
      1. `<`  开始标签的标志
      1. `/n/t `  空格标志后面是标签属性
   2. 结束标签
      1. `/`  结束标签的标志
      1. `>`  标签解析结束，开始下一个标签解析
   3. 自封闭标签
      1. ` </ `  自闭和标签的标志
3. 利用 currentToken 变量接受解析结果

   1. `text`文本类型
   1. `statTag`开始标签类型
   1. `endTag`结束标签类型
   1. `isSelfCloseTag`自闭和标签标志
      > 每次当前标签结束时需要`emit(currentToken)`来触发当前标签结束的事件

4. 解析标签属性
   1. 氛围 attributeName 和 value 两部分进行解析
   1. 解析完成之后 emit 对应数据
5. 构建 DOM 树
   1. 使用栈进行构造
   1. 遇到开始标签就处理好相关属性和元素名入栈
   1. 遇到自闭和标签就相当入栈后立即出栈
   1. 遇到闭合标签就找到对应标签出栈

### CSS 计算

1. 遇到 style 标签时，将 css 规则保存起来，使用 css parser 解析规则
1. 当创建一个元素后应立即计算 css
   1. 理论上当我们分析一个元素时，是假设这个元素的 css 规则已经全部收集完毕的
1. 在 computedCss 函数中，我们需要知道所有元素的父元素才可以去判断元素是否与规则匹配
   1. 由于我们首先获取的是当前元素，所以我们获得和计算父元素匹配的顺序是由内到外的
1. 选择器也是由内向外匹配排列的
1. 根据选择器的类型和元素属性，计算是否当前的元素匹配
1. 元素匹配完成就应用选择器到元素上，形成 computedSty
1. css 规则具有优先级，我们利用 specificity 特征去进行判断比较
   1. specificity 是一个四元组，越左边权重越重
      1. [0,0,0,]
      1. inline、id、class、tag
   2. css 规则的 specificity 是根据所包含的简单选择器相加形成的
      ![](https://cdn.nlark.com/yuque/0/2020/jpeg/2705850/1607825664969-8a234679-ac32-4c70-a494-6f6a7a466227.jpeg)
