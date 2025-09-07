# unity catalog

```
workspace_id = dbutils.notebook.entry_point.getDbutils().notebook().getContext().workspaceId().get()
print(workspace_id)
```

```
# list table
path = "abfss://container_name@storage_account_name.dfs.core.windows.net/"
dbutils.fs.ls(path) 
```

```
# list all table in a db
SHOW TABLES IN hive_metastore.db_name
# show the location 
DESCRIBE DETAIL hive_metastore.db_name.table_name
# Drop table
query = f"DROP TABLE IF EXISTS {uc_name}.{db_name}.{table}"
# Create UC table
CREATE TABLE IF NOT EXISTS uc_catalog.db_name.table_name
USING DELTA 
LOCATION 'abfss:/container_name@storage_account_name.dfs.core.windows.net/db_name/table_name';
```

# What is the difference between ABFSS and WASBS in Azure storage?
- ABFSS
- WASBS
https://stackoverflow.com/questions/60277545/what-is-the-difference-between-abfss-and-wasbs-in-azure-storage

# pandas api on pyspark  

- https://sparkbyexamples.com/pyspark/pandas-api-on-apache-spark-pyspark/
- 
```
from pyspark import pandas as pd
```

# https://stackoverflow.com/questions/34077353/how-to-change-dataframe-column-names-in-pyspark  
```
df = df.select(col("colName").alias("col_newname"), col("col2").alias("col2_newname"))
```

# https://stackoverflow.com/questions/31385363/how-to-export-a-table-dataframe-in-pyspark-to-csv
- sdf.repartition(1).write.csv("/path/to/csv/data.csv")


# https://www.databricks.com/sites/default/files/2024-11/delta-lake-the-definitive-guide-by-o-reilly.pdf  
# 
# Cosmosdb
- https://github.com/Azure/azure-cosmosdb-spark    
- https://learn.microsoft.com/en-us/azure/databricks/scenarios/service-endpoint-cosmosdb  
# Hive data type, cast
- https://learn.microsoft.com/en-us/azure/databricks/sql/language-manual/sql-ref-datatypes
- 
# https://stackoverflow.com/questions/41944689/pyspark-parse-fixed-width-text-file

```
x= '''    1 123121234 joe
    2 234234234jill
    3 345345345jane
    4abcde12345jack'''

schema = [
          ("id",1,5),
          ("ssn",6,10),
          ("name",16,4)
]
          
with open("personfixed.csv", "w") as f:
  f.write(x)

df = spark.read.text("personfixed.csv")
df.show()

df2 = df
for colinfo in schema:
  df2 = df2.withColumn(colinfo[0], df2.value.substr(colinfo[1],colinfo[2]))

df2.show()

```

# Run Notebook and pass parameters    
- https://docs.databricks.com/en/dev-tools/databricks-utils.html#widgets-utility-dbutilswidgets
- 
```
# main notebook to run 2nd notebook and pass parameter to 2nd paramenters
dbutils.notebook.run("path/to/2nd notebook",{"Data_Source_Group":"All Data Source"})
# 2nd notebook: retrive parameters
dbutils.widgets.text(name="Data_Source_Group", defaultValue="All Data Source", label="Enter Data Source Group")
Data_Source_Group_Name = dbutils.widgets.get("Data_Source_Group")
print(Data_Source_Group_Name)
```


# JSON Path
- https://goessner.net/articles/JsonPath/index.html#e2
- https://jsonpath.com/
- https://medium.com/@uzzaman.ahmed/introduction-to-pyspark-json-api-read-and-write-with-parameters-3cca3490e448
```
# https://stackoverflow.com/questions/54210389/is-there-a-generic-way-to-read-the-multiline-json-in-spark-more-specifically-sp
#Create data frame to read files from a folder
from pyspark.sql.functions import explode, col
dfReadFHIR_raw = spark.read.option("multiline", "true").json(filePath)
val df         = spark.read.option("multiline", "true").json("multi.json")
val df2= df.withColumn("scores",explode(col("scores"))).select(col("_id.*"), col("class_id"),col("scores.*"),col("student_id"))
```

```
You can provide the `skipRows` option while reading.
> spark.read.format("csv").option("skipRows", 4).load("<filepath>")
```

```
df = spark.read.csv(path, header=True, inferSchema=True, sep="|")
```

- [delta-batch](https://docs.microsoft.com/en-us/azure/databricks/delta/delta-batch)

```
-- load multiple csv file
from functools import reduce  
from pyspark.sql import DataFrame  
def load_csv(file):   
    import pyspark.sql.functions as F  
    batch = file.name.split("_")[-1].split(".")[0]  
    df = spark.read.format("csv").\  
            option("Header", "true").\  
            option("delimiter", "|").\  
            option("multipleLines", "true").\  
            option("quote", "'").\  
            option("escape", "\\").\  
            load(file.path).\  
            withColumn("batch",F.lit(batch))  
    
    return df  
df = reduce(DataFrame.unionByName, [load_csv(file) for file in files])  
```

# Hash  
- https://docs.databricks.com/en/sql/language-manual/functions/sha2.html
- 
# Create function    
- https://learn.microsoft.com/en-us/azure/databricks/sql/language-manual/sql-ref-syntax-ddl-create-sql-function
- https://learn.microsoft.com/en-us/azure/databricks/sql/language-manual/sql-ref-syntax-ddl-create-sql-function
```
—- Hello World-like functionality using Python UDFs
> CREATE FUNCTION main.default.greet(s STRING)
  RETURNS STRING
  LANGUAGE PYTHON
  AS $$
    def greet(name):
      return "Hello " + name + "!"

    return greet(s) if s else None
  $$

—- Can import functions from std library and environment
> CREATE FUNCTION main.default.isleapyear(year INT)
  RETURNS BOOLEAN
  LANGUAGE PYTHON
  AS $$
    import calendar
    return calendar.isleap(year) if year else None
  $$

—- Must return the correct type. Otherwise will fail at runtime.
> CREATE FUNCTION main.default.a_number()
  RETURNS INTEGER
  LANGUAGE PYTHON
  AS $$
    # does not work: return "10"
    # does not work: return 3.14
    return 10
  $$

—- Deal with exceptions.
> CREATE FUNCTION main.default.custom_divide(n1 INT, n2 INT)
  RETURNS FLOAT
  LANGUAGE PYTHON
  AS $$
    try:
      return n1/n2
    except ZeroDivisionException:
    # in case of 0, we can return NULL.
    return None
  $$
```
- 


# https://stackoverflow.com/questions/64380125/how-to-optimize-the-pyspark-topandas-with-type-hints
```
from pyspark.sql.types import FloatType
from pyspark.sql.functions import col

# find all decimal columns in your SparkDF
decimals_cols = [c for c in df.columns if 'Decimal' in str(df.schema[c].dataType)]

# convert all decimals columns to floats
for col in decimals_cols:
    df = df.withColumn(col, df[col].cast(FloatType()))

#Now you can easily convert Spark DF to Pandas DF without decimal errors
pandas_df = df.toPandas() 

#sometimes if you have spaces in the column names it will convert to lowercase 
pandas_df.columns = pandas_df.columns.str.upper()
```

```
withColumn('source', coalesce(col('source'),(lit(source))))
```

```
# create database
-- CREATE SCHEMA IF NOT EXISTS cldw;
# drop database
-- DROP DATABASE IF EXISTS path_to.cldw CASCADE;

```

# Table manipulation  
```
# Renaming Table Name
ALTER TABLE <current_table_name> 
RENAME TO <new_table_name>;

# Add column
ALTER TABLE <table_name> 
ADD COLUMNS (<col-name>  <data-type>  COMMENT ”, <col-name>  <data-type>  COMMENT ”, ….. )
```
ALTER TABLE HIE.EVENT_FACT
ADD COLUMNS (
        Clo1 int,
        Col2 string,
        col3 timestamp
)
```
# Example : add contact
ALTER TABLE customer 
ADD COLUMNS ( contact BIGINT COMMENT ‘Store the customer contact number’);

# Change Column
ALTER TABLE <table_name> 
CHANGE <column_name> <new_column_name> <new_data_type>;
# Example
ALTER TABLE customer 
CHANGE demo_name customer_name STRING; 

# Change column  
# example: For example in our customer table, we have 2 attributes customer_name and contact. 
# If we want to remove the contact attribute the query should be like as shown below.
ALTER TABLE customer 
REPLACE COLUMNS (
customer_name STRING
);
```

# Truncate table
```
ALTER TABLE mytable SET TBLPROPERTIES ('external.table.purge'='true');
truncate table abc;
```



## [External table]()

```
query = f"""
CREATE TABLE IF NOT EXISTS {zone.lower()}.{external_table} 
USING DELTA 
LOCATION '{target_file}';
"""
print(query) 
spark.sql(query).display()
```

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

# 1. Date manipulation 

- [date function example](https://sparkbyexamples.com/apache-hive/hive-date-and-timestamp-functions-examples/#:~:text=Hive%20Date%20and%20Timestamp%20functions%20are%20used%20to%20manipulate%20Date,dd%20HH%3Amm%3Ass%20.)
```
# The ISO 8601 format YYYY-MM-DD (2022-11-01)
date_format(current_date,'yyyy-MM-01')

add_months("2022-01-01, 5, "yyyy-MM-dd")   # return string '2022-05-01'

add_months(current_date(), -24)            # return the date 24 months before

last_day(string date)

trunc(string date, string format)

date_add(string startdate, tinyint/smallint/int days)

date_add(timestamp startdate, tinyint/smallint/int days)

date_add(date startdate, tinyint/smallint/int days)

date_sub(string startdate, tinyint/smallint/int days)

datediff(string enddate, string startdate)
```

```
-- string to date
--
TO_DATE(
    from_unixtime(
        unix_timestamp(CAST(OnsetDateKey AS STRING) ,  'yyyyMMdd')
    )
)
```

```
 select
 CONCAT_WS('', 'Week', WEEKOFYEAR(appt.Appt_Time )) Week_Year

-- below code to calculate the date of Sunday of the week. 
,  date_sub(appt.Appt_Time,
                pmod(
                    datediff(to_date(appt.Appt_Time),'1900-01-07'),
                    7)
            ) Week_Year_Date

, date_format(appt.Appt_Time , 'EE') Day_Week
```

```
-- Get Age
, (year(current_date) - year(birth_date) 
   +case when  month(current_date) < month(birth_date)  then -1
         when month(birth_date) = month(current_date) and day(current_date) < day(birth_date)  then -1
         else 0 
     end) as age
```

```
%sql
CREATE OR REPLACE FUNCTION cldw.get_age(birth_date STRING, to_date STRING)
    RETURNS INT
    RETURN 
        (year(to_date) - year(birth_date) 
      + case when  month(to_date) < month(birth_date)  then -1
            when month(birth_date) = month(to_date) and day(to_date) < day(birth_date)  then -1
            else 0 
        end)
```

[TO_DATE: DATETIME-PATTERN](https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html)



# 2. String manipulation 



# Regexp 

```
select regexp_extract('foo|the|bar', r'(foo)\\|(.*?).(bar)', 0)  # foo|the|bar
select regexp_extract('foo|the|bar', r'(foo)\\|(.*?).(bar)', 1)  # foo
select regexp_extract('foo|the|bar', r'(foo)\\|(.*?).(bar)', 2)  # the
select regexp_extract('foo|the|bar', r'(foo)\\|(.*?).(bar)', 3)  # bar

regexp_extract(session, '([^\-]+)$', 1)
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


```
-- string replace
REGEXP_REPLACE(mbi_number,"-","") mbi  # delete the '-'
```

# https://stackoverflow.com/questions/41758949/difference-between-translate-and-regexp-replace
```
spark.sql("SELECT TRANSLATE('ed-ba', 'abcde', '12345')").show()
spark.sql("SELECT TRANSLATE('hello', 'e', 'a')").show() # e TO a
```



# Other 

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


# [convert array to string with group by](https://stackoverflow.com/questions/38711201/how-can-i-convert-array-to-string-in-hive-sql)
```
select 
  actor, 
  concat_ws(',',collect_set(date)) as grpdate 
from actor_table 
group by actor;
```

```
# Key words
, concat_ws("|", sort_array(collect_set(FromDate))) From_Date
, size(collect_set(FromDate)) size
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

## [restore a delta table to an ealier state](https://docs.databricks.com/delta/delta-utility.html#restore-a-delta-table-to-an-earlier-state)
```
RESTORE TABLE db.target_table TO VERSION AS OF <version>
RESTORE TABLE delta.`/data/target/` TO TIMESTAMP AS OF <timestamp>
```

```
VACUUM table_name [RETAIN num HOURS] [DRY RUN]
ALTER TABLE table_name SET TBLPROPERTIES ('delta.deletedFileRetentionDuration' = '30 days');
```

```
VACUUM table_name                   -- vacuum files not required by versions older than the default retention period
VACUUM table_name RETAIN 100 HOURS  -- vacuum files not required by versions more than 100 hours old
VACUUM table_name DRY RUN           -- do dry run to get the list of files to be deleted
```

```
## time travel
select * from table_name version as of 0
select * from table_name TIMESTAMP AS OF <timestamp>
```


## detlta table





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
# https://learn.microsoft.com/en-us/azure/databricks/sql/language-manual/delta-merge-into
MERGE INTO db.target_table t
USING db.source_upload s
ON t.id = s.id
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *

# https://learn.microsoft.com/en-us/azure/databricks/delta/merge
MERGE INTO people10m
USING people10mupdates
ON people10m.id = people10mupdates.id
WHEN MATCHED THEN
  UPDATE SET
    id = people10mupdates.id,
    firstName = people10mupdates.firstName,
    middleName = people10mupdates.middleName,
    lastName = people10mupdates.lastName,
    gender = people10mupdates.gender,
    birthDate = people10mupdates.birthDate,
    ssn = people10mupdates.ssn,
    salary = people10mupdates.salary
WHEN NOT MATCHED
  THEN INSERT (
    id,
    firstName,
    middleName,
    lastName,
    gender,
    birthDate,
    ssn,
    salary
  )
  VALUES (
    people10mupdates.id,
    people10mupdates.firstName,
    people10mupdates.middleName,
    people10mupdates.lastName,
    people10mupdates.gender,
    people10mupdates.birthDate,
    people10mupdates.ssn,
    people10mupdates.salary
  )
```

- If you specify *, this updates or inserts all columns in the target table. This assumes that the source table has the same columns as those in the target table, otherwise the query will throw an analysis error.


```
# delete and append 
spark.sql(f"DELETE FROM db.metric_result WHERE measure_id IN (SELECT DISTINCT measure_id FROM v_ma_star_measure)")
spark.sql("SELECT * FROM v_CCLF").write.format("delta").option("mergeSchema",     "true").mode("append"   ).save(target_file)
spark.sql("SELECT * FROM v_cclf").write.format("delta").option("overwriteSchema", "true").mode("overwrite").save(target_file)
```
Ref: 
- [cwiki.apache.org: Merge](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML#LanguageManualDML-Merge)
- [delta: quick start](https://docs.microsoft.com/en-us/azure/databricks/delta/quick-start)


## [Azure delta lake quick start with python](https://docs.microsoft.com/en-us/azure/databricks/_static/notebooks/delta/quickstart-python.html)




## [Apacke Hive UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-CreatingCustomUDFs)
## [pyspark udf decorate](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.functions.udf.html)

```
# This converts the input string characters into title case / proper case letters.
SELECT InitCap(‘This is posted on BIGDATAnSQL blog’);
```

```
# Define a function to clean up human names
def clean_name(name):
    # Remove leading and trailing white spaces
    cleaned_name = name.strip()
    
    # Convert the name to title case
    cleaned_name = cleaned_name.title()
    
    # Remove special characters and punctuation
    special_chars = '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
    cleaned_name = ''.join([char for char in cleaned_name if char not in special_chars])
    
    # Remove multiple spaces
    cleaned_name = ' '.join(cleaned_name.split())
    
    # Return the cleaned name
    return cleaned_name

# Register the clean_name function as a SQL function
spark.udf.register("clean_name", clean_name, "string")

# Use the clean_name SQL function in your queries to clean the name column
spark.sql("SELECT clean_name('jOhn/\. dOE') AS clean_name").display()


```

```
# Define a function to clean the address
def get_clean_address(address):
    # Remove leading and trailing white spaces
    cleaned_address = address.strip()
    
    # Convert the address to lowercase
    cleaned_address = cleaned_address.lower()
    
    # Remove special characters and punctuation
    special_chars = '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
    cleaned_address = ''.join([char for char in cleaned_address if char not in special_chars])

    # Remove multiple spaces
    cleaned_address = ' '.join(cleaned_address.split())

    # Remove numbers
    # cleaned_address = ''.join([char for char in cleaned_address if not char.isdigit()])
    
    # Return the cleaned address
    return cleaned_address

# Register the clean_address function as a SQL function
spark.udf.register("get_clean_address", get_clean_address, "string")

# Use the clean_address SQL function in your queries to clean the address column
spark.sql("SELECT get_clean_address('12 olive-dr.') AS clean_address").display()   
```

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

## Not in : One null value is enough to cause your query to return no results.
- hive> select * from A where A.id not in (select id from B where id is not null);
- NOT IN in the WHERE clause with uncorrelated subqueries is supported since Hive 0.13 which was released more than 3 years ago, on 21 April, 2014.

```
select * from A where id not in (select id from B where id is not null);
+----+--------+
| id |  name  |
+----+--------+
|  3 | George |
+----+--------+
On earlier versions the column of the outer table should be qualified with the table name/alias.

hive> select * from A where id not in (select id from B where id is not null);
FAILED: SemanticException [Error 10249]: Line 1:22 Unsupported SubQuery Expression 'id': Correlating expression cannot contain unqualified column references.
hive> select * from A where A.id not in (select id from B where id is not null);
OK
3   George

P.s.
When using NOT IN you should add is not null to the inner query, unless you are 100% sure that the relevant column does not contain null values.
**One null value is enough to cause your query to return no results.**

- the SQL standard treats x not in (a,b,c) as x<>a and x<>b and x<>c. If for example c is NULL then x<>c is UNKNOWN, therefore the whole expression is UNKNOWN. UNKNOWN is treated as FALSE. That means that the query is not going to return any rows no matter what x value is.
- ref: https://stackoverflow.com/questions/44714625/how-to-use-not-in-in-hive
```  
