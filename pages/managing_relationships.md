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

In the previous example we had a 双向关系: from a `Car` instance you could find its owner, and from a `Owner` instance you could get all of its cars.

多对一（many-to-one）关系意味着 car 对象可以关联到他们的 owner, 但反过来不行。

    Owner (1) <----- (*) Car

You would do that关系 for two reasons:

- From a business point of view, you only use your entities in this way. So you don't want to have an API that allows developers to do something which doesn't make sense.
- You have a small performance gain when using the `Owner` entity (as it won't have to manage the collection of cars).

In that case, you would still create the `Owner` first, this time with no关系:

    jhipster entity Owner
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? No

And then the `Car` entity, as in the previous example:

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Owner
    ? What is the name of the relationship? owner
    ? What is the type of the relationship? many-to-one
    ? When you display this relationship with Angular, which field from 'Owner' do you want to use? id

This will work as in the previous example, but you won't be able to add or remove cars from the `Owner` entity. On the generated Angular/React client UI you will have a dropdown in `Car` to select a `Owner`.
This is the corresponding JDL:

    entity Owner
    entity Car

    relationship ManyToOne {
      Car{owner} to Owner
    }


## <a name="3"></a> 单向一对多（one-to-many）关系

单向的一对多（one-to-many）关系 means that the `Owner` instance can get its collection of cars, but not the opposite. It is the opposite from the previous example.

    Owner (1) -----> (*) Car

这种关系类型是目前 JHipster 不支持的，参考 [#1569](https://github.com/jhipster/generator-jhipster/issues/1569) for more information.

你可以尝试这两种解决方法：

- 设置一个双向映射，且禁止修改：这是我们目前推荐的方式，使用起来也比较方便
- 设置一个双向映射，然后修改为一个单向的映射：
    - 删除 `@OneToMany` 注解上的 "mappedBy" 属性
    - 创建关联表：执行 `mvn liquibase:diff`，参考 [documentation about using Liquibase diff]({{ site.url }}/development/)

这在 JDL 里面是不支持的因为这不是在 JHipster 里。（译注：？）

## <a name="4"></a> 两个一对多（one-to-many） 关系，在两个相同的实体对象上

For this example, a `Person` can be the owner of many cars, and he can also be the driver of many cars:

    Person (1) <---owns-----> (*) Car
    Person (1) <---drives---> (*) Car

为了实现这种关系，我们需要设置关系的 name 属性, which we have left with their default values in the previous examples.

Generate the `Person` entity, which has two one-to-many关系s to the `Car` entity:

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

Generate the `Car` entity, which use the same关系 name has was configured in the `Person` entity:

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

The same can be achieved using the below JDL as well

    entity Person
    entity Car

    relationship OneToMany {
      Person{ownedCar} to Car{owner}
    }

    relationship OneToMany {
      Person{drivedCar} to Car{driver}
    }

A `Car` can now have a driver and a owner, which are both `Person` entities. On the generated Angular/React client UI you will dropdowns in `Car` to select a `Person` for `owner` field and `driver` field.

## <a name="5"></a> 多对多（many-to-many）关系

A `Driver` can drive many cars, but a `Car` can also have many drivers.

    Driver (*) <-----> (*) Car

At the database level, 我们需要一个关联表来关联 `Driver` 和 `Car` 表.

For JPA, one of those two entities will need to manage the关系: in our case, that would be the `Car` entity, which will be responsible to add or remove drivers.

Let us generate the 非管理方（non-owning side） of the关系, the `Driver`, with a many-to-many关系:

    jhipster entity Driver
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? car
    ? What is the type of the relationship? many-to-many
    ? Is this entity the owner of the relationship? No
    ? What is the name of this relationship in the other entity? driver

Then generate the `Car`, with the 管理方（owning side） of the many-to-many关系:

    jhipster entity Car
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Driver
    ? What is the name of the relationship? driver
    ? What is the type of the relationship? many-to-many
    ? Is this entity the owner of the relationship? Yes
    ? When you display this relationship with Angular, which field from 'Driver' do you want to use? id

The same can be achieved using the below JDL as well

    entity Driver
    entity Car

    relationship ManyToMany {
      Car{driver} to Driver{car}
    }

That's it, you now have a many-to-many关系 between those two entities! On the generated Angular/React client UI you will have a multi-select dropdown in `Car` to select multiple `Driver` since `Car` is the owning side.

## <a name="6"></a> 一对一（one-to-one）关系

Following our example, a one-to-one关系 would mean that a `Driver` can drive only one `Car`, and a `Car` can only have one `Driver`.

    Driver (1) <-----> (1) Car

Let us create the 非管理方（non-owning side） of the关系, in our case the `Driver`:

    jhipster entity Driver
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? Yes
    ? What is the name of the other entity? Car
    ? What is the name of the relationship? car
    ? What is the type of the relationship? one-to-one
    ? Is this entity the owner of the relationship? No
    ? What is the name of this relationship in the other entity? driver

Then generate the `Car`, which owns the关系:

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

The same can be achieved using the below JDL as well

    entity Driver
    entity Car

    relationship OneToOne {
      Car{driver} to Driver{car}
    }

That's it, you now have a one-to-one关系 between those two entities! On the generated Angular/React client UI you will have a dropdown in `Car` to select a `Driver` since `Car` is the owning side.

## <a name="7"></a> 单向一对一（one-to-one）关系

单向一对一（one-to-one）关系 means that the `citizen` instance can get its passport, but the `passport` instance can't get to its owner.

    Citizen (1) -----> (1) Passport

Generate the `Passport` entity first, without any关系 to its owner:

    jhipster entity Passport
    ...
    Generating relationships with other entities
    ? Do you want to add a relationship to another entity? No

Then, generate the `Citizen` entity:

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

After doing this, a `Citizen` possesses a passport, but no `Citizen` instance is defined in `Passport`. On the generated Angular/React client UI you will have a dropdown in `Citizen` to select a `Passport` since `Citizen` is the owning side.
This is the corresponding JDL:


    entity Citizen
    entity Passport

    relationship OneToOne {
      Citizen{passport} to Passport
    }
