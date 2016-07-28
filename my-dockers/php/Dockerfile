FROM centos:6.7

WORKDIR /root

RUN rpm --import http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-6 \
    && rpm --import https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-6 \
    && rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm \
    && rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

RUN yum update -y

RUN yum groupinstall -y "Development Tools"

RUN yum install -y php70w

RUN yum install -y php70w-devel php70w-pdo php70w-mysqlnd php70w-fpm php70w-opcache php70w-cli php70w-gd php70w-mcrypt php70w-mbstring php70w-xml

COPY ./etc /etc

EXPOSE 9000

CMD ["php-fpm"]
