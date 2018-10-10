---
title: gradle学习笔记
urlName: gradle-notes
date: 2018-09-27 21:49:42
tags: 
  - gradle

---

> maven 和 gradle 作为java的项目构建工具, 两者各有优缺点, 就我个人感觉而言, 觉得maven这种xml格式的上手比较容易, 新手看几条命令也能很容易的理解要表达的意思, 而 gradle 用的脚本语言, 可能需要一定的学习成本, 但是表现力更为强大. 这次由于要学习springcloud要使用多项目构建, 就来学习一下在这边面更优秀的 gradle.

<!-- more -->

## 命令

- https://www.w3cschool.cn/gradle/9b5m1htc.html

## 生命周期

- `compile`

  用来编译项目源代码的依赖.

- `runtime`

  在运行时被生成的类使用的依赖. 默认的, 也包含了编译时的依赖.

- `testCompile`

  编译测试代码的依赖. 默认的, 包含生成的类运行所需的依赖和编译源代码的依赖.

- `testRuntime`

  运行测试所需要的依赖. 默认的, 包含上面三个依赖.