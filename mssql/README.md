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
* `select`<br />
  `[name]`<br />
  `from sys.databases;`
  
### Database Drop
* `if db_id (N'Databasename') is not null`<br />
  `drop database N'Databasename'`

### Database Create
* `create database [Databasename];`
  
### Database Connect
* `use [Databasename];`

### Database Identity Cache
* `alter database scoped configuration set identity_cache = on;`<br />
  `alter database scoped configuration set identity_cache = off;`
  
### Tables
* `select`<br />
  `*`<br />
  `from media.information_schema.tables`<br />
  `where`<br />
  `table_type = 'BASE TABLE';`
