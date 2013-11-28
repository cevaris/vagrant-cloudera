# -*- mode: ruby -*-
# vi: set ft=ruby :

$master_script = <<SCRIPT
#!/bin/bash
# cat > /etc/hosts <<EOF
# 127.0.0.1       localhost

# # The following lines are desirable for IPv6 capable hosts
# ::1     ip6-localhost ip6-loopback
# fe00::0 ip6-localnet
# ff00::0 ip6-mcastprefix
# ff02::1 ip6-allnodes
# ff02::2 ip6-allrouters

# 10.211.55.100   vm-cluster-node1
# 10.211.55.101   vm-cluster-node2
# 10.211.55.102   vm-cluster-node3
# 10.211.55.103   vm-cluster-node4
# 10.211.55.104   vm-cluster-node5
# 10.211.55.105   vm-cluster-client
# EOF

apt-get install curl -y
REPOCM=${REPOCM:-cm4}
CM_REPO_HOST=${CM_REPO_HOST:-archive.cloudera.com}
CM_MAJOR_VERSION=$(echo $REPOCM | sed -e 's/cm\\([0-9]\\).*/\\1/')
CM_VERSION=$(echo $REPOCM | sed -e 's/cm\\([0-9][0-9]*\\)/\\1/')
OS_CODENAME=$(lsb_release -sc)
OS_DISTID=$(lsb_release -si | tr '[A-Z]' '[a-z]')
if [ $CM_MAJOR_VERSION -ge 4 ]; then
  cat > /etc/apt/sources.list.d/cloudera-$REPOCM.list <<EOF
deb [arch=amd64] http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm $OS_CODENAME-$REPOCM contrib
deb-src http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm $OS_CODENAME-$REPOCM contrib
EOF
curl -s http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm/archive.key > key
apt-key add key
rm key
fi
apt-get update
export DEBIAN_FRONTEND=noninteractive
apt-get -q -y --force-yes install oracle-j2sdk1.6 cloudera-manager-server-db cloudera-manager-server cloudera-manager-daemons
service cloudera-scm-server-db initdb
service cloudera-scm-server-db start
service cloudera-scm-server start
SCRIPT

# $slave_script = <<SCRIPT
# cat > /etc/hosts <<EOF
# 127.0.0.1       localhost

# # The following lines are desirable for IPv6 capable hosts
# ::1     ip6-localhost ip6-loopback
# fe00::0 ip6-localnet
# ff00::0 ip6-mcastprefix
# ff02::1 ip6-allnodes
# ff02::2 ip6-allrouters

# 10.211.55.100   vm-cluster-node1
# 10.211.55.101   vm-cluster-node2
# 10.211.55.102   vm-cluster-node3
# 10.211.55.103   vm-cluster-node4
# 10.211.55.104   vm-cluster-node5
# 10.211.55.105   vm-cluster-client
# EOF
# SCRIPT

# $client_script = <<SCRIPT
# cat > /etc/hosts <<EOF
# 127.0.0.1       localhost

# # The following lines are desirable for IPv6 capable hosts
# ::1     ip6-localhost ip6-loopback
# fe00::0 ip6-localnet
# ff00::0 ip6-mcastprefix
# ff02::1 ip6-allnodes
# ff02::2 ip6-allrouters

# 10.211.55.100   vm-cluster-node1
# 10.211.55.101   vm-cluster-node2
# 10.211.55.102   vm-cluster-node3
# 10.211.55.103   vm-cluster-node4
# 10.211.55.104   vm-cluster-node5
# 10.211.55.105   vm-cluster-client
# EOF
# SCRIPT

Vagrant.configure("2") do |config|
  config.cache.auto_detect = true

  config.vm.define :master do |master|
    master.vm.provider :aws do |aws|
      aws.tags = { 'Name' => 'vm-cluster-master', }
    end
    master.vm.box = "aws_ubuntu"
    master.vm.provision :shell, :inline => $master_script

  end

end