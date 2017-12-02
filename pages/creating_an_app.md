---
layout: default
title: 创建应用
permalink: /creating-an-app/
redirect_from:
  - /creating_an_app.html
sitemap:
    priority: 0.7
    lastmod: 2017-10-17T00:00:00-00:00
---

# <i class="fa fa-rocket"></i> 创建应用

_**请查看我们的 [视频教程]({{ site.url }}/video-tutorial/) 来学习创建 JHipster 应用**_

1. [快速上手](#1)
2. [创建应用的各选项](#2)
3. [命令行选项](#3)
4. [小提示](#4)

## <a name="1"></a> 快速上手

首先，创建一个空的目录来放你的项目：

`mkdir myapplication`

进入目录：

`cd myapplication/`

开始创建项目，输入命令：

`jhipster`

回答生成器问的一些问题来根据你的需要创建项目。这些问题将在 [下一小节](#2) 介绍。

一旦项目创建完成，你可以使用 Maven（Linux/MacOS/Windows PowerShell 上执行 `./mvnw`，在 Windows Cmd 里执行 `mvnw`） 或 Gradle (在 Linux/MacOS/Windows PowerShell 上执行 `./gradlew`，在 Windows Cmd 里执行 `gradlew`).

应用启动在： [http://localhost:8080](http://localhost:8080)

**重要** 如果你希望使用 "动态加载" JavaScript/TypeScript 代码的功能，你需要执行 `gulp` (JavaScript/AngularJS 1) 或 `yarn start` (TypeScript/Angular 2+)。你可以去 [JHipster 开发的相关技术]({{ site.url }}/development/) 也了解更多信息。

## <a name="2"></a> 创建应用的各选项

_部分问题的选项会根据你之前的选择有所变化。比如你不需要设置 Hibernate 缓存如果你之前没有选择使用 SQL 数据库。_

### Which *type* of application would you like to create? （你要创建的应用类型）

你的应用类型取决于你是否需要使用微服务架构。关于微服务的使用 [这里]({{ site.url }}/microservices-architecture/) 有完整的说明，如果不是很确定，就选择默认的 "Monolithic application"。

你可以使用这些：

*   巨石类应用（Monolithic application，也可翻译为传统类应用）: 这个是经典的、通用的应用类型。比较容易使用和开发，是我们推荐的默认类型。
*   微服务应用（Microservice application）: 在微服务架构中，这是一个服务。
*   微服务网关（Microservice gateway）: 在微服务架构中，这个是边缘服务，提供请求的路由和安全控制。
*   JHipster UAA 服务：在微服务架构中，这里提供了 OAuth2 认证，安全控制。参考 <a href="/using-uaa/">JHipster UAA 文档</a>。

### What is the base name of your application? （应用名称）

你项目的名称。
（译注：只能大小写、数字，没有特殊符号等）

### What is your default Java package name? （Java 包名）

项目的根目录包设置。这个值存在 Yeoman 中，所以下一次你运行 generator 命令时该值为成为默认值。当然你也可以修改的。

### Do you want to use the JHipster Registry to configure, monitor and scale your application? （是否需要使用 JHipster Registry 来配置、监控和扩展你的应用）

[JHipster Registry]({{ site.url }}/jhipster-registry/) 是一个开源的工具，用于管理你的应用。

在使用微服务架构时是必要的 (所以在在创建巨石类应用是询问为可选的)。

### Which *type* of authentication would you like to use? （使用哪种认证）

这个问题也依赖于之前的回答。比如说，如果你选择了 [JHipster Registry]({{ site.url }}/jhipster-registry/) ，你就只能选择 JWT 认证方式了。

一共有着这些可选项：

*   JWT authentication: 使用 [JSON Web Token (JWT)](https://jwt.io/)，这个是默认值。
*   HTTP Session Authentication: 经典的基于 session 的认证方式，好比我们在 Java 中的用法 (这是大部分用户使用使用 [Spring Security](http://docs.spring.io/spring-security/site/index.html) 的方式)。你可以和 Spring Social 配套使用这个选项，能让你使用 "social login" (如 Google, Facebook, Twitter) 特性：这个是由 Spring Boot 的 Spring Social 支持来实现的。
*   OAuth 2.0 / OIDC Authentication: 这个选项使用 OpenID Connect server, 比如 [Keycloak](http://www.keycloak.org/) 或者 [Okta](https://www.okta.com)，可以在引用外部处理认证。（译注：应该还能支持 [CAS](https://www.apereo.org/projects/cas)）
*   Authentication with JHipster UAA server: 这使用的 <a href="/using-uaa/">JHipster UAA server</a> 需要被独立的创建，同时也是 OAuth2 server 在应用外处理认证。

你可以到 [应用安全]({{ site.url }}/security/) 章节获取更多信息。

### Which *type* of database would you like to use? （使用哪种数据库）

You can choose between:

- No database (only available when using a [microservice application]({{ site.url }}/microservices-architecture/))
- An SQL database (H2, MySQL, MariaDB, PostgreSQL, MSSQL, Oracle), which you will access with Spring Data JPA
- [MongoDB]({{ site.url }}/using-mongodb/)
- [Cassandra]({{ site.url }}/using-cassandra/)
- [Couchbase]({{ site.url }}/using-couchbase/)

### Which *production* database would you like to use? （生产环境使用哪种数据库）

This is the database you will use with your "production" profile. To configure it, please modify your `src/main/resources/config/application-prod.yml` file.

If you want to use Oracle, you will need to [install the Oracle JDBC driver manually]({{ site.url }}/using-oracle/).

### Which *development* database would you like to use? （开发环境使用哪种数据库）

This is the database you will use with your "development" profile. You can either use:

*   H2, running in-memory. This is the easiest way to use JHipster, but your data will be lost when you restart your server.
*   H2, with its data stored on disk. This is currently in BETA test (and not working on Windows), but this would eventually be a better option than running in-memory, as you won't lose your data upon application restart.
*   The same database as the one you chose for production: it's a bit more complex to set up, but it should be better in the end to work on the same database as the one you will use in production. This is also the best way to use liquibase-hibernate as described in [the development guide]({{ site.url }}/development/).

（译注：推荐3，和生产库使用同样的数据库，而不是 H2，驱动不说，丢数据麻烦。只是需要设置一下数据库位置而已）

To configure it, please modify your `src/main/resources/config/application-dev.yml` file.

### Do you want to use Hibernate 2nd level cache? （是否使用 Hibernate 二级缓存）

[Hibernate](http://hibernate.org/) is the JPA provider used by JHipster. For performance reasons, we highly recommend you to use a cache, and to tune it according to your application's needs.
If you choose to do so, you can use either [ehcache](http://ehcache.org/) (local cache) or [Hazelcast](http://www.hazelcast.com/) (distributed cache, for use in a clustered environnement)

### Would you like to use Maven or Gradle? （使用 Maven 或 Gradle）

You can build your generated Java application either with [Maven](http://maven.apache.org/) or [Gradle](http://www.gradle.org/). Maven is more stable and more mature. Gradle is more flexible, easier to extend, and more hype.

### Which other technologies would you like to use? （使用哪些额外的技术）

This is a multi-select answer, to add one or several other technologies to the application. Available technologies are:

#### Social login (Google, Facebook, Twitter)（社区登入功能）

This option is only available if you selected an SQL, MongoDB, or Couchbase database. It adds [Spring Social](http://projects.spring.io/spring-social/) support to JHipster, so end-users can log-in using their Google, Facebook or Twitter account.

#### API first development using swagger-codegen（API 优先开发模式，使用 swagger-codegen）

This option lets you do [API-first development]({{ site.url }}/doing-api-first-development) for your application by integrating the [Swagger-Codegen](https://github.com/swagger-api/swagger-codegen) into the build.

#### Search engine using ElasticSearch（搜索引擎 ElasticSearch）

[Elasticsearch](https://github.com/elastic/elasticsearch) will be configured using Spring Data Elasticsearch. You can find more information on our [Elasticsearch guide]({{ site.url }}/using-elasticsearch/).

#### Clustered HTTP sessions using Hazelcast（使用 Hazelcast 来设置 Http Session 集群）

By default, JHipster uses a HTTP session only for storing [Spring Security](http://docs.spring.io/spring-security/site/index.html)'s authentication and authorisation information. Of course, you can choose to put more data in your HTTP sessions.
Using HTTP sessions will cause issues if you are running in a cluster, especially if you don't use a load balancer with "sticky sessions".
If you want to replicate your sessions inside your cluster, choose this option to have [Hazelcast](http://www.hazelcast.com/) configured.

#### WebSockets using Spring Websocket （使用 Spring Websocket）

Websockets can be enabled using Spring Websocket. We also provide a complete sample to show you how to use the framework efficiently.

#### Asynchronous messages using Apache Kafka

Use [Apache Kafka]({{ site.url }}/using-kafka/) as a publish/subscribe message broker.

### Which *Framework* would you like to use for the client? （使用哪种客户端框架）

The client-side framework to use.

You can either use:

*   Angular version 4+
*   AngularJS version 1.x (which will be deprecated in the future)

### Would you like to use the LibSass stylesheet preprocessor for your CSS? （使用 LibSass 预处理 CSS？）

[Node-sass](https://www.npmjs.com/package/node-sass) a great solution to simplify designing CSS. To be used efficiently, you will need to run a [Gulp](http://www.gulpjs.com) server, which will be configured automatically.

### Would you like to enable internationalization support? （支持国际化？）

By default JHipster provides excellent internationalization support, both on the client side and on the server side. However, internationalization adds a little overhead, and is a little bit more complex to manage, so you can choose not to install this feature.

### Which testing frameworks would you like to use? （使用哪些测试框架）

By default JHipster provide Java unit/integration testing (using Spring's JUnit support) and JavaScript unit testing (using Karma.js). As an option, you can also add support for:

*   Performance tests using Gatling
*   Behaviour tests using Cucumber
*   Angular integration tests with Protractor

You can find more information on our ["Running tests" guide]({{ site.url }}/running-tests/).

### Would you like to install other generators from the JHipster Marketplace? （是否要从 JHipster Marketplace 上下载额外的插件）

The [JHipster Marketplace]({{ site.url }}/modules/marketplace/) is where you can install additional modules, written by third-party developers, to add non-official features to your project.

## <a name="3"></a> 命令行选项

You can also run JHipster with some optional command-line options. Reference for those options can be found by typing `jhipster app --help`.

Here are the options you can pass:

* `--help` - 输出所有选项和使用帮助
* `--skip-cache` - Do not remember prompt answers (Default: false)
* `--skip-git` - Do not add the generated project to Git automatically (Default: false)
* `--skip-install` - Do not automatically install dependencies (Default: false)
* `--skip-client` - Skip the client-side application generation, so you only have the Spring Boot back-end code generated (Default: false). This is same as running server sub-generator with `jhipster server`.
* `--skip-server` - Skip the server-side application generation, so you only have the front-end code generated (Default: false). This is same as running client sub-generator with `jhipster client`.
* `--skip-user-management` - Skip the user management generation, both on the back-end and on the front-end (Default: false)
* `--i18n` - Disable or enable i18n when skipping client side generation, has no effect otherwise (Default: true)
* `--auth` - Specify the authentication type when skipping server side generation, has no effect otherwise but mandatory when using `skip-server`
* `--db` - Specify the database when skipping server side generation, has no effect otherwise but mandatory when using `skip-server`
* `--with-entities` - Regenerate the existing entities if they were already generated (using their configuration in the `.jhipster` folder) (Default: false)
* `--skip-checks` - Skip the check of the required tools (Default: false)
* `--jhi-prefix` - Add prefix before services, components and state/route names (Default: jhi)
* `--npm` - Use NPM instead of Yarn (Default: false)
* `--experimental` - Enable experimental features. Please note that these features may be unstable and may undergo breaking changes at any time

## <a name="4"></a> 小提示

If you are an advanced user you can use our client and server sub-generators by running `jhipster client --[options]` and `jhipster server --[options]`.
Run the above sub-generators with `--help` flag to view all the options that can be passed.

You can also use the Yeoman command-line options, like `--force` to automatically overwrite existing files. So if you want to regenerate your whole application, including its entities, you can run `jhipster --force --with-entities`.
