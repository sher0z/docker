FROM centos:centos7
MAINTAINER Sheroz Khaydarov "http://sheroz.com"

RUN yum -y update && yum -y --nogpgcheck install epel-release && yum clean all
RUN yum install -y gcc make && yum clean all 

ENV REDIS_VERSION=3.2.8
ENV REDIS_DOWNLOAD_SHA1=6780d1abb66f33a97aad0edbe020403d0a15b67f
ENV REDIS_DOWNLOAD_MIRROR=http://download.redis.io/releases

# RUN yum -y install tcl

RUN mkdir -p /tmp/build; cd /tmp/build && \
curl -o redis.tar.gz "$REDIS_DOWNLOAD_MIRROR/redis-$REDIS_VERSION.tar.gz" && \
echo "$REDIS_DOWNLOAD_SHA1 redis.tar.gz" | sha1sum -c - && \
mkdir redis;tar xzf redis.tar.gz -C redis --strip-components=1 && \
cd redis;make;make install && \
rm -rf /tmp/build && \
yum -y remove make gcc && yum clean all && \
mkdir -p /data/redis && \
groupadd -r redis && \
useradd -r -g redis redis  && \
chown redis:redis /data/redis

VOLUME /data/redis
WORKDIR /data/redis
EXPOSE 6379
CMD [ "redis-server" ]
