---
layout: default
title: Consul
permalink: /consul/
sitemap:
    priority: 0.7
    lastmod: 2017-05-03T00:00:00-00:00
---

# <i class="fa fa-bullseye"></i> Consul

## Consul 概述

作为 JHipster Registry 的一个替代，你可以使用 [Consul](https://www.consul.io/)，一个由 Hashicorp 提供的数据中心解决方案。
比较与 Eureka，它拥有很多优势：

- 比 Eureka 更容易在多节点集群架构上维护。
- 它比起可用性更关注于强一致性（译注：CA），这使得它的集群状态的变化可以更快的传播。
- Consul 服务发现可以和现有系统更方便地集成，通过 [DNS 接口](https://www.consul.io/docs/agent/dns.html) 或 [HTTP API](https://www.consul.io/docs/agent/http.html)。

## 架构

<img src="{{ site.url }}/images/microservices_architecture_detail.003.png" alt="Diagram" style="width: 800; height: 600" class="img-responsive"/>

## 开始

开始开发一个依赖 Consul 注册中心的应用，你需要先在 docker 容器中启动一个 Consul 实例：

- 运行 `docker-compose -f src/main/docker/consul.yml up` to start a Consul server in `dev` mode. Consul will then be available on port `8500` of your Docker host, so if it runs on your machine it should be at [http://127.0.0.1:8500/](http://127.0.0.1:8500/).

You can also use the [Docker Compose subgenerator]({{ site.url }}/docker-compose/#docker-compose-subgen) to generate a docker configuration for several consul-enabled applications.

## 应用的 Consul 设置

If you have chosen the Consul option when generating your JHipster microservice or gateway app, they will be automatically configured to retrieve their configuration from Consul's **Key/Value store**.

The Key/Value (K/V) store can be modified using either its UI available at [http://localhost:8500/v1/kv/](http://localhost:8500/v1/kv/) or its [REST API](https://www.consul.io/intro/getting-started/kv.html). However changes made this way are temporary and will be lost on Consul server/cluster shutdown. So, in order to help you interact easily with the Key/Value store and manage your configuration as simple YAML files, the JHipster Team has developed a small tool: the [consul-config-loader](https://github.com/jhipster/consul-config-loader). The **consul-config-loader** is automatically configured when starting Consul from the `consul.yml` docker-compose file but it can also be run as a standalone tool.
It can be run in two modes:

- a **dev** mode, where YAML files from the `central-server-config` directory are automatically loaded into Consul. Moreover any change to this directory will be immediately synchronized with Consul.
- a **prod** mode, that uses Git2Consul to setup the YAML files contained in a Git repository as a configuration source for the Key/Value store.

Note that as with the JHipster Registry, your configuration files will need to be named `appname-profile.yml` where appname and profile correspond to the application’s name and profile of the service that you want to configure. For example, adding properties in a `consulapp-prod.yml` file will set those properties only for the application named `consulapp` started with a `prod` profile. Moreover, properties defined in `application.yml` will be set for all your applications.
