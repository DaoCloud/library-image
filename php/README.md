# PHP

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/php/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源维护在 [Github](https://github.com/docker-library/official-images/blob/master/library/php)。


## 什么是 PHP ？

PHP（全称：PHP: Hypertext Preprocessor，中文名：「超文本预处理器」）是一种通用开源脚本语言。语法吸收了 C 语言、Java 和 Perl 的特点，利于学习，使用广泛，主要适用于 Web 开发领域。PHP  独特的语法混合了 C、Java、Perl 以及 PHP 自创的语法。它可以比 CGI 或者 Perl 更快速地执行动态网页。用 PHP 做出的动态页面与其他的编程语言相比，PHP 是将程序嵌入到 HTML（标准通用标记语言下的一个应用）文档中去执行，执行效率比完全生成 HTML 标记的 CGI 要高许多；PHP 还可以执行编译后代码，编译可以达到加密和优化代码运行，使代码运行更快。

> 来自[百度百科](http://baike.baidu.com/subview/99/5828265.htm)


## 如何使用 PHP 镜像？

### 通过命令行

对于通过命令行界面 (CLI) 执行的 PHP 项目，您可以执行以下操作。

#### 在您的 PHP 项目中创建一个`Dockerfile`

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

对于很多简单的单文件项目，您可能发现写一个完整的`Dockerfile`很不方便。在这种情况下，您可以通过使用 PHP Docker 镜像来直接的执行 PHP 脚本：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp php:5.6-cli php your-script.php
```

### 使用 Apache

更常见的是，您也许想和 Apache httpd 一起执行 PHP 。方便的是，已经有了一个 PHP 容器版本打包了 Apache Web 服务器。

#### 在您的 PHP 项目中创建一个`Dockerfile`：

```
FROM php:5.6-apache
COPY src/ /var/www/html/
```

在`src/`文件夹包含了您全部的 PHP 代码。然后，通过执行命令来构建和运行 Docker 镜像：

```
docker build -t my-php-app .
docker run -it --rm --name my-running-app my-php-app
```

我们建议您添加自定义的`php.ini`配置文件。通过添加一行到 Dockerfile 将它`COPY`到`/user/local/etc/php`中并执行相同的命令来构建和运行：

```
FROM php:5.6-apache
COPY config/php.ini /usr/local/etc/php
COPY src/ /var/www/html/
```

在`src/`文件夹包含了您全部的 PHP 代码，`config/`包含了您的`php.ini`文件。

#### 如何安装更多的 PHP 扩展

我们提供了两款名为`docker-php-ext-configure`和`docker-php-ext-install`安装 PHP 扩展。

比如，如果您想有一个带`icov`,`mcrypt`和`gd`扩展的 PHP-FPM 镜像，您可以通过继承您喜欢的基础镜像，并编写您自己的`Dockerfile`：

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

请记住，您必须手动安装扩展所需要的依赖。如果一个扩展需要自定义的配置参数，您可以像这个例子一样使用`docker-php-ext-configure`脚本。

#### 不使用`Dockerfile`

如果您不想在您的项目中引入`Dockerfile`，您可以执行以下操作：

```
docker run -it --rm --name my-apache-php-app -v "$PWD":/var/www/html php:5.6-apache
```

## 支持的 Docker 版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。 
