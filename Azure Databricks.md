# How to get current working dir/path  

```
import os
from pathlib import Path
cwd = os.getcwd().replace("/Workspace", '') + '/'
str(cwd)
```

# Unity Catalog
- https://learn.microsoft.com/en-us/azure/databricks/data-governance/unity-catalog/ 

# App
- https://www.databricks.com/blog/introducing-databricks-apps#:~:text=With%20Databricks%20Apps%2C%20data%20scientists,to%20quickly%20build%20flexible%20apps.  

# Azure Blob File Storage System  - ABFFS
## Access abfss:   
dbutils.fs.ls("abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/<path-to-data>")   
- https://learn.microsoft.com/en-us/azure/databricks/connect/storage/azure-storage
- https://docs.databricks.com/en/connect/storage/azure-storage.html   

- 
# Databricks workflow
https://docs.databricks.com/en/notebooks/notebook-workflows.html

## NCC for SQL warhouse
- https://learn.microsoft.com/en-us/azure/databricks/security/network/serverless-network-security/serverless-private-link

## databricks workflows ModuleNotFoundError: No module
- from the pipeline click the 2nd tab called Tasks
- add python package name into the dependent libraries. 

## convert between spark & R data frame  

```
SparkR::as.data.frame(SparkR::sql(query))
SparkR::createDataFrame(r_data_frame)
```

## Read and Write

```
df.write.format("delta").
df.write.format('parquet').mode("overwrite").save(target_file)
```

```
# Load data
for i,file in enumerate(csv_files):
    # setup

    files_loading.append(file.path)
    file_prefix = [x for x in file.path.replace('dbfs:','').replace(lz_path,'').split('/') if x != ''][:-1]
    file_postfix = re.sub('.*-{3,}(.+)','\\1',file.name).replace(".csv","")
    file_postfix = re.sub('_{2,}','_',
                          re.sub(r'\(|\)| |-', '_', 
                                 file_postfix))
    file_postfix = re.sub('^_|_$', '', file_postfix)
    target_file    = rz_path + '/' + '/'.join(file_prefix) + '/' +file_postfix
    external_table = ('_'.join(file_prefix) + '_' +file_postfix).lower()
    source = "_".join(file_prefix)
    print(str(i), external_table)
    # target_file, external_table
    # read the file
    df1 = spark.read.format("csv").option("Header", "true").option('quote','"').option("escape", "\\").option("escape", '"').load(file.path)

    # clean up the file header
    rename_str = "df2 = df1." + '.'.join([f"""withColumnRenamed("{col}", "{name_clean(col)}")"""
                                for col in df1.schema.fieldNames()])
    exec(rename_str)
    df3 = df2.withColumn("sessionid",lit(get_session_id()))
    if 'source' in df3.columns:
        df3 = df3.withColumn('source', coalesce(col('source'),(lit(source))))

    print('shape: ',df2.count(), ",", len(df2.columns), '\n')
    # save file to rawzone
    df3.write.format('delta').option("overwriteSchema", "true").mode('overwrite').save(target_file)

    # create a external table if not exists
    query = f"""
    CREATE TABLE IF NOT EXISTS rawzone.{external_table} 
    USING DELTA 
    LOCATION '{target_file}';
    """
    spark.sql(query)
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

## Exit 

```
path = dbutils.notebook.entry_point.getDbutils().notebook().getContext().notebookPath().get()
dbutils.notebook.exit({"Run Finished: ": path})
```
## Read and write data from Snowflake
- https://learn.microsoft.com/en-us/azure/databricks/external-data/snowflake
- https://learn.microsoft.com/en-us/azure/databricks/_extras/notebooks/source/snowflake-python.html


```
from pyspark.sql.types import FloatType, StringType
from pyspark.sql.functions import col
for index, row in tables_df.iterrows():
    table_schema = row['TABLE_SCHEMA']
    table_name   = row['TABLE_NAME']
    print(index, table_schema, table_name)
    df = spark.read \
        .format("snowflake") \
        .options(**options) \
        .options(sfSchema=table_schema) \
        .option("dbtable", table_name) \
        .load() 

    #find all decimal columns in your SparkDF
    decimals_cols = [c for c in df.columns if 'Decimal' in str(df.schema[c].dataType)]
    
    #convert all decimals columns to floats
    for col in decimals_cols:
        # df = df.withColumn(col, df[col].cast(FloatType()))
        df = df.withColumn(col, df[col].cast(StringType()))

    file_full_name = '/dbfs/mnt/storage_container/' + 'LandingZone/' + table_schema + '/' + table_name + '.csv'
    df.toPandas().to_csv(file_full_name, index = False, sep = ',')
```

```
# Use dbutils secrets to get Snowflake credentials.
user = dbutils.secrets.get("data-warehouse", "<snowflake-user>")
password = dbutils.secrets.get("data-warehouse", "<snowflake-password>")
 
options = {
  "sfUrl": "<snowflake-url>",
  "sfUser": user,
  "sfPassword": password,
  "sfDatabase": "<snowflake-database>",
  "sfSchema": "<snowflake-schema>",
  "sfWarehouse": "<snowflake-cluster>"
}

# Read the data written by the previous cell back.
df = spark.read \
  .format("snowflake") \
  .options(**options) \
  .option("dbtable", "table_name") \
  .load()
display(df)

# Write the data to a Delta table
df.write.format("delta").saveAsTable("sf_ingest_table")

# Generate a simple dataset containing five values and write the dataset to Snowflake.
spark.range(5).write \
  .format("snowflake") \
  .options(**options) \
  .option("dbtable", "table_name") \
  .save()
```

## Create Secrets Scope  
- 38. Create Databricks Scope using Azure Key Vault and List secrets from Scope  
- https://www.youtube.com/watch?v=2gMX98-RXPM
- notebook_url + secrets/createScope (example below)
- (from)https://adb-1279999333333333.6.azuredatabricks.net/?o=1279900022221111#notebook/2309313612374424/command/3439165058487049
- (to)  https://adb-1279999333333333.6.azuredatabricks.net/?o=1279900022221111#secrets/createScope
    - Scope Name: 
    - properties: DNS Name    = properties/setting/Vault URI  in the Key Vault ,
    - properties: Resource ID = properties/setting/Resource ID in the Key Vault

- dbutils.secrets.listScopes()  
- dbutils.secrets.list(scope="scope-name") # list the secret from Azure Key Vault. It will go to the Azure Key Vault, and list what are the secrets in the scope.
- dbutils.secrests.get("<scope name>", "<secrets name>") 
# 
- https://docs.snowflake.com/en/user-guide/spark-connector-overview
- The connector uses Scala 2.12.x or 2.13.x to perform these operations and uses the Snowflake JDBC driver to communicate with Snowflake.

```
# list scope name
dbutils.secrets.listScopes()
# List secret key
dbutils.secrets.list('Scope Shared-Dev-Scus-001')
# get the secret
secret = dbutils.secrets.get(scope="myScope", key="myKey")

for char in secret:
    print(char, end=" ")
```

- key valuts --> Secrets --> Generate/Import to create a secret
- fill the fields: Name, Secret value
 
## Run multiple notebook concurently
```
from multiprocessing.pool import ThreadPool
pool = ThreadPool(5)
notebooks = ['dim_1', 'dim_2']
pool.map(
    lambda path: dbutils.notebook.run(
        "/Test/Threading/"+path, 
        timeout_seconds= 60, 
        arguments={"input-data": path}),
    notebooks)
    
```
- https://stackoverflow.com/questions/68937734/execute-multiple-notebooks-in-parallel-in-pyspark-databricks

## Get Workspace ID from python notebook
- [how-to-get-workspace-name-inside-a-python-notebook-in-databricks](https://stackoverflow.com/questions/70932930/how-to-get-workspace-name-inside-a-python-notebook-in-databricks)

```
# get key
keys = spark.conf.getAll
display(keys)
# list workspace ID
spark.conf.get("spark.databricks.workspaceUrl")
spark.conf.get("spark.databricks.workspaceUrl").split('.')[0]
```
## Run a Databricks notebook from another notebook
- https://docs.databricks.com/notebooks/notebook-workflows.html  
- dbutils.notebook API
  - run(path: String,  timeout_seconds: int, arguments: Map): String
  - exit

```
# pass arguments
# in the parent notebook
path = cwd + '106_051_person_RAF'
arguments_list = [
    {"Data_Source_Group": 'All Data Source',         "Data_Source": "Data_Source_Name"},
    {"Data_Source_Group": 'Claims',                  "Data_Source": "Data_Source_Name"}, 
    {"Data_Source_Group": 'RCM',                     "Data_Source": "Data_Source_Name"}, 
    {"Data_Source_Group": 'EMR',                     "Data_Source": "Data_Source_Name"}]
for arguments in arguments_list:
    print(arguments)
    dbutils.notebook.run(path=path, timeout_seconds=-1, arguments=arguments)

