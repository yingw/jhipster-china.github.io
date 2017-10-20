---
layout: default
title: JHipster Domain Language
permalink: /jdl/
sitemap:
    priority: 0.5
    lastmod: 2017-10-11T12:00:00-00:00
---

# <i class="fa fa-star"></i> JHipster Domain Language (JDL 语法)


JDL 是 JHipster 特质的 domain 语言，我们提供了能在一个文件（或多个）中来描述所有实体对象及其关系的能力，并且只需要很简单的、友好的语法。

可以使用我们的在线工具 [JDL-Studio]({{ site.url }}/jdl-studio/) 来创建 JDL 及其 UML 视图。你还可以创建、导出、或分享你的模型。

一旦你创建了项目（现有的或者用 `jhipster` 命令工具创建的), 接下来你可以使用命令 `import-jdl` 来从 JDL 文件生成实体对象，执行：`jhipster import-jdl your-jdl-file.jh` (确保在 JHipster 项目目录下执行)。
你还能用 [JHipster UML]({{ site.url }}/jhipster-uml/) 来创建实体对象并导出为 JDL 文件，执行 `jhipster-uml your-xmi-file.xmi --to-jdl`  (确保在 JHipster 项目目录下执行)。要了解更多关于如何安装和使用 JHipster UML, 访问 [JHipster UML documentation]({{ site.url }}/jhipster-uml/).

这可以作为 [entity sub-generator]({{ site.url }}/creating-an-entity/) 工具的替代。
核心思想是使用图形工具比使用传统的 Yeoman 问答来 [管理关联关系]({{ site.url }}/managing-relationships/) 要简单的多。

JDL 项目在 [GitHub 地址](https://github.com/jhipster/jhipster-core/), 它和 JHipster 一样是开源的并且遵循 Apache 2.0 开源协议。
It can also be used as a node library to do JDL parsing.

_如果你喜欢 JHipster Domain Language，别忘了给我们的项目一颗星 [GitHub](https://github.com/jhipster/jhipster-core/)!_
_如果你喜欢 JDL Studio，也不要吝啬给我们再来一颗 [GitHub](https://github.com/jhipster/jdl-studio/)!_

下面是完整的 JDL 文档：

1. [JDL 样例](#sample)
2. [如何使用](#howtojdl)  
3. [语法](#jdllanguage)  
  3.1 [声明实体对象](#entitydeclaration)  
  3.2 [声明关联关系](#relationshipdeclaration)  
  3.3 [枚举](#enumerationdeclaration)  
  3.4 [Blob 类型](#blobdeclaration)  
  3.5 [可选声明](#optiondeclaration)  
  3.6 [微服务相关选项](#microserviceoptions)
4. [备注](#commentingjdl)  
5. [所有的关联](#jdlrelationships)  
6. [内容](#constants)  
7. [附录](#annexes)  
  7.1 [类型和约束](#types_and_constraints)  
  7.2 [可选项](#all_options)  
8. [错误和 Bug](#issues)  

***

# <a name="sample"></a> JDL 样例

我们将 Oracle 的 "人力资源" 样例程序翻译成了 JDL，放在 [这里](https://github.com/jhipster/jhipster-core/blob/master/lib/dsl/example.jh). 并且该程序是 [JDL-Studio]({{ site.url }}/jdl-studio/) 默认加载的模型。

## <a name="howtojdl"></a> 如何使用

如果你打算使用 JHipster UML 而不是 `import-jdl` 命令工具，你还得安装它：`npm install -g jhipster-uml`.

你可以使用 JDL 文件来生成实体对象：

  - 创建一个后缀为 '.jh' 或 '.jdl' 的文件，
  - 声明实体对象和关联关系，或者从 [JDL-Studio]({{ site.url }}/jdl-studio/) 下载该文件，
  - 在 JHipster 应用的根目录中，执行：`jhipster import-jdl my_file.jdl` 或者 `jhipster-uml my_file.jdl`.

and *Voilà*, 就行了！

[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in  the output file unless you use it in)
[comment]: <> (a reference style link.)
[//]: <> (This is also a comment.)
[//]: # (This may be the most platform independent comment)


如果你在团队内工作，有可能需要多个文件而不是一个。我们也支持这样使你不需要手工把它们拼在一起，你需要执行：
`jhipster import-jdl my_file1.jh my_file2.jh` 或者 `jhipster-uml my_file1.jh my_file2.jh`.

如果你不想创建实体对象类，而只是需要 JSON 文件，你可以在导入时加上参数：`--json-only`，这样就只会在 `.jhipster` 目录里创建 json 文件，而不会创建那些实体对象了。

    jhipster import-jdl ./my-jdl-file.jdl --json-only
    
默认情况下，`import-jdl` 命令只会生成发生了改变的实体对象, 如果你需要所有的实体对象都重新生成一遍，可以加上参数：`--force`。请注意这会覆盖你在这些对象上做的所有变更。

    jhipster import-jdl ./my-jdl-file.jdl --force

如果你希望在项目内使用，你可以执行:`npm install jhipster-core --save` 来将之安装在本地，并保存在你的 `package.json` 文件里。

## <a name="jdllanguage"></a> 语法
我们尽量将语法设计的对开发人员友好，你可用用它来做三件事：
  - 定义实体对象和它们的属性，
  - 定义它们之间的关联关系，
  - 定义一下 JHipster 的选项。


### <a name="entitydeclaration"></a> 声明实体对象

实体对象的声明如下格式：

    entity <entity name> {
      <field name> <type> [<validation>*]
    }

  - `<entity name>` 是实体对象的名称，
  - `<field name>` 实体对象的属性，
  - `<type>` JHipster 支持的属性类型，
  - 可选的 `<validation>` 类型的校验规则。

所有的属性类型和校验规则在 [这里](#annexes)，如果校验规则需要一个值，还要加上 `(<value>)` 在校验规则的右边。


一些 JDL 的例子：

```
entity A
entity B
entity C {}
entity D {
  name String required,
  address String required maxlength(100),
  age Integer required min(18)
}
```

正则表达式的写法稍微特别些 (v1.3.6 版本开始支持):
```
entity A {
  myString required min(1) max(42) pattern(/[A-Z]+/)
}
```
如果你在使用 v4.9.X 之前的版本，你得这样使用正则表达式：`pattern('[A-Z]+')`.

JDL 设计的易用可读，如果你的实体是空的（没有属性），你可以这样声明：`entity A` 或 `entity A {}`。

Note that JHipster adds a default `id` field so that you needn't worry about it.


### <a name="relationshipdeclaration"></a> Relationship declaration

The relationships declaration is done as follows:

    relationship (OneToMany | ManyToOne | OneToOne | ManyToMany) {
      <from entity>[{<relationship name>[(<display field>)]}] to <to entity>[{<relationship name>[(<display field>)]}]
    }

  - `(OneToMany | ManyToOne| OneToOne | ManyToMany)` is the type of your relationship,
  - `<from entity>` is the name of the entity owner of the relationship: the source,
  - `<to entity>` is the name of the entity where the relationship goes to: the destination,
  - `<relationship name>` is the name of the field having the other end as type,
  - `<display field>` is the name of the field that should show up in select boxes (default: `id`),
  - `required` whether the injected field is required.


Here's a simple example:

A Book has one, required, Author, an Author has several Books.

    entity Book
    entity Author

    relationship OneToMany {
      Author{book} to Book{writer(name) required}
    }

Of course, in real cases, you'd have a lot of relationships and always writing the same three lines could be tedious.
That's why you can declare something like:

```
entity A
entity B
entity C
entity D

relationship OneToOne {
  A{b} to B{a},
  B{c} to C
}
relationship ManyToMany {
  A{d} to D{a},
  C{d} to D{c}
}
```

The join is always done using the `id` field which is also the default field shown when editing a relation in the frontend. If another field should be shown instead, you can specify it like this:

```
entity A {
  name String required
}
entity B


relationship OneToOne {
  A{b} to B{a(name)}
}
```

This makes JHipster generate a REST resource that returns both `id` and `name` of the linked entity to the frontend, so the name can be shown to the user instead.

### <a name="enumerationdeclaration"></a> Enumerations

To make Enums with JDL just do as follows:

- Declare an Enum where you want in the file:

        enum Language {
          FRENCH, ENGLISH, SPANISH
        }

- In an entity, add fields with the Enum as a type:

        entity Book {
          title String required,
          description String,
          language Language
        }


### <a name="blobdeclaration"></a> Blob (byte[])
JHipster gives a great choice as one can choose between an image type or any binary type. JDL lets you do the same: just create a custom type (see DataType) with the editor, name it according to these conventions:

  - `AnyBlob` or just `Blob` to create a field of the "any" binary type;
  - `ImageBlob` to create a field meant to be an image.
  - `TextBlob` to create a field for a CLOB (long text).

And you can create as many DataTypes as you like.


### <a name="optiondeclaration"></a> Option declaration

In JHipster, you can specify options for your entities such as pagination or DTO.
You can do the same with the JDL:

    entity A {
      name String required
    }

    entity B {}

    entity C {}

    dto A, B with mapstruct

    paginate A with infinite-scroll
    paginate B with pagination
    paginate C with pager  // pager is only available in AngularJS

    service A with serviceClass
    service C with serviceImpl

The keywords `dto`, `paginate`, `service` and `with` were added to the grammar to support these changes.
If a wrong option is specified, JDL will inform you of that with a nice, red message and will just ignore it so as not to corrupt JHipster's JSON files.

#### Service option

No services specified will create a resource class which will call the repository interface directly. This is the default and simplest option, see A.
Service with serviceClass (see B) will make the resource call the service class which will call the repository interface. Service with serviceImpl (see C) will make a service interface which will be used by the resource class. The interface is implemented by an impl class which will call the repository interface.

Use no service if not sure it's the simplest option and good for CRUD. Use service with a Class if you will have a lot of business logic which will use multiple repositories making it ideal for a service class. Jhipster's are not a fan of unnecessary Interfaces but if you like them go for service with impl.

    entity A {}
    entity B {}
    entity C {}

    // no service for A
    service B with serviceClass
    service C with serviceImpl


JDL also supports mass-option setting. it is possible to do:

    entity A
    entity B
    ...
    entity Z

    dto * with mapstruct
    service all with serviceImpl
    paginate C with pagination

Note that `*` and `all` are equivalent.
Latest version introduces exclusions (which is quite a powerful option when setting options for every entity):

    entity A
    entity B
    ...
    entity Z

    dto * with mapstruct except A
    service all with serviceImpl except A, B, C
    paginate C with pagination


With JHipster, you can also tell whether you don't want any client code, or server code. Even if you want to add a suffix to Angular-related files, you can do that in JHipster.
In your JDL file, simply add these lines to do the same:

```
entity A
entity B
entity C

skipClient for A
skipServer for B
angularSuffix * with mySuperEntities
```

Finally, table names can also be specified (the entity's name will be used by default):

```
entity A // A is the table's name here
entity B (the_best_entity) // the_best_entity is the table's name
```


### <a name="microserviceoptions"></a> Microservice-related options

As of JHipster v3, microservices can be created. You can specify some options to generate your entities in the JDL: the microservice's name and the search engine.

Here is how you can specify your microservice's name (the JHipster app's name):

```
entity A
entity B
entity C

microservice * with mysuperjhipsterapp except C
microservice C with myotherjhipsterapp
search * with elasticsearch except C
```

The first option is used to tell JHipster that you want your microservice to deal with your entities, whereas the second specifies how and if you want your entities searched.


## <a name="commentingjdl"></a> Commenting & Javadoc
It is possible to add Javadoc & comments to JDL files.  
Just like in Java, this example demonstrates how to add Javadoc comments:

    /**
     * Class comments.
     * @author The JHipster team.
     */
    entity MyEntity { // another form of comment
      /** A required attribute */
      myField String required,
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

These comments will later be added as Javadoc comments by JHipster.

JDL possesses its own kind of comment:

    // an ignored comment
    /** not an ignored comment */

Therefore, anything that starts with `//` is considered an internal comment for JDL, and will not be counted as Javadoc.

Please note that the JDL Studio directives that start with `#` will be ignored during parsing.

Another form of comments are the following comments:
```
entity A {
  name String /** My super field */
  count Integer /** My other super field */
}
```
Here A's name will be commented with `My super field`, B with `My other super field`.
Yes, commas are not mandatory but it's wiser to have them so as not to make mistakes in the code.
If you want to mix commas and following comments, beware!
```
entity A {
  name String, /** My comment */
  count Integer
}
```
A's name won't have the comment, because the count will.


## <a name="jdlrelationships"></a>All the relationships

Explanation on how to create relationships with JDL.


### One-to-One

A bidirectional relationship where the Car has a Driver, and the Driver has a Car.

    entity Driver {}
    entity Car {}
    relationship OneToOne {
      Car{driver} to Driver{car}
    }

A Unidirectional example where a Citizen has a Passport, but the Passport has no access to sole its owner.

    entity Citizen {}
    entity Passport {}
    relationship OneToOne {
      Citizen{passport} to Passport
    }


### One-to-Many

A bidirectional relationship where the Owner has none, one or more Car objects, and the Car knows its owner.

    entity Owner {}
    entity Car {}
    relationship OneToMany {
      Owner{car} to Car{owner}
    }

Unidirectional versions for this relationship are not supported by JHipster, but it would look like this:

    entity Owner {}
    entity Car {}
    relationship OneToMany {
      Owner{car} to Car
    }


### Many-to-One

The reciprocal version of One-to-Many relationships is the same as previously.
The unidirectional version where the Car knows its owners:

    entity Owner {}
    entity Car {}
    relationship ManyToOne {
      Car{owner} to Owner
    }


### Many-to-Many

Finally, in this example we have the Car that knows of its drivers, and the Driver object can access its cars.

    entity Driver {}
    entity Car {}
    relationship ManyToMany {
      Car{driver} to Driver{car}
    }

Please note that the owning side of the relationship has to be on the left side

# <a name="constants"></a>Constants

As of JHipster Core v1.2.7, the JDL supports numerical constants.
Here is an example:

```
DEFAULT_MIN_LENGTH = 1
DEFAULT_MAX_LENGTH = 42
DEFAULT_MIN_BYTES = 20
DEFAULT_MAX_BYTES = 40
DEFAULT_MIN = 0
DEFAULT_MAX = 41

entity A {
  name String minlength(DEFAULT_MIN_LENGTH) maxlength(DEFAULT_MAX_LENGTH)
  content TextBlob minbytes(DEFAULT_MIN_BYTES) maxbytes(DEFAULT_MAX_BYTES)
  count Integer min(DEFAULT_MIN) max(DEFAULT_MAX)
}
```

# <a name="annexes"></a>Annexes

## <a name="types_and_constraints"></a>Available types and constraints

Here is the types supported by JDL:

<table class="table table-striped table-responsive">
  <tr>
    <th>SQL</th>
    <th>MongoDB</th>
    <th>Cassandra</th>
    <th>Validations</th>
  </tr>
  <tr>
    <td>String</td>
    <td>String</td>
    <td>String</td>
    <td><dfn>required, minlength, maxlength, pattern</dfn></td>
  </tr>
  <tr>
    <td>Integer</td>
    <td>Integer</td>
    <td>Integer</td>
    <td><dfn>required, min, max</dfn></td>
  </tr>
  <tr>
    <td>Long</td>
    <td>Long</td>
    <td>Long</td>
    <td><dfn>required, min, max</dfn></td>
  </tr>
  <tr>
    <td>BigDecimal</td>
    <td>BigDecimal</td>
    <td>BigDecimal</td>
    <td><dfn>required, min, max</dfn></td>
  </tr>
  <tr>
    <td>Float</td>
    <td>Float</td>
    <td>Float</td>
    <td><dfn>required, min, max</dfn></td>
  </tr>
  <tr>
    <td>Double</td>
    <td>Double</td>
    <td>Double</td>
    <td><dfn>required, min, max</dfn></td>
  </tr>
  <tr>
    <td>Enum</td>
    <td>Enum</td>
    <td></td>
    <td><dfn>required</dfn></td>
  </tr>
  <tr>
    <td>Boolean</td>
    <td>Boolean</td>
    <td>Boolean</td>
    <td>required</td>
  </tr>
  <tr>
    <td>LocalDate</td>
    <td>LocalDate</td>
    <td></td>
    <td><dfn>required</dfn></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td>Date</td>
    <td><dfn>required</dfn></td>
  </tr>
  <tr>
    <td>ZonedDateTime</td>
    <td>ZonedDateTime</td>
    <td></td>
    <td><dfn>required</dfn></td>
  </tr>
  <tr>
    <td></td>
    <td></td>
    <td>UUID</td>
    <td><dfn>required</dfn></td>
  </tr>
  <tr>
    <td>Blob</td>
    <td>Blob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes</dfn></td>
  </tr>
  <tr>
    <td>AnyBlob</td>
    <td>AnyBlob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes</dfn></td>
  </tr>
  <tr>
    <td>ImageBlob</td>
    <td>ImageBlob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes</dfn></td>
  </tr>
  <tr>
    <td>TextBlob</td>
    <td>TextBlob</td>
    <td></td>
    <td><dfn>required, minbytes, maxbytes</dfn></td>
  </tr>
  <tr>
    <td>Instant</td>
    <td>Instant</td>
    <td>Instant</td>
    <td><dfn>required</dfn></td>
  </tr>
</table>

## <a name="all_options"></a> Available options

### Unary options

These options don't have any value:
  - `skipClient`
  - `skipServer`
  - `noFluentMethod`
  - `filter`

They can be used like this: `<OPTION> <ENTITIES | * | all> except? <ENTITIES>`

### Binary options

These options take values:
  - `dto` (`mapstruct`)
  - `service` (`serviceClass`, `serviceImpl`)
  - `paginate` (`pager`, `pagination`, `infinite-scroll`)
  - `searchEngine` (`elasticsearch`)
  - `microservice` (custom value)
  - `angularSuffix` (custom value)

# <a name="issues"></a>Issues and bugs

JDL is [available on GitHub](https://github.com/jhipster/jhipster-core), and follows the same [contributing guidelines as JHipster]( https://github.com/jhipster/generator-jhipster/blob/master/CONTRIBUTING.md).

Please use our project for submitting issues and Pull Requests concerning the library itself.

- [JDL issue tracker](https://github.com/jhipster/jhipster-core/issues)
- [JDL Pull Requests](https://github.com/jhipster/jhipster-core/pulls)

When submitting anything, you must be as precise as possible:  
  - **One posted issue must only have one problem** (or one demand/question);  
  - Pull requests are welcome, but the commits must be 'atomic' to really be understandable.  
