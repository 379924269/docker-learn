FROM sshd:dockerfile
# 继承我们的sshd镜像

MAINTAINER huazai from 379924269@qq.com
# 创建者的基本信息

ENV DEBIAN_FRONTEND noninteractive
# 设置环境变量，所有操作都是非交互式的

# 安装
RUN apt-get -yq install apache2&&\
    rm -rf /var/lib/apt/lists/*

RUN echo "Asia/Shanghai" > /etc/timezone &&\
    dpkg-reconfigure -f noninteractive tzdata
# 注意这里要更改系统的时取设置，应为在web应用中经常会用到时区这个系统变量，记得引用程序发生不可思议的效果

#添加我们的脚本
ADD run.sh /run.sh
RUN chmod 775 /*.sh

#添加一个实列web站点 删掉默认安装在apache文件夹下面的文件，并将我们的实列用软连接到/var/www/html 目录下面
RUN mkdir -p /var/lock/apache2 && mkdir -p /app && rm -rf /var/www/html && ln -s /app /var/www/html

COPY sample/ /app

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_SERVERADMIN admin@localhost
ENV APACHE_SERVERNAME localhost
ENV APACHE_SERVERALIAS docker.localhost
ENV APACHE_DOCUMENTROOT /var/www

RUN echo "ServerName 172.17.0.4" >> /etc/apache2/apache2.conf

EXPOSE 80
WORKDIR /app
CMD ["/run.sh"]
