# Fedora

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/fedora/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/fedora)。

![image](https://raw.githubusercontent.com/docker-library/docs/master/fedora/logo.png)

### Fedora

该镜像包括官方 Fedora 22, Fedora 21 和 半官方的 Fedora 20 (heisenbug)。

`fedora:latest`会一直指向到最新的稳定版本。目前的稳定版本是 Fedora 22。因此`fedora:latest`和`fedora:22`是同一个镜像.

Fedora rawhide 可以通过`fedora:rawhide`获得。Fedora 20 可以通过`fedora:20`和`fedora:heisenbug`获得。 目前 Fedora 20 已经不再维护了，最后一个补丁是 CVE-2015-4000 (Logjam).

http://mirrors.fedoraproject.org 这个链接可以用于自动选择镜像源。`docker build`和`docker run`的时候都可以使用。

```
$ docker run fedora cat /etc/yum.repos.d/fedora.repo | grep metalink
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-$releasever&arch=$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-debug-$releasever&arch=$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-source-$releasever&arch=$basearch
```

### 支持的 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。
