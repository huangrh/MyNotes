
# Python on Azure 
https://learn.microsoft.com/en-us/python/api/overview/azure/identity-readme?view=azure-python  
https://learn.microsoft.com/en-us/azure/developer/python/get-started?view=azure-python

```
import geopandas as gpd

# Load the census tract shapefile
tracts = gpd.read_file("tracts.shp")

# Convert the lat lon coordinates to a point
point = gpd.points([lat, lon])

# Get the census tract that contains the point
tract = tracts.loc[tracts.contains(point)]

# Print the census tract name
print(tract.name)


```


```
# https://www.geeksforgeeks.org/get-post-requests-using-python/
import requests
response = requests.get('https://www.google.com')
```


# Extract files from an encrpyted zip file with python3
- https://gist.github.com/colmcoughlan/db1384156b8efe6676c9a6cc47756933
```
from zipfile import ZipFile

zip_file = 'test.zip'
password = 'password'

with ZipFile(zip_file) as zf:
  zf.extractall(pwd=bytes(password,'utf-8'))
```

- https://stackoverflow.com/questions/3451111/unzipping-files-in-python
```
import zipfile
with zipfile.ZipFile(path_to_zip_file, 'r') as zip_ref:
    zip_ref.extractall(directory_to_extract_to)
```


# SFTP. SCP
- https://stackoverflow.com/questions/250283/how-to-scp-in-python
```
def createSSHClient():
    import paramiko
    client = paramiko.SSHClient()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    client.connect('<IP Address>', username='<User Name>',password='' ,key_filename='<.PEM File path')
    #  Setup sftp connection and transmit this script 
    print ("copying")  
    sftp = client.open_sftp()  
    return sftp  

with createSSHClient() as sftp:
    sftp.put(<Source>, <Destination>)
    # sftp.close()  

# ------------------
# option 2
import paramiko
from scp import SCPClient

def createSSHClient(server, port, user, password):
    client = paramiko.SSHClient()
    client.load_system_host_keys()
    client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    client.connect(server, port, username='<User Name>',password='' ,key_filename='<.PEM File path')
    return client

ssh = createSSHClient(server, port, user, password)
scp = SCPClient(ssh.get_transport())  
```

# Repalce by matched part
```
#  https://note.nkmk.me/en/python-str-replace-translate-re-sub/
# 
re.sub(r'.*(\d{4}_\d+).*',   lambda m: m.group(1),   file.split("/")[-1])

```

```
import re
str = 'purple alice@google.com, blah monkey bob@abc.com blah dishwasher'
print(re.sub(r'.*(google.com).*(@abc.com).*', r'\1_\2', str))
```


# https://stackoverflow.com/questions/68937734/execute-multiple-notebooks-in-parallel-in-pyspark-databricks

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

- More: https://stackoverflow.com/questions/46045956/whats-the-difference-between-threadpool-vs-pool-in-the-multiprocessing-module
- thread vs process



# [List unpack with a * ](https://stackoverflow.com/questions/1720421/how-do-i-concatenate-two-lists-in-python/35631185#35631185)

```
>>> l1 = [1, 2, 3]
>>> l2 = [4, 5, 6]
>>> joined_list = [*l1, *l2]  # unpack both iterables in a list literal
>>> print(joined_list)
[1, 2, 3, 4, 5, 6]
```


# [try-except](https://www.geeksforgeeks.org/try-except-else-and-finally-in-python/)
```
try:
       # Some Code.... 

except:
       # optional block
       # Handling of exception (if required)

else:
       # execute if no exception

finally:
      # Some code .....(always executed)

```

# [convert-image-to-base64-string-in-python](https://appdividend.com/2022/09/15/how-to-convert-image-to-base64-string-in-python/#:~:text=Python%20image%20to%20base64%20String%20To%20convert%20the,a%20string%20using%20Base64%20and%20returns%20that%20string.)
```
import base64

with open("grayimage.png", "rb") as img_file:
    b64_string = base64.b64encode(img_file.read())
print(b64_string)
```

```
import base64

with open("grayimage.png", "rb") as img_file:
    b64_string = base64.b64encode(img_file.read())
print(b64_string.decode('utf-8'))
```

# Extract table from pdf to pandas dataframe    
- updated 2024

```
# [tabula-py installation and getting started](https://tabula-py.readthedocs.io/en/latest/getting_started.html#installation)
# https://pypi.org/project/tabula-py/
!pip install tabula-py
import tabula
# Read pdf into list of DataFrame
dfs = tabula.read_pdf("test.pdf", pages='all')

# Read remote pdf into list of DataFrame
dfs2 = tabula.read_pdf("https://github.com/tabulapdf/tabula-java/raw/master/src/test/resources/technology/tabula/arabic.pdf")

# convert PDF into CSV file
tabula.convert_into("test.pdf", "output.csv", output_format="csv", pages='all')

# convert all PDFs in a directory
tabula.convert_into_by_batch("input_directory", output_format='csv', pages='all')
```

- !pip install tabula-py  
```
import tabula.io as tabula
# Read pdf into list of DataFrame
dfs = tabula.read_pdf('CCLF_IP_508 (1).pdf', pages='all', encoding='cp1252')

```

- history    
- [Extract and Convert Tables From PDF Files to Pandas Data frame](https://towardsdatascience.com/how-to-extract-and-convert-tables-from-pdf-files-to-pandas-dataframe-cb2e4c596fa8)    
- [**Parse PDF Files While Retaining Structure with Tabula-py with Web-App](https://aegis4048.github.io/parse-pdf-files-while-retaining-structure-with-tabula-py)  

# date
```
import datetime
current_year = str(datetime.datetime.now().date().year)

# or
str(datetime.date.today().year)
```

# 
```
# https://stackoverflow.com/questions/3694487/in-python-how-do-you-convert-seconds-since-epoch-to-a-datetime-object
import datetime
timestamp = 1339521878.04 
value = datetime.datetime.fromtimestamp(timestamp)
print(value.strftime('%Y-%m-%d %H:%M:%S'))
```

```
# timestamp to datetime object
import datetime
#           1704067200
timestamp = 1683625197
dt_object = datetime.datetime.fromtimestamp(timestamp)
int(dt_object.timestamp())

# create datetime object 
import datetime
start_of_2024 = datetime.datetime(2024, 1, 1)
timestamp = start_of_2024.timestamp()
int(timestamp)

#
now = datetime.datetime.now()
int(now.timestamp())
```

```
# strptime() is another method available in DateTime which is used to format the time stamp which is in string format to date-time object.
from datetime import datetime
datetime.strptime(to, '%Y%m%d')
```

```
string to date in python   
>>> import datetime   
>>> datetime.datetime.strptime('24052010', "%d%m%Y").date()    
datetime.date(2010, 5, 24)   
```

# [Get Age ](https://stackoverflow.com/questions/2217488/age-from-birthdate-in-python)
```
from datetime import date

def calculate_age(born):
    today = date.today()
    return today.year - born.year - ((today.month, today.day) < (born.month, born.day))
```

# [File Exists](https://www.pythontutorial.net/python-basics/python-check-if-file-exists/)

Use os.path.exists() function or Path.is_file() method to check if a file exists   
```
from pathlib import Path
path = Path(path_to_file)
path.is_file()

from os.path import exists as file_exists
file_exists('readme.txt')
```

# [How to import local modules with Python](https://fortierq.github.io/python-import/)

```
from pathlib import Path
import sys
path_root = Path(__file__).parent
# sys.path.append(str(path_root))
# print(sys.path)
print(path_root)
```

# [read ndjson in python](https://stackoverflow.com/questions/63501251/how-to-open-ndjson-file-in-python)

```
import ujson as json
import pandas as pd

records = map(json.loads, open('/path/to/records.ndjson'))
df = pd.DataFrame.from_records(records)
```

# [how-to-run-javascript-from-python](https://www.geeksforgeeks.org/how-to-run-javascript-from-python/)
# [PyMiniRacer - support ECMA 6](https://github.com/sqreen/PyMiniRacer)

```
import js2py
js2py.eval_js(javascript code)
js2py.translate_file(Javascript File, Python File)
js2py.run_file(Javascript File)
```

# [python package](https://packaging.python.org/en/latest/tutorials/packaging-projects/)

# Test if path exists
```
dat_path = "data/dat.pkl"
#
from pathlib import Path
if Path(dat_path).exists(): 
    print("read data from: ", dat_path)
    dat = pd.read_pickle(dat_path)
else:
    query = f"SELECT * FROM `project_id`.dataset.table"
    print("Download data query: ", query)
    client = bigquery.Client(location = "US")
    job = client = client.query(query)
    dat = job.to_dataframe()
    dat.to_pickle(dat_path)
```

```
# https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-dummies
# columns = the column to encoder. 
pandas.get_dummies(df, columns = ['col1','col2']) 

# One Hot Encoder
def cat1hot_encoder(x):
    from sklearn.preprocessing import OneHotEncoder
    cat_encoder = OneHotEncoder(sparse=False)
    cat_1hot = cat_encoder.fit_transform(x)
    cat_1hot_df = pd.DataFrame(cat_1hot, dtype = int, columns = cat_encoder.categories_[0])
    return cat_1hot_df
```


# https://github.com/pwwang/datar
```
from datar import f
from datar.dplyr import mutate, filter, if_else
from datar.tibble import tibble
# or
# from datar.all import f, mutate, filter, if_else, tibble

df = tibble(
    x=range(4),
    y=['zero', 'one', 'two', 'three']
)
df >> mutate(z=f.x)
"""# output
        x        y       z
  <int64> <object> <int64>
0       0     zero       0
1       1      one       1
2       2      two       2
3       3    three       3
"""

df >> mutate(z=if_else(f.x>1, 1, 0))
"""# output:
        x        y       z
  <int64> <object> <int64>
0       0     zero       0
1       1      one       0
2       2      two       1
3       3    three       1
"""

df >> filter(f.x>1)
"""# output:
        x        y
  <int64> <object>
0       2      two
1       3    three
"""

df >> mutate(z=if_else(f.x>1, 1, 0)) >> filter(f.z==1)
"""# output:
        x        y       z
  <int64> <object> <int64>
0       2      two       1
1       3    three       1
"""
```


```
# https://stackoverflow.com/questions/40923165/python-pandas-equivalent-to-r-groupby-mutate
df = pd.DataFrame(
    dict(
        a=(1 , 1, 0, 1, 0 ), 
        b=(1 , 0, 0, 1, 0 ),
        c=(10, 5, 1, 5, 10),
        d=(3 , 1, 2, 1, 2 ),
    )
).assign(
    prod_c_d = lambda x: x['c'] * x['d'], 
    ratio    = lambda x: x['c'] / (x.groupby(['a','b']).transform('sum')['prod_c_d'])
)
```

# https://stackoverflow.com/questions/993358/creating-a-range-of-dates-in-python
```
import pandas as pd
from datetime import datetime

pd.date_range(end = datetime.today(), periods = 100).to_pydatetime().tolist()

#OR

pd.date_range(start="2018-09-09",end="2020-02-02")
```

# https://datagy.io/python-f-strings/
```
print(f'{"apple" : >30}') # Right align
print(f'{"apple" : <30}') # Left align
print(f'{"apple" : ^30}') # Center align
print(f'Percentage format for number with two decimal places: {0.9124325345:.2%}') # Percent
print(f'Fixed point format for number with three decimal places: {0.9124325345:.3f}')
print(f'Currency format for large_number with two decimal places: ${126783.6457:.2f}')
print(f'Currency format for large_number with two decimal places and comma seperators: ${126783.6457:,.2f}')
print(f'Exponent format for number: {0.9124325345:e}')
```
