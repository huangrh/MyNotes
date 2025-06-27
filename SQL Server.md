# ODBC Data Source Administrator (64-bit)
- How to Configure ODBC to Access a Microsoft SQL Server: https://www.youtube.com/watch?v=tUiaK5fRH7k
- ODBC

# SQL Server 
```
-- In SQL Server, you can use the following query to list all the databases:
SELECT name FROM master.sys.databases;
```

# REGEXP_LIKE (Transact-SQL)
```
SELECT *
FROM PRODUCTS
WHERE REGEXP_LIKE (PRODUCT_NAME, '[AEIOU]{3,}');   
```
- https://learn.microsoft.com/en-us/sql/t-sql/functions/regexp-like-transact-sql?view=sql-server-ver17
