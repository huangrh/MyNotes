# [conver array to string with group by](https://stackoverflow.com/questions/38711201/how-can-i-convert-array-to-string-in-hive-sql)
```
select 
  actor, 
  concat_ws(',',collect_set(date)) as grpdate 
from actor_table 
group by actor;
```

# [Apacke Hive UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-CreatingCustomUDFs)


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
