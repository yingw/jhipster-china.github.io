---
layout: default
title: Deploying to Kubernetes
permalink: /kubernetes/
redirect_from:
  - /kubernetes.html
sitemap:
    priority: 0.7
    lastmod: 2018-06-10T00:00:00-00:00
---

# 部署至Kubernetes

这个子生成器允许将您的JHipster应用部署至[Kubernetes](http://kubernetes.io/)。

[![]({{ site.url }}/images/logo/logo-kubernetes.png)](http://kubernetes.io/)

## 限制

- 目前尚不支持Cassandra
- 要求Kubernetes v1.9+

## 先决条件

您必须安装：

- [Docker](https://docs.docker.com/installation/#installation)
- [kubectl](http://kubernetes.io/docs/user-guide/prereqs/)

您必须有一个Docker registry。 如果您没有, 您可以使用官方 [Docker Hub](https://hub.docker.com/)

## Minikube

[Minikube](https://github.com/kubernetes/minikube)是一个可以轻松在本地运行Kubernetes的工具。 Minikube在笔记本电脑上的VM内运行一个单节点Kubernetes集群，以供希望试用Kubernetes或日常开发的用户使用。

您可以在将其推送到[Kubernetes](http://kubernetes.io/)之前使用它来测试您的应用程序。


## 运行子生成器

要为Kubernetes生成配置文件，请在新文件夹中运行此命令：

`jhipster kubernetes`

然后回答所有问题以部署您的应用程序。

### Which *type* of application would you like to deploy?

您的应用程序类型取决于您是希望部署微服务架构还是传统应用程序。


### Enter the root directory where your applications are located

输入路径。

### Which applications do you want to include in your Kubernetes configuration?

选择您的应用程序.

### Enter the admin password used to secure the JHipster Registry admin

仅当您选择微服务架构时，才会显示此问题。

### What should we use for the Kubernetes namespace?

请参阅[here](http://kubernetes.io/docs/user-guide/namespaces/)有关名称空间的文档。

### What should we use for the base Docker repository name?

如果选择[Docker Hub](https://hub.docker.com/)作为 main registry，它将是您的Docker Hub login。


如果您选择 [Google Container Registry](https://cloud.google.com/container-registry/), 则 它将是 `gcr.io/[PROJECT ID]`, or a regional registry, such as `eu.grc.io/[PROJECT ID]`, `us.gcr.io/[PROJECT ID]`, or `asia.gcr.io/[PROJECT ID]`。 有关更多详情，请参阅[Pushing and Pulling Images](https://cloud.google.com/container-registry/docs/pushing-and-pulling)。

### What command should we use for push Docker image to repository?

推送到Docker Hub的默认命令是 `docker image push`

如果您使用Google Container Registry托管Docker images，它将是：`gcloud docker push`

## 更新已部署的应用程序

### 准备新的部署

在已经部署了应用程序之后，可以通过构建新的Docker images来重新部署它：

`./mvnw package -Pprod -DskipTests jib:dockerBuild`

或当使用Gradle时:

`./gradlew -Pprod bootWar jibDockerBuild -x test`

### 推送到 Docker Hub

在本地标记（tag）您的image：

`docker image tag application username/application`

将image推送到 Docker Hub：

`docker image push username/application`

## 部署单体应用（monolith application）

部署您的应用：

`kubectl apply -f application/`

它将为您的应用程序及其关联的依赖服务（database，Elasticsearch ...）以及Kubernetes服务创建一个Kubernetes部署（deployment），以将应用程序暴露给外部。


## 部署微服务应用（microservice application）

在部署微服务之前，请首先部署服务发现（service discovery）服务（JHipster Registry或Consul）。 如果选择了JHipster Console或Prometheus，则建议在微服务之前部署它们。 子生成器（sub-generator）在工程中放置了具有正确执行顺序的README文件。

### 自定义名称空间（namespaces）

可以为整个部署指定自定义名称空间。 要执行自定义命令，必须指定目标名称空间，如以下示例所示：

`kubectl get pods -n <custom-namespace>`

### 扩展部署

您可以使用以下方法扩展应用程序

`kubectl scale deployment <app-name> --replicas <replica-count> `

### 零停机部署


更新Kubernetes中正在运行的应用程序的默认方法是将新的image tag部署到Docker registry中，然后使用以下方法进行部署：

`kubectl set image deployment/<app-name>-app <app-name>=<new-image>`

使用livenessProbes和ReadinessProbe可以使Kubernetes知道应用程序的状态，以确保服务的可用性。 如果要实现零停机部署，则每个应用程序至少需要2个副本（replicas）。 这是因为滚动升级策略首先会杀死正在运行的副本，以便放置新副本。 仅运行一个副本将导致升级期间的短暂停机时间。

### 在Kubernetes中部署Service Registry

尽管Kubernetes通过**Kube-DNS**拥有自己的内部服务发现，但是JHipster依靠Spring Cloud进行服务发现，因此它依赖于第三方service registry，例如Eureka或Consul。 这种方式具有平台独立的优势，并且可以在生产环境和本地开发计算机上类似地工作。

因此，对于微服务应用程序，JHipster Kubernetes子生成器将生成Kubernetes清单文件（manifest files），以部署service registries，例如**JHipster-Registry**（基于Eureka）或**Consul**。 此外，生成的微服务和网关Kubernetes清单将包含适当的配置，以将自身注册到其central registry。

### 在Kubernetes中管理JHipster Registry或者Consul 

对于JHipster Registry and Consul，提供了[StatefulSets](https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/)配置。 这些是Kubernetes的一种特殊资源，可以处理有状态的应用程序，并使您能够扩展service registries以实现高可用性。 有关Eureka和Consul的高可用性的更多信息，请参阅它们各自的文档。

### Kubernetes中的集中配置

还可以使用**Spring Cloud Config Server**（使用JHipster Registry时）或**Consul Key/Value store**（使用Consul时）来设置集中式配置。 默认情况下，两个配置服务器都从Kubernetes [ConfigMap](http://kubernetes.io/docs/user-guide/configmap/)加载其配置，该文件包含以下格式的属性文件：

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: application-config
  namespace: default
data:
  application.yml: |- # global properties shared by all applications
    jhipster:
      security:
        authentication:
          jwt:
            secret: secret
  gateway-prod.yml: |- # gateway application properties for the "prod" profile
    foo:
      bar: foobar
```

默认情况下，配置服务器在开发模式下运行，这意味着YAML属性文件直接从文件系统中读取，并在更改时热重新加载。 对于生产环境，建议根据我们的[JHipster-Registry config server]({{site.url}}/jhipster-registry)和[Consul config server]({{ site .url }}/consul)。

### 暴露 headless services

registry是使用Kubernetes中的无头服务部署的，因此主要服务没有IP地址，并且无法获取节点端口。 您可以使用以下任何一种类型创建辅助服务：

`kubectl expose service jhipster-registry --type=NodePort --name=exposed-registry `

并使用如下命令暴露细节：

`kubectl get svc exposed-registry `

要扩展JHipster registry，请使用

`kubectl scale statefulset jhipster-registry --replicas 3 `

## 监控工具

该子生成器提供监视工具和配置，以供您的应用程序使用。

### JHipster Console

您的应用程序日志可以在JHipster Console（由Kibana支持）中找到。 您可以通过以下方式找到其服务详细信息
`kubectl get svc jhipster-console `

将浏览器指向任何节点的IP，然后使用输出中描述的节点端口。

### Prometheus metrics

如果尚未安装, 请安装[Prometheus operator by CoreOS](https://github.com/coreos/prometheus-operator). 您可以使用以下方法快速部署operator

`kubectl create -f https://raw.githubusercontent.com/coreos/prometheus-operator/master/bundle.yaml`

**hint**: 您可以在我们的[monitoring documentation]({{ site.url }}/monitoring/#configuring-metrics-forwarding)中找到有关如何在应用程序中启用和保护Prometheus指标的更多信息。

可以使用如下命令探索您的应用程序的Prometheus实例

`kubectl get svc prometheus-appname `

## 利用Kubernetes的优势

Kubernetes提供了许多开箱即用的工具来帮助微服务部署，例如：
* Service Registry - Kubernetes `Service` 是一等公民，它通过DNS名称提供服务注册和查找。
* Load Balancing - Kubernetes服务充当L4负载平衡器
* Health Check - Liveness probes and readiness probes 可帮助确定服务的运行状况。
* Configuration - Kubernetes `ConfigMap` 可用于在应用程序外部存储和应用配置。

使用Kubernetes设施有很多好处：
* 简化部署
* 无需额外的Eureka /Consul部署
* 无需Zuul代理/路由请求
* 无需Ribbon

同时，还有一些缺点：
* 没有通过JHipster Registry进行应用程序管理 - 该功能依赖于Spring Cloud的`DiscoveryClient`。 以后可以更新以添加`spring-cloud-kubernetes`
* 不支持本地Docker Compose -您必须使用`minikube`进行本地开发，并使用Ingress路由流量
* 没有请求级别的负载平衡 - Kubernetes Service是一个L4负载平衡器，可对每个连接进行负载平衡。 使用Istio进行请求级别的负载平衡（请参阅下文）。

### 使用Kubernetes作为Service Registry

为了避免依赖Eureka或Consul，您需要完全禁用服务发现
* When asked `Which service discovery server do you want to use?`, simply choose `No service discovery`

JHipster网关通常在API调用之前，并使用`Zuul`路由这些调用。 如果没有 a service registry, 则无法通过`Zuul`路由。您将需要使用Kubernetes的`Ingress`将流量路由到微服务。
* When asked `Choose the kubernetes service type for your edge services`, choose `Ingress`.

## Istio

您可以将微服务部署到启用了[Istio](https://istio.io)的Kubernetes集群中。在Kubernetes管理微服务部署和配置的同时，Istio可以管理服务之间的通信，例如请求级负载平衡，重试，断路器，流量路由/拆分等。


启用Istio支持:
* When asked `Do you want to configure Istio?`, choose one of the Istio options
* When asked `Do you want to generate Istio route files`, choose `Yes` to generate default configuration for circuit breaking, etc.

## 故障排除

> My applications don't get pulled, because of 'imagePullBackoff'

检查您的Kubernetes集群正在访问的registry. 如果您使用的是private registry, 则应通过 `kubectl create secret docker-registry` 将其添加到命名空间中(检查 [docs](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/) 获取更多信息)。

> My applications get killed, before they can boot up

如果您的群集资源不足（例如Minikube），则会发生这种情况。 增加部署的livenessProbe的`initialDelySeconds`值。

> My applications are starting very slow, despite I have a cluster with many resources

默认设置针对中规模集群进行了优化。 您可以随意增加JAVA_OPTS环境变量，资源请求和限制以提高性能。 请谨慎操作！

> I have selected Prometheus but no targets are visible

这取决于Prometheus运营商的设置以及集群中的访问控制策略。 要使RBAC设置正常工作，需要版本1.6.0+。

> I have selected Prometheus, but my targets never get scraped

这意味着您的应用程序可能不是使用Maven / Gradle中的`prometheus`配置文件构建的。

> My SQL-based microservice are stuck during Liquibase initialization when running multiple replicas

有时数据库changelog lock被破坏。 您将需要使用`kubectl exec -it`连接到数据库，并删除所有liquibases数据库`databasechangeloglock`表中的行。


## 更多信息

*   [Kubernetes documentation](http://kubernetes.io/docs/)
