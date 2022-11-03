```
# create database
-- CREATE SCHEMA IF NOT EXISTS cldw;
# drop database
-- DROP DATABASE IF EXISTS cldw CASCADE;

```

```
date_format(current_date,'yyyy-MM-01')
```


```
# GREATEST
with data as (
select NUll a , 1 b
union all
select 1 a, 2 b
union all 
select 2 a , 3 b
)

select a, b, greatest(a, b) c from data
order by a is null, a desc
```


```
regexp_extract(session, '([^\-]+)$', 1)
```


```
# Key words
, concat_ws("|", sort_array(collect_set(FromDate))) From_Date
, size(collect_set(FromDate)) size
```

```
# string to date
TO_DATE(from_unixtime(unix_timestamp(CAST(OnsetDateKey AS STRING) ,  'yyyyMMdd')))

```


```
# Hive: Long to Wide
select YEAR_LAST_SEEN, sum(group_map['key1']) Key01, sum(group_map['key2']) Key02
from
( 
        select YEAR_LAST_SEEN, 
        map(practice ,  UNIQUE_PATIENT) as group_map 
        from dat
 ) a 
 group by a.YEAR_LAST_SEEN
 order by year_last_seen
```


# https://stackoverflow.com/questions/41758949/difference-between-translate-and-regexp-replace
```
spark.sql("SELECT TRANSLATE('ed-ba', 'abcde', '12345')").show()

```


## [restore a delta table to an ealier state](https://docs.databricks.com/delta/delta-utility.html#restore-a-delta-table-to-an-earlier-state)
```
RESTORE TABLE db.target_table TO VERSION AS OF <version>
RESTORE TABLE delta.`/data/target/` TO TIMESTAMP AS OF <timestamp>
```

```
select * from table_name version as of 0
select * from table_name TIMESTAMP AS OF <timestamp>
```


## detlta table

- [delta-batch](https://docs.microsoft.com/en-us/azure/databricks/delta/delta-batch)

## [External table]()
```
CREATE EXTERNAL TABLE IF NOT EXISTS mydb.contacts (
  name         STRING ,
  -- ... other variables
  city         STRING ,
LOCATION '/user/hive/warehouse/mydb.db/contacts';
```
- An optional path to the directory where table data is stored, which could be a path on distributed storage. path must be a STRING literal. If you specify no location the table is considered a managed table and Azure Databricks creates a default table location. Specifying a location makes the table an external table. [ref to microsoft](https://docs.microsoft.com/en-us/azure/databricks/sql/language-manual/sql-ref-syntax-ddl-create-table-using)

```
-- Creates a Delta table
> CREATE TABLE student (id INT, name STRING, age INT);

-- Use data from another table
> CREATE TABLE student_copy AS SELECT * FROM student;

-- Creates a CSV table from an external directory
> CREATE TABLE student USING CSV LOCATION '/mnt/csv_files';

-- Specify table comment and properties
> CREATE TABLE student (id INT, name STRING, age INT)
    COMMENT 'this is a comment'
    TBLPROPERTIES ('foo'='bar');

-- Specify table comment and properties with different clauses order
> CREATE TABLE student (id INT, name STRING, age INT)
    TBLPROPERTIES ('foo'='bar')
    COMMENT 'this is a comment';

-- Create partitioned table
> CREATE TABLE student (id INT, name STRING, age INT)
    PARTITIONED BY (age);

-- Create a table with a generated column
> CREATE TABLE rectangles(a INT, b INT,
                          area INT GENERATED ALWAYS AS (a * b));
```

## [UPDATE & MERGE](https://stackoverflow.com/questions/69206470/equivalent-of-merge-in-hive-query)

```
-- Perform shallow clone
CREATE OR REPLACE TABLE my_test SHALLOW CLONE my_prod_table;

UPDATE my_test WHERE user_id is null SET invalid=true;
-- Run a bunch of validations. Once happy:

-- This should leverage the update information in the clone to prune to only
-- changed files in the clone if possible
MERGE INTO my_prod_table
USING my_test
ON my_test.user_id <=> my_prod_table.user_id
WHEN MATCHED AND my_test.user_id is null THEN UPDATE *;

--
DROP TABLE my_test;
```

```
MERGE INTO db.target_table t
USING db.source_upload s
ON t.id = s.id
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *
```

- If you specify *, this updates or inserts all columns in the target table. This assumes that the source table has the same columns as those in the target table, otherwise the query will throw an analysis error.

Ref: 
- [cwiki.apache.org: Merge](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML#LanguageManualDML-Merge)
- [delta: quick start](https://docs.microsoft.com/en-us/azure/databricks/delta/quick-start)


## [Azure delta lake quick start with python](https://docs.microsoft.com/en-us/azure/databricks/_static/notebooks/delta/quickstart-python.html)

# Regexp 
> select regexp_extract('foo|the|bar', '(foo)\\|(.*?).(bar)', 1) 

# [conver array to string with group by](https://stackoverflow.com/questions/38711201/how-can-i-convert-array-to-string-in-hive-sql)
```
select 
  actor, 
  concat_ws(',',collect_set(date)) as grpdate 
from actor_table 
group by actor;
```

## [Apacke Hive UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-CreatingCustomUDFs)
## [pyspark udf decorate](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.functions.udf.html)

```
from pyspark.sql.types import IntegerType
slen = udf(lambda s: len(s), IntegerType())
@udf
def to_upper(s):
    if s is not None:
        return s.upper()

@udf(returnType=IntegerType())
def add_one(x):
    if x is not None:
        return x + 1

df = spark.createDataFrame([(1, "John Doe", 21)], ("id", "name", "age"))
df.select(slen("name").alias("slen(name)"), to_upper("name"), add_one("age")).show()
```


```
https://docs.microsoft.com/en-us/azure/databricks/scenarios/connect-databricks-excel-python-r
```




```
-- string manipulation: regexp_extract and split and explode  

%sql
WITH dat AS (
SELECT '123456789 10' AS c1
UNION ALL
SELECT '12345678 9 10' AS c1
UNION ALL
SELECT '12345 678 9 10' AS c1
)
SELECT  
  explode(split(c1, ' ')) as col, 
  regexp_extract(c1,'(\\d)+ ', 0) c1, 
  split(c1, ' ')[0] c2 
FROM dat
``` 

[TO_DATE: DATETIME-PATTERN](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html)

[spark-sql/udf-python](https://docs.databricks.com/spark/latest/spark-sql/udf-python.html)  
```
# UDF Example
# define function in python
from pyspark.sql.types import LongType
def squared_typed(s):
  return s * s
# register the function
spark.udf.register("squaredWithPython", squared_typed, LongType())
# create an example dataset
spark.range(1, 20).createOrReplaceTempView("test")
# test the function
%sql select id, squaredWithPython(id) as id_squared from test
```
