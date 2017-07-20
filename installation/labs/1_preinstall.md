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


//////INSTALACIÓN BASE DE DATOS

[root@ip-172-31-43-96 etc]# vi my.cnf
[mysqld]
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
# symbolic-links = 0

key_buffer = 16M
key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space. Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your system
#and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid
~
"my.cnf" 42L, 1090C


//////

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n]



By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
[root@ip-172-31-43-96 etc]#


////descomprimir el jdbc mysql


Last login: Mon Jul 17 18:16:57 UTC 2017 from 192.55.54.58 on pts/1
[centos@ip-172-31-43-96 ~]$ tar zxvf mysql-connector-java-5.1.42.tar.gz
mysql-connector-java-5.1.42/
mysql-connector-java-5.1.42/docs/
mysql-connector-java-5.1.42/src/
mysql-connector-java-5.1.42/src/com/
mysql-connector-java-5.1.42/src/com/mysql/
mysql-connector-java-5.1.42/src/com/mysql/fabric/
mysql-connector-java-5.1.42/src/com/mysql/fabric/hibernate/
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/exceptions/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/authentication/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/integration/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/integration/c3p0/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/integration/jboss/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/interceptors/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jmx/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/profiler/
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/
mysql-connector-java-5.1.42/src/demo/
mysql-connector-java-5.1.42/src/demo/fabric/
mysql-connector-java-5.1.42/src/demo/fabric/resources/
mysql-connector-java-5.1.42/src/demo/fabric/resources/com/
mysql-connector-java-5.1.42/src/demo/fabric/resources/com/mysql/
mysql-connector-java-5.1.42/src/demo/fabric/resources/com/mysql/fabric/
mysql-connector-java-5.1.42/src/demo/fabric/resources/com/mysql/fabric/demo/
mysql-connector-java-5.1.42/src/doc/
mysql-connector-java-5.1.42/src/doc/sources/
mysql-connector-java-5.1.42/src/lib/
mysql-connector-java-5.1.42/src/org/
mysql-connector-java-5.1.42/src/org/gjt/
mysql-connector-java-5.1.42/src/org/gjt/mm/
mysql-connector-java-5.1.42/src/org/gjt/mm/mysql/
mysql-connector-java-5.1.42/src/testsuite/
mysql-connector-java-5.1.42/src/testsuite/fabric/
mysql-connector-java-5.1.42/src/testsuite/fabric/jdbc/
mysql-connector-java-5.1.42/src/testsuite/perf/
mysql-connector-java-5.1.42/src/testsuite/regression/
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc4/
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc42/
mysql-connector-java-5.1.42/src/testsuite/simple/
mysql-connector-java-5.1.42/src/testsuite/simple/jdbc4/
mysql-connector-java-5.1.42/src/testsuite/simple/jdbc42/
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/
mysql-connector-java-5.1.42/CHANGES
mysql-connector-java-5.1.42/COPYING
mysql-connector-java-5.1.42/README
mysql-connector-java-5.1.42/README.txt
mysql-connector-java-5.1.42/build.xml
mysql-connector-java-5.1.42/docs/README.txt
mysql-connector-java-5.1.42/docs/connector-j.html
mysql-connector-java-5.1.42/docs/connector-j.pdf
mysql-connector-java-5.1.42/mysql-connector-java-5.1.42-bin.jar
mysql-connector-java-5.1.42/src/com/mysql/fabric/FabricCommunicationException.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/FabricConnection.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/FabricStateResponse.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/HashShardMapping.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/RangeShardMapping.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/Response.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/Server.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ServerGroup.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ServerMode.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ServerRole.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ShardIndex.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ShardMapping.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ShardMappingFactory.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ShardTable.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/ShardingType.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/hibernate/FabricMultiTenantConnectionProvider.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/ErrorReportingExceptionInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/FabricMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/FabricMySQLConnectionProperties.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/FabricMySQLConnectionProxy.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/FabricMySQLDataSource.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/FabricMySQLDriver.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/JDBC4FabricMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/jdbc/JDBC4FabricMySQLConnectionProxy.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/AuthenticatedXmlRpcMethodCaller.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/DigestAuthentication.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/InternalXmlRpcMethodCaller.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/ResultSetParser.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/XmlRpcClient.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/proto/xmlrpc/XmlRpcMethodCaller.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/Client.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Array.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Data.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Fault.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Member.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/MethodCall.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/MethodResponse.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Param.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Params.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/ResponseParser.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Struct.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/base/Value.java
mysql-connector-java-5.1.42/src/com/mysql/fabric/xmlrpc/exceptions/MySQLFabricException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/AbandonedConnectionCleanupThread.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/AssertionFailedException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/AuthenticationPlugin.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/BalanceStrategy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/BestResponseTimeBalanceStrategy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Blob.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/BlobFromLocator.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Buffer.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/BufferRow.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ByteArrayRow.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CacheAdapter.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CacheAdapterFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CachedResultSetMetaData.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CallableStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CharsetMapping.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Charsets.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Clob.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CommunicationsException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/CompressedInputStream.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Connection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionFeatureNotAvailableException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionGroup.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionGroupManager.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionImpl.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionLifecycleInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionProperties.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionPropertiesImpl.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ConnectionPropertiesTransform.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Constants.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/DatabaseMetaData.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/DatabaseMetaDataUsingInfoSchema.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/DocsConnectionPropsHelper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Driver.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/EscapeProcessor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/EscapeProcessorResult.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/EscapeTokenizer.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ExceptionInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ExportControlled.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Extension.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/FailoverConnectionProxy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Field.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/IterateBlock.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC42CallableStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC42Helper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC42PreparedStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC42ResultSet.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC42ServerPreparedStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC42UpdatableResultSet.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4CallableStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4ClientInfoProvider.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4ClientInfoProviderSP.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4CommentClientInfoProvider.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4Connection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4DatabaseMetaData.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4DatabaseMetaDataUsingInfoSchema.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4LoadBalancedMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4MultiHostMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4MySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4MysqlSQLXML.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4NClob.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4PreparedStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4PreparedStatementHelper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4ReplicationMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4ResultSet.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4ServerPreparedStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/JDBC4UpdatableResultSet.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LicenseConfiguration.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LoadBalanceExceptionChecker.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LoadBalancedAutoCommitInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LoadBalancedConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LoadBalancedConnectionProxy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LoadBalancedMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/LocalizedErrorMessages.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Messages.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MiniAdmin.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MultiHostConnectionProxy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MultiHostMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MysqlDataTruncation.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MysqlDefs.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MysqlErrorNumbers.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MysqlIO.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MysqlParameterMetadata.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/MysqlSavepoint.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NamedPipeSocketFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NdbLoadBalanceExceptionChecker.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NetworkResources.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NoSubInterceptorWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NonRegisteringDriver.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NonRegisteringReplicationDriver.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NotImplemented.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/NotUpdatable.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/OperationNotSupportedException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/OutputStreamWatcher.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/PacketTooBigException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ParameterBindings.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/PerConnectionLRUFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/PerVmServerConfigCacheFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/PingTarget.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/PreparedStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ProfilerEventHandlerFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/RandomBalanceStrategy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReflectiveStatementInterceptorAdapter.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReplicationConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReplicationConnectionGroup.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReplicationConnectionGroupManager.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReplicationConnectionProxy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReplicationDriver.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ReplicationMySQLConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ResultSetImpl.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ResultSetInternalMethods.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ResultSetMetaData.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ResultSetRow.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/RowData.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/RowDataCursor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/RowDataDynamic.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/RowDataStatic.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/SQLError.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Security.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/SequentialBalanceStrategy.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/ServerPreparedStatement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/SingleByteCharsetConverter.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/SocketFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/SocketMetadata.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/SocksProxySocketFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StandardLoadBalanceExceptionChecker.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StandardSocketFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Statement.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StatementImpl.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StatementInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StatementInterceptorV2.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StreamingNotifiable.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/StringUtils.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/TimeUtil.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/TimeZoneMapping.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/UpdatableResultSet.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Util.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/V1toV2StatementInterceptorAdapter.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/WatchableOutputStream.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/WatchableWriter.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/Wrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/WriterWatcher.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/authentication/MysqlClearPasswordPlugin.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/authentication/MysqlNativePasswordPlugin.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/authentication/MysqlOldPasswordPlugin.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/authentication/Sha256PasswordPlugin.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/3-0-Compat.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/5-0-Compat.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/clusterBase.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/coldFusion.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/fullDebug.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/maxPerformance.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/configs/solarisMaxPerformance.properties
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/DeadlockTimeoutRollbackMarker.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLDataException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLIntegrityConstraintViolationException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLInvalidAuthorizationSpecException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLNonTransientConnectionException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLNonTransientException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLQueryInterruptedException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLStatementCancelledException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLSyntaxErrorException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLTimeoutException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLTransactionRollbackException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLTransientConnectionException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/MySQLTransientException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/CommunicationsException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLDataException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLIntegrityConstraintViolationException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLInvalidAuthorizationSpecException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLNonTransientConnectionException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLNonTransientException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLQueryInterruptedException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLSyntaxErrorException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLTimeoutException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLTransactionRollbackException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLTransientConnectionException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/exceptions/jdbc4/MySQLTransientException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/integration/c3p0/MysqlConnectionTester.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/integration/jboss/ExtendedMysqlExceptionSorter.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/integration/jboss/MysqlValidConnectionChecker.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/interceptors/ResultSetScannerInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/interceptors/ServerStatusDiffInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/interceptors/SessionAssociationInterceptor.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/CallableStatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/ConnectionWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC42CallableStatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC42PreparedStatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4CallableStatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4ConnectionWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4MysqlPooledConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4MysqlXAConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4PreparedStatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4StatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/JDBC4SuspendableXAConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlConnectionPoolDataSource.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlDataSource.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlDataSourceFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlPooledConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlXAConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlXADataSource.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlXAException.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/MysqlXid.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/PreparedStatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/StatementWrapper.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/SuspendableXAConnection.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jdbc2/optional/WrapperBase.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jmx/LoadBalanceConnectionGroupManager.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jmx/LoadBalanceConnectionGroupManagerMBean.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jmx/ReplicationGroupManager.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/jmx/ReplicationGroupManagerMBean.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/Jdk14Logger.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/Log.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/LogFactory.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/LogUtils.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/NullLogger.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/Slf4JLogger.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/log/StandardLogger.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/profiler/LoggingProfilerEventHandler.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/profiler/ProfilerEvent.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/profiler/ProfilerEventHandler.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/Base64Decoder.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/BaseBugReport.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/ErrorMappingsDocGenerator.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/LRUCache.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/PropertiesDocGenerator.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/ReadAheadInputStream.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/ResultSetUtil.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/ServerController.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/TimezoneDump.java
mysql-connector-java-5.1.42/src/com/mysql/jdbc/util/VersionFSHierarchyMaker.java
mysql-connector-java-5.1.42/src/demo/fabric/Client1_Fabric.java
mysql-connector-java-5.1.42/src/demo/fabric/Employee.java
mysql-connector-java-5.1.42/src/demo/fabric/EmployeesDataSource.java
mysql-connector-java-5.1.42/src/demo/fabric/EmployeesJdbc.java
mysql-connector-java-5.1.42/src/demo/fabric/HibernateFabric.java
mysql-connector-java-5.1.42/src/demo/fabric/resources/com/mysql/fabric/demo/employee.hbm.xml
mysql-connector-java-5.1.42/src/doc/sources/pom.xml
mysql-connector-java-5.1.42/src/lib/c3p0-0.9.1-pre6.jar
mysql-connector-java-5.1.42/src/lib/c3p0-0.9.1-pre6.src.zip
mysql-connector-java-5.1.42/src/lib/jboss-common-jdbc-wrapper-src.jar
mysql-connector-java-5.1.42/src/lib/jboss-common-jdbc-wrapper.jar
mysql-connector-java-5.1.42/src/lib/slf4j-api-1.6.1.jar
mysql-connector-java-5.1.42/src/org/gjt/mm/mysql/Driver.java
mysql-connector-java-5.1.42/src/testsuite/BaseStatementInterceptor.java
mysql-connector-java-5.1.42/src/testsuite/BaseTestCase.java
mysql-connector-java-5.1.42/src/testsuite/UnreliableSocketFactory.java
mysql-connector-java-5.1.42/src/testsuite/fabric/BaseFabricTestCase.java
mysql-connector-java-5.1.42/src/testsuite/fabric/SetupFabricTestsuite.java
mysql-connector-java-5.1.42/src/testsuite/fabric/TestAdminCommands.java
mysql-connector-java-5.1.42/src/testsuite/fabric/TestDumpCommands.java
mysql-connector-java-5.1.42/src/testsuite/fabric/TestResultSetParser.java
mysql-connector-java-5.1.42/src/testsuite/fabric/TestShardMapping.java
mysql-connector-java-5.1.42/src/testsuite/fabric/TestXmlRpcCore.java
mysql-connector-java-5.1.42/src/testsuite/fabric/jdbc/TestBasicConnection.java
mysql-connector-java-5.1.42/src/testsuite/fabric/jdbc/TestFabricMySQLConnectionSharding.java
mysql-connector-java-5.1.42/src/testsuite/fabric/jdbc/TestHABasics.java
mysql-connector-java-5.1.42/src/testsuite/fabric/jdbc/TestHashSharding.java
mysql-connector-java-5.1.42/src/testsuite/fabric/jdbc/TestRegressions.java
mysql-connector-java-5.1.42/src/testsuite/perf/BasePerfTest.java
mysql-connector-java-5.1.42/src/testsuite/perf/LoadStorePerfTest.java
mysql-connector-java-5.1.42/src/testsuite/perf/RetrievalPerfTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/BlobRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/CachedRowsetTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/CallableStatementRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/CharsetRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/ConnectionRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/DataSourceRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/EscapeProcessorRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/MetaDataRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/MicroPerformanceRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/NumbersRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/PooledConnectionRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/ResultSetRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/StatementRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/StressRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/StringRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/SubqueriesRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/SyntaxRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/UtilsRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc4/ConnectionRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc4/ExceptionSubclassesTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc4/MetaDataRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc4/StatementRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc42/ConnectionRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc42/ResultSetRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/regression/jdbc42/StatementRegressionTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/BlobTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/CallableStatementTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/CharsetTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/ConnectionTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/DataSourceTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/DateTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/EscapeProcessingTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/MetadataTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/MiniAdminTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/MultiHostConnectionTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/NumbersTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/ReadOnlyCallableStatementTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/ResultSetTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/SSLTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/ServerControllerTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/SimpleTransformer.java
mysql-connector-java-5.1.42/src/testsuite/simple/SplitDBdotNameTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/StatementsTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/StringUtilsTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/TestBug57662Logger.java
mysql-connector-java-5.1.42/src/testsuite/simple/TestLifecycleInterceptor.java
mysql-connector-java-5.1.42/src/testsuite/simple/TransactionTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/TraversalTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/UpdatabilityTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/UtilsTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/XATest.java
mysql-connector-java-5.1.42/src/testsuite/simple/jdbc4/StatementsTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/jdbc42/ConnectionTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/jdbc42/ResultSetTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/jdbc42/StatementsTest.java
mysql-connector-java-5.1.42/src/testsuite/simple/tb2-data.txt.gz
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/ca-cert.pem
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/ca-key.pem
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/ca-truststore
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/certs_howto.txt
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/client-cert.pem
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/client-key.pem
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/client-keystore
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/mykey.pem
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/mykey.pub
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/server-cert.pem
mysql-connector-java-5.1.42/src/testsuite/ssl-test-certs/server-key.pem

