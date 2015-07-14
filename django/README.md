# Django

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/django/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/django)。

### 什么是 Django？

Django是一个开放源代码的Web应用框架，由Python写成。采用了MVC的软件设计模式，即模型M，视图V和控制器C。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。并于2005年7月在BSD许可证下发布。这套框架是以比利时的吉普赛爵士吉他手Django Reinhardt来命名的。

> [维基百科](https://zh.wikipedia.org/wiki/Django)


### 使用方式

在你的Django项目中创建 `Dockerfile`

```
FROM django:onbuild
```

把这个文件放到项目的根目录

这个镜像包含很多 `ONBUILD`，处理了大部分情况。构建时会执行 `COPY . /usr/src/app`， `RUN pip install`， `EXPOSE 8000`，并且设置默认的执行命令: `python manage.py runserver`。

然后你可以构建并运行镜像:

```
docker build -t my-django-app .
docker run --name some-django-app -d my-django-app
```

你可以访问 `http://container-ip:8000` 来测试，如果想要通过主机ip来访问(如 `http://localhost:8000`)，要执行下面的命令:

```
docker run --name some-django-app -p 8000:8000 -d my-django-app
```

### 不通过 Dockerile 使用

你可以直接使用 `docker run` 来避免在项目中添加 `Dockerfile`

```
docker run --name some-django-app -v "$PWD":/usr/src/app -w /usr/src/app -p 8000:8000 -d django bash -c "pip install -r requirements.txt && python manage.py runserver 0.0.0.0:8000"
```

生成一个 django 项目

```
docker run -it --rm --user "$(id -u):$(id -g)" -v "$PWD":/usr/src/app -w /usr/src/app django django-admin.py startproject mysite
This will create a sub-directory named mysite inside your current directory.
```

### 支持 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。
