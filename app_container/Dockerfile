FROM centos:centos6
# install basic packages
RUN yum install -y wget tar gcc
# set timezone
RUN rm -f /etc/localtime
RUN ln -s /usr/share/zoneinfo/UTC /etc/localtime
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
# ENTRYPOINT ["/usr/bin/supervisord"] does not work.
# --> "Error: positional arguments are not supported"
# http://stackoverflow.com/questions/22465003/error-positional-arguments-are-not-supported
CMD ["/usr/bin/supervisord"]
# install vim
#########################################################################################
RUN yum install -y MAKEDEV
##EDIT IN YUM REPOSITORY
#RUN  sed -i "42 s/enabled=0/enabled=1/" /etc/yum.repos.d/CentOS-Base.repo
##ADD puppet package to the container 
#RUN sudo rpm -ivh http://yum.puppetlabs.com/puppetlabs-release-el-6.noarch.rpm
#RUN sed -i "19 s/enabled=0.*/enabled=1/" /etc/yum.repos.d/puppetlabs.repo
RUN yum -y install puppet-server puppet
############################################################################################
RUN yum install -y vim
# for root
