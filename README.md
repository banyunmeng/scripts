scripts
=======
install collectd 5.4.1 on CentOS 6
#!/bin/bash
# Perform installation as root
# Install prereqs
 
#yum -y install libcurl libcurl-devel rrdtool rrdtool-devel rrdtool-prel libgcrypt-devel gcc make gcc-c++
yum install perl-ExtUtils-MakeMaker.x86_64
# Get Collectd, untar it, make it and install
 
wget http://collectd.org/files/collectd-5.4.0.tar.gz
tar zxvf collectd-5.4.0.tar.gz
cd collectd-5.4.0
./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib --mandir=/usr/share/man --enable-all-plugins
make
make install
 
 
# Copy the default init.d script
 
cp /root/collectd-5.4.0/contrib/redhat/init.d-collectd /etc/init.d/collectd
 
# Set the correct permissions
 
chmod +x /etc/init.d/collectd
 
# Start the deamon
 
service collectd start
 
 
# NOTE! FQDN lookup is enabled by default. This might give you troubles if you'r running on Vagrant or some cloud server
# Check /var/log/messages for something like "Looking up "<yourhost>" failed. You have set the "FQDNLookup" option, but I cannot resolve my hostname...
# The fix is easy, just set "FQDNLookup false" in /etc/collectd.conf
