# Debian
> 此镜像从[Docker Hub](https://hub.docker.com/_/debian/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/debian)。

## 什么是Debian?

Debian 是完全由自由软件组成的类UNIX操作系统，其包含的多数软件使用GNU通用公共许可协议授权，并由Debian计划的参与者组成团队对其进行打包、开发与维护。

> 来自[维基百科](https://zh.wikipedia.org/wiki/Debian)

## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### debian:8

```
$ docker run daocloud.io/debian:8 grep -v '^#' /etc/apt/sources.list
```

### debian:7

```
$ docker run daocloud.io/debian:7 cat /etc/apt/sources.list
```

## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

本作品采用[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/4.0/)进行许可。
