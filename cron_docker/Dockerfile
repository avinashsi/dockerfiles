FROM boxupp/centos-base:latest

##ADD FOR SSH

RUN yum -y install openssh-server openssh-clients libedit sudo

##Set Up sshd

#RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key
#RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config
RUN sed -i "s/UsePAM.*/UsePAM no/g" /etc/ssh/sshd_config

##Disable Defaults requiretty in sudoers file

RUN sed -i "s/Defaults requiretty.*/ #Defaults requiretty/g" /etc/sudoers


#SET YOUR ROOT PASSWORD

RUN echo 'root:root123' | chpasswd

##INSTALLING EPEL REPO
RUN rpm -ivUh http://mirror-fpt-telecom.fpt.net/fedora/epel/6/i386/epel-release-6-8.noarch.rpm
RUN sed -i "42 s/enabled=0/enabled=1/" /etc/yum.repos.d/CentOS-Base.repo

##ADDED FOR US EST TIME ZONE
RUN  cp /usr/share/zoneinfo/US/Eastern /etc/localtime
ADD clock /etc/sysconfig/clock

##INSTALLING SUPERVISORD.CONF

RUN yum --enablerepo=epel install -y supervisor
RUN mv -f /etc/supervisord.conf /etc/supervisord.conf.org
ADD supervisord.conf /etc/
# install rsyslogd
RUN yum install -y rsyslog
# install crond
RUN yum install -y cronie-noanacron
# no PAM
RUN cp -a /etc/pam.d/crond /etc/pam.d/crond.org
RUN sed -i -e 's/^\(session\s\+required\s\+pam_loginuid\.so\)/#\1/' /etc/pam.d/crond

##INSTALLING DEVLOPMENT TOOLS

RUN  yum -y groupinstall 'Development Tools'

# for sshd
EXPOSE 22
CMD ["/usr/bin/supervisord"]



