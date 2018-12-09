---
layout: default
title: 监控你的 JHipster 应用
permalink: /monitoring/
sitemap:
    priority: 0.7
    lastmod: 2016-03-17T00:00:00-00:00
---
# <i class="fa fa-line-chart"></i> 监控你的 JHipster 应用

JHipster provides several options to monitor your applications at runtime.

## 简述

1. [自动创建的仪表盘](#generated-dashboards)
2. [JHipster Registry](#jhipster-registry)
3. [JHipster Console](#jhipster-console)
4. [将度量值转发给第三方监控系统](#configuring-metrics-forwarding)
5. [Zipkin](#zipkin)
6. [使用 Elastalert 告警](#elastalert)

## <a name="generated-dashboards"></a> 自动创建的仪表盘

针对传统类应用和网关，JHipster 会自动生成几个仪表盘来监控这些应用。这些仪表盘会在运行时工作，并提供一些简单的监控以最简单的方式。

![JHipster 度量页][jhipster-metrics-page]

### 度量仪表盘

度量仪表盘使用 Dropwizard 度量工具来展示应用性能。

它可以监控这些：

- JVM
- HTTP 请求
- 在 Spring Beans 中使用的方法(使用 `@Timed` 注解)
- 数据连接池

点击 JVM 线程度量旁边的眼睛按钮，你将能获取到应用的 stacktrace，对找线程阻塞（blocked threads）非常有帮助。

### health 仪表盘

The health dashboard uses Spring Boot Actuator's health endpoint to give health information on various parts of the application. Many health checks are provided out-of-the-box by Spring Boot Actuator, and it's also very easy to add application-specific health checks.

### 日志仪表盘

日志仪表盘可以将 Logback 配置的应用日志合并显示。点击旁边的按钮可以改变 Java 包上日志的记录等级。这在开发和生产环境中都非常方便有用。

## <a name="jhipster-registry"></a> JHipster Registry

The JHipster Registry has [its own documentation page here]({{ site.url }}/jhipster-registry/).

It mostly provides the same monitoring dashboards as in the previous section, but it works on a separate server. As such, it is a bit more complex to set up, but it is highly recommended to have dashboards running outside of the running application: otherwise, they won't be available when there is an application failure.

## <a name="jhipster-console"></a> JHipster Console

该仪表盘The dashboards described in the previous sections only show the current value of application metrics, when advanced users want to monitor the evolution of those values over time.

Therefore JHipster applications can be configured to forward their metrics to an external monitoring system where they can be graphed over time and analyzed.

To achieve this, JHipster provide the JHipster Console, a custom monitoring solution based on the ELK stack and fully integrated with JHipster.

### 将日志转发到 JHipster Console

要配置 JHipster 应用转发日志到 JHipster Console，需要在 `application-dev.yml` 或 `application-prod.yml` 中打开 logstash 的 logging 参数：

    jhipster:
        logging:
            logstash:
                enabled: true
                host: localhost # 如果使用 Mac 虚拟机或者 Windows 的 docker-machine，在这里配置 Docker 主机 IP
                port: 5000
                queueSize: 512

要配置度量监控，打开度量的日志报告功能：

    jhipster:
        metrics:
            logs:
                enabled: true
        	    reportFrequency: 60 # 秒

设置这些属性就能将你的日志转发到 Dropwizard 度量工具。

### JHipster Console 概况

JHipster Console is a monitoring tool based on the [ELK Stack](https://www.elastic.co/products). It provides ready-to-use dashboards and analytics tools to have a real-time overview of your infrastructure's performance.

It is an Open Source application, available on GitHub at [jhipster/jhipster-console](https://github.com/jhipster/jhipster-console).

The ELK stack is composed of:

- [Elasticsearch](https://www.elastic.co/products/elasticsearch) for indexing the data (logs and metrics)
- [Logstash](https://www.elastic.co/products/logstash) to manage and process the logs received from the applications
- [Kibana](https://www.elastic.co/products/kibana) to visualize the logs with a nice interface

The JHipster Console is a Docker-based project that adds features on top of the official Elasticsearch, Logstash and Kibana Docker images. We have made a few visual changes to Kibana and set up useful dashboards, so that you can get started to monitor your JHipster applications in minutes instead of the hours that would be needed to set up your own monitoring infrastructure.

![JHipster Console Monitoring Dashboard][monitoring-dashboard]

### Monitoring a JHipster microservice architecture

The JHipster Console fully supports the monitoring of a JHipster microservice architecture and even provides the following microservice specific features :

- Distributed tracing with Zipkin
- Log enriching with service name, instance ID, Zipkin correlation IDs
- Zipkin server and UI to visualize traces and spans
- Linking between the Zipkin UI and Kibana so that you can jump to the logs corresponding to a particular trace ID (to use this, click on the <span class="btn btn-primary btn-xs badge">Logs</span> icon in the trace page)

### 安装 JHipster Console

If you already have a JHipster [microservice architecture]({{ site.url }}/microservices-architecture/) set up with the Docker Compose workflow, the JHipster Console can be automatically set up by the Docker Compose sub-generator.

如果你使用的是传统应用，可以使用 JHipster Console 的 Docker-Compose 定义 [GitHub](https://github.com/jhipster/jhipster-console/blob/master/bootstrap/docker-compose.yml) 或者用下面的命令启动：

    curl -O https://raw.githubusercontent.com/jhipster/jhipster-console/master/bootstrap/docker-compose.yml

然后启动之：

    docker-compose up -d

这会一次性启动 Elasticsearch, Logstash, Kibana 以及 ElastAlert。
通过 [http://localhost:5601](http://localhost:5601) 访问 JHipster Console。如果一切配置正确，它就能开始从应用接受日志并转发日志和度量结果到 Logstash 了。

<div class="alert alert-warning"><i> Warning: </i>
如果使用的是 docker-machine 来创建的 Docker 主机，使用 Docker 的主机 IP，例如：http://&lt;docker-host-ip&gt;:5601，而不是 http://localhost:5601。
</div>

停止 Console：

    docker-compose stop

停止后，如果你不再需要了的话可以删除容器：

    docker-compose rm

可以把两条命令合并了使用：`docker-compose down`.

### 使用 JHipster Console

Once your application is running with logs and metrics forwarding enabled, you can view a dashboards by clicking on the **Load Saved Dashboards** icon ( <i class="fa fa-folder-open-o"></i> ) in the **Dashboard** tab.

<div class="alert alert-info">Tip: If you encounter the following error with dashboards: <i>Cannot read property 'byName' of undefined</i>, try refreshing the <b>logstash-*</b> index pattern field list under <b>Settings</b> > <b>Indices</b> using the yellow refresh button (<i class="fa fa-refresh"></i>)</div>

You can also use Kibana's **Discover** and **Visualize** tabs to explore your data and create new visualizations. To understand how to use Kibana's interface effectively please refer to its official documentation in particular the [Discover](https://www.elastic.co/guide/en/kibana/current/discover.html), [Visualize](https://www.elastic.co/guide/en/kibana/current/visualize.html) and [Dashboard](https://www.elastic.co/guide/en/kibana/current/dashboard.html) sections of the Kibana User Guide.

![JHipster Console JVM Dashboard][jvm-dashboard]

### Data persistence with docker volumes

When using JHipster Console you can enable docker volumes in the `docker-compose.yml` file by uncommenting the appropriate lines. Those volumes are used to share data between containers and the host. They will persist data and configuration even if containers are removed from your system.

- Elasticsearch has its data saved to `log-data/`
- Logstash loads its configuration from `log-conf/logstash.conf`, you can edit this file to add new parsing rules for data received by logstash on UDP port 5000.
- Kibana loads dashboards description files in `dashboards/` on each startup.

<div class="alert alert-warning"><i>Warning: </i>
If you are using Docker Machine on Mac or Windows, your Docker daemon has only limited access to your OS X or Windows file system. Docker Machine tries to auto-share your /Users (OS X) or C:\Users\&lt;username&gt; (Windows) directory. So you have to create the project folder under these directory to avoid any issues with volumes.
</div>

### Save your custom searches, visualizations and dashboards as JSON for auto import

Searches, visualization and dashboards created in Kibana can be exported using the **Management** > **Saved Objects** menu.
You can then extract the JSON description of a specific object under the `_source` field of the export.json file.
You can then put this data in a JSON file in one of the `jhipster-console/dashboards` sub-folder for auto-import.

If you have created useful dashboards and visualizations for your JHipster applications please consider contributing those back to the community by submitting a Pull Request on the [JHipster Console's GitHub project](https://github.com/jhipster/jhipster-console).

## <a name="configuring-metrics-forwarding"></a> Forwarding metrics to a supported third party monitoring system (JMX, Prometheus)

JHipster also provides a Metrics exporters for JMX.

Forwarding metrics to alternative systems is also supported and can also simply be enabled in your YAML configuration files.

    jhipster:
        metrics:
            jmx:
                enabled: true

Note: Unlike in previous JHipster versions, JHipster 5 metrics reporting only support JMX out of the box. Please have a look to the Metrics official documentation for instructions on how to setup other reporter like [Graphite](https://metrics.dropwizard.io/4.0.0/manual/graphite.html#manual-graphite).

JHipster also supports [Prometheus](https://prometheus.io/) as a Metrics exporter. Prometheus is disabled by default and can be enabled 
in your YAML configuration file.

    jhipster:
        metrics:
            prometheus:
                enabled: true

This will export your metrics under `/prometheusMetrics`. As this endpoint is not secured, you can protect it with basic auth, such that 
prometheus can still scrape the endpoint by creating a new configuration file (e.g. `BasicAuthConfiguration.java`).

    @Configuration
    @Order(1)
    @ConditionalOnProperty(prefix = "jhipster", name = "metrics.prometheus.enabled")
    public class BasicAuthConfiguration extends WebSecurityConfigurerAdapter {

        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .antMatcher("/prometheusMetrics/**")
                .authorizeRequests()
                .anyRequest().hasAuthority(AuthoritiesConstants.ADMIN)
                .and()
                .httpBasic().realmName("jhipster")
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and().csrf().disable();
        }
    }

You can login with the default `admin/admin`. You must add following configuration to you prometheus configuration such that prometheus can still scrape your application.

    basic_auth:
        [ username: "admin" ]
        [ password: "admin" ]


## <a name="zipkin"></a> Zipkin

JHipster applications can integrate with [Zipkin](http://zipkin.io/) through [Spring Cloud Sleuth](https://cloud.spring.io/spring-cloud-sleuth/) to provide distributed tracing for your microservice architecture. To enable Zipkin tracing, package your application with the `zipkin` maven/gradle profile and set the `spring.zipkin.enabled` property to true. This will trigger span reporting to the Zipkin server and also add correlation IDs (TraceId, SpanId and ParentId) to request headers and logs. The Zipkin server and UI is provided as part of the JHipster Console and integrates with the Kibana dashboard.

Zipkin also provide a service dependency graph feature that lets you visualize the dependencies between microservices over time.

The Zipkin instance should be available on [http://127.0.0.1:9411/](http://127.0.0.1:9411/) if you run it on your machine, or on http://&lt;docker-host-ip&gt;:9411 if you run it with Docker.

## <a name="alerting"></a> Alerting with Elastalert

JHipster Console comes with built-in alerting by integrating [Elastalert](https://github.com/Yelp/elastalert), an alerting system that can generate alerts from data in Elasticsearch. Elastalert is simple to use and able to define complex alerting rules to detect failures, spikes or any pattern based on an Elasticsearch Query.

### Enable alerting

To enable alerting, setup the `jhipster-alerter` container by adding the following lines [`docker-compose.yml`](https://github.com/jhipster/jhipster-console/blob/master/bootstrap/docker-compose.yml).

    jhipster-alerter:
        image: jhipster/jhipster-alerter
        #volumes:
        #    - ../jhipster-alerter/rules/:/opt/elastalert/rules/
        #    - ../alerts/config.yaml:/opt/elastalert/config.yaml

### Configure alerting

Elastalert configuration can be modified in `alerts/config.yaml`. For example, you can configure the alerting frequency and the buffer period, by changing the following properties:

    run_every:
        minutes: 1
    buffer_time:
        minutes: 5

Then you will need to write some rules that define when alerts will be thrown.

### Write alertings rules

To define new alerts, add new Yaml rule files in `alerts/rules` and then test them over past data with:

    ./test-alerting-rule.sh rule.yaml

Note that those Yaml files should have a `.yaml` file extension. Read more on how to write rules at [Elastalert's official documentation](https://elastalert.readthedocs.org/en/latest/ruletypes.html).

[jhipster-metrics-page]: {{ site.url }}/images/jhipster_metrics_page.png "JHipster Metrics page"
[monitoring-dashboard]: {{ site.url }}/images/jhipster-console-monitoring.png "Monitoring Dashboard"
[jvm-dashboard]: {{ site.url }}/images/jhipster-console-jvm.png "JVM Dashboard"