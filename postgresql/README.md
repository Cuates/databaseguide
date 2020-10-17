# PostgreSQL Database Guide
> PostgreSQL Database Guide

## Table of Contents
* [Version](#version)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Tables](#tables)

### Version
* 0.0.1

### Databases
* <pre>
    select
    datname
    from pg_database;
  </pre>

### Database Drop
* `drop database if exists <databasename>;`

### Database Create
* `create database <databasename>;`
  
### Database Connect
* `\c <databasename>;` **NOTE Does not have 'if exists' when creating databases**

### Tables
* <pre>
    select
    tablename
    from pg_catalog.pg_tables
    where
    schemaname = 'public';
  </pre>
