# PostgreSQL Database Guide
> PostgreSQL Database Guide

## Table of Contents
* [Version](#version)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Tables](#tables)
* [Table Drop](#table-drop)
* [Table Create](#table-create)
* [Table Columns](#table-columns)
* [Table Select](#table-select)
* [Table Insert](#table-insert)
* [Table Update](#table-update)
* [Table Delete](#table-delete)
* [Table Truncate](#table-truncate)
* [Table Truncate And Reseed Identity](#table-truncate-and-reseed-identity)
* [Functions And Procedures](#functions-and-procedures)

### Version
* 0.0.1

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
  
### Database Connect
* `\c <databasename>;`

### Tables
* <pre>
  select
  tablename as "Table Name"
  from pg_catalog.pg_tables
  where
  schemaname = 'public';
  </pre>

### Table Drop
* `drop table if exists <tablename>;`

### Table Create
* <pre>
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
  </pre>

### Table Columns
* <pre>
  select
  *
  from information_schema.columns
  where
  table_name = '&lt;tablename&gt;';
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
* **NOTE Need to be user owner of the table**
* `truncate table <tablename> restart identity;`

### Functions and Procedures
* **NOTE Stored Procedures are new; Before it was Functions only**
* User Defined Functions Universal
  * <pre>
    select
    n.nspname as "function_schema",
    p.proname as "function_name",
    l.lanname as "function_language",
    case
      when l.lanname = 'internal'
        then
          p.prosrc
      else
        pg_get_functiondef(p.oid)
    end as "definition",
    pg_get_function_arguments(p.oid) as "function_arguments",
    t.typname as "return_type"
    from pg_proc p
    left join pg_namespace n on p.pronamespace = n.oid
    left join pg_language l on p.prolang = l.oid
    left join pg_type t on t.oid = p.prorettype
    where
    n.nspname not in ('pg_catalog', 'information_schema')
    order by function_schema, function_name;
    </pre>
* User Defined Functions 11+
  * <pre>
    select
    n.nspname as "schema_name",
    p.proname as "specific_name",
    case p.prokind
      when 'f'
        then
          'FUNCTION'
      when 'p'
        then
          'PROCEDURE'
      when 'a'
        then
          'AGGREGATE'
      when 'w'
        then
          'WINDOW'
    end as "kind",
    l.lanname as "language",
    case
      when l.lanname = 'internal'
        then
          p.prosrc
      else
        pg_get_functiondef(p.oid)
    end as "definition",
    pg_get_function_arguments(p.oid) as "arguments",
    t.typname as "return_type"
    from pg_proc p
    left join pg_namespace n on p.pronamespace = n.oid
    left join pg_language l on p.prolang = l.oid
    left join pg_type t on t.oid = p.prorettype
    where
    n.nspname not in ('pg_catalog', 'information_schema')
    order by schema_name, specific_name;
    </pre>
