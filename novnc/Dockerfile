FROM alpine:3.7

#############################################


# Author  forsrc

# who is the author
MAINTAINER forsrc

# make a new directory to store the jdk files
RUN mkdir /usr/local/java

# copy the jdk  archive to the image,and it will automaticlly unzip the tar file

#ADD jdk-8u151-linux-x64.tar.gz /usr/local/java/

RUN   apk update \
 &&   apk add ca-certificates wget \
 &&   update-ca-certificates \
 &&   cd /usr/local/java \
 &&   wget --no-check-certificate -c --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/10.0.1+10/fb4372174a714e6b8c52526dc134031e/jdk-10.0.1_linux-x64_bin.tar.gz \
 &&   tar -zxvf jdk-10.0.1_linux-x64_bin.tar.gz \
 &&   rm -f jdk-10.0.1_linux-x64_bin.tar.gz
# make a symbol link
RUN ln -s /usr/local/java/jdk-10.0.1 /usr/local/java/jdk

# set environment variables
ENV JAVA_HOME /usr/local/java/jdk
ENV JRE_HOME ${JAVA_HOME}/jre
ENV CLASSPATH .:${JAVA_HOME}/lib:${JRE_HOME}/lib
ENV PATH ${JAVA_HOME}/bin:$PATH


#############################################

# install openssh
RUN apk add openssh

RUN sed -ri 's/^#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/^UsePAM yes/UsePAM no/' /etc/ssh/sshd_config

#generate key files
RUN ssh-keygen -q -t rsa   -b 2048 -f /etc/ssh/ssh_host_rsa_key      -N ''
RUN ssh-keygen -q -t ecdsa         -f /etc/ssh/ssh_host_ecdsa_key    -N ''
RUN ssh-keygen -q -t dsa           -f /etc/ssh/ssh_host_ed25519_key  -N ''

# login localhost without password
RUN ssh-keygen -f /root/.ssh/id_rsa -N ''
RUN touch /root/.ssh/authorized_keys
RUN cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys

# set password of root
RUN echo "root:root" | chpasswd

# open the port 22
EXPOSE 22

# when start a container it will be executed
CMD ["/usr/sbin/sshd","-D"]


##############################################

# Setup demo environment variables
ENV HOME=/root \
	DEBIAN_FRONTEND=noninteractive \
	LANG=en_US.UTF-8 \
	LANGUAGE=en_US.UTF-8 \
	LC_ALL=C.UTF-8 \
	DISPLAY=:0.0 \
	DISPLAY_WIDTH=1024 \
	DISPLAY_HEIGHT=768

# x11vnc is in testing repo
RUN echo "http://dl-3.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

# Install git, supervisor, VNC, & X11 packages
RUN apk --update --upgrade add \
	bash \
	fluxbox \
	git \
	socat \
	supervisor \
	x11vnc \
	xterm \
	xvfb

# Clone noVNC from github
RUN git clone https://github.com/kanaka/noVNC.git /root/noVNC \
	&& git clone https://github.com/kanaka/websockify /root/noVNC/utils/websockify \
	&& rm -rf /root/noVNC/.git \
	&& rm -rf /root/noVNC/utils/websockify/.git \
	&& apk del git

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Modify the launch script 'ps -p'
RUN sed -i -- "s/ps -p/ps -o pid | grep/g" /root/noVNC/utils/launch.sh

EXPOSE 8080

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
