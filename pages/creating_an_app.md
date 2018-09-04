---
layout: default
title: 创建应用
permalink: /creating-an-app/
redirect_from:
  - /creating_an_app.html
sitemap:
    priority: 0.7
    lastmod: 2018-03-18T18:20:00-00:00
---

# <i class="fa fa-rocket"></i> 创建应用

_**请查看我们的 [视频教程]({{ site.url }}/video-tutorial/) 来学习创建 JHipster 应用**_

1. [快速上手](#1)
2. [创建应用的各选项](#2)
3. [使用蓝图](#5)
4. [命令行选项](#3)
5. [小提示](#4)

## <a name="1"></a> 快速上手

首先，创建一个空的目录来放你的项目：

`mkdir myapplication`

进入目录：

`cd myapplication/`

开始创建项目，输入命令：

`jhipster`

接下来生成器会问你关于项目的一些问题，来根据你的需要创建项目。这些问题将在 [下一小节](#2) 介绍。

一旦项目创建完成，你可以运行 Maven（Linux/MacOS/Windows PowerShell 上执行 `./mvnw`，在 Windows Cmd 里执行 `mvnw`），或 Gradle (在 Linux/MacOS/Windows PowerShell 上执行 `./gradlew`，在 Windows Cmd 里执行 `gradlew`)。

应用启动在： [http://localhost:8080](http://localhost:8080)

**重要** 如果你希望使用 "动态加载" JavaScript/TypeScript 代码的功能，你需要执行 `npm start` 或 `yarn start`。你可以去 [JHipster 开发的相关技术]({{ site.url }}/development/) 也了解更多信息。

## <a name="2"></a> 创建应用的各选项

_部分问题的选项会根据你之前的选择有所变化。比如你不需要设置 Hibernate 缓存如果你之前没有选择使用 SQL 数据库。_

### Which _type_ of application would you like to create? （你要创建的应用类型）

你的应用类型取决于你是否需要使用微服务架构。关于微服务的使用 [这里]({{ site.url }}/microservices-architecture/) 有完整的说明，如果不是很确定，就选择默认的 "Monolithic application"。

你可以使用这些：

*   巨石类应用（Monolithic application，也可翻译为传统类应用）: 这个是经典的、通用的应用类型。比较容易使用和开发，是我们推荐的默认类型。
*   微服务应用（Microservice application）: 在微服务架构中，这是一个服务。
*   微服务网关（Microservice gateway）: 在微服务架构中，这个是边缘服务，提供请求的路由和安全控制。
*   JHipster UAA 服务：在微服务架构中，这里提供了 OAuth2 认证，安全控制。参考 [JHipster UAA documentation]({{ site.url }}/using-uaa/)。

### What is the base name of your application? （应用名称）

你项目的名称。
（译注：只能大小写、数字，没有特殊符号等）

### What is your default Java package name? （Java 包名）

项目的根目录包设置。这个值存在 Yeoman 中，所以下一次你运行 generator 命令时该值为成为默认值。当然你也可以修改的。

### Do you want to use the JHipster Registry to configure, monitor and scale your application? （是否需要使用 JHipster Registry 来配置、监控和扩展你的应用）

[JHipster Registry]({{ site.url }}/jhipster-registry/) 是一个开源的工具，用于管理你的应用。

在使用微服务架构时是必要的 (所以在在创建巨石类应用是询问为可选的)。

### Which _type_ of authentication would you like to use? （使用哪种认证）

这个问题也依赖于之前的回答。比如说，如果你选择了 [JHipster Registry]({{ site.url }}/jhipster-registry/) ，你就只能选择 JWT 认证方式了。

一共有着这些可选项：

*   JWT authentication: 使用 [JSON Web Token (JWT)](https://jwt.io/)，这个是默认值。
*   HTTP Session Authentication: 经典的基于 session 的认证方式，好比我们在 Java 中的用法 (这是大部分用户使用使用 [Spring Security](http://docs.spring.io/spring-security/site/index.html) 的方式)。
*   OAuth 2.0 / OIDC Authentication: 这个选项使用 OpenID Connect server, 比如 [Keycloak](http://www.keycloak.org/) 或者 [Okta](https://www.okta.com)，可以在引用外部处理认证。（译注：应该还能支持 [CAS](https://www.apereo.org/projects/cas)）
*   Authentication with JHipster UAA server: 这使用的 [JHipster UAA server]({{ site.url }}/using-uaa/) 需要被独立的创建，同时也是 OAuth2 server 在应用外处理认证。

你可以到 [应用安全]({{ site.url }}/security/) 章节获取更多信息。

### Which _type_ of database would you like to use? （使用哪种数据库）

你可以选择：

- No database，不使用数据库 (只在使用 [微服务应用]({{ site.url }}/microservices-architecture/) 时可选)
- SQL 数据库 (支持 H2, MySQL, MariaDB, PostgreSQL, MSSQL, Oracle)，通过 Spring Data JPA 来访问数据库
- [MongoDB]({{ site.url }}/using-mongodb/)
- [Cassandra]({{ site.url }}/using-cassandra/)
- [Couchbase]({{ site.url }}/using-couchbase/)

### Which _production_ database would you like to use? （生产环境使用哪种数据库）

这个是你将在生产环境使用的数据库。在配置文件 `src/main/resources/config/application-prod.yml` 中配置。

如果你希望使用 Oracle，你还需要 [手动安装 Oracle JDBC 驱动]({{ site.url }}/using-oracle/).

### Which _development_ database would you like to use? （开发环境使用哪种数据库）

这个是在开发环境中使用的数据库，你可以使用：

*   H2 数据库，运行在内存中。这是运行 JHipster 最简单的方法，但是在服务重启后你所有的数据都会丢失。
*   H2 数据库，数据存储于磁盘上。比运行在内存中的方式要好些，因为你不会丢失数据了。
*   和生产环境一样的数据库：这种方式设置稍微复杂，但这样和生产一致是更好的方式。这也是在使用 liquibase-hibernate [the development guide]({{ site.url }}/development/) 时最好的方式。

（译注：推荐3，和生产库使用同样的数据库，而不是 H2，驱动不说，丢数据麻烦。只是需要设置一下数据库位置而已）

在配置文件 `src/main/resources/config/application-dev.yml` 中进行设置。

### Do you want to use the Spring cache abstraction? （是否使用 Spring 缓存）

Spring 的缓存抽象可以使用多种缓存实现：你可以用 [ehcache](http://ehcache.org/) (本地缓存), [Hazelcast](http://www.hazelcast.com/) (分布式缓存), 或者 [Infinispan](http://infinispan.org/) (另一种分布式缓存)。这能极大地提升你应用的性能表现，当然这个是可选项。

### Do you want to use Hibernate 2nd level cache? （是否使用 Hibernate 二级缓存）

该选项只有在你选择了使用 SQL 数据库后才会出现 (因为 JHipster 是使用 Spring Data JPA 来访问它的) 并选择一个上一个问题中的缓存提供者。

[Hibernate](http://hibernate.org/) 是 JHipster 使用的 JPA 提供实现，它使用缓存来极大地提升性能。我们强烈推荐你选择该项，并根据你的应用的需要调整缓存。

### Would you like to use Maven or Gradle? （使用 Maven 或 Gradle）

你可以编译你的 Java 项目使用 [Maven](http://maven.apache.org/) 或者 [Gradle](http://www.gradle.org/)。Maven 更稳定更成熟些。Gradle 更灵活、易扩展、以及酷炫些。

### Which other technologies would you like to use? （使用哪些额外的技术）

#### API first development using swagger-codegen（API 优先开发模式，使用 swagger-codegen）

该选项让你可以采用 [API优先（API-first）的开发方式]({{ site.url }}/doing-api-first-development)，集成了 [Swagger-Codegen](https://github.com/swagger-api/swagger-codegen)。

#### Search engine using ElasticSearch（搜索引擎 ElasticSearch）

[Elasticsearch](https://github.com/elastic/elasticsearch) 将会配置 Spring Data Elasticsearch。阅读 [Elasticsearch 说明]({{ site.url }}/using-elasticsearch/) 获取更多信息。

#### Clustered HTTP sessions using Hazelcast（使用 Hazelcast 来设置 Http Session 集群）

默认，JHipster 使用 HTTP session 来存储 [Spring Security](http://docs.spring.io/spring-security/site/index.html) 的认证和授权信息。当然，你可以选择在 HTTP sessions 中存放其他更多信息。
使用 HTTP sessions 将会导致在集群环境中的问题，特别是你没有使用带有 "粘滞会话（sticky sessions）" 功能的负载均衡器。
如果你想在集群中复制 session，选中这个选项来配置 [Hazelcast](http://www.hazelcast.com/)。

#### WebSockets using Spring Websocket （使用 Spring Websocket）

Websockets 由 Spring Websocket 实现。我们还提供了一个完整的例子来展示如何使用。

#### Asynchronous messages using Apache Kafka （使用 Apache Kafka 异步消息）

使用 [Apache Kafka]({{ site.url }}/using-kafka/) 作为发布/订阅的消息服务。

### Which _Framework_ would you like to use for the client? （使用哪种客户端框架）

客户端框架。

可以选择：

*   Angular
*   React

### Would you like to use the LibSass stylesheet preprocessor for your CSS? （使用 LibSass 预处理 CSS？）

[Node-sass](https://www.npmjs.com/package/node-sass) 是一个简化 CSS 的工具。还需要运行 [Gulp](http://www.gulpjs.com) 来使用它，自动配置好了。

### Would you like to enable internationalization support? （支持国际化？）

默认情况下 JHipster 提供了非常棒的国际化支持，在客户端和服务端都是。同时，国际化也是有点复杂，你可以选择不适用改特性。

### Which testing frameworks would you like to use? （使用哪些测试框架）

默认 JHipster 提供了 Java 的单元/集成测试 (使用 Spring 的 JUnit 支持) 和 JavaScript 的单元测试 (使用 Jest)。作为可选项，你还可以选择增加：

*   Gatling：性能测试
*   Cucumber：行为测试
*   Protractor：Angular 集成测试

阅读更多关于测试的信息 ["Running tests" guide]({{ site.url }}/running-tests/).

### Would you like to install other generators from the JHipster Marketplace? （是否要从 JHipster Marketplace 上下载额外的插件）

[JHipster Marketplace]({{ site.url }}/modules/marketplace/) 是一个提供了额外功能的插件库，由外部工程师编写的各种功能，来添加各种非官方的特性。

## <a name="5"></a> Using a blueprint

JHipster 5 introduces the concept of a blueprint. Blueprints are JHipster modules that can provide custome client/server side templates that will override the ones from JHipster. For example, the [Kotlin blueprint](https://github.com/jhipster/jhipster-kotlin) replaces most of the Java server side code with Kotlin.

For example, to use the Kotlin blueprint pass the name of the blueprint like below while generating an app.

```bash
jhipster --blueprint kotlin
```

The name of the blueprint is saved in the `.yo-rc.json` and will be automatically used while executing sub-generators like `entity`, `spring-controller` and `spring-service`.

If a blueprint doesn't implement a specific sub-generator, it will be skiped and the JHipster templates for the same sub-generator will be used.

**Note:** An application can use only one blueprint, multiple blueprints are not supported yet.

## <a name="3"></a> 命令行选项

你还可以使用一些命令行选项来运行 JHipster。可以通过命令参考：`jhipster app --help`.

有这些可用参数：

* `--help` - 输出所有选项和使用帮助
* `--blueprint` - 指定一个使用的蓝图功能，例如：`jhipster --blueprint kotlin`
* `--skip-cache` - 不要记住之前提示的回答 (默认: false)
* `--skip-git` - 不要创建 Git 仓库 (默认: false)
* `--skip-install` - 不要自动安装依赖 (默认: false)
* `--skip-client` - 跳过生成客户端程序，这样就可以只创建 Spring Boot 的后端程序 (默认: false)。这和使用 server 工具命令类似：`jhipster server`。
* `--skip-server` - 跳过生成服务端程序，这样就可以只创建前端程序 (默认: false)。这和使用 client 工具命令类似：`jhipster client`。
* `--skip-user-management` - 跳过生成用户管理功能，同时作用于前端和后端 (默认: false)
* `--i18n` - 开启或关闭国际化（i18n）功能，跳过客户端代码时没有作用 (默认: true)
* `--auth` - 在跳过服务端代码生成时指定认证类型，使用 `skip-server` 时必填，其用其他模式时没有作用。
* `--db` - 在跳过 Server 端代码时指定数据库类型，其他情况无效，但是在使用 `skip-server` 时是必填的。
* `--with-entities` - 重新生成实体对象相关程序 (使用 `.jhipster` 目录下的配置) (默认：false)
* `--skip-checks` - 跳过必要工具的检查 (默认: false)
* `--jhi-prefix` - 在 service，components，state/route 前加的前缀 (默认: jhi)
* `--yarn` - 使用 Yarn 替代 NPM (默认: false)
* `--experimental` - 开启实验性质特性。请注意这些特性是不稳定的并且会在将来发生变化。

## <a name="4"></a> 提示

如果你是高级用户，还可以使用 client 和 server 的 sub-generators：`jhipster client --[options]` 和 `jhipster server --[options]`.
运行上面的命令时加上 `--help` 来查看所有的选项。

还可以使用 Yeoman 的命令行选项，例如 `--force` 来覆盖现有文件。如果你想重新生成整个项目，包括实体对象相关代码，执行：`jhipster --force --with-entities`.
