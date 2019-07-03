---
layout: default
title: Profiles
permalink: /profiles/
redirect_from:
  - /profiles.html
sitemap:
    priority: 0.7
    lastmod: 2014-11-26T00:00:00-00:00
---

# <i class="fa fa-group"></i> Profiles

JHipster 配以两个 [Spring profiles](http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html) :

*   `dev` 开发环境: 关注与开发方便和高效
*   `prod` 生产环境: 关注于高性能和可扩展性

这些 profile 以两种形式存在：

*   Maven/Gradle 的 profile 在编译时使用。例如 `./mvnw -Pprod package` 或 `./gradlew bootWar -Pprod` 将打生产环境包。
*   Spring 的 profile 在运行时使用。一些 Spring 的 Bean 在不同 profile 环境中表现不太一样。

Spring 的 profile 也由 Maven/Gradle 设置，所以我们具备一套一致的设置方式，你会得到一个生产环境 `prod` 的 profile 同时在 Maven/Gradle 和 Spring 上。

_注意:_ Spring 的 profile 用来设置 JHipster 应用的属性（propertie），感兴趣的话可以阅读 [应用属性配置说明]({{ site.url }}/common-application-properties/).

## 默认 JHipster 使用 `dev` profile

如果你不用 Maven/Gradle 启动应用，启动 "Application" 类 (在 IDE 中右键类来运行)。

如果使用 Maven 启动，运行 `./mvnw` 来使用 Maven Wrapper, 或者 `mvn` 使用已安装的 Maven。

如果使用 Gradle 启动应用，运行 `./gradlew` 来使用 Gradle Wrapper, 或者 `gradle` 使用已安装的 Gradle。

当使用 Angular 2+，如果希望运行一次完整的 webpack 编译 `dev` profile，你可以传入 `webpack` 参数：

  `./mvnw -Pdev,webpack`
  或
  `./gradlew -Pdev -Pwebpack`

## 在生产环境，JHipster 必须使用 `prod` profile

你可以在生产环境中用 Maven 或 Gradle 来运行 JHipster 应用：

*   使用 Maven, 运行 `./mvnw -Pprod` (或 `mvn -Pprod`)
*   使用 Gradle, 运行 `./gradlew -Pprod` (或 `gradle -Pprod`)

如果希望打包为可执行 WAR 文件，可以在 package 时传 profile 给 Maven 或 Gradle。例如：

*   使用 Maven, 运行 `./mvnw -Pprod package` (或 `mvn -Pprod package`)
*   使用 Gradle, 运行 `./gradlew -Pprod bootWar` (或 `gradle -Pprod bootWar`)

当以 WAR 文件运行生产环境应用，默认既使用打包时指定的 profile。如果你希望复写这个 profile，可以在 VM 参数上提供明确的声明：

*   `./java -jar jhipster-0.0.1-SNAPSHOT.war --spring.profiles.active=...`

## 其他 Spring profiles

JHipster 还有三个可选的 profiles:

*   `swagger` 来启用 swagger
*   `no-liquibase` 来禁用 liquibase
*   `tls` 开启用 TLS 安全并使用 HTTP/2 协议 (参考 [TLS 和 HTTP/2 文档]({{ site.url }}/tls/))

这些可以和 `dev` 以及 `prod` profile 一起使用。注意默认 `swagger` profile 在 `prod` 下禁用了，在 `dev` 下启用，通过 `application.yml` 设置 `spring.profiles.include` 属性来设置的。

`swagger`, `no-liquibase`, `tls` 只作用于运行时：

*   在你的 IDE 里，运行应用主类加上 `spring.profiles.active=dev,no-liquibase` (注意你需要显式地加上 `dev` 或 `prod` profile 声明)
*   使用打包的应用时：`./java -jar jhipster-0.0.1-SNAPSHOT.war --spring.profiles.active=prod,no-liquibase`

使用 Maven，你还可以直接调用这些 profile:

*   `./mvnw -Pprod,swagger,no-liquibase`
*   `./mvnw -Pdev,no-liquibase`

使用 Gradle，你也可以直接调用这些 profile:

*   `./gradlew -Pprod -Pswagger -Pno-liquibase`
*   `./gradlew -Pno-liquibase`
