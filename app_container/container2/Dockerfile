FROM centos:centos6
# install basic packages
RUN yum install -y wget tar gcc
# set timezone

##ADDED FOR US EST TIME ZONE
RUN  cp /usr/share/zoneinfo/US/Eastern /etc/localtime
ADD clock /etc/sysconfig/clock

#####################################################################################
# install supervisord
RUN rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
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
# install sshd

#######################################################################################
RUN yum -y install openssh-server openssh-clients libedit sudo
##Set Up sshd
RUN sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config 
RUN sed -i "s/UsePAM.*/UsePAM no/g" /etc/ssh/sshd_config
##Disable Defaults requiretty in sudoers file 
RUN  sed -i "s/Defaults    requiretty.*/ #Defaults    requiretty/g" /etc/sudoers
#SET YOUR ROOT PASSWORD
RUN echo 'root:root123' | chpasswd
#########################################################################################

# for sshd
EXPOSE 22
###########################################################################################
CMD ["/usr/bin/supervisord"]
############################################################################################
RUN yum install -y vim
RUN  yum -y groupinstall 'Development Tools'
#############################################################################################
