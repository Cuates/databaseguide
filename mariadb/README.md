# MariaDB Database Guide
> MariaDB Database Guide

## Table of Contents
* [Version](#version)
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
* [Table Select](#table-select)
* [Table Insert](#table-insert)
* [Table Update](#table-update)
* [Table Delete](#table-delete)
* [Table Truncate](#table-truncate)
* [Procedures](#procedures)
* [Functions](#functions)

### Version
* 0.0.1

### Databases
* `show databases;`

### Database Drop
* `drop database if exists <databasename>;`

### Database Create
* ``create database if not exists `databasename` default character set utf8mb4 collate utf8mb4_unicode_520_ci;``

### Database Connect
* `use <databasename>;`

### Tables
* `show tables;`

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
    unique key `UQ_&lt;tablename&gt;_columnOne` (`columnOne`),
    index `IX_&lt;tablename&gt;_columnTwo` (`columnTwo`)
  ) engine=InnoDB default charset=utf8mb4 collate utf8mb4_unicode_520_ci;
  </pre>

### Table Creation
* `show create table <tablename>;`

### Table Columns
* `show columns in <tablename>;`

### Table Indexes
* `show index from <tablename>;`

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

### Procedures
* `show procedure status;`

### Functions
* `show function status;`
