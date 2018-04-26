FROM ubuntu:16.04

MAINTAINER CBDS

# ENV TOMCAT_VERSION 8.5.30

RUN apt-get update && apt-get -y upgrade
RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get -y update

# Accept the license
RUN echo "oracle-java8-installer shared/accepted-oracle-license-v1-1 boolean true" | debconf-set-selections
RUN apt-get -y install oracle-java8-installer

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

RUN apt-get install -y maven

# Get Tomcat
# notice that i can add the same RUN command across multiple lines
RUN wget --quiet --no-cookies http://apache.mirrors.tds.net/tomcat/tomcat-8/v8.5.30/bin/apache-tomcat-8.5.30.tar.gz -O /tmp/tomcat.tgz && \
tar xzvf /tmp/tomcat.tgz -C /opt && \
mv /opt/apache-tomcat-8.5.30 /opt/tomcat && \
rm /tmp/tomcat.tgz  && \
rm -rf /opt/tomcat/webapps/examples && \
rm -rf /opt/tomcat/webapps/docs && \
rm -rf /opt/tomcat/webapps/ROOT

ADD . /usr/local/docker-spring-mvc
RUN cd /usr/local/docker-spring-mvc && mvn clean install
RUN cp /usr/local/docker-spring-mvc/target/docker-spring-mvc.war /opt/tomcat/webapps/docker-spring-mvc.war
EXPOSE 8080
CMD /opt/tomcat/bin/catalina.sh run