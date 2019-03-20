FROM docker.io/mysql:5.7.25

#安装依赖
RUN set -x \
  && apt-get update
  && apt-get install -y vim
  
  #设置容器的时区
  && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
