---
title: DAO、Service、Controller、Model、View的简单思考
tags: Java
categories: Java
---
### 前言
> 最近海口天气略为湿热，基本上脑子也处于腐朽的状态。最近师傅和哥哥叫我碰了JFinal这个RESTful这个轻量级的框架。感觉脱离了视频自学还是那么困难，只发现一味地去翻手册索然无味，然后就是一堆概念的袭来。DAO、Model、Service、Controller、 View。什么玩意啊，从Servlet和JSP跳到这一块我感觉很懵蔽，特别是AOP那一部分本身就感觉Spring的AOP已经很难去理解，还要火上浇油。

### 正文
> 首先是MVC这个概念俗称Model-View-Controller的缩写。其实谈不上设计模式，他只是存在在表现层的一种模式。我们可以来简单看一下一般流程是什么加深我自己的理解
1.首先是用户通过浏览器在页面上发生操作比如说发微博（当然微博怎么具体操作我不知道我只能稍微敏感下）那么输入完内容肯定是发表了，这一瞬间就会提交给服务器
2.根据提交的这个请求跳转到Controller层的具体Action处理，那么接着它会调用可以提供发微博这项服务的Service去
3.然后就是通过Service去调用DAO（持久层）全称Data Acess Object，那么DAO是干什么的呢？DAO其实就是封装出来和底层数据库进行操作的对象。（狭义理解广义其实只要对数据有操作都可以算DAO，比如说对数据的写入硬盘）
4.DAO再通过对数据库进行CRUD的操作再更改model对象model对象其实就是一个封装了数据的对象（方便进行操作），其实通过fastjson解析这个对象其实可以转换为json数组进行传输！
> 上面只是我对这几个概念的简单理解希望以后能多写些代码再去加深理解

### 其他
> 稍微来说一下RESTful这个概念REST全称（Resource）Representational State Transfer 中文来说就是资源在表现层进行状态转换
> 我们想让表现层状态发生转变，只能用到HTTP协议了，那么只能靠GET（获取资源）、PUT（更新资源）、POST（创建资源）、DELETE（删除资源）。
- 每一个URI代表资源
- 客户端和服务器之间，传递这种资源的某种表现层；
- 客户端通过四个动词，对服务器资源进行操作，实现“表现层状态转化”。

> 总的来说就是通过设计服务器端的RESTful的API让客户端访问URI的时候充分利用HTTP协议，让URI符合RESTful这种风格