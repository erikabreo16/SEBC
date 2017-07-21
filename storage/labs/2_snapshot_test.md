// snapshot

sudo -u hdfs hdfs dfs -mkdir /precious 
cp SEBC-master.zip /tmp
sudo chown hdfs:hdfs SEBC-master.zip
sudo -u hdfs hdfs dfs -put /tmp/SEBC-master.zip /precious

[centos@ip-172-31-38-4 tmp]$ sudo -u hdfs hdfs dfsadmin -allowSnapshot /precious
Allowing snaphot on /precious succeeded
[centos@ip-172-31-38-4 tmp]$


[centos@ip-172-31-38-4 tmp]$ sudo -u hdfs  hdfs dfs -createSnapshot /precious sebc-hdfs-test
Created snapshot /precious/.snapshot/sebc-hdfs-test
[centos@ip-172-31-38-4 tmp]$


[centos@ip-172-31-38-4 tmp]$ sudo -u hdfs  hdfs dfs -ls /precious/.snapshot
Found 1 items
drwxr-xr-x   - hdfs supergroup          0 2017-07-21 03:49 /precious/.snapshot/sebc-hdfs-test
[centos@ip-172-31-38-4 tmp]$

[centos@ip-172-31-38-4 ~]$ sudo -u hdfs  hdfs dfs -rm /precious/SEBC-master.zip
17/07/21 04:06:31 INFO fs.TrashPolicyDefault: Moved: 'hdfs://ip-172-31-38-4.us-west-2.compute.internal:8020/precious/SEBC-master.zip' to trash at: hdfs://ip-172-31-38-4.us-west-2.compute.internal:8020/user/hdfs/.Trash/Current/precious/SEBC-master.zip

[centos@ip-172-31-38-4 ~]$ sudo -u hdfs  hdfs dfs -ls /precious/

[centos@ip-172-31-38-4 ~]$ sudo -u hdfs  hdfs dfs -ls /precious/.snapshot/sebc-hdfs-test
Found 1 items
-rw-r--r--   3 hdfs supergroup     474826 2017-07-21 03:46 /precious/.snapshot/sebc-hdfs-test/SEBC-master.zip
[centos@ip-172-31-38-4 ~]$


[centos@ip-172-31-38-4 ~]$ sudo -u hdfs  hdfs dfs -cp /precious/.snapshot/sebc-hdfs-test/SEBC-master.zip /precious

[centos@ip-172-31-38-4 ~]$ sudo -u hdfs  hdfs dfs -ls -R /precious/
-rw-r--r--   3 hdfs supergroup     474826 2017-07-21 04:10 /precious/SEBC-master.zip
[centos@ip-172-31-38-4 ~]$
