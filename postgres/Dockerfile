FROM centos:centos7
MAINTAINER Sheroz Khaydarov "http://sheroz.com"

RUN yum -y update && yum -y --nogpgcheck install epel-release && yum clean all
RUN yum install -y gcc make && yum clean all 

ENV	POSTGRES_VERSION=10.0 \
	POSTGRES_DOWNLOAD_SHA256=86c83a664b770114861bc96a9eca3c3efe28c0384c6a71c610426cd42b0164fb \
	POSTGRES_DOWNLOAD_MIRROR=https://ftp.postgresql.org/pub/source \
	POSTGRES_HOME=/opt/postgres

# RUN yum -y install tcl

RUN yum install -y readline-devel.x86_64 zlib-devel.x86_64 && \
mkdir -p /tmp/build && \
cd /tmp/build && \
curl -o postgresql.tar.gz "$POSTGRES_DOWNLOAD_MIRROR/v$POSTGRES_VERSION/postgresql-$POSTGRES_VERSION.tar.gz" && \
echo "$POSTGRES_DOWNLOAD_SHA256 postgresql.tar.gz" | sha256sum -c && \
mkdir postgresql && \
tar xzf postgresql.tar.gz -C postgresql --strip-components=1 && \
cd /tmp/build/postgresql && \
./configure -prefix=$POSTGRES_HOME/$POSTGRES_VERSION && \
make && \
make install && \
rm -rf /tmp/build && \
yum -y remove make gcc && \
rm -rf /var/cache/yum && \
yum clean all && \
groupadd -r postgres && \
useradd --no-log-init -r -g postgres postgres && \
mkdir -p $POSTGRES_HOME/$POSTGRES_VERSION/data && \
chown postgres $POSTGRES_HOME/$POSTGRES_VERSION/data

# RUN systemctl status firewalld

# open docker network interface on firewalld
# RUN firewall-cmd --permanent --zone=trusted --change-interface=docker0
# or disable firewalld

# RUN systemctl stop firewalld && \
# systemctl disable firewalld && \
# systemctl mask firewalld

USER postgres

# allow remote connections
RUN $POSTGRES_HOME/$POSTGRES_VERSION/bin/initdb -E UTF8 -D $POSTGRES_HOME/$POSTGRES_VERSION/data && \
echo "host all  all    0.0.0.0/0  trust" >> $POSTGRES_HOME/$POSTGRES_VERSION/data/pg_hba.conf && \
echo "listen_addresses='*'" >> $POSTGRES_HOME/$POSTGRES_VERSION/data/postgresql.conf

# add VOLUME
VOLUME  ["/opt/postgres/10.0"]
EXPOSE 5432

# default command to run on start
CMD ["/opt/postgres/10.0/bin/postgres", "-D", "/opt/postgres/10.0/data", "-c", "config_file=/opt/postgres/10.0/data/postgresql.conf"]

# docker build -t postgres:10 .
# docker run -p 5432:5432 --name postgres-10 -d postgres:10
# docker pull dpage/pgadmin4
# docker run -p 80:80 --name pgadmin --link postgres-10:postgres-10 -d dpage/pgadmin4

# PGADMIN_DEFAULT_EMAIL: container@pgadmin.org
# PGADMIN_DEFAULT_PASSWORD: Conta1ner