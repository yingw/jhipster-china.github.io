---
layout: default
title: 应用属性配置说明
permalink: /common-application-properties/
sitemap:
    priority: 0.7
    lastmod: 2018-03-18T18:20:00-00:00
---

# <i class="fa fa-flask"></i> 应用属性 (property) 配置说明

JHipster generates a Spring Boot application, and can be configured using the standard Spring Boot properties mechanism.

Those properties are configured at generation-time by JHipster, and often have different values in development and production modes: learn more about this in our [Profiles documentation]({{ site.url }}/profiles/).

In a JHipster application, there are three kinds of properties:

1. [Spring Boot 标准属性](#1)
2. [JHipster 属性](#2)
3. [应用专有的属性](#3)

## <a name="1"></a> Spring Boot 标准属性

JHipster 应用就是 Spring Boot 应用，所以你可以配置任何标准的 [Spring Boot 应用属性](http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html).

## <a name="2"></a> JHipster 属性

JHipster 提供了一些属性，服务于 [JHipster 服务端库](https://github.com/jhipster/jhipster)。这些属性是所有 JHipster 项目的标准属性，但是其中一部分也依赖于你在构建项目时选择的组件，举例来说：`jhipster.cache.hazelcast` 属性只有在你选择了 Hazelcast 作为 Hibernate 二级缓存的时候才有效。

这些属性实现在类 `io.github.jhipster.config.JHipsterProperties` 里。

这里有一个这些属性的说明：

    jhipster:

        # 异步调用的线程池
        async:
            core-pool-size: 2 # 初始化池大小
            max-pool-size: 50 # 最大池大小
            queue-capacity: 10000 # 池的 Queue 容量

        # Specific configuration for JHipster gateways
        # See https://www.jhipster.tech/api-gateway/ for more information on JHipster gateways
        gateway:
            rate-limiting:
                enabled: false # Rate limiting is disabled by default
                limit: 100_000L # By default we allow 100,000 API calls
                duration-in-seconds: 3_600 # By default the rate limiting is reinitialized every hour
            authorized-microservices-endpoints: # Access Control Policy, if left empty for a route, all endpoints will be accessible
                app1: /api # recommended prod configuration, it allows the access to all API calls from the "app1" microservice

        # HTTP configuration
        http:
            # V_1_1 for HTTP/1.1 或 V_2_0 for HTTP/2.
            # 如果你要使用 SSL，设置 HTTP/2 (参考 the Spring Boot "server.ssl" configuration)
            version: V_1_1
            #Force the server cipher suite to follow the exact order specifying in server.ssl.ciphers (For perfect forward secrecy)
            useUndertowUserCipherSuitesOrder: true
            cache: # Used by io.github.jhipster.web.filter.CachingHttpHeadersFilter
                timeToLiveInDays: 1461 # Static assets are cached for 4 years by default

        # Hibernate 二级缓存, used by CacheConfiguration
        cache:
            hazelcast: # Hazelcast configuration
                time-to-live-seconds: 3600 # By default objects stay 1 hour in the cache
                backup-count: 1 # Number of objects backups
                # Configure the Hazelcast management center
                # Full reference is available at: http://docs.hazelcast.org/docs/management-center/3.9/manual/html/Deploying_and_Starting.html
                management-center:
                    enabled: false # Hazelcast management center is disabled by default
                    update-interval: 3 # Updates are sent to the Hazelcast management center every 3 seconds by default
                    # Default URL for Hazelcast management center when using JHipster's Docker Compose configuration
                    # See src/main/docker/hazelcast-management-center.yml
                    # Warning, the default port is 8180 as port 8080 is already used by JHipster
                    url: http://localhost:8180/mancenter
            ehcache: # Ehcache configuration
                time-to-live-seconds: 3600 # By default objects stay 1 hour in the cache
                max-entries: 100 # Number of objects in each cache entry
            infinispan: #Infinispan configuration
                config-file: default-configs/default-jgroups-tcp.xml
                # local app cache
                local:
                    time-to-live-seconds: 60 # By default objects stay 1 hour (in minutes) in the cache
                    max-entries: 100 # Number of objects in each cache entry
                #distributed app cache
                distributed:
                    time-to-live-seconds: 60 # By default objects stay 1 hour (in minutes) in the cache
                    max-entries: 100 # Number of objects in each cache entry
                    instance-count: 1
                #replicated app cache
                replicated:
                    time-to-live-seconds: 60 # By default objects stay 1 hour (in minutes) in the cache
                    max-entries: 100 # Number of objects in each cache entry
            # Memcached configuration
            # Uses the Xmemcached library, see https://github.com/killme2008/xmemcached
            memcached:
             # Disabled by default in dev mode, as it does not work with Spring Boot devtools
                enabled: true
                servers: localhost:11211 # Comma or whitespace separated list of servers' addresses
                expiration: 300 # Expiration time (in seconds) for the cache
                use-binary-protocol: true # Binary protocol is recommended for performance (and security)

        # E-mail 属性
        mail:
            enabled: false # If e-mail sending is enabled. The standard `spring.mail` keys will need to be configured
            from: jhipster@localhost # The default "from" address for e-mails
            base-url: http://127.0.0.1:8080 # URL to the application, used inside e-mails

        # Spring Security 配置
        security:
            remember-me: # JHipster secure implementation of the remember-me mechanism, for session-based authentication
                # security key (this key should be unique for your application, and kept secret)
                key: 0b32a651e6a65d5731e869dc136fb301b0a8c0e4
            client-authorization: # Used with JHipster UAA authentication
                access-token-uri: # URL of the JHipster UAA server OAuth tokens
                token-service-id: # ID of the current application
                client-id: # OAuth client ID
                client-secret: # OAuth client secret
            authentication:
                jwt: # JHipster specific JWT implementation
                    # The secret token should be encoded using Base64 (you can type `echo 'secret-key'|base64` on your command line).
                    # If both properties are configured, the `secret` property has a higher priority than the `base64-secret` property.
                    secret: # JWT secret key in clear text (not recommended)
                    base64-secret:  # JWT secret key encoded in Base64 (recommended)
                    token-validity-in-seconds: 86400 # Token is valid 24 hours
                    token-validity-in-seconds-for-remember-me: 2592000 # Remember me token is valid 30 days

        # Swagger 配置
        swagger:
            default-include-pattern: /api/.*
            title: JHipster API
            description: JHipster API documentation
            version: 0.0.1
            terms-of-service-url:
            contact-name:
            contact-url:
            contact-email:
            license:
            license-url:
            host:
            protocols:

        # DropWizard Metrics 配置, used by MetricsConfiguration
        metrics:
            jmx: # Export metrics as JMX beans
                enabled: true # JMX is enabled by default
            # Send metrics to a Graphite server
            # Use the "graphite" Maven profile to have the Graphite dependencies
            graphite:
                enabled: false # Graphite is disabled by default
                host: localhost
                port: 2003
                prefix: jhipster
            # Send metrics to a Prometheus server
            prometheus:
                enabled: false # Prometheus is disabled by default
                endpoint: /prometheusMetrics
            logs: # Reports Dropwizard metrics in the logs
                enabled: false
                reportFrequency: 60 # frequency of reports in seconds

        # Logging 配置, used by LoggingConfiguration
        logging:
            logstash: # Forward logs to Logstash over a socket
                enabled: false # Logstash is disabled by default
                host: localhost # Logstash server URL
                port: 5000 # Logstash server port
                queue-size: 512 # Queue for buffering logs
            spectator-metrics: # Reports Netflix Spectator metrics in the logs
                enabled: false # Spectator is disabled by default

        # By default cross-origin resource sharing (CORS) is enabled in "dev" mode for
        # monoliths and gateways.
        # It is disabled by default in "prod" mode for security reasons, and for microservices
        # (as you are supposed to use a gateway to access them).
        # This configures a standard org.springframework.web.cors.CorsConfiguration
        # Note that "exposed-headers" is mandatory for JWT-based security, which uses
        # the "Authorization" header, and which is not a default exposed header.
        cors:
            allowed-origins: "*"
            allowed-methods: "*"
            allowed-headers: "*"
            exposed-headers: "Authorization"
            allow-credentials: true
            max-age: 1800

        # Ribbon displayed on the top left-hand side of JHipster applications
        ribbon:
            # Comma-separated list of profiles that display a ribbon
            display-on-active-profiles: dev

## <a name="3"></a> 应用专有的属性

Your generated application can also have its own Spring Boot properties. This is highly recommended, as it allows type-safe configuration of the application, as well as auto-completion and documentation within an IDE.

JHipster has generated a `ApplicationProperties` class in the `config` package, which is already preconfigured, and it is already documented at the bottom the `application.yml`, `application-dev.yml` and `application-prod.yml` files. All you need to do is code your own specific properties.
