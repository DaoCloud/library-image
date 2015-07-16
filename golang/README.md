# Golang
> 此镜像从[Docker Hub](https://registry.hub.docker.com/_/golang/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。	
> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/golang)。

## 什么是 Go?

Go 是 Google 开发的一种编译型，可平行化，并具有垃圾回收功能的编程语言。罗伯特·格瑞史莫（Robert Griesemer），罗勃·派克（Rob Pike）及肯·汤普逊于 2007 年 9 月开始设计 Go 语言，稍后 Ian Lance Taylor, Russ Cox 加入项目中。Go 语言是基于 Inferno 操作系统所开发的。Go 语言于 2009 年 11 月正式宣布推出，成为开放源代码项目，并在 Linux 及 Mac OS X 平台上进行了实现，后追加 Windows 系统下的实现。

> 来自[百度百科](http://baike.baidu.com/view/9257526.htm)

## 如何使用这个镜像?

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 在你的应用中启动一个 Go 实例

你可以使用本镜像来创建一个 Go 的容器作为应用的构建环境，也可作为运行环境。在你的`Dockerfile`中写下以下代码行，Docker 会编译并且运行你的项目：

```
FROM daocloud.io/golang:1.3-onbuild
```

上面指定的这个镜像包含'ONBUILD'触发器，这个构建会依次执行`COPY . /usr/src/app`，`RUN go get -d -v`，和`RUN go install -v`。

这个镜像也包含`CMD ["app"]`指令，表示应用的默认启动命令，不带任何参数。

接着，你可以构建并运行你的 Docker 镜像：

```
docker build -t my-golang-app .
docker run -it --rm --name my-running-app my-golang-app
```

### 在 Docker 容器中编译你的应用

某些情况下，你并不需要在容器中运行你的应用，以下命令将使你在容器中编译，而不运行你的应用：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/golang:1.3 go build -v
```

这个命令将当前目录作为一个 volume 挂载进容器，并把这个 volume 设置成工作目录，然后运行命令`go build`来编译应用。如果你的工程包含一个`Makefile`，你也可以运行一下命令：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/golang:1.3 make
```

### 在 Docker 容器中交叉编译

如果你的应用并非运行在 linux/amd64 上，比如 windows/386， 你同样可以利用 Docker 来编译：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp -e GOOS=windows -e GOARCH=386 daocloud.io/golang:1.3-cross go build -v
```

当然，你也可以使用以下脚本一次编译多个平台的版本：

```
docker run --rm -it -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/golang:1.3-cross bash
$ for GOOS in darwin linux; do
>   for GOARCH in 386 amd64; do
>     go build -v -o myapp-$GOOS-$GOARCH
>   done
> done
```

## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">署名-非商业性使用-禁止演绎</a>进行许可。
