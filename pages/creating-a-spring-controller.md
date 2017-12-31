---
layout: default
title: 创建控制器
permalink: /creating-a-spring-controller/
sitemap:
    priority: 0.7
    lastmod: 2017-10-17T00:00:00-00:00
---

# <i class="fa fa-bolt"></i> 创建控制器

## 简介

_说明: 此工具比 [entity 工具]({{ site.url }}/creating-an-entity/) 更简单，entity 是创建完整的 CRUD 实体操作的。_

这是个创建 Spring MVC REST 控制器的工具，也可以用来创建简单的 REST 方法。

比如需要创建一个名为 "Foo" 的 Spring MVC REST 控制器：

`jhipster spring-controller Foo`

该命令工具会询问你需要用创建哪个方法：你只需要回答方法名和需要的 HTTP 参数，方法就创建好了。

## 可以用 Swagger 来定义 Spring MVC REST 控制器的文档吗？

可以！事实上这已经做好了！在 `dev` 模式中，用菜单 `Administration > API` 来访问 Swagger UI 来早点你创建的控制器吧。

## 可以给 Spring MVC REST 控制器加上权限控制吗？

可以的！只需要给你的类或者方法上增加 Spring Security 的 `@Secured` 注解，并用 `AuthoritiesConstants` 类来配置用户权限。

## 可以监控 Spring MVC REST 控制器吗？

可以！给需要监控的方法加上 Metrics 的 `@Timed` 注解即可。
