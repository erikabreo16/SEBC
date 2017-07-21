*maria db hostname:


MariaDB [(none)]> select @@hostname;
+------------------+
| @@hostname       |
+------------------+
| ip-172-31-42-115 |
+------------------+
1 row in set (0.00 sec)


*maria db version

MariaDB [(none)]> SELECT VERSION(); +----------------+ | VERSION() | +----------------+ | 5.5.52-MariaDB | +----------------+ 1 row in set (0.00 sec)

*maria db databases

MariaDB [(none)]> SHOW DATABASES; +--------------------+ | Database | +--------------------+ | information_schema | | hive | | hue | | mysql | | oozie | | performance_schema | | rman | | scm | +--------------------+ 8 rows in set (0.00 sec)
