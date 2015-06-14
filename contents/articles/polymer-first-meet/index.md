---
title: 初识polymer
date: 2014-10-28 00:14:48
template: article.jade
---

![polymer](https://www.polymer-project.org/images/logos/lockup.svg)

<span class="more"></span>

polymer是谷大很有野心的一个项目 用于web开发 终极目的是把js/css/html封装成标签的形式做成组件放在一个公共的仓库供开发者下载使用 比如bower或甚至有可能google自己开发维护一个gower之类的东西来做组件管理

举个例子 比如我想在google map上放一个marker 传统的做法是调用google map的api写一段js来实现 而用了polymer后只需
```
<google-map lat="37.774" lng="-122.419"></google-map>
```
醉了吗？

polymer的一些亮点:

* 不再需要模版系统 因为polymer自己本身就是模版 而且实现了命名空间 妈妈再也不用担心global问题了
* shadow dom 这个碉堡了的特性从安全性和性能上都能提升 就像他的名字一样酷
* binding 这个是目前angular最为人称道的特性 当然polymer也用上了 一样一样的\{\{\}\}

反正我是有点喜欢上polymer了 不知道polymer是什么并有兴趣的同学请戳下面
[Eric Bidelman: Polymer and Web Components change everything you know about Web development](https://www.youtube.com/watch?v=8OJ7ih8EE7s)
