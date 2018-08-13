# presetup

#!/bin/bash
yum update -y
yum install -y ca-certificates
update-ca-trust force-enable
cp /home/certs/*.crt /etc/pki/ca-trust/source/anchors/
update-ca-trust extract
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
echo "net.bridge.bridge-nf-call-ip6tables = 1" >> /etc/sysctl.conf
echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.conf

sed -i '11,12 s/^/#/' /etc/fstab
firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=2379-2380/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10251/tcp
firewall-cmd --permanent --add-port=10252/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --permanent --add-port=6739/tcp
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --permanent --add-port=30000-32767/tcp
firewall-cmd --permanent --add-port=6783/tcp
firewall-cmd --reload
yum install -y bind-utils conntrack ntp
ntpdate <host-ip>
echo "server <host-ip>" >> /etc/ntp.conf
systemctl enable ntpd
systemctl start ntpd
systemctl stop firewalld
systemctl disable firewalld
history
reboot

//setup

#!/bin/bash
setenforce 0
yum -y install docker
echo "[kubernetes]" >>/etc/yum.repos.d/kubernetes.repo
echo "name=Kubernetes" >>/etc/yum.repos.d/kubernetes.repo
echo "baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64" >>/etc/yum.repos.d/kubernetes.repo
echo "enabled=1" >>/etc/yum.repos.d/kubernetes.repo
echo "gpgcheck=1" >>/etc/yum.repos.d/kubernetes.repo
echo "repo_gpgcheck=1" >>/etc/yum.repos.d/kubernetes.repo
echo "gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg" >>/etc/yum.repos.d/kubernetes.repo
systemctl enable docker 
yum -y install kubeadm
systemctl enable kubelet
systemctl start docker && systemctl start kubelet
history
systemctl status docker
