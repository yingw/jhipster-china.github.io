---
layout: default
title: JHipster Registry
permalink: /jhipster-registry/
sitemap:
    priority: 0.7
    lastmod: 2017-05-03T00:00:00-00:00
---

# <i class="fa fa-dashboard"></i> JHipster Registry

## 概述

JHipster Registry 是一个运行时应用，由 JHipster 团队开发。就像 JHipster，它也是开源的，基于 Apache 2 开原协议，它的代码在 GitHub 上：[jhipster/jhipster-registry](https://github.com/jhipster/jhipster-registry).

JHipster Registry 的三个主要目标：

- 作为 [Eureka 服务](https://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html)，作为应用的服务发现服务。这是 JHipster 处理路由、负载均衡、以及弹性伸缩的功能。
- 作为 [Spring Cloud 配置中心](https://cloud.spring.io/spring-cloud-config/spring-cloud-config.html)，这提供所有应用以运行时配置的能力。
- 作为管理中心，具备一个仪表板来监控应用。

所有这些功能，都打包在同一个应用中，并具备一个潮流的 Angular 用户界面。

![]({{ site.url }}/images/jhipster-registry-animation.gif)

## 摘要

1. [安装](#installation)
2. [使用 Eureka 服务发现](#eureka)
3. [使用 Spring Cloud Config 配置应用](#spring-cloud-config)
4. [管理中心](#dashboards)
5. [JHipster Registry 的安全](#security)

## <a name="installation"></a> 安装

### Spring profiles

JHipster Registry 使用常规的 `dev` 和 `prod` profile，以及标准的 Spring Cloud `composite` profile。（参考 [官方文档](https://cloud.spring.io/spring-cloud-config/multi/multi__spring_cloud_config_server.html#composite-environment-repositories)）。

由此：

- 使用 `dev` profile 将会以 `dev` 和 `composite` profile 运行 JHipster Registry。`dev` 将会从文件系统加载 Spring Cloud 配置，查找 `central-config` 目录，这是运行目录的相对目录，定义在 `src/main/resources/config/bootstrap.yml` 文件里。
- 使用 `prod` profile 将会以 `prod` 和 `composite` profile 运行 JHipster Registry。`prod` 将会从 Git 仓库里加载 Spring Cloud 配置，默认值是 [https://github.com/jhipster/jhipster-registry-sample-config](https://github.com/jhipster/jhipster-registry-sample-config)。根据真实情况修改这个仓库的地址，配置 `src/main/resources/config/bootstrap-prod.yml` 文件，或 `spring.cloud.config.server.composite` 参数。

一旦 JHipster Registry 运行起来了，查看菜单 `Configuration > Cloud Config` 下的配置项。请注意如果你无法登入，有可能是你配置里的 JWT 秘钥没有正确设置。

### 使用打好的 WAR 包

JHipster Registry 发布为一个可运行的 WAR 包，提供在我们的网页上：[发布网页](https://github.com/jhipster/jhipster-registry/releases).

下载 WAR 文件，已普通 JHipster 应用的方式启动，设置你想要使用的 profile (看上面有关 profile 的章节)。例如，使用在 `central-config` 目录下配置的方式运行：

    ./jhipster-registry-<version>.war --spring.security.user.password=admin --jhipster.security.authentication.jwt.secret=my-secret-key-which-should-be-changed-in-production-and-be-base64-encoded --spring.cloud.config.server.composite.0.type=native --spring.cloud.config.server.composite.0.search-locations=file:./central-config
（译注：windows 下不能这样运行，windows 环境应该是：`java -jar jhipster-registry-<version>.war`）

请注意这非常重要：要在启动时设置一个 JWT 秘钥，在 `JHIPSTER_SECURITY_AUTHENTICATION_JWT_SECRET` 环境变量上设置，或者类似上面例子里在启动命令里设置参数。还有一个可以设置的地方是 `application.yml` 文件，在配置中心源里（在启动时都会被应用加载上）。

请注意从 JHipster 5.3.0 开始，我们设置了一个新的 `jhipster.security.authentication.jwt.base64-secret` 属性，这个更安全，但你可能还在用老版本。
我们在这个文档中使用 `jhipster.security.authentication.jwt.secret` 。更多有关这些属性的说明参考文档 [安全]({{ site.url }}/security/).

类似的，还可以运行 `prod` profile，设置参数，如：

    ./jhipster-registry-<version>.war --spring.profiles.active=prod --spring.security.user.password=admin --jhipster.security.authentication.jwt.secret=my-secret-key-which-should-be-changed-in-production-and-be-base64-encoded --spring.cloud.config.server.composite.0.type=git --spring.cloud.config.server.composite.0.uri=https://github.com/jhipster/jhipster-registry-sample-config

    ./jhipster-registry-<version>.war --spring.profiles.active=prod --spring.security.user.password=admin --jhipster.security.authentication.jwt.secret=my-secret-key-which-should-be-changed-in-production-and-be-base64-encoded --spring.cloud.config.server.composite.0.type=git --spring.cloud.config.server.composite.0.uri=https://github.com/jhipster/jhipster-registry --spring.cloud.config.server.composite.0.search-paths=central-config

### 从源码构建

JHipster Registry 可以从 [jhipster/jhipster-registry](https://github.com/jhipster/jhipster-registry) 仓库 克隆/fork/下载。因为 JHipster Registry 也是一个用 JHipster 生成的应用，你也可以像运行其他 JHipster 应用一样运行：

- 运行开发模式 `./mvnw` (启动 Java server) 并且 `yarn start` (启动前端)，将会使用默认的 `dev` profile，访问 [http://127.0.0.1:8761/](http://127.0.0.1:8761/).
- 使用 `./mvnw -Pprod package` 来打生产环境包，并且是可执行的 WAR 包。然后你可以运行 WAR 包并使用 `dev` 或 `prod` profile，如：`./jhipster-registry-<version>.war --spring.profiles.active=prod`

请注意使用 `dev` 及 `composite` profile 时，你还需要设置一个 `central-config` 目录， 所以你运行 `./jhipster-registry-<version>.war --spring.profiles.active=dev` 时，需要设置那个目录.

### 使用 Docker

如果你希望使用 Docker 来运行 JHipster Registry，可以从 Docker Hub [jhipster/jhipster-registry](https://hub.docker.com/r/jhipster/jhipster-registry/) 下载。有个 docker-compose 文件来运行镜像连同在 `src/main/docker` 目录下的所有微服务：

- 执行 `docker-compose -f src/main/docker/jhipster-registry.yml up` 启动 JHipster Registry。他将运行在 Docker 主机的端口 `8761`，所以如果是在你的电脑上运行，可以访问 [http://127.0.0.1:8761/](http://127.0.0.1:8761/)。

请参阅 [Docker Compose]({{ site.url }}/docker-compose/) 文档来获取更多关于使用 Docker Compose 运行 JHipster Registry 的信息。

### 运行在云环境

将 JHipster Registry 实例部署到云里非常方便。这对于生产环境是必要的，但同时对于开发环境也是非常有用的（并不需要在你的开发工作机上运行）。

请参阅 ["生产环境中的微服务" 文档]({{ site.url }}/microservices-in-production/) 来了解更多关于如何部署 JHipster Registry 到 Cloud Foundry 或 Heroku 的信息。

## <a name="eureka"></a> 使用 Eureka 服务发现

![]({{ site.url }}/images/jhipster-registry-eureka.png)

JHipster Registry 是一个 [Netflix Eureka server](https://github.com/Netflix/eureka) 的应用，提供了应用的服务发现服务。

- 这对于微服务架构是非常有用的功能：这使得网关（Gateway）能知晓哪些微服务可用，哪些实例正在运行。
- 对于所有应用，包括巨石类应用，使得 Hazelcast 分布式缓存可以自动伸缩，参考 [Hazelcast 缓存]({{ site.url }}/using-cache/) 文档。

## <a name="spring-cloud-config"></a> 使用 Spring Cloud Config 配置应用

![]({{ site.url }}/images/jhipster-registry-spring-cloud-config.png)

JHipster Registry 同时也是一个 [Spring Config Server](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html) 应用：当应用刚启动时他们会连接 JHipster Registry 来获取他们的配置项。对于网关和微服务都是如此。

配置是一个 Spring Boot 配置文档，就如同 JHipster 的 `application-*.yml` 文件，但是是存储在中央服务器中的，方便管理。

启动时，你的网关应用和微服务应用都会查询 Registry 的配置服务器并覆盖他们本地定义的配置。

支持两种配置源（定义在 `spring.cloud.config.server.composite` 属性中）：

- `native` 配置，这在开发环境中是默认选项（使用 JHipster `dev` profile），会使用本地文件系统。
- `Git` 配置，这在生产环境中是默认选项（使用 JHipster `prod` profile），会使用存储在 Git 服务器上的配置。这种方式下还允许使用 Tag、分支或使用 Git 工具回滚配置，这在某些场合下非常有用。
- （译注：Spring Cloud Config 还支持很多配资源，传统如数据库。Git 环境如果不是作为生产环境的一部分可以考虑使用数据库方式，参考[文档](https://cloud.spring.io/spring-cloud-config/spring-cloud-config.html#_environment_repository)。）

要管理你的配置，你只需要在配置源中添加 `appname-profile.yml` 文件，这里的 **appname** 和 **profile** 就是应用的名称和当前你在配置的 profile。
例如：添加配置文件 `gateway-prod.yml` 会影响应用 **gateway** 并允许于 **prod** profile。另外，定义在 `application[-dev|prod].yml` 配置文件中的属性会影响你所有的应用。

由于网关应用的路由也是由 Spring Boot 配置的，它们也可以用 Spring Config Server 来管理，例如你可以配置应用 `app1-v1` 映射至 `/app1` URL 在你的 `v1` 分支上，同时 `app1-v2` 映射至 `/app1` URL 在 `v2` 分支。这对于升级微服务并避免停机升级很有帮助。

### <a name="encryption"></a> 使用加密配置值

JHipster Registry 有一个特殊的 `configuration > encryption` 网页来执行加密和解密配置。

要加密配置项的值（比如，数据库密码）：

- 下载 [JCE](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) 并根据下载文件中的说明安装（只在你使用的是Oracle JDK 的时候需要这一步）。
- 设置 `encrypt.key` 属性，在 `bootstrap.yml` 文件里 (注意不是 `application.yml`) 或者设置 `ENCRYPT_KEY` 环境变量 with your symmetric key passphrase.

所有都设置正确的话，你应该可以访问 `Configuration > Encryption` 页，并能提交 POST 请求到 `/config/encrypt` 和 `/config/decrypt` 并带上你希望加密或解密的文本，放在请求的 `body` 中。

如： `curl localhost:8761/config/encrypt -d mypassword` （译注：应该还有个 -X POST）

加密的文本要放在 `*.yml` 配置文件里，格式为 `password= '{cipher}myciphertextafterencryotion'` 并在配置服务发给客户端前会被解密。这样你的配置文件（存在 Git 或 "natively" 在文件系统上）就不会有未加密文本了。

查看更多说明，访问 Spring Cloud Config 的文档 [加密和解密](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html#_encryption_and_decryption).

## <a name="dashboards"></a> 管理中心

JHipster Registry 提供了一个管理面板，适用于所有类型的应用。当应用在 Eureka 上注册成功后，就可以在管理面板里看到了。

为访问应用的敏感信息，JHipster Registry 使用了一个 JWT token (这也是为什么 JHipster Registry 只能工作与使用 JWT 应用的原因)。 用于应用和 JHipster Registry 的 JWT key 应该是相同的：由于 JHipster Registry 默认使用 Spring Cloud Config，它会发送统一的 key 给所有应用，应该是原生可用。

### 度量面板

![]({{ site.url }}/images/jhipster-registry-metrics.png)

度量面板使用了 Dropwizard 的度量共来提供一个详细的应用性能描述。

提供了关于：

- JVM
- HTTP 请求
- Spring Beans 里的方法 (使用 `@Timed` 注解)
- 数据库连接池

点击 JVM 线程旁边的眼睛图标，你可以查看运行中应用的堆栈记录，这对于找线程问题很有帮助。

### 健康度面板

![]({{ site.url }}/images/jhipster-registry-health.png)

健康度使用 Spring Boot Actuator 的 health 管理端点来提供应用的多方面健康信息。很多 Spring Boot Actuator 提供的健康度检查都是开箱即用的，并且也非常易于扩展添加新的健康检查。

### 配置面板

![]({{ site.url }}/images/jhipster-registry-configuration.png)

配置面板使用 Spring Boot Actuator 的 configuration 管理端点来获得当前应用的所有配置。

### 日志面板

![]({{ site.url }}/images/jhipster-registry-logs.png)

日志面板可以用来管理运行时的 Logback 配置。修改 Java 包的日志等级只需要简单地点击一个按钮，这在开发和生产环境中都是十分有帮助的。

## <a name="security"></a> JHipster Registry 的安全

JHipster Registry 是默认需要认证的。可以使用默认的 "admin/admin" 账号登入。

应用也使用这个 "admin" 用户连接 JHipster Registry，但是是通过 HTTP 基础认证。所以如果你的微服务连不上 registry，并看到类如 "401 authentication error" 错误消息，说明应用里的设置有问题。

为了更安全地使用 JHipster Registry：

- 你必须修改默认的 "admin" 密码。该密码设置在 Spring Boot 标准属性 `spring.security.user.password`，你可以使用常用的 Spring Boot 方式去修改之：修改项目的 `application-*.yml` 文件，或添加 `SPRING_SECURITY_USER_PASSWORD` 环境变量。[Docker Compose sub-generator]({{ site.url }}/docker-compose/) 只能使用环境变量方式。
- 应用使用 HTTP 协议连接 Registry，加密连接通道非常重要。有很多方式，比较简单是使用 HTTPS。
