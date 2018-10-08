---
layout: default
title: 配置 Eclipse 使用 Maven
permalink: /configuring-ide-eclipse/
redirect_from:
  - /configuring_ide_eclipse.html
sitemap:
    priority: 0.7
    lastmod: 2015-05-22T18:40:00-00:00
---

# <i class="fa fa-keyboard-o"></i> 配置 Eclipse

在 Eclipse 里配置 JHipster 应用需要一些手工步骤。你需要做以下设置：

- 设置 Maven
- 设置 JavaScript (让 Eclipse 能忽略一些静态文件的目录)

## 1. 以 Maven 项目导入

- 选择 File -> Import
- 选择 "Existing Maven Projects"
- 选择你的项目
- 点击 "Finish"

![Import]({{ site.url }}/images/configuring_ide_eclipse_1.png)

![Select]({{ site.url }}/images/configuring_ide_eclipse_2.png)


在导入的最后，你会看到这个对话框。"Maven plugin connectors" 是 m2eclipse 的扩展。需要安装且 Eclipse 在安装完会重启。

如果你之前已经装过了，你就可以继续不需要做任何额外的操作。

![Select]({{ site.url }}/images/configuring_ide_eclipse_maven_processor.png)

__Note__: 如果你已经有一个 JHipster 项目且没有安装管理的 connector，你会看到如下错误：

`Plugin execution not covered by lifecycle configuration: org.bsc.maven:maven-processor-plugin:2.2.4:process (execution: process, phase: generate-sources)`

点击 Quick Fix/Ctrl+1 (Cmd+1 on Mac) ，选择 "Discover new m2e connectors"

## 2. 排除生成的静态资源目录
现阶段你应该还没有看到任何 Java 代码报错，但是会有些 JavaScript 错误。这是因为有些 JavaScript 文件 Eclipse 不能正常解析。这些文件是运行时需要的，它们不需要在你的工作空间使用。他们可以被排除掉。


### 排除 ‘node_modules’ 目录

- 右键你的项目 -> Properties -> Resource -> Resource Filters
- 选择: Exclude all, Applies to folders, Name matches 输入 node_modules
- 点击 "Ok"

![Right-click]({{ site.url }}/images/configuring_ide_eclipse_3.png)

![Exclude]({{ site.url }}/images/configuring_ide_eclipse_4.png)


### 排除 'app' 在目录 src/main/webapp 里

- 右键你的项目 -> Properties -> Javascript -> Include path
- 点击 “source” 标签，选择你的项目目录 /src/main/webapp
- 选择 “Excluded: (None) -> Edit -> Add multiple
- 选择  `app` 点击 “Ok”
- 这些目录应该会自动被排除（如果没有，手动排除一些）:
    - `bower_components`
    - `node_modules/`

![Right-click]({{ site.url }}/images/configuring_ide_eclipse_5.png)

![Exclude]({{ site.url }}/images/configuring_ide_eclipse_6.png)

![Multiple select]({{ site.url }}/images/configuring_ide_eclipse_7.png)

### Maven IDE profile

如果你在使用 Maven, 你还要激活 `IDE` profile。This is used for applying IDE-specific tweaks, which currently only includes applying the MapStruct annotation processor.

- 右键你的项目 -> Properties -> Maven
- 在 "Active Maven Profiles" 里，输入 `dev,IDE`

配置完成后，你就能使用 JHipster 的 `dev` 和 `IDE` profiles 了。


### 配置 MapStruct 插件

为了使你的 IDE 能正确认识 mapstruct 生成的代码，还需要做些设置：

需要安装插件 m2e-apt (https://marketplace.eclipse.org/content/m2e-apt)，来让 Eclipse 和 mapstruct 一起工作。

也可以安装插件：MapStruct Eclipse Plugin (https://marketplace.eclipse.org/content/mapstruct-eclipse-plugin) 来从 IDE 获取帮助和提示。 
