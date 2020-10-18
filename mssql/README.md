# MSSQL Database Guide
> MSSQL Database Guide

## Table of Contents
* [Version](#version)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Database Identity Cache](#database-identity-cache)
* [Tables](#tables)

### Version
* 0.0.1

### Databases
* <pre>
  select
  [name] as [Database Name]
  from sys.databases
  </pre>
  
### Database Drop
* <pre>
  if db_id (N'&lt;Databasename&gt;') is not null
    begin
      drop database N'Databasename'
    end
  </pre>

### Database Create
* `create database [Databasename];`
  
### Database Connect
* `use [Databasename]`

### Database Identity Cache
* `alter database scoped configuration set identity_cache = on`<br />
  `alter database scoped configuration set identity_cache = off`
  
### Tables
* <pre>
  select
  table_catalog as [table_catalog],
  table_schema as [table_schema],
  table_name as [table_name],
  table_type as [table_type]
  from &lt;Databasename&gt;.information_schema.tables
  where
  table_type = 'BASE TABLE'
  </pre>

### Table Drop
* <pre>
  if exists
  (
    select
    table_catalog as [table_catalog],
    table_schema as [table_schema],
    table_name as [table_name],
    table_type as [table_type]
    from &lt;Databasename&gt;.information_schema.tables
    where
    table_name = '&lt;Tablename&gt;'
  )
    begin
      drop table &lt;table_schema&gt;.&lt;Tablename&gt;
    end
  </pre>

