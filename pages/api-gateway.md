---
layout: default
title: API 网关
permalink: /api-gateway/
sitemap:
    priority: 0.7
    lastmod: 2017-05-03T00:00:00-00:00
---

# <i class="fa fa-exchange"></i> JHipster API 网关

JHipster 可以用来创建 API 网关应用。一个网关应用是一个普通的 JHipster 应用，
所以你可以使用常规的 JHipster 选项及一些开发在这类应用上，
 同时它还扮演者微服务入口的角色。举例来说，它能提供 HTTP 路由、负载均衡、quality of service、安全、以及 所有微服务的 API 文档。

## 摘要

1. [架构图](#architecture_diagram)
2. [HTTP 路由](#http_routing)
3. [安全](#security)
4. [自动生成文档](#documentation)
5. [限速](#rate_limiting)
6. [访问控制策略](#acl)

## <a name="architecture_diagram"></a> 架构图

<img src="{{ site.url }}/images/microservices_architecture_detail.001.png" alt="Diagram" style="width: 800; height: 600" class="img-responsive"/>

## <a name="http_routing"></a> HTTP 请求路由网关

当网关和微服务启动时，他们会将自己注册到注册中心上去（在 `src/main/resources/config/application.yml` 文件中配置 `eureka.client.serviceUrl.defaultZone` 参数）。

网关会自动代理所有请求到各微服务，以这些微服务的名字：比如，微服务 `app1` 注册后，他在网关上的 `/app1` 可供调用。

举例来说，如果你的网关运行在 `localhost:8080`，你可以通过 [http://localhost:8080/app1/rest/foos](http://localhost:8080/app1/rest/foos) 来访问微服务 `app1` 的 `foos` 资源。如果你在浏览器中这么做，别忘记 REST 的资源还被 JHipster 做了安全防护，所以你还需要发送正确的 JWT 头（参考下面的安全章节说明） ，或者从微服务 `MicroserviceSecurityConfiguration` 的类中去掉安全防护.

如果存在多个相同的服务运行，网关会从注册中心中或得到这些实例，并且：

- 使用 [Netflix Ribbon](https://github.com/Netflix/ribbon) 进行负载均衡（分流）。
- 使用 [Netflix Hystrix](https://github.com/Netflix/hystrix) 做熔断器（断路器），失效的实例可以很快被安全移除。

每一个网关程序都有一个 "admin > gateway" 菜单，在那里可以监控开启的 HTTP 路由和微服务实例。

## <a name="security"></a> 安全

### JWT (JSON Web Token)

JWT (JSON Web Token) 是一种标准，具有简单易用的方法来防护微服务应用。

JHipster 使用 [JJWT library](https://github.com/jwtk/jjwt)，由 Okta 公司（译注：Matt 的现就职公司，关注于安全框架）提供，来实现 JWT。

令牌（Tokens）由网关程序创建，然后发送到下层的微服务：由于他们共享一个公共的秘钥（secret key），微服务可以验证该令牌，并对使用改令牌的用户进行认证（鉴权）。

这些令牌都是 self-sufficient 的：他们具有认证和授权信息，微服务不需要查询数据库或外部系统。这对于一个可扩展的架构非常重要。

为了使安全机制工作，JWT 秘钥需要在各应用中共享。

- 对于每一个应用，默认的令牌都是唯一的，由 JHipster 创建。存储在 `.yo-rc.json` 文件里。（译注：jwtSecretKey 配置）
- 可以在配置文件 `src/main/resources/config/application.yml` 中设置令牌在配置 `jhipster.security.authentication.jwt.secret` 上。
- 为了共享这个配置到各应用，复制网关程序中的这个配置到各微服务程序中，或者通过 JHipster Registry 的 Spring Config Server（配置中心）共享。
- 一个好的经验是在开发和生产环境中使用不同的 key。

### OAuth2

该功能目前是 **测试** 阶段，所以其文档还未完成。

JHipster 提供了一种创建 "UAA" (User Account and Authentication，用户账户和认证) 服务程序的方式，基于 Spring Security。该服务提供 OAuth2 令牌来防护网关程序。

你可以在 <a href="/using-uaa/">使用 JHipster UAA 作为微服务安全的服务</a> 章节找到所有和 UAA 相关的信息。

还有，网关程序使用 Spring Security 的实现来发送 JWT 令牌给微服务程序。

## <a name="documentation"></a> 自动生成文档

网关程序还提供他 proxifies 的服务的 Swagger API 定义，所以你可以得到相关很多工具的支持，比如 Swagger UI 和 swagger-codegen。

在网关程序的 "admin > API" 菜单下面有一个下拉列表，列出了网关的 API 以及所有微服务的 API。

在该列表中，所有的微服务 API 文档都自动创建好了，并且可以在网关程序中测试。

当使用了安全包含的 API，安全令牌会被自动加到 Swagger UI 的接口中，所有的请求就可以 work out-of-the-box.

## <a name="rate_limiting"></a> 限速

这个是高级特性，使用 [Bucket4j](https://github.com/vladimir-bukhtoyarov/bucket4j) 和 [Hazelcast](https://hazelcast.com/) 来提供微服务的 quality of service。

网关可以提供限速功能，REST 请求的数量可以被限制为：

- 根据 IP 地址 (匿名用户)
- 根据用户登入 (登入用户)

JHipster 使用 [Bucket4j](https://github.com/vladimir-bukhtoyarov/bucket4j) 和 [Hazelcast](https://hazelcast.com/) 来计算请求次数，并且会在达到限制时发送 HTTP 429 (请求过多)。默认的限制是每个用户每小时 100,000 次 API 调用。

这是个重要的特性，来保护微服务架构不被用户的请求攻击。

由于网关程序保护了所有的 REST 端点，它具有用户权限的所有访问权，所以它可以很容易的扩展来提供特定的关于用户角色的限速功能。

要使用该限速共，在 `application-dev.yml` 或 `application-prod.yml` 文件中设置 `enabled` 参数为 `true`：

    jhipster:
        gateway:
            rate-limiting:
                enabled: true

数据存储在 Hazelcast 中，所以可以扩展网关，只要 Hazelcast 分布式的缓存配置好，这是开箱即用的功能：

- 所有的网关，默认配置使用 Hazelcast
- 如果使用 [JHipster Registry]({{ site.url }}/jhipster-registry/)，所有的网关实例都会自动将它们自己注册到分布式缓存中去

如果你还想加入更多的规则，或者修改现有的规则，可以在 `RateLimitingFilter` 类中修改。一些常见的修改：

- 降低 HTTP 调用的限制
- 增加每天或每分钟的限制
- 取消管理员用户的所有限制

## <a name="acl"></a> 访问控制策略

默认情况下，所有的微服务都通过网关程序访问。如果你希望一些 API 不要从网关中访问，可以设置网关的访问控制策略来过滤。可以修改 `application-*.yml` 文件中的 `jhipster.gateway.authorized-microservices-endpoints` 配置：

    jhipster:
        gateway:
            authorized-microservices-endpoints: # Access Control Policy, if left empty for a route, all endpoints will be accessible
                app1: /api,/v2/api-docs # recommended dev configuration

例如，你只希望开放微服务 `bar` 的 `/api/foo` 端点：

    jhipster:
        gateway:
            authorized-microservices-endpoints:
                bar: /api/foo
