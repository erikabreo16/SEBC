--salida TERAGEN

[root@ip-172-31-38-4 hadoop-mapreduce]# sudo -u erikabreo16 time hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce/hadoop-examples.jar teragen -Dmapred.compress.map.output=false -Dmapreduce.jobs.maps=4 32000 /user/erikabreo16/teragen
17/07/19 22:59:26 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-43-140.us-west-2.compute.internal/172.31.43.140:8032
17/07/19 22:59:26 INFO terasort.TeraGen: Generating 32000 using 2
17/07/19 22:59:27 INFO mapreduce.JobSubmitter: number of splits:2
17/07/19 22:59:27 INFO Configuration.deprecation: mapred.compress.map.output is deprecated. Instead, use mapreduce.map.output.compress
17/07/19 22:59:27 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1500493697580_0003
17/07/19 22:59:27 INFO impl.YarnClientImpl: Submitted application application_1500493697580_0003
17/07/19 22:59:27 INFO mapreduce.Job: The url to track the job: http://ip-172-31-43-140.us-west-2.compute.internal:8088/proxy/application_1500493697580_0003/
17/07/19 22:59:27 INFO mapreduce.Job: Running job: job_1500493697580_0003
17/07/19 22:59:34 INFO mapreduce.Job: Job job_1500493697580_0003 running in uber mode : false
17/07/19 22:59:34 INFO mapreduce.Job: map 0% reduce 0%
17/07/19 22:59:40 INFO mapreduce.Job: map 100% reduce 0%
17/07/19 22:59:40 INFO mapreduce.Job: Job job_1500493697580_0003 completed successfully
17/07/19 22:59:40 INFO mapreduce.Job: Counters: 33
File System Counters
FILE: Number of bytes read=0
FILE: Number of bytes written=255586
FILE: Number of read operations=0
FILE: Number of large read operations=0
FILE: Number of write operations=0
HDFS: Number of bytes read=164
HDFS: Number of bytes written=3200000
HDFS: Number of read operations=8
HDFS: Number of large read operations=0
HDFS: Number of write operations=4
Job Counters
Launched map tasks=2
Other local map tasks=2
Total time spent by all maps in occupied slots (ms)=7295
Total time spent by all reduces in occupied slots (ms)=0
Total time spent by all map tasks (ms)=7295
Total vcore-milliseconds taken by all map tasks=7295
Total megabyte-milliseconds taken by all map tasks=7470080
Map-Reduce Framework
Map input records=32000
Map output records=32000
Input split bytes=164
Spilled Records=0
Failed Shuffles=0
Merged Map outputs=0
GC time elapsed (ms)=67
CPU time spent (ms)=3290
Physical memory (bytes) snapshot=410132480
Virtual memory (bytes) snapshot=3195957248
Total committed heap usage (bytes)=579862528
Peak Map Physical memory (bytes)=206913536
Peak Map Virtual memory (bytes)=1600569344
org.apache.hadoop.examples.terasort.TeraGen$Counters
CHECKSUM=68613941816777
File Input Format Counters
Bytes Read=0
File Output Format Counters
Bytes Written=3200000
5.71user 0.24system 0:16.67elapsed 35%CPU (0avgtext+0avgdata 186336maxresident)k
0inputs+968outputs (0major+32289minor)pagefaults 0swaps

--salida terasort

[root@ip-172-31-38-4 hadoop-mapreduce]# sudo -u erikabreo16 time hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce/hadoop-examples.jar terasort /user/erikabreo16/teragen /user/erikabreo16/terasort
17/07/19 23:00:36 INFO terasort.TeraSort: starting
17/07/19 23:00:38 INFO input.FileInputFormat: Total input paths to process : 2
Spent 135ms computing base-splits.
Spent 2ms computing TeraScheduler splits.
Computing input splits took 137ms
Sampling 2 splits of 2
Making 8 from 32000 sampled records
Computing parititions took 507ms
Spent 647ms computing partitions.
17/07/19 23:00:38 INFO client.RMProxy: Connecting to ResourceManager at ip-172-31-43-140.us-west-2.compute.internal/172.31.43.140:8032
17/07/19 23:00:39 INFO mapreduce.JobSubmitter: number of splits:2
17/07/19 23:00:39 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1500493697580_0004
17/07/19 23:00:39 INFO impl.YarnClientImpl: Submitted application application_1500493697580_0004
17/07/19 23:00:39 INFO mapreduce.Job: The url to track the job: http://ip-172-31-43-140.us-west-2.compute.internal:8088/proxy/application_1500493697580_0004/
17/07/19 23:00:39 INFO mapreduce.Job: Running job: job_1500493697580_0004
17/07/19 23:00:46 INFO mapreduce.Job: Job job_1500493697580_0004 running in uber mode : false
17/07/19 23:00:46 INFO mapreduce.Job: map 0% reduce 0%
17/07/19 23:00:52 INFO mapreduce.Job: map 100% reduce 0%
17/07/19 23:00:58 INFO mapreduce.Job: map 100% reduce 13%
17/07/19 23:00:59 INFO mapreduce.Job: map 100% reduce 38%
17/07/19 23:01:03 INFO mapreduce.Job: map 100% reduce 50%
17/07/19 23:01:04 INFO mapreduce.Job: map 100% reduce 75%
17/07/19 23:01:08 INFO mapreduce.Job: map 100% reduce 100%
17/07/19 23:01:08 INFO mapreduce.Job: Job job_1500493697580_0004 completed successfully
17/07/19 23:01:08 INFO mapreduce.Job: Counters: 54
File System Counters
FILE: Number of bytes read=1376753
FILE: Number of bytes written=4043670
FILE: Number of read operations=0
FILE: Number of large read operations=0
FILE: Number of write operations=0
HDFS: Number of bytes read=3200312
HDFS: Number of bytes written=3200000
HDFS: Number of read operations=30
HDFS: Number of large read operations=0
HDFS: Number of write operations=16
Job Counters
Launched map tasks=2
Launched reduce tasks=8
Data-local map tasks=1
Rack-local map tasks=1
Total time spent by all maps in occupied slots (ms)=8085
Total time spent by all reduces in occupied slots (ms)=30399
Total time spent by all map tasks (ms)=8085
Total time spent by all reduce tasks (ms)=30399
Total vcore-milliseconds taken by all map tasks=8085
Total vcore-milliseconds taken by all reduce tasks=30399
Total megabyte-milliseconds taken by all map tasks=8279040
Total megabyte-milliseconds taken by all reduce tasks=31128576
Map-Reduce Framework
Map input records=32000
Map output records=32000
Map output bytes=3264000
Map output materialized bytes=1376165
Input split bytes=312
Combine input records=0
Combine output records=0
Reduce input groups=32000
Reduce shuffle bytes=1376165
Reduce input records=32000
Reduce output records=32000
Spilled Records=64000
Shuffled Maps =16
Failed Shuffles=0
Merged Map outputs=16
GC time elapsed (ms)=334
CPU time spent (ms)=17710
Physical memory (bytes) snapshot=2535780352
Virtual memory (bytes) snapshot=15985405952
Total committed heap usage (bytes)=3437232128
Peak Map Physical memory (bytes)=472092672
Peak Map Virtual memory (bytes)=1589784576
Peak Reduce Physical memory (bytes)=202682368
Peak Reduce Virtual memory (bytes)=1611558912
Shuffle Errors
BAD_ID=0
CONNECTION=0
IO_ERROR=0
WRONG_LENGTH=0
WRONG_MAP=0
WRONG_REDUCE=0
File Input Format Counters
Bytes Read=3200000
File Output Format Counters
Bytes Written=3200000
17/07/19 23:01:08 INFO terasort.TeraSort: done
6.80user 0.26system 0:33.11elapsed 21%CPU (0avgtext+0avgdata 194872maxresident)k
0inputs+1008outputs (0major+34257minor)pagefaults 0swaps
