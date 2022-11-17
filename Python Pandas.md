## Pivot long to wide
```
# https://stackoverflow.com/questions/47152691/how-can-i-pivot-a-dataframe
pd.pivot(
    data=df,        
    index='foo',    # Column to use to make new frame’s index. If None, uses existing index.
    columns='bar',  # Column to use to make new frame’s columns.
    values='baz'    # Column(s) to use for populating new frame’s values.
)
```

## Get Age

```
def get_age(x):
    from datetime import datetime
    from_service_year = current_year
    to  = str(from_service_year) + '0201'
    to  = datetime.strptime(to, '%Y%m%d')
    dob = x['date_birth']
    if dob is None: 
        age = None
    else:
        age = to.year - dob.year - ((to.month, to.day) < (dob.month, dob.day))
#     dob =datetime.strptime(dob, '%Y-%m-%d')
    return age
hcc_dat['age'] = hcc_dat.apply(get_age, axis = 1)
```

```
def get_icd10_list(icd10):
    if icd10 is None:
        return []
    return icd10.split("|")
hcc_dat['icd10list_current'] = hcc_dat['diag_current'].apply(get_icd10_list)

# 
# https://www.geeksforgeeks.org/partial-functions-python/
from functools import partial
he = HCCEngine(version = '24')
def get_raf(row, col='icd10list_current'):
    icd10 = row[col]
    age   = row['age']
    sex   = row['gender']
    if not all([age, sex]):
        return None
    rp = he.profile(dx_lst=icd10, age=age, sex=sex, elig="CNA", orec="0", medicaid=False)
    return rp['risk_score']    
     
hcc_dat['raf_current']         = hcc_dat.apply(partial(get_raf, col='icd10list_current') , axis = 1)
hcc_dat['raf_past']            = hcc_dat.apply(partial(get_raf, col='icd10list_past') , axis = 1)
hcc_dat['raf_current_chronic'] = hcc_dat.apply(partial(get_raf, col='icd10list_current_chronic') , axis = 1)
hcc_dat['raf_past_chronic']    = hcc_dat.apply(partial(get_raf, col='icd10list_past_chronic') , axis = 1)
```

## split string into multiple rows
```
# https://stackoverflow.com/questions/60674954/explode-r-dataframe
df["Name"] = df["Name"].apply(lambda x: x.split(";"))
df.explode("Name")
```

## [quantile calculation](https://datagy.io/pandas-quantile/)
```
# Loading a Sample Pandas Dataframe
import pandas as pd
df = pd.DataFrame.from_dict({
    'Student': ['Nik', 'Kate', 'Kevin', 'Evan', 'Jane', 'Kyra', 'Melissa'],
    'English': [90, 95, 75, 93, 60, 85, 75],
    'Chemistry': [95, 95, 75, 65, 50, 85, 100],
    'Math': [100, 95, 50, 75, 90, 50, 80]
})
print(df.head())

# Understanding the Pandas .quantile() method to calculate percentiles
df.quantile(
    q=0.5,                      # The percentile to calculate
    axis=0,                     # The axis to calculate the percentile on
    numeric_only=True,          # To calculate only for numeric columns
    interpolation='linear'      # The type of interpolation to use when the quantile is between 2 values
)
```

## [Apply a Function to Multiple Columns](https://www.delftstack.com/howto/python-pandas/pandas-apply-multiple-columns/)

```
import pandas as pd
import numpy as np

df = pd.DataFrame([
                    [5,6,7,8],
                    [1,9,12,14],
                    [4,8,10,6]
                    ],
                  columns = ['a','b','c','d'])

print("The original dataframe:")
print(df)

def func(x):
    return x['a'] + x['b']

df['e']  = df.apply(func, axis = 1)

print("The new dataframe:")
print(df)
```

## [Hash each value in a pandas data frame](https://stackoverflow.com/questions/30143911/hash-each-value-in-a-pandas-data-frame)  

```
import pandas as pd
df = pd.DataFrame({'a':['asds','asdds','asdsadsdas', '123']})
df["hash"] = pd.util.hash_array(df["a"].to_numpy())
df["hash2"] = pd.util.hash_array(df["a"].to_numpy())
```


```
from pyspark.sql.functions import udf, col
from Crypto.Cipher import AES

def encrypt(key, text):
    obj = AES.new(key, AES.MODE_CFB, 'This is an IV456')
    return obj.encrypt(text)
    
from functools import partial
encrypt_with_key = partial(encrypt, secret_key)

@pandas_udf(BinaryType())
def pandas_udf_encrypt(clear_strings: pd.Series) -> pd.Series:
    return clear_strings.apply(encrypt_with_key)

df.withColumn('encrypted', pandas_udf_encrypt(clear_text_column)).show()

```

# [pandas-parsing-json](https://github.com/ankitgoel1602/data-science/blob/master/json-data/pandas-parsing-json.ipynb)

```
# Read the JSON data
import json
import pandas as pd
level1_json_data = pd.read_json('level_1.json')

# As we saw above, we can not use read_json directly, lets see how we can convert it
# Read the JSON data using json python module
with open('multiple_levels.json','r') as f:
    data = json.loads(f.read())
    
# Now many times we would like to add a string to each field we flattened out.
# For dictionaries this function automatically appends the parent dictionary name
# For lists we can use 'prefix'.
# the meta data we had is basically the config_params for the problem and we can use meta_prefix for that.
# other attributes are basically related to DBSCAN algorithm and we can use record_prefix to show that.
multiple_level_data = pd.json_normalize(data, record_path=['Results'], \
                    meta=['original_number_of_clusters','Scaler','family_min_samples_percentage'],
                 meta_prefix='config_params_', record_prefix='dbscan_')
```


# Pandas options
```
pd.set_option("display.max.columns", None)
pd.set_option("display.precision", 2)

```

# Pandas cheatsheet
- Calculate the row average

```
# calculate
df['mean'] = df.mean(axis=1)
# fill the na by row average
# Pandas Dataframe: Replacing NaN with row average
df.apply(lambda row: row.fillna(row.mean()), axis=1)
```

```
https://stackoverflow.com/questions/20069009/pandas-get-topmost-n-records-within-each-group
df.sort_values(['id', 'value'], axis=0).groupby('id').head(2)
```

# Pandas Groupby + apply
```
# setup
import pandas as pd
import numpy as np
a = np.array(range(15)).reshape(5,3)
df = pd.DataFrame(a, 
                 index = ['r'+str(n) for n in range(5)],
                 columns=['a','b','c'])
df['key1'] = ['a','a','b','b','c']
df['key2'] = ['a','a','b','c','c']

# group by and apply
df.groupby(['key1','key2'], as_index = False).apply(lambda g: pd.Series(dict(
    col1 = np.sum(g.a),
    col2 = np.sum(g.b),
    col3 = np.sum(g.a) + np.sum(g.b),
    col4 = np.sum(g.a * g.b),
    col5 = g.a
)))
```
# merge / join

# pivot  
## wide to long
## long to wide 



