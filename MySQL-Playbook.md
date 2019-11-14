**Welcome to the MySQL-Playbook wiki!**

MySQL is a most popular open-source Relational Database Management System(RDBMS). MySQL is used for many small and big businesses. It is developed, marketed and supported by MySQL AB, a Swedish company. It is written in C and C++.

### Table of Contents

1) [Introduction](#wiki-introduction)
2) [Best Practices in MySQL](#wiki-best-practices-in-mysql)
3) [MySQL Storage Engines](#wiki-mysql-storage-engines)
4) [MySQL Stored Function](#wiki-mysql-stored-function)
5) [MySQL Stored Procedures](#wiki-mysql-stored-procedures)
6) [MySQL Triggers](#wiki-mysql-triggers)
7) [Optimization and Indexing](#wiki-optimization-and-indexing)
8) [TroubleShooting](#wiki-troubleshooting)
9) [Why Choose MySQL](#wiki-why-choose-mysql)

## Introduction

Reasons of popularity (From [Javatpoint.com](https://www.javatpoint.com/))

MySQL is becoming so popular because of these following reasons:

* MySQL is an open-source database so you don't have to pay a single penny to use it.
* MySQL is a very powerful program so it can handle a large set of functionality of the most expensive and powerful database packages.
* MySQL is customizable because it is an open source database and the open-source GPL license facilitates programmers to * * modify the SQL software according to their own specific environment.
* MySQL is quicker than other databases so it can work well even with the large data set.
* MySQL supports many operating systems with many languages like PHP, PERL, C, C++, JAVA, etc.
* MySQL uses a standard form of the well-known SQL data language.
* MySQL is very friendly with PHP, the most popular language for web development.
* MySQL supports large databases, up to 50 million rows or more in a table. The default file size limit for a table is 4GB, but you can increase this (if your operating system can handle it) to a theoretical limit of 8 million terabytes (TB).

Find how MySQL started and evolved [MySQL History](https://www.javatpoint.com/mysql-history).

### MySQL Data Types

1) [Numeric Data Type](https://www.javatpoint.com/mysql-data-types)
2) [Date and Time Data Type](https://www.javatpoint.com/mysql-data-types)
3) [String Data Types](https://www.javatpoint.com/mysql-data-types)
4) [Large Object Data Type](https://www.javatpoint.com/mysql-data-types)

MySQL CRUD Queries on table [MySQL Queries](https://www.javatpoint.com/mysql-queries)

## Best-Practices-in-MySQL

To Select proper data type that is required is Necessary. Index key columns, Don't use functions over index columns many other aspects that need to be taken care when designing your database.

1) [Always use Proper Data type](http://bigdata-madesimple.com/top-10-best-practices-in-mysql/)
2) [Vertical Partioning and Smaller Columns are Faster](https://code.tutsplus.com/tutorials/top-20-mysql-best-practices--net-7855)
3) [MySQL Security Best Practices](https://www.mysql.com/why-mysql/presentations/mysql-security-best-practices/)
4) [Best Practices for InnoDB Tables](https://dev.mysql.com/doc/refman/5.7/en/innodb-best-practices.html)
5) [MySQL Performance Tips](https://www.slideshare.net/imosquera/my-sql-performance-36722400)
6) [Best Practices for Configuring Optimal MySQL Memory Usage](https://www.percona.com/blog/2016/05/03/best-practices-for-configuring-optimal-mysql-memory-usage/)
7) [Best Practice for Database Design](https://dzone.com/articles/20-database-design-best)
8) [Data Handled by MySQL Effectively](https://stackoverflow.com/questions/18268196/best-practices-while-designing-databases-in-mysql)
9) [MySQL Indexes](https://stackoverflow.com/questions/3049283/mysql-indexes-what-are-the-best-practices)
10) [MySQL Performance Tuning](https://logicalread.com/mysql-tuning-best-practices/#.WS4LHJKGO00)
11) [MySQL High Availability](https://www.percona.com/resources/webinars/best-practices-mysql-high-availability)

Follow above links to Build High Available, Reliable and Efficient use of Resources is Confirmed.

## MySQL-Storage-Engines

Storage engines are MySQL components that handle the SQL operations for different table types. InnoDB is the default and most general-purpose storage engine, and Oracle recommends using it for tables except for specialized use cases. (The CREATE TABLE statement in MySQL 5.7 creates InnoDB tables by default.)

MySQL Server uses a pluggable storage engine architecture that enables storage engines to be loaded into and unloaded from a running MySQL server.

To determine which storage engines your server supports, use the SHOW ENGINES statement. The value in the Support column indicates whether an engine can be used. A value of YES, NO, or DEFAULT indicates that an engine is available, not available, or available and currently set as the default storage engine.


mysql> SHOW ENGINES;

| Engine     | Support | Comment                                                        | Transactions | XA  | Savepoints| 
|------------|---------|----------------------------------------------------------------|--------------|------|----------|
| InnoDB     | YES     | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES      | 
| MRG_MYISAM | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO       | 
| BLACKHOLE  | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO       |  
| CSV        | YES     | CSV storage engine                                             | NO           | NO   | NO       |  
| MEMORY     | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO       | 
| FEDERATED  | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL     |  
| ARCHIVE    | YES     | Archive storage engine                                         | NO           | NO   | NO       | 
| MyISAM     | DEFAULT | Default engine as of MySQL 3.23 with great performance         | NO           | NO   | NO       |  

### Understand each Storage Engine

* InnoDB: The default storage engine in MySQL 5.7. InnoDB is a transaction-safe (ACID compliant) storage engine for MySQL that has commit, rollback, and crash-recovery capabilities to protect user data. InnoDB row-level locking (without escalation to coarser granularity locks) and Oracle-style consistent nonblocking reads increase multi-user concurrency and performance. InnoDB stores user data in clustered indexes to reduce I/O for common queries based on primary keys. To maintain data integrity, InnoDB also supports FOREIGN KEY referential integrity constraints. For more information about InnoDB, see Chapter 14, The InnoDB Storage Engine.

* MyISAM: These tables have a small footprint. Table-level locking limits the performance in read/write workloads, so it is often used in read-only or read-mostly workloads in Web and data warehousing configurations.

* Memory: Stores all data in RAM, for fast access in environments that require quick lookups of non-critical data. This engine was formerly known as the HEAP engine. Its use cases are decreasing; InnoDB with its buffer pool memory area provides a general-purpose and durable way to keep most or all data in memory, and NDBCLUSTER provides fast key-value lookups for huge distributed data sets.

* CSV: Its tables are really text files with comma-separated values. CSV tables let you import or dump data in CSV format, to exchange data with scripts and applications that read and write that same format. Because CSV tables are not indexed, you typically keep the data in InnoDB tables during normal operation and only use CSV tables during the import or export stage.

* Archive: These compact, unindexed tables are intended for storing and retrieving large amounts of seldom-referenced historical, archived, or security audit information.

* Blackhole: The Blackhole storage engine accepts but does not store data, similar to the Unix /dev/null device. Queries always return an empty set. These tables can be used in replication configurations where DML statements are sent to slave servers, but the master server does not keep its own copy of the data.

* NDB (also known as NDBCLUSTER): This clustered database engine is particularly suited for applications that require the highest possible degree of uptime and availability.

* Merge: Enables a MySQL DBA or developer to logically group a series of identical MyISAM tables and reference them as one object. Good for VLDB environments such as data warehousing.

* Federated: Offers the ability to link separate MySQL servers to create one logical database from many physical servers. Very good for distributed or data mart environments.

**Example**: This engine serves as an example in the MySQL source code that illustrates how to begin writing new storage engines. It is primarily of interest to developers. The storage engine is a “stub” that does nothing. You can create tables with this engine, but no data can be stored in them or retrieved from them.

You are not restricted to using the same storage engine for an entire server or schema. You can specify the storage engine for any table. For example, an application might use mostly InnoDB tables, with one CSV table for exporting data to a spreadsheet and a few MEMORY tables for temporary workspaces.

Choosing a Storage Engine

The various storage engines provided with MySQL are designed with different use cases in mind. The following table provides an overview of some storage engines provided with MySQL:


| Feature	                | MyISAM    | Memory | InnoDB | Archive  |  NDB     |
|-------------------------------|-----------|--------|--------|----------|----------|
|Storage limits	                | 256TB	    |  RAM   | 64TB   |  None    |  384EB   |
| Transactions	                | No	    |  No    | Yes    |  No      |  Yes     |
| Locking granularity           | Table	    |  Table | Row    |  Row     |  Row     |
| MVCC	                        | No	    | No     | Yes    |  No      |  No      |
| Geospatial data type support	| Yes	    | No     | Yes    |  Yes	 |  Yes     |
| Geospatial indexing support	| Yes	    | No     | Yes[a] |	 No	 |  No      |
| B-tree indexes                | Yes       | Yes    | Yes    |  No      |  No      |
| T-tree indexes	        | No	    | No     | No     |	 No	 |  Yes     |
| Hash indexes	                | No	    | Yes    | No[b]  |	 No	 |  Yes     |
| Full-text search indexes      | Yes	    | No     | Yes[c] |	 No	 |  No      |
| Clustered indexes	        | No	    | No     | Yes    |	 No	 |  No      |
| Data caches	                | No	    | N/A    | Yes    |	 No	 |  Yes     |
| Index caches	                | Yes	    | N/A    | Yes    |  No   	 |  Yes     |
| Compressed data	        | Yes[d]    | No     | Yes[e] |  Yes	 |  No      |
| Encrypted data[f]	        | Yes	    | Yes    | Yes    |  Yes     |  Yes     |
| Cluster database support      | No	    | No     | No     |  No	 |  Yes     |
| Replication support[g]	| Yes	    | Yes    | Yes    | Yes      |  Yes     |
| Foreign key support	        | No	    | No     | Yes    | No       |  Yes[h]  |
| Backup/point-in-time recovery | Yes	    | Yes    | Yes    | Yes 	 | Yes      |
| Query cache support	        | Yes	    | Yes    | Yes    | Yes      | Yes      |
| Update statistics for data    | Yes       | Yes    | Yes    | Yes      | Yes      |

## MySQL-Stored-Function

A stored function is a special kind stored program that returns a single value. You use stored functions to encapsulate common formulas or business rules that are reusable among SQL statements or stored programs.

Different from a stored procedure, you can use a stored function in SQL statements wherever an expression is used. This helps improve the readability and maintainability of the procedural code.

## MySQL stored function syntax

The following illustrates the simplest syntax for creating a new stored function:

    CREATE FUNCTION function_name(param1,param2,…)
        RETURNS datatype
    [NOT] DETERMINISTIC
       statements

    CREATE FUNCTION function_name(param1,param2,…)
        RETURNS datatype
    [NOT] DETERMINISTIC
       statements

First, you specify the name of the stored function after  CREATE FUNCTION  clause.

Second, you list all parameters of the stored function inside the parentheses. By default, all parameters are IN parameters. You cannot specify IN, OUT or INOUT modifiers to the parameters.

Third, you must specify the data type of the return value in the RETURNS statement. It can be any valid MySQL data types.

Fourth, for the same input parameters, if the stored function returns the same result, it is considered deterministic and otherwise, the stored function is not deterministic. You have to decide whether a stored function is deterministic or not. If you declare it incorrectly, the stored function may produce an unexpected result, or the available optimization is not used which degrades the performance.

Fifth, you write the code in the body of the stored function. It can be a single statement or a compound statement. Inside the body section, you have to specify at least one RETURN statement. The RETURN statement returns a value to the caller. Whenever the RETURN statement is reached, the stored function’s execution is terminated immediately.

MySQL stored function example

Let’s take a look at an example of using stored function. We will use the customers table in the sample database for the demonstration.

The following example is a function that returns the level of a customer based on credit limit. We use the IF statement to decide the credit limit.


	DELIMITER $$

	CREATE FUNCTION CustomerLevel(p_creditLimit double) RETURNS VARCHAR(10)
		DETERMINISTIC
	BEGIN
		DECLARE lvl varchar(10);

		IF p_creditLimit > 50000 THEN
		SET lvl = 'PLATINUM';
		ELSEIF (p_creditLimit <= 50000 AND p_creditLimit >= 10000) THEN
			SET lvl = 'GOLD';
		ELSEIF p_creditLimit < 10000 THEN
			SET lvl = 'SILVER';
		END IF;

		RETURN (lvl);
	END

	DELIMITER $$
	 
	CREATE FUNCTION CustomerLevel(p_creditLimit double) RETURNS VARCHAR(10)
		DETERMINISTIC
	BEGIN
		DECLARE lvl varchar(10);
	 
		IF p_creditLimit > 50000 THEN
	 SET lvl = 'PLATINUM';
		ELSEIF (p_creditLimit <= 50000 AND p_creditLimit >= 10000) THEN
			SET lvl = 'GOLD';
		ELSEIF p_creditLimit < 10000 THEN
			SET lvl = 'SILVER';
		END IF;
	 
	 RETURN (lvl);
	END


Now, we can call the CustomerLevel() in an SQL SELECT statement as follows:

	SELECT customerName, CustomerLevel(creditLimit)	FROM customers
	ORDER BY customerName;

	SELECT customerName, CustomerLevel(creditLimit) FROM customers
	ORDER BY customerName;

## MySQL-Stored-Procedures

Writing the first MySQL stored procedure

We are going to develop a simple stored procedure named GetAllProducts()  to help you get familiar with the syntax. The GetAllProducts()  stored procedure selects all products from the products  table.

Launch the mysql client tool and type the following commands:

 	 DELIMITER //
	 CREATE PROCEDURE GetAllProducts()
	   BEGIN
	   SELECT *  FROM products;
	   END //
	 DELIMITER ;

	 DELIMITER //
	 CREATE PROCEDURE GetAllProducts()
	   BEGIN
	   SELECT *  FROM products;
	   END //
	 DELIMITER ;

Create MySQL stored procedure using command-line tool

Let’s examine the stored procedure in greater detail:

* The first command is DELIMITER //, which is not related to the stored procedure syntax. The DELIMITER statement changes the standard delimiter which is a semicolon (; ) to another. In this case, the delimiter is changed from the semicolon(; ) to double slashes // Why do we have to change the delimiter? Because we want to pass the stored procedure to the server as a whole rather than letting myself tool interpret each statement at a time.  Following the END keyword, we use the delimiter //  to indicate the end of the stored procedure. The last command ( DELIMITER; ) changes the delimiter back to the semicolon (;).

* We use the CREATE PROCEDURE  statement to create a new stored procedure. We specify the name of stored procedure after the CREATE PROCEDURE  statement. In this case, the name of the stored procedure is GetAllProducts. We put the parentheses after the name of the stored procedure.
The section between BEGIN and END  is called the body of the stored procedure. You put the declarative SQL statements in the body to handle business logic. In this stored procedure, we use a simple SELECT statement to query data from the products table.

* It’s kind of tedious to write the stored procedure in MySQL client tool, especially when the stored procedure is complex. Most of the GUI tools for MySQL allow you to create new stored procedures via an intuitive interface.

## MySQL-Triggers

A SQL trigger is a set of  SQL statements stored in the database catalog. A SQL trigger is executed or fired whenever an event associated with a table occurs e.g.,  insert, update or delete.

A SQL trigger is a special type of stored procedure. It is special because it is not called directly like a stored procedure. The main difference between a trigger and a stored procedure is that a trigger is called automatically when a data modification event is made against a table whereas a stored procedure must be called explicitly.

**Advantages of using SQL triggers**

* SQL triggers provide an alternative way to check the integrity of data.
* SQL triggers can catch errors in business logic in the database layer.
* SQL triggers provide an alternative way to run scheduled tasks. By using SQL triggers, you don’t have to wait to run the scheduled tasks because the triggers are invoked automatically before or after a change is made to the data in the tables.
* SQL triggers are very useful to audit the changes of data in tables.

**Disadvantages of using SQL triggers**

* SQL triggers only can provide an extended validation and they cannot replace all the validations. Some simple validations have to be done in the application layer. For example, you can validate user’s inputs on the client side by using JavaScript or on the server side using server-side scripting languages such as JSP, PHP, ASP.NET, Perl, etc.
* SQL triggers are invoked and executed invisible from the client applications, therefore, it is difficult to figure out what happens in the database layer.
* SQL triggers may increase the overhead of the database server.

A database trigger is a powerful tool for protecting the integrity of the data in your MySQL databases. In addition, it is useful to automate some database operations such as logging, auditing, etc.

1) [Implementation of MySQL Triggers](http://www.mysqltutorial.org/mysql-trigger-implementation.aspx)
2) [Create Triggers in MySQL](http://www.mysqltutorial.org/create-the-first-trigger-in-mysql.aspx)
3) [Create Multiple Triggers for the Same Trigger Event and Action time](http://www.mysqltutorial.org/mysql-triggers/create-multiple-triggers-for-the-same-trigger-event-and-action-time/)
4) [Managing Triggers in MySQL](http://www.mysqltutorial.org/managing-trigger-in-mysql.aspx)
5) [Working With MySQL Scheduled Event](http://www.mysqltutorial.org/mysql-triggers/working-mysql-scheduled-event/)
6) [Modifying MySQL Events](http://www.mysqltutorial.org/mysql-triggers/modifying-mysql-events/)


### MySQL Cursors

MySQL supports cursors inside stored programs. The syntax is as in embedded SQL. Cursors have these properties:
A sensitive: The server may or may not make a copy of its result table

Read only: Not updatable
Nonscrollable: Can be traversed only in one direction and cannot skip rows
Cursor declarations must appear before handler declarations and after variable and condition declarations.

**Example:**

    CREATE PROCEDURE curdemo()
    BEGIN
       DECLARE done INT DEFAULT FALSE;
       DECLARE a CHAR(16);
       DECLARE b, c INT;
       DECLARE cur1 CURSOR FOR SELECT id,data FROM test.t1;
       DECLARE cur2 CURSOR FOR SELECT i FROM test.t2;
       DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
       OPEN cur1;
       OPEN cur2;
       read_loop: LOOP
	   FETCH cur1 INTO a, b;
	   FETCH cur2 INTO c;
	   IF done THEN
		 LEAVE read_loop;
	   END IF;
	   IF b < c THEN
		 INSERT INTO test.t3 VALUES (a,b);
	   ELSE
		 INSERT INTO test.t3 VALUES (a,c);
	   END IF;
	END LOOP;
	CLOSE cur1;
	CLOSE cur2;
    END;

**Cursor Syntax**
1) [Cursor CLOSE Syntax](https://dev.mysql.com/doc/refman/5.7/en/close.html)
2) [Cursor Declare Syntax](https://dev.mysql.com/doc/refman/5.7/en/declare-cursor.html)
3) [Cursor FETCH Syntax](https://dev.mysql.com/doc/refman/5.7/en/fetch.html)
4) [Cursor OPEN Syntax](https://dev.mysql.com/doc/refman/5.7/en/open.html)

## Optimization-and-Indexing

* [How MySQL Uses Indexes](#how-mysql-uses-indexes)
* [Using Primary Keys](https://dev.mysql.com/doc/refman/5.5/en/optimizing-primary-keys.html)
* [Foreign Indexes](https://dev.mysql.com/doc/refman/5.5/en/optimizing-foreign-keys.html)
* [Column Indexes](https://dev.mysql.com/doc/refman/5.5/en/column-indexes.html)
* [Multi Column Indexes](https://dev.mysql.com/doc/refman/5.5/en/multiple-column-indexes.html)
* [Verifying Index Usages](https://dev.mysql.com/doc/refman/5.5/en/verifying-index-usage.html)
* [InnoDB and MyISAM Index Statistics Collection](https://dev.mysql.com/doc/refman/5.5/en/index-statistics.html)
* [Comparison of B-Trees and Hash Indexes](https://dev.mysql.com/doc/refman/5.5/en/index-btree-hash.html)

To improve your SELECT(DML Language) query to improve its performance then create index of one or more of the columns that are tested in the query. The index entries works as a pointer to the rows of the table. thus the complexity of selecting any record from where clause reduces. Also all MySQL data types can be indexed. But you need to take care while using index that indexing allocated memory. So using index for each column with takes a lot of memory that slow downs Disk IO operations.

## How MySQL uses Indexes

Indexes are used to find row with a specific column. if you retrieving data from your email column that its good practice to create an index of it to access that row with with low temporal complexity but increases spatial complexity increases because of the allocated resources in disk.

Most MySQL indexes (**PRIMARY KEY, UNIQUE, INDEX, and FULLTEXT**) are stored in B-trees. Exceptions are that indexes on spatial data types use R-trees, and that MEMORY tables also support hash indexes.

In Memory tables hash indexes are used. To compare them  [ Comparison of B-Tree and Hash Indexes”](https://dev.mysql.com/doc/refman/5.5/en/index-btree-hash.html).

**MySQL uses indexes for these operations:**

* To find the rows matching a WHERE clause quickly.

* To eliminate rows from consideration. If there is a choice between multiple indexes, MySQL normally uses the index that finds the smallest number of rows (the most selective index).

* If the table has a multiple-column index, any leftmost prefix of the index can be used by the optimizer to look up rows. For example, if you have a three-column index on (col1, col2, col3), you have indexed search capabilities on (col1), (col1, col2), and (col1, col2, col3). For more information, see Section 8.3.5, “Multiple-Column Indexes”.

* To retrieve rows from other tables when performing joins. MySQL can use indexes on columns more efficiently if they are declared as the same type and size. In this context, `VARCHAR` and `CHAR` are considered the same if they are declared as the same size. For example, `VARCHAR(10)` and `CHAR(10)` are the same size, but `VARCHAR(10)` and `CHAR(15)` are not.

* For comparisons between nonbinary string columns, both columns should use the same character set. For example, comparing a utf8 column with a latin1 column precludes use of an index.

* Comparison of dissimilar columns (comparing a string column to a temporal or numeric column, for example) may prevent use of indexes if values cannot be compared directly without conversion. For a given value such as 1 in the numeric column, it might compare equal to any number of values in the string column such as '1', ' 1', '00001', or '01.e1'. This rules out use of any indexes for the string column.

* To find the MIN() or MAX() value for a specific indexed column key_col. This is optimized by a preprocessor that checks whether you are using WHERE key_part_N = constant on all key parts that occur before key_col in the index. In this case, MySQL does a single key lookup for each `MIN()` or `MAX()` expression and replaces it with a constant. If all expressions are replaced with constants, the query returns at once. For example:

   ` SELECT MIN(key_part2),MAX(key_part2)`
  `  FROM tbl_name WHERE key_part1=10;`

* To sort or group a table if the sorting or grouping is done on a leftmost prefix of a usable index (for example, ORDER BY key_part1, key_part2). If all key parts are followed by DESC, the key is read in reverse order. See Section 8.2.1.10, “ORDER BY Optimization”, and Section 8.2.1.11, “GROUP BY Optimization”.

* In some cases, a query can be optimized to retrieve values without consulting the data rows. (An index that provides all the necessary results for a query is called a covering index.) If a query uses from a table only columns that are included in some index, the selected values can be retrieved from the index tree for greater speed:

   ` SELECT key_part3 FROM tbl_name`
`    WHERE key_part1=1`

* Indexes are less important for queries on small tables, or big tables where report queries process most or all of the rows. When a query needs to access most of the rows, reading sequentially is faster than working through an index. Sequential reads minimize disk seeks, even if not all the rows are needed for the query. See Section 8.2.1.16, “Avoiding Full Table Scans” for details.

## SET Operations and Relations on MySQL

### MySQL INNER JOIN clause

The MySQL INNER JOIN clause matches rows in one table with rows in other tables and allows you to query rows that contain columns from both tables.

The MySQL INNER JOIN clause is an optional part of the SELECT statement. It appears immediately after the FROM clause.

Before using MySQL INNER JOIN clause, you have to specify the following criteria:

First, you have to specify the main table that appears in the FROM clause.
Second, you need to specify the table that you want to join with the main table, which appears in the INNER JOIN clause. Theoretically, you can join a table with many tables. However, for better query performance, you should limit the number of tables to join.
Third, you need to specify the join condition or join predicate. The join condition appears after the keyword ON of the INNER JOIN clause. The join condition is the rule for matching rows in the main table and the other tables.
The syntax of the MySQL INNER JOIN clause is as follows:


    SELECT column_list
    FROM t1
    INNER JOIN t2 ON join_condition1
    INNER JOIN t3 ON join_condition2
    ...
    WHERE where_conditions;

    SELECT column_list 
    FROM t1
    INNER JOIN t2 ON join_condition1
    INNER JOIN t3 ON join_condition2
    ...
    WHERE where_conditions;

Let’s simplify the syntax above by assuming that we are joining two tables T1 and T2 using the INNER JOIN clause.

For each row in the T1 table, the MySQL INNER JOIN clause compares it with each row of the T2 table to check if both of them satisfy the join condition. When the join condition is matched, it will return the row that combines columns in both T1 and T2 tables.

Notice that the rows in both T1 and T2 tables have to be matched based on the join condition. If no match found, the query will return an empty result set. This logic is also applied if we join more than 2 tables.

The following Venn diagram illustrates how the MySQL INNER JOIN clause works. The rows in the result set must appear in both tables: T1 and T2.

> MySQL INNER JOIN Venn Diagram

> MySQL INNER JOIN Venn Diagram

> Avoid ambiguous column error in MySQL INNER JOIN

If you join multiple tables that have the same column name, you have to use table qualifier to refer to that column in the SELECT clause to avoid ambiguous column error.

For example, if both T1 and T2 tables have the same column named C in the SELECT clause, you have to refer to the C column using the table qualifiers as T1.C or T2.C.

### MySQL LEFT JOIN

Introducing to MySQL LEFT JOIN

The MySQL LEFT JOIN clause allows you to query data from two or more database tables. The LEFT JOIN clause is an optional part of the SELECT statement, which appears after the FROM clause.

Let’s assume that we are going to query data from two tables T1 and T2. The following statement illustrates the syntax of LEFT JOIN clause that joins the two tables:

 
    SELECT 
       T1.c1, T1.c2, T2.c1, T2.c2
    FROM
       T1
        LEFT JOIN
       T2 ON T1.c1 = T2.c1;
    
    SELECT 
       T1.c1, T1.c2, T2.c1, T2.c2
    FROM
       T1
        LEFT JOIN
    T2 ON T1.c1 = T2.c1;

When we join the T1 table to the T2 table using the LEFT JOIN clause, if a row from the left table T1 matches a row from the right table T2 based on the join condition ( T1.c1 = T2.c1 ), this row is included in the result set.

In case the row in the left table does not match the row in the right table, the row in the left table is also selected and combined with a “fake” row from the right table. The fake row contains NULL values for all corresponding columns in the SELECT clause.

In other words, the LEFT JOIN clause allows you to select rows from the both left and right tables that are matched, plus all rows from the left table ( T1 ) even there is no match found for them in the right table ( T2 ).

The following Venn diagram helps you visualize how the LEFT JOIN clause works. The intersection between two circles are rows that match in both tables, and the remaining part of the left circle are rows in the T1 table that do not have any matching row in the T2 table. All rows in the left table are included in the result set.

> MySQL LEFT JOIN - Venn Diagram

> MySQL LEFT JOIN – Venn Diagram

Notice that the returned rows must also match the condition in the WHERE and Be HAVING clauses if those clauses are available in the query.

### MySQL Self Join

You use self-join when you want to combine rows with other rows in the same table. To perform the self-join operation, you must use a table alias to help MySQL distinguish the left table from the right table of the same table.

MySQL self-join examples

Let’s take a look at the employee's table in the sample database.

In the employee's table, we store not only employees data but also organization structure data. The reports to the column are used to determine the manager ID of an employee.

myself self-join employees table

In order to get the whole organization structure, we can join the employee's table to itself using the employeeNumber and reportsTo columns. The employee's table has two roles: one is Manager and the other is Direct Report.


    SELECT CONCAT(m.lastname,', ',m.firstname) AS 'Manager',
          CONCAT(e.lastname,', ',e.firstname) AS 'Direct report'	
    FROM employees e
    INNER JOIN employees m ON m.employeeNumber = e.reportsto
    ORDER BY manager;

    SELECT CONCAT(m.lastname,', ',m.firstname) AS 'Manager',
          CONCAT(e.lastname,', ',e.firstname) AS 'Direct report' 
    FROM employees e
    INNER JOIN employees m ON m.employeeNumber = e.reportsto
    ORDER BY manager;

## Troubleshooting

List of Issues and Solutions for your MYSQL DB design, installation, High Availability Architecture.

## Why Choose MySQL

We are Listing Top 10 Reasons why and when you should choose MySQL as your choice of Database. 

The hardest part of enumerating the best reasons to use MySQL is figuring out in what order the reasons should be presented. It's like the age-old debate about the chicken and the egg. Are MySQL's lower costs of ownership (TCO) attributable to its simplicity? Is it ubiquitous because of that low TCO, or is it the other way around? There are no hard and fast boundaries separating the best features of MySQL; they run together like in a watercolor painting.

1) [Top 10 reasons from  Oracle](http://www.oracle.com/us/products/mysql/mysql-wp-top10-webbased-apps-461054.pdf)
2) [Benefits of MySQL](https://www.novell.com/documentation/nw65/web_mysql_nw/data/aj5bj52.html)
3) [Advantages of MySQL Over Other Database](https://www.datarealm.com/blog/five-advantages-disadvantages-of-mysql/)
4) [MySQL features](http://searchitchannel.techtarget.com/feature/What-are-the-top-MySQL-features-What-is-MySQL)
5) [Optimizations, Backups, Replica](https://books.google.co.in/books?id=JXFuCQAAQBAJ&pg=PR19&lpg=PR19&dq=MySQL+Suits+large+scale+application&source=bl&ots=8oYeW-tIdk&sig=FMaq7dWv0uS5SFHglszTQITW1As&hl=en&sa=X&ved=0ahUKEwimyfWs8ZjUAhWDwI8KHY9aDlMQ6AEILTAB#v=onepage&q=MySQL%20Suits%20large%20scale%20application&f=false)
6) [MySQL Supports Transaction](https://dev.mysql.com/doc/refman/5.7/en/sql-syntax-transactions.html)
7) [[In-Memory Database as MySQL (MySQL Engines)](https://stackoverflow.com/questions/4233816/what-are-mysql-database-engines)
8) [Five Compelling Reasons  ](http://www.cio.com/article/2438900/data-management/five-compelling-reasons-to-use-mysql.html)

### References

1) [MySQL Tutorials](http://www.mysqltutorial.org/)
2) [Official MySQL Documentation](https://dev.mysql.com/doc/refman/5.7/en/tutorial.html)
3) [Tutorials Point](https://www.tutorialspoint.com/mysql/)
4) [Javatpoint.com](https://www.javatpoint.com)