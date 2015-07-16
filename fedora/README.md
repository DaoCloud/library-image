# Fedora

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/fedora/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/fedora)。

## Fedora

Fedora 是一个知名的 Linux 发行版，是一款由全球社区爱好者构建的面向日常应用的快速、稳定、强大的操作系统。它允许任何人自由地使用、修改和重发布，无论现在还是将来。它由一个强大的社群开发，这个社群的成员以自己的不懈努力，提供并维护自由、开放源码的软件和开放的标准。Fedora 项目由 Fedora 基金会管理和控制，得到了 Red Hat, Inc. 的支持。

> 来自[百度百科](http://baike.baidu.com/view/182182.htm)

## 如何使用这个镜像?

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

该镜像包括官方 Fedora 22, Fedora 21 和 半官方的 Fedora 20 (heisenbug)。

标签`fedora:latest`会一直指向到最新的稳定版本。目前的稳定版本是 Fedora 22。因此标签`fedora:latest`和`fedora:22`是同一个镜像。

Fedora rawhide 可以通过`fedora:rawhide`获得。Fedora 20 可以通过`fedora:20`和`fedora:heisenbug`获得。 目前 Fedora 20 已经不再维护了，最后一个补丁是 CVE-2015-4000 (Logjam)。

Federa 的源位置可以通过设置 metalink 的位置到`http://mirrors.fedoraproject.org`来自动选择。

```
$ docker run daocloud.io/library/fedora cat /etc/yum.repos.d/fedora.repo | grep metalink
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-$releasever&arch=$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-debug-$releasever&arch=$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-source-$releasever&arch=$basearch
```

## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">署名-非商业性使用-禁止演绎</a>进行许可。
