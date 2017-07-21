*maria db hostname:

[root@ip-172-31-42-115 centos]# mysql -u root -p -h localhost
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
Server version: 5.5.52-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


*maria db version


MariaDB [(none)]> SELECT VERSION();
+----------------+
| VERSION()      |
+----------------+
| 5.5.52-MariaDB |
+----------------+
1 row in set (0.00 sec)



*maria db databases

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive               |
| hue                |
| mysql              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
+--------------------+
8 rows in set (0.00 sec)

