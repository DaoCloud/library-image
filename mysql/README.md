# MySQL

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/centos/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。
>
> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/mysql)。

MySQL 由于其性能高、成本低、可靠性好，已经成为全球最流行的开源数据库软件，被广泛地被应用在 Internet 上的中小型网站中。随着 MySQL 的不断成熟，它也逐渐出现在更多大规模网站和应用上，比如 Facebook、Twitter 和 Yahoo! 等站点。

## 如何使用本镜像

### 启动一个 `mysql` 服务实例

启动一个 MySQL 实例非常简单：

```console
$ docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

在上述命令中，`some-mysql` 指定了该容器的名字，`my-secret-pw` 指定了 root 用户的密码，`tag` 参数指定了你想要的 MySQL 版本。

### 从另外一个 Docker 容器连接 MySQL 服务

本镜像会暴露 MySQL 的标准端口 3306，你可以使用 `link` 功能来让其他应用容器能够访问 MySQL 容器，就像下面这样：

```console
$ docker run --name some-app --link some-mysql:mysql -d app-that-uses-mysql
```

### 使用 MySQL 命令行工具连接 MySQL

下面的命令启动了另一个 MySQL 容器并使用 MySQL 命令行工具访问你之前的 MySQL 服务，之后能你就能向你的数据库执行 SQL 语句了：

```console
$ docker run -it --link some-mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
```

在上述命令中 `some-mysql` 就是你原来 MySQL 服务容器的名字。

想要了解更多有关 MySQL 命令行工具的信息可以参考 [MySQL 官方文档](http://dev.mysql.com/doc/en/mysql.html)。

### 查看 MySQL 日志

`docker exec` 命令能让你在一个容器中额外地运行新命令。比如你可以执行下面的命令来获得一个 bash shell：

```console
$ docker exec -it some-mysql bash
```

你可以通过查看 Docker 容器的日志获得 MySQL 服务的日志：

```console
$ docker logs some-mysql
```

### 使用自定义 MySQL 配置文件

当 MySQL 服务启动时会以 `/etc/mysql/my.cnf` 为配置文件，本文件会导入 `/etc/mysql/conf.d` 目录中所有以 `.cnf` 为后缀的文件。这些文件会拓展或覆盖 `/etc/mysql/my.cnf` 文件中的配置。因此你可以创建你自己需要的配置文件并挂载至 MySQL 容器中的 `/etc/mysql/conf.d` 目录。

假设 `/my/custom/config-file.cnf` 是你自定义的配置文件，你可以像这样启动一个 MySQL 容器（注意这里直接挂载了配置文件的目录）：

```console
$ docker run --name some-mysql -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

这会启动一个名为 `some-mysql` 且同时加载了 `/etc/mysql/my.cnf` 和 `/etc/mysql/conf.d/config-file.cnf` 这两个配置文件的新容器，注意这时以后者的配置优先。

SELinux 用户在这里可能会遇到一个问题，目前的解决方法是为你的配置文件指定相关的 SELinux 策略配置，那样容器才可以访问它：

```console
$ chcon -Rt svirt_sandbox_file_t /my/custom
```

### 环境变量

当你启动 `mysql` 镜像时，你可以通过 `docker run` 命令传递几个特定的环境变量来调整相关设置，特别注意当容器启动时，所有环境变量都不会影响一个容器中已存在的数据库内容。

#### `MYSQL_ROOT_PASSWORD`

本变量必填，它指定了 MySQL `root` 的用户的密码。在刚才的例子中，该密码被设置为 `my-secret-pw`。

#### `MYSQL_DATABASE`

本变量可选，通过该变量当 MySQL 启动时会创建一个由你指定的数据库。如果你另外又提供了一对用户名和密码（见下方），那么他将会被授予本数据库的所有权限。

#### `MYSQL_USER`, `MYSQL_PASSWORD`

这两个变量可选，同时使用的话会创建一个新用户并设置相应的密码，该用户会被授予由 `MYSQL_DATABASE` 变量指定的数据库的所有权限（见上方）。只有当同时提供了这两个变量时该用户才会被创建。

