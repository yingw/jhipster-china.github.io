---
layout: default
title: 创建实体对象
permalink: /creating-an-entity/
redirect_from:
  - /creating_an_entity.html
sitemap:
    priority: 0.7
    lastmod: 2018-09-04T00:00:00-00:00
---

# <i class="fa fa-bolt"></i> 创建实体对象

_**请查看我们的 [视频教程]({{ site.url }}/video-tutorial/) 来学习创建 JHipster 应用！**_

**重要** 如果你希望“热加载”你的 JavaScript/TypeScript 代码，你需要运行 `npm start` or `yarn start` 。
可以在 [JHipster 开发相关技术]({{ site.url }}/development/) 获取更多信息。

## 介绍

当你创建好了应用，就可以开始创建 _实体对象_ 了。例如，你可能需要创建一个 _Author（作者）_ 和一个 _Book（书籍）_ 的实体对象。
对每个实体对象，你需要：

*   一张数据库表
*   一个 Liquibase 变更记录
*   一个 JPA 实体对象
*   一个 Spring Data JPA Repository
*   一个 Spring MVC REST Controller, 来处理基础的 CRUD 操作
*   一个 Angular router, 一个 component 以及一个 service
*   一个 HTML 视图
*   集成测试，来验证一切运行正常
*   性能测试，来验证一切运行平滑

如果你有多个实体对象，你可能还希望设置它们之间的关系。你需要：

*   一个数据库外键
*   特别的 JavaScript 和 HTML 代码来管理这些关系

使用 "entity" 子命令（sub-generator）来创建所有这些必要的文件，以及这些实体对象的 CRUD 前端 (参考 [Angular 项目结构]({{ site.url }}/using-angular/) 和 [React 项目结构]({{ site.url }}/using-react/))。 该子命令可以这样执行：`jhipster entity <entityName> --[options]`。 
查看相关帮助：`jhipster entity --help`

下面是该命令可支持的选项：

*   `--table-name <table_name>` - 默认情况下 JHipster 创建的表名是和 Entity 名字一致的，如果你希望使用别的名字可以使用这个参数。
*   `--angular-suffix <suffix>` - 如果你希望你的 Angular 路由对象设置个前缀，可以用这个参数。
*   `--client-root-folder <folder-name>` - 设置客户端存放 Entity 的目录名称，默认情况下单体应用是空的，微服务架构是网关应用名称。
*   `--regenerate` - 该参数可以设置重新生成一个 Entity，不询问任何问题。
*   `--skip-server` - 只创建客户端代码，跳过生成服务端代码。
*   `--skip-client` - 只创建服务端代码，跳过生成客户端代码。
*   `--db` - 跳过生成数据库步骤，跳过服务端时无效。

## JHipster UML 和 JDL Studio 工具

这里描述如何用命令行工具创建多个 Entity，如果你有许多 Entity 要创建，建议使用这些图像工具。

有两个选择：

