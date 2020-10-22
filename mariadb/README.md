# MariaDB Database Guide
> MariaDB Database Guide

## Table of Contents
* [Version](#version)
* [User Create](#user-create)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Tables](#tables)
* [Table Drop](#table-drop)
* [Table Create](#table-create)
* [Table Creation](#table-creation)
* [Table Columns](#table-columns)
* [Table Indexes](#table-indexes)
* [Index Create](#index-create)
* [Index Drop](#index-drop)
* [Table Select](#table-select)
* [Table Insert](#table-insert)
* [Table Update](#table-update)
* [Table Delete](#table-delete)
* [Table Truncate](#table-truncate)
* [Functions](#functions)
* [Function Drop](#function-drop)
* [Function Call](#function-call)
* [Procedures](#procedures)
* [Procedure Drop](#procedure-drop)
* [Procedure Call](#procedure-call)
* [Procedure Status](#procedure-status)
* [Procedure Creation](#procedure-creation)

### Version
* 0.0.1

### User Create
* `GRANT ALL ON *.* TO <user_name>@<ip_address> IDENTIFIED BY '<userpassword>' WITH GRANT OPTION;`
  * IP_Address means user can login from a specific IP address
  * "WITH GRANT OPTION" is optional
* `FLUSH PRIVILEGES;`
  * The above command is to apply changes
* `GRANT ALL ON *.* TO <user_name>@'%' IDENTIFIED BY '<userpassword>' WITH GRANT OPTION;`
  * '%' means user can login from any IP
  * "WITH GRANT OPTION" is optional
* FLUSH PRIVILEGES;
  * The above command is to apply changes

### Databases
* `show databases;`

### Database Drop
* `drop database if exists <databasename>;`

### Database Create
* ``create database if not exists `<databasename>` default character set utf8mb4 collate utf8mb4_unicode_520_ci;``

### Database Connect
* `use <databasename>;`

### Tables
* <pre>
  select
  tab.table_schema as `table_schema`,
  tab.table_name as `table_name`
  from information_schema.tables tab
  where
  tab.table_type in ('BASE TABLE') and
  tab.table_schema not in ('information_schema', 'mysql', 'performance_schema','sys')
  order by tab.table_schema asc, tab.table_name asc;
  </pre>

### Table Drop
* `drop table if exists <tablename>;`

### Table Create
* <pre>
  create table if not exists &lt;tablename&gt;(
    `tableID` bigint(20) unsigned not null auto_increment,
    `columnOne` int(11) not null,
    `columnTwo` varchar(255) collate utf8mb4_unicode_520_ci not null,
    `columnThree` text collate utf8mb4_unicode_520_ci default null,
    `columnFour` bit(1) not null default b'0',
    `columnFive` datetime(6) not null default current_timestamp(),
    `columnSix` datetime(6) default current_timestamp(),
    primary key (`tableID`),
    unique key `UQ_&lt;tablename&gt;_&lt;columnname&gt;` (`&lt;columnname&gt;`),
    index `IX_&lt;tablename&gt;_&lt;columnname&gt;` (`&lt;columnname&gt;`)
  ) engine=InnoDB default charset=utf8mb4 collate utf8mb4_unicode_520_ci;
  </pre>

### Table Creation
* `show create table <tablename>;`

### Table Columns
* <pre>
  select
  col.table_schema as `table_schema`,
  col.table_name as `table_name`,
  col.ordinal_position as `position`,
  col.column_name as `column_name`,
  col.data_type as `data_type`,
  if(col.character_maximum_length is not null, col.character_maximum_length, col.numeric_precision) as `max_length`,
  if(col.datetime_precision is not null, col.datetime_precision, if(col.numeric_scale is not null, col.numeric_scale, 0)) as `data_precision`,
  col.is_nullable as `is_nullable`,
  col.column_default as `column_default`
  from information_schema.columns col
  where
  col.table_name in ('&lt;tablename&gt;') and
  col.table_schema in ('&lt;tableschema&gt;')
  order by col.table_name asc, col.ordinal_position asc;
  </pre>

### Table Indexes
* `show index from <tablename>;`

### Index Create
* `create index `IX_<tablename>_<columnname>` on <tablename> (`<columnname>`);`

### Index Drop
* `drop index if exists `IX_<tablename>_<columnname>` on <tablename>;`

### Table Select
* <pre>
  select
  tableID as `tableID`,
  columnOne as `columnOne`,
  columnTwo as `columnTwo`,
  columnThree as `columnThree`,
  columnFour as `columnFour`,
  columnFive as `columnFive`,
  columnSix as `columnSix`
  from &lt;tablename&gt;;
  </pre>

### Table Insert
* `insert into <tablename> (columnOne, columnTwo, columnThree, columnFour, columnFive, columnSix) values (0, 'columnTwo', 'columnThree', 1, current_timestamp(6), current_timestamp(6));`

### Table Update
* <pre>
  update &lt;tablename&gt;
  set
  columnFour = 1,
  columnSix = current_timestamp(6)
  where
  columnFour = 0;
  </pre>

### Table Delete
* <pre>
  delete from &lt;tablename&gt;
  where
  tableID = 1;
  </pre>

### Table Truncate
* `truncate table <tablename>;`

### Functions
* `show function status;`

### Function Drop
* `drop function if exists <functionname>;`

### Function Call
* `call <functionname> ('<parametervalue>', ...);`

### Procedures
* `show procedure status;`

### Procedure Drop
* `drop procedure if exists <procedurename>;`

### Procedure Call
* `call <procedurename> ('<parametervalue>', ...);`

### Procedure Status
* `show procedure status;`

### Procedure Creation
* `show create procedure <procedurename>;`
