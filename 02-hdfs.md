# HFDS

## What is HDFS?

HDFS stands for **Hadoop Distributed File System**. It is a file system for Hadoop apps as NTFS and FAT32 for Windows apps. Also, this system is distributed as it's indicating its name. HDFS provides high-performance access to data across Hadoop clusters.  

HDFS supports larger file sizes than in other systems which helps to perform very high I/O. This returns to its architecture based on the large data blocks. The default block size is 64 MB and in the Cloudera Hadoop the default is 128 MB. NTFS & EXT3 file systems typically used by NFS have 4-8KB block sizes that result in large metadata about each file and multiple reads from the file system.

In a Hadoop ecosystem, HDFS stores data reliably. If a server in the cluster fails, the data is still available on other servers.

HFDS is built on top of OS file system. Files are written once so there's no random writes. For reads, HDFS is optimized for large, streaming reads of files rather than random operations.

## Commands


### List files

`hdfs dfs -ls .` lists files/ directories for a specific path under hdfs:


    [cloudera@quickstart examples]$ hdfs dfs -ls 
	Found 5 items
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 05:15 _sqoop
	-rw-r--r--   1 cloudera cloudera       5051 2017-12-26 06:07 cloudera-manager.html
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 05:10 order_join
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 05:14 order_joined
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:32 problem1

---

`hdfs dfs -ls -d .` lists directories only for a specific path under hdfs:

	[cloudera@quickstart examples]$ hdfs dfs -ls  problem1
	Found 2 items
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:32 problem1/order-items
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:30 problem1/orders
	[cloudera@quickstart examples]$ hdfs dfs -ls -d  problem1
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:32 problem1

---

`hdfs dfs -ls -h  problem1/orders` lists file sizes in human readable fashion

	[cloudera@quickstart examples]$ hdfs dfs -ls -h  problem1/orders
	Found 5 items
	-rw-r--r--   1 cloudera cloudera          0 2017-12-28 07:30 problem1/orders/_SUCCESS
	-rw-r--r--   1 cloudera cloudera    160.2 K 2017-12-28 07:30 problem1/orders/part-m-00000.avro
	-rw-r--r--   1 cloudera cloudera    160.3 K 2017-12-28 07:30 problem1/orders/part-m-00001.avro
	-rw-r--r--   1 cloudera cloudera    160.4 K 2017-12-28 07:30 problem1/orders/part-m-00002.avro
	-rw-r--r--   1 cloudera cloudera    165.4 K 2017-12-28 07:30 problem1/orders/part-m-00003.avro
	[cloudera@quickstart examples]$ hdfs dfs -ls   problem1/orders
	Found 5 items
	-rw-r--r--   1 cloudera cloudera          0 2017-12-28 07:30 problem1/orders/_SUCCESS
	-rw-r--r--   1 cloudera cloudera     164090 2017-12-28 07:30 problem1/orders/part-m-00000.avro
	-rw-r--r--   1 cloudera cloudera     164157 2017-12-28 07:30 problem1/orders/part-m-00001.avro
	-rw-r--r--   1 cloudera cloudera     164278 2017-12-28 07:30 problem1/orders/part-m-00002.avro
	-rw-r--r--   1 cloudera cloudera     169339 2017-12-28 07:30 problem1/orders/part-m-00003.avro


---
	
`hdfs dfs -ls -R` lists files and directories recursively:

	[cloudera@quickstart examples]$ hdfs dfs -ls -R  problem1
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:32 problem1/order-items
	-rw-r--r--   1 cloudera cloudera          0 2017-12-28 07:32 problem1/order-items/_SUCCESS
	-rw-r--r--   1 cloudera cloudera     381708 2017-12-28 07:32 problem1/order-items/part-m-00000.avro
	-rw-r--r--   1 cloudera cloudera     385380 2017-12-28 07:32 problem1/order-items/part-m-00001.avro
	-rw-r--r--   1 cloudera cloudera     385026 2017-12-28 07:32 problem1/order-items/part-m-00002.avro
	-rw-r--r--   1 cloudera cloudera     376923 2017-12-28 07:32 problem1/order-items/part-m-00003.avro
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:30 problem1/orders
	-rw-r--r--   1 cloudera cloudera          0 2017-12-28 07:30 problem1/orders/_SUCCESS
	-rw-r--r--   1 cloudera cloudera     164090 2017-12-28 07:30 problem1/orders/part-m-00000.avro
	-rw-r--r--   1 cloudera cloudera     164157 2017-12-28 07:30 problem1/orders/part-m-00001.avro
	-rw-r--r--   1 cloudera cloudera     164278 2017-12-28 07:30 problem1/orders/part-m-00002.avro
	-rw-r--r--   1 cloudera cloudera     169339 2017-12-28 07:30 problem1/orders/part-m-00003.avro

You can use * to match specific files and directory names.


### read/ Write files

`hdfs dfs -cat filename` shows the content of a file as it is: 

	[cloudera@quickstart examples]$ hdfs dfs -cat problem1/orders/part-m-00000.avro
	��76��-����9����J�� 0:��sF-
	����.��2k������&���JD��\F���J�=
	�.����=A�j5���89���J�?���.���K*��.0��J�*��.�E��Q5�
	��J���g9L��. 1�J2(��J���.�{�.v*��J��J��2�7��)
	�ܺ6��� ....
	
---

`hdfs dfs -cat filename` shows the content of a file as a text on the terminal: 

	{"order_id":{"int":17219},"order_date":{"long":1383984000000},
	"order_customer_id":{"int":7749},"order_status":{"string":"CLOSED"}}
	{"order_id":{"int":17220},"order_date":{"long":1383984000000},
	"order_customer_id":{"int":4821},"order_status":{"string":"COMPLETE"}}
	{"order_id":{"int":17221},"order_date":{"long":1383984000000},
	"order_customer_id":{"int":2366},"order_status":{"string":"PENDING"}}

---

`hdfs dfs -appendToFile fsrc fdst` writes data to a file based on an existant files: 

	[cloudera@quickstart examples]$ hdfs dfs -touchz  problem1/orders/part-m-30000.avro
	[cloudera@quickstart examples]$ hdfs dfs -appendToFile 
	problem1/orders/part-m-00000.avro 
	problem1/orders/part-m-30000.avro

---

### Upload and download files

`hdfs dfs -put src dst` copies files from local OS FS to HDFS: 

	[cloudera@quickstart examples]$ ls
	cars.txt
	[cloudera@quickstart examples]$ hdfs dfs -put cars.txt problem1/cars 
	[cloudera@quickstart examples]$
	[cloudera@quickstart examples]$ hdfs dfs -ls -R  problem1
	-rw-r--r--   1 cloudera cloudera       3561 2018-04-02 06:30 problem1/cars
	drwxr-xr-x   - cloudera cloudera          0 2017-12-28 07:32 problem1/order-items
	-rw-r--r--   1 cloudera cloudera          0 2017-12-28 07:32 problem1/order-items/_SUCCESS
	...

 f option forces to overwrite the content

 l option forces to copy files with 1 factor replication
  
---

`hdfs dfs -get src dst` copies files from HFDS to OS FS:

	[cloudera@quickstart examples]$ hdfs dfs -get  problem1/cars ncars.txt
	[cloudera@quickstart examples]$ ls
	cars.txt  ncars.txt
	[cloudera@quickstart examples]$

p option preserves access and modification times, ownership and the mode

copyFromLocal, copyToLocal, and moveFromLocal do the same thing as get and put but they reference the file.

---

### File management

`hdfs dfs -cp src dst` copies a file from dst to new src

`hdfs dfs -mv src dst` moves a file from dst to new src

`hdfs dfs -rm file` sends files to the bin

`hdfs dfs -rm -r dir` `hdfs dfs -rm -R dir` `hdfs dfs -rmr dir` sends files under the directory to the bin recursively


f option hide error messages


`hdfs dfs -rmdir dir` sends files under the directory to the bin

`hdfs dfs -mkdir dir` creates a new directory 

`hdfs dfs -mkdir dir` creates a new directory 

f option will force the creation if the directory exits

`hdfs dfs -touchz` create an empty file

