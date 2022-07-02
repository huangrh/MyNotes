## [create-mount-point-in-azure-databricks](https://bigdataprogrammers.com/create-mount-point-in-azure-databricks/)
## [](https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/azure/azure-storage)

## [K Means Clustering using PySpark on Big Data](https://towardsdatascience.com/k-means-clustering-using-pyspark-on-big-data-6214beacdc8b)

```
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName(‘Clustering using K-Means’).getOrCreate()
data_customer=spark.read.csv('CC General.csv', header=True, inferSchema=True)
data_customer.printSchema()
# 
from pyspark.ml.feature import VectorAssembler
data_customer.columns
assemble=VectorAssembler(inputCols=[
 'BALANCE',
 'BALANCE_FREQUENCY',
 'PURCHASES',
 'ONEOFF_PURCHASES',
 'INSTALLMENTS_PURCHASES',
 'CASH_ADVANCE',
 'PURCHASES_FREQUENCY',
 'ONEOFF_PURCHASES_FREQUENCY',
 'PURCHASES_INSTALLMENTS_FREQUENCY',
 'CASH_ADVANCE_FREQUENCY',
 'CASH_ADVANCE_TRX',
 'PURCHASES_TRX',
 'CREDIT_LIMIT',
 'PAYMENTS',
 'MINIMUM_PAYMENTS',
 'PRC_FULL_PAYMENT',
 'TENURE'], outputCol='features')
assembled_data=assemble.transform(data_customer)
assembled_data.show(2)

# 
from pyspark.ml.feature import StandardScaler
scale=StandardScaler(inputCol='features',outputCol='standardized')
data_scale=scale.fit(assembled_data)
data_scale_output=data_scale.transform(assembled_data)
data_scale_output.show(2)

# 
from pyspark.ml.clustering import KMeans
from pyspark.ml.evaluation import ClusteringEvaluator
silhouette_score=[]
evaluator = ClusteringEvaluator(predictionCol='prediction', featuresCol='standardized', \
                                metricName='silhouette', distanceMeasure='squaredEuclidean')
for i in range(2,10):
    
    KMeans_algo=KMeans(featuresCol='standardized', k=i)
    
    KMeans_fit=KMeans_algo.fit(data_scale_output)
    
    output=KMeans_fit.transform(data_scale_output)
    
    
    score=evaluator.evaluate(output)
    
    silhouette_score.append(score)
    
    print("Silhouette Score:",score)
    
#Visualizing the silhouette scores in a plot
import matplotlib.pyplot as plt
fig, ax = plt.subplots(1,1, figsize =(8,6))
ax.plot(range(2,10),silhouette_score)
ax.set_xlabel(‘k’)
ax.set_ylabel(‘cost’)    
```

## Age
```
 , (year(current_date) - year(patient_dob) 
     + case when month(patient_dob) > month(current_date) then -1
           when month(patient_dob) = month(current_date) and day(patient_dob) > day(current_date) then -1
           else 0 
       end) as age
```

## [pyspark case when](https://sparkbyexamples.com/pyspark/pyspark-when-otherwise/)
```
from pyspark.sql.functions import when
df2 = df.withColumn("new_gender", when(df.gender == "M","Male")
                                 .when(df.gender == "F","Female")
                                 .when(df.gender.isNull() ,"")
                                 .otherwise(df.gender))
df2.show()
```

## [how to split and explode into multiple row](https://www.geeksforgeeks.org/pyspark-split-multiple-array-columns-into-rows/#:~:text=To%20split%20multiple%20array%20column%20data%20into%20rows,values%20present%20in%20the%20array%20will%20be%20ignored.)

```

df2 = df.select(df.Name,explode(df.Courses_enrolled))
```

## [how to get the last item from a list in pyspark](https://stackoverflow.com/questions/40467936/how-do-i-get-the-last-item-from-a-list-using-pyspark)

```
# element_at
# split
from pyspark.sql.functions import element_at, split, col
df.withColumn('Practice', element_at(split(col('source'), '_'),-1))
```

## [Create Global Temp View](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.createOrReplaceGlobalTempView.html#pyspark.sql.DataFrame.createOrReplaceGlobalTempView)
```
# create global temp view
df.createOrReplaceGlobalTempView("people")
df2 = df.filter(df.age > 3)
df2.createOrReplaceGlobalTempView("people")
df3 = spark.sql("select * from global_temp.people")
sorted(df3.collect()) == sorted(df2.collect())  # True
# Drop
spark.catalog.dropGlobalTempView("people")    
```

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

```
# https://www.youtube.com/watch?v=pc8Kv-lRD8k
aggregated
   .write
   .format("com.crealytics.spart.excel")
   .option("header", true)
   .option("dataAddress", "sheet1!A1")  # sheet name, can write to multiple sheets
   .mode("overwrite")
   .save("/mnt/myblob/output.xlsx")
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

