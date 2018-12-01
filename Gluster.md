gluster peer probe kube3
gluster volume create daten replica 3 kube1:/var/data kube2:/var/data kube3:/var/data force
gluster volume start
mount -t glusterfs kube1:daten /mnt/brick1/