////////

[centos@ip-172-31-43-96 ~]$ sudo cp mysql-connector-java-5.1.42/mysql-connector-java-5.1.42-bin.jar /usr/share/java/mysql-connector-java.jar
cp: cannot create regular file ‘/usr/share/java/mysql-connector-java.jar’: No such file or directory
[centos@ip-172-31-43-96 ~]$ sudo mkdir -p /usr/share/java/
[centos@ip-172-31-43-96 ~]$ sudo cp mysql-connector-java-5.1.42/mysql-connector-java-5.1.42-bin.jar /usr/share/java/mysql-connector-java.jar
[centos@ip-172-31-43-96 ~]$

///crear las bases de datos

[centos@ip-172-31-43-96 ~]$ mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 10
Server version: 5.5.52-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

MariaDB [(none)]> create database amon DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database rman DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database metastore DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database sentry DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected (0.01 sec)

MariaDB [(none)]> create database nav DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database navms DEFAULT CHARACTER SET utf8;
Query OK, 1 row affected (0.00 sec)


MariaDB [(none)]> grant all on amon.* TO 'amon'@'%' IDENTIFIED BY 'amon_password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on rman.* TO 'rman'@'%' IDENTIFIED BY 'rman_password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on metastore.* TO 'hive'@'%' IDENTIFIED BY 'hive_password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on sentry.* TO 'sentry'@'%' IDENTIFIED BY 'sentry_password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on nav.* TO 'nav'@'%' IDENTIFIED BY 'nav_password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on navms.* TO 'navms'@'%' IDENTIFIED BY 'navms_password';
Query OK, 0 rows affected (0.00 sec)

////oracle

[centos@ip-172-31-43-96 ~]$ sudo yum install oracle-j2sdk1.7-1.7.0+update67-1.x86_64.rpm
Loaded plugins: fastestmirror
Examining oracle-j2sdk1.7-1.7.0+update67-1.x86_64.rpm: oracle-j2sdk1.7-1.7.0+update67-1.x86_64
Marking oracle-j2sdk1.7-1.7.0+update67-1.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package oracle-j2sdk1.7.x86_64 0:1.7.0+update67-1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=========================================================================================================
 Package             Arch       Version               Repository                                    Size
=========================================================================================================
Installing:
 oracle-j2sdk1.7     x86_64     1.7.0+update67-1      /oracle-j2sdk1.7-1.7.0+update67-1.x86_64     279 M

Transaction Summary
=========================================================================================================
Install  1 Package

Total size: 279 M
Installed size: 279 M
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : oracle-j2sdk1.7-1.7.0+update67-1.x86_64                                               1/1
  Verifying  : oracle-j2sdk1.7-1.7.0+update67-1.x86_64                                               1/1

Installed:
  oracle-j2sdk1.7.x86_64 0:1.7.0+update67-1

///INSTALACION CLOUDERA

sudo yum install cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64.rp                                                               m
Loaded plugins: fastestmirror
Examining cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64.rpm: cloudera-manager-server-5.11.0-1                                                               .cm5110.p0.101.el7.x86_64
Marking cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package cloudera-manager-server.x86_64 0:5.11.0-1.cm5110.p0.101.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=========================================================================================================
 Package
      Arch   Version                    Repository                                                  Size
