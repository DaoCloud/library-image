
# Ubuntu
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/ubuntu/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/ubuntu)。

## 什么是Ubuntu?

Ubuntu 是基于Debian GNU/Linux，支持x86、amd64（即x64）和ppc架构，由全球化的专业开发团队（Canonical Ltd）打造的开源GNU/Linux操作系统。为桌面虚拟化提供支持平台。Ubuntu对GNU/Linux的普及特别是桌面普及作出了巨大贡献，由此使更多人共享开源的成果与精彩。（来自[百度百科](http://baike.baidu.com/view/4236.htm))

## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### ubuntu:14.04

```
$ docker run daocloud.io/library/ubuntu:14.04 grep -v '^#' /etc/apt/sources.list
```

### ubuntu:12.04

```
$ docker run daocloud.io/library/ubuntu:12.04 cat /etc/apt/sources.list
```

## 支持的Docker版本

这个镜像在Docker1.7.0上提供最佳的官方支持，对于其他老版本的Docker(1.0之后)也能提供基本的兼容。

## 该翻译的许可证

<span style="font-size: 75%; text-align: center; display: block;"><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/88x31.png" /></a>本作品由 DaoCloud 翻译并采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/">知识共享署名-非商业性使用-相同方式共享 3.0 未本地化版本许可协议</a>进行许可。</span>