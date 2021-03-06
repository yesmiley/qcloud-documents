本文为您介绍如何构建 PHP 容器。


## 步骤1：编写基础应用

1. 创建名为 `helloworld-php` 的新目录，并进入到此目录中：
```plaintext
mkdir helloworld-php
cd helloworld-php
```
2. 创建名为 `index.php` 的文件，并将以下代码粘贴到其中：
```php
<?php
echo sprintf("Hello World!");
```
此代码会对所有请求响应“Hello World”，HTTP 处理由容器中的 Apache Web 服务器进行。

## 步骤2：将应用容器化

1. 在项目根目录下，创建一个名为 `Dockerfile` 的文件，内容如下：
```docker
	# 使用官方 PHP 7.3 镜像.
	# https://hub.docker.com/_/php
	FROM php:7.3-apache

	# 将本地代码复制到容器内
	COPY index.php /var/www/html/

	# Apache 配置文件内使用 8080 端口
	RUN sed -i 's/80/8080/g' /etc/apache2/sites-available/000-default.conf /etc/apache2/ports.conf

	# 将 PHP 配置为开发环境
	# 如果您需要配置为生产环境，可以运行以下命令
	# RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
	# 参考：https://hub.docker.com/_/php#configuration
	RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
```
2. 添加一个 `.dockerignore` 文件，以从容器映像中排除文件：
```plaintext
Dockerfile
README.md
vendor
```

## 步骤3（可选）：本地构建镜像

1. 如果您本地已经安装了 Docker，可以运行以下命令，在本地构建 Docker 镜像：
```plaintext
docker build -t helloworld-php .
```
2. 构建成功后，运行 `docker images`，可以看到构建出的镜像：
```plaintext
REPOSITORY        TAG       IMAGE ID         CREATED           SIZE
helloworld-php   latest    1c8dfb88c823     8 seconds ago      411MB
```
随后您可以将此镜像上传至您的镜像仓库。

## 步骤4：部署到 CloudBase 云托管

请参见 [部署服务](https://cloud.tencent.com/document/product/1243/46127) 与 [版本配置说明](https://cloud.tencent.com/document/product/1243/49177)。
