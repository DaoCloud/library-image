# PHP

> 此镜像从 [Docker Hub](https://registry.hub.docker.com/_/php/) 同步并由 DaoCloud 提供中文文档支持，用来帮助国内开发者更方便的使用 Docker 镜像。

> 该镜像源由 Nginx 公司维护在 [Github](https://github.com/nginxinc/docker-nginx)。

### 什么是 Nginx？

Nginx 是一款轻量级的 Web 服务器、反向代理服务器、及电子邮件（IMAP/POP3）代理服务器，并在一个 BSD-like 协议下发行。由俄罗斯的程序设计师 Igor Sysoev 所开发，供俄国大型的入口网站及搜索引擎 Rambler（俄文：Рамблер）使用。其特点是占有内存少，并发能力强，事实上 Nginx 的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用 Nginx 网站用户有：新浪、网易、腾讯等。
>来自：[百度百科](http://baike.baidu.com/view/926025.htm)


### 如何使用 PHP 镜像？

#### 通过命令行

对于通过命令行界面（CLI）执行的 PHP 项目，您可以根据以下进行操作。

#### 在你的 PHP 项目中创建一个 `Dockerfile`

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

对于很多简单的单文件项目，您可能发现写一个完整的 ｀Dockerfile｀ 很不方便。在这种情况下，您可以通过使用 PHP Docker 镜像来直接的执行 PHP 脚本：

```
docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp php:5.6-cli php your-script.php
```

### With Apache

更常见的事，您也许想和 Apache httpd 一起执行 PHP 。幸运的事，已经有了一个 PHP 容器版本打包了 Apache Web 服务器。

#### 在你的 PHP 项目中创建一个 `Dockerfile`

```
FROM php:5.6-apache
COPY src/ /var/www/html/
```

`src/` 文件夹包含了您全部的 PHP 代码。然后，然后，通过执行命令来构建和运行 Docker 镜像：

```
docker build -t my-php-app .
docker run -it --rm --name my-running-app my-php-app
```

我们建议您添加自定义的 `php.ini` 配置文件。通过添加一行到 Dockerfile 将它 `复制` 到 `/user/local/etc/php` 中并执行相同的命令来构建和运行：

```
FROM php:5.6-apache
COPY config/php.ini /usr/local/etc/php
COPY src/ /var/www/html/
```

`src/` 文件夹包含了您全部的 PHP 代码， `config/` 包含了您的 `php.ini` 文件。


