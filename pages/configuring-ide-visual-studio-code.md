---
layout: default
title: Configuring Visual Studio Code
permalink: /configuring-ide-visual-studio-code/
sitemap:
    priority: 0.7
    lastmod: 2016-09-15T17:13:00-00:00
---

# <i class="fa fa-keyboard-o"></i> 配置 Visual Studio Code

Visual Studio Code 是一个微软开发的开源文本编辑器。它对 TypeScript 的非常完善，所以大家喜欢用它来开发 Angular 2 的应用。

![Screenshot]({{ site.url }}/images/configuring_ide_visual_studio_code_1.png)

## Yeoman 支持

**警告！目前改插件尚有些问题**

Visual Studio Code 有 Yeoman 的扩展插件，可以用来运行 JHipster 命令。

可以在 Visual Studio Code 的插件库中查找安装：

![Screenshot]({{ site.url }}/images/configuring_ide_visual_studio_code_2.png)

## Java 支持

Visual Studio Code 还有个由 Red Hat 提供的 Java 插件。它具有非常好的 Java 支持功能，并且使用 Maven 或 Gradle。

可以在 Visual Studio Code 的插件库中查找安装：

![Screenshot]({{ site.url }}/images/configuring_ide_visual_studio_code_3.png)

## 通用任务：编译，运行，打包

Visual Studio Code 的 Java 插件不能用来执行这些命令：不能编译、运行、或者打包应用。

有两种方式来完成这些任务：

- 使用 [JHipster App]({{ site.url }}/jhipster-app)，提供了图形界面来支持这些命令
- 或者使用终端，例如使用 Visual Studio Code 提供的内部终端来手动运行这些命令

## 使用 Spring Boot devtools 的 “热部署” 功能

JHipster 默认配置了 [Spring Boot devtools](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html)，它能让你的应用在类编译的时候自动“热部署”。这是个非常棒的功能，可以让你的开发非常顺畅。

要在 Visual Studio Code 内使用这个功能，需要：

- 在一个终端中运行程序，一般是执行： `./mvnw`
- 在另一个终端中，编译：`./mvnw compile`

在第一个终端中，JHipster 程序为自动重新部署。

如果使用了 JHipster App，就只需要点击 2 个按钮（一个用来运行程序，另一个编译），应用也会自动重新部署了。

## 定制化配置

为了得到更好的体验，建议排除一些目录，在项目根目录下的 `.vscode` 文件夹内创建一个 `settings.json`：

```
{
    // 设置排除的文件和目录。
    "files.exclude": {
        "**/.git": true,
        "**/.idea": true,
        "**/.mvn": true,
        "**/.svn": true,
        "**/.hg": true,
        "**/.DS_Store": true
    },
    // 设置搜索时排除的文件和目录。继承了 files.exclude 配置中的设置。
    "search.exclude": {
        "**/node": true,
        "**/node_modules": true,
        "**/bower_components": true,
        "**/target": true
    }
}
```
