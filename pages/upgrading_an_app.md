---
layout: default
title: Updating an application
permalink: /upgrading-an-application/
sitemap:
    priority: 0.7
    lastmod: 2014-06-02T00:00:00-00:00
gitgraph: http://jsfiddle.net/lordlothar99/tqp9gyu3
---

# <i class="fa fa-refresh"></i> 升级应用

每当有新的 JHipster 版本发布，JHipster 的 upgrade 命令可以帮助我们升级一个现有的应用到最新版，并且不会丢失你所做的变更。

这非常有用：

- 可以获得最新的 JHipster 特性
- 获得重要的补丁修复或安全更新
- 在你的代码仓库从保存这些变化，并且在重新生成的代码上很方便地合并

_请在升级你的应用前仔细阅读这篇关于升级的说明，来理解更新是如何工作的_

## 准备工作

要让这个命令工作，你需要事先安装好 `git` [http://git-scm.com](http://git-scm.com/).

## 执行升级工具

进入应用根目录：

`cd myapplication/`

升级，执行:

`jhipster upgrade`

还可以有这些可选参数：

* `--verbose` - 观察模式，更新过程中查看更新步骤的详细日志
* `--target-version=4.2.0` - 更新到一个特定的版本，而不是最新版，这在一个项目已经落后最新版好多个版本的时候比较有用
* `--force` - 强制执行更新重新生成项目，即使没有更新的 JHipster 版本

## 升级过程的图形化视角

下面的是升级过程的图形化展示 (后面的章节会进行详细的解释):

![GitGraph]({{ site.url }}/images/upgrade_gitgraph.png)

(图像来自于 [JSFiddle](http://jsfiddle.net/lordlothar99/tqp9gyu3/) )

请注意 `jhipster_upgrade` 分支是从项目上孤立创建出来的，他不会正确地在上面的图上显示出来。

## 上图流程的解释

下面是 JHipster upgrade 命令的详细步骤：

1. 检查是否有 JHipster 更新版本 (如果使用了 `--force` 就不检查)。
2. 检查项目是否初始化了 `git` 仓库，否则 JHipster 会初始化并提交当前代码到 master 分支。
3. 检查没有未提交的代码。如果还有未提交的代码升级流程就会自动退出。
4. 检查是否有一个 `jhipster_upgrade` 分支。如果没，创建：这里的细节参考下面的“第一次升级的具体步骤”章节。
5. 检出 `jhipster_upgrade` 分支。
6. 全局更新 JHipster 到最新版。
7. 清楚当前项目目录。
8. 用 `jhipster --force --with-entities` 命令重新生成当前项目。
9. 将生成的代码提交到 `jhipster_upgrade` 分支。
10. 将 `jhipster_upgrade` 分支合并回原来 `jhipster upgrade` 命令启动时的分支。
11. 最后需要手工处理合并的冲突。

祝贺，你的应用已经完成升级到最新版本的 JHipster 了。

## 第一次升级的具体步骤

第一次执行 JHipster 升级命令时，为了避免你所做的变更被清除，建议执行下面的操作：

1. 创建一个孤立的 `jhipster_upgrade` 分支（没有父分支）
2. 应用创建好（使用当前的 JHipster 版本）。
3. 在 `master` 分支上提交 block-merge : 确保没有 `master` 上的修改；这只是个比较实用的方式来记录 `master` 的 HEAD 是更新到当前版本的 JHipster 了。

### 建议

不要在 `jhipster_upgrade` 分支上提交任何代码。这个分支是作为 JHipster 更新用的：每次 upgrade 命令执行，这里都会创建一个新的 commit。
