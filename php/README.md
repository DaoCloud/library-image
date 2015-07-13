# PHP

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/php/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。


## 什么是 PHP ？

PHP（全称：PHP：Hypertext Preprocessor，即“PHP：超文本预处理器”）是一种开源的通用计算机脚本语言，尤其适用于网络开发并可嵌入HTML中使用。PHP的语法借鉴吸收C语言、Java和Perl等流行计算机语言的特点，易于一般程序员学习。PHP的主要目标是允许网络开发人员快速编写动态页面，但PHP也被用于其他很多领域。
>来自：[维基百科](https://zh.wikipedia.org/wiki/PHP)


## 如何使用 PHP 镜像？

### 通过命令行

对于通过命令行界面（CLI）执行的 PHP 项目，您可以执行以下操作。

#### 在您的 PHP 项目中创建一个 `Dockerfile`

```
FROM php:5.6-cli
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
CMD [ "php", "./your-script.php" ]
```

然后，通过执行命令来构建和运行 Docker 镜像：

```
docker build -t my-php-app .
docker run -it --rm --name my-running-app my-php-app
```

#### 执行一个单独的 PHP 脚本

对于很多简单的单文件项目，您可能发现写一个完整的 `Dockerfile` 很不方便。在这种情况下，您可以通过使用 PHP Docker 镜像来直接的执行 PHP 脚本：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp php:5.6-cli php your-script.php
```

### 使用 Apache

更常见的是，您也许想和 Apache httpd 一起执行 PHP 。方便的是，已经有了一个 PHP 容器版本打包了 Apache Web 服务器。

#### 在您的 PHP 项目中创建一个 `Dockerfile`

```
FROM php:5.6-apache
COPY src/ /var/www/html/
```

`src/` 文件夹包含了您全部的 PHP 代码。然后，通过执行命令来构建和运行 Docker 镜像：

```
docker build -t my-php-app .
docker run -it --rm --name my-running-app my-php-app
```

我们建议您添加自定义的 `php.ini` 配置文件。通过添加一行到 Dockerfile 将它 `COPY` 到 `/user/local/etc/php` 中并执行相同的命令来构建和运行：

```
FROM php:5.6-apache
COPY config/php.ini /usr/local/etc/php
COPY src/ /var/www/html/
```

`src/` 文件夹包含了您全部的 PHP 代码， `config/` 包含了您的 `php.ini` 文件。

#### 如何安装更多的 PHP 插件

我们提供了两款名为 `docker-php-ext-configure` 和 `docker-php-ext-install` 安装 PHP 插件。

比如，如果您想有一个带 `icov`, `mcrypt` 和 `gd` 插件的 PHP-FPM 镜像，您可以通过继承您喜欢的基础镜像，并编写您自己的 `Dockerfile` ：

```
FROM php:5.6-fpm
# Install modules
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd
CMD ["php-fpm"]
```

请记住，您必须手动安装插件所需要的依赖。如果一个插件需要自定义的 `配置` 参数，您可以像这个例子一样使用 `docker-php-ext-configure` 脚本。

#### 不使用 `Dockerfile`

如果您不想在您的项目中引入 `Dockerfile` ，您可以执行以下操作：

```
docker run -it --rm --name my-apache-php-app -v "$PWD":/var/www/html php:5.6-apache
```

## 支持的 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。 