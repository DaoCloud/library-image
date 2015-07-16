# Ruby
> 此镜像从[Docker Hub](https://registry.hub.docker.com/_/ruby/)
同步并提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/ruby)。

## 什么是 Ruby?

Ruby 是一个动态的，反射的，面向对象的，多用途的，开源的编程语言。根据它的作者所说，Ruby 是受 Perl，Smalltalk， Eiffel，Ada，和 Lisp 的影响。它支持多个编程范式，包括函数式，面向对象式，命令式。它也有一个动态类型系统和自动化内存管理。

> 来自[维基百科](wikipedia.org/wiki/Ruby)

## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 创建一个`Dockerfile`在你的 Ruby 项目

```
FROM daocloud.io/ruby:2.1-onbuild
CMD ["./your-daemon-or-script.rb"]

```

放置这个`Dockerfile`在你的 app 根目录下，和`Gemfile`在一起。

这个镜像包含多个`ONBUILD`触发器，这些触发器包含了所有的你启动应用启动所需要的引导程序。触发器将`COPY . /usr/src/app`和`RUN bundle install`。

你能构建和运行 Ruby 镜像：

```
docker build -t my-ruby-app .
docker run -it --name my-running-script my-ruby-app
```

### 生成一个 Gemfile.lock 文件

标签`ONBUILD`期望在您的应用目录中找到一个`Gemfile.lock`文件。 下面的`docker run`指令将会帮您生成一个。请在您应用的顶级目录（`Gemfile`所在的目录）下执行这个指令：

```
docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app daocloud.io/ruby:2.1 bundle install
```

### 运行一个单独的 Ruby 脚本

对于很多简单，单文件的工程，可以直接从本镜像直接运行一个 Ruby 脚本：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/ruby:2.1 ruby your-daemon-or-script.rb
```

## 镜像的版本

Ruby 镜像有很多不同的分类，每种分类是为特定案例所设计的。

### `ruby:<version>`

这个是默认镜像，如果你不确定你需要哪种镜像，你可能想去使用这个镜像。这个镜像被设计用着丢弃容器（绑定你的源代码和启动容器去运行程序），也可以作为基本镜像去构建其它镜像。这个标签的镜像是基于`buildpack-deps` 。 标签`buildpack-deps`是专门为 Docker 的普通使用者设计的。通常这些使用者有很多镜像在他们的系统上。它有很多的 Debain 的常用包。这个将减少他们在构建镜像时所需要拉取的包数量，因此也就减少了你系统上所有镜像的整体的大小。

### `ruby:onbuild`

这个镜像使得构建派生镜像更简单。对应很多用例，在你的工程中创建一个带有`FROM daocloud.io/ruby:onbuild`的`Dockerfile`去构建一个独立的镜像就已经足够了。

然而`ONBUILD`版本的真正用意在于创建一个构建好就可以跑的镜像（短期内从零到容器化），由于缺少对*何时*`ONBUILD`操作会触发的掌控，并不推荐在项目里长期使用这个版本。

> 参考 **[`docker/docker#5714`](https://github.com/docker/docker/issues/5714)**，**[`docker/docker#8240`](https://github.com/docker/docker/issues/8240)**，**[`docker/docker#11917`](https://github.com/docker/docker/issues/11917)**

一旦您掌握了您的项目是如何在 Docker 里工作的，您可能想要将您的`Dockerfile`的基础镜像调整为一个非`ONBUILD`版本。并且拷贝`ONBUILD`版本中的命令到新的`Dockerfile`中（将带有`ONBUILD`的指令移动到文件底部并且移除`ONBUILD`关键词），这样您就有了对触发器何时触发的控制并且使得您自己和其他阅读您`Dockerfile`的人们更容易读懂。

同时这也会使随着时间的推移添加额外的需求变得更加容易（比如在执行`ONBUILD`前安装更多的软件包）

### `ruby:slim`

这个镜像不包含那些被包含在默认标签镜像中的那些包，仅仅包含运行 Ruby 所需要的最小化的包。除非你的工作环境只运行 Ruby 和你的空间有限，否则我们强烈推荐使用这个仓库的默认镜像。


## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">署名-非商业性使用-禁止演绎</a>进行许可。