*   [JHipster UML]({{ site.url }}/jhipster-uml/)，用 UML 描述的编辑器。
*   [JDL Studio](https://start.jhipster.tech/jdl-studio/)，这个是我们的一个在线创建 [JDL]({{ site.url }}/jdl/) 的工具。

如果你使用 JDL Studio:

*   你可以使用 `import-jdl` 子命令从一个 JDL 文件来生成实体对象，执行：`jhipster import-jdl your-jdl-file.jh`.

    * 如果你不想重新生成 Entity 代码，可以加上 `--json-only` 标志来跳过创建 Entity，只创建 `.jhipster` 目录下的 json 文件。

    ```
    jhipster import-jdl ./my-jdl-file.jdl --json-only
    ```

    * 默认情况下 `import-jdl` 只创建变化了的 Entity 代码，如果你希望所有的 Entity 都重新生成一遍，可以加上 `--force` 标志。请注意这将会覆盖本地所有和 Entity 相关的代码。

    ```
    jhipster import-jdl ./my-jdl-file.jdl --force
    ```

*   如果你想用 JHipster UML 而不是 `import-jdl`，可以先安装 `npm install -g jhipster-uml`，后执行 `jhipster-uml yourFileName.jh`。

## 实体对象字段

对于每个 Entity，你能定义任意多的字段。你需要输入他们的名称和字段类型，JHipster 会生成所有的代码和配置，包括 Angular 的 HTML 视图和 Liquibase changelog。

注意这些字段不能使用相关技术的保留关键字，比如你使用了 MySQL：

*   不能使用 Java 保留关键字 (否则你的代码编译不了)
*   不能使用 MySQL 保留关键字 (否则你的数据库脚本会更新失败)

## 字段类型

JHipster 支持许多类型。依赖于你的数据库类型，我们用 Java 类型来统一描述它们：Java 的 `String` 会在 Oracle 或 Cassandra 里转换为不同的类型，这由 JHipster 来负责生成正确的数据库脚本。

*   `String`: Java 的字符串。其默认大小依赖于后端 (比如用使用 JPA，默认是 255 个字符)，你可以用一些校验规则来修改（比如说加上 `max` 来设置大小为 1024,）。
*   `Integer`: Java 整形。
*   `Long`: Java 长整型。
*   `Float`: Java 浮点型。
*   `Double`: Java 双精度浮点型。
*   `BigDecimal`: [java.math.BigDecimal](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html) object, used when you want exact mathematic calculations (often used for financial operations).
*   `LocalDate`: [java.time.LocalDate](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html) object, used to correctly manage dates in Java.
*   `Instant`: [java.time.Instant](https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html) object, used to represent a timestamp, an instantaneous point on the time-line.
*   `ZonedDateTime`: [java.time.ZonedDateTime](https://docs.oracle.com/javase/8/docs/api/java/time/ZonedDateTime.html) object, used to represent a local date-time in a given timezone (typically a calendar appointment). 注意 time zone 在 REST 和 persistence 层中不支持，所以尽可能使用 `Instant` 类型。
*   `Boolean`: Java 布尔值。
*   `Enumeration`: Java 枚举对象。相关值是可选的，命令会问希望使用哪些值，会创建相关的 `enum` 类来管理它们。
*   `Blob`: Blob 对象，用来存储二进制对象。当使用这个类型时，命令会问你是要使用普通二进制对象，图片，还是 CLOB (大文本)。图片对象在 Angular 会被特殊处理下，用以正确在客户端显示。

## 验证

可以给每一个字段设置验证规则。根据字段类型，可以设置不同的验证选项。

验证会自动创建在：

*   HTML 视图，使用 Angular 或 React 验证机制
*   Java domain 对象，使用 [Bean Validation](http://beanvalidation.org/)

Bean validation 会在下面这些场景里自动地加入验证：

*   Spring MVC REST controllers (使用 `@Valid` 注解)
*   Hibernate/JPA ( Entity 在保存前会自动验证)

验证信息还用于更精细的数据库字段元数据描述：

*   必填字段会被标识为非空（non-nullable）
*   唯一字段会创建唯一约束
*   字段的最大长度会设置为数据库列的长度

验证也有一些限制：

*   我们没有支持所有 Angular, React 以及 Bean 上的验证，我们只支持那些对于客户端和服务端 API 来说比较常见的验证
*   正则表达式（Regular Expression patterns）在 JavaScript 和 Java 里表现得不太一样，如果你配置了，你可能要做其中一边的一些调整
*   JHipster 创建了单元测试，但并不包括验证逻辑，有可能测试会通不过。如果失败了的话，你可能需要调整单元测试中的测试数据，来让它们正确工作。

## 实体对象之间的关系

实体对象之间的关系只支持关系型数据库。这是个比较复杂的话题，参考文档： [管理关联关系]({{ site.url }}/managing-relationships/).

## 创建单独的 Service 类来处理业务逻辑

拥有单独的 Service 可以处理更加复杂的业务逻辑，比起直接用 Spring REST Controller。使用 Service 层 (使用接口或直接实现) 还能使用 DTO（数据传输对象，参考下面章节）。

参考文档 [创建 Spring service]({{ site.url }}/creating-a-spring-service/)。

## 数据传输对象 (DTOs)

JHipster 默认不会生成 DTOs，但是如果你选择了创建 Service 层（参考上面的章节），他们也是可选的。文档：[使用 DTOs]({{ site.url }}/using-dtos/).

## 过滤

另外，存于关系型数据库的实体对象还可以用 JPA 来实现过滤功能。参考文档：[过滤实体]({{ site.url }}/entities-filtering/)。

## 翻页

请注意如果使用了 [Cassandra]({{ site.url }}/using-cassandra/) 作为数据库，翻页功能就不可用了。这功能将来会提供。

翻页使用了 [the Link header](http://tools.ietf.org/html/rfc5988) 理论，还有 [GitHub API](https://developer.github.com/v3/#pagination)。
JHipster 提供了客户端 (Angular/React) 和服务端 (Spring MVC REST) 的实现。

创建实体时，JHipster 提供了 4 种翻页选项：

*   不翻页 (这个选项，后端不会支持翻页)
*   简单翻页，依据 [the Bootstrap pager](http://getbootstrap.com/components/#pagination-pager)
*   完整翻页，依据 [the Bootstrap pagination component](http://getbootstrap.com/components/#pagination)
*   无限滚动模式（infinite scroll system），依据 [the infinite scroll directive](http://sroze.github.io/ngInfiniteScroll/)

## 更新现有的实体对象

Entity 的配置信息保存在 `.json` 文件里，在 `.jhipster` 文件夹。如果你重新执行子命令，使用一个已经存在的 Entity 名称，你就可以重新生成或更新对象。

当你对一个已存在的 Entity 执行重新生成，你会被问到：'Do you want to update the entity? This will replace the existing files for this entity, all your custom code will be overwritten' （确定更新 Entity 吗？这会覆盖此 Entity 的所有文件，所有的 定制代码都会被覆盖掉），你有这些选项：

*   `Yes, re generate the entity`（是的，重新生成） - 这会重新生成 Entity。提示：也可以在命令上加上 `--regenerate` 标志来直接选择重新生成
*   `Yes, add more fields and relationships`（是的，增加更多的字段和关系） - 接下来会问增加那些字段和关系
*   `Yes, remove fields and relationships`（是的，移除字段和关系） - 接下来会问移除哪些字段或关系
*   `No, exit` - 什么也不做退出

你可能因为以下原因要更新 Entity：

*   添加或删除一些字段和关系
*   重置 Entity 相关代码到原始版本
*   你更新了 JHipster，希望用新版本来重新生成 Entity
*   你更新了 `.json` 配置文件 (格式和前面那些问题很像，所以并不复杂)，你能获取了更新版的  Entity
*   拷贝了新的 `.json` 文件，希望生成一个类似的 Entity

提示：如果要重新生成所有的 Entity，可以用下面的指令 (可以去掉 `--force` 来回答各种问题)。

*   Linux & Mac: ``for f in `ls .jhipster`; do jhipster entity ${f%.*} --force ; done``
*   Windows: `for %f in (.jhipster/*) do jhipster entity %~nf --force`

## 教程

这里有一个简单的教程来创建两个 Entity（作者（Author）和书籍（Book）），他们有一对 one-to-many（一对多）关系。

**重要** 如果你期望得到 "热加载" JavaScript/TypeScript 的能力，你需要运行 `npm start` 或 `yarn start`。
参考 [开发相关技术]({{ site.url }}/development/)。

### 创建 "Author" 实体对象

我们还想要一个一对多关系 (一个作者对象对应多个书籍)，我们先创建作者对象。在数据库基本，JHipster 会在 Book 表上创建外接，指向 Author 表。

`jhipster entity author`

回答接下来的问题来设置字段，Author 具有：

*   一个 "name" 属性， "String" 类型
*   一个 "birthDate" 属性，"LocalDate" 类型

下面的问题涉及到关系，Author 具有：

*   一个 one-to-many 关系关联 "book" entity (目前还没创建)

### 创建 "Book" 实体对象

`jhipster entity book`

回答下面的问题：

*   一个 "title" 属性，"String" 类型
*   一个 "description"，"String" 类型
*   一个 "publicationDate"，"LocalDate" 类型
*   一个 "price" 属性，"BigDecimal" 类型

回答关于关联关系的问题，Book 对象：

*   有个 many-to-one（多对一）关系，指向 Author entity
*   这个关系使用 name 属性 (Author 的) 为显示字段

### 检查生成好的代码

运行测试 `mvn test`，测试 Author 和 Book entity。

启动应用 (可以执行 `mvn`)，登入，选择 entities 菜单下面的 "Author" 和 "Book"。

检查数据库，是否正确插入。

### 改进代码

所创建的代码，包含了所有的 CRUD 操作，如果需求简单的话不需要在做什么修改了。

如果你想要修改代码或者数据库脚本，参考 [开发相关技术]({{ site.url }}/development/)

如果还想要实现更复杂的业务逻辑，你可以添加一个 Spring `@Service` 类，参考 [创建 Spring Service]({{ site.url }}/creating-a-service/)。

### 全部搞定！

已经生成好了所有的 CRUD 页面，这个样子：

![]({{ site.url }}/images/screenshot_5.png)
