# Node

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/u/library/node/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/node)。

## 什么是 Node.js？

Node.js 是一个基于 Chrome JavaScript 运行时建立的平台，用于方便地搭建响应速度快、易于扩展的网络应用。 Node.js 使用事件驱动，非阻塞 I/O 模型而得以轻量和高效，非常适合在分布式设备上运行的数据密集型的实时应用。

>来自[百度百科](http://baike.baidu.com/view/3974030.htm)


## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 在您的 Node.js 应用项目中创建一个 `Dockerfile`

```
FROM daocloud.io/node:0.10-onbuild
# replace this with your application's default port
EXPOSE 8888
```

然后您可以构建并且运行该 Docker 镜像

```
docker build -t my-nodejs-app .
docker run -it --rm --name my-running-app my-nodejs-app
```

### 注意

在该镜像中假定您的应用中有一个名为 `package.json` 的文件，其中列出了项目依赖以及定义了入口脚本

## 运行单个 Node.js 脚本

对于许多简单的单一文件项目，你会发现写一个完整的 `Dockerfile` 是不合适的。 所以，您可以直接通过使用 Node.js 镜像来运行一个 Node.js 脚本。

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp 
daocloud.io/node:0.10 node your-daemon-or-script.js
```

## 镜像的版本

Node 的镜像有多个版本，分别适用于不同的场合。

### node:<version>

这是标准的 Node 镜像分支，如果你没有什么特别的需求，默认使用这个分支即可。这个分支被用于常规的容器创建，同时也用来作为构建其它镜像的基础镜像。 `buildpack-deps` 方案被应用于此分支，这是一种专门为在系统中大范围使用 Docker 镜像的用户而设计的方案。通过提供大量的常用的 dpkg 应用软件包，减少了软件包的安装量，进而降低了整个系统内的镜像总大小。

### node:onbuild

这个分支可以用来很轻松地创建衍生镜像。在多数应用场景下，只需在项目中的 Dockfile 里添加一行 `FROM: daocloud.io/node:onbuild` 就完全可以创建一个独立的镜像。

### node:slim

这是一个精简分支，它仅仅包含了可以保证 node 运行的最少软件包。 除非您的部署环境对镜像尺寸大小的要求比较苛刻，否则我们强烈建议使用默认分支。

## 支持的Docker版本

这个镜像在 Docker 1.7.1 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">署名-非商业性使用-禁止演绎</a>进行许可。
