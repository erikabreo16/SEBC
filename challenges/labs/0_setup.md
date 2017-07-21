1. CLOUD PROVIDER

AWS


2. INSTANCES:

  INSTANCE 1:
  
  172.31.42.115
  
  ec2-54-245-23-19.us-west-2.compute.amazonaws.com
  
  INSTANCE 2: 
  
  172.31.46.47
  
  ec2-54-218-86-121.us-west-2.compute.amazonaws.com
  
  INSTANCE 3:
  
  172.31.41.0
  
  ec2-54-202-246-81.us-west-2.compute.amazonaws.com
  
  INSTANCE 4:
  
  172.31.44.227
  
  ec2-54-200-246-91.us-west-2.compute.amazonaws.com
  
  INSTANCE 5:
  
  172.31.33.64
  
  ec2-54-202-245-43.us-west-2.compute.amazonaws.com
  
 3. LINUX
 

 CentOS Linux 7 x86_64 HVM EBS 1704_01-b7ee8a69-ee97-4a49-9e68-afaee216db2e-ami-d52f5bc3.4 (ami-f4533694)
 
 
 4. 
 
 
[root@ip-172-31-42-115 opt]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       50G  943M   50G   2% /
devtmpfs         15G     0   15G   0% /dev
tmpfs            15G     0   15G   0% /dev/shm
tmpfs            15G   17M   15G   1% /run
tmpfs            15G     0   15G   0% /sys/fs/cgroup
/dev/xvdb        74G   52M   70G   1% /mnt
tmpfs           3.0G     0  3.0G   0% /run/user/0
tmpfs           3.0G     0  3.0G   0% /run/user/1000


5. usuarios


[root@ip-172-31-42-115 centos]# useradd saturn -d /home/saturn -p saturn -s /bin/bash -u 2800
[root@ip-172-31-42-115 centos]# useradd haley -d /home/haley -p haley -s /bin/bash -u 2900

6. grupos

[root@ip-172-31-42-115 centos]# groupadd comets
[root@ip-172-31-42-115 centos]# groupadd planets


[root@ip-172-31-42-115 centos]# usermod saturn -g planets
[root@ip-172-31-42-115 centos]# usermod haley -g comets

7. listar usuarios


[root@ip-172-31-42-115 centos]# cat /etc/passwd | grep saturn
saturn:x:2800:2800::/home/saturn:/bin/bash
[root@ip-172-31-42-115 centos]# cat /etc/passwd | grep haley
haley:x:2900:2900::/home/haley:/bin/bash