# in the child notebook
#
dbutils.widgets.text(name="Data_Source_Group", defaultValue="All Data Source", label="Enter Data Source Group")
Data_Source_Group_Name = dbutils.widgets.get("Data_Source_Group")
print(Data_Source_Group_Name)
#
dbutils.widgets.text(name = "Data_Source", defaultValue="Data_Source_Name", label="Enter Data Source")
Data_Source_Name = dbutils.widgets.get("Data_Source")
print(Data_Source_Name)
```

```
# Example 1 - returning data through temporary views.
# You can only return one string using dbutils.notebook.exit(), but since called notebooks reside in the same JVM, you can
# return a name referencing data stored in a temporary view.

## In callee notebook
spark.range(5).toDF("value").createOrReplaceGlobalTempView("my_data")
dbutils.notebook.exit("my_data")

## In caller notebook
returned_table = dbutils.notebook.run("LOCATION_OF_CALLEE_NOTEBOOK", 60)
global_temp_db = spark.conf.get("spark.sql.globalTempDatabase")
display(table(global_temp_db + "." + returned_table))

# Example 2 - returning data through DBFS.
# For larger datasets, you can write the results to DBFS and then return the DBFS path of the stored data.

## In callee notebook
dbutils.fs.rm("/tmp/results/my_data", recurse=True)
spark.range(5).toDF("value").write.format("parquet").load("dbfs:/tmp/results/my_data")
dbutils.notebook.exit("dbfs:/tmp/results/my_data")

## In caller notebook
returned_table = dbutils.notebook.run("LOCATION_OF_CALLEE_NOTEBOOK", 60)
display(spark.read.format("parquet").load(returned_table))

# Example 3 - returning JSON data.
# To return multiple values, you can use standard JSON libraries to serialize and deserialize results.

## In callee notebook
import json
dbutils.notebook.exit(json.dumps({
  "status": "OK",
  "table": "my_data"
}))

## In caller notebook
result = dbutils.notebook.run("LOCATION_OF_CALLEE_NOTEBOOK", 60)
print(json.loads(result))
```

## Defining custom schema for a dataframe
- https://stackoverflow.com/questions/57901493/pyspark-defining-custom-schema-for-a-dataframe
```
table_schema = StructType([StructField('ID', StringType(), True),
                     StructField('Name', StringType(), True),
                     StructField('Tax_Percentage(%)', IntegerType(), False),
                     StructField('Effective_From', TimestampType(), False),
                     StructField('Effective_Upto', TimestampType(), True)])

# CSV options
infer_schema = "true"
first_row_is_header = "true"
delimiter = ","


df = spark.read.format(file_type) \
  #.option("inferSchema", infer_schema) \
  .option("header", first_row_is_header) \
  .option("sep", delimiter) \
  #.option("schema", table_schema) \
  .schema(table_schema) \
  .load(file_location)
```
## rm directory
https://docs.databricks.com/dev-tools/databricks-utils.html#file-system-utilities
```
# dbutils.fs.rm(dir: String, recurse: boolean = false): boolean -> Removes a file or directory
dbutils.fs.rm('adl://azurelake.azuredatalakestore.net/landing/stageone/',True)
```
## 

```
awv_code_desc = sc.broadcast({
    'G0402': 'Initial Preventive Physical Examination',
    'G0438': 'Initial Medicare AWV',
    'G0439': 'Subsequent Medicare AWV'
})

@udf
def get_awv_desc(x):
    if x in awv_code_desc.value.keys():
        return awv_code_desc.value[x]
    else: 
        return x
```

## [create-mount-point-in-azure-databricks](https://bigdataprogrammers.com/create-mount-point-in-azure-databricks/)
```
dbutils.fs.mount(
  source = "wasbs://<container-name>@<storage-account-name>.blob.core.windows.net/<directory-name>",
  mountPoint = "/mnt/<mount-name>",
  extraConfigs = Map("<conf-key>" -> dbutils.secrets.get(
                scope = "<scope-name>", 
                key = "<key-name>")
        )
dbutils.fs.mount(
  source = "wasbs://<container-name>@<storage-account-name>.blob.core.windows.net/",
  mount_point = "/mnt/<mount-name>",
  extra_configs = {"fs.azure.account.key.<storage-account-name>.blob.core.windows.net":"<key>"})
```

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


## spark-excel
driver: https://mvnrepository.com/artifact/com.crealytics/spark-excel_2.13/3.2.1_0.17.1  
instruction: https://stackoverflow.com/questions/56426069/how-to-read-xlsx-or-xls-files-as-spark-dataframe
github: https://github.com/crealytics/spark-excel
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

 ## [Learn Databricks - with CloudFitness](https://www.youtube.com/watch?v=3P32gepRMxc&list=PLtlmylp_ZK5wF5EbBKRBBATCzS2xbs_53)
