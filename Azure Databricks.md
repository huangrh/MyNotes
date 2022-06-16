## [Get Age in hive SQL](https://stackoverflow.com/questions/23384130/calculating-age-from-date-of-birth-using-hive)
```hive
   , (year(current_date) - year(den.patient_dob) 
     + case when month(den.patient_dob) > month(current_date) then -1
           when month(den.patient_dob) = month(current_date) and day(den.patient_dob) > day(current_date) then -1
           else 0 
       end) as age
```


## [How to import OneDrive data into Databricks](https://www.cdata.com/kb/tech/onedrive-jdbc-azure-databricks.rst)  

## Schema
[PySpark StructType & StructField Explained with Examples](https://sparkbyexamples.com/pyspark/pyspark-structtype-and-structfield/)


## Read spark excel
driver: https://mvnrepository.com/artifact/com.crealytics/spark-excel_2.13/3.2.1_0.17.1  
instruction: https://stackoverflow.com/questions/56426069/how-to-read-xlsx-or-xls-files-as-spark-dataframe
- clusters > your cluster > libraries > install new > select Maven and in 'Coordinates' paste com.crealytics:spark-excel_2.13:3.2.1_0.17.1

```
df = spark.read.format("com.crealytics.spark.excel") \
    .option("useHeader", "true") \
    .option("inferSchema", "true") \
    .option("dataAddress", "'NameOfYourExcelSheet'!A1") \
    .load(filePath)
df1 = spark.read.format("com.crealytics.spark.excel").option("Header", "true").option('quote','"').option("escape", "\\").option("escape", '"').schema(schema).load(file.path)
```

## List file recursive

```
def lsR(path):
  return([fname for flist in [([fi] 
                               if fi.isFile() 
                               else lsR(fi.path)) 
                              for fi in dbutils.fs.ls(path)] 
          for fname in flist])
files = lsR('/path/to/folder')
```
## Pyspark : title case
```
upper
lower
initcap
```


https://docs.databricks.com/notebooks/notebooks-use.html#develop-notebooks


https://www.youtube.com/watch?v=cggfZYZtUzE

# Databricks - MLflow guide  
https://docs.microsoft.com/en-us/azure/databricks/applications/mlflow/

# [connect-databricks-excel-python-r](https://docs.microsoft.com/en-us/azure/databricks/scenarios/connect-databricks-excel-python-r)
# [Download simba ODBC Driver for Apache Spark](https://downloads.datastax.com/#odbc-jdbc-drivers)

## [Create database in Azure databricks using delta lake. ](https://docs.microsoft.com/en-us/azure/databricks/data/tables)

```

```
## [Databricks Runtime Mannual SQL Language manual](https://docs.microsoft.com/en-us/azure/databricks/spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-function)  
## [Databricks : Get started with Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/azure/adls-gen2/)  
https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/azure/adls-gen2/azure-datalake-gen2-get-started

## [fileinfo ](https://stackoverflow.com/questions/69391541/databricks-fileinfo-java-lang-classcastexception-com-databricks-backend-daemon)
```
n = dbutils.fs.ls("dbfs:/mnt/...")
display(n)
df = pd.DataFrame(n)
```

