FROM centos

ADD ./lucian.repo /etc/yum.repos.d/

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*

RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Linux-*

RUN yum --assumeyes update && \
yum --assumeyes install nmap iproute && \
bash && \
yum clean all

CMD ["/usr/bin/nmap", "-sn","172.17.0.0/24"]