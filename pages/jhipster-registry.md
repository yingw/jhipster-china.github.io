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

If you'd rather run the JHipster Registry from a Docker image, it is available on Docker Hub at [jhipster/jhipster-registry](https://hub.docker.com/r/jhipster/jhipster-registry/). A docker-compose file to easily run this image is already present within each microservice `src/main/docker` directory:

- run `docker-compose -f src/main/docker/jhipster-registry.yml up` to start the JHipster Registry. It will be available on port `8761` of your Docker host, so if it runs on your machine it should be at [http://127.0.0.1:8761/](http://127.0.0.1:8761/).

Please read our [Docker Compose documentation]({{ site.url }}/docker-compose/) for more information on using the JHipster Registry with Docker Compose.

### 运行在云环境

It's very easy to host a JHipster Registry instance in the cloud. This is mandatory in production, but this can also be useful in development (there is no need to run it on your laptop).

Please read [the "microservices in production" documentation]({{ site.url }}/microservices-in-production/) to learn how to deploy the JHipster Registry to Cloud Foundry or to Heroku.

## <a name="eureka"></a> 使用 Eureka 服务发现

![]({{ site.url }}/images/jhipster-registry-eureka.png)

The JHipster Registry is a [Netflix Eureka server](https://github.com/Netflix/eureka), that provides service discovery for all applications.

- This is very useful for microservices architectures: this is how the gateways know which microservices are available, and which instances are up
- For all applications, including monoliths, this is how the Hazelcast distributed cache can automatically scale, see [the Hazelcast cache documentation]({{ site.url }}/using-cache/)

## <a name="spring-cloud-config"></a> 使用 Spring Cloud Config 配置应用

![]({{ site.url }}/images/jhipster-registry-spring-cloud-config.png)

The JHipster Registry is a [Spring Config Server](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html): when applications are launched they will first connect to the JHipster Registry to get their configuration. This is true for both gateways and microservices.

This configuration is a Spring Boot configuration, like the one found in the JHipster `application-*.yml` files, but it is stored in a central server, so it is easier to manage.

On startup, your gateways and microservices app will query the Registry's config server and overwrite their local properties with the ones defined there.

Two kinds of configurations sources are available (defined by the `spring.cloud.config.server.composite` property):

- A `native` configuration, which is used by default in development (using the JHipster `dev` profile), and which uses the local filesystem.
- A `Git` configuration, which is used by default in production (using the JHipster `prod` profile), and which stores the configuration in a Git server. This allows to tag, branch or rollback configurations using the usual Git tools, which are very powerful in this use-case.

To manage your centralized configuration you just need to add `appname-profile.yml` files in your configuration source where **appname** and **profile** correspond to the application's name and current profile of the service that you want to configure.
For example, adding properties in a `gateway-prod.yml` file will set those properties only for the application named **gateway** started with a **prod** profile. Moreover, properties defined in `application[-dev|prod].yml` will be set for all your applications.

As the Gateway routes are configured using Spring Boot, they can also be managed using the Spring Config Server, for example you could map application `app1-v1` to the `/app1` URL in your `v1` branch, and map application `app1-v2` to the `/app1` URL in your `v2` branch. This is a good way of upgrading microservices without any downtime for end-users.

### <a name="encryption"></a> 使用加密配置值

JHipster Registry 有一个特殊的 `configuration > encryption` 网页来执行加密和解密配置。

To encrypt configuration values (for example, database passwords) you need to:

- 下载 [JCE](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) and install it by following the instructions in the downloaded files (this is only required if you are using the Oracle JDK).
- 设置 `encrypt.key` 属性，在 `bootstrap.yml` 文件里 (注意不是 `application.yml`) 或者设置 `ENCRYPT_KEY` 环境变量 with your symmetric key passphrase.

If everything is setup correctly, you should be able to use the specific `Configuration > Encryption` page, and also send POST requests to `/config/encrypt` and `/config/decrypt` endpoints with the text you want to manipulate in the `body` of the requests.

For example: `curl localhost:8761/config/encrypt -d mypassword`

The cipher text must be placed in any `*.yml` configuration file, in the form `password= '{cipher}myciphertextafterencryotion'` and it will be decrypted by the config server before sending it to its clients. This way your configuration files (stored in Git or stored "natively" on your filesystem) do not have plain text values.

For more information, please refer to Spring Cloud Config's [Encryption and Decryption documentation](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html#_encryption_and_decryption).

## <a name="dashboards"></a> 管理中心

The JHipster Registry provides administration dashboards, which are used for all application types. As soon as an application registers on the Eureka server, it will become available in the dashboards.

In order to access sensitive information from the applications, the JHipster Registry will use a JWT token (this is why the JHipster Registry only works for applications using JWT). The JWT key used to sign the request should be the same for the applications and the JHipster Registry: as by default the JHipster Registry configures applications through Spring Cloud Config, this should work out-of-the-box, as it will send the same key to all applications.

### The metrics dashboard

![]({{ site.url }}/images/jhipster-registry-metrics.png)

The metrics dashboard uses Dropwizard metrics to give a detailed view of the application performance.

It gives metrics on:

- the JVM
- HTTP requests
- methods used in Spring Beans (using the `@Timed` annotation)
- database connection pool

By clicking on the eye next to the JVM thread metrics, you will get a stacktrace of the running application, which is very useful to find out blocked threads.

### The health dashboard

![]({{ site.url }}/images/jhipster-registry-health.png)

The health dashboard uses Spring Boot Actuator's health endpoint to give health information on various parts of the application. Many health checks are provided out-of-the-box by Spring Boot Actuator, and it's also very easy to add application-specific health checks.

### The configuration dashboard

![]({{ site.url }}/images/jhipster-registry-configuration.png)

The configuration dashboard uses Spring Boot Actuator's configuration endpoint to give a full view of the Spring configuration of the current application.

### The logs dashboard

![]({{ site.url }}/images/jhipster-registry-logs.png)

The logs dashboard allows to manage at runtime the Logback configuration of the running application. Changing the log level of a Java package is as simple as clicking on a button, which is very convenient both in development and in production.

## <a name="security"></a> JHipster Registry 的安全

The JHipster Registry is secured by default. You can login using the usual "admin/admin" login and password that are used in normal JHipster applications.

Applications also connect to the JHipster Registry using that same "admin" user, but use HTTP Basic authentication. So if your microservices cannot access the registry, and you see some "401 authentication error" messages, it is because you have misconfigured those applications.

In order to secure your JHipster Registry:

- You must change the default "admin" password. This password is set using the standard Spring Boot property `spring.security.user.password`, so you can use the usual Spring Boot mechanisms to modify it: you could modify the project's `application-*.yml` files, or add a `SPRING_SECURITY_USER_PASSWORD` environment variable. The [Docker Compose sub-generator]({{ site.url }}/docker-compose/) uses the environment variable method.
- As your applications will connect to the registry using HTTP, it is very important to secure that connection channel. There are many ways to do it, and the easiest one is probably to use HTTPS.
