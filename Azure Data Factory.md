# ADF: sync between working branch and adf_publish branch
- https://praveenkumarsreeram.com/2020/06/27/azure-data-factory-all-about-publish-branch-adf_publish/

# ADF Connectors

- [Azure Data Lake Storage Gen2(ADLS2)](https://learn.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage?tabs=data-factory)
- [sftp](https://learn.microsoft.com/en-us/azure/data-factory/connector-sftp?tabs=data-factory)
- [snowflake](https://resources.snowflake.com/data-warehousing/snowflake-connector-for-azure-data-factory-adf)

## [Azure Data Factory Custom Email Notifications Tutorial through logic app](https://www.youtube.com/watch?v=zyqf8e-6u4w)

## [adf expression](https://docs.microsoft.com/en-us/azure/data-factory/parameterize-linked-services?tabs=data-factory)  

adf
sql server
SQL database

storage account
Blobs + New container called input
upload file

## [ADF from beginning to Pro](https://www.youtube.com/watch?v=DLmlFlQGQWo)

Youtube: Global Parameters in Azure Data Factory | Azure Data Factory Tutorial 2021

## ADF version control 
- https://docs.microsoft.com/en-us/azure/data-factory/source-control  
- https://stackoverflow.com/questions/65817692/adf-publish-confusion-in-git-mode  

## Continuous Integration, Continuous Deployment (CI-CD) with Azure DevOps
https://www.youtube.com/watch?v=jRgLSMlp28U&t=26s

## ADF: [transform data using databricks notebook](https://docs.microsoft.com/en-us/azure/data-factory/transform-data-using-databricks-notebook)
1. create a New linked service :  This linked service contains the connection information to the Databricks cluster.   
  - On the home page, switch to the Manage tab in the left panel.
  - Select Linked services under Connections, and then select + New.
  - select Computer > Azure Databricks. 
2. Create a pipeline
  - Go the Authoring tab: select the + (plus) button, and then select Pipeline on the menu.
  - Create a parameter to be used in the Pipeline. Later you pass this parameter to the Databricks Notebook Activity. In the empty pipeline, select the Parameters tab, then select + New and name it as 'input'.
  - In the Activities toolbox, expand Databricks. Drag the Notebook activity from the Activities toolbox to the pipeline designer surface.

## Connector: ADF & snowflake   
- https://learn.microsoft.com/en-us/azure/data-factory/connector-snowflake?tabs=data-factory  
- https://learn.microsoft.com/en-us/azure/data-factory/connector-snowflake?tabs=data-factory  

