# Java

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/java/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/java)。

### 什么是 Java？

Java 是由 Sun Microsystems 公司推出的 Java 面向对象程序设计语言（以下简称 Java 语言）和 Java 平台的总称。由 James Gosling 和同事们共同研发，并在 1995 年正式推出。Java 最初被称为 Oak，是1991年为消费类电子产品的嵌入式芯片而设计的。1995年更名为 Java，并重新设计用于开发 Internet 应用程序。用 Java 实现的 HotJava 浏览器（支持Java Applet）显示了 Java 的魅力：跨平台、动态 Web、Internet 计算。从此，Java 被广泛接受并推动了 Web 的迅速发展，常用的浏览器均支持 Java Applet。另一方面，Java 技术也不断更新。Java 自面世后就非常流行，发展迅速，对 C++ 语言形成有力冲击。在全球云计算和移动互联网的产业环境下，Java 更具备了显著优势和广阔前景。

>来自[百度百科](http://baike.baidu.com/subview/29/12654100.htm)


### 如何使用？

#### 在你的应用中启动一个 Java 实例

用这个镜像最简单的方式是直接用一个 Java 容器作为应用构建环境或者运行时环境。 可以像下面这样编写你的`Dockerfile`去编译和运行你的项目：

```
FROM daocloud.io/library/java:7
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
RUN javac Main.java
CMD ["java", "Main"]
```

然后你就可以构建和运行这个 Docker 镜像了：

```
docker build -t my-java-app .
docker run -it --rm --name my-running-app my-java-app
```

#### 在你的容器内编译应用

在某些情况下，在容器内部运行你的应用可能不是很合适。你可以像下面这样，只在容器内编译你的应用，但是并不运行应用：

```
docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp daocloud.io/library/java:7 javac Main.java
```

在上面的实例中，你的当前目录会以`volume`的形式挂接进容器，并且设置工作目录为 volume 目录，然后运行命令`javac Main.java`触发 Java 编译`Main.java`并指定输出文件为`Main.class`。

### 为什么是 OpenJDK/OpenJRE？

就像所用主流 Linux 发行版不会在它们的发行渠道中分发 Oracle Java 一样，我们也选择遵循这个惯例。 下面引用了一些发行版不分发 Oracle Java 的方式：

* Ubuntu 在 Oracle 失效了 "Operating System Distributor License for Java" 授权后停止分发`sun-java6`软件包 ([lists.ubuntu.com](https://lists.ubuntu.com/archives/ubuntu-security-announce/2011-December/001528.html))
* Debian 要求用户从 oracle.com 手动下载 Java tar 包，然后用 `java-package` 安装 ([wiki.debian.net](https://wiki.debian.org/Java/Sun))
*  Ubuntu 和 Debian 上的 webupd8 PPA 要求用户先接受 Oracle 授权才能用它们的软件下载和安装 Oracle Java ([webupd8.org](http://www.webupd8.org/2012/09/install-oracle-java-8-in-ubuntu-via-ppa.html))
* Gentoo 有一个获取限制 (fetch-restriction) 要求用户从 Oracle 网站手动下载 Java tar包并接受授权 ([wiki.gentoo.org](https://wiki.gentoo.org/wiki/Java))
* CentOS 要求用户从 java.com 下载 Oracle 提供的 rpm 包，然后接受 Oracle 授权。([wiki.centos.org](https://wiki.centos.org/HowTos/JavaRuntimeEnvironment))
* RedHat 提供如何增加一个 Oracle 维护的 repo 的说明 ([access.redhat.com](https://access.redhat.com/solutions/732883))

### 支持的 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。 
