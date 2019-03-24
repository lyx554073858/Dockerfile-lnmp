FROM daocloud.io/library/redis:4.0.11-alpine
#FROM docker.io/redis:4.0.13


#v2版本中redis的启动用户为redis不是root，所以在宿主机中挂载的./redis/redis.log和./redis/data需要有写入权限
RUN set -x \
  && cd /usr/local/redis \
  && chmod 777 data redis.log

#COPY redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis.conf" ]
