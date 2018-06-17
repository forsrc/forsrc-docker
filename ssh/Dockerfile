# build a new image with centos-jdk-ssh
FROM forsrc/centos-jdk

# who is the author
MAINTAINER forsrc

# install openssh
RUN yum -y  install openssh-server openssh-clients

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
