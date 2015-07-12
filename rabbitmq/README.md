# RabbitMQ
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/rabbitmq/)同步并提供中文文档支持，用来帮助国内开发者更方便的使用Docker镜像。

### 什么是 RabbitMQ?

RabbitMQ 是开源的消息队列系统（或称消息中间件），它实现了高级的消息队列协议（AMQR）。RabbitMQ 服务端是由 Erlang 编写的，同时也是基于开放电信平台框架（OTP）开发的。客户端的接口则几乎兼容所有的主流语言。（来自[维基百科](https://en.wikipedia.org/wiki/RabbitMQ)）

### 如何使用这个镜像？

#### 启动一个实例

RabbitMQ 通过节点名（通常是主机名）存储数据。所以我们启动 Docker 时需要设置 -h/--hostname 参数，这样可以让我们知道数据存在哪里。
'''

docker run --name some-rabbit -d daocloud.io/library/rabbitmq 

'''
如果你已经执行了上面的命令，那你就可以通过 'docker logs some-rabbit' 查看这个容器实例的日志了。

### Erlang Cookie




