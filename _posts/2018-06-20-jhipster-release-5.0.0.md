---
layout: default
title: Release 5.0.0（JHipster v5.0.0 正式发布）
---

JHipster v5.0.0 发布
==================

JHipster 5 第一个稳定版正式发布了！

下面是我们 JHipster 5.0.0 前 4 个 beta 版本的 release notes。

## 前端

- 支持了 React [#6044](https://github.com/jhipster/generator-jhipster/issues/6044) (已经不是实验性特性了)
    - 同样支持了 Angular 的特性 (包括下面提到的对话框和目录结构)
    - 使用 Redux 做状态管理 
    - 使用 React Router v4 做路由
    - 使用 Typescript
    - 使用 Webpack 4 编译，和之前的 Angular 设置类似
    - Jest + Mocha + Chai 单元测试以及极高测试覆盖率
- 增强了 Angular 支持
    - 升级到 Angular 6，感谢 [William Marques](https://github.com/wmarques) in [#7582](https://github.com/jhipster/generator-jhipster/pull/7582)
    - 包含迁移到 Webpack 4, 带来更佳的性能表现 (it varies depending on your situation, but you can expect a noticeable positive impact) [#7186](https://github.com/jhipster/generator-jhipster/pull/7186)
    - admin 模块延迟加载 [#7235](https://github.com/jhipster/generator-jhipster/pull/7235)
    - 重新设计了实体的 创建/编辑 页面，它们现在是普通页面而不再是弹出页面了 [#7368](https://github.com/jhipster/generator-jhipster/pull/7368)
    - 优化了 AOT （Ahead of Time，预编译）技术
    - 实体对象的目录结构改进了，尤其是微服务目前根据服务分组了。增加了个 `--skip-ui-grouping` 标志来保留原来的结构 [#7079](https://github.com/jhipster/generator-jhipster/pull/7079)
- 使 Webpack 4 同时支持 Angular 和 React，编译更快。
- 支持了 [Jest](https://facebook.github.io/jest/) ，替代了原来的 Puppeteer (以及 PhantomJS) ，Angular 和 React 都将安装和测试得更快 (以及并行测试)。这同时解决了 CI 做前端测试时的一些问题 (尤其是和 Jenkins 一起使用时)，不再需要本地包安装。参考 [#7636](https://github.com/jhipster/generator-jhipster/pull/7636) and [#7663](https://github.com/jhipster/generator-jhipster/pull/7663) from [William Marques](https://github.com/wmarques).
- 支持了 Prettier [#6906](https://github.com/jhipster/generator-jhipster/pull/6906)
    - Angular 和 React 现在开始使用 Prettier 来做代码格式化。
    - 同时增加了 [Husky](https://github.com/typicode/husky) 并 list-staged 来做提交检测钩子
    - 生成应用时请使用 `skip-commit-hook` 来禁用提交检测
    - Prettier 也配置检测了 CSS 和 SCSS 文件 [#7451](https://github.com/jhipster/generator-jhipster/issues/7451).
- 更新到 Font Awesome 5 [#7516](https://github.com/jhipster/generator-jhipster/issues/7516).
- 移除了 AngularJS
    - 目前我们关注在 Angular 5+ 了，我们已经移除了老的 AngularJS 1.x 支持
    - 连带的移除了 Bower 和 Gulp 支持

## 后端

- 支持了 Spring Boot 2.0.0 [#7061](https://github.com/jhipster/generator-jhipster/pull/7061)
    - 所有的 Spring 组件都更新了，包括 Spring Data, Spring Security 和 Spring Cloud
    - Spring Boot 的相关设置也更新了
- 概述了 REST 支持，删除了用注解 `@PutMapping` 来创建实体 [#7425](https://github.com/jhipster/generator-jhipster/issues/7425).
- 更好地支持了 OAuth2，感谢 [Fabien Arrault](https://github.com/farrault) ，由 [Matt Raible](https://github.com/mraible) 集成。参考 [#7666](https://github.com/jhipster/generator-jhipster/pull/7666).
- 迁移了 swagger-codegen 到 openapi-generator (新组件是 swagger-codegen 的社区分支) 由 [Christophe Bornet](https://github.com/cbornet) 支持，他也是 JHipster 核心组成员以及 openapi-generator 团队成员。参考 [#7728](https://github.com/jhipster/generator-jhipster/pull/7728).
- 支持了 Memcached，作为 Spring Cache 实现的一个可选项。这个在 Heroku, GCP and AWS 等平台上使用比用 Ehcache/Hazelcast/Infinispan 更方便。

## 插件（Sub-generators）和工具

- JDL v2 支持了创建应用 [#7339](https://github.com/jhipster/generator-jhipster/pull/7339)
    - 所以语言也进化了，现在可以通过 JDL 来创建引用了，而不只是实体对象。这对于希望分享和重用 JHipster 配置的场景来说是个大消息。
- 新的 JHipster 蓝图（blueprints）
    - 蓝图系统使得扩展或替换 JHipster 模板更方便了，新的 [JHipster Kotlin](https://github.com/jhipster/jhipster-kotlin) 工作于此。目前文档还没有完整，但是 JHipster Kotlin 已经给出了例子。
- 新的之命令（Sub-generator）来发布到 AWS 容器 [#7035](https://github.com/jhipster/generator-jhipster/pull/7035)
- 初级的 [Istio](https://istio.io/) 支持 [#7337](https://github.com/jhipster/generator-jhipster/issues/7337)，[#7695](https://github.com/jhipster/generator-jhipster/pull/7695) 由 Google 的 [Ray Tsang](https://github.com/saturnism) 增加，以及 [#7697](https://github.com/jhipster/generator-jhipster/pull/7697) by [Srinivasa Vasu](https://github.com/srinivasa-vasu). 以及 [Pierre Besson](https://github.com/PierreBesson).
- 支持了 Google App Engine 上部署 monoliths，由 Google 的 [Ray Tsang](https://github.com/saturnism) 提供，参考 [#7765](https://github.com/jhipster/generator-jhipster/pull/7765).

所有的 tickets 以及 pull requests
------------
如同以往， __[请在这里查看所有的 tickets 和 pull requests](https://github.com/jhipster/generator-jhipster/issues?q=milestone%3A5.0.0+is%3Aclosed)__.

如何更新
------------

**自动更新**

自动更新参考 [JHipster upgrade sub-generator]({{ site.url }}/upgrading-an-application/)，在一个已有项目上：

升级 JHipster 版本：

```
yarn global upgrade generator-jhipster
```

运行之命令：

```
jhipster upgrade
```

**手动升级**

要手动升级，首先升级 JHipster 版本：

```
yarn global upgrade generator-jhipster
```

如果已有项目，还继续使用原来的 JHipster 版本。
要升级项目，首先删除 `node_modules` 目录，然后运行：

```
jhipster
```

也可以升级整个项目以及实体对象：

```
jhipster --with-entities
```

也可以一个个升级实体对象，比如升级对象 _Foo_

```
jhipster entity Foo
```

帮助和问题
--------------

如果发现此版本的问题，请：

- 提交 Issue [bug tracker](https://github.com/jhipster/generator-jhipster/issues?state=open)
- 提问在 [Stack Overflow](http://stackoverflow.com/tags/jhipster/info)

如果是一个非常紧急或安全问题，请：

- 联系 [@java_hipster](https://twitter.com/java_hipster) on Twitter
