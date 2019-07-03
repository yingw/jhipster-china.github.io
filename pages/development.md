---
layout: default
title: JHipster 开发相关技术
permalink: /development/
redirect_from:
  - /development.html
sitemap:
    priority: 0.7
    lastmod: 2016-12-01T00:00:00-00:00
---

# <i class="fa fa-code"></i> JHipster 开发相关技术

_**请在这里 [视频教程]({{ site.url }}/video-tutorial/) 查看我们关于创建 JHipster 应用的教程！**_

## 概述

1.  [通用配置](#general-configuration)
2.  [运行 Java 服务器](#running-java-server)
3.  [运行 Angular/React](#working-with-angular)
4.  [使用数据库](#using-a-database)
5.  [国际化](#internationalization)

## <a name="general-configuration"></a> 通用配置

### IDE 设置

If you haven't configured your IDE yet, please go to the [Configuring your IDE]({{ site.url }}/configuring-ide/) page.

### 应用配置

By default, JHipster uses the "development" profile, so you don't have to configure anything.

If you want more information on the available profiles, please go the section titled "[Profiles]({{ site.url }}/profiles/)".

If you want to configure some specific JHipster properties, have a look at the [common application properties]({{ site.url }}/common-application-properties/) page.

## <a name="running-java-server"></a> 运行 Java 服务器

### 作为 "main" Java 类

From your IDE, right-click on the "Application" class at the root of your Java package hierarchy, and run it directly. You should also be able to debug it as easily.

The application will be available on [http://localhost:8080](http://localhost:8080).

This application will have "hot reload" enabled by default, so if you compile a class, the Spring application context should refresh itself automatically, without the need to restart the server.

### 作为 Maven 项目

You can launch the Java server with Maven. JHipster provides a Maven wrapper, so you don't need to install Maven, and you have the guarantee that all project users have the same Maven version:

`./mvnw` (on Mac OS X/Linux) of `mvnw` (on Windows)

(this will run our default Maven task, `spring-boot:run`)

The application will be available on [http://localhost:8080](http://localhost:8080).

Alternatively, if you have installed Maven, you can launch the Java server with Maven:

`mvn`

If you want more information on using Maven, please go to [http://maven.apache.org](http://maven.apache.org)

### (可选) 作为 Gradle 项目

If you selected the Gradle option, JHipster provides a Gradle wrapper, so you don't need to install Gradle, and you have the guarantee that all project users have the same Gradle version:

`./gradlew` (on Mac OS X/Linux) of `gradlew` (on Windows)

(this will run our default Gradle task, `bootRun`)

Alternatively, if you have installed Gradle, you can launch the Java server with Gradle:

`gradle`

The application will be available on [http://localhost:8080](http://localhost:8080).

If you want more information on using Gradle, please go to [https://gradle.org](https://gradle.org)

## <a name="working-with-angular"></a> 运行 Angular/React

### 运行 Webpack

_This step is required to see changes in your TypeScript code and have live reloading of your client-side code._

Running Webpack is the default task in the `package.json` file, so you just need to run:

`npm start`

(or, if you use Yarn, `yarn start`).

This provides very impressive features:

*   As soon as you modify one of your HTML/CSS/TypeScript file, your browser will refresh itself automatically
*   When you test your application on several different browsers or devices, all your clicks/scrolls/inputs should be automatically synchronized on all screens

This will launch:

- A Webpack task that will automatically compile TypeScript code into JavaScript
- A Webpack "hot module reload" server that will run on [http://localhost:9060/](http://localhost:9060/) (and has a proxy to [http://127.0.0.1:8080/api](http://127.0.0.1:8080/api) to access the Java back-end)
- A BrowserSync task that will run on [http://localhost:9000/](http://localhost:9000/), which has a proxy to [http://localhost:9060/](http://localhost:9060/) (the Webpack "hot module reload" server), and which will synchronize the user's clicks/scrolls/inputs
- The BrowserSync UI, which will be available on [http://localhost:3001/](http://localhost:3001/)

### 运行 NPM

Direct project dependencies are configured into `package.json`, but transitive dependencies are defined into the `package-lock.json` file, that get generated when `npm install` is run.

It is advised to check `package-lock.json`[https://docs.npmjs.com/files/package-lock.json] into source control, so that all team members of a project have the same versions of all dependencies. Running `npm install` again will regenerate the `package-lock.json` with the latest versions of transitive dependencies.

### 其他 NPM/Yarn 任务

Those tasks are the same whether you use NPM or Yarn, we use the `npm` command as an example but you can replace it with `yarn`.

- `npm run lint`: check for code style issues in the TypeScript code
- `npm run lint:fix`: try to automatically correct TypeScript lint issues
- `npm run tsc`: compile the TypeScript code
- `npm run test`: run unit tests with Jest
- `npm run test:watch`: keep the Jest unit tests running, for live feedback when code is changed
- `npm run e2e`: run "end to end" tests with Protractor (only works if the Protractor option has been selected when the project was generated)

## <a name="using-a-database"></a> 使用数据库

### 运行数据库

If you use a non-embedded database, like MySQL, MariaDB, PostgreSQL, MSSQL, MongoDB, Cassandra or Couchbase, you will need to install and configure that database.

The easiest and recommended way with JHipster is to use Docker Compose. [Follow our Docker Compose guide here.]({{ site.url }}/docker-compose/)

If you prefer to install and configure your database manually, don't forget to configure your Spring Boot properties accordingly in your `src/main/resources/config/application-*.yml` files (for example your database URL, login and password).

### 在开发环境中使用 H2 数据库

If you choose the H2 database, you will have an in-memory database running inside your application, and you can access its console at [http://localhost:8080/h2-console](http://localhost:8080/h2-console) by default.

To connect to the database, select the pre-configured options:

*   Driver Class: org.h2.Driver
*   JDBC URL: jdbc:h2:mem:jhipster
*   User name: <blank>
*   Password: <blank>

![]({{ site.url }}/images/h2.png)

### 在开发环境中使用 MySQL, MariaDB 或 PostgreSQL

This option is bit more complex than using H2, but you have a some important benefits:

*   Your data is kept across application restarts
*   Your application starts a little bit faster
*   You can use the great `./mvnw liquibase:diff` goal (see below)

**Note**: for MySQL, you probably need to start your database with these options:

*   `--lower_case_table_names=1` : see the [documentation](https://dev.mysql.com/doc/refman/5.7/en/identifier-case-sensitivity.html)
*   `--skip-ssl` : see the [documentation](https://dev.mysql.com/doc/refman/5.7/en/encrypted-connection-options.html#option_general_ssl)
*   `--character_set_server=utf8` : see the [documentation](https://dev.mysql.com/doc/refman/5.7/en/server-options.html#option_mysqld_character-set-server)
*   `--explicit_defaults_for_timestamp` : see the [documentation](https://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_explicit_defaults_for_timestamp)

The command is:

    mysqld --lower_case_table_names=1 --skip-ssl --character_set_server=utf8 --explicit_defaults_for_timestamp

## 数据库更新

If you add or modify a JPA entity, you will need to update your database schema.

JHipster uses [Liquibase](http://www.liquibase.org) to manage the database updates, and stores its configuration in the `/src/main/resources/config/liquibase/` directory. There are 3 ways to work with Liquibase: use the entity sub-generator, use the liquibase:diff Maven goal, or update the configuration files manually.

### 使用 entity 命令更新数据库

If you use the [entity sub-generator]({{ site.url }}/creating-an-entity/), here is the development workflow:

*   Run the [entity sub-generator]({{ site.url }}/creating-an-entity/)
*   A new "change log" is created in your `src/main/resources/config/liquibase/changelog` directory, and has been automatically added to your `src/main/resources/config/liquibase/master.xml` file
*   Review this change log, it will be applied the next time you run your application

### 用 Maven liquibase:diff 命令更新数据库

If you have choosen to use MySQL, MariaDB or PostgreSQL in development, you can use the `./mvnw liquibase:diff` goal to automatically generate a changelog.

If you are running H2 with disk-based persistence, this workflow is not yet working perfectly, but you can start trying to use it (and send us feedback!).

[Liquibase Hibernate](https://github.com/liquibase/liquibase-hibernate) is a Maven plugin that is configured in your pom.xml, and is independant from your Spring application.yml file, so if you have changed the default settings (for example, changed the database password), you need to modify both files.

Here is the development workflow:

*   Modify your JPA entity (add a field, a relationship, etc.)
*   Compile your application (this works on the compiled Java code, so don't forget to compile!)
*   Run `./mvnw liquibase:diff` (or `./mvnw compile liquibase:diff` to compile before)
*   A new "change log" is created in your `src/main/resources/config/liquibase/changelog` directory
*   Review this change log and add it to your `src/main/resources/config/liquibase/master.xml` file, so it is applied the next time you run your application

If you use Gradle instead of Maven, you can use the same workflow by running `./gradlew liquibaseDiffChangelog -PrunList=diffLog`, and change the database configuration in `build.gradle` in the liquibase configuration if required.

### 手动修改变更日志来更新数据库

If you prefer (or need) to do a database update manually, here is the development workflow:

*   Modify your JPA entity (add a field, a relationship, etc.)
*   Create a new "change log" in your `src/main/resources/config/liquibase/changelog` directory. The files in that directory are prefixed by their creation date (in yyyyMMddHHmmss format), and then have a title describing what they do. For example, `20141006152300_added_price_to_product.xml` is a good name.
*   Add this "change log" file in your `src/main/resources/config/liquibase/master.xml` file, so it is applied the next time you run your application

If you want more information on using Liquibase, please go to [http://www.liquibase.org](http://www.liquibase.org).

## <a name="internationalization"></a> 国际化

Internationalization (or i18n) is a first-class citizen in JHipster, as we believe it should be set up at the beginning of your project (and not as an afterthought).

Usage is really easy:

- With Angular, thanks to [NG2 translate](https://github.com/ocombe/ng2-translate) and a specific JHipster component, which uses simple JSON files for translation
- With React, thanks to a specific JHipster component, which works the same way as the Angular component, and uses the same files

For example, to add a translation to the "first name" field, just add a "translate" attribute with a key: `<label jhiTranslate="settings.form.firstname">First Name</label>`

This key references a JSON document, which will return the translated String. Angular/React will then replace the "First Name" String with the translated version.

If you want more information on using languages, read our [Installing new languages documentation]({{ site.url }}/installing-new-languages/).
