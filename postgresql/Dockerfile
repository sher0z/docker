FROM centos:centos7
MAINTAINER Sheroz Khaydarov "http://sheroz.com"

RUN yum -y update && yum -y --nogpgcheck install epel-release && yum clean all
RUN yum install -y gcc make && yum clean all 

ENV	POSTGRESQL_VERSION=10rc1 \
	POSTGRESQL_DOWNLOAD_SHA256=4fb649cf2c01f7e3a653023cd218bb0ce3f11a05eaa111729cbc370f7b60ab28 \
	POSTGRESQL_DOWNLOAD_MIRROR=https://ftp.postgresql.org/pub/source \
	POSTGRESQL_HOME=/opt/postgresql

# RUN yum -y install tcl

RUN yum install -y readline-devel.x86_64 zlib-devel.x86_64 && \
mkdir -p /tmp/build && \
cd /tmp/build && \
curl -o postgresql.tar.gz "$POSTGRESQL_DOWNLOAD_MIRROR/v$POSTGRESQL_VERSION/postgresql-$POSTGRESQL_VERSION.tar.gz" && \
echo "$POSTGRESQL_DOWNLOAD_SHA256 postgresql.tar.gz" | sha256sum -c && \
mkdir postgresql && \
tar xzf postgresql.tar.gz -C postgresql --strip-components=1 && \
cd /tmp/build/postgresql && \
./configure -prefix=$POSTGRESQL_HOME/$POSTGRESQL_VERSION && \
make && \
make install && \
rm -rf /tmp/build && \
yum -y remove make gcc && \
rm -rf /var/cache/yum && \
yum clean all && \
groupadd -r postgres && \
useradd --no-log-init -r -g postgres postgres && \
mkdir -p $POSTGRESQL_HOME/$POSTGRESQL_VERSION/data && \
chown postgres $POSTGRESQL_HOME/$POSTGRESQL_VERSION/data
USER postgres
RUN $POSTGRESQL_HOME/$POSTGRESQL_VERSION/bin/initdb -E UTF8 -D $POSTGRESQL_HOME/$POSTGRESQL_VERSION/data

#VOLUME /data/postgresql
#WORKDIR /data/postgresql
#EXPOSE 5432
# CMD [ "postgresql" ]
