---
title: Coding Devops / Jenkinsfile
date: 2022/01/04
categories: 
- logs
---
Suda的小程序页面编辑器是基于Vue.js/ElementUI, 编辑器目前已经演化为多个版本，小商店装修中使用的就是其中一个特别针对小商店的定制版本。
当出现版本分支后，主分支的维护和次分支的合并更新等都会比较麻烦，为了解决这个问题，我们在Coding上的做法如下：

主分支以更新核心功能和实现各种各样的组件
不同分支从主分支拉取更新并合并，尽量保证不去修改主分支的代码，这个过程比较痛苦
不同分支的新组件可以通过request的方式加入主分支

然后为每个编辑器进行单独的build和test

---

Coding.net 使用的是 Jenkinsfile [了解Jenkinsfile](https://www.jenkins.io)

上手难度很小，并且具有图形化的编辑器，对于不太复杂的编译和构建，都可以很轻松的进行编辑

![screen shot](https://live.staticflickr.com/65535/51801710355_162f81b4ff_z.jpg)

