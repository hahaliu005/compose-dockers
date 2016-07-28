FROM centos:6.7

WORKDIR /root

RUN yum update -y

RUN yum groupinstall -y "Development Tools"

RUN yum install -y pcre pcre-devel zlib zlib-devel

RUN yum install -y wget tar

RUN wget http://nginx.org/download/nginx-1.10.1.tar.gz

RUN tar zxvf nginx-1.10.1.tar.gz

RUN cd nginx-* \
    && ./configure \
    && make \
    && make install

COPY ./conf /usr/local/nginx/conf

EXPOSE 80

CMD ["/usr/local/nginx/sbin/nginx"]
