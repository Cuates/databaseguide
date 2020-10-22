# PostgreSQL Database Guide
> PostgreSQL Database Guide

## Table of Contents
* [Version](#version)
* [Extension](#extension)
* [User Create](#user-create)
* [Owner Reassign](#owner-reassign)
* [Owner Drop](#owner-drop)
* [User Drop](#user-drop)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Grant Privileges](#database-grant-privileges)
* [Database Alter Owner To Role](#database-alter-owner-to-role)
* [Database Connect](#database-connect)
* [Tables](#tables)
* [Table Drop](#table-drop)
* [Table Create](#table-create)
* [Sequence Drop](#sequence-drop)
* [Table Alter Owner](#table-alter-owner)
* [Table Owner](#table-owner)
* [Table Grant Privileges](#table-grant-privileges)
* [Index Create](#index-create)
* [Index Show All In Schema](#index-show-all-in-schema)
* [Indexes Show From A Table](#indexes-show-from-a-table)
* [Index Drop](#index-drop)
* [Sequence Grant Privileges](#sequence-grant_privileges)
* [Access Future Objects In The Database Automatically](#access-future-objects-in-the-database-automatically)
* [Access Future Objects In The Schema Automatically](#access-future-objects-in-the-schema-automatically)
* [Table Columns](#table-columns)
* [Sequnce Show Last Value](#sequnce-show-last-value)
* [Table Select](#table-select)
* [Table Insert](#table-insert)
* [Table Update](#table-update)
* [Table Delete](#table-delete)
* [Table Truncate](#table-truncate)
* [Table Truncate And Reseed Identity](#table-truncate-and-reseed-identity)
* [Functions](#functions)
* [Function Drop](#function-drop)
* [Function Call](#function-call)
* [Procedures](#procedures)
* [Procedure Alter Owner To Role](#procedure-alter-owner-to-role)
* [Procedure Drop](#procedure-drop)
* [Procedure Call](#procedure-call)

### Version
* 0.0.1

### Extension
* <pre>
  select
  pge.extname as "extname"
  from pg_extension pge;
  </pre>

### Extension Drop
* `drop extension if exists <extensionname>;`

### Extension Create
* `create extension if not exists <extensionname> with schema <tableschema>;`

### User Create
* `create user <userrolename> with password '<userrolepassword>';`

### Owner Reassign
* `reassign owned by <userrolename> to postgres;`

### Owner Drop
* `drop owned by <userrolename>;`

### User Drop
* `drop user <userrolename>;`

### Databases
* <pre>
  select
  datname as "Database Name"
  from pg_database;
  </pre>

### Database Drop
* `drop database if exists <databasename>;`

### Database Create
* `create database <databasename>;` **NOTE Does not have 'if exists' when creating databases**

### Database Grant Privileges
* `grant all privileges on database media to mediasql;`

### Database Alter Owner To Role
* `alter database <databasename> owner to <userrolename>;`

### Database Connect
* `\c <databasename>;`

### Tables
* <pre>
  select
  tablename as "Table Name"
  from pg_catalog.pg_tables
  where
  schemaname in ('&lt;tableschema&gt;');
  </pre>

### Table Drop
* `drop table if exists <tablename>;`

### Table Create
* <pre>
  -- Sequence Create
  create sequence &lt;tablename&gt;_&lt;tableID&gt;_seq;
  
  -- Table Create
  create table if not exists &lt;tablename&gt;(
    tableID bigint not null default nextval('&lt;tablename&gt;_&lt;tableID&gt;_seq'),
    columnOne int not null,
    columnTwo varchar(255) not null,
    columnThree text default null,
    columnFour bit(1) not null default b'0',
    columnFive timestamp not null default current_timestamp,
    columnSix timestamp default current_timestamp,
    constraint PK_&lt;tablename&gt;_&lt;columnOne&gt; primary key (columnOne)
  );
  
  -- Sequence Alter Ownership
  alter sequence &lt;tablename&gt;_&lt;tableID&gt;_seq; owned by &lt;tablename&gt;.&lt;tableID&gt;;
  
  -- Sequence Grant Permission
  grant usage, select on sequence &lt;tablename&gt;_&lt;tableID&gt;_seq to &lt;userrolename&gt;;
  </pre>

### Sequence Drop
* `drop sequence if exists <tablename>_<tableID>_seq;`

### Table Alter Owner
* `alter table <tablename> owner to <userrolename>;`

### Table Owner
* <pre>
  select
  tab.table_name as "table_name",
  tab.table_type as "table_type",
  pgc.relname as "relname",
  pgc.relowner as "relowner",
  pgu.usename as "usename"
  from information_schema.tables tab
  join pg_catalog.pg_class pgc on pgc.relname = tab.table_name
  join pg_catalog.pg_user pgu on pgu.usesysid = pgc.relowner
  where
  tab.table_schema in ('&lt;tableschema&gt;');
  </pre>

### Table Grant Privileges
* `grant all privileges on all tables in schema <table_schema> to <userrolename>;`

### Index Create
* `create index IX_<tablename>_<columnname> on <tablename> (<columnname>);`

### Index Show All In Schema
* <pre>
  select
  pgi.tablename as "tablename",
  pgi.indexname as "indexname",
  pgi.indexdef as "indexdef"
  from pg_indexes pgi
  where
  pgi.schemaname in ('&lt;tableschema&gt;')
  order by pgi.tablename, pgi.indexname;
  </pre>
  
### Indexes Show From A Table
* <pre>
  select
  pgi.indexname as "indexname",
  pgi.indexdef as "indexdef"
  from pg_indexes pgi
  where
  pgi.tablename in ('&lt;tablename&gt;');
  </pre>

### Index Drop
* `drop index if exists IX_<tablename>_<columname> on <tablename>;`

### Sequence Grant Privileges
* `grant all privileges on all sequences in schema <tableschema> to <userrolename>;`

### Access Future Objects In The Database Automatically
* `alter default privileges grant all on tables to <userrolename>;`

### Access Future Objects In The Schema Automatically
* `alter default privileges in schema <tableschema> grant all on tables to <userrolename>;`

### Table Columns
* <pre>
  select
  col.table_schema as "table_schema",
  col.table_name as "table_name",
  col.ordinal_position as "position",
  col.column_name as "column_name",
  col.data_type as "data_type",
  case
    when col.character_maximum_length is not null
      then
        col.character_maximum_length
    else
      col.numeric_precision
  end as "max_length",
  col.is_nullable as "is_nullable",
  col.column_default as "default_value"
  from information_schema.columns col
  where
  col.table_name in ('&lt;tablename&gt;') and
  col.table_schema in ('&lt;table_schema&gt;')
  order by col.table_name asc, col.ordinal_position asc;
  </pre>

### Sequnce Show Last Value
* <pre>
  select
  pgc.relname as "relname"
  from pg_class pgc
  where
  pgc.relkind = 'S';
  </pre>
* <pre>
  select
  last_value
  from &lt;tablename&gt;_&lt;tableID&gt;_seq;
  </pre>

### Table Select
* <pre>
  select
  tableID as "tableID",
  columnOne as "columnOne",
  columnTwo as "columnTwo",
  columnThree as "columnThree",
  columnFour as "columnFour",
  columnFive as "columnFive",
  columnSix as "columnSix"
  from &lt;tablename&gt;;
  </pre>

### Table Insert
* `insert into <tablename> (columnOne, columnTwo, columnThre, columnFour, columnFive, columnSix) values (0, 'columnTwo', 'columnThree', 1, current_timestamp, current_timestamp);`

### Table Update
* <pre>
  update &lt;tablename&gt;
  set
  columnFour = 1,
  columnSix = current_timestamp
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

### Table Truncate And Reseed Identity
* **NOTE Need to be owner user of the table**
* `truncate table <tablename> restart identity;`

### Functions
* <pre>
  select
  rou.specific_schema as "procedure_schema",
  rou.routine_name as "procedure_name",
  rou.routine_type as "kind",
  rou.external_language as "external_language",
  par.ordinal_position as "position",
  par.parameter_name as "paramter_name",
  par.parameter_mode as "parameter_mode",
  par.data_type as "data_type"
  from information_schema.routines rou
  left join information_schema.parameters par on par.specific_schema = rou.specific_schema and par.specific_name = rou.specific_name
  where
  rou.routine_schema not in ('pg_catalog', 'information_schema') and
  rou.routine_type in ('FUNCTION')
  order by rou.specific_schema asc, rou.routine_name asc, par.ordinal_position asc;
  </pre>
  
### Function Drop
* `drop function if exists <functionname>;`

### Function Call
* `call <functionname> ('<parametervalue>', ...);`

### Procedures
* **NOTE Stored Procedures are new; Before it was Functions only**
* <pre>
  select
  rou.specific_schema as "procedure_schema",
  rou.routine_name as "procedure_name",
  rou.routine_type as "kind",
  rou.external_language as "external_language",
  par.ordinal_position as "position",
  par.parameter_name as "paramter_name",
  par.parameter_mode as "parameter_mode",
  par.data_type as "data_type"
  from information_schema.routines rou
  left join information_schema.parameters par on par.specific_schema = rou.specific_schema and par.specific_name = rou.specific_name
  where
  rou.routine_schema not in ('pg_catalog', 'information_schema') and
  rou.routine_type in ('PROCEDURE')
  order by rou.specific_schema asc, rou.routine_name asc, par.ordinal_position asc;
  </pre>

### Procedure Alter Owner To Role
* `alter procedure <procedurename> owner to <userrolename>;`

### Procedure Drop
* `drop procedure if exists <procedurename>;`

### Procedure Call
* `call <procedurename> ('<parametervalue>', ...);`
