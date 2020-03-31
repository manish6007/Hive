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





	


