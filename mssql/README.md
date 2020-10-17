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
    [name]
    from sys.databases;
  </pre>
  
### Database Drop
* <pre>
    if db_id (N'Databasename') is not null
      drop database N'Databasename'
  </pre>

### Database Create
* `create database [Databasename];`
  
### Database Connect
* `use [Databasename];`

### Database Identity Cache
* `alter database scoped configuration set identity_cache = on;`<br />
  `alter database scoped configuration set identity_cache = off;`
  
### Tables
* <pre>
    select
    *
    from media.information_schema.tables
    where
    table_type = 'BASE TABLE';
  </pre>
