# 拉取 CentOS7
FROM docker.io/centos:7.4.1708

# 维护者
MAINTAINER lyx <lyx@qq.com>

# 设置 PHP 和redis版本
#ENV PHP_VERSION 7.1.5
ENV PHP_VERSION 7.2.16

#ENV REDIS_VERSION 3.1.2
ENV REDIS_VERSION 4.2.0

#set timezome
ENV TZ=Asia/Shanghai

# 安装依赖
RUN set -x \
#  && yum update -y \
  && yum install epel-release -y \
  && yum install -y wget git gcc gcc-c++ make automake autoconf perl file tar re2c libjpeg libpng libjpeg-devel libpng-devel libjpeg-turbo freetype freetype-devel \
        libcurl-devel libxml2-devel libjpeg-turbo-devel libXpm-devel libXpm libicu-devel libmcrypt libmcrypt-devel libxslt-devel libxslt openssl openssl-devel bzip2-devel \
  
  #设置容器的时区
  && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone\
  
  # 建立目录
  
  && mkdir ~/download \
  && cd ~/download \

  # 编译 PHP
  && wget http://cn2.php.net/distributions/php-$PHP_VERSION.tar.gz \
#  && wget https://php.net/get/php-7.2.16.tar.gz/from/a/mirror/php-$PHP_VERSION.tar.gz \
  && tar -zxf php-$PHP_VERSION.tar.gz \
  && cd php-$PHP_VERSION \
  && groupadd -r nginx \
  && useradd -r -g nginx nginx \
  && ./configure \
       --prefix=/usr/local/php \
       --with-config-file-path=/usr/local/php/etc/ \
       --with-config-file-scan-dir=/usr/local/php/conf.d/ \
       --enable-fpm \
       --enable-cgi \
       --with-fpm-user=nginx  \
       --with-fpm-group=nginx \
       --disable-phpdbg \
       --enable-mbstring \
       --enable-calendar \
       --with-xsl \
       --with-openssl \
       --enable-soap \
       --enable-zip \
       --enable-shmop \
       --enable-sockets \
       --with-gd \
       --with-freetype-dir=/usr/include/freetype2/freetype \
       --with-jpeg-dir \
       --with-png-dir \
       --with-xpm-dir \
       --with-xmlrpc \
       --enable-pcntl \
       --enable-intl \
       --with-mcrypt \
       --enable-sysvsem \
       --enable-sysvshm \
       --enable-sysvmsg \
       --enable-opcache \
       --with-iconv \
       --with-bz2 \
       --with-curl \
       --enable-mysqlnd \
       --with-mysqli=mysqlnd \
       --with-pdo-mysql=mysqlnd \
       --with-zlib \
       --with-gettext \
  && make \
  && make install \

  # 复制配置文件
  && cp ~/download/php-$PHP_VERSION/php.ini-production /usr/local/php/etc/php.ini \
  && cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf \
  && cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf \

  # 安装 Redis 扩展
  && cd ~/download \
  && wget https://github.com/phpredis/phpredis/archive/$REDIS_VERSION.tar.gz \
  && tar -zxvf $REDIS_VERSION.tar.gz \
  && cd phpredis-$REDIS_VERSION \
  && /usr/local/php/bin/phpize \
  && ./configure --with-php-config=/usr/local/php/bin/php-config \
  && make \
  && make install \

  # 加入环境变量
  && echo "export PATH=/usr/local/php/bin:/usr/local/php/sbin:$PATH" >> ~/.bashrc \
  && source ~/.bashrc \

  # 安装 Composer
  && cd ~/download \
  && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php composer-setup.php --install-dir=/usr/local/sbin --filename=composer \
  && php -r "unlink('composer-setup.php');" \
  
#  #安装 Swoole
#  && yum install -y  libstdc++ libpng-dev \
#  && cd ~/download \
#  && wget https://github.com/swoole/swoole-src/archive/v4.2.12.tar.gz \
#  && tar -zxvf v4.2.12.tar.gz \
#  && mkdir -p /usr/src/php/ext \
#  && mv swoole-src-4.2.12 /usr/src/php/ext/swoole \
##  && cd /usr/src/php/ext/swoole \
##  && phpize \
##  && /usr/local/php/bin/phpize \
##  && ./configure \
##  && make \
##  && make install \
#  && docker-php-ext-install swoole \
  
  
  # 删除安装文件
  && rm -rf ~/download \
  && yum clean all

CMD ["/usr/local/php/sbin/php-fpm"]

EXPOSE 9000
