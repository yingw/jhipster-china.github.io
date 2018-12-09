---
layout: default
title: Docker 和 Docker Compose
permalink: /docker-compose/
redirect_from:
  - /docker_compose.html
sitemap:
    priority: 0.7
    lastmod: 2016-12-01T00:00:00-00:00
---

# <i class="fa fa-music"></i> Docker 和 Docker Compose

## Summary

推荐在开发环境中使用 Docker 或 Docker Compose，即使在生产环境中，docker 也是个非常好的解决方案。


1. [描述](#1)
2. [准备工作](#2)
3. [制作应用的 docker 镜像](#3)
4. [制作多个应用的 Docker-Compose 配置](#docker-compose-subgen)
5. [使用数据库](#4)
6. [Elasticsearch](#5)
7. [Sonar](#6)
7. [Keycloak](#7)
8. [常用命令](#8)
9. [内存调整](#9)

## <a name="1"></a> 描述

_请注意：这里描述的 Docker 设置是为了容器化运行应用的。这和在 [Docker 设置]({{ site.url }}/installation/) 章节描述的不是一个含义，那是指在容器中运行 JHipster 的 generator 环境_

JHipster 提供了完整的 Docker 支持，为了：

- 帮助开发，可以使你非常方便的构建起完整的基础架构，即使在使用了复杂的微服务架构的场景下
- 对于使用了 Docker Swarm 的场景，可以直接部署到生产环境，得益于其使用了相同的 Docker Compose 配置

使用 Docker Compose 的一个好处是你可以非常方便的扩展（scale）容器，使用命令 `docker-compose scale`。这在使用 JHipster 的 [微服务架构](#3) 时非常有趣。

生成一个新项目时，JHipster 已经自动生成好了：

- 一个 `Dockerfile` 用于编译 Docker 以及在容器中启动应用
- 多个 Docker Compose 配置来帮助你启动应用所需的第三方服务，比如数据库

这些文件在 `src/main/docker/` 目录下。

## <a name="2"></a> 准备工作

先安装 Docker 和 Docker Compose：

- [Docker](https://docs.docker.com/installation/#installation)
- [Docker Compose](https://docs.docker.com/compose/install)

Docker now requires creating an account to the docker store to download Docker for Mac and Docker for Windows. To bypass this

<div class="alert alert-info"><i>小提示：</i>

在 Windows 和 Mac OS X 环境下，Kitematic 是个非常方便的图形化管理工具，集成在 Docker Toolbox 里。

</div>

<div class="alert alert-warning"><i>警告：</i>

如果在 Mac 或 Windows 上使用 Docker Machine，Docker 的守护进程只有访问 OS X 或 Windows 文件系统的有限权限。Docker Machine 尝试自动共享 /Users 目录 (OS X) 或 C:\Users\&lt;username&gt; 目录 (Windows)。所以必须将项目建立在该目录下，才能避免一些问题，尤其是当你在使用 <a href="{{ site.url }}/monitoring/">JHipster Console</a> 监控。

</div>

如果在安装 JHipster UML （或者其他 unbundled package）时碰到错误 `npm ERR! Error: EACCES: permission denied`，可能是你的容器没有用 `sudo` 安装（比如，sudo isn't bundled with Ubuntu Xenial).

__解决方案 1__

NPM 的文档建议不要用 root 安装任何包。参考 [官方说明](https://docs.npmjs.com/getting-started/fixing-npm-permissions) 来修复这个问题。

__解决方案 2__

  - `docker container exec -u root -it jhipster bash`,
  - `npm install -g YOUR_PACKAGE`,
  - 退出然后重新进入容器：`docker container exec -it jhipster bash`

## <a name="3"></a> 制作应用的 docker 镜像

制作应用的 Docker 镜像，并且提交到 Docker registry：

- 使用 Maven，执行：`./mvnw package -Pprod verify jib:dockerBuild`
- 使用 Gradle，执行：`./gradlew -Pprod bootWar jibDockerBuild`

这将使用 profile `prod` 打包你的应用，and build a docker image using [Jib](https://github.com/GoogleContainerTools/jib) connecting to the local docker daemon.

在 Windows 上，由于 [lack of named pipes](https://github.com/spotify/docker-client/issues/875)，你需要调整设置，打开 “Expose daemon on tcp://localhost:2375 without TLS”。

运行镜像，可以使用 `src/main/docker` 目录下的 Docker Compose 配置：

- `docker-compose -f src/main/docker/app.yml up`

该命令将会启动你的应用及所需的服务 (database, search engine, JHipster Registry...).

如果你选择了 OAuth 2.0 作为认证方案，请参阅 [Keycloak section on this documentation](#7).

## <a name="docker-compose-subgen"></a> 制作多个应用的 Docker-Compose 配置

如果你的架构由多个 JHipster 应用组成，你可以使用 JHipster 的 `docker-compose` 命令，它会为所有应用生成一个全局的 Docker Compose 配置。这将让你能够只用一条命令就可以部署和扩展你的架构。
要使用 `docker-compose` 命令：

- 将你所有的 monolith(s), 网关以及微服务项目，放在同一个目录下。
- 创建另一个目录，比如 `mkdir docker-compose`。
- 进入：`cd docker-compose`。
- 执行命令：`jhipster docker-compose`。
- 该命令会询问你的架构中的那些项目有哪些，以及是否需要设置 ELK 或 Prometheus。

创建好了全局的 Docker Compose 配置，执行 `docker-compose up` 来启动它，你的所有服务就会一起启动了。

在使用微服务架构的场景中，该配置还会设置个 JHipster Registry 或者 Consul, that will configure your services automatically:

- 所有的服务都会等待直到 JHipster Registry (或 Consul) 启动好。可以通过配置文件 `bootstrap-prod.yml` 中的 `spring.cloud[.consul].config.fail-fast` 和 `spring.cloud[.consul].config.retry` 配置项设置。
- Registry 会设置你的应用，例如它会在所有服务中共享 JWT 秘钥令牌。
- 使用 Docker Compose 来扩展服务，例如执行 `docker-compose scale test-app=4` 来配置名为 "test" 的应用运行 4 个实例。这些实例自动由网关负载平衡，并且会加入同一个 Hazelcast 群（如果你选择了 Hazelcast 作为 Hibernate 二级缓存）。


## <a name="4"></a> 使用数据库

### MySQL, MariaDB, PostgreSQL, Oracle, MongoDB 或 Cassandra

执行 `docker-compose -f src/main/docker/app.yml up` 已经自动启动了数据库。

如果你希望启动自己的数据库，而不是另一个服务，使用这些数据库的 Docker Compose 配置：

- 使用 MySQL: `docker-compose -f src/main/docker/mysql.yml up`
- 使用 MariaDB: `docker-compose -f src/main/docker/mariadb.yml up`
- 使用 PostgreSQL: `docker-compose -f src/main/docker/postgresql.yml up`
- 使用 Oracle: `docker-compose -f src/main/docker/oracle.yml up`
- 使用 MongoDB: `docker-compose -f src/main/docker/mongodb.yml up`
- 使用 Cassandra: `docker-compose -f src/main/docker/cassandra.yml up`
- 使用 Couchbase: `docker-compose -f src/main/docker/couchbase.yml up`

### MongoDB 集群节点

如果你希望使用 MongoDB with a replica set or shards 及一个共享的配置，你需要手动编辑编译 MongoDB 的镜像。
执行以下步骤：

- 编译镜像：`docker-compose -f src/main/docker/mongodb-cluster.yml build`
- 运行数据库：`docker-compose -f src/main/docker/mongodb-cluster.yml up -d`
- 扩展 MongoDB 节点（你需要选择奇数数量的节点数）：`docker-compose -f src/main/docker/mongodb-cluster.yml scale <name_of_your_app>-mongodb-node=<X>`
- 初始化 replica set (parameter X is the number of nodes you input in the previous step, folder is the folder where the YML file is located, it's `docker` by default): `docker container exec -it <yml_folder_name>_<name_of_your_app>-mongodb-node_1 mongo --eval 'var param=<X>, folder="<yml_folder_name>"' init_replicaset.js`
- Init the shard: `docker container exec -it <yml_folder_name>_<name_of_your_app>-mongodb_1 mongo --eval 'sh.addShard("rs1/<yml_folder_name>_<name_of_your_app>-mongodb-node_1:27017")'`
- 编译应用的镜像：`./mvnw package -Pprod verify jib:dockerBuild`
- 启动应用：`docker-compose -f src/main/docker/app.yml up -d <name_of_your_app>-app`

如果需要添加或者移除一些 MongoDB 节点，只需要重复步骤 3 和步骤 4。

### Couchbase 集群模式

如果你想要使用 Couchbase 多节点模式，你需要手动编译和设置 Couchbase 的镜像。
步骤：

- 编译镜像：`docker-compose -f src/main/docker/couchbase-cluster.yml build`
- 运行数据库：`docker-compose -f src/main/docker/couchbase-cluster.yml up -d`
- 扩展 Couchbase 节点（你需要选择奇数数量的节点数）：`docker-compose -f src/main/docker/couchbase-cluster.yml scale <name_of_your_app>-couchbase-node=<X>`
- 编译应用的镜像：`./mvnw package -Pprod verify jib:dockerBuild`
- 启动应用：`docker-compose -f src/main/docker/app.yml up -d <name_of_your_app>-app`

### Cassandra

不像其他数据库，schema migrations 是由应用执行的；Cassandra 的 schema migrations 需要由一个专用的 Docker 容器来执行。

#### <a name="cassandra-in-development"></a>Cassandra 开发环境
要本地启动 Cassandra 集群，可以使用 docker_compose 文件：
`docker-compose -f src/main/docker/cassandra.yml up -d`

Docker-compose 会启动 2 个服务：

- `<name_of_your_app>-cassandra`:  Cassandra node contact point 容器
- `<name_of_your_app>-cassandra-migration`: 自动应用所有 CQL 迁移脚本的容器 (创建 Keyspace, 创建表, 所有的数据迁移, ...)

参考 [使用 Cassandra]({{ site.url }}/using-cassandra/) 来了解更多关于如何不重启直接添加新的 CQL 的方式。

#### Cassandra 生产环境：
`app.yml` docker-compose 文件使用了 `cassandra-cluster.yml` 来配置集群。
应用会在几秒钟 (参考 _JHIPSTER_SLEEP_ 变量) 后启动来给集群足够的时间启动和执行迁移脚本。

在 Cassandra 和其他数据库之间有个很大的差别是，你可以用 Docker Compose 扩展你的集群。为设置 X+1 个节点，可以执行：

- `docker-compose -f src/main/docker/cassandra-cluster.yml scale <name_of_your_app>-cassandra-node=X`

### Microsoft SQL Server

如果要使用 MSSQL 的 Docker 镜像，执行这几步：

- 提示运行 Docker 的 RAM 到至少 3.25GB
- 运行数据库：`docker-compose -f src/main/docker/mssql.yml up -d`
- 用 MSSQL 客户端来创建数据库
- 启动应用：`docker-compose -f src/main/docker/app.yml up -d <name_of_your_app>-app`

## <a name="5"></a> Elasticsearch

执行 `docker-compose -f src/main/docker/app.yml up` 就已经启动了该搜索引擎了。

如果你希望启动自己的 Elasticsearch 节点，而不启动其他服务，使用该 Docker Compose 配置：

- `docker-compose -f src/main/docker/elasticsearch.yml up`

## <a name="6"></a> Sonar

启动 Sonar 的 Docker Compose 配置：

- `docker-compose -f src/main/docker/sonar.yml up`

检查代码，在项目目录里执行：

- With Maven: `./mvnw sonar:sonar`
- With Gradle: `./gradlew sonar`

Sonar 的报告会生成在：[http://localhost:9000](http://localhost:9000)

## <a name="7"></a> Keycloak

如果你选择了 OAuth 2.0 作为认证方案，将 Keycloak 作为默认的身份提供。执行 `docker-compose -f src/main/docker/app.yml up` 来启动 Keycloak。

为使 Keycloak 工作，还需要设置 hosts ( Mac/Linux 的 `/etc/hosts`，或者 Windows 的 `c:\Windows\System32\Drivers\etc\hosts`）。

```
127.0.0.1	keycloak
```

这是由于你通过物理机浏览器访问应用 (名为 localhost，或 `127.0.0.1`)，但在 Docker 内部会运行在它自己的容器里，名称为 `keycloak`。

如果希望独立启动 Keycloak，而不启动其他服务，使用该 Docker Compose 配置：

- `docker-compose -f src/main/docker/keycloak.yml up`

## <a name="8"></a> 常用命令

### 列出所有容器

使用命令：`docker container ps -a` 来列出所有容器（译注：不加 -a 是列出所有运行中容器）

    $ docker container ps -a
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
    fc35e1090021        mysql               "/entrypoint.sh mysql"   4 seconds ago       Up 4 seconds        0.0.0.0:3306->3306/tcp   sampleApplication-mysql

### 容器的状态
`docker container stats` 或 {% raw %}`docker container stats $(docker container ps --format={{.Names}})`{% endraw %} 来列出所有容器以及它们的 CPU，内存，网络 I/O ，及 Block I/O 的状态。

    $ docker container stats {% raw %}$(docker container ps --format={{.Names}}){% endraw %}
    CONTAINER                 CPU %               MEM USAGE / LIMIT     MEM %               NET I/O               BLOCK I/O             PIDS
    jhuaa-mysql               0.04%               221 MB / 7.966 GB     2.77%               66.69 kB / 36.78 kB   8.802 MB / 302.5 MB   37
    00compose_msmongo-app_1   0.09%               965.6 MB / 7.966 GB   12.12%              121.3 kB / 54.64 kB   89.84 MB / 14.88 MB   35
    00compose_gateway-app_1   0.39%               1.106 GB / 7.966 GB   13.89%              227.5 kB / 484 kB     117 MB / 28.84 MB     92
    jhipster-registry         0.74%               1.018 GB / 7.966 GB   12.78%              120.2 kB / 126.4 kB   91.12 MB / 139.3 kB   63
    gateway-elasticsearch     0.27%               249.1 MB / 7.966 GB   3.13%               42.57 kB / 21.33 kB   48.16 MB / 4.096 kB   58
    00compose_jhuaa-app_1     0.29%               1.042 GB / 7.966 GB   13.08%              101.8 kB / 78.84 kB   70.08 MB / 13.5 MB    68
    msmongo-mongodb           0.34%               44.8 MB / 7.966 GB    0.56%               49.72 kB / 48.08 kB   33.97 MB / 811 kB     18
    gateway-mysql             0.03%               202.7 MB / 7.966 GB   2.54%               60.84 kB / 31.22 kB   27.03 MB / 297 MB     37

### 扩展容器

执行 `docker-compose scale test-app=4` 将 "test" 应用扩展到 4 个实例。

### 停止容器

`docker-compose -f src/main/docker/app.yml stop`

也可以用 Docker 的命令：

`docker container stop <container_id>`

停止容器后，数据不会被删除，除非你删了容器。

### 删除容器

小心！所有的数据都会被删除：

`docker container rm <container_id>`


## <a name="9"></a> 内存调整

为了调整优化内存使用，可以设置 Java 的内存参数，在 `Dockerfile` 或 `docker-compose.yml` 内配置

### 添加内存参数到 Dockerfile 配置

设置环境变量：

    ENV JAVA_OPTS=-Xmx512m -Xms256m

### 添加内存参数到 docker-compose.yml 配置

This solution is desired over Dockerfile. In this way, you have a single control point for your memory configuration on all containers that compose you application.

添加 `JAVA_OPTS` 配置到 `environment` 段落下：

```
    environment:
      - (...)
      - JAVA_OPTS=-Xmx512m -Xms256m
```

根据 Docker 的基础镜像，`JAVA_OPTS` 可能不工作。如果这样的话，尝试使用 `_JAVA_OPTIONS` 配置：

```
    environment:
      - (...)
      - _JAVA_OPTIONS=-Xmx512m -Xms256m
```
