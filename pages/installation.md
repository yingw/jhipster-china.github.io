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
*   "Installation with a package manager" is only available for Mac OS X and Windows. This is a very simple installation method, if you use a package manager, but it is still in BETA.
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
2.  安装 Node.js [Node.js 官网](http://nodejs.org/) (建议选择 LTS 版本)
3.  安装 Yarn [the Yarn 官网](https://yarnpkg.com/en/docs/install)
4.  如果你打算使用 JHipster Marketplace, 安装 Yeoman: `yarn global add yo`
5.  安装 JHipster: `yarn global add generator-jhipster`

一旦 JHipster 安装好了，下一步就可以开始：[创建应用]({{ site.url }}/creating-an-app/)

### 使用 AngularJS 1.x 的设置

1.  安装 Java 8 [Oracle 官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2.  安装 Node.js [Node.js 官网](http://nodejs.org/) (建议选择 LTS 版本)
3.  安装 Yarn [the Yarn 官网](https://yarnpkg.com/en/docs/install)
4.  安装 Bower: `yarn global add bower`
5.  安装 Gulp: `yarn global add gulp-cli`
6.  如果你打算使用 JHipster Marketplace, 安装 Yeoman: `yarn global add yo`
7.  安装 JHipster: `yarn global add generator-jhipster`

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

## Installation with a package manager

__Please note this is a BETA feature!__ If you selected this installation, don't hesitate to send us a [bug report](https://github.com/jhipster/generator-jhipster/issues) or feedback on [@java_hipster](https://twitter.com/java_hipster).

### Installation with Homebrew on Mac OS X

JHipster provides a [Homebrew](https://brew.sh/) package, available on [http://formulae.brew.sh/formula/jhipster](http://formulae.brew.sh/formula/jhipster).

To install JHipster (as well as Node and Yarn), just type:

    brew install jhipster

New versions of this package are published each time a new JHipster release is created, but it might take time for the Homebrew team to validate this package - so if you have an older JHipster release, please be patient or use the Yarn installation above.

### Installation with Chocolatey on Windows

JHipster provides a [Chocolatey](https://chocolatey.org/) package, available on [https://chocolatey.org/packages/jhipster](https://chocolatey.org/packages/jhipster).

To install JHipster (as well as Node, Yarn, Yeoman, Java and Git), just type:

    choco install jhipster

New versions of this package are published each time a new JHipster release is created, but it might take time for the Chocolatey team to validate this package - so if you have an older JHipster release, please be patient or use the Yarn installation above.

## Vagrant box 安装

The [JHipster development box](https://github.com/jhipster/jhipster-devbox) project gives you a virtual machine with all the necessary tools to develop your JHipster project.

It's an easy way to get up and running very quickly with JHipster.

Besides JHipster, this virtual machine includes many development tools, as well as Docker, so you should have everything ready for working.

Please go to the [JHipster development box page](https://github.com/jhipster/jhipster-devbox) for installation and configuration information.

## Docker 安装 (限高级用户使用)

_Please note: this Docker image is for running the JHipster generator inside a container. It's completely different from the [Docker and Docker Compose configurations]({{ site.url }}/docker-compose/) that JHipster will generate, which goal is to run your generated application inside a container_

### 信息

JHipster has a specific [Dockerfile](https://github.com/jhipster/generator-jhipster/blob/master/Dockerfile), which provides a [Docker](https://www.docker.io/) image.

It makes a Docker "Automated build" that is available on: [https://hub.docker.com/r/jhipster/jhipster/](https://hub.docker.com/r/jhipster/jhipster/)

This image will allow you to run JHipster inside Docker.

### 前提

This depends on your operating system.

1.  **Linux:** Linux supports Docker out-of-box. You just need to follow the tutorial on the [Docker](https://docs.docker.com/installation/#installation) website.
2.  **Mac & Windows:** install the [Docker Toolbox](https://www.docker.com/docker-toolbox) to get Docker installed easily.

As the generated files are in your shared folder, they will not be deleted if you stop your Docker container. However, if you don't want Docker to keep downloading all the Maven and NPM dependencies every time you start the container, you should commit its state or mount a volume.

<div class="alert alert-warning"><i>Warning: </i>

Based on your OS, your <code>DOCKER_HOST</code> will differ. On Linux, it will be simply your localhost.
For Mac/Windows, you will have to obtain the IP using following command: <code>docker-machine ip default</code>

</div>

<div class="alert alert-info"><i>Tip: </i>

Kitematic is an easy-to-use graphical interface provided with the Docker Toolbox, which will makes this installation a lot easier.

</div>

On Linux, you might need to run the `docker` command as root user if your user is not part of docker group. It's a good idea to add your user to docker group so that you can run docker commands as a non-root user. Follow the steps on [http://askubuntu.com/a/477554](http://askubuntu.com/a/477554) to do so.

### Linux/Mac Windows 使用说明 (使用 Docker Toolbox)

#### 拉取镜像

Pull the latest JHipster Docker image:

`docker image pull jhipster/jhipster`

Pull the development JHipster Docker image:

`docker image pull jhipster/jhipster:master`

You can see all tags [here](https://hub.docker.com/r/jhipster/jhipster/tags/)

#### 运行容器

<div class="alert alert-warning"><i>Warning: </i>

If you are using Docker Machine on Mac or Windows, your Docker daemon has only limited access to your OS X or Windows file system. Docker Machine tries to auto-share your /Users (OS X) or C:\Users\&lt;username&gt; (Windows) directory. So you have to create the project folder under these directory to avoid any volume mounting issues.

</div>


Create a "jhipster" folder in your home directory:

`mkdir ~/jhipster`

Run the Docker image, with the following options:

*   The Docker "/home/jhipster/app" folder is shared to the local "~/jhipster" folder
*   Forward all ports exposed by Docker (8080 for the Java application, 9000 for BrowserSync, 3001 for the BrowserSync UI)

`docker container run --name jhipster -v ~/jhipster:/home/jhipster/app -v ~/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster`

<div class="alert alert-info"><i>Tip: </i>

If you have already started the container once before, you do not need to run the above command, you can simply start/stop the existing container.

</div>

#### 检查容器是否正常运行

To check that your container is running, use the command `docker container ps`:

    CONTAINER ID    IMAGE               COMMAND                 CREATED         STATUS          PORTS                                                       NAMES
    4ae16c0539a3    jhipster/jhipster   "tail -f /home/jhipst"  4 seconds ago   Up 3 seconds    0.0.0.0:9000-3001->9000-3001/tcp, 0.0.0.0:8080->8080/tcp    jhipster

#### 一些常规操作

*   To stop the container execute: `docker container stop jhipster`
*   And to start again, execute: `docker container start jhipster`

In case you update the Docker image (rebuild or pull from the Docker hub), it's better to remove the existing container, and run the container all over again. To do so, first stop the container, remove it and then run it again:

1.  `docker container stop jhipster`
2.  `docker container rm jhipster`
3.  `docker image pull jhipster/jhipster`
4.  `docker container run --name jhipster -v ~/jhipster:/home/jhipster/app -v ~/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster`

### 访问容器

<div class="alert alert-warning"><i>Warning: </i>

On Windows, you need to execute the Docker Quick Terminal as Administrator to be able to create symlinks during the `yarn install` step.

</div>

The easiest way to log into the running container is by executing following command:

`docker container exec -it <container_name> bash`

If you copy-pasted the above command to run the container, notice that you have to specify `jhipster` as the container name:

`docker container exec -it jhipster bash`

You will log in as the "jhipster" user.

If you want to log in as "root", as the `sudo` command isn't available in Ubuntu Xenial, you need to run:

`docker container exec -it --user root jhipster bash`

### 第一个项目

You can then go to the /home/jhipster/app directory in your container, and start building your app inside Docker:

`cd /home/jhipster/app`

`jhipster`

<div class="alert alert-info"><i>Tip: </i>

If you are having issues with Yarn, you can use <code>jhipster --npm</code>, to use NPM instead of Yarn.

</div>

Once your application is created, you can run all the normal gulp/bower/maven commands, for example:

`./mvnw`

**恭喜你! 你已经成功在 Docker 里运行了一个 JHipster 应用！**

On your host machine, you should be able to :

*   Access the running application at `http://DOCKER_HOST:8080`
*   Get all the generated files inside your shared folder

<div class="alert alert-warning"><i>Warning: </i>
    By default, Docker is not installed inside the <code>jhipster/jhipster</code> image.
    <br/>
    So you won't be able to:
    <ul>
        <li>use the docker-compose files</li>
        <li>build a Docker image (Maven goal: <code>dockerfile:build</code> or Gradle task: <code>buildDocker</code>)</li>
    </ul>
</div>
