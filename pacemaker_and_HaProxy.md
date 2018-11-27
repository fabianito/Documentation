Clustered HAproxy for load balancing web sites
In this setup I configure 2 clustered HAproxies in CentOS 7 to be the frontend of a web application.


I set static IPs and add them to /etc/hosts:

10.0.0.1/24 haproxy01
10.0.0.2/24 haproxy02
Disable firewalld

# systemctl stop firewalld.service
# systemctl mask firewalld.service

Disable SELinux
# setenforce 0

Edit /etc/selinux/config:

SELINUX=permissive
Add to /etc/sysctl.d/haproxy.conf:

net.ipv4.ip_nonlocal_bind = 1
Install the required packages

# yum install pacemaker corosync haproxy pcs fence-agents-all

pcsd is in charge of synchronize the cluster configuration across the nodes.
http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Clusters_from_Scratch/_setup.html

# passwd hacluster
# systemctl enable pcsd.service pacemaker.service corosync.service haproxy.service
# systemctl start pcsd.service
# pcs cluster auth haproxy01 haproxy02
# pcs cluster setup --start --name http-cluster haproxy01 haproxy02
# pcs cluster enable --all

We check if everything is all right:
http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Clusters_from_Scratch/_verify_corosync_installation.html

# corosync-cfgtool -s
# corosync-cmapctl  | grep members
# pcs status corosync
# pcs status

http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Clusters_from_Scratch/ch05.html
# pcs property set stonith-enabled=false

If we only have two nodes:

http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Clusters_from_Scratch/_perform_a_failover.html
# pcs property set no-quorum-policy=ignore

To prevent a resource to fail back when a node recovers:
# pcs resource defaults resource-stickiness=100

Is the config ok?:

# crm_verify -L -V

Add cluster resources:
# pcs resource create ClusterIP-01 ocf:heartbeat:IPaddr2 ip=10.0.0.3 cidr_netmask=24 op monitor interval=5s
# pcs resource create ClusterIP-02 ocf:heartbeat:IPaddr2 ip=10.0.0.4 cidr_netmask=24 op monitor interval=5s
# pcs resource create HAproxy systemd:haproxy op monitor interval=5s
We group the IPs together:
# pcs resource group add HAproxyIPs ClusterIP-01 ClusterIP-02

Add some constraints to move the IPs to the other host when HAproxy is down.
# pcs constraint colocation add HAproxy HAproxyIPs INFINITY
# pcs constraint order HAproxyIPs then HAproxy

Finally I’ve configured HAproxy with the two web applications and different backends, with http and https. This /etc/haproxy/haproxy.cnf must be the same in both servers.
