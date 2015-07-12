# Python
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/python/)同步并提供中文文档支持，用来帮助国内开发者更方便的使用Docker镜像。

### 什么是 Python

Python是一种解释型、交互性、面向对象的开源编程语言。它集成了众多的高级功能，比如：模块、异常处理、动态输入、高级的动态数据以及动态类。Python兼备异常清晰的语法。Python拥有丰富的系统调用接口和库，还包括各式各样的窗口系统，同时Python还可扩展使用C语言和C++语言。对于需要编程接口的应用而言，Python还可以作为一种扩展语言进行使用。最后，Python还具备强大的移植性：它可以很方便地运行在很多的Unix系统、Mac OS、以及Windows 2000及后续版本平台上。

###如何使用Python镜像

####在你的Python应用项目下创建一个`Dockerfile`文件
```
FROM python:3-onbuild
CMD [ "python", "./your-daemon-or-script.py" ]
```

或者（如果你需要使用Python 2）:

```
FROM python:2-onbuild
CMD [ "python", "./your-daemon-or-script.py" ]
```

这些景象都包含了多个`ONBUILD`触发器，这些触发器在加载大多数应用时满足你的需求。构建部分回`COPY`一个`requirements.txt`文件，在Dockerfile中`RUN pip install`，然后将当前目录拷贝至`/usr/src/app`。

然后，你就可以构建和运行Docker镜像了：

```
docker build -t my-python-app .
docker run -it --rm --name my-running-app my-python-app
```

####运行单个Python脚本

对于很多简单的单个文件项目而言，编写一个完整的`Dockerfile`会带来些许的不方便。在这种情况下，你可以通过直接使用Docker镜像来运行Python脚本：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp python:3 python your-daemon-or-script.py
```

或者（如果你需要使用Python 2）：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp python:2 python your-daemon-or-script.py
```

###镜像标签含义

`Python`镜像会有很多的形式，每一种形式都用以一个特殊的用途.

`python:<version>`

这是最原生的Python镜像。如果你不确定需要什么类型的Python镜像，那么你最有可能需要使用的就是这样的镜像。这类镜像设计的初衷，就是为了既满足直接运行的容器（挂载你的Python源代码并直接启动容器来运行你的应用），又满足基础镜像的需求，来构建其他的镜像。这些标签是`buildpack-deps`的衍生和成熟版本。`buildpacks－deps`是设计用来满足那些在系统中拥有很多奖项的大众化docker用户的。从设计原则出发，它会包含很多冗余的Debian软件包。这将减少镜像在再构建时所需安装的软件数量，同时降低镜像在你系统中的存储空间。

###Docker的支持版本

这个镜像在 Docker1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker(1.0之后)也能提供基本的兼容。