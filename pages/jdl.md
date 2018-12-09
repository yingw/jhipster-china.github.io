---
layout: default
title: JHipster 领域模型语言
permalink: /jdl/
sitemap:
    priority: 0.5
    lastmod: 2018-06-29T12:00:00-00:00
---

# <i class="fa fa-star"></i> JHipster 领域模型语言 (JDL 语法)


JDL 是 JHipster 特质的 domain 语言，提供了能在一个文件（或多个）中来描述整个应用、部署、实体对象及其关系的能力，并且只需要很简单的、友好的语法。

可以使用我们的在线工具 [JDL-Studio](https://start.jhipster.tech/jdl-studio/) 或者插件 [JHipster IDE](https://www.jhipster.tech/jhipster-ide/) ，支持多个平台： [Eclipse](https://marketplace.eclipse.org/content/jhipster-ide), [VS Code](https://marketplace.visualstudio.com/items?itemName=jhipster-ide.jdl) 以及 [Atom](https://atom.io/packages/ide-jhipster)  来创建 JDL 及其 UML 视图。你还可以创建、导出、或分享你的模型。

You can generate applications, deployments and entities from a JDL file using the `import-jdl` sub-generator, by running `jhipster import-jdl your-jdl-file.jdl`. 

一旦你创建了项目 (用 `jhipster import-jdl` 或者 `jhipster` 命令工具创建), 接下来你可以使用命令 `jhipster import-jdl your-jdl-file.jdl` 来从 JDL 文件生成实体对象 (确保在 JHipster 项目目录下执行)。

你还能用 [JHipster UML]({{ site.url }}/jhipster-uml/) 来创建实体对象或引用并导出为 JDL 文件，执行 `jhipster-uml your-xmi-file.xmi --to-jdl`  (确保在 JHipster 项目目录下执行)。要了解更多关于如何安装和使用 JHipster UML, 访问 [JHipster UML documentation]({{ site.url }}/jhipster-uml/).

这可以作为 [entity sub-generator]({{ site.url }}/creating-an-entity/) 工具的替代，并且是推荐这样做的。
核心思想是使用图形工具比使用传统的 Yeoman 问答来 [管理关联关系]({{ site.url }}/managing-relationships/) 要简单的多。

JDL 项目在 [GitHub 地址](https://github.com/jhipster/jhipster-core/), 它和 JHipster 一样是开源的并且遵循 Apache 2.0 开源协议。
It can also be used as a node library to do JDL parsing.

_如果你喜欢 [JHipster Domain Language](https://github.com/jhipster/jhipster-core/),  [JDL Studio](https://github.com/jhipster/jdl-studio/) 或 [JHipster IDE](https://github.com/jhipster/jhipster-ide/) 欢迎给我们点赞加星 [GitHub](https://github.com/jhipster/) - 谢谢！_

***

下面是完整的 JDL 文档：

1. [JDL 样例](#sample)
1. [如何使用](#howtojdl)
1. [语法](#jdllanguage)
   1. [声明应用](#applicationdeclaration)
   1. [声明实体对象](#entitydeclaration)
   1. [声明关联关系](#relationshipdeclaration)
   1. [枚举](#enumerationdeclaration)
   1. [Blob 类型](#blobdeclaration)
   1. [可选声明](#optiondeclaration)
   1. [微服务相关选项](#microserviceoptions)
   1. [注解](#annotations)
   1. [声明部署](#deploymentdeclaration)
1. [备注](#commentingjdl)
1. [所有的关联](#jdlrelationships)
1. [常量](#constants)
1. [工作流](#workflows)
1. [附录](#annexes)
   1. [应用选项](#application_options)
   1. [可选的部署选项](#deployment_options)
   1. [支持的类型和约束](#types_and_constraints)
   1. [其他选项](#all_options)
1. [错误和 Bug](#issues)

***

# <a name="sample"></a> JDL 样例

我们将 Oracle 的 "人力资源" 样例程序翻译成了 JDL，放在 [这里](https://github.com/jhipster/jdl-samples/blob/master/Oracle-Human-Resources-sample.jdl).
并且设置为默认加载的应用在 [JDL-Studio](https://start.jhipster.tech/jdl-studio/) 以及 [JHipster IDE](https://www.jhipster.tech/jhipster-ide/) 

如果你想了解更多例子，这里有些例子：[here](https://github.com/jhipster/jdl-samples).

## <a name="howtojdl"></a> 如何使用

你可以使用 JDL 文件来生成实体对象：
  - 创建一个后缀为 '.jh' 或 '.jdl' 的文件，
  - 声明应用、部署、实体对象和关联关系，或者用 [JDL-Studio](https://start.jhipster.tech/jdl-studio/) 或者 [JHipster IDE](https://www.jhipster.tech/jhipster-ide/) 开发下载该文件，
  - 如果你创建了实体对象，就在应用的根目录中，执行： `jhipster import-jdl my_file.jdl` 
  - 如果你创建的是应用，就在一个目录里执行： `jhipster import-jdl my_file.jdl` 

*欢呼吧*, 就行了！

[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in  the output file unless you use it in)
[comment]: <> (a reference style link.)
[//]: <> (This is also a comment.)
[//]: # (This may be the most platform independent comment)


如果你在团队内工作，有可能需要多个文件而不是一个。
我们也支持这样使你不需要手工把它们拼在一起，
你只需要执行：

    jhipster import-jdl my_file1.jdl my_file2.jdl

如果你不想创建实体对象类，而只是需要 JSON 文件，你可以在导入时加上参数：`--json-only`，这样就只会在 `.jhipster` 目录里创建 json 文件，而不会创建那些实体对象了。

    jhipster import-jdl ./my-jdl-file.jdl --json-only

默认情况下，`import-jdl` 命令只会生成发生了改变的实体对象, 如果你需要所有的实体对象都重新生成一遍，
可以加上参数：`--force`。
请注意这会覆盖你在这些对象上做的所有变更。

    jhipster import-jdl ./my-jdl-file.jdl --force

如果你希望在项目内使用，你可以执行:
  - NPM: `npm install jhipster-core --save`
  - Yarn: `yarn add jhipster-core`

来将之安装在本地，并保存在你的 `package.json` 文件里。

---

## <a name="jdllanguage"></a> 语法
我们尽量将语法设计的对开发人员友好，你可用用它来：
  - 定义应用和它的属性以及实体，
  - 定义实体对象和它们的属性，
  - 定义它们之间的关联关系，
  - 定义一下 JHipster 的选项。

如果你需要了解 JDL 的语法，这里有个文档
[here](https://github.com/jhipster/jhipster-core/blob/master/lib/dsl/gen/grammar.html).


### <a name="applicationdeclaration"></a> 声明应用

从 v2.0.0 开始, application declaration is possible (compatible with JHipster v5).

_To import one or several applications, you needn't be in a JHipster application folder._

The most basic declaration is done as follows:

```
application {
  config {}
}
```

A JHipster application has a config with default values and using the previous syntax will
ensure your application use the default values (as if you didn't make any specific choice).
The resulting application will have:
  - baseName: `jhipster`
  - applicationType: `monolith`
  - databaseType: `sql`
  - etc.

Now, if you want some custom options:

```
application {
  config {
    baseName myapp
    applicationType microservice
    prodDatabaseType postgresql
    buildTool gradle
  }
}
```

Those options are only a sample of what's available in the JDL.
The complete list of options is available in the annexes, [here](#annexes).

If you want more than one application, here's how you do it:

```
application { // will be generated under the 'myFirstApp' folder
  config {
    baseName myFirstApp
  }
}

application { // will be generated under the 'mySecondApp' folder
  config {
    baseName mySecondApp
    applicationType microservice
  }
}
```

You can have as many application as you want in as many file as you wish: there's no limitation.

Declaring entities is the most basic thing available and now you can set what entity should be generated
in the applications you want.

Let's improve the previous example:

```
application {
  config {
    baseName myMonolith
    applicationType monolith
  }
  entities * except C, D
}

application {
  config {
    baseName myGateway
    applicationType gateway
    serverPort 9042
  }
  entities * except A, B
}

application {
  config {
    baseName microserviceA
    applicationType microservice
  }
  entities C
}

application {
  config {
    baseName microserviceB
    applicationType microservice
    serverPort 8082
  }
  entities D
}

entity A
entity B
entity C
entity D

dto * with mapstruct
service * with serviceClass
paginate D with pager
```

Now, several things will happen when generating these applications and folders:
  - Four applications will be created:
    - myMonolith in `./myMonolith`, with the server port `8080`
    - myGateway in `./myGateway`, with the server port `9042`
    - microserviceA in `./microserviceA`, with the server port `8081`
      - Even though we didn't specify a server port, JHipster sets one by default.
      - For microservices, the default one is `8081`
      - For gateways and monoliths, it's `8080`
      - For UAA apps, it's `9999`
    - microserviceB in `./microserviceB` with the server port `8082`
  - Four entities will be generated
    - `A` and `B` in the monolith
    - `C` and `D` both in the gateway
      - `C` in the first microservice
      - `D` in the second microservice
  - The `microservice` option is implicit for `C` and `D`
    - Because they get generated on the two microservices, this option will be set by default.
  - Options work the same way as before

Note that the generator sets default values if they aren't present (like the `databaseType`).
JHipster Core does the exact same things.


### <a name="entitydeclaration"></a> 声明实体对象

实体对象的声明如下格式：

    entity <entity name> {
      <field name> <type> [<validation>*]
    }

  - `<entity name>` 是实体对象的名称，
  - `<field name>` 实体对象的属性，
  - `<type>` JHipster 支持的属性类型，
  - 可选的 `<validation>` 类型的校验规则。

所有的属性类型和校验规则在 [这里](#annexes)，
如果校验规则需要一个值，还要加上 `(<value>)` 在校验规则的右边。


一些 JDL 的例子：

```
entity A
entity B
entity C
entity D {
  name String required
  address String required maxlength(100)
  age Integer required min(18)
}
```

正则表达式的写法稍微特别些 (v1.3.6 版本开始支持):
```
entity A {
  myString String required minlength(1) maxlength(42) pattern(/[A-Z]+/)
}
```
如果你在使用 v4.9.X 之前的版本，你得这样使用正则表达式：`pattern('[A-Z]+')`.

JDL 设计的易用可读，如果你的实体是空的（没有属性），你可以这样声明：`entity A` 或 `entity A {}`。

请注意 JHipster 会增加一个默认的 `id` 字段，所以你可以不用管它。


### <a name="relationshipdeclaration"></a> 声明关联关系

关联关系的声明如下格式：

    relationship (OneToMany | ManyToOne | OneToOne | ManyToMany) {
      <from entity>[{<relationship name>[(<display field>)]}] to <to entity>[{<relationship name>[(<display field>)]}]
    }

  - `(OneToMany | ManyToOne| OneToOne | ManyToMany)` 是关系的类型，
  - `<from entity>` 是关系所有者的名称、来源，
  - `<to entity>` 是关系所指向的目标，
  - `<relationship name>` 是关系在另一边对象内属性的名称，
  - `<display field>` 是关系在选择列表中显示的属性字段 (默认是: `id`),
  - `required` 是指属性是否是必填的。

这是个例子：

    entity Book
    entity Author

    relationship ManyToOne {
      Book to Author
    }

Declaring an injected field is optional, as one (or both) is set by default as needed.
The previous example is equivalent to this one:

    entity Book
    entity Author

    relationship ManyToOne {
      Book{author} to Author
    }

让我们来做的更复杂点。
一本书，有一个必填的作者，一个作者，有多本书。

    entity Book
    entity Author {
      name String required
    }

    relationship OneToMany {
      Author{book} to Book{writer(name) required}
    }
    
在这个 `Book` 例子中，它有一个 **必填** 字段 `writer` 关联到 `Author` 的 `name` 属性。

当然，在真实场景中，你会需要些一堆的关系并且不断重复写上面那三行代码感觉有点傻。
所以，你可以这样合并了声明：

```
entity A
entity B
entity C
entity D

relationship OneToOne {
  A{b} to B{a}
  B{c} to C
}
relationship ManyToMany {
  A{d} to D{a}
  C{d} to D{c}
}
```

这些关系一般使用 `id` 字段作为关联属性，同时它也是在前端编辑这个关系时默认显示的字段。如果你希望显示其他的字段，可以这样设置：
（译注：这里应该指显示和关联都用这个字段，需要唯一；如果只需要显示，还得手动修改 global 里的 other 字段重新生成）

```
entity A {
  name String required
}
entity B


relationship OneToOne {
  A{b} to B{a(name)}
}
```

这样的话 JHipster 会产生一组 REST 资源到前端并包含 `id` 和 `name` 字段，这样 name 属性就可以正常的显示给用户了。

### <a name="enumerationdeclaration"></a> 枚举

在 JDL 里声明枚举类型的语法：

- 声明枚举：

        enum Language {
          FRENCH, ENGLISH, SPANISH
        }

- 在实体对象中，使用枚举作为字段的属性：

        entity Book {
          title String required,
          description String,
          language Language
        }


### <a name="blobdeclaration"></a> Blob 类型 (byte[])
JHipster 提供了用户选择图像类型或者二进制类型。JDL 同样支持：
需要在编辑器中创建一个客制化类型 (参考数据类型章节)，用以下规约来命名：
  - `AnyBlob` 或 `Blob` 类型来创建适合任意二进制类型的字段；
  - `ImageBlob` 类型来创建用于指代图片的字段；
  - `TextBlob` 类型创建 CLOB (长文本)字段。

你可以创建任意多类型。


### <a name="optiondeclaration"></a> 声明的可选项

JHipster 中，你可以定义额外的选项来描述类似翻页或 DTO 等功能。
你可以这样：

    entity A {
      name String required
    }
    entity B
    entity C

    dto A, B with mapstruct

    paginate A with infinite-scroll
    paginate B with pagination
    paginate C with pager  // pager is only available in AngularJS

    service A with serviceClass
    service C with serviceImpl

关键字 `dto`, `paginate`, `service` 还有 `with` 来支持这些特性。
如果定义了错误的选项，JDL 有一个红色消息的通知并将之忽略在 JHipster 的 JSON 文件之外。

#### Service 选项

不定义 Service 将会让创建的资源类直接调用 repository 接口。
这个是默认选项，参考下面的 A 对象。
定义 Service 并且配置 serviceClass (参考 B 对象) 将会让资源类调用 service 类，service 类再调用 repository 接口。
定义 Service 并配置 serviceImpl (参考 C) 将会让资源类调用一个 service 接口。
该接口会有一个实现类，实现类再调用 repository 接口。

如果你不清楚使用哪种，就不要定义 service，这是最简单的 CRUD 实现方式。
如果你的业务逻辑非常复杂且调用多个 repository，实现一个 serviceClass 是理想方式。
Jhipster 不喜欢不必要的接口方式，不过如果你喜欢你还是可以实现他们。

    entity A
    entity B
    entity C

    // no service for A
    service B with serviceClass
    service C with serviceImpl


JDL 还支持批量选项设置。可以这么做：

    entity A
    entity B
    ...
    entity Z

    dto * with mapstruct
    service all with serviceImpl
    paginate C with pagination

请注意 `*` 和 `all` 是等价的。
最近的版本还支持排除 (在给所有实体对象设置选项时非常有用):

    entity A
    entity B
    ...
    entity Z

    dto * with mapstruct except A
    service all with serviceImpl except A, B, C
    paginate C with pagination


你还可以告诉 JHipster，你不需要创建客户端代码、或者服务端代码。
甚至你需要给 Angular 相关的文件增加前缀。（译注：为什么要给 Angular 代码增加前缀？译者试过设置这个前缀为空，结果引起一堆命名冲突，会有关键字什么的）
[Filtering](https://www.jhipster.tech/entities-filtering/) options can be activated on a per entity basis: filter `<entity name>` or for all entities: filter `*`.
在 JDL 中，这样定义：

```
entity A
entity B
entity C

skipClient A
skipServer B
angularSuffix * with mySuperEntities
filter C
```

最后，可以定义表名（默认使用实体对象的名称）：

```
entity A // A 就是表名
entity B (the_best_entity) // 在这里，the_best_entity 是表名
```


### <a name="microserviceoptions"></a> 微服务相关选项

从 JHipster v3 开始，可以创建微服务了。
你可以定义一些选项来生成实体对象：微服务名称，和搜索引擎。

这样定义微服务名称 (就是 JHipster 应用的名称):

```
entity A
entity B
entity C

microservice * with mysuperjhipsterapp except C
microservice C with myotherjhipsterapp
search * with elasticsearch except C
```

第一个选项告诉 JHipster 你希望微服务来处理你的实体对象，
第二个选项定义你的实体对象是否会被搜索到。

### <a name="annotations"></a> 注解

从 JHipster v5 版本开始支持使用注解。注解和 Java 中的使用方式类似，annotations work the same way so that
你可以给你的实体对象添加注解了。

假设有个这样的 JDL：
```
entity A
entity B
entity C

dto C with mapstruct
paginate * with pager except C
search A with elasticsearch
```

和以下使用注解方式等价：

```
@paginate(pager)
@search(elasticsearch)
entity A

@paginate(pager)
entity B

@dto(mapstruct)
entity C
```

看上去会比移除的代码添加更多，但是这的确对项目有帮助，尤其是使用微服务时使用多个 JDL 文件时。

### <a name="deploymentdeclaration"></a> 声明部署

从版本 v3.6.0 开始，可以在 JDL 中声明部署了 (兼容 JHipster v5.7 以上).

_To import one or several deployments, you need not be in a JHipster application folder._

The most basic declaration is done as follows:

```
deployment {
  deploymentType docker-compose
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}
```

A JHipster deployment has a config with default values for all other properties and using the previous syntax will
ensure your deployment will use the default values (as if you didn't make any specific choice).
The resulting deployment will have:
  - deploymentType: `docker-compose`
  - appsFolders: `foo, bar`
  - dockerRepositoryName: `yourDockerLoginName`
  - serviceDiscoveryType: `eureka`
  - gatewayType: `zuul`
  - directoryPath: `../`
  - etc.

Now, if you want some custom options:

```
deployment {
  deploymentType kubernetes
  appsFolders [store, invoice, notification, product]
  dockerRepositoryName "yourDockerLoginName"
  serviceDiscoveryType no
  istio autoInjection
  istioRoute true
  kubernetesServiceType Ingress
  kubernetesNamespace jhipster
  ingressDomain "jhipster.192.168.99.100.nip.io"
}
```

Those options are only a sample of what's available in the JDL.
The complete list of options is available in the annexes, [here](#annexes).

If you want more than one deployment, here's how you do it:

```
// will be created under 'docker-compose' folder
deployment {
  deploymentType docker-compose
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}

// will be created under 'kubernetes' folder
deployment {
  deploymentType kubernetes
  appsFolders [foo, bar]
  dockerRepositoryName "yourDockerLoginName"
}
```

You can have one deployment per deploymentType. The applications defined in `appsFolders` should be in the same folder where you are creating deployments or in the folder defined in `directoryPath`. For example for above you need to have a folder structure like this

```
.
├── yourJdlFile.jdl
├── foo
├── bar
├── kubernetes // will created by the JDL
└── docker-compose // will created by the JDL
```

## <a name="commentingjdl"></a> 注释 & Javadoc
在 JDL 文件中还可以增加 Javadoc 和注释。
和在 Java 中写法一致，Just like in Java, 下面的例子展示了如何添加 Javadoc 类型注释：

    /**
     * Class comments.
     * @author The JHipster team.
     */
    entity MyEntity { // another form of comment
      /** A required attribute */
      myField String required
      mySecondField String // another form of comment
    }

    /**
     * Second entity.
     */
    entity MySecondEntity {}

    relationship OneToMany {
      /** This is possible too! */
      MyEntity{mySecondEntity}
      to
      /**
       * And this too!
       */
      MySecondEntity{myEntity}
    }

这些注释随后会添加为 Javadoc 注释。

JDL 还会这样处理注释：

    // 被忽略的注释
    /** 不被忽略的注释 */

所以，任何以 `//` 开头的内容都是 JDL 内部注释，不会产生 Javadoc。

请注意 JDL Studio 中以 `#` 开头的指令在解析时会被忽略掉。
（译注：# entity A？不就等于 // 么？）

还有一种注释形式：

```
entity A {
  name String /** My super field */
  count Integer /** My other super field */
}
```

A 类型的 name 属性会注释为 `My super field`, B 是 `My other super field`.
备注不是必要的，但是认真写注释才是明智之举。
如果你希望混合使用逗号和注释，小心！

```
entity A {
  name String, /** My comment */
  count Integer
}
```
（译注：刚发现上面的例子，不要逗号也可以）
A 的 name 属性不会有注释，count 有。


## <a name="jdlrelationships"></a> 关联关系

这里解释如果用 JDL 创建关联关系。


### 一对一（One-to-One）

双向关系，Car 有一个 Driver, Driver 有一个 Car.

    entity Driver
    entity Car
    relationship OneToOne {
      Car{driver} to Driver{car}
    }

单向关系，Citizen 有一个 Passport, 但是 Passport 关联不到他的所有者。

    entity Citizen
    entity Passport
    relationship OneToOne {
      Citizen to Passport
    }


### 一对多（One-to-Many）

双向关系，Owner 有一个、或没有、或多个 Car 对象，Car 关联它的所有者：

    entity Owner
    entity Car
    relationship OneToMany {
      Owner{car} to Car{owner}
    }

单向关系，JHipster 还不支持，可以这样标识：
（关于为什么不支持，参考另一个 关联关系 章节）

    entity Owner
    entity Car
    relationship OneToMany {
      Owner to Car
    }


### 多对一（Many-to-One）

和上面一对多（One-to-Many）相反的关系一样的写法。
（译注：应该是 one-to-many，many-to-one 都可以写的意思）
单向关系，只有 Car 关联它的所有者：

    entity Owner
    entity Car
    relationship ManyToOne {
      Car to Owner
    }


### 多对多（Many-to-Many）

最后，Car 和 Driver 的多对多关系，可以互相访问：

    entity Driver
    entity Car
    relationship ManyToMany {
      Car{driver} to Driver{car}
    }

请注意：关系的管理方（owning side）需要写在定义的左边！

# <a name="constants"></a> 常量

从 JHipster Core v1.2.7 版本开始，JDL 支持数字型常量。
例子：

```
DEFAULT_MIN_LENGTH = 1
DEFAULT_MAX_LENGTH = 42
DEFAULT_MIN_BYTES = 20
DEFAULT_MAX_BYTES = 40
DEFAULT_MIN = 0
DEFAULT_MAX = 41

entity A {
  name String minlength(DEFAULT_MIN_LENGTH) maxlength(DEFAULT_MAX_LENGTH)
  content TextBlob required
  count Integer min(DEFAULT_MIN) max(DEFAULT_MAX)
}
```

# <a name="workflows"></a>工作流

## <a name="workflow_monolith"></a>单体应用工作流

没有特别的
  - 创建应用
  - 创建 JDL 文件
  - 导入 JDL

## <a name="workflow_microservice"></a>微服务工作流

Dealing with microservices is a bit trickier, but the JDL gives you some options to handle your entities as you see fit.

With the `microservice <ENTITIES> with <MICROSERVICE_APP_NAME>` you can specify which entity gets generated in which microservice.
Take this setup for instance:
```
entity A
entity B
entity C

microservice A with firstMS
microservice B with secondMS
```
Given two JHipster applications ('firstMS' and 'secondMS'), here's what you're going to get if you import the JDL file
in the two applications:
  - In 'firstMS', entities `A` and `C` will be generated.
  - In 'secondMS', entities `B` and `C` will be generated.

`C` gets generated in both because if there's no microservice option specifying where this entity gets generated, it
will be generated everywhere.
If you decide to import this JDL in a monolith app, every entity will be generated (monoliths don't have restriction
options in the JDL).

Note: if you want to make the same entity be generated in two different microservices, you can write two JDL files
instead of updating the JDL file. everytime.

The previous example couldn't have been written like this:
```
entity A
entity B
entity C

microservice * except B with firstMS
microservice * except A with secondMS
```
Here's the result:
  - In 'firstMS', only the entity `C` will be generated
  - In 'secondMS', entities `B` and `C` will be generated.
It's because, at parsing-time, if an option overlaps with another, the latter takes precedence.

You can also create entire microservice stack using JDL, [see this blog post](https://medium.com/@deepu105/create-full-microservice-stack-using-jhipster-domain-language-under-30-minutes-ecc6e7fc3f77) for example

---


# <a name="annexes"></a>附录

## <a name="application_options">Available application options
Here are the application options supported in the JDL:

<table class="table table-striped table-responsive">
  <tr>
    <th>JDL option name</th>
    <th>Default value</th>
    <th>Possible values</th>
    <th>Comment</th>
  </tr>
  <tr>
    <td>applicationType</td>
    <td>monolith</td>
    <td>monolith, microservice, gateway, uaa</td>
    <td></td>
  </tr>
  <tr>
    <td>baseName</td>
    <td>jhipster</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>packageName</td>
    <td>com.mycompany.myapp</td>
    <td></td>
    <td>Sets the packageFolder option</td>
  </tr>
  <tr>
    <td>authenticationType</td>
    <td>jwt or uaa</td>
    <td>jwt, session, uaa, oauth2</td>
    <td>uaa for UAA apps, jwt otherwise</td>
  </tr>
  <tr>
    <td>uaaBaseName</td>
    <td></td>
    <td></td>
    <td>Mandatory for gateway and microservices if auth type is uaa, must be between double-quotes</td>
  </tr>
  <tr>
    <td>buildTool</td>
    <td>maven</td>
    <td>maven, gradle</td>
    <td></td>
  </tr>
  <tr>
    <td>databaseType</td>
    <td>sql</td>
    <td>sql, mongodb, cassandra, couchbase, no</td>
    <td></td>
  </tr>
  <tr>
    <td>devDatabaseType</td>
    <td>h2Disk</td>
    <td>h2Disk, h2Memory, *</td>
    <td>* + the prod database type</td>
  </tr>
  <tr>
    <td>prodDatabaseType</td>
    <td>mysql</td>
    <td>mysql, mariadb, mssql, postgresql, oracle, no</td>
    <td></td>
  </tr>
  <tr>
    <td>cacheProvider</td>
    <td>ehcache or hazelcast</td>
    <td>ehcache, hazelcast, infinispan, no</td>
    <td>ehcache for monoliths and gateways, hazelcast otherwise</td>
  </tr>
  <tr>
    <td>enableHibernateCache</td>
    <td>true</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>clientFramework</td>
    <td>angularX</td>
    <td>angularX, react</td>
    <td></td>
  </tr>
  <tr>
    <td>useSass</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>clientPackageManager</td>
    <td>npm</td>
    <td>npm, yarn</td>
    <td></td>
  </tr>
  <tr>
    <td>jhiPrefix</td>
    <td>jhi</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>enableTranslation</td>
    <td>true</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>nativeLanguage</td>
    <td>en</td>
    <td>Any language supported by JHipster</td>
    <td></td>
  </tr>
  <tr>
    <td>languages</td>
    <td>[en, fr]</td>
    <td>Languages available in JHipster</td>
    <td>Braces are mandatory</td>
  </tr>
  <tr>
    <td>enableSwaggerCodegen</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>serviceDiscoveryType</td>
    <td>false</td>
    <td>eureka, consul, no</td>
    <td></td>
  </tr>
  <tr>
    <td>messageBroker</td>
    <td>false</td>
    <td>kafka, false</td>
    <td></td>
  </tr>
  <tr>
    <td>searchEngine</td>
    <td>false</td>
    <td>elasticsearch, false</td>
    <td></td>
  </tr>
  <tr>
    <td>serverPort</td>
    <td>8080, 8081 or 9999</td>
    <td></td>
    <td>Depends on the app type</td>
  </tr>
  <tr>
    <td>websocket</td>
    <td>false</td>
    <td>spring-websocket, false</td>
    <td></td>
  </tr>
  <tr>
    <td>testFrameworks</td>
    <td>[]</td>
    <td>protractor, cucumber, gatling</td>
    <td>Braces mandatory</td>
  </tr>
  <tr>
    <td>skipClient</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>skipServer</td>
    <td>false</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>skipUserManagement</td>
    <td>true</td>
    <td></td>
    <td></td>
  </tr>
</table>

## <a name="deployment_options">可选的部署选项

Here are the application options supported in the JDL:

<table class="table table-striped table-responsive">
  <tr>
    <th>JDL option name</th>
    <th>Default value</th>
    <th>Possible values</th>
    <th>Comment</th>
  </tr>
  <tr>
    <td>deploymentType</td>
    <td>docker-compose</td>
    <td>docker-compose, kubernetes, openshift, rancher-compose</td>
    <td></td>
  </tr>
  <tr>
    <td>directoryPath</td>
    <td>"../"</td>
    <td></td>
    <td>Relative path. Must be in double quotes</td>
  </tr>
  <tr>
    <td>appsFolders</td>
    <td>[]</td>
    <td></td>
    <td>Directory names for the applications separated by comma. Must be a list, example [foo, bar]</td>
  </tr>
  <tr>
    <td>clusteredDbApps</td>
    <td>[]</td>
    <td></td>
    <td>Directory names for the applications with clustered DB separated by comma. Must be a list, example [foo, bar]</td>
  </tr>
  <tr>
    <td>gatewayType</td>
    <td>zuul</td>
    <td>zuul, traefik</td>
    <td>Value is ignored when serviceDiscoveryType is `no`</td>
  </tr>
  <tr>
    <td>monitoring</td>
    <td>no</td>
    <td>no, elk, prometheus</td>
    <td></td>
  </tr>
  <tr>
    <td>consoleOptions</td>
    <td>[]</td>
    <td>[curator, zipkin]</td>
    <td>Must be a list</td>
  </tr>
  <tr>
    <td>serviceDiscoveryType</td>
    <td>eureka</td>
    <td>eureka, consul, no</td>
    <td></td>
  </tr>
  <tr>
    <td>dockerRepositoryName</td>
    <td></td>
    <td></td>
    <td>The name or URL of the docker repository. Must be in double quotes</td>
  </tr>
  <tr>
    <td>dockerPushCommand</td>
    <td>"docker push"</td>
    <td></td>
    <td>The docker push command to use. Must be in double quotes</td>
  </tr>
  <tr>
    <td>kubernetesNamespace</td>
    <td>default</td>
    <td></td>
    <td>Applicable only when deploymentType is kubernetes</td>
  </tr>
  <tr>
    <td>kubernetesServiceType</td>
    <td>LoadBalancer</td>
    <td>LoadBalancer, NodePort, Ingress</td>
    <td>Applicable only when deploymentType is kubernetes</td>
  </tr>
  <tr>
    <td>ingressDomain</td>
    <td></td>
    <td></td>
    <td>The domain for Ingress when kubernetesServiceType is `Ingress`. Must be in double quotes. Applicable only when deploymentType is kubernetes</td>
  </tr>
  <tr>
    <td>istio</td>
    <td>no</td>
    <td>no, manualInjection, autoInjection</td>
    <td>Applicable only when deploymentType is kubernetes</td>
  </tr>
  <tr>
    <td>istioRoute</td>
    <td>false</td>
    <td></td>
    <td>Applicable only when deploymentType is kubernetes</td>
  </tr>
  <tr>
    <td>enableRancherLoadBalancing</td>
    <td>false</td>
    <td></td>
    <td>Applicable only when deploymentType is rancher-compose</td>
  </tr>
  <tr>
    <td>openshiftNamespace</td>
    <td>default</td>
    <td></td>
    <td>Applicable only when deploymentType is openshift</td>
  </tr>
  <tr>
    <td>storageType</td>
    <td>ephemeral</td>
    <td>ephemeral, persistent</td>
    <td>Applicable only when deploymentType is openshift</td>
  </tr>
</table>

## <a name="types_and_constraints"></a>支持的类型和约束

JDL 支持如下类型：

Common databases:
  - PostgreSQL
  - MySQL
  - MariaDB
  - Oracle
  - MsSQL
  - MongoDB
  - Couchbase

<table class="table table-striped table-responsive">
  <tr>
    <th>Common databases</th>
    <th>Cassandra</th>
    <th>Validations</th>
  </tr>
  <tr>
    <td>String</td>
    <td>String</td>
    <td><dfn>required, minlength, maxlength, pattern, unique</dfn></td>
  </tr>
  <tr>
    <td>Integer</td>
    <td>Integer</td>
    <td><dfn>required, min, max, unique</dfn></td>
  </tr>
  <tr>
    <td>Long</td>
    <td>Long</td>
    <td><dfn>required, min, max, unique</dfn></td>
  </tr>
  <tr>
    <td>BigDecimal</td>
    <td>BigDecimal</td>
    <td><dfn>required, min, max, unique</dfn></td>
  </tr>
  <tr>
    <td>Float</td>
    <td>Float</td>
    <td><dfn>required, min, max, unique</dfn></td>
  </tr>
  <tr>
    <td>Double</td>
    <td>Double</td>
    <td><dfn>required, min, max, unique</dfn></td>
  </tr>
  <tr>
    <td>Enum</td>
    <td></td>
    <td><dfn>required, unique</dfn></td>
  </tr>
  <tr>
    <td>Boolean</td>
    <td>Boolean</td>
    <td>required, unique</td>
  </tr>
  <tr>
    <td>LocalDate</td>
    <td></td>
    <td><dfn>required, unique</dfn></td>
  </tr>
  <tr>
    <td></td>
    <td>Date</td>
    <td><dfn>required, unique</dfn></td>
  </tr>
  <tr>
    <td>ZonedDateTime</td>
    <td></td>
    <td><dfn>required, unique</dfn></td>
  </tr>
  <tr>
    <td></td>
    <td>UUID</td>
    <td><dfn>required, unique</dfn></td>
  </tr>
  <tr>
    <td>Blob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes, unique</dfn></td>
  </tr>
  <tr>
    <td>AnyBlob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes, unique</dfn></td>
  </tr>
  <tr>
    <td>ImageBlob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes, unique</dfn></td>
  </tr>
  <tr>
    <td>TextBlob</td>
    <td></td>
    <td><dfn>required, unique</dfn></td>
  </tr>
  <tr>
    <td>Instant</td>
    <td>Instant</td>
    <td><dfn>required, unique</dfn></td>
  </tr>
</table>

## <a name="all_options"></a> 其他选项

### 无参数选项

这些选项没有选项值：
  - `skipClient`
  - `skipServer`
  - `noFluentMethod` （译注：这是啥？）
  - `filter` （译注：这是啥？）

他们是这样用的：`<OPTION> <ENTITIES | * | all> except? <ENTITIES>`

### 有参数选项

这些选项有选项值：
  - `dto` (`mapstruct`)
  - `service` (`serviceClass`, `serviceImpl`)
  - `paginate` (`pager`, `pagination`, `infinite-scroll`)
  - `search` (`elasticsearch`)
  - `microservice` (custom value)
  - `angularSuffix` (custom value)
  - `clientRootFolder` (custom value)

# <a name="issues"></a> 问题和 Bug

JDL 开源在 [GitHub](https://github.com/jhipster/jhipster-core) 上， 
遵循指引 [贡献 JHipster]( https://github.com/jhipster/generator-jhipster/blob/master/CONTRIBUTING.md).

请帮助我们提交 issue，PR（Pull requests）。

- [JDL issue tracker](https://github.com/jhipster/jhipster-core/issues)
- [JDL Pull Requests](https://github.com/jhipster/jhipster-core/pulls)

提交时，请尽可能描述清楚： 
  - **一个 issue 只包含一个问题** (或者请求、问题);  
  - 欢迎大家提 PR（Pull requests）, 但请保证提交尽量单一完整且易于理解。 