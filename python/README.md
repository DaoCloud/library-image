# Python
> 此镜像从[Docker Hub](https://registry.hub.docker.com/_/python/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/python)。

## 什么是 Python?

Python 是一种解释型、交互性、面向对象的开源编程语言。它集成了众多的高级功能，比如：模块、异常处理、动态输入、高级的动态数据以及动态类。Python 兼备异常清晰的语法。Python 拥有丰富的系统调用接口和库，还包括各式各样的窗口系统，同时 Python 还可扩展使用 C 语言和 C++ 语言。对于需要编程接口的应用而言，Python 还可以作为一种扩展语言进行使用。最后，Python 还具备强大的移植性：它可以很方便地运行在很多的 Unix 系统、Mac OS、以及 Windows 2000 及后续版本平台上。

> 来自[百度百科](http://baike.baidu.com/view/21087.htm)

## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 在你的 Python 应用项目下创建一个`Dockerfile`文件

```
FROM daocloud.io/python:3-onbuild
CMD [ "python", "./your-daemon-or-script.py" ]
```

或者（如果你需要使用 Python 2）:

```
FROM daocloud.io/python:2-onbuild
CMD [ "python", "./your-daemon-or-script.py" ]
```

这些镜像都包含了多个`ONBUILD`触发器，在大部分情况下这些触发器能满足你的需求。构建部分会`COPY`一个`requirements.txt`文件，在 Dockerfile 中`RUN pip install`，然后将当前目录复制至`/usr/src/app`。

然后，你就可以构建和运行 Docker 镜像了：

```
docker build -t my-python-app .
docker run -it --rm --name my-running-app my-python-app
```

### 运行单个Python脚本

对于很多简单的单个文件项目而言，编写一个完整的`Dockerfile`会带来些许的不方便。在这种情况下，你可以通过直接使用 Docker 镜像来运行 Python 脚本：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/python:3 python your-daemon-or-script.py
```

或者（如果你需要使用 Python 2）：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/python:2 python your-daemon-or-script.py
```

## 镜像标签含义

Python镜像会有很多的形式，每一种形式都用以一个特殊的用途.

`daocloud.io/python:<version>`

这是最原生的 Python 镜像。如果你不确定需要什么类型的 Python 镜像，那么你最有可能需要使用的就是这样的镜像。这类镜像设计的初衷，就是为了既满足直接运行的容器（挂载你的 Python 源代码并直接启动容器来运行你的应用），又满足基础镜像的需求，来构建其他的镜像。这些标签是`buildpack-deps` 的衍生和成熟版本。`buildpacks－deps`是设计用来满足那些在系统中拥有很多镜像的 Docker 用户的。从设计原则出发，它会包含很多冗余的 Debian 软件包。这将减少镜像在再构建时所需安装的软件数量，同时降低镜像在你系统中的存储空间。

## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">署名-非商业性使用-禁止演绎</a>进行许可。
