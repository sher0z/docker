FROM centos:centos7
MAINTAINER Sheroz Khaydarov "http://sheroz.com"

RUN yum -y update && yum -y --nogpgcheck install epel-release && yum clean all
RUN yum install -y gcc make && yum clean all 

ENV	LANG=en_US.UTF-8
ENV	RUBY_MAJOR=2.4
ENV	RUBY_VERSION=2.4.2
ENV	RUBY_DOWNLOAD_SHA256=93b9e75e00b262bc4def6b26b7ae8717efc252c47154abb7392e54357e6c8c9c
ENV	RUBY_DOWNLOAD_MIRROR=https://cache.ruby-lang.org/pub/ruby

RUN yum install -y openssl-devel libyaml-devel libffi-devel readline-devel zlib-devel gdbm-devel ncurses-devel && \
yum-config-manager --enable epel && \
yum install -y nodejs && \
curl -o /etc/yum.repos.d/yarn.repo https://dl.yarnpkg.com/rpm/yarn.repo && \
yum install -y yarn && \
yum clean all && \
mkdir -p /usr/local/etc && \
echo -e "install: --no-document\nupdate: --no-document" > /usr/local/etc/gemrc && \
mkdir -p /tmp/build && cd /tmp/build && \
curl -o ruby.tar.gz "$RUBY_DOWNLOAD_MIRROR/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz" && \
echo "$RUBY_DOWNLOAD_SHA256 ruby.tar.gz" | sha256sum -c - && \
mkdir ruby && tar -xzf ruby.tar.gz -C ruby --strip-components=1 && \
cd ruby && \
./configure --disable-install-doc --enable-shared && \
make -j"$(nproc)" && \
make install && \
yum -y remove make gcc && yum clean all && \
rm -rf /tmp/build && \
cd / && \
gem update --system && gem install bundler && \
rm -rf "$(gem env home)"/cache/*

	