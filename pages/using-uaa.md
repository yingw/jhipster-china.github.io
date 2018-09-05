---
layout: default
title: 使用 JHipster UAA 作为微服务安全的服务
permalink: /using-uaa/
redirect_from:
  - /security.html
sitemap:
    priority: 0.7
    lastmod: 2016-08-25T00:00:00-00:00
---
# <i class="fa fa-lock"></i> 使用 JHipster UAA 作为微服务安全的服务

JHipster UAA 是 JHipster 微服务的一种账号和认证的安全服务，使用 OAuth2 授权协议。
（译注：也可以用于非 JHipster 应用）

要区别出 JHipster UAA 和其他 "UAA" 应用比如 [Cloudfoundry UAA](https://github.com/cloudfoundry/uaa)，
JHipster UAA 提供了完整的 OAuth2 认证服务并集成用户和角色管理端点，封装成为一个普通的 JHipster 应用。
这允许开发人员根据自己的需要做深度配置定制，不受别的封装好的 UAA 产品的策略限制。

## 概况

1. [架构](#architecture_diagram)
2. [微服务架构的安全声明](#claims)
3. [理解这里的 OAuth2](#oauth2)
4. [使用 JHipster UAA](#jhipster-uaa)
  * 初始设置
  * 理解组件
  * 刷新 Token
  * 常见错误
5. [加密 Feign 客户端调用的内部服务通讯](#inter-service-communication)
  * 使用 Eureka, Ribbon, Hystrix 以及 Feign
  * 使用 `@AuthorizedFeignClients` 注解
6. [测试 UAA 应用](#testing)
  * Feign 客户端存根
  * 模拟 OAuth2 认证

## <a name="architecture_diagram"></a> 架构

<img src="{{ site.url }}/images/microservices_architecture_detail.002.png" alt="Diagram" style="width: 800; height: 600" class="img-responsive"/>

## <a name="claims"></a> 1. 微服务架构的安全声明

在开始研究 OAuth2 之前，先来了解下稳固的安全解决方案是什么样的。

### 1. 中央认证

由于微服务建立了很多独立自治的应用，我们想得到一致的认证体验，用户不会注意到他的请求是由不同的应用提供的服务来做安全配置支持的。

### 2. 无状态

使用微服务架构的一个主要好处就是可扩展性。所以安全的解决方案也不会影响这个有点这使得在服务器上保存用户的 session 变得复杂，所以在这样的场景下使用无状态解决方案根被推崇。

### 3. 人机访问区别

There is a need of having a clear distinction of different users, and also different machines. Using microservice architecture leads to building a large multi-purpose data-center of different domains and resources, so there is a need to restrict different clients, such as native apps, multiple SPAs etc. in their access.

### 4. 细致的访问控制

While maintaining centralized roles, there is a need of configuring detailed access control policies in each microservice. A microservice should be unaware of the responsibility of recognizing users, and must just authorize incoming requests.

### 5. 攻击防护

不论安全方案能解决多少问题，都应该尽可能地抵御漏洞。

### 6. 扩展性

Using stateless protocols is not a warranty of the security solution is scalable. In the end, there should not be any single point of failure. A counter-example is a shared auth database or single auth-server-instance, which is hit once per request.


## <a name="oauth2"></a> 2. 理解这里的 OAuth2

使用 OAuth2 协议 (注意，这是个 **协议**，而非框架，也不是应用) 满足上面的 6 个诉求。该协议遵循标准，这使得该解决方案也适用于其他微服务架构。JHipster 提供了一系列解决方案，基于以下的安全设计：

![JHipster UAA architecture]({{ site.url }}/images/jhipster_uaa.png)

* Every request to any endpoint of the architecture is performed via an "client"
* A "client" is an abstract word for things like "Angular $http client", some "REST-Client", "curl", or anything able to perform requests.
* A "client" may also be used in conjunction with user authentication, like the Angular $http in the frontend client application
* Every microservice serving resources on endpoints (including the UAA), are resource servers
* Blue arrows show clients authenticate on an Oauth authorization server
* Green arrows show requests on resource servers performed by the client
* The UAA server is a combination of authorization server and resource server
* The UAA server is the owner of all the data inside the microservice applications (it approves automatically access to resource servers)
* Clients accessing resources with user authentication, are authenticated using "password grant" with the client ID and secret safely stored in the gateway configuration files
* Clients accessing resources without user, are authenticated using "client credentials grant"
* Every client is defined inside UAA (web-app, internal, ...)

这个设计可以应用到其他任何微服务架构中，不依赖于语言和框架。

另外，还提供了下面这些访问控制规则的能力：

* 用户访问可以用 "roles" 和 [RBAC][https://zh.wikipedia.org/zh/%E4%BB%A5%E8%A7%92%E8%89%B2%E7%82%BA%E5%9F%BA%E7%A4%8E%E7%9A%84%E5%AD%98%E5%8F%96%E6%8E%A7%E5%88%B6] 来设置
* 机器访问可以用 "scopes" [RBAC][] 来设置
* 复杂的访问设置用 [ABAC][https://en.wikipedia.org/wiki/Attribute-based_access_control] 来表达，再在 "roles" 和 "scopes" 上层的表达式来声明
  * 比如: hasRole("ADMIN") and hasScope("shop-manager.read", "shop-manager.write")

## <a name="jhipster-uaa"></a> 3. 使用 JHipster UAA

当开始使用 JHipster 微服务架构，你可能会选择 UAA，而不是 JWT 认证。

**注意**: UAA 的解决方案也使用 JWT，这对于定制配置以及 JWT 都较为容易使用，默认使用 Spring Cloud Security。

### 初始设置

最基础的设置组成：

1. 一个 JHipster UAA server (一种应用类型)
2. 至少一个微服务 (并使用 UAA 认证)
3. 一个 JHipster 网关 (使用 UAA 认证)

这是创建顺序。

除了认证类型，还要设置 UAA 的地址。

对于基础使用，该设置和 JWT 认证类型工作方式相同，只是基于多一个的服务。

### 理解组件

JHipster UAA server 开箱即用的三个功能：

* 它提供默认的 JHipster 用户领域模型管理，管理用户和账号资源（这是由网关提供的，以 JWT 认证方式）
* 它实现了 `AuthorizationServerConfigurerAdapter` 来支持 OAuth2 并定义了基础客户端 ("web_app" 和 "internal")
* 它提供 JWT 公钥服务在 `/oauth/token_key` 上，并被所有其他微服务调用

数据库的选择、缓存方案、搜索引擎、编译工具以及其他 JHipster 选项都可以有开发人员自行选择。

当一个微服务应用启动时，它会预期 UAA Server 已经启动并共享了公钥。它会调用 `/oauth/token_key` 来获取公钥并设置秘钥签名 (`JwtAccessTokenConverter`).

如果 UAA 没有启动，应用会在之后继续启动并尝试获取公钥。有两个设置 - `uaa.signature-verification.ttl` 控制 key 的有效期，`uaa.signature-verification.public-key-refresh-rate-limit` 限制访问 UAA 的请求来避免被爆。These values are usually left at their default values. In any case, if verification fails, then the microservice will check if there's a new key. That way, keys can be replaced on the UAA and the services will catch up.

From this point there are two use cases that may happen in this basic setup: user calls and machine calls.

For the user calls, a login request is sent to the gateway's `/auth/login` endpoint.  This endpoint uses `OAuth2TokenEndpointClientAdapter` to send a request to the UAA authenticating with the "password" grant.  Because this request happens on the gateway, the client ID and secret are not stored in any client-side code and are inaccessible to users.  The gateway returns a new Cookie containing the token, and this cookie is sent with each request performed from the client to the JHipster backend.

For the machine calls, the machine has to authenticate as a UAA using client credentials grant. JHipster provides a standard solution, described in [secure inter-service-communication using feign clients](#inter-service-communication)

### 刷新 Tokens

The general flow for refreshing access tokens happens on the gateway and is as follows:

- Authentication is done via `AuthResource` calling `OAuth2AuthenticationService`'s authenticate which will set Cookies.
- For each request, the `RefreshTokenFilter` (installed by `RefreshTokenFilterConfigurer`) checks whether the access token is expired and whether it has a valid refresh token
- If so, then it triggers the refresh process via `OAuth2AuthenticationService` refreshToken.
- This uses the `OAuth2TokenEndpointClient` interface to send a refresh token grant to the OAuth2 server of choice, in our case UAA (via `UaaTokenEndpointClient`).
- The result of the refresh grant is then used downstream as new cookies and set upstream (to the browser) as new cookies.

### 常见错误

Here is a brief list of the very major things a developer should be aware of.

#### ***生产测试环境使用相同的秘钥***

It is strictly recommended to use different signing keys as much as possible. Once a signing key gets into wrong hands, it is possible to generate full access granting key without knowing login credentials of any user.

#### ***不使用 TLS***

If an attacker manages to intercept an access token, he will gain all the rights authorized to this token, until the token expires. There are a lot of ways to achieve that, in particular when there is no TLS encryption. This was not a problem in the days of version 1 of OAuth, because protocol level encryption was forced.

#### ***在 URL 中使用访问 token***

As of standard, access tokens can be either passed by URL, in headers, or in a cookie. From the TLS point of view, all three ways are secure. In practice passing tokens via URL is less secure, since there several ways of getting the URL from records.

#### ***使用对称加密***

RSA(非对称加密算法) is not required for JWT signing, and Spring Security does provide symmetric token signing as well. This also solves some problems, which make development harder（译注：应该是 easier）. But this is insecure, since an attacker just needs to get into one single microservice to be able to generate its own JWT tokens.

## <a name="inter-service-communication"></a> 4. 加密 Feign 客户端调用的内部服务通讯

Currently only JHipster UAA is providing an scalable approach of secure inter-service-communication.

Using JWT authentication without manually forwarding JWTs from request to internal request forces microservices to call other microservices over the gateway, which involves additional internal requests per one master requests. But even with forwarding, it's not possible to cleanly separate user and machine authentication.

Since JHipster UAA is based on OAuth2, all these problems are solved on protocol definition.

This chapter covers how to easily get started with this.

### 使用 Eureka, Ribbon, Hystrix 和 Feign

When one service wants to request data from another, finally all these four players come into play. So it is important, to briefly know what each of them is responsible for:

* Eureka: this is where services (un-)register, so you can ask "foo-service" and get a set of IPs of instances of the foo-service, registered in Eureka
* Ribbon: when someone asked for "foo-service" and already retrieved a set of IPs, Ribbon does the load balancing over these IPs.

So to sum up, when we got a URL like "http://uaa/oauth/token/" with 2 instances of JHipster UAA server running on 10.10.10.1:9999 and 10.10.10.2:9999, we may use Eureka and Ribbon to quickly transform that URL either to "http://10.10.10.1:9999/oauth/token" or "http://10.10.10.2:9999/oauth/token" using a Round Robin algorithm.

* Hystrix: a circuit breaker system solving fall-back scenarios on service fails
* Feign: using all that in a declarative style

In real world, there is no warranty of all instances of all services to be up. So Hystrix works as a circuit breaker, to handle failure scenarios in a well-defined way, using fallbacks.

But wiring and coding all these things manually is a lot of work: Feign provides the option of writing ***Ribbon*** load balanced REST clients for endpoints registered in ***Eureka***, with fallback implementations controlled using ***Hystrix***, using nothing more then an Java interfaces with some annotations.

So for inter-service-communication, Feign clients are very helpful. When one service needs a REST client to access an "other-service", serving some "other-resource", it's possible to declare an interface like:

``` java
@FeignClient(name = "other-service")
interface OtherServiceClient {
  @RequestMapping(value = "/api/other-resources")
  List<OtherResource> getResourcesFromOtherService();
}
```

And then, using it via dependency injection, like:

``` java
@Service
class SomeService {
  private OtherServiceClient otherServiceClient;

  @Inject
  public SomeService(OtherServiceClient otherServiceClient) {
    this.otherServiceClient = otherServiceClient;
  }
}
```

Similar to Spring Data JPA, there is no need to implement that interface. But you may do so, if using Hystrix. Implemented classes of Feign client interfaces act as fallback implementations.

One open issue is, to make this communication secure using UAA. To accomplish this, there should be some request interceptor for Feign, which implements the client 凭证 flow from OAuth, to authorize the current service for requesting the other service. 在 JHipster 应用中，你只需要加上 `@AuthorizedFeignClients` 注解。这是个特殊的 JHipster 注解，完成了客户端 OAuth 凭证实现。

### 使用 `@AuthorizedFeignClients` 注解

Considering the above Feign client should be used to an "other-service", which
serves protected resources, the interface must be annotated like this:

``` java
@AuthorizedFeignClient(name = "other-service")
interface OtherServiceClient {
  @RequestMapping(value = "/api/other-resources")
  List<OtherResource> getResourcesFromOtherService();
}
```

**note**: Due to a bug in Spring Cloud, it's currently not possible to use a different
notation for the service name（译注：也就是说，一定要用 name 属性）, as

``` java

@AuthorizedFeignClient("other-service")
```

or

``` java

@AuthorizedFeignClient(value = "other-service")
```

The REST client automatically gets authorized with your UAA server, when there is no valid access token stored in memory.

This approach addresses a scenario when machine request run over a separate OAuth client not referring to an user session. This is important, in particular when entity auditing is used on a request, issued by another request in another service. As an alternative, the access token of the initial request may be forwarded to further calls. Currently, there is no "default solution" provided by JHipster.

## <a name="testing"></a> 5. 测试 UAA applications

### 模拟 Feign 客户端

Components working with Feign clients should be testable. Using Feign in tests the same way it is used in production would force the JHipster Registry and the UAA server to be up and reachable to the same machine where the tests are run. But in most cases, you don't want to test that Feign itself works (it usually does), but your components using Feign clients.

To test components, which are using feign clients inside is possible using `@MockBean`, which is part of spring boot since 1.4.0.

Here is an example, testing `SomeService` works as expected, with mocked values for the client:

``` java

@RunWith(SpringRunner.class)
@SpringBootTest(App.class)
public class SomeServiceTest {

    @MockBean
    private OtherServiceClient otherServiceClient;

    @Inject
    private SomeService someService;

    @Test
    public void testSomeService() {
        given(otherServiceClient.getResourcesFromOtherService())
        .willReturn(Arrays.asList(new OtherResource(...));

        someService.performActionWhichInkvokesTheAboveMentionedMethod();

        //assert that your application is in the desired state
    }
}
```

So with this technology you are simulating the behavior of the other service, and provide expected resource entity, which would come from the origin.
All Beans injecting a client will behave as mocked, so you can focus on the logic of these Beans.

### 模拟 OAuth2 认证

Using Spring's integration tests against the REST controllers is usually bypassing the security configuration, since it would make testing hard, when the only intention is to prove the controller is functional doing what it should do. But sometimes, testing a controller's security behavior is part of testing, too.

For this use-case, JHipster is providing an component called `OAuth2TokenMockUtil`, which can emulate a valid authentication without forcing the user or client to exist.

To use this feature, two things have to be done:

#### 1. Enabling security in the mock Spring MVC context and inject the mock util

``` java

    @Inject
    private OAuth2TokenMockUtil tokenUtil;

    @PostConstruct
    public void setup() {
        this.restMockMvc = MockMvcBuilders
            .webAppContextSetup(context)
            .apply(springSecurity())
            .build();

    }
```

***In this test no single instance of the controller has to be mocked, but the
application's `WebApplicationContext`***

#### 2. 使用 `OAuth2TokenMockUtil`

The util offers a method "oaut2authentication", which is usable to MockMvc "with" notation. Currently it can be configured to mock a authentication with the following fields:

* username
* roles (Set<String>)
* scope (Set<String>)

例子：

``` java

@Test
public void testInsufficientRoles() {
    restMockMvc.peform(
        get("url/requiring/ADMIN/role")
        .with(tokenUtil.oauth2Authentication("unpriveleged.user@example.com", Sets.newSet("some-scope"), Sets.newSet("ROLE_USER")))
    ).andExpect(status().isForbidden());
}
```

[RBAC]: https://de.wikipedia.org/wiki/Role_Based_Access_Control
[ABAC]: https://en.wikipedia.org/wiki/Attribute-Based_Access_Control
