Hive:
------
- It is Datawarehouse on top of HADOOP.
- Hive is originated by FACEBOOK.
- It is sql inerface on top of HADOOP.
- Hive parses the sql query in the series of map reduce jobs.
- Hive has a schema evolution available.
- Map reduce code can be integrated with hive using UDFs.
- Have has jdbc/odbc drivers to connect the db clients.
- Hive uses Hadoop as storage.
- Data must be in HADOOP to run hive queries.
- Informatica can be connected to Hive and it can do ETL operations.
- Main difference b/w Hive and pig is connectivity.(Pig doesn't support connectivity)
- HQL is 99% similar to SQL.
- Hive cannot replace RDBMS.
- Hive cannot be installed without Hadoop.
- Hive Supports:
    DDL(Create, Drop, Alter, Load)
    DML(Select, Where, Order By, Group By)
    UDF(Python, Java etc.)

Important Points:
----------------
- If we are having cloudera cluster and we are running Hive query then the query will be converted into map reduce job.

- If we are having Hortonworks cluster and we are running Hive query then the query will be converted into Tez job.
  We can change the setting to run MR job.
  set hive.execution.engine=mr;

- Tez is another framework like map reduce which is quite faster than mapreduce.

- Because of the limilation of performance with cloudera. Cloudera is having another query engine Impala which is quite faster. So for best performance in cloudera cluster we should use tool called Impala. One drawback wit Impala is that it is not fault tolerant.

- Open-Source updates will not be available immidiately in cloudera but will be available in hortonworks as this is apache project.

- Using Hive LLAP. Queries can be run in microseconds. Its is available in hortonworks plateform.

Hive Architecture:
----------------------
- Hive provides CLI and Web GUI to run the Hive queries.
- We can write Hive queries in .sql file and execute that.


Hive Building blocks:
--------------------

Hive Metastore:
---------------
All the metadata is getting stored in metastore. Metastore is nothing but the RDBMS. It stores the schema of:
 - Tables
 - Databases
 - Columns in table
 - Datatypes
 - HDFS mapping

JDBC/ODBC Connector:
---------------------
Allows Java/Python, C++ etc applications to connect using JDBC driver.

Thrift Server:
--------------
- Thrift is apache protocol.
- It allows us to communicate through any language.
- Spark SQL can communicate to hive using thrift server. 
- Where ODBC/JDBC is not supported. Thrift server can handle that communication.

Hive Driver:
------------
- When we run a hive query it will hit the server(The driver part). Te sql query will be compiled and then optimized by CBO(Cost based optimizer).
After that the query will be given to executor that is nothing but the map reduce or Tez. This map reduce job then executed through Name node using resource manager.

Driver has 4 parts:
  - Parser & Planner: 
	Driver passes query to a compiler where parsing and planning phases happen.
	In Parsing query is analyzed into logical components.
	In Planning, query is converted to MR jobs.
	It analizes the semantics of the query.
  - Optimizer & Executor:
	Optimizer optimizes the query.
	Executor executes the MR jobs.
	
Data Organisation in Hive(Data Model):
-------------------------------------
- Hive stores the data in HDFS.
- All the data in a table will be there in HDFS directory.
- This directory can have subdirectories which are called partitions.
- These partition contains the files called buckets.

ACID Transactions in Hive:
--------------------------
Hive is not designed for ACID transactions. But Hive has started supporting ACID transactions recently. We need to manually enable it on hortonworks plateform. We can use SCDs in hive now.

Indexing in Hive:
------------------
Hive has 2 types of indexes.
 - Compact Indexes
 - Bitmap Indexes

Note:
----- 
 - Opensource community doesn't recommend to use these indexes because they are not efficient.

 - Whenever possible create a hive table in ORC format for faster operations because ORC has built in indexes.

 - ORC is default standard in Hive.
 
Partitioning & Bucketing:
---------------------------
 Partitions: 
 -----------
- Groups same type of data together based on column or partition key.
- Using partition, it is easy to query a portion of data.
- Users need to filter data on specific column values.
- Determines distribution of data within subdirectories.
- Supported for both managed and external tables.
- Hive scans only data directories that are needed.

Example:
-------
	hive> CREATE TABLES Sales(sale_id INT, amout FLOAT) PARTITIONED BY (country STRING, year INT, month INT);

So each artition will be split out into different folders like

Sales/country=US/year=2012/month=12

Types of partitions:
--------------------
 Static Partition:
 -----------------
 - Insert input data files individually into a partition tables is static partitioning
 - We "Statically" add a partition in table and move the file into the partition of the table
 - Preferred while loading files into table.
 
  Example:
  --------
	LOAD DATA LOCAL inpath '/home/manish6007/hyderabad.log' INTO TABLE cityreport partition (city='hyderabad');

 Dynamic Partition:
 ------------------
 - Single insert to partitioned table  is known as dynamic patitioning.
 - Loads data from non partitioned table.
 - Preferred when we have large data stored in a table.
 - Below properties must be enabled(either in hive shell or hive-site.xml) while using dynamic partitioning.
	hive> SET hive.exec.dynamic.partition = true;
	hive> SET hive.exec.dynamic.partition.mode = nonstrict;
	
  Example:
  --------
 	 hive> INSERT INTO TABLE temps_partition_date PARTITION (datelocal) SELECT statecode, contrycode, itenum, paramcode, poc, latitude, longitude, datum, param, datelocal FROM temps_txt;
	 
Buckets (or Clusters):
----------------------
- Data in each partition may in turn divided into Buckets based on the hash function of a column of a table.
- Table may be bucketed by field of the table, which is one of the columns, other than the partition columns of the table.
- Can be used to efficiently group data.
- Used in instances where partitioned file size still remains huge.
- Allows user to devide table datasets into more manageable parts.
- Records which are bucketed by the same column will always be saved in the same bucket.
- CLUSTERED BY clause is used to devide table into buckets.
- Like partition creates a directory, a bucket created a file.
- Can be done even without partitioning
-- Below properties must be enabled(either in hive shell or hive-site.xml) while using bucketing.
	hive> SET hive.enforce.bucketing = true
	
 Creating bucket table:
 ----------------------
  	hive> CREATE TABLE bucket_table(street STRING, zip INT, state STRING, beds INT, baths INT, sq_ft INT, flat_type STRING, price FLOAT) PARTITIONED BT (city STRING) CLUSTERED BY (street) INTO 4 BUCKETS ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
  
 Inserting data in bucketed table:
 ---------------------------------
 	hive> INSERT INTO TABLE bucket_table PARTITION(city) select street,zip,state,beds,baths,sq_ft,flat_type,price,city from input_table;



	


