# MSSQL Database Guide
> MSSQL Database Guide

[SQL Server Identity Jumping 1000 Identity Cache](https://blog.sqlauthority.com/2018/01/24/sql-server-identity-jumping-1000-identity_cache/)<br />
[SQL Server List Tables How To Show All Tables](https://chartio.com/resources/tutorials/sql-server-list-tables-how-to-show-all-tables/)<br />
[List All Indexes In The Database](https://dataedo.com/kb/query/sql-server/list-all-indexes-in-the-database)<br />
[List Columns Names In Specific Table](https://dataedo.com/kb/query/sql-server/list-columns-names-in-specific-table)<br />
[List Of Tables By The Number Of Rows](https://dataedo.com/kb/query/sql-server/list-of-tables-by-the-number-of-rows)<br />
[List Of Tables By Their Size](https://dataedo.com/kb/query/sql-server/list-of-tables-by-their-size)<br />
[List Stored Procedures](https://dataedo.com/kb/query/sql-server/list-stored-procedures)<br />
[Tip SP Validatelogins And SP Change Users Login](https://dbtut.com/index.php/2018/12/26/tip-sp_validatelogins-and-sp_change_users_login/)<br />
[How To Create A Linked Server To Connect To PostgreSQL From SQL Server](https://dbtut.com/index.php/2019/10/22/how-to-create-a-linked-server-to-connect-to-postgresql-from-sql-server/)<br />
[View A List Of Databases On An Instance Of SQL Server](https://docs.microsoft.com/en-us/sql/relational-databases/databases/view-a-list-of-databases-on-an-instance-of-sql-server?view=sql-server-ver15)<br />
[Create Linked Servers SQL Server Database Engine](https://docs.microsoft.com/en-us/sql/relational-databases/linked-servers/create-linked-servers-sql-server-database-engine?view=sql-server-ver15)<br />
[Create Database Transact SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-database-transact-sql?view=sql-server-ver15&tabs=sqlpool)<br />
[Create Table Transact SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-table-transact-sql?view=sql-server-ver15)<br />
[Drop Index Transact SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-index-transact-sql?view=sql-server-ver15)<br />
[Drop Procedure Transact SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-procedure-transact-sql?view=sql-server-ver15)<br />
[Drop Table Transact SQL](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-table-transact-sql?view=sql-server-ver15)<br />
[MSSQL MySQL Linked Server](https://gunnarpeipman.com/mssql-mysql-linked-server/)<br />
[Creating A Linked Server With A Postgres Database](https://peter-whyte.com/creating-a-linked-server-with-a-postgres-database/)<br />
[TSQL Remove User From All Databases](https://sqlsolace.blogspot.com/2009/07/tsql-remove-user-from-all-databases.html)<br />
[TSQL Script Current User Database Permissions](https://stackoverflow.com/questions/15386692/t-sql-script-current-user-database-permissions)<br />
[Does A Drop Table Also Drop The Constraints](https://stackoverflow.com/questions/43488105/does-a-drop-table-also-drop-the-constraints)<br />
[How To Drop A Table If It Exists](https://stackoverflow.com/questions/7887011/how-to-drop-a-table-if-it-exists)<br />
[SQL Server Create User](https://www.guru99.com/sql-server-create-user.html)<br />
[SQL Server And PostgreSQL Linked Server Configuration Part 2](https://www.mssqltips.com/sqlservertip/3662/sql-server-and-postgresql-linked-server-configuration--part-2/)<br />
[Create A Linked Server To MySQL From SQL Server](https://www.mssqltips.com/sqlservertip/4577/create-a-linked-server-to-mysql-from-sql-server/)<br />
[Examples Using The Sys Schema In SQL Server](https://www.roundthecode.com/sql-server/examples-using-the-sys-schema-in-sql-server)<br />
[The Identity Cache Option In SQL Server](https://www.sqlnethub.com/blog/the-identity-cache-option-in-sql-server/)<br />
[SQL Server Create Database](https://www.sqlservertutorial.net/sql-server-basics/sql-server-create-database/)<br />
[Configure ODBC Drivers For MySQL](https://www.sqlshack.com/configure-odbc-drivers-for-mysql/)<br />
[Enable Disable Identity Cache SQL Server 2017](https://www.sqlshack.com/enable-disable-identity-cache-sql-server-2017/)

## Table of Contents
* [Version](#version)
* [User Create](#user-create)
* [User Orphaned](#user-orphaned)
* [User Drop](#user-drop)
* [Databases](#databases)
* [Database Drop](#database-drop)
* [Database Create](#database-create)
* [Database Connect](#database-connect)
* [Database Identity Cache](#database-identity-cache)
* [Tables](#tables)
* [Table Drop](#table-drop)
* [Table Create](#table-create)
* [Table Columns](#table-columns)
* [Index Create](#index-create)
* [Index Drop](#index-drop)
* [Table Select](#table-select)
* [Table Insert](#table-insert)
* [Table Update](#table-update)
* [Table Delete](#table-delete)
* [Table Truncate](#table-truncate)
* [Functions](#functions)
* [Function Drop](#function-drop)
* [Function Execute](#function-execute)
* [Procedures](#procedures)
* [Procedure Drop](#procedure-drop)
* [Procedure Execute](#procedure-execute)
* [Delete User From Database Issue](#delete-user-from-database-issue)
* [Check New User Permission](#check-new-user-permission)
* [Import](#import)

### Version
* 0.0.1

### User Create
* `use [<databasename>];`
* `create login [<loginname>] with password=N'<userpassword>', default_database=[<databasename>], default_language=[us_english], check_expiration=off, check_policy=on;`
* `create user [<username>] for login <loginname>;`
* `grant <permissionname> on [<databasename>] to <username>;`
  * <pre>
    select
    entity_name,
    subentity_name,
    permission_name
    from fn_my_permissions('&lt;Databasename&gt;', 'Database')
    order by subentity_name asc, permission_name asc;
    </pre>

### User Orphaned
* `alter user <username> with login = <loginname>;`

### User Drop
* <pre>
  if exists
  (
    select
    ssp.[name] as [name]
    from sys.server_principals ssp
    where
    ssp.[name] = N'&lt;username&gt;'
  )
    begin
      drop login [&lt;username&gt;];
    end;
  </pre>

### Databases
* <pre>
  select
  sd.[name] as [Database Name]
  from sys.databases sd;
  </pre>
  
### Database Drop
* <pre>
  if db_id (N'&lt;Databasename&gt;') is not null
    begin
      drop database [&lt;Databasename&gt;];
    end;
  </pre>

### Database Create
* `create database [Databasename];`
  
### Database Connect
* `use [Databasename];`

### Database Identity Cache
* `alter database scoped configuration set identity_cache = on; -- Pre allocate record ids`<br />
  `alter database scoped configuration set identity_cache = off; -- Do not pre allocate record ids`
  
### Tables
* <pre>
  select
  sch.[name] as [table_schema],
  tab.[name] as [table_name]
  from sys.tables tab
  inner join sys.schemas sch on sch.schema_id = tab.schema_id
  where
  tab.[type] in ('U')
  order by sch.[name] asc, tab.[name] asc;
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
    end;
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
  order by tab.[name] asc, col.column_id asc;
  </pre>

### Index Create
* <pre>
  create nonclustered index [IX_&lt;Tablename&gt;_&lt;columnname&gt;] on [&lt;tableschema&gt;].[&lt;Tablename&gt;]
  (
    [&lt;columnname&gt;] asc
  )with (pad_index = off, statistics_norecompute = off, sort_in_tempdb = off, drop_existing = off, online = off, allow_row_locks = on, allow_page_locks = on, optimize_for_sequential_key = off) on [primary];
  </pre>

### Index Drop
* `drop index if exists IX_<Tablename>_<columnname> on <tableschema>.<Tablename>;`

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
  from &lt;table_schema&gt;.&lt;Tablename&gt;;
  </pre>

### Table Insert
* `insert into <table_schema>;.<Tablename> (columnOne, columnTwo, columnThre, columnFour, columnFive, columnSix) values (0, 'columnTwo', 'columnThree', 1, getdate(), getdate());`

### Table Update
* <pre>
  update &lt;table_schema&gt;.&lt;Tablename&gt;
  set
  columnFour = 1,
  columnSix = getdate()
  where
  columnFour = 0;
  </pre>

### Table Delete
* <pre>
  delete from &lt;table_schema&gt;.&lt;Tablename&gt;
  where
  tableID = 1;
  </pre>

### Table Truncate
* `truncate table <table_schema>.<Tablename>;`

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

### Function Drop
* `drop procedure if exists <tableschema>.<functionname>;`

### Function Execute
* `exec <tableschema>.<functionname> (parameterone, ...);`

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

### Procedure Drop
* `drop procedure if exists <tableschema>.<procedurename>;`

### Procedure Execute
* `exec <tableschema>.<procedurename> @parameterone = '', ...;`

### Delete User From Database Issue
* If you have issues with deleting a user from the database then proceed with the following
  * Issue with not able to delete user because they own a database
    * Check what the user has ownership to
      * <pre>
          select
          s.name
          from sys.schemas s
          where s.principal_id = user_id('user_name');
        </pre>
    * Assign dbo to owner of the database again
      * <pre>
          ALTER AUTHORIZATION ON SCHEMA::db_owner TO dbo;
          ALTER AUTHORIZATION ON SCHEMA::db_datareader TO dbo;
          ALTER AUTHORIZATION ON SCHEMA::db_datawriter TO dbo;
        </pre>

### Check New User Permission
* Check new user's permission to make sure they are able to access the newly created database.
  * Open Microsoft SQL Server Management Studio
      * Sign in as a root user for the SQL server
        * Expand the SQL server you just connected to
          * Expand Database under the server you just connected to
            * Expand Security under the database you expanded
              * Expand Users under the database you expanded
               * Right click on "Properties" for the newly created user
                 * Provide new user with the following
                   * Check
                     * db_datareader
                     * db_datawriter
                     * db_owner
                 * Adjust any other properties for the user that is needed
                 * Click button "OK"
        * Expand the SQL server you just connected to
          * Expand Security under the server you just connected to
            * Expand on "Logins"
              * Right click on "Properties" for the newly created user
                * Select the "User Mapping" tab
                  * Select the database of choice in the "Users mapped to this login" panel
                    * Make sure the user is inputted in the User section and the Default Schema is dbo
                  * Provide new user with the following in the "Database role membership for: " section
                   * Check
                     * db_datareader
                     * db_datawriter
                     * db_owner
                * Click button "OK"

### Import
* Import
  * The following is for a Linux MSSQL server
    * Open a terminal of choice
      * Sign into the linux server as root user
        * Copy the .bak file you created when you exported the database to the following location '/var/opt/mssql/data'
          * `cp ~/media.bak .`
        * Adjust the permissions to the mssql user, this will allow the mssql user to interact with the backup file
          * `chown mssql:mssql media.bak`
  * Open Microsoft SQL Server Management Studio on Windows or any operating system that will open the SSMS application
    * Sign in as a root user for the SQL server
      * Expand the SQL server you just connected to
        * Right click on "Databases"
            * Click on "Restore Files and Filegroups"
              * Under the General tab perform the necessary changes
                * Type the "To database" name
                * Select "From device" radio button
                  * Click on the 3 dots to the right of the radio button
                  * Make sure "Backup media type is "File"
                  * Click button "Add"
                  * Select the "data" folder in the tree structure
                  * Select the backup of choice
                    * Click button "OK"
                  * Click button "OK"
                * Check "Restore" of the backup you created earlier
                  * You see which one to check, look at the "Start Date" and "Finish Date" columns for the latest backup
              * Adjust anything else for any of the other tabs if needed
              * Click button "OK" when you are done with the modifications
              * Wait for the "Executing" to finish
                * This may take some time depending on how much data was backup
              * You will get a pop window stating
                * "Database 'database_name' restored successfully."
                  * Click button "OK"
        * Right click on "Databases"
          * Click "Refresh"
      * You have successfully imported your backup from your old SQL server
