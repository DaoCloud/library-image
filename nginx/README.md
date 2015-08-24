# Nginx

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/nginx/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/nginx)。

## 什么是 Nginx？

Nginx 是一款轻量级的 Web 服务器、反向代理服务器、及电子邮件（IMAP/POP3）代理服务器，并在一个 BSD-like 协议下发行。由俄罗斯的程序设计师 Igor Sysoev 所开发，供俄国大型的入口网站及搜索引擎 Rambler（俄文：Рамблер）使用。其特点是占有内存少，并发能力强，事实上 Nginx 的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用 Nginx 网站用户有：新浪、网易、腾讯等。

>来自[百度百科](http://baike.baidu.com/view/926025.htm)


## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 托管静态网页内容

```
docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d daocloud.io/nginx
```

另外一种比上面绑定 volume 更推荐的做法是用`Dockerfile`生成包含网页内容的新镜像，如下所示：

```
FROM daocloud.io/nginx
COPY static-html-directory /usr/share/nginx/html
```

把上面的`Dockerfile`和您的网页内容（static-html-directory）放在同一目录下，然后运行命令生成新镜像：

```
docker build -t some-content-nginx .
```

最后启动容器：

```
docker run --name some-nginx -d some-content-nginx
```

### 暴露端口

```
docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

这样启动，您就可以通过 `http://localhost:8080` 或者 `http://宿主 IP:8080` 访问 Nginx 了。


### 进阶配置
```
docker run --name some-nginx -v /some/nginx.conf:/etc/nginx/nginx.conf:ro -d daocloud.io/nginx
```

> 了解详细的 Nginx 配置文件语法，请参考：[官方文档](http://nginx.org/en/docs/)。

为了确保 Nginx 容器能够持续运行，请务必在您自定义的 Nginx 配置文件中包含`deamon off`配置项。 

下面的命令从一个正在运行的 Nginx 容器中复制出配置文件：

```
docker cp some-nginx:/etc/nginx/nginx.conf /some/nginx.conf
```

您也可以通过推荐的`Dockerfile`方式来生成一个包含自定义配置文件的镜像，如下所示：

```
FROM daocloud.io/nginx
COPY nginx.conf /etc/nginx/nginx.conf
```

再用下面的命令构建镜像：

```
docker build -t some-custom-nginx . 
```

最后启动容器：

```
docker run --name some-nginx -d some-custom-nginx
```

## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

本作品采用[署名-非商业性使用-禁止演绎](http://creativecommons.org/licenses/by-nc-nd/4.0/)进行许可。
