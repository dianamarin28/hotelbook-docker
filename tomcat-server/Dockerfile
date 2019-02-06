FROM tomcat:8.0.43-jre8
EXPOSE 8080

COPY mysql-connector-java-5.1.41-bin.jar /usr/local/tomcat/lib/

RUN apt-get -y update
RUN apt-get -y install maven git openjdk-8-jdk
RUN cd /root && git clone --branch architectures https://github.com/dianamohanu/apiproject.git
RUN cd /root/apiproject && mvn clean package
RUN cp /root/apiproject/target/myproject.war /usr/local/tomcat/webapps/

CMD chmod +x /usr/local/tomcat/bin/catalina.sh
CMD ["catalina.sh", "run"]