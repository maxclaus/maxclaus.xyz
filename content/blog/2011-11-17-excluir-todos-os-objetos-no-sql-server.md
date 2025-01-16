+++
title = "Excluir todos os Objetos no SQL Server"
+++

Este artigo é uma referência á um script que salvou tempo e serviço em um projeto atual. No qual precisávamos zerar o banco, mas com o detalhe que não poderíamos excluí-lo e criá-lo novamente.

Sendo assim o que este script faz é excluir todos os objetos nesta ordem:

1.  Stored Procedures
2.  Views
3.  Functions
4.  Foreign Keys
5.  Primary Keys
6.  Tables
7.  Indexes

```sql
-- Drop all non-system stored procs
DECLARE @name VARCHAR(128)
DECLARE @SQL VARCHAR(254)
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] = 'P' AND category = 0 ORDER BY \[name\])
WHILE @name is not null
BEGIN
SELECT @SQL = 'DROP PROCEDURE \[dbo\].\[' + RTRIM(@name) +'\]'
EXEC (@SQL)
PRINT 'Dropped Procedure: ' + @name
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] = 'P' AND category = 0 AND \[name\] > @name ORDER BY \[name\])
END
GO

-- Drop all views
DECLARE @name VARCHAR(128)
DECLARE @SQL VARCHAR(254)
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] = 'V' AND category = 0 ORDER BY \[name\])
WHILE @name IS NOT NULL
BEGIN
SELECT @SQL = 'DROP VIEW \[dbo\].\[' + RTRIM(@name) +'\]'
EXEC (@SQL)
PRINT 'Dropped View: ' + @name
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] = 'V' AND category = 0 AND \[name\] > @name ORDER BY \[name\])
END
GO

-- Drop all functions
DECLARE @name VARCHAR(128)
DECLARE @SQL VARCHAR(254)
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] IN (N'FN', N'IF', N'TF', N'FS', N'FT') AND category = 0 ORDER BY \[name\])
WHILE @name IS NOT NULL
BEGIN
SELECT @SQL = 'DROP FUNCTION \[dbo\].\[' + RTRIM(@name) +'\]'
EXEC (@SQL)
PRINT 'Dropped Function: ' + @name
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] IN (N'FN', N'IF', N'TF', N'FS', N'FT') AND category = 0 AND \[name\] > @name ORDER BY \[name\])
END
GO

-- Drop all Foreign Key constraints
DECLARE @name VARCHAR(128)
DECLARE @constraint VARCHAR(254)
DECLARE @SQL VARCHAR(254)
SELECT @name = (SELECT TOP 1 TABLE\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'FOREIGN KEY' ORDER BY TABLE\_NAME)
WHILE @name is not null
BEGIN
SELECT @constraint = (SELECT TOP 1 CONSTRAINT\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'FOREIGN KEY' AND TABLE\_NAME = @name ORDER BY CONSTRAINT\_NAME)
WHILE @constraint IS NOT NULL
BEGIN
SELECT @SQL = 'ALTER TABLE \[dbo\].\[' + RTRIM(@name) +'\] DROP CONSTRAINT ' + RTRIM(@constraint)
EXEC (@SQL)
PRINT 'Dropped FK Constraint: ' + @constraint + ' on ' + @name
SELECT @constraint = (SELECT TOP 1 CONSTRAINT\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'FOREIGN KEY' AND CONSTRAINT\_NAME <> @constraint AND TABLE\_NAME = @name ORDER BY CONSTRAINT\_NAME)
END
SELECT @name = (SELECT TOP 1 TABLE\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'FOREIGN KEY' ORDER BY TABLE\_NAME)
END
GO

-- Drop all Primary Key constraints
DECLARE @name VARCHAR(128)
DECLARE @constraint VARCHAR(254)
DECLARE @SQL VARCHAR(254)
SELECT @name = (SELECT TOP 1 TABLE\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'PRIMARY KEY' ORDER BY TABLE\_NAME)
WHILE @name IS NOT NULL
BEGIN
SELECT @constraint = (SELECT TOP 1 CONSTRAINT\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'PRIMARY KEY' AND TABLE\_NAME = @name ORDER BY CONSTRAINT\_NAME)
WHILE @constraint is not null
BEGIN
SELECT @SQL = 'ALTER TABLE \[dbo\].\[' + RTRIM(@name) +'\] DROP CONSTRAINT ' + RTRIM(@constraint)
EXEC (@SQL)
PRINT 'Dropped PK Constraint: ' + @constraint + ' on ' + @name
SELECT @constraint = (SELECT TOP 1 CONSTRAINT\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'PRIMARY KEY' AND CONSTRAINT\_NAME <> @constraint AND TABLE\_NAME = @name ORDER BY CONSTRAINT\_NAME)
END
SELECT @name = (SELECT TOP 1 TABLE\_NAME FROM INFORMATION\_SCHEMA.TABLE\_CONSTRAINTS WHERE constraint\_catalog=DB\_NAME() AND CONSTRAINT\_TYPE = 'PRIMARY KEY' ORDER BY TABLE\_NAME)
END
GO

-- Drop all tables
DECLARE @name VARCHAR(128)
DECLARE @SQL VARCHAR(254)
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] = 'U' AND category = 0 ORDER BY \[name\])
WHILE @name IS NOT NULL
BEGIN
SELECT @SQL = 'DROP TABLE \[dbo\].\[' + RTRIM(@name) +'\]'
EXEC (@SQL)
PRINT 'Dropped Table: ' + @name
SELECT @name = (SELECT TOP 1 \[name\] FROM sysobjects WHERE \[type\] = 'U' AND category = 0 AND \[name\] > @name ORDER BY \[name\])
END
GO

-- Drop all indexes
declare @RETURN\_VALUE int
declare @command1 nvarchar(2000)
set @command1 = 'DECLARE @indexName NVARCHAR(128)'
set @command1 = @command1 + ' DECLARE @dropIndexSql NVARCHAR(4000)'
set @command1 = @command1 + ' DECLARE tableIndexes CURSOR FAST\_FORWARD FOR'
set @command1 = @command1 + ' SELECT name FROM sys.indexes'
set @command1 = @command1 + ' WHERE object\_id = OBJECT\_ID(''?'') AND index\_id > 0 AND index\_id < 255 AND is\_primary\_key = 0'
set @command1 = @command1 + ' ORDER BY index\_id DESC'
set @command1 = @command1 + ' OPEN tableIndexes FETCH NEXT FROM tableIndexes INTO @indexName'
set @command1 = @command1 + ' WHILE @@fetch\_status = 0'
set @command1 = @command1 + ' BEGIN'
set @command1 = @command1 + ' SET @dropIndexSql = N''DROP INDEX ?.\['' + @indexName + ''\]'''
set @command1 = @command1 + ' EXEC sp\_executesql @dropIndexSql'
set @command1 = @command1 + ' print @dropIndexSql'
set @command1 = @command1 + ' FETCH NEXT FROM tableIndexes INTO @indexName'
set @command1 = @command1 + ' END'
set @command1 = @command1 + ' CLOSE tableIndexes'
set @command1 = @command1 + ' DEALLOCATE tableIndexes'
Print '-----------------------------------------'
exec @RETURN\_VALUE = sp\_MSforeachtable @command1=@command1
GO
```

Segue o link para o post, no qual encontrei este script: [http://paigecsharp.blogspot.com/2008/03/drop-all-objects-in-sql-server-database.html](http://paigecsharp.blogspot.com/2008/03/drop-all-objects-in-sql-server-database.html)

