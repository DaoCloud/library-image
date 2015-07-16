# Django

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/django/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/django)。

## 什么是 Django？

Django 是一个开放源代码的 Web 应用框架，由 Python 写成。采用了MVC的软件设计模式，即模型M，视图V和控制器C。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的，即是 CMS（内容管理系统）软件。并于 2005 年 7 月在 BSD 许可证下发布。这套框架是以比利时的吉普赛爵士吉他手 Django Reinhardt 来命名的。

> 来自[百度百科](http://baike.baidu.com/subview/962167/9372788.htm)


## 如何使用这个镜像？

> 因所有镜像均位于境外服务器，为了确保所有示例能正常运行，DaoCloud 提供了一套境内镜像源，并与官方源保持同步。

### 创建 Dockerfile（推荐）

```
FROM daocloud.io/django:onbuild
```

把这个文件放到项目的根目录。

这个镜像包含很多`ONBUILD`，处理了大部分情况。构建时会执行`COPY . /usr/src/app`，`RUN pip install`，`EXPOSE 8000`，并且设置默认的执行命令：`python manage.py runserver`。

然后你可以构建并运行镜像:

```
docker build -t my-django-app .
docker run --name some-django-app -d my-django-app
```

你可以访问`http://容器 IP:8000` 来测试，如果想要通过主机 IP 来访问（如 `http://localhost:8000`），要执行下面的命令:

```
docker run --name some-django-app -p 8000:8000 -d my-django-app
```

### 不创建 Dockerile

你可以直接使用`docker run`创建。

```
docker run --name some-django-app -v "$PWD":/usr/src/app -w /usr/src/app -p 8000:8000 -d daocloud.io/django bash -c "pip install -r requirements.txt && python manage.py runserver 0.0.0.0:8000"
```

生成一个 django 项目

```
docker run -it --rm --user "$(id -u):$(id -g)" -v "$PWD":/usr/src/app -w /usr/src/app daocloud.io/django django-admin.py startproject mysite
```
上面的命令会在你当前目录下产生一个名为`mysite`的子文件夹。

## 支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

## 该翻译的许可证

<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/80x15.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">署名-非商业性使用-禁止演绎</a>进行许可。
