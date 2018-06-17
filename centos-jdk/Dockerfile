# JDK 10
# Author  forsrc

# build a new image with basic  centos
FROM centos

# who is the author
MAINTAINER forsrc

# make a new directory to store the jdk files
RUN mkdir /usr/local/java

# RUN   apk update &&   apk add ca-certificates wget &&   update-ca-certificates

RUN yum -y install wget
RUN cd /usr/local/java \
    && wget --no-check-certificate -c --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/10.0.1+10/fb4372174a714e6b8c52526dc134031e/jdk-10.0.1_linux-x64_bin.tar.gz \
    && tar -zxvf jdk-10.0.1_linux-x64_bin.tar.gz \
    && rm -f jdk-10.0.1_linux-x64_bin.tar.gz

RUN ln -s /usr/local/java/jdk-10.0.1 /usr/local/java/jdk

# set environment variables
ENV JAVA_HOME /usr/local/java/jdk
ENV JRE_HOME ${JAVA_HOME}/jre
ENV CLASSPATH .:${JAVA_HOME}/lib:${JRE_HOME}/lib
ENV PATH ${JAVA_HOME}/bin:$PATH
