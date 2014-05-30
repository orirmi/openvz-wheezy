
1. INSTALLATION

1.1 Create seperate ext4 partition, mount it at /var/lib/vz

1.2 There are official Debian kernel builds available. Add them to the apt system.

$ sudo echo deb http://download.openvz.org/debian wheezy main > /etc/apt/openvz.list
$ wget -qO - http://deb.opera.com/archive.key | sudo apt-key add -
$ apt-get update

1.3 Install openvz kernel.
# apt-get install linux-image-openvz-amd64

1.4 edit /etc/sysctl.conf

# packet forwarding enabled and proxy arp disabled
net.ipv4.ip_forward = 1
net.ipv6.conf.default.forwarding = 1
net.ipv6.conf.all.forwarding = 1
net.ipv4.conf.default.proxy_arp = 0

# Enables source route verification
net.ipv4.conf.all.rp_filter = 1

# Enables the magic-sysrq key
kernel.sysrq = 1

# We do not want all our interfaces to send redirects
net.ipv4.conf.default.send_redirects = 1
net.ipv4.conf.all.send_redirects = 0

1.5 Install tools
# apt-get install vzctl vzquota ploop vzstats

1.6 Disable stat collection
# touch /etc/vz/vzstats-disable

1.7 Reboot.



2. RUNNING

1. Precreated templates
OS templates are available at http://download.openvz.org/template/precreated/. Download them to /vz/template/cache.

2. Creating and starting a server
[host-node]# vzctl create 101 --ostemplate fedora-core-5-minimal
[host-node]# vzctl set 101 --ipadd 10.1.2.3 --save
[host-node]# vzctl set 101 --nameserver 10.0.2.1 --save
[host-node]# vzctl start 101

101 is the container id (CTID).

3. Executing commands inside the container
[host-node]# vzctl exec CTID ps ax

4. Entering the container
[host-node]# vzctl enter CTID
entered into container CTID
[container]# exit
[host-node]# 

5. Stopping the container
[host-node]# vzctl stop CTID

6. Destroying the container
[host-node]# vzctl destroy CTID
