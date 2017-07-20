verificar swappiness

sudo sysctl -w vm.swappiness=1


[centos@ip-172-31-33-146 ~]$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       40G   11G   30G  27% /
devtmpfs        7.3G     0  7.3G   0% /dev
tmpfs           7.2G     0  7.2G   0% /dev/shm
tmpfs           7.2G   25M  7.2G   1% /run
tmpfs           7.2G     0  7.2G   0% /sys/fs/cgroup
/dev/xvdc        37G   49M   35G   1% /data02
/dev/xvdb        37G   49M   35G   1% /data01
tmpfs           1.5G     0  1.5G   0% /run/user/996
tmpfs           1.5G     0  1.5G   0% /run/user/0
cm_processes    7.2G  4.1M  7.2G   1% /run/cloudera-scm-agent/process
tmpfs           1.5G     0  1.5G   0% /run/user/1000

[centos@ip-172-31-33-146 ~]$ ifconfig -a
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
        inet 172.31.33.146  netmask 255.255.240.0  broadcast 172.31.47.255
        inet6 fe80::42b:aeff:fe72:edb0  prefixlen 64  scopeid 0x20<link>
        ether 06:2b:ae:72:ed:b0  txqueuelen 1000  (Ethernet)
        RX packets 4533493  bytes 4883029694 (4.5 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1823159  bytes 2002289385 (1.8 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1  (Local Loopback)
        RX packets 6411740  bytes 8948676055 (8.3 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 6411740  bytes 8948676055 (8.3 GiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


[centos@ip-172-31-33-146 ~]$ cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

[centos@ip-172-31-33-146 ~]$ ping -c2 localhost
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.014 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.028 ms

--- localhost ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.014/0.021/0.028/0.007 ms
[centos@ip-172-31-33-146 ~]$ nslookup localhost
Server:         172.31.0.2
Address:        172.31.0.2#53

Name:   localhost
Address: 127.0.0.1



[centos@ip-172-31-33-146 ~]$ ps -fea | grep nscd
centos    2247  1641  0 17:22 pts/0    00:00:00 grep --color=auto nscd
[centos@ip-172-31-33-146 ~]$ ps -fea | grep ntpd
centos    2285  1641  0 17:22 pts/0    00:00:00 grep --color=auto ntpd

