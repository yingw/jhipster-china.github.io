---
layout: default
title: 创建 service
permalink: /creating-a-spring-service/
redirect_from:
  - /creating_a_service.html
  - /creating-a-service/
sitemap:
    priority: 0.7
    lastmod: 2017-10-17T00:00:00-00:00
---

# <i class="fa fa-bolt"></i> 创建 Spring service

## 介绍

_注意: 这个工具比起 [entity 子命令]({{ site.url }}/creating-an-entity/) 要简单的多_

该命令用来创建 Spring Service bean，在里面写应用的复杂逻辑代码。

创建一个 "Bar" Service bean：

`jhipster spring-service Bar`

_这就创建了一个简单的 "BarService"，非常少的代码，但他们通常会带来一连串的问题。我们来做一些常见问题的解答：

## 为什么 “entity” 命令不直接创建 Service 类？

我们主要有两个架构考量：

*   我们不想创建无用的 Services：如果你需要的只是简单的 CRUD（增删改查）数据库操作，你就不需要 Service bean。所以默认 JHipster 不会创建他们。
*   我们认为 Service bean 比起 repository 更加 coarse-grained（粗放）。Service bean 会使用多个 repository 来提供业务实现。所以创建单个实体时不需要创建 Service bean。

## 我们需要使用接口来实现 Service Bean 吗？

简单来说： **不需**.

如果你想听更长的回复，理由是：

One of the main interests of using Spring is AOP. This is the technology that allows Spring to add new behaviors on top of your Beans: for instance, this is how transactions or security work.

In order to add those behaviors, Spring needs to create a proxy on your class, and there are two ways of creating a proxy:

*   If your class uses an interface, Spring will use a standard mechanism provided by Java to create a dynamic proxy.
*   If your class doesn't use an interface, Spring will use CGLIB to generate a new class on the fly: this is not a standard Java mechanism, but it works as well as the standard mechanism.

Some people will also argue that interfaces are better for writing tests, but we believe we shouldn't modify our production code for tests, and that all the new mocking frameworks (like EasyMock) allow you to create very good unit tests without any interfaces.

所以，总得来说，我们认为 Service 的接口是没用的，所以我们不推荐使用（但我们还是留了那个选项给你，哦，天呐）。

## 为什么我需要在事务中查询延迟加载的 JPA 关系？

JPA 默认使用延迟加载技术来管理 one-to-many（一对多） 以及 many-to-many（多对多） 实体关系。如果你使用了这个默认设置，你可能会得到一个可怕的 `LazyInitializationException` 异常：它是说你尝试在事务外使用了未初始化的关系。

默认创建的 Service 类使用 `@Transactional` ，所有的方法都是事务控制了的。这意味着可以在十五中查询到所有的延迟加载关系，也不会有 `LazyInitializationException` 异常。

_提示：_ 用 `@Transactional(readOnly = true)` 注解来声明不需要修改数据不启动事务。这会得到更好的性能 (Hibernate 不会刷新一级缓存)，as well as a quality enhancement with some JDBC drivers (Oracle won't allow you to send INSERT/UPDATE/DELETE statements)

## 能给 Service Beans 安全管理么？

可以，加上 Spring Security 的注解 `@Secured` 在类上或者方法上，还可以用注解 `AuthoritiesConstants` 来限制不同用户的访问。

## 可以监控 Service Bean 么？

可以！加上注解 `@Timed`（译注：Dropwizard metric）在方法上。
