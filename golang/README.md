# Golang
> 此镜像从[Docker Hub](https://registry.hub.docker.com/_/golang/)同步并提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/golang)。

### 什么是 Go

Go 是 Google 开发的一种编译型，可平行化，并具有垃圾回收功能的编程语言。

> 来自[百度百科](http://baike.baidu.com/view/9257526.htm)

### 如何使用 golang 镜像

#### 在你的应用中启动一个 Go 实例

你可以使用本镜像来创建一个 Go 的容器作为应用的构建环境，也可作为运行环境。在你的 Dockerfile 中写下以下代码行，docker会编译并且运行你的项目：

```
FROM golang:1.3-onbuild
```

上面指定的这个镜像包含 ONBUILD 触发器，这个构建会依次执行 COPY . /usr/src/app，RUN go get -d -v，和 RUN go install -v。

这个镜像也包含 CMD ["app"] 指令，表示应用的默认启动命令，不带任何参数。

接着，你可以构建并运行你的 Docker 镜像：

```
docker build -t my-golang-app .
docker run -it --rm --name my-running-app my-golang-app
```

#### 在 Docker 容器中编译你的应用

某些情况下，你并不需要在容器中运行你的应用，以下命令将使你在容器中编译，而不运行你的应用：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.3 go build -v
```

这个命令将当前目录作为一个 volume 挂载进容器，并把这个 volume 设置成工作目录，然后运行命令 go build 来编译应用。如果你的工程包含一个 Makefile，你也可以运行一下命令：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.3 make
```

#### 在 Docker 容器中交叉编译

如果你的应用并非运行在 linux/amd64 上，比如 windows/386， 你同样可以利用 Docker 来编译：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp -e GOOS=windows -e GOARCH=386 golang:1.3-cross go build -v
```

当然，你也可以使用以下脚本一次编译多个平台的版本：

```
docker run --rm -it -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.3-cross bash
$ for GOOS in darwin linux; do
>   for GOARCH in 386 amd64; do
>     go build -v -o myapp-$GOOS-$GOARCH
>   done
> done
```

### License

点击[这里](http://golang.org/LICENSE)可以查看本镜像的 license 信息。

### Supported Docker versions

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。 
