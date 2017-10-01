FROM centos:centos7
MAINTAINER Sheroz Khaydarov "http://sheroz.com"

RUN yum -y update && yum -y --nogpgcheck install epel-release && yum clean all 
	
# Copy yum repo config file to add MongoDB repo
COPY mongodb-org-3.4.repo /etc/yum.repos.d/     

RUN yum install -y --nogpgcheck install mongodb-org-server && yum clean all && \ 
mkdir -p /data/mongodb && chown -R mongod:mongod /data/mongodb

VOLUME ["/data/mongodb"]                    			
EXPOSE 27017

# Execute the mongod binary on container startup
# ENTRYPOINT ["mongod"]                

CMD ["/usr/bin/mongod", "--dbpath", "/data/mongodb"] 
