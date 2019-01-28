---
layout: default
title: Managing relationships
permalink: /managing-relationships/
redirect_from:
  - /managing_relationships.html
sitemap:
    priority: 0.7
    lastmod: 2016-03-26T18:40:00-00:00
---

# <i class="fa fa-sitemap"></i> 管理关联关系

如果使用了 JPA，可以用 [entity sub-generator]({{ site.url }}/creating-an-entity/) 工具在实体对象之间创建关联关系。

## 准备工作

关联关系设置只适用于 JPA。如果你在使用 [Cassandra]({{ site.url }}/using-cassandra/) 或者 [MongoDB]({{ site.url }}/using-mongodb/) 或者 [Couchbase]({{ site.url }}/using-couchbase/)，该关系是不可用的。

一个管理关系作用于两个实体对象之间，JHipster 会创建以下代码：

- 在实体对象内用 JPA 来管理这些关系
- 创建正确的 Liquibase 变更记录，来维护数据库中的关联关系
- 在前端的 Angular/React 代码中生成代码来让用户能在用户界面上图形化方式设置这些关系

## JHipster UML 和 JDL Studio 工具

这篇文章标书的是用 JHipster 的命令行工具创建关联关系。如果你希望创建很多关系，建议使用图形化工具。

在这一章节，有两个可行选项：

- [JHipster UML]({{ site.url }}/jhipster-uml/), 使你能够使用 UML 编辑器。
- [JDL Studio](https://start.jhipster.tech/jdl-studio/), 这是我们的在线实体对象及管理关系编辑工具，使用我们的 DSL （domain-specific language）。

你可以从一个 JDL 文件来使用 `import-jdl` sub-generator 创建实体对象及关联关系, 执行命令：`jhipster import-jdl your-jdl-file.jh`.

## 支持的关联关系类型

使用 JPA 时, 普通的一对多（one-to-many）, 多对一（many-to-one）, 多对多（many-to-many），一对一（one-to-one）关联关系有如下场景：

1. [双向一对多（one-to-many）关系](#1)
2. [单向多对一（many-to-one）关系](#2)
3. [单向一对多（one-to-many）关系](#3)
4. [两个一对多（one-to-many） 关系，在两个相同的实体对象上](#4)
5. [多对多（many-to-many）关系](#5)
6. [一对一（one-to-one）关系](#6)
7. [单向一对一（one-to-one）关系](#7)

_提示: 关于 `User` 对象_

请注意关于 `User` 对象，是由 JHipster 管理的，特定的，你可以：

- 创建管理这个对象的 `many-to-one` 关系 (比如一个 `Car` 可以有一个 many-to-one 关系来关联 `User`)。这能在你的 repository 里创建特定的查询，让你能根据用户做查询过滤，这个需求很普遍。在 Angular/React 创建的客户端界面上，你会在 `Car` 对象上得到一个下拉控件来选择 `User`，
- 创建 `many-to-many` 及 `one-to-one` 关系来关联 `User` 对象，但是对方实体 __必须__ 是该关系的 owner， (`Team` 可以有一个 many-to-many 关系来关联 `User`，但只有 `Team` 才可以添加/移除 `User`，反过来 `User` 不能管理 `Team`）。在 Angular/React 的客户端界面上，你会得到一个 `User` 对象的多选框。

## <a name="1"></a> 双向一对多（one-to-many）关系

我们来定义两个实体对象，`Owner` 和 `Car`。一个 owner 可以拥有多辆 car，一个 car 只能有一个 owner。

这是一个非常普通的一对多（one-to-many）关系(一个 owner 拥有多个 car) 在“一”的一边, 以及一个多对一（many-to-one）关系 (多个 car 拥有同一个 owner) 在另一边对象上:

    Owner (1) <-----> (*) Car

我们先创建 `Owner`。下面是创建 `Owner` 时的相关问题：

    jhipster entity Owner
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? car
    ? What is the type of the relationship? one-to-many
    ? What is the name of this relationship in the other entity? owner

请注意我们选择了关系的默认名称。

接下来创建 `Car`:

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Owner
    ? What is the name of the relationship? owner
    ? What is the type of the relationship? many-to-one
    ? When you display this relationship with Angular, which field from 'Owner' do you want to use? id


同样的，可以用下面的 JDL 来完成

    entity Owner
    entity Car

    relationship OneToMany {
      Owner{car} to Car{owner}
    }

OK了，现在我们有了一个 one-to-many 关系。在 Angular/React 创建的客户端界面上有一个下拉框来选择 `Owner` 的 `Car` 。

## <a name="2"></a> 单向多对一（many-to-one）关系

在上面的例子里我们创建了双向关系: 从 `Car` 上你可以找到它的 owner，从 `Owner` 上你能找到它所有的 car。

多对一（many-to-one）关系意味着 car 对象可以关联到他们的 owner, 但反过来不行。

    Owner (1) <----- (*) Car

你可能会因为这些场景使用该关系:

- 对于业务视角，你只在这种方式下使用你的对象。不需要一些允许开发人员实现一些不清楚的 API。
- 需要做些性能优化在 `Owner` 上 (因为它不会管理 car 的集合)。

在这类场景下，你可以先创建 `Owner`，并且这次不要创建任何关联关系:

    jhipster entity Owner
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? No

然后创建 `Car` ，和前面的例子一样:

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Owner
    ? What is the name of the relationship? owner
    ? What is the type of the relationship? many-to-one
    ? When you display this relationship with Angular, which field from 'Owner' do you want to use? id

这前面的例子一样可以工作，但是你不能从 `Owner` 这一端添加或删除 `car`。在 Angular/React 创建的客户端界面上有一个下拉框来选择 `Owner` 的 `Car` 。
这是相关的 DDL:

    entity Owner
    entity Car

    relationship ManyToOne {
      Car{owner} to Owner
    }


## <a name="3"></a> 单向一对多（one-to-many）关系

单向的一对多（one-to-many）关系是指 `Owner` 对象管理 `Car` 的集合，但是反过来不管。这个和上面的例子是相反的。

    Owner (1) -----> (*) Car

这种关系类型是目前 JHipster 不支持的，参考 [#1569](https://github.com/jhipster/generator-jhipster/issues/1569) 。

你可以尝试这两种解决方法：

- 设置一个双向映射，且禁止修改：这是我们目前推荐的方式，使用起来也比较方便
- 设置一个双向映射，然后修改为一个单向的映射：
    - 删除 `@OneToMany` 注解上的 "mappedBy" 属性
    - 创建关联表：执行 `mvn liquibase:diff`，参考 [使用 Liquibase diff]({{ site.url }}/development/)

这在 JDL 里面是不支持。

## <a name="4"></a> 两个一对多（one-to-many） 关系，在两个相同的实体对象上

在这个例子里，一个 `Person` 对象可以是多个 `Car` 的 owner ，同时他也可以成为多个 `Car` 的司机：

    Person (1) <---owns-----> (*) Car
    Person (1) <---drives---> (*) Car

为了实现这种关系，我们需要设置关系的 name 属性，在之前的例子里我们使用了它们的默认值。

创建 `Person` 对象，有两个 one-to-many 关系关联 `Car` ：

    jhipster entity Person
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? ownedCar
    ? What is the type of the relationship? one-to-many
    ? What is the name of this relationship in the other entity? owner
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? drivedCar
    ? What is the type of the relationship? one-to-many
    ? What is the name of this relationship in the other entity? driver

创建 `Car` 对象，使用在 `Person` 里配置的关系属性名称：

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Person
    ? What is the name of the relationship? owner
    ? What is the type of the relationship? many-to-one
    ? When you display this relationship with Angular, which field from 'Person' do you want to use? id
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Person
    ? What is the name of the relationship? driver
    ? What is the type of the relationship? many-to-one
    ? When you display this relationship with Angular, which field from 'Person' do you want to use? id

也可以用一下的 JDL 来实现：

    entity Person
    entity Car

    relationship OneToMany {
      Person{ownedCar} to Car{owner}
    }

    relationship OneToMany {
      Person{drivedCar} to Car{driver}
    }

`Car` 对象现在可以同时拥有 driver 和 owner，都是 `Person` 对象。在 Angular/React 生成的前端界面上，会有选择 `Person` 和 `owner` 字段的下拉框。

## <a name="5"></a> 多对多（many-to-many）关系

一个司机 `Driver` 可以驾驶多个汽车，同时 `Car` 也可以拥有多个司机对象。

    Driver (*) <-----> (*) Car

在数据库层面，我们需要一个关联表来关联 `Driver` 和 `Car` 表。

使用 JPA，这两者中的一个需要负责管理这个关系: 在我们的例子里，使用 `Car` 来负责添加或删除司机。

先来创建这个关系的非管理方（non-owning side），`Driver` 对象，拥有一个 many-to-many 关系:

    jhipster entity Driver
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? car
    ? What is the type of the relationship? many-to-many
    ? Is this entity the owner of the relationship? No
    ? What is the name of this relationship in the other entity? driver

然后创建 `Car`，作为 many-to-many 关系的管理方（owning side）:

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Driver
    ? What is the name of the relationship? driver
    ? What is the type of the relationship? many-to-many
    ? Is this entity the owner of the relationship? Yes
    ? When you display this relationship with Angular, which field from 'Driver' do you want to use? id

也可以用一下的 JDL 来实现：

    entity Driver
    entity Car

    relationship ManyToMany {
      Car{driver} to Driver{car}
    }

这就ok了，你现在拥有了一个在两个对象之间的 many-to-many 多对多关系。在 Angular/React 创建的 `Car` 对象的客户端界面上，会有一个多选下拉框来选择多个 `Driver`，因为 `Car` 是管理方。

## <a name="6"></a> 一对一（one-to-one）关系

下面的例子里， one-to-one 一对一关系意味着一个 `Driver` 只能驾驶一辆 `Car`，并且一辆 `Car` 只能被一个 `Driver` 开。

    Driver (1) <-----> (1) Car

先来创建非管理方（non-owning side），我们的 `Driver` 对象：

    jhipster entity Driver
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? car
    ? What is the type of the relationship? one-to-one
    ? Is this entity the owner of the relationship? No
    ? What is the name of this relationship in the other entity? driver

然后是 `Car`，管理方：

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Driver
    ? What is the name of the relationship? driver
    ? What is the type of the relationship? one-to-one
    ? Is this entity the owner of the relationship? Yes
    ? What is the name of this relationship in the other entity? car
    ? When you display this relationship with Angular, which field from 'Driver' do you want to use? id

也可以用一下的 JDL 来实现：

    entity Driver
    entity Car

    relationship OneToOne {
      Car{driver} to Driver{car}
    }

现在你拥有了 one-to-one 关系！在 Angular/React 创建的客户端界面上，`Car` 界面上有一个选择 `Driver` 的下拉框，因为 `Car` 是管理方。

## <a name="7"></a> 单向一对一（one-to-one）关系

单向一对一（one-to-one）关系意味着 `citizen` 实例可以管理他的护照 `Passport`，但是 `passport` 获取不到 `Owner`。

    Citizen (1) -----> (1) Passport

创建 `Passport`，不配置任何关系：

    jhipster entity Passport
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? No

然后是 `Citizen` ：

    jhipster entity Citizen
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Passport
    ? What is the name of the relationship? passport
    ? What is the type of the relationship? one-to-one
    ? Is this entity the owner of the relationship? Yes
    ? What is the name of this relationship in the other entity? citizen
    ? When you display this relationship with Angular, which field from 'Passport' do you want to use? id

完成后，`Citizen` 处理护照，但是反过来 `Passport` 不管理 `Citizen`。在 Angular/React 创建的客户端界面上， `Citizen` 界面有一个下拉框选择 `Passport` 。

也可以用一下的 JDL 来实现：


    entity Citizen
    entity Passport

    relationship OneToOne {
      Citizen{passport} to Passport
    }
