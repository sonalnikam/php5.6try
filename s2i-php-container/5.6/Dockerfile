FROM centos/s2i-base-centos7


MAINTAINER Milan Sulc <sulcmil@gmail.com>
EXPOSE 8000
ENV PHP_VERSION=5.6 \
PATH=$PATH:/opt/rh/rh-php56/root/usr/bin
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

RUN curl 'https://setup.ius.io/' -o setup-ius.sh && \
bash setup-ius.sh
RUN yum install -y --setopt=tsflags=nodocs --enablerepo=centosplus \
php56-fpm nginx  
RUN yum -y install php56w \
php56w-opcache \ 
php56w-fpm \
php56w-cli \
nginx \
php56w-devel \
php56w-gd \
php56w-mbstring \
php56w-ldap \
php56w-intl \
php56w-pdo \
php56w-pear \
php56w-xml \
php56w- soap \
php56w-fpm-nginx

RUN yum clean all -y
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

COPY ./contrib/ /opt/app-root
RUN cp /opt/app-root/etc/conf.d/php-fpm/pool.conf /etc/php-fpm.d/www.conf
RUN cp /opt/app-root/etc/conf.d/php-fpm/fpm.conf /etc/php-fpm.conf
RUN mkdir /tmp/sessions && \
mkdir -p /var/lib/nginx && \
mkdir -p /var/log/nginx && \
touch /var/log/nginx/error.log && \
touch /var/log/nginx/access.log && \
chown -R 1001:0 /var/log/nginx && \
chown -R 1001:0 /var/lib/nginx && \
chown -R 1001:0 /opt/app-root /tmp/sessions && \
chmod -R a+rwx /tmp/sessions && \
chmod -R a+rwx /var/log/nginx && \
chmod -R a+rwx /var/lib/nginx && \
chmod -R ug+rwx /var/log/nginx && \
chmod -R ug+rwx /var/lib/nginx && \
chmod -R ug+rwx /opt/app-root && \
#chmod -R a+rwx /opt/app-root/services.sh \
#chmod -R a+rwx /opt/app-root
chmod -R a+rwx /opt/app-root/services.sh

USER 1001
CMD $STI_SCRIPTS_PATH/usage
