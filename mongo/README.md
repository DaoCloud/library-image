# MongoDB

> 此镜像从 [Docker Hub](https://hub.docker.com/_/mongo/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。
>
> 该镜像源维护在 [Github](https://github.com/docker-library/mongo)。

## 什么是 MongoDB？

MongoDB （源自 "humogous"）是一个面向文档的跨平台数据库。作为一个 NoSQL 数据库，MongoDB 避开了传统关系型数据库结构，转而使用动态类似 JSON 的 BSON 格式，使其能轻松地将多个数据写在同一类型中。MongoDB 以 AGPL 和 Apache License 联合协议发布。

MongoDB 最早由 10gen 软件公司（现在为 MongoDB 公司）于 2007 年 10 月时作为 PaaS 服务的一个组件开始开发，而后在 2009 年该公司转型为开源模式并与 10gen 公司提供商业技术支持。之后 MongoDB 就被一些大型网站和服务商作为后端软件使用，包括 Craigslist、eBay、Foursquare、SourceForge、Viacom 和纽约时报等等。MongoDB 是当今最流行的 NoSQL 数据库软件。

> [wikipedia.org/wiki/MongoDB](https://en.wikipedia.org/wiki/MongoDB)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/mongo/logo.png)

## 如何使用本镜像

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 启动一个 mongo 实例

```console
$ docker run --name some-mongo -d daocloud.io/mongo
```

由于该镜像的 Dockerfile 中包含了 `EXPOSE 27017`（mongo 默认端口），所以可以通过 `link` 两个容器来访问它（就像下面的示例）。

### 在应用中连接该实例

```console
$ docker run --name some-app --link some-mongo:mongo -d application-that-uses-mongo
```

### 使用 `mongo` 命令

```console
$ docker run -it --link some-mongo:mongo --rm daocloud.io/mongo sh -c 'exec mongo "$MONGO_PORT_27017_TCP_ADDR:$MONGO_PORT_27017_TCP_PORT/test"'
```

### 相关配置

你可以查阅[官方文档](http://docs.mongodb.org/manual/)来了解如何配置 MongoDB 进行复制或分片操作。

你也可以简单地设置 `--storageEngine` 参数来指定你需要的储存引擎（比如 MongoDB 3.0 中的 WiredTiger）。如果需要从旧版本升级的话，请确保你已熟悉此[文档](http://docs.mongodb.org/manual/release-notes/3.0-upgrade/#change-storage-engine-to-wiredtiger)。

```console
$ docker run --name some-mongo -d daocloud.io/mongo --storageEngine wiredTiger
```

### 储存数据的位置

摘要：下面介绍了多种储存 Docker 容器中数据的方式，我们鼓励 `mongo` 镜像用户熟悉下面各项技术：

- 使用 Docker 自带的 [Volume 机制](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume)将数据库文件写入宿主机的磁盘。这是默认的方式，对用户来讲简单且透明。缺点是宿主机上的工具或应用可能难以定位这些文件。
- 在宿主机上创建一个数据目录（在容器外部）并把他[挂载至容器内部](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume)。此时数据库文件被放置在宿主机上一个已知的目录里，那样容器外部的应用和工具就可以方便地访问这些文件。缺点是用户需要确保这些目录存在，且宿主机上正确配置了权限设置。

**警告**：由于 MongoDB 使用了内存映射机制，因此当你使用 vboxsf 时会出现错误，这是一个已知的 bug ([vbox bug](https://www.virtualbox.org/ticket/819))。

阅读 Docker 文档能快速了解不同的储存选项，并且有很多博客或论坛讨论并给出了这方面的建议。我们会在下面简单地演示一下：

1. 在宿主机上创建一个数据目录，例：`/my/own/datadir`。
2. 使用下面的命令启动 `mongo` 容器：

```console
$ docker run --name some-mongo -v /my/own/datadir:/data/db -d daocloud.io/mongo:tag
```

我们通过 `-v /my/own/datadir:/data/db` 参数从宿主机挂载 `/my/own/datadir` 目录至容器内作为 `/data/db` 目录，那样 MongoDB 就会默认将数据文件写入这个目录中。

注意 SELinux 用户可能会遇到一个问题，目前的解决方法是为你的数据目录指定相关的 SELinux 策略配置，那样容器才可以访问它：

```console
$ chcon -Rt svirt_sandbox_file_t /my/own/datadir
```

## 协议

你可以查阅 [license information](https://github.com/mongodb/mongo/blob/7c3cfac300cfcca4f73f1c3b18457f0f8fae3f69/README#L71) 了解本镜像中软件的使用协议。

## 支持的 Docker 版本

本镜像正式支持 Docker 1.8.1 版本。

我们最低为 Docker 1.0 版本提供力所能及的支持。

### 文档

你可以从 GitHub 的 [`docker-library/docs`](https://github.com/docker-library/docs) 库中的 [`mongo/`](https://github.com/docker-library/docs/tree/master/mongo)目录找到本镜像的文档。在发起一个 pull request 前请确保你已熟悉仓库中 [`README.md`](https://github.com/docker-library/docs/blob/master/README.md) 文档中的相关说明。

### 提问

如果你对本镜像有任何问题的话可以在 [GitHub issue](https://github.com/docker-library/mongo/issues) 上联系我们。

你也可以使用 IRC 通过 [Freenode](https://freenode.net) 上的 `#docker-library` 频道联系许多官方镜像的维护者。

### 贡献

我们总是乐意接受你的 pull request，不管问题大小，我们会尽可能快速地处理它们。

对于热情的贡献者，在编码之前，我们建议你通过 [GitHub issue](https://github.com/docker-library/mysql/issues) 讨论你的计划。这样其他贡献者会协助你优化你的设计，并避免你做了别人重复的工作。

## 该翻译的许可证

本作品采用[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/4.0/)进行许可。

