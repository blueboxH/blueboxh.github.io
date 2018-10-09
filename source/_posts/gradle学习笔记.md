---
title: gradle学习笔记
urlName: gradle-notes
date: 2018-09-27 21:49:42
tags: 
  - gradle

---

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