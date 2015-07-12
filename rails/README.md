# 被支持的标签和与之对应的 `Dockerfile` 链接

* **[`4.2.3`, `4.2`, `4`, `latest` (*Dockerfile*)](https://github.com/docker-library/rails/blob/e3a9c6f610e9d1e428cecfae02feddcd1e22b36c/Dockerfile)**
* **[`onbuild` (*onbuild/Dockerfile*)](https://github.com/docker-library/rails/blob/9fb5d2b7e0f2e7029855028e07e86ab7ec54abaa/onbuild/Dockerfile)**

想要了解更多关于该镜像的信息和更新历史，请参考在 **[GitHub 上的代码仓库 `docker-library/official-images`](https://github.com/docker-library/official-images)** 内 **[(`library/rails`) 相应的清单文件](https://github.com/docker-library/official-images/blob/master/library/rails)**。

# 什么是 Ruby on Rails？

Ruby on Rails 或 Rails，是一个用 Ruby 运行的开源互联网应用框架。它是一个全栈框架。这就意味着它是「开箱即用」的，Rails 能够创建可以从网站服务器获取请求信息、连接和查询数据库、并且渲染模板的页面和应用。因此 Rails 还自带一个独立于网站服务器的路由体系。

> 参考：维基百科上 **[Ruby on Rails](https://en.wikipedia.org/wiki/Ruby_on_Rails)** 的介绍。

![Rails 图标](https://raw.githubusercontent.com/docker-library/docs/master/rails/logo.png)

# 如何使用这个镜像？

## 在您的 Rails 项目中创建一个 `Dockerfile`

```
FROM rails:onbuild
```

将这个文件置于您应用的顶层目录，与 `Gemfile` 在同一个目录下。

这个镜像包含了应该适用于大部分项目的多个 `ONBUILD` 触发器。这将会在构建时 `COPY . /usr/src/app`，`RUN bundle install`，`EXPOSE 3000`，并且将默认的命令设置为 `rails server`。

这时您就可以构建和运行这个 Docker 镜像：

```
docker build -t my-rails-app .
docker run --name some-rails-app -d my-rails-app
```

您可以通过在浏览器中访问 `http://<容器 IP>:3000` 来测试。如果您需要让宿主外的机器访问，您可以通过下面的指令来映射至 8080 端口。

```
docker run --name some-rails-app -p 8080:3000 -d my-rails-app
```

这样您就可以在浏览器中通过访问 `http://localhost:8080` 或者 `http://<宿主 IP>:8080` 来查看了。

### 生成一个 `Gemfile.lock`

`onbuild` 标签期望在您的应用目录中找到一个 `Gemfile.lock` 文件。 下面的 `docker run` 指令将会帮您生成一个。请在您应用的顶级目录（`Gemfile` 所在的目录）下执行这个指令：

```
docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app ruby:2.1 bundle install
```

## 创建一个新的 Rails 应用

如果您想为一个新的 Rails 项目生成一个目录结构，您可以这样：

```
docker run -it --rm --user "$(id -u):$(id -g)" -v "$PWD":/usr/src/app -w /usr/src/app rails rails new webapp
```

这将会在您的当前目录下创建一个叫做 `webapp` 的子目录。

# 镜像的版本

`rails` 镜像带有很多不同标签的版本，每个版本都为一个特定的用例设计。

## `rails:<版本号>`

这是标准镜像。如果您不知道应该用哪个版本，您基本上应该选择这个。它被设计为一个既可以用过就丢弃的容器（用来挂载您的源码和用容器启动您的应用），同时一个可以当做另一个镜像的基础镜像。

## `rails:onbuild`

这个镜像使得构建衍生镜像变得更加容易。大部分项目中，在您的项目的顶级目录中创建一个以 `FROM rails:onbuild` 开头的 `Dockerfile` 已经足够让您创建一个独立的镜像了。

然而 `onbuild` 版本的真正用意在于创建一个构建好就可以跑（短期内从零到容器化），由于缺少对*何时* `ONBUILD` 操作会触发的掌控（参考 **[`docker/docker#5714`](https://github.com/docker/docker/issues/5714)**，**[`docker/docker#8240`](https://github.com/docker/docker/issues/8240)**，**[`docker/docker#11917`](https://github.com/docker/docker/issues/11917)**），在项目里长期使用这个版本并不推荐。

一旦您掌握了您的项目是如何在 Docker 里工作的，您可能想要将您的 `Dockerfile` 的基础镜像调整为一个非 `onbuild` 版本。并且拷贝 `onbuild` 版本中的命令到新的 `Dockerfile` 中（将带有 `ONBUILD` 的指令移动到文件底部并且移除 `ONBUILD` 关键词），这样您就有了对触发器何时触发的控制并且使得您自己和其他阅读您 `Dockerfile` 的人们更容易读懂。
同时这也会使随着时间的推移添加额外的需求变得更加容易（比如在执行 `ONBUILD` 前安装更多的软件包）。

# 许可证

要了解这个镜像中软件的许可证请浏览[许可证信息](https://github.com/rails/rails#license)。

# 支持的 Docker 版本

这个镜像官方支持的 Docker 版本为 1.7.0。

对于（1.0 以上的）旧版本将尽可能支持。

# 用户反馈

## 文档

这个镜像的文档储存在 **[GitHub 上的代码仓库 `docker-library/docs`](https://github.com/docker-library/docs)** 中的 **[`rails/` 目录下](https://github.com/docker-library/docs/tree/master/rails)**。在尝试提交拉取请求前请确保自己已经熟悉**[代码仓库中的 `README.md` 文件](https://github.com/docker-library/docs/blob/master/README.md)**。

## 问题

如果您在使用这个镜像时遇到了任何问题或者对镜像有任何疑问，请通过 **[GitHub issue](https://github.com/docker-library/rails/issues)** 联系我们。

您也可以通过 **[Freenode](https://freenode.net)** 上的 IRC 频道 `#docker-library` 来接触多数官方镜像的维护者。

## 贡献

我们邀请您贡献新的功能、修复、和更新（无论大小）；我们总是很高兴能收到拉取请求，同时我们也会尽最快的速度去处理它们。

在您开始编码之前，我们建议您先浏览下 **[GitHub issue](https://github.com/docker-library/rails/issues)**，尤其是那些有野心的贡献。这将会给其他的贡献值一个给您指引正确方向的机会，并对您的设计提供反馈和帮助您找到是否有其他人正在做相同的事情。