=========================================================================================================
Installing:
 cloudera-manager-server
      x86_64 5.11.0-1.cm5110.p0.101.el7 /cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64  12 k

Transaction Summary
=========================================================================================================

  
  Install  1 Package

Total size: 12 k
Installed size: 12 k
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64                             1/1
  Verifying  : cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64                             1/1

Installed:
  cloudera-manager-server.x86_64 0:5.11.0-1.cm5110.p0.101.el7

Complete!

//instalacion daemons

Examining cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64.rpm: cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64
Marking cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package cloudera-manager-daemons.x86_64 0:5.11.0-1.cm5110.p0.101.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=========================================================================================================
 Package
     Arch   Version                    Repository                                                   Size
=========================================================================================================
Installing:
 cloudera-manager-daemons
     x86_64 5.11.0-1.cm5110.p0.101.el7 /cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64 785 M

Transaction Summary
=========================================================================================================
Total size: 785 M
Installed size: 785 M
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64                            1/1
  Verifying  : cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64                            1/1

Installed:
  cloudera-manager-daemons.x86_64 0:5.11.0-1.cm5110.p0.101.el7

Complete!


///ejecutar shell

/usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm 123456 -u root -perika123

JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/java/jdk1.7.0_67-cloudera/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
[                          main] DbCommandExecutor              INFO  Successfully connected to database.
All done, your SCM database is configured correctly!


////subir servicio

[centos@ip-172-31-43-96 ~]$ sudo service cloudera-scm-server start
Starting cloudera-scm-server (via systemctl):              [  OK  ]
