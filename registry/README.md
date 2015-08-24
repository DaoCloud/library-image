# Docker Registry

> 此镜像从[Docker Hub](https://registry.hub.docker.com/_/registry/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/registry)。

镜像标签高于「2」的为[新版的Registry](https://github.com/docker/distribution)。

其他标签则为[老版本的Registry](https://github.com/docker/docker-registry)。

## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

可以参考[安装站点](http://get.daocloud.io/)安装 Docker

### 快速启动 Registry

* 启动 Registry：

`docker run -p 5000:5000 -v <HOST_DIR>:/tmp/registry daocloud.io/registry`

* 修改 docker deamon 启动脚本，增加： 

`-H tcp://127.0.0.1:2375 -H unix:///var/run/docker.sock –insecure-registry <REGISTRY_HOSTNAME>:5000`


### 推荐的方式启动 Registry

```
docker run \
         -e SETTINGS_FLAVOR=s3 \
         -e AWS_BUCKET=acme-docker \
         -e STORAGE_PATH=/registry \
         -e AWS_KEY=AKIAHSHB43HS3J92MXZ \
         -e AWS_SECRET=xdDowwlK7TJajV1Y7EoOZrmuPEJlHYcNP2k4j49T \
         -e SEARCH_BACKEND=sqlalchemy \
         -p 5000:5000 \
         daocloud.io/registry
```

注意：上面的示例中容器试图映射主机的 5000 端口，如果该端口已经被占用，可以映射其他空闲端口。

## 支持的 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

本作品采用[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/4.0/)进行许可。
