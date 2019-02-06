# Copyright (c) 2017, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA
FROM centos:latest

ENV MYSQL_ROOT_PASSWORD="root"
ENV MYSQL_DATABASE="apiproject"
ENV MYSQL_USER="hotelbook"
ENV MYSQL_PASSWORD="hotelbookpw"

ARG PACKAGE_URL=https://repo.mysql.com/yum/mysql-5.6-community/docker/x86_64/mysql-community-server-minimal-5.6.39-2.el7.x86_64.rpm
ARG PACKAGE_URL_SHELL=""

#Mysql server needs 
RUN yum install -y net-tools wget

# Install server
RUN rpmkeys --import https://repo.mysql.com/RPM-GPG-KEY-mysql \
  && yum install -y $PACKAGE_URL $PACKAGE_URL_SHELL libpwquality \
  && yum clean all \
  && mkdir /docker-entrypoint-initdb.d

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /entrypoint.sh
COPY healthcheck.sh /healthcheck.sh
COPY my.cnf /etc
RUN chmod 644 /etc/my.cnf

HEALTHCHECK CMD /healthcheck.sh
EXPOSE 3306

# install supervisord
RUN yum -y install epel-release python-setuptools
RUN easy_install supervisor
RUN /usr/bin/echo_supervisord_conf > /etc/supervisord.conf
RUN mkdir -p /var/log/supervisor

# make supervisor run in foreground
RUN sed -i -e "s/^nodaemon=false/nodaemon=true/" /etc/supervisord.conf

# tell supervisor to include relative .ini files
RUN mkdir /etc/supervisord.d
RUN echo [include] >> /etc/supervisord.conf
RUN echo 'files = /etc/supervisord.d/*.ini' >> /etc/supervisord.conf

# add mysql program to supervisord config
RUN echo [program:mysqld] >> /etc/supervisord.d/mysql.ini
RUN echo 'command=/entrypoint.sh mysqld' >> /etc/supervisord.d/mysql.ini     
RUN echo  >> /etc/supervisord.d/mysql.ini

CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]