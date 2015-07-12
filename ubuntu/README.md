
# Ubuntu
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/ubuntu/)同步并提供中文文档支持，用来帮助国内开发者更方便的使用Docker镜像。

### 什么是Ubuntu?

Ubuntu 是基于Debian GNU/Linux，支持x86、amd64（即x64）和ppc架构，由全球化的专业开发团队（Canonical Ltd）打造的开源GNU/Linux操作系统。为桌面虚拟化提供支持平台。Ubuntu对GNU/Linux的普及特别是桌面普及作出了巨大贡献，由此使更多人共享开源的成果与精彩。（来自[百度百科](http://baike.baidu.com/view/4236.htm))

### 如何使用这个镜像？

#### buntu:14.04

```
$ docker run daocloud.io/library/ubuntu:14.04 grep -v '^#' /etc/apt/sources.list
```

#### ubuntu:12.04

```
$ docker run daocloud.io/library/ubuntu:12.04 cat /etc/apt/sources.list
```

### 支持的Docker版本

这个镜像在Docker1.7.0上提供最佳的官方支持，对于其他老版本的Docker(1.0之后)也能提供基本的兼容。