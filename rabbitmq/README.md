# RabbitMQ
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/rabbitmq/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/nginx)。

### 什么是 RabbitMQ?

RabbitMQ 是开源的消息队列系统（或称消息中间件），它实现了高级的消息队列协议（AMQR）。RabbitMQ 服务端是由 Erlang 编写的，同时也是基于开放电信平台框架（OTP）开发的。客户端的接口则几乎兼容所有的主流语言。（
> 来自[维基百科](https://en.wikipedia.org/wiki/RabbitMQ)

### 如何使用这个镜像？

#### 启动一个实例

RabbitMQ 通过节点名（通常是主机名）存储数据。所以我们启动 Docker 时需要设置 -h/--hostname 参数，这样可以让我们知道数据存在哪里。

```
docker run -d --hostname my-rabbit --name some-rabbit daocloud.io/library/rabbitmq:3
```

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

如果你已经执行了上面的命令，那你就可以通过 `docker logs some-rabbit` 查看这个容器实例的日志了：

```
=INFO REPORT==== 6-Jul-2015::20:47:02 ===
node           : rabbit@my-rabbit
home dir       : /var/lib/rabbitmq
config file(s) : /etc/rabbitmq/rabbitmq.config
cookie hash    : UoNOcDhfxW9uoZ92wh6BjA==
log            : tty
sasl log       : tty
database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit
```

#### Erlang Cookie

节点之间使用 cookie（[关于 RabbitMQ 集群](https://www.rabbitmq.com/clustering.html#erlang-cookie)） 来判断是否通信，唯有 cookie 相同的两个节点才能通信。

你可使用 `RABBITMQ_ERLANG_COOKIE` 来设置 rabbitmq 实例的 cookie ：

```
docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' daocloud.io/library/rabbitmq:3
```

设置完成后，通过 `docker link` 连接此 rabbitmq 实例：


```
$ docker run -it --rm --link some-rabbit:my-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' daocloud.io/library/rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl -n rabbit@my-rabbit list_users
Listing users ...
guest   [administrator]
```

当然，你也可以设置 `RABBITMQ_NODENAME` ,这样你就可以更好的使用 `rabbitmqctl` 命令了。

```
$ docker run -it --rm --link some-rabbit:my-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' -e RABBITMQ_NODENAME=rabbit@my-rabbit daocloud.io/library/rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl list_users
Listing users ...
guest   [administrator]
```


#### 管理你的 RabbitMQ 服务

RabbitMQ 已经有一些自带管理插件的镜像。用这些镜像创建的容器实例可以直接使用默认的 15672 端口访问，默认账号密码是 guest/guest ：

```
docker run -d --hostname my-rabbit --name some-rabbit daocloud.io/library/rabbitmq:3-management
```

然后打开浏览器访问 `http://container-ip:15672` ，就可以管理你的 rabbitmq 实例了，或者你可以暴露主机端口来访问：

```
docker run -d --hostname my-rabbit --name some-rabbit -p 8080:15672 daocloud.io/library/rabbitmq:3-management
``` 
这时，你可访问 `http://localhost:8080` 或者 `http://host-ip:8080` 管理 rabbitmq 服务了。


#### 连接 daemon

```
docker run --name some-app --link some-rabbit:rabbit -d application-that-uses-rabbitmq
```

### 许可

要了解这个镜像中软件的许可证请浏览[许可证信息](https://www.rabbitmq.com/mpl.html)。

### 翻译许可

<span style="font-size: 75%; text-align: center; display: block;"><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/88x31.png" /></a>本作品由 DaoCloud 翻译并采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/">知识共享署名-非商业性使用-相同方式共享 3.0 未本地化版本许可协议</a>进行许可。</span>
