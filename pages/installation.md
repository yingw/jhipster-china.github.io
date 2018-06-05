---
layout: default
title: 安装 JHipster
permalink: /installation/
redirect_from:
  - /installation.html
sitemap:
    priority: 0.7
    lastmod: 2016-12-21T00:00:00-00:00
---

# <i class="fa fa-cloud-download"></i> 安装 JHipster

## 安装方式

我们提供了 6 种安装 JHipster 的方式。如有不确定选择哪种，请选择第二种“使用 Yarn 本地安装”：

*   [JHipster Online](https://start.jhipster.tech/) 是一个最简单的方式来使用 JHipster 生成应用，甚至不需要安装本地 JHipster。
*   "使用 Yarn 本地安装" 是典型的安装方式。所有需要的组件都会安装在你的机器上，设置稍微有点复杂，但是是大部分人的选择。如果不确定选择哪种安装方式，选择这个就是了。
*   "使用 NPM 本地安装" 基本类似 "使用 Yarn 本地安装"，区别在于使用 NPM 而不是 [Yarn](https://yarnpkg.com/)
*   "使用包管理器安装" 只支持 Mac OS X 和 Windows。如果你已经在使用包管理器的话，这个是比较简单的安装方式，请注意该功能目前还是 BETA 阶段。
*   基于 Vagrant "[development box](https://github.com/jhipster/jhipster-devbox)", 所有工具都安装在一个 Ubuntu 虚拟机中。
*   基于 "[Docker](https://www.docker.io/)" 容器，提供更轻量的的 JHipster 环境。

## JHipster Online (为那些希望简单上手 JHipster 的用户)

[JHipster Online](https://start.jhipster.tech/) 使你能方便的生成 JHipster 应用，且不需要安装 JHipster。

目的适用于那些第一次尝试 JHipster 的用户，或者简单看一下 JHipster 能力的用户。

虽然使用上简单，但这个并非 "完整的 JHipster 体验", 一旦你的应用创建好了你还是需要根据后面章节（使用 Yarn 本地安装）的大部分步骤，就如你也需要 Java (来运行你的程序) 和 Yarn (来管理你的前端)。

我们希望在为了给 JHipster Online 提供更多的功能。

## 使用 Yarn 本地安装（推荐方式）

### 使用 Angular 的设置

1.  安装 Java 8 [Oracle 官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2.  安装 Node.js [Node.js 官网](http://nodejs.org/) (建议选择 LTS 64位 版本)
3.  安装 Yarn [the Yarn 官网](https://yarnpkg.com/en/docs/install)
4.  如果你打算使用 JHipster Marketplace, 安装 Yeoman: `yarn global add yo`
5.  安装 JHipster: `yarn global add generator-jhipster`

一旦 JHipster 安装好了，下一步就可以开始：[创建应用]({{ site.url }}/creating-an-app/)

### 可选安装

1.  安装 Java 编译工具。
    *   无论你使用的是 [Maven](http://maven.apache.org/) 还是 [Gradle](http://www.gradle.org/)，你都不需要安装任何额外的工具，因为 JHipster 自动安装了 [Maven Wrapper](https://github.com/takari/maven-wrapper) 或 [Gradle Wrapper](http://gradle.org/docs/current/userguide/gradle_wrapper.html)。
    *   如果你不希望使用这些 wrapper，可以去官网下载独立安装 [Maven](http://maven.apache.org/) 或 [Gradle](http://www.gradle.org/)。
2.  安装 Git [git-scm.com](http://git-scm.com/)。如果你是 Git 新手，我们也推荐你使用工具 [SourceTree](http://www.sourcetreeapp.com/)。
    * JHipster 会尝试提交你的项目到 Git。
    * [JHipster 升级工具]({{ site.url }}/upgrading-an-application/) 需要用到 Git。

### 问题

说明：如果你在使用 Yarn 的时候遇到问题，务必确保你的环境变量 path 里有 `$HOME/.config/yarn/global/node_modules/.bin` 这个目录。

在 Mac 或 Linux 上：```export PATH="$PATH:`yarn global bin`:$HOME/.config/yarn/global/node_modules/.bin"```

JHipster 使用 [Yeoman](http://yeoman.io/) 来作为代码生成器。
需要更多关于 Yeoman 的信息、帮助、使用技巧，请看一下 [the Yeoman "getting starting" guide](http://yeoman.io/learning/index.html)，在 [提交 Bug](https://github.com/jhipster/generator-jhipster/issues?state=open) 也请阅读 [Yarn documentation](https://yarnpkg.com/)。

## 使用 NPM 本地安装 (类似 Yarn)

安装方式和使用 Yarn 安装基本一致，除了这两处：

1.  第三步不需要安装 yarn，而是升级 NPM: `npm install -g npm`
2.  把所有 `yarn global add` 改成 `npm install -g`，例如：
    * 安装 Yeoman：`npm install -g yo`
    * 安装 JHipster：`npm install -g generator-jhipster`

请查阅 [NPM 文档](https://docs.npmjs.com/).

## 使用包管理器安装

__请注意目前是 BETA 功能！__ 如果你选择这种安装方式，请给我们您的 [bug report](https://github.com/jhipster/generator-jhipster/issues) 或反馈 [@java_hipster](https://twitter.com/java_hipster)。

### 在 Mac OS X 上用 Homebrew 安装

JHipster 提供了一个 [Homebrew](https://brew.sh/) 包，在 [http://formulae.brew.sh/formula/jhipster](http://formulae.brew.sh/formula/jhipster)。

安装 JHipster (以及 Node 和 Yarn)，键入：

    brew install jhipster

这个包的新版本会在每一个 JHipster 发布后自动创建，但是需要 Homebrew 团队一点时间来验证 - 所以如果你拿到的一个较老的 JHipster 版本，请耐心或者使用上面的 Yarn 来安装。

### Installation with Chocolatey on Windows

JHipster 提供了一个 [Chocolatey](https://chocolatey.org/) 包，在 [https://chocolatey.org/packages/jhipster](https://chocolatey.org/packages/jhipster)。

安装 JHipster (以及 Node，Yarn，Yeoman，Java，Git)，键入：

    choco install jhipster

这个包的新版本会在每一个 JHipster 发布后自动创建，但是需要 Chocolatey 团队一点时间来验证 - 所以如果你拿到的一个较老的 JHipster 版本，请耐心或者使用上面的 Yarn 来安装。

## Vagrant box 安装

[JHipster development box](https://github.com/jhipster/jhipster-devbox) 提供了一个完整的开发 JHipster 工具的虚拟机环境。

这是个设置和运行 JHipstert 的便捷方法。

除了 JHipster，这个虚拟机中还包含了许多其他开发工具，诸如 Docker，所以可以说是开发工作环境已万事俱备。

请查看文档 [JHipster development box page](https://github.com/jhipster/jhipster-devbox) 获取安装和设置信息。

## Docker 安装 (限高级用户使用)

_请注意：这里描述的 Docker 镜像是指在容器中运行 JHipster 的 generator 环境。这和 [Docker 和 Docker Compose]({{ site.url }}/docker-compose/) 章节中描述的不是一个含义，那是指在容器中运行你的应用_

### 信息

JHipster 定义了一个 [Dockerfile](https://github.com/jhipster/generator-jhipster/blob/master/Dockerfile)，由此提供 [Docker](https://www.docker.io/) 镜像。

它提供了一个自动编译好的镜像在：[https://hub.docker.com/r/jhipster/jhipster/](https://hub.docker.com/r/jhipster/jhipster/) 上。

该镜像将让你能在容器中运行 JHipster。

### 前提

根据你的操作系统镜像不同的设置：

1.  **Linux:** Linux 是原生支持 Docker 的。你只需要根据 [Docker](https://docs.docker.com/installation/#installation) 官网上的指引安装 Docker。
2.  **Mac & Windows:** 安装 [Docker Toolbox](https://www.docker.com/docker-toolbox) 来获取 Docker。

由于 Docker 生成的文件是保存在你的共享目录中的，在你关闭 Docker 容器后它们不会被删除。如果你不希望 Docker 每次都下载 Maven 或 NPM 依赖文件，你可以提交他们的状态或者挂载一个卷。

<div class="alert alert-warning"><i>警告：</i>

根据你的操作系统，<code>DOCKER_HOST</code> 参数是不一样的。在 Linux 上，这就是 localhost。
在 Mac/Windows 上，你需要用命令获取 IP 地址：<code>docker-machine ip default</code>

</div>

<div class="alert alert-info"><i>小贴士：</i>

Kitematic 是个非常好用图形化管理工具，集成在 Docker Toolbox 中，这会让安装更方便。

</div>

在 Linux 上，如果你的用户不是 docker 组，你需要以 root 用户来运行 `docker` 命令。所以推荐将你的用户加入到 docker 组，就可以以非 root 用户来运行 docker 命令了。参考这里的步骤：[http://askubuntu.com/a/477554](http://askubuntu.com/a/477554)。

### Linux/Mac Windows 使用说明 (使用 Docker Toolbox)

#### 拉取镜像

拉取最新的 JHipster 镜像：

`docker image pull jhipster/jhipster`

拉取最新的开发版 JHipster 镜像：

`docker image pull jhipster/jhipster:master`

查看所有的版本 [这里](https://hub.docker.com/r/jhipster/jhipster/tags/)

#### 运行容器

<div class="alert alert-warning"><i>警告：</i>

如果你是在 Mac 或 Windows 上运行 Docker 的话，Docker 的进程只能访问受限的文件。Docker Machine 会尝试共享你的 /Users (OS X) 目录或者 C:\Users\&lt;username&gt; (Windows) 目录。所以你需要在这些目录下创建项目来避免一些卷挂载的问题。

</div>


在你的 home 目录中创建一个 "jhipster" 目录：

`mkdir ~/jhipster`

运行 Docker 镜像，加上下面这些选项：

*  Docker 的 "/home/jhipster/app" 目录和本地的 "~/jhipster" 目录共享
*  转发 Docker 的端口 (Java 应用：8080，BrowserSync：9000, BrowserSync UI：3001)

`docker container run --name jhipster -v ~/jhipster:/home/jhipster/app -v ~/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster`

<div class="alert alert-info"><i>提示：</i>

如果曾经已经启动过容器了，不需要运行上面的命令，只需要重新 start/stop 容器。

</div>

#### 检查容器是否正常运行

检查容器是否正常运行：`docker container ps`:

    CONTAINER ID    IMAGE               COMMAND                 CREATED         STATUS          PORTS                                                       NAMES
    4ae16c0539a3    jhipster/jhipster   "tail -f /home/jhipst"  4 seconds ago   Up 3 seconds    0.0.0.0:9000-3001->9000-3001/tcp, 0.0.0.0:8080->8080/tcp    jhipster

#### 一些常规操作

*   停止容器：`docker container stop jhipster`
*   启动容器：`docker container start jhipster`

如果需要更新 Docker 镜像 (重新编译的或者从 Docker hub 重新拉取的)，最好将现有容器移除，再重新运行。先停止容器，删除，然后重新运行：

1.  `docker container stop jhipster`
2.  `docker container rm jhipster`
3.  `docker image pull jhipster/jhipster`
4.  `docker container run --name jhipster -v ~/jhipster:/home/jhipster/app -v ~/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster`

### 访问容器

<div class="alert alert-warning"><i>警告：</i>

在 Windows 上，需要以 Administrator 管理员来执行 Docker Quick Terminal，以便在 `yarn install` 步骤时创建 symlinks。

</div>

最简单的进入容器的命令是：

`docker container exec -it <container_name> bash`

复制粘贴的请注意将上面的容器名改为 `jhipster`：

`docker container exec -it jhipster bash`

你会以 "jhipster" 用户登入。

如果你希望以 "root" 用户登入，由于 `sudo` 命令在 Ubuntu Xenial 是不可用的，你需要这样：

`docker container exec -it --user root jhipster bash`

### 第一个项目

进入容器的 /home/jhipster/app 目录，开始构建你的第一个项目：

`cd /home/jhipster/app`

`jhipster`

<div class="alert alert-info"><i>提示：</i>

如果使用 Yarn 有些问题，你可以运行 <code>jhipster --npm</code> 来使用 NPM 而不是 Yarn。

</div>

一旦你的应用创建好了，你就可以正常运行 gulp/bower/maven 等相关命令，比如：

`./mvnw`

**恭喜你! 你已经成功在 Docker 里运行了一个 JHipster 应用！**

在你的主机上，你可以：

*   在 `http://DOCKER_HOST:8080` 地址上访问你的应用
*   在共享文件夹中获取项目的所有文件

<div class="alert alert-warning"><i>警告：</i>
    默认情况下，在 <code>jhipster/jhipster</code> 镜像内是没有安装 Docker 的。
    <br/>
    所以你不能：
    <ul>
        <li>运行 docker-compose</li>
        <li>创建 Docker 镜像 (Maven 的 goal: <code>dockerfile:build</code> 或 Gradle task: <code>buildDocker</code>)</li>
    </ul>
</div>
