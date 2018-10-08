---
layout: default
title: 配置 Intellij IDEA
permalink: /configuring-ide-idea/
redirect_from:
  - /configuring_ide_idea.html
sitemap:
    priority: 0.7
    lastmod: 2015-11-28T17:13:00-00:00
---

# <i class="fa fa-keyboard-o"></i> 配置 Intellij IDEA

## 打开你的项目

- 首先打开你的项目
- IDEA 会检测到 Maven , 然后你的项目会开始自动编译

如果你希望更多的控制项目的设置，你可以选择导入项目 “Import project”

## 排除一些目录

如果你使用 Git, 只需要用 Git 初始化你的项目 (`git init && git add . && git commit -m 'Initial commit'`), Intellij IDEA 会自动排除一些目录 (所以，你不需要做任何额外的操作).

如果要手动排除一些目录:

- 右键 `node_modules/` 目录
- 选择 "Mark Directory As"，选择 "Excluded"

![Exclude]({{ site.url }}/images/configuring_ide_idea_1.png)

因为`node_modules/` 目录只需要被 JHipster 本地调用，它也可以被排除掉。

_注意_ 在使用 AngularJS 1 时，一些开发者习惯于排除目录 `src/main/webapp/bower_components` ，因为那里面有很多 JavaScript 代码。但是，这个目录中包含着开发应用所需的框架和工具，所以简单的排除它们会引起 JavaScript 支持的一些错误。所以，这种情况下，_不建议_ 排除这个目录。

## Spring 支持 (注意 IDEA CE 版本不支持)

添加 Spring 支持，打开菜单：`File → Project Structure`.

![Project Structure]({{ site.url }}/images/configuring_ide_idea_2.png)

进入 Modules 标签，点击 `+` 按钮，选择 "Spring" 来添加 Spring 代码助手。

![Spring]({{ site.url }}/images/configuring_ide_idea_3.png)

你会得到一个没有 map 到 Spring 配置文件的提示，点击右下角的 `+` (不是之前那个 + 按钮)，选择所有你项目上的 Spring 文件，选择目录就可以选择所有文件。

![Spring Application Context]({{ site.url }}/images/configuring_ide_idea_4.png)

点击 `OK`，Spring 支持就被配置好并且会有代码提示助手功能了。

现在点击原来那个添加 Spring 支持的 `+` 按钮，选择添加 Hibernate。你不需要添加任何
文件，只需要添加就能完成 Hibernate 支持的代码提示了。记得点击 Project structure 对话框上的 `OK`。

现在你应该能具备代码库里的 Spring 支持。每次新建一个项目你都需要这么做一次，因为这些设置是项目特定的。

## 使用 Spring Boot devtools 来使项目支持 "热重启" 

JHipster 配置了 [Spring Boot devtools](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html) ，它可以让你的项目在类编译后自动“热启动”。这是个非常棒的特性，可以让你开发效率大大提升。

默认情况下，IntelliJ IDEA 设置为不会自动编译项目文件。如果要打开 "Compile on save" 设置:

* `File -> Settings -> Build, Execution, Deployment -> Compiler`，勾选 "Make project automatically"
* 打开 Action 设置窗口 :
  * Linux : `CTRL+SHIFT+A`
  * Mac OSX : `SHIFT+COMMAND+A`
  * Windows : `CTRL+ALT+SHIFT+/`
* 输入 `Registry...`，勾选 `compiler.automake.allow.when.app.running`

## Maven 的 IDE profile 支持

如果你使用的是 Maven, 你需要在 Intellij IDEA 里激活 `IDE` profile 功能。This is used for applying IDE-specific tweaks
which currently only includes applying the MapStruct annotation processor.

打开 "Maven Projects" 工具窗口 (View -> Tool Windows), 点击 `IDE` maven profile 来激活这项功能。（译注：我没找到）

## Gradle

In order to get the best out-of-the-box experience with Gradle you should delegate all [IDE build/run actions to Gradle](https://www.jetbrains.com/idea/whatsnew/#v2017-3-gradle) directly. With this setting annotation processing is automatically configured and you won't have duplicated classes when mixing IDE and cli builds. If you are using an older version ( < 2016.3) you have to enable annotaion processing manually.
