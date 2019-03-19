# 拉取 CentOS
FROM docker.io/centos:centos7.4.1708

# 维护者
MAINTAINER lyx <lyx@qq.com>

# 设置 Redis 版本
ENV REDIS_VERSION 4.0.13

#set timezome
ENV TZ=Asia/Shanghai

# 下载并编译
RUN set -x \
  && yum update -y \
  && yum install -y wget gcc gcc-c++ make perl tar \
  && cd /opt \
  && wget http://download.redis.io/releases/redis-$REDIS_VERSION.tar.gz \

  && tar -zxf redis-$REDIS_VERSION.tar.gz -C /usr/local \
  && rm -f redis-$REDIS_VERSION.tar.gz \
  && cd /usr/local \
  && mv redis-$REDIS_VERSION redis \

  && cd /usr/local/redis \
  && make \
  && make -C /usr/local/redis/src install \
  && mkdir /usr/local/redis/data \
  
  #设置容器的时区
  && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \

  && yum clean all

CMD ["usr/local/redis/src/redis-server","/usr/local/redis/redis.conf"]

EXPOSE 6379
