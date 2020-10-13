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
* `\l`

### Database Drop
* `drop database if exists <databasename>;`

### Database Create
* `create database <databasename>;`
  
### Database Connect
* `\c <databasename>;`

### Tables
* `select datname from pg_database;`
