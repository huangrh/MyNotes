# [convert-image-to-base64-string-in-python](https://appdividend.com/2022/09/15/how-to-convert-image-to-base64-string-in-python/#:~:text=Python%20image%20to%20base64%20String%20To%20convert%20the,a%20string%20using%20Base64%20and%20returns%20that%20string.)
```
import base64

with open("grayimage.png", "rb") as img_file:
    b64_string = base64.b64encode(img_file.read())
print(b64_string)
```

# Extract table from pdf to pandas dataframe    
- [tabula-py installation](https://tabula-py.readthedocs.io/en/latest/getting_started.html#installation)  
- [Extract and Convert Tables From PDF Files to Pandas Data frame](https://towardsdatascience.com/how-to-extract-and-convert-tables-from-pdf-files-to-pandas-dataframe-cb2e4c596fa8)    
- [**Parse PDF Files While Retaining Structure with Tabula-py with Web-App](https://aegis4048.github.io/parse-pdf-files-while-retaining-structure-with-tabula-py)  

# date
```
import datetime
current_year = str(datetime.datetime.now().date().year)

# or
str(datetime.date.today().year)
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

