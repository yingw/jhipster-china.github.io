---
layout: default
title: 设置 Eclipse 使用 Gradle
permalink: /configuring-ide-eclipse-gradle/
redirect_from:
  - /configuring-ide-eclipse-gradle.html
sitemap:
    priority: 0.7
    lastmod: 2015-05-22T18:40:00-00:00
---

# <i class="fa fa-keyboard-o"></i> 设置 Eclipse 使用 Gradle

要在 Eclipse 中使用 Gradle，你需要安装 [buildship 插件](https://gradle.org/eclipse/).
配置 [JavaScript]({{ site.url }}/configuring-ide-eclipse/) 的方式可以参考 Maven 设置章节的说明。

## 1. 将项目已 Gradle 项目导入

- 选择 ``File -> Import``
- 选择 ``Gradle Project``
- 选择项目的根目录
- 点击 ``Next`` 结束向导

![Import]({{ site.url }}/images/configuring_ide_eclipse_gradle_1.png)

![Select]({{ site.url }}/images/configuring_ide_eclipse_gradle_2.png)

## 2. 添加 apt 生成代码的目录到编译路径

使用 buildship gradles 默认的输出目录被过滤了，在你的工作空间中是不可见的。
因此，你需要将之移出 Eclipse 资源过滤设置。

- 右键项目，选中 ``Properties``
- 选择 ``Resources``
- 移除 ``build``
- 选择 ``Java Build Path``
- 点击 ``Add Folder...``
- 选中 ``build/generated/source/apt/main``

请确保新的源码路径包含了正确的 mapper 实现。

![Exclude]({{ site.url }}/images/configuring_ide_eclipse_gradle_3.png)

![Buildpath]({{ site.url }}/images/configuring_ide_eclipse_gradle_4.png)
