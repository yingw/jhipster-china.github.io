---
layout: default
title: 查询过滤
permalink: /entities-filtering/
sitemap:
    priority: 0.7
    lastmod: 2017-08-22T00:00:00-00:00
---

# <i class="fa fa-filter"></i> 实体对象的查询过滤

## 介绍

在实现了实体的基础增删查改（CRUD）功能后，就会碰到一个常见的需求：需要对实体对象的属性进行多种过滤查询条件，
使得服务端查询会变得更有效。这些查询条件可以作为请求参数，来使客户端和浏览器都能很方便地使用。
另外，这些条件应该能遵循一定的模式，使得它们能被自由地使用。

## 如何启用

当使用 `jhipster entity` 命令创建实体时，选择使用 services 或 service implementation 来在这个实体上启用查询条件。

如果你想在一个现有的实体上启用，可以编辑项目文件夹 `.jhipster` 里的实体对象配置文件，把 `service` 参数从 `no` 改为 `serviceClass` 或 `serviceImpl`，以及 `jpaMetamodelFiltering` 属性改为 `true`，最后重新生成对象命令：`jhipster entity <entity name>`。

如果使用了 JDL，添加一行：`filter <entity name>`，然后重新导入： `jhipster import-jdl`。

## 公共接口

对于每个实体对象，你可以启用查询条件，然后你就可以通过 `/api/my-entity` 的 GET 端点以及这些参数:

* 对于每一个 *xyz* 字段
    * *xyz.equals=someValue*
        - 列出所有 xyz 等于 'someValue' 的对象
    * *xyz.in=someValue,otherValue*
        - 列出所有 xyz 等于 'someValue' 或 'otherValue' 的对象
    * *xyz.specified=true*
        - 列出所有 xyz 不为空的对象
    * *xyz.specified=false*
        - 列出所有 xyz 为空的对象
* 如果 *xyz* 字段的类型是 string:
    * *xyz.contains=something*
        - 列出所有 xyz 的值含有 'something' 的对象
* 如 *xyz* 字段是任意数字类型，或日期类型
    * *xyz.greaterThan=someValue*
        - 列出所有 xyz 的值大于 'someValue'
    * *xyz.lessThan=someValue*
        - 列出所有 xyz 的值小于 'someValue'
    * *xyz.greaterOrEqualThan=someValue*
        - 列出所有 xyz 的值大于等于 'someValue'
    * *xyz.lessOrEqualThan=someValue*
        - 列出所有 xyz 的值小于等于 'someValue'

当然的，他们之间还能自由组合。

对于该表达式的一个不错的测试的方式是从你的 JHipster 应用的 API 文档页面的 swagger-ui 来进行测试

![]({{ site.url }}/images/entities_filtering_swagger.png)

## 实现

当此特性启用时，会创建好一个新的服务类 `EntityQueryService` 以及另一个 `EntityCriteria`。Spring 会将请求参数转换成 `EntityCriteria` 类的属性。

在 `EntityQueryService` 类里，我们将 criteria 对象转换成一个静态的、类型安全的 JPA 查询对象。对此，还需要在编译时 **开启了静态元模型生成（static metamodel generation）** 。参考 [JPA Static Metamodel Generator 文档](http://docs.jboss.org/hibernate/orm/current/topical/html_single/metamodelgen/MetamodelGenerator.html)。

为了验证生成的查询条件生效了，以及相关的 Spring 配置，继承 `EntityResourceIntTest` 生成了很多测试案例，每一个都算独立的查询条件。

## 限制

目前只支持 SQL 数据库 (并使用了 JPA)，并启用了服务层（接口或实现）。
