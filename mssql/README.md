# MSSQL Database Guide
> MSSQL Database Guide

## Table of Contents
* [Version](#version)
* [Databases](#databases)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Database Identity Cache](#database-identity-cache)
* [Tables](#tables)

### Version
* 0.0.1

### Databases
* `select`<br />
  `[name]`<br />
  `from sys.databases`

### Database Create
* `-- Database Create`<br />
  `use [master]`<br />
  `go`<br />
  `-- Database Check Existence`<br />
  `if db_id (N'Databasename') is not null`<br />
  `drop database N'Databasename'`<br />
  `go`<br />
  `create database [Databasename]`<br />
  `containment = none`<br />
  `on primary`<br />
  `(`<br />
  `name = N'Databasename',`<br />
  `filename = N'\path\to\mssql\data\folder',`<br />
  `size = #####KB,`<br />
  `maxsize = unlimited,`<br />
  `filegrowth = #####KB`<br />
  `)`<br />
  `log on`<br />
  `(`<br />
  `name = N'Databasename_log',`<br />
  `filename = N'\path\to\mssql\data\folder',`<br />
  `size = ######KB,`<br />
  `maxsize = ####GB,`<br />
  `filegrowth = #####KB`<br />
  `)`<br />
  `with catalog_collation = database_default`<br />
  `go`
  
### Database Connect
* `use [Databasename]`

### Database Identity Cache
* `alter database scoped configuration set identity_cache = on`<br />
  `alter database scoped configuration set identity_cache = off`
  
### Tables
* `select`<br />
  `*`<br />
  `from media.information_schema.tables`<br />
  `where`<br />
  `table_type = 'BASE TABLE';`
