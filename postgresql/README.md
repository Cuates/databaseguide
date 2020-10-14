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
* `select datname from pg_database;`

### Database Drop
* `drop database if exists <databasename>;`

### Database Create
* `create database <databasename>;`
  
### Database Connect
* `\c <databasename>;`

### Tables
* `select`<br />
  `*`<br />
  `from pg_catalog.pg_tables`<br />
  `where`<br />
  `schemaname = 'public';`
