# MariaDB Database Guide
> MariaDB Database Guide

## Table of Contents
* [Version](#version)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Tables](#tables)
* [Table Create](#table-create)
* [Table Creation](#table-creation)
* [Table Columns](#table-columns)
* [Table Indexes](#table-indexes)
* [Table Drop](#table-drop)
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

### Table Create
* <pre>
    create table if not exists <tablename>(
      `tableID` bigint(20) unsigned not null auto_increment,
      `columnOne` int(11) not null,
      `columnTwo` varchar(255) collate utf8mb4_unicode_520_ci not null,
      `columnThree` text collate utf8mb4_unicode_520_ci default null,
      `columnFour` bit(1) not null default b'0',
      `columnFive` datetime not null default current_timestamp(),
      `columnSix` datetime default current_timestamp(),
      primary key (`tableID`),
      unique key `UQ_<tablename>_columnOne` (`columnOne`),
      index `IX_<tablename>_columnTwo` (`columnTwo`)
    ) engine=InnoDB default charset=utf8mb4 collate utf8mb4_unicode_520_ci;
  </pre>

### Table Creation
* `show create table <tablename>;`

### Table Columns
* `show columns in <tablename>;`

### Table Indexes
* `show index from <tablename>;`

### Table Drop
* `drop table if exists <tablename>;`

### Table Select
* `select tableID, columnOne, columnTwo, columnThree, columnFour, columnFive, columnSix from <tablename>;`

### Table Insert
* `insert into <tablename> (columnOne, columnTwo, columnThree, columnFour, columnFive, columnSix) values (0, 'columnTwo', 'columnThree', 1, current_timestamp(), current_timestamp());`

### Table Update
* `update <tablename> set columnFour = 1 where columnFour = 0;`

### Table Delete
* `delete from <tablename> where tableID = 1;`

### Table Truncate
* `truncate table <tablename>;`

### Procedures
* `show procedure status;`

### Functions
* `show function status;`
