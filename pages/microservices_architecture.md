---
layout: default
title: 用 JHipster 设计微服务架构
permalink: /microservices-architecture/
sitemap:
    priority: 0.7
    lastmod: 2016-03-10T00:00:00-00:00
---

# <i class="fa fa-sitemap"></i> 用 JHipster 设计微服务架构

## <a name="microservices_vs_monolithic"></a> 微服务 vs 传统架构

使用 JHipster 时，第一个问题就是询问你选择哪种类型的应用。你可以选择两种应用架构风格：

- "巨石（monolithic）" 类架构使用一个单一的、通用的应用，其内部包含前端的 Angular 代码，以及后端的 Spring Boot 代码。
- "微服务（microservices）" 架构将前端和后端拆分开来，使你的应用 to scale and survive infrastructure issues.

"巨石 monolithic" 应用比较容易上手，所以如果你不是有特别的需要，这还是我们的推荐方式，也是 JHipster 的默认选项。

## <a name="overview"></a> 微服务架构预览

JHipster 的微服务架构以下面介绍的方式工作：

 * [网关（gateway）]({{ site.url }}/api-gateway/) 是一个 JHipster 生成的应用（使用应用类型：`microservice gateway`），它处理网络流量，
  并且服务为一个 Angular 应用。可以存在多个网关，如果你遵循 [Backends for Frontends pattern（译注：多前端架构）](https://www.thoughtworks.com/insights/blog/bff-soundcloud), 但这个并不是必要的。
 * [Traefik]({{ site.url }}/traefik/) 是一个现代化的 HTTP 反向代理及负载均衡器，它可以和网关集成工作。
 * [JHipster Registry]({{ site.url }}/jhipster-registry/) 是一个所有应用注册和配置中心。它还能提供运行时监控仪表盘。
 * [Consul]({{ site.url }}/consul/) 是一个服务发现提供者，使用键/值对存储。它可以作为 JHipster Registry 的替代。
 * [JHipster UAA]({{ site.url }}/us/) 是一个 JHipster 构建的 User 认证和鉴权中心，使用 OAuth2 协议。
 * [Microservices]({{ site.url }}/creating-microservices/) 是指一堆用 JHipster 构建的应用（使用应用类型：`microservice application`），它们处理 REST 请求。它们都是无状态的，以及它们中的一些可以被运行为多个实例来应对高负载。
 * [JHipster Console](https://github.com/jhipster/jhipster-console) 是一个监控和告警控制台，使用 ELK 架构。

下面的图，绿色组件是指你的应用，蓝色的组件提供基础架构。

<img src="{{ site.url }}/images/microservices_architecture_2.png" alt="Diagram" style="width: 930px; height: 558px"/>
