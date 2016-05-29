FROM centos:6.7

WORKDIR /root

RUN yum groupinstall -y 'Development Tools'

RUN yum install -y wget vim tar pcre-devel openssl-devel

RUN wget https://openresty.org/download/openresty-1.9.7.4.tar.gz

RUN tar zxvf openresty-1.9.7.4.tar.gz

RUN cd openresty-1.9.7.4 && \
    ./configure && \
    gmake && \
    gmake install

