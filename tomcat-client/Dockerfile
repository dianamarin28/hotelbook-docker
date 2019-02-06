FROM tomcat:8.0.43-jre8
EXPOSE 8080

COPY mysql-connector-java-5.1.41-bin.jar /usr/local/tomcat/lib/

RUN apt-get -y update
RUN apt-get -y install maven git openjdk-8-jdk
RUN cd /root && git clone https://github.com/dianamarin28/hotelbook-client.git
RUN cd /root/clientproject && mvn clean package
RUN cp /root/clientproject/target/clientproject.war /usr/local/tomcat/webapps/

CMD chmod +x /usr/local/tomcat/bin/catalina.sh
CMD ["catalina.sh", "run"]