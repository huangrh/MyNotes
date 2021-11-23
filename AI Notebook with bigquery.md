# 1. conda setup

### create a new enviroment  
  -  create a new enviroment with a specific version of python
  - ref: https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-python.html
  - -n : --name; py39 is the name of the new enviroment  
  
> conda create -n myenv python=3.6 -y  
> conda create -n py39  python=3.9    anaconda 

- Please update conda by running  

> conda update -n base conda

- To activate this environment, use
> conda activate py39
 
- To deactivate an active environment, use
> conda deactivate

### conda install update   
- https://stackoverflow.com/questions/36183486/importerror-no-module-named-google
> conda install -c conda-forge google-cloud-bigquery
> conda install --upgrade google-api-python-client  


- install into a specific environment
> conda install -n myenv requests -y


- install the latest pandas-gbq from github  
> conda install git pip   
> pip install git+https://github.com/pydata/pandas-gbq.git

- install pytorch
> conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch


# 2. Get data from bigquery

```py
# Import libraries
from google.cloud import bigquery
import pandas as pd

# init a client
client = bigquery.Client(location="US")

# create a query
query = """
SELECT * FROM `bigquery-public-data.cms_codes.icd10_diagnoses_2019` LIMIT 1000
"""

# query job
query_job = client.query(
    query,
    # Location must match that of the dataset(s) referenced in the query.
    location="US",
)  # API request - starts the query


# to a data frame 
df = query_job.to_dataframe()
df
```

# 3. upload data to bigquery

```py
# https://stackoverflow.com/questions/48886761/efficiently-write-a-pandas-dataframe-to-google-bigquery
client = bigquery.Client(location="US")
project_id = 
table_id = 'my_dataset_id.mytable_name'

# convert into a string
df = df.astype(str)

# 
to_gbq( destination_table, 
        project_id=None, 
        chunksize=None, 
        reauth=False, 
        if_exists='fail', 
        auth_local_webserver=False, 
        table_schema=None, 
        location=None, 
        progress_bar=True, 
        credentials=None)
        
df.to_gbq(destination_table = 'my_dataset_id.mytable_name', 
         project_id = 'my_project_id',
         chunksize  = None, # I have tried with several chunk sizes, it runs faster when it's one big chunk (at least for me)
         if_exists='append')
```
