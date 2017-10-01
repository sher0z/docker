FROM centos:centos7
MAINTAINER Sheroz Khaydarov "http://sheroz.com"

RUN yum -y update && yum -y --nogpgcheck install epel-release && yum clean all
RUN yum install -y gcc make && yum clean all 

ENV	REDIS_VERSION=4.0.2 \
	REDIS_DOWNLOAD_SHA256=b1a0915dbc91b979d06df1977fe594c3fa9b189f1f3d38743a2948c9f7634813 \
	REDIS_DOWNLOAD_MIRROR=http://download.redis.io/releases

# RUN yum -y install tcl

RUN mkdir -p /tmp/build; cd /tmp/build && \
curl -o redis.tar.gz "$REDIS_DOWNLOAD_MIRROR/redis-$REDIS_VERSION.tar.gz" && \
echo "$REDIS_DOWNLOAD_SHA256 redis.tar.gz" | sha256sum -c - && \
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
