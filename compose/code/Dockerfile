FROM centos:7
MAINTAINER Amit Bakshi <ambakshi@gmail.com>

ENV container docker
# See: https://hub.docker.com/_/centos/
RUN \
cd /lib/systemd/system/sysinit.target.wants/ && \
for i in *; do \
    [ $i == systemd-tmpfiles-setup.service ] || rm -vf $i ; \
done ; \
rm -vf /lib/systemd/system/multi-user.target.wants/* && \
rm -vf /etc/systemd/system/*.wants/* && \
rm -vf /lib/systemd/system/local-fs.target.wants/* && \
rm -vf /lib/systemd/system/sockets.target.wants/*udev* && \
rm -vf /lib/systemd/system/sockets.target.wants/*initctl* && \
rm -vf /lib/systemd/system/basic.target.wants/* && \
rm -vf /lib/systemd/system/anaconda.target.wants/* && \
mkdir -p /etc/selinux/targeted/contexts/ && \
echo '<busconfig><selinux></selinux></busconfig>' > /etc/selinux/targeted/contexts/dbus_contexts


RUN yum localinstall -y https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm && \
        yum install -y puppet openssh-server openssh-clients

ADD id_rsa.pub /root/.ssh/authorized_keys
RUN chmod 0700 /root/.ssh && chmod 0600 /root/.ssh/authorized_keys && systemctl enable sshd.service

EXPOSE 22
VOLUME [ "/run", "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
