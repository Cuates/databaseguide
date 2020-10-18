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
* [Table Drop](#table-drop)
* [Table Create](#table-create)
* [Table Columns](#table-columns)
* [Table Select](#table-select)
* [Table Insert](#table-insert)
* [Table Update](#table-update)
* [Table Delete](#table-delete)
* [Table Truncate](#table-truncate)
* [Functions](#functions)
* [Procedures](#procedures)

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
  sch.[name] as [table_schema],
  tab.[name] as [table_name]
  from sys.tables tab
  inner join sys.schemas sch on sch.schema_id = tab.schema_id
  where
  tab.[type] in ('U')
  order by sch.[name] asc, tab.[name] asc
  </pre>

### Table Drop
* <pre>
  if exists
  (
    select
    sch.[name] as [table_schema],
    tab.[name] as [table_name]
    from sys.tables tab
    inner join sys.schemas sch on sch.schema_id = tab.schema_id
    where
    tab.[type] in ('U') and
    tab.[name] = '&lt;Tablename&gt;'
    order by sch.[name] asc, tab.[name] asc
  )
    begin
      drop table &lt;table_schema&gt;.&lt;Tablename&gt;
    end
  </pre>

### Table Create
* <pre>
  create table [&lt;table_schema&gt;].[&lt;Tablename&gt;](
    [tableID] [bigint] identity(1, 1) not null,
    [columnOne] [int] not null,
    [columnTwo] [nvarchar](255) not null,
    [columnThree] [text] null,
    [columnFour] [bit] not null,
    [columnFive] [datetime2](6) not null,
    [columnSix] [datetime2](6) null
    constraint [PK_&lt;Tablename&gt;_columnOne] primary key clustered
    (
      [columnOne] asc
    ) with (pad_index = off, statistics_norecompute = off, ignore_dup_key = off, allow_row_locks = on, allow_page_locks = on, fillfactor = 90, optimize_for_sequential_key = off) on [primary]
  ) on [primary]
  go

  alter table [&lt;table_schema&gt;].[&lt;Tablename&gt;] add  default ((0)) for [columnFour]
  go

  alter table [&lt;table_schema&gt;].[&lt;Tablename&gt;] add  default (getdate()) for [columnFive]
  go

  alter table [&lt;table_schema&gt;].[&lt;Tablename&gt;] add  default (getdate()) for [columnSix]
  go
  </pre>

### Table Columns
* <pre>
  select
  sch.[name] as [schema_name],
  tab.[name] as [table_name],
  col.column_id as [id],
  col.[name] as [column_name],
  typ.[name] as [data_type],
  col.max_length as [max_length],
  col.precision as [precision],
  col.is_nullable as [is_nullable]
  from sys.tables as tab
  inner join sys.columns as col on col.object_id = tab.object_id
  left join sys.types as typ on col.user_type_id = typ.user_type_id
  inner join sys.schemas sch on sch.schema_id = tab.schema_id
  where
  tab.[name] in ('&lt;Tablename&gt;') and
  sch.[name] in ('&lt;table_schema&gt;')
  order by tab.[name] asc, col.column_id asc
  </pre>

### Table Select
* <pre>
  select
  tableID as [tableID],
  columnOne as [columnOne],
  columnTwo as [columnTwo],
  columnThree as [columnThree],
  columnFour as [columnFour],
  columnFive as [columnFive],
  columnSix as [columnSix]
  from &lt;table_schema&gt;.&lt;Tablename&gt;
  </pre>

### Table Insert
* `insert into <table_schema>;.<Tablename> (columnOne, columnTwo, columnThre, columnFour, columnFive, columnSix) values (0, 'columnTwo', 'columnThree', 1, getdate(), getdate())`

### Table Update
* <pre>
  update &lt;table_schema&gt;.&lt;Tablename&gt;
  set
  columnFour = 1,
  columnSix = getdate()
  where
  columnFour = 0
  </pre>

### Table Delete
* <pre>
  delete from &lt;table_schema&gt;.&lt;Tablename&gt;
  where
  tableID = 1
  </pre>

### Table Truncate
* `truncate table <table_schema>.<Tablename>`

### Functions
* <pre>
  select
  obj.type_desc as [type],
  schema_name(obj.schema_id) as [schema_name],
  obj.[name] as [object_name],
  substring(par.parameters, 0, len(par.parameters)) as [parameters],
  mod.definition as [definition]
  from sys.objects obj
  join sys.sql_modules mod on mod.object_id = obj.object_id
  cross apply
  (
    select
    p.[name] + ' ' + type_name(p.user_type_id) + ', ' 
    from sys.parameters p
    where
    p.object_id = obj.object_id and
    p.parameter_id <> 0 
    for xml path ('')
  ) par (parameters)
  where
  obj.type in ('FN')
  order by schema_name(obj.schema_id) asc, obj.[name] asc;
  </pre>

### Procedures
* <pre>
  select
  obj.type_desc as [type],
  schema_name(obj.schema_id) as [schema_name],
  obj.[name] as [object_name],
  substring(par.parameters, 0, len(par.parameters)) as [parameters],
  mod.definition as [definition]
  from sys.objects obj
  join sys.sql_modules mod on mod.object_id = obj.object_id
  cross apply
  (
    select
    p.[name] + ' ' + type_name(p.user_type_id) + ', ' 
    from sys.parameters p
    where
    p.object_id = obj.object_id and
    p.parameter_id <> 0 
    for xml path ('')
  ) par (parameters)
  where
  obj.type in ('P', 'X')
  order by schema_name(obj.schema_id) asc, obj.[name] asc;
  </pre>
