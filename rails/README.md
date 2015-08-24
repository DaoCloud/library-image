# Rails

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/rails/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/rails)。

## 什么是 Ruby on Rails？

Ruby on Rails 或 Rails，是一个用 Ruby 运行的开源互联网应用框架。它是一个全栈框架。这就意味着它是「开箱即用」的，Rails 能够创建可以从网站服务器获取请求信息、连接和查询数据库、并且渲染模板的页面和应用。因此 Rails 还自带一个独立于网站服务器的路由体系。

> 来自[维基百科](https://en.wikipedia.org/wiki/Ruby_on_Rails)

## 如何使用这个镜像？
> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 在您的 Rails 项目中创建一个`Dockerfile`

```
FROM daocloud.io/rails:onbuild
```

将这个文件置于您应用的顶层目录，与`Gemfile`在同一个目录下。

这个镜像包含了应该适用于大部分项目的多个`ONBUILD`触发器。这将会在构建时`COPY . /usr/src/app`，`RUN bundle install`，`EXPOSE 3000`，并且将默认的命令设置为`rails server`。

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

### 生成一个`Gemfile.lock`

标签`ONBUILD`期望在您的应用目录中找到一个`Gemfile.lock`文件。 下面的`docker run`指令将会帮您生成一个。请在您应用的顶级目录（`Gemfile`所在的目录）下执行这个指令：

```
docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app daocloud.io/ruby:2.1 bundle install
```

### 创建一个新的 Rails 应用

如果您想为一个新的 Rails 项目生成一个目录结构，您可以这样：

```
docker run -it --rm --user "$(id -u):$(id -g)" -v "$PWD":/usr/src/app -w /usr/src/app daocloud.io/rails rails new webapp
```

这将会在您的当前目录下创建一个叫做`webapp`的子目录。

## 镜像的版本

`rails` 镜像带有很多不同标签的版本，每个版本都为一个特定的用例设计。

### `rails:<版本号>`

这是标准镜像。如果您不知道应该用哪个版本，您基本上应该选择这个。它即可以启动为一个用过就丢弃的容器（用来挂载您的源码和用容器启动您的应用），也可以当做另一个镜像的基础镜像。

### `rails:onbuild`

这个镜像使得构建衍生镜像变得更加容易。大部分项目中，在您的项目的顶级目录中创建一个以`FROM daocloud.io/rails:onbuild`开头的`Dockerfile`已经足够让您创建一个独立的镜像了。

然而`ONBUILD`版本的真正用意在于创建一个构建好就可以跑的镜像（短期内从零到容器化），由于缺少对*何时*`ONBUILD`操作会触发的掌控，并不推荐在项目里长期使用这个版本。

> 参考 **[`docker/docker#5714`](https://github.com/docker/docker/issues/5714)**，**[`docker/docker#8240`](https://github.com/docker/docker/issues/8240)**，**[`docker/docker#11917`](https://github.com/docker/docker/issues/11917)**

一旦您掌握了您的项目是如何在 Docker 里工作的，您可能想要将您的`Dockerfile`的基础镜像调整为一个非`ONBUILD`版本。并且拷贝`ONBUILD`版本中的命令到新的`Dockerfile`中（将带有`ONBUILD`的指令移动到文件底部并且移除`ONBUILD`关键词），这样您就有了对触发器何时触发的控制并且使得您自己和其他阅读您`Dockerfile`的人们更容易读懂。

同时这也会使随着时间的推移添加额外的需求变得更加容易（比如在执行`ONBUILD`前安装更多的软件包）。

## 支持的 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

本作品采用[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/4.0/)进行许可。
