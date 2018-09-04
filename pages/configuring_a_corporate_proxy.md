---
layout: default
title: Configuring a corporate proxy
permalink: /configuring-a-corporate-proxy/
redirect_from:
  - /configuring_a_corporate_proxy.html
sitemap:
    priority: 0.7
    lastmod: 2016-08-18T08:00:00-00:00
---

# <i class="fa fa-exchange"></i> 配置代理服务器

如果是在公司环境中使用 JHipster，有可能需要设置相关工具来通过代理服务器访问网络。

可以设置 `HTTP_PROXY` 和 `HTTPS_PROXY` 两个环境变量，或者使用类似 [Cntlm](http://cntlm.sourceforge.net/) 的工具来设置。

但是这样可能还不够，还需要单独设置每一个 JHipster 使用的工具。

## 介绍

假设代理服务器的参数为：

- username
- password
- host
- port

那么配置为：`http://username:password@host:port`

如果使用：[Cntlm](http://cntlm.sourceforge.net/)，设置为：`127.0.0.1:3128`。另外的，根据下面的步骤设置其他工具。

## NPM configuration

执行命令：

```
npm config set proxy http://username:password@host:port
npm config set https-proxy http://username:password@host:port
```

或者直接编辑配置文件 `~/.npmrc`：

```
proxy=http://username:password@host:port
https-proxy=http://username:password@host:port
https_proxy=http://username:password@host:port
```

## Yarn 的设置

执行命令：

```
yarn config set proxy http://username:password@host:port
yarn config set https-proxy http://username:password@host:port
```

## Git 的设置

执行命令：

```
git config --global http.proxy http://username:password@host:port
git config --global https.proxy http://username:password@host:port
```

或者直接编辑配置文件 `~/.gitconfig`：

```
[http]
        proxy = http://username:password@host:port
[https]
        proxy = http://username:password@host:port
```

## Maven configuration

编辑配置文件 `~/.m2/settings.xml` 中的 `proxies`：

```
<proxies>
    <proxy>
        <id>id</id>
        <active>true</active>
        <protocol>http</protocol>
        <username>username</username>
        <password>password</password>
        <host>host</host>
        <port>port</port>
        <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
</proxies>
```

### Maven Wrapper 的设置

创建一个新的文件 `.mvn/jvm.config`，设置配置属性：

```
-Dhttp.proxyHost=host 
-Dhttp.proxyPort=port 
-Dhttps.proxyHost=host 
-Dhttps.proxyPort=port 
-Dhttp.proxyUser=username 
-Dhttp.proxyPassword=password
```

## Gradle 的配置

在配置文件 `gradle.properties` 中，如果使用 gradle wrapper，在文件 `gradle/wrapper/gradle-wrapper.properties` 中设置，

也可以在全局配置文件 `USER_HOME/.gradle/gradle.properties` 中设置：

```
## Proxy setup
systemProp.proxySet="true"
systemProp.http.keepAlive="true"
systemProp.http.proxyHost=host
systemProp.http.proxyPort=port
systemProp.http.proxyUser=username
systemProp.http.proxyPassword=password
systemProp.http.nonProxyHosts=local.net|some.host.com

systemProp.https.keepAlive="true"
systemProp.https.proxyHost=host
systemProp.https.proxyPort=port
systemProp.https.proxyUser=username
systemProp.https.proxyPassword=password
systemProp.https.nonProxyHosts=local.net|some.host.com
## end of proxy setup
```

## Docker 的设置

### 原生使用 Docker

根据操作系统，设置 (`/etc/sysconfig/docker` 或 `/etc/default/docker`)。

重启 docker：`sudo service docker restart`。

这不会设置到 systemd。参考 [page from docker](https://docs.docker.com/engine/admin/systemd/#http-proxy)
来设置代理。

### 用 docker-machine

通过命令启动 docker-machine：

```
docker-machine create -d virtualbox \
    --engine-env HTTP_PROXY=http://username:password@host:port \
    --engine-env HTTPS_PROXY=http://username:password@host:port \
    default
```

或者编辑配置文件：`~/.docker/machine/machines/default/config.json`.
