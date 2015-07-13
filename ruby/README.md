# Ruby
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/ruby/)
同步并提供中文文档支持，用来帮助国内开发者更方便的使用Docker镜像。

### 什么是 Ruby?

Ruby 是一个动态的，反射的，面向对象的，多用途的，开源的编程语言。根据它的作者所说，Ruby 是受Perl，Smalltalk， Eiffel， Ada，和 Lisp的影响。它支持多个编程范式，包括函数式，面向对象式，命令式。它也有一个动态类型系统和自动化内存管理。（来自[维基百科](wikipedia.org/wiki/Ruby)）

### 如何使用这个镜像？

#### 创建一个 `Dockerfile` 在你的ruby 项目

```
FROM daocloud.io/library/ruby:2.1-onbuild
CMD ["./your-daemon-or-script.rb"]

```
放置这个 `Dockerfile` 在你的app的根目录下，和 `Gemfile` 在一起

这个镜像包含多个构件时触发器，这些触发器包含了所有的你启动应用启动所需要的引导程序。触发器将 `COPY . /usr/src/app` 和 `RUN bundle install` 。

你能构建和运行 Ruby 镜像

```
docker build -t my-ruby-app .
docker run -it --name my-running-script my-ruby-app

```


#### 生成一个 Gemfile.lock 文件
`onbuild` 标签 期望一个 `Gemfile.lock` 文件在你的app目录。 这个 docker run 将帮组你生成一个 `Gemfile.lock` 文件。运行这个命令在你应用的根目录，和 `Gemfile` 在一起

```
docker run --rm -v "$PWD":/usr/src/app -w /usr/src/app daocloud.io/library/ruby:2.1 bundle install
```

#### 运行一个单独的Ruby 脚本

对于很多简单，单文件的工程，你可能发现写一个完整的 `Dockerfile` 很不方便。对于这些案例，你能直接用Ruby Docker的镜像直接运行一个Ruby 脚本

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/library/ruby:2.1 ruby your-daemon-or-script.rb
```

### 镜像多样化

Ruby 镜像 有很多不同的分类，每种分类是为特定案例所设计的。

`
ruby:<version>
`

这个是默认镜像，如果你不确定你需要哪种镜像，你可能想去使用这个镜像。这个镜像被设计用着丢弃容器（绑定你的源代码和启动容器去运行程序），也可以作为基本镜像去构建其它镜像。这个标签的镜像是基于 `buildpack-deps` 。 `buildpack-deps` 是专门为docker的普通使用者设计的。通常这些使用者有很多镜像在他们的系统上。它有很多的Debain的常用包。这个将减少他们在构建镜像时所需要拉取的包数量，因此也就减少了你系统上所有镜像的整体的大小。


`
ruby:onbuild
`

这个镜像使得构建派生镜像更简单。对应很多用例，在你的工程中创建一个带有 `FROM daocloud.io/library/ruby:onbuild` 的 `Dockerfile` 去构建一个独立的镜像就已经足够了。

但是 `onbuild` 对于那些不想去docker化应用或者在很短时间内就可以用docker来运行的用户很有用。但这种使用方式长期来说不被推荐，因为很多控制权都在 `ONBUILD` 时运行。（参考 `docker/docker#5714`，`docker/docker#8240`，`docker/docker#11917`）。

一旦你知道了怎样用docker去处理你应用程序的功能，你可能想去调整你的Dockerfile去继承一个非 `onbuild` 类型和从 `onbuild` 的 `Dockerfile` 里复制命令到你自己的文件中，以至于对于你或者其他人在看你的 `Dockerfile` 时会有更多的控制权和更高的透明度。这个也使得在后期添加更多其它的需求变得更加容易。


`
ruby:slim
`

这个镜像不包含那些被包含在默认标签镜像中的那些包，仅仅包含运行 `ruby` 所需要的最小化的包。除非你的工作环境只运行ruby和你的空间有限，否则我们强烈推荐使用这个仓库的默认镜像。



### 支持的Docker版本

这个镜像在Docker1.7.0上提供最佳的官方支持，对于其他老版本的Docker(1.0之后)也能提供基本的兼容。