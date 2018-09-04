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

You can run JHipster in production directly using Maven or Gradle:

*   With Maven, run `./mvnw -Pprod` (or `mvn -Pprod`)
*   With Gradle, run `./gradlew -Pprod` (or `gradle -Pprod`)

如果希望打包为可执行 WAR 文件，可以在 package 时传 profile 给 Maven or Gradle。例如：

*   With Maven, run `./mvnw -Pprod package` (or `mvn -Pprod package`)
*   With Gradle, run `./gradlew -Pprod bootWar` (or `gradle -Pprod bootWar`)

当以 WAR 文件运行生产环境应用，默认既使用打包时指定的 profile。如果你希望复写这个 profile，可以在 VM 参数上提供明确的声明：

*   `./java -jar jhipster-0.0.1-SNAPSHOT.war --spring.profiles.active=...`

## Spring profiles switches

JHipster comes with two additional profiles used as switches:

*   `swagger` to enable swagger
*   `no-liquibase` to disable liquibase

These can be used along with both the `dev` and `prod` profiles. Please note that by default, the `swagger` profile is disabled in `prod` and enabled in `dev` by setting the `spring.profiles.include` property in `application.yml`.

`swagger` and `no-liquibase` are only used at runtime:

*   In your IDE, run your main application class with `spring.profiles.active=dev,no-liquibase` (please note you need to include the `dev` or `prod` profile explicitly)
*   With a packaged application: `./java -jar jhipster-0.0.1-SNAPSHOT.war --spring.profiles.active=prod,no-liquibase`

With Maven, you can also use those profiles directly:

*   `./mvnw -Pprod,swagger,no-liquibase`
*   `./mvnw -Pdev,no-liquibase`

With Gradle, you can also use those profiles directly:

*   `./gradlew -Pprod -Pswagger -Pno-liquibase`
*   `./gradlew -Pno-liquibase`
