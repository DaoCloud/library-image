# Postgres
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/postgres/)同步并提供中文文档支持，用来帮助国内开发者更方便的使用Docker镜像。

### 什么是 PostgreSQL?

PostgreSQL 是以加州大学伯克利分校计算机系开发的 Postgres，现在已经更名为PostgreSQL，版本 4.2为基础的对象关系型数据库管理系统（ORDBMS）。PostgreSQL支持大部分 SQL标准并且提供了许多其他现代特性：复杂查询、外键、触发器、视图、事务完整性、MVCC。同样，PostgreSQL 可以用许多方法扩展，比如， 通过增加新的数据类型、函数、操作符、聚集函数、索引。免费使用、修改、和分发 PostgreSQL，不管是私用、商用、还是学术研究使用。（来自[百度百科](http://baike.baidu.com/item/PostgreSQL)）

### 如何使用这个镜像？

#### 启动一个 postgres 实例

```
docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d daocloud.io/library/postgres
```

这个镜像会导出 postgres 的5432端口, 因此通过标准的 link 机制就可以方便的访问 postgres 数据库实例。 容器启动时会通过 `initdb` 自动创建默认的 `postgres` 用户和数据库。

>
postgres 数据库是可以被用户，工具和第三方应用程序访问的默认数据库。 [postgres 文档](postgresql.org/docs)


#### 从应用中连接数据库

```
docker run --name some-app --link some-postgres:postgres -d application-that-uses-postgres
```

#### 或者通过 psql

```
docker run -it --link some-postgres:postgres --rm postgres sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'
```

#### 环境变量

PostgreSQL 镜像通过一系列环境变量来配置容器，虽然这些环境变量都不是必须的，但是它们会极大的方便您使用镜像。

`
POSTGRES_PASSWORD
`

推荐您使用镜像时指定这个环境变量，它用来设置超级用户的密码。 默认的超级用户是由环境变量 `POSTGRES_USER` 指定的。 在开始的例子中，超级用户密码被设置为 "mysecretpassword"。


`
POSTGRES_USER
`

这个可选的环境变量是搭配 `POSTGRES_PASSWORD` 一起来设置用户名和密码的，它会创建一个指定名称的超级管理员和同名数据库。 如果没有设置这个环境变量，将使用默认值 `postgres` 。

### 如何扩展这个镜像

如果您希望在这个镜像的派生镜像中执行额外的初始化工作，可以在 `/docker-entrypoint-init.d` 目录下增加 `*.sh` 脚本 （如果该目录不存在则创建目录），在初始化过程调用 `initdb` 创建默认的 postgres 用户和数据库后，它会执行该目录下的所有 `*.sh` 脚本完成额外初始化操作再启动服务。 如果您希望在初始化中执行 SQL 语句，强烈建议您使用 Postgres [单用户模式](http://www.postgresql.org/docs/9.3/static/app-postgres.html#AEN90580)。

您还可以通过一个简单的 Dockerfile 设置 `locale`， 下面这个例子将设置默认的 `locale` 为 `de_DE.utf8`：

```
FROM daocloud.io/library/postgres:9.4
RUN localedef -i de_DE -c -f UTF-8 -A /usr/share/locale/locale.alias de_DE.UTF-8
ENV LANG de_DE.utf8
```
因为数据库初始化仅发生在容器启动时，因此您可以在数据库创建前设置语言。

### 注意

如果容器启动时没有数据库，postgres 会为您创建一个默认数据库。虽然这是 postgres 正常的行为，但它意味着在这个阶段数据库是不接受连接请求的。 这个行为会对一些自动化工具产生影响，比如 fig 会同时启动多个容器。

### 支持的Docker版本

这个镜像在Docker1.7.0上提供最佳的官方支持，对于其他老版本的Docker(1.0之后)也能提供基本的兼容。