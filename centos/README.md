# CentOS
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/centos/)同步并提供中文文档支持，用来帮助国内开发者更方便的使用Docker镜像。

### CentOS

CentOS 是一个基于 RedHat Linux 提供的可自由使用源代码的企业级Linux发行版本。每个版本的 CentOS 都会获得十年的支持（通过安全更新方式）。 新版本的 CentOS 大约每两年发行一次，而每个版本的 CentOS 会定期（大概每六个月）更新一次，以便支持新的硬件。 这样，建立一个安全、低维护、稳定、高预测性、高重复性的 Linux 环境。 CentOS是Community Enterprise Operating System的缩写。 （来自[百度百科](http://baike.baidu.com/view/26404.htm))

### CentOS 镜像

centos:latest 总是指向了最新的可用版本。

#### 持续构建

CentOS 项目会对所有活跃操作系统版本进行定期的更新，这些镜像会每月更新或者针对紧急情况立刻更新。这些持续构建的镜像只会打上主版本标签，比如 

```
docker pull daocloud.io/library/centos:6

docker pull daocloud.io/library/centos:7
```

#### 小版本标签

除此之外，还会根据操作系统厂商提供的不同版本提供包括小版本的镜像。请注意，这些小版本的镜像一旦推出就不会更新了。 如果您选择这些镜像，强烈推荐您在 Dockerfile 里包括 `RUN yum -y update && yum clean all`， 否则有可能会有安全隐患。 这些镜像的使用方式如下：

```
docker pull daocloud.io/library/centos:5.11
```

### 包管理

默认情况下，为了减小镜像的尺寸，在构建 CentOS 镜像时用了 `yum` 的 `nodocs` 选项。 如果您安装一个包后发现文件缺失，请在`/etc/yum.conf` 中注释掉 `tsflogs=nodocs` 并重新安装您的包。 

### systemd 整合

当前，因为 systemd 要求 CAP_SYS_ADMIN 权限，从而得到了读取主机 cgroup 的能力，CentOS7 中已经用 fakesystemd 代替了 systemd 来解决依赖问题。 如果您仍然希望使用 systemd，可用参考下面的 Dockerfile：

```
FROM daocloud.io/library/centos:7
MAINTAINER "you" <your@email.here>
ENV container docker
RUN yum -y swap -- remove fakesystemd -- install systemd systemd-libs
RUN yum -y update; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i ==
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
```

上面这个 Dockerfile 首先删除了 fakesystemd 并且安装了 systemd。然后您就可以构建基础镜像了。

```
docker build --rm -t local/c7-systemd .
```

#### 一个包含 systemd 的应用容器示例

为了使用像上面那样包含 systemd 的容器，你需要创建一个类似下面的 Dockerfile

```
FROM local/c7-systemd
RUN yum -y install httpd; yum clean all; systemctl enable httpd.service
EXPOSE 80
CMD ["/usr/sbin/init"]
```

构建镜像:

```
docker build --rm -t local/c7-systemd-httpd
```

#### 运行一个包含 systemd 的应用容器

为了运行一个包含 systemd 的容器，您需要使用 `--privileged` 选项， 并且挂载主机的 cgroups 文件夹。 下面是运行 包含 systemd 的 httpd 容器的示例命令： 

```
docker run --privileged -ti -v /sys/fs/cgroup:/sys/fs/cgroup:ro -p 80:80 local/c7-systemd-httpd
```

