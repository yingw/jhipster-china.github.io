---
layout: default
title: 使用 Elasticsearch
permalink: /using-elasticsearch/
redirect_from:
  - /using_elasticsearch.html
sitemap:
    priority: 0.7
    lastmod: 2017-04-16T00:00:00-00:00
---

# <i class="fa fa-search"></i> 使用 Elasticsearch

Elasticsearch 是一个数据库上增加搜索能力的可选项。

当然这个功能有些限制：

*   它只能和 SQL 数据库以及 MongoDB 一同工作。 Cassandra 和 Couchbase 的支持将来会被加入（寻求帮助！）
*   在你的数据库和 Elasticsearch 之间是没有一致性保障的，所以你可以会得到过时的（out-of-sync）数据。这是正常的，因为 Elasticsearch 并不是一个真正意义上的数据库。作为应对，你可能需要编写一些功能来同步你的数据，比如使用 Spring 的 `@Scheduled` 注解，在晚上运行。
    *   这也意味着如果你的数据库如果在你的应用以外的地方修改了，你的搜索索引也会过时。有个 JHipster 的插件 [Elasticsearch Reindexer](https://www.jhipster.tech/modules/marketplace/#/details/generator-jhipster-elasticsearch-reindexer) 可以帮助你解决这个问题。

当使用了 Elasticsearch：

*   默认开启 Spring Data Elasticsearch，以及 [Spring Data Jest](https://github.com/VanRoy/spring-data-jest)。Spring Data Jest 可以让应用使用 REST API 访问 Elasticsearch。它会禁用 Spring Boot 的自动配置并使用它自己的配置方式。
*   "repository" 包下面多出来一个包："search"，里面是 ElastiSearch 数据访问方法。
*   "User" 对象在 Elasticsearch 中被索引好了，你可以用 `/api/_search/users/:query` REST 接口访问查询方法。
*   使用 [entity 命令]({{ site.url }}/creating-an-entity/) 时，生成的对象自动被 Elasticsearch 索引，以及 REST 访问端点。在 Angular/React 的界面上也加入了搜索功能，你就能在主界面进行对象查询了。

### 在开发环境中使用

在开发过程中，JHipster 运行了一个嵌入式的 Elasticsearch 实例。你也可以使用外部 Elasticsearch，只需要设置 `SPRING_DATA_JEST_URI` 环境变量 (或者在 `application-dev.yml` 配置文件中添加 `spring.data.jest.uri` 属性)。

还有更简单运行外部 Elasticsearch 的方式，使用项目提供的 Docker Compose 配置启动一个实例:

    docker-compose -f src/main/docker/elasticsearch.yml up -d

然后设置环境变量来指向它：

    export SPRING_DATA_JEST_URI=http://localhost:9200

### 在生产环境中使用

在生产环境中，JHipster 期望使用外部的 Elasticsearch。默认配置下，应用会往 localhost 寻找 Elasticsearch。这个地址可以在 Spring Boot 配置文件 `application-prod.yml` 中进行设置。

### 在 Heroku 上使用

在 Heroku 上，使用 [Bonsai Elasticsearch](https://elements.heroku.com/addons/bonsai) 可作为附加设置。JHipster 已做了设置自动与其通讯。
