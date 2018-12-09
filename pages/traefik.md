---
layout: default
title: Traefik 简介
permalink: /traefik/
sitemap:
    priority: 0.7
    lastmod: 2017-09-27T00:00:00-00:00
---

# <i class="fa fa-exchange"></i> Traefik

## Traefik 简介

[Traefik](https://traefik.io/) 是一个现代化的 HTTP 反向代理及负载均衡来使微服务的部署更加方便

它可以像 Zuul 一样路由 HTTP 请求，所以也有一部分和 [JHipster gateway]({{ site.url }}/api-gateway/) 重叠的功能，但是它比 API 网关工作在更底层：它只会路由 HTTP 请求，并不提供限速、安全、或 Swagger 文档集成。

用 Traefik 的一个好处就是它可以和许多不同的服务发现方案协同工作：但是和 JHipster 一起，它默认只能使用 [Consul]({{ site.url }}/consul/)。

它可以工作在两种不同模式的架构中，如下所描述：

## 架构图 1: 默认设置

Traefik 是一个反向代理及负载均衡器，它可以代替 Zuul，并且直接将 HTTP 请求路由到对应的服务上。

<img src="{{ site.url }}/images/microservices_architecture_detail.004.png" alt="Diagram" style="width: 800; height: 600" class="img-responsive"/>

在该架构中，JHipster 的网关（Gateway）不是我们之前说的网关程序，它在这里只服务于 Angular 程序。

这是我们的默认设置。

## 架构图 2: Traefik 和 Zuul

Traefik 也可以和 Zuul 一起工作：在这个例子中，到微服务的 HTTP 请求先通过 Traefik 再通过 Zuul 最终到达它的目的地。

<img src="{{ site.url }}/images/microservices_architecture_detail.005.png" alt="Diagram" style="width: 800; height: 600" class="img-responsive"/>

这将需要消耗多一次的网络请求，显而易见没有上一个架构方式有效。但是，这种情形下允许使用 JHipster 的网关程序的所有能力：可以处理限速、或者 Swagger 文档聚合功能。

结果是，Traefik 可以作为边缘服务使用，这使得 JHipster 网关程序可扩展。

这个配置是 JHipster 开箱即用的：唯一的问题是客户端程序使用的是绝对路径 URL，比如，对于 "microservice1"：

- 默认 URL 是 "/microservice1"，只经过 Traefik (这是上面的默认设置)。
- "/gateway/microservice1" URL 将使用设置在 Traefik 里的 "gateway" 网关程序，这将通过 Zuul 来到达 "microservice1" 应用。

## 开始

请注意，Traefik 只能和 [Consul]({{ site.url }}/consul/) 一起工作，所以如果你使用的是 [JHipster Registry]({{ site.url }}/jhipster-registry/) 它将无法工作。

要使用 Traefik 在微服务架构中，运行 [docker-compose sub-generator]({{ site.url }}/docker-compose/) 并在其问你使用哪种网管材程序的时候选择 Traefik。

这将会创建一个 `traefik.yml` 配置文件来运行 Traefik 在 Docker 里，以及一个 `traefik/traefik.toml` 文件，这个是 Traefik 的配置文件。

这个配置文件设置了：

- Traefik 运行在端口 `80` 上，所以如果你的网关程序叫做 `gateway`，你可以通过 [http://localhost/gateway/](http://localhost/gateway/) 来访问它。
- Traefik 管理控制台工作在端口 `28080` 上，你可以通过 [http://localhost:28080](http://localhost:28080) 访问它。

由于 Traefik 使用了 Consul，检查 Consul 管理控制台也很有用，它工作在端口 `8500` 上：[http://localhost:8500](http://localhost:8500)。