特别注意没有必要使用这个机制来创建 root 用户，root 用户的密码会被设置为 `MYSQL_ROOT_PASSWORD` 变量的值。

#### `MYSQL_ALLOW_EMPTY_PASSWORD`

本变量可选，当其被设置为 `yes` 时将会允许当前容器中的 root 用户能够使用空密码。*注意*：绝对不建议将该变量设置为 `yes`，除非你知道自己在做什么。如果这么做的话你的 MySQL 服务将会失去保护，所有人都可以以超级用户的身份访问该 MySQL 服务。

## 重要说明

### 储存数据的位置

摘要：下面介绍了多种储存 Docker 容器中数据的方式，我们鼓励 `mysql` 镜像用户熟悉下面各项技术：

- 使用 Docker 自带的 [Volume 机制](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume)将数据库文件写入宿主机的磁盘。这是默认的方式，对用户来讲简单且透明。缺点是宿主机上的工具或应用可能难以定位这些文件。
- 在宿主机上创建一个数据目录（在容器外部）并把他[挂载至容器内部](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume)。此时数据库文件被放置在宿主机上一个已知的目录里，那样容器外部的应用和工具就可以方便地访问这些文件。缺点是用户需要确保这些目录存在，且宿主机上正确配置了权限设置。

阅读 Docker 文档能快速了解不同的储存选项，并且有很多博客或论坛讨论并给出了这方面的建议。我们会在下面简单地演示一下：

1. 在你宿主机的一个合适的卷上创建一个数据目录，例：`/my/own/datadir`。
2. 使用下面的命令启动你的 `mysql` 容器：

```console
$ docker run --name some-mysql -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

我们通过 `-v /my/own/datadir:/var/lib/mysql` 参数从宿主机挂载 `/my/own/datadir` 目录至容器内作为 `/var/lib/mysql` 目录，那样 MySQL 就会默认将数据文件写入这个目录中。

注意 SELinux 用户可能会遇到一个问题，目前的解决方法是为你的数据目录指定相关的 SELinux 策略配置，那样容器才可以访问它：

```console
$ chcon -Rt svirt_sandbox_file_t /my/own/datadir
```

### MySQL 初始化完成前无连接

如果容器启动时没有任何数据库被初始化，那么就会创建一个默认的数据库。这是一个预料中的行为，它意味着除非初始化完成，否则 MySQL 不会接受任何连接。当你使用 `docker-compose` 等自动化工具同时启动一系列容器时，可能会因为这个特性而造成一些问题。

### 使用已存在的数据库

如果从一个已存在数据库文件的目录上启动一个 `mysql` 容器的话（特别是一个 `mysql` 子目录），那么 `$MYSQL_ROOT_PASSWORD` 变量应该在启动命令中略去；即使设置了也会被忽略，已存在的数据库不会以任何方式被改变。

## 用户反馈

### 文档

你可以从 GitHub 的 [`docker-library/docs`](https://github.com/docker-library/docs) 库中的 [`mysql/`](https://github.com/docker-library/docs/tree/master/mysql)目录找到本镜像的文档。在发起一个 pull request 前请确保你已熟悉仓库中 [`README.md`](https://github.com/docker-library/docs/blob/master/README.md) 文档中的相关说明。

### 提问

如果你对本镜像有任何问题的话可以在 [GitHub issue](https://github.com/docker-library/mysql/issues) 上联系我们。

你也可以使用 IRC 通过 [Freenode](https://freenode.net) 上的 `#docker-library` 频道联系许多官方镜像的维护者。

### 贡献

我们总是乐意接受你的 pull request，不管问题大小，我们会尽可能快速地处理它们。

对于热情的贡献者，在编码之前，我们建议你通过 [GitHub issue](https://github.com/docker-library/mysql/issues) 讨论你的计划。这样其他贡献者会协助你优化你的设计，并避免你做了别人重复的工作。