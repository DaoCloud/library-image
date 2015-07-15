# Registry
> 此镜像从[DockerHub](https://registry.hub.docker.com/_/registry/)同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/registry)。

镜像 Tag >= 2 为[新版的Registry](https://github.com/docker/distribution)

其他 Tag 则为[老版本的Registry](https://github.com/docker/docker-registry)

## 启动 Registry

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

可以参考[安装文档](http://docs.docker.io/installation/#installation)安装 Docker

### 快速启动 Registry
* 启动 Registry：`docker run -p 5000:5000 -v <HOST_DIR>:/tmp/registry-dev daocloud.io/library/registry`
* 修改 docker deamon 启动脚本，增加： `-H tcp://127.0.0.1:2375 -H unix:///var/run/docker.sock –insecure-registry <REGISTRY_HOSTNAME>:5000`

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。



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
         daocloud.io/library/registry
```

注意：上面的示例中容器试图映射主机的5000端口，如果该端口已经被占用，可以映射其他空闲端口。


## 支持的 Docker 版本

这个镜像在 Docker1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker(1.0之后)也能提供基本的兼容。 

## 该翻译的许可证

<span style="font-size: 75%; text-align: center; display: block;"><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/88x31.png" /></a>本作品由 DaoCloud 翻译并采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/">知识共享署名-非商业性使用-相同方式共享 3.0 未本地化版本许可协议</a>进行许可。</span>
