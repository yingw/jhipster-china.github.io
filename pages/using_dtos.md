---
layout: default
title: 使用 DTO
permalink: /using-dtos/
redirect_from:
  - /using_dtos.html
sitemap:
    priority: 0.7
    lastmod: 2015-05-28T23:41:00-00:00
---

# <i class="fa fa-briefcase"></i> [BETA] 使用 DTO

__警告!__ 这个新特性目前还是 <b>测试</b> 阶段。谨慎使用！欢迎反馈！

## 介绍

默认情况下，JHipster 使用领域模型对象（domain objects，通常是 JPA 实体对象）直接给到 REST 端点。这样做有很多好处，主要好处是代码很简单，易于理解。

对于复杂的场景，你还是有可能会需要使用数据传输对象（Data Transfer Objects ，DTOs），被用来暴露给 REST 端点。这些对象在领域模型上增加了额外的一层，并对 REST 层做了优化和适配：主要的好处是他们可以集成多种领域模型。

## JHipster 里的 DTO

当创建 JHipster 实体对象时，你可以增加一层服务层：DTO 选项也只会在你选择了服务层时才可用，因为它需要这一层来做映射 (如果你使用了 JPA，也因为这一层处理了事务，延迟加载才会正常工作）。

一旦你选择创建了服务层，你就可以选择创建实体对象的 DTO。如果你选择了该选项：

- 创建一个 DTO ，并映射到实体对象。
- 它会集合管理 many-to-one 关系，只使用 ID 和另一个在客户端显示的字段。以至于需要在 DTO 上添加一个 many-to-one 关系指向 `User` 对象，添加 `userId` 字段和 `userLogin` 字段。
- 它会在非管理端忽略 one-to-many 和 many-to-many 关系：这和实体对象的工作方式匹配 (他们会有个 `@JsonIgnore` 字段注解)。
- 对于 many-to-many 关系，在管理端：会从另一端实体使用 DTO，并把它们放在一个 `Set` 里。也就是，其他实体也是用了 DTO 的时候才会起作用。

## 使用 MapStruct 来映射 DTO 和实体

大部分 DTO 和实体对象看上去差不多，所以经常会有需要能在他们之间做自动映射。

这个章节介绍使用 [MapStruct](http://mapstruct.org/)。这是一个组件处理器，作为 Java 编译器的插件，能自动创建所需的映射。

我们发现这是非常简洁有效的，并且不需要反射（反射在映射中的使用会有较大的性能影响）。

## 设置你的 IDE 来支持 MapStruct

MapStruct 是一种注释处理器，所以它需要在你的 IDE 编译项目的时候正确设置。

如果你使用的是 Maven，你需要启用 maven 的 `IDE` profile。Gradle 用户不需要做任何设置。

启用这个 profile 的介绍参考： [配置你的 IDE]({{ site.url }}/configuring-ide/).

## 高级 MapStruct 使用

MapStruct 的 mapper 配置为 Spring Bean，并支持依赖注入（dependency injection）。一个好的建议是你可以把 `Repository` 注入到 mapper 里，使你能用 ID 从 mapper 中抓取 JPA 实体。

这里是一个例子，抓取 `User` 实体：

    @Mapper
    public abstract class CarMapper {

        @Inject
        private UserRepository userRepository;

        @Mapping(source = "user.id", target = "userId")
        @Mapping(source = "user.login", target = "userLogin")
        public abstract CarDTO carToCarDTO(Car car);

        @Mapping(source = "userId", target = "user")
        public abstract Car carDTOToCar(CarDTO carDTO);

        public User userFromId(Long id) {
            if (id == null) {
                return null;
            }
            return userRepository.findOne(id);
        }
    }
