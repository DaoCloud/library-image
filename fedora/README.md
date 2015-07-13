# Fedora

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/nginx/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/fedora)。

### Fedora？

该镜像包括官方 Fedora 22, Fedora 21 和 半官方的 Fedora 20 (heisenbug)。

![image](https://raw.githubusercontent.com/docker-library/docs/master/fedora/logo.png)

fedora:latest 会一直指向到最新的稳定版本。目前的稳定版本是Fedora 22。因此 fedora:latest 和 fedora:22 是同一个镜像.

Fedora rawhide 可以通过 fedora:rawhide 获得。Fedora 20 可以通过 fedora:20 和 fedora:heisenbug 获得。 目前 Fedora 20 已经不再维护了，最后一个补丁是 CVE-2015-4000 (Logjam).

http://mirrors.fedoraproject.org 这个链接可以用于自动选择镜像源。 Build和Run的时候都可以用。

```
$ docker run fedora cat /etc/yum.repos.d/fedora.repo | grep metalink
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-$releasever&arch=$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-debug-$releasever&arch=$basearch
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-source-$releasever&arch=$basearch
```

### 支持的Docker版本

该镜像支持Docker 1.7.0 
对于老版本(低于1.0)，尽可能的支持。


### 用户反馈


#### 文档

原始的文档，在[Github](https://github.com/docker-library/docs/tree/master/fedora)。
在您提交Pull Request之前建议阅读文档 [README.md](https://github.com/docker-library/docs/blob/master/README.md)

对于翻译的文档。可以在DaoCloud客服系统中提交。

#### 问题

度过您对该镜像有什么问题。可以提供Bug在 [Fedora's bugzilla](https://bugzilla.redhat.com/enter_bug.cgi?product=Fedora)。

您也可以在 [Freenode](https://freenode.net/) 的 #docker-library IRC 频道下讨论