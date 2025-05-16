## Read pdf to data frame

```
# pip install tabula.io
import tabula.io as tabula
import pandas as pd 

# Read pdf into list of DataFrame
file = "2025tables.pdf"
dfs = tabula.read_pdf(file, pages='all', encoding='cp1252',  pandas_options={'header': None})
dfs[0].head()
df = pd.concat(dfs)
df.head()
```

## Excel
```
pip install openpyxl

df = pd.read_excel(path,sheet_name = "npi_exclusion", na_values=["","NA", 'NULL','null'], keep_default_na=False, dtype=str)
```

```
# https://stackoverflow.com/questions/17977540/pandas-looking-up-the-list-of-sheets-in-an-excel-file
df = pandas.read_excel("/yourPath/FileName.xlsx", sheet_name=None);
df.keys()
```

## HOW to drop columns if the columns is null.   

```
# https://www.geeksforgeeks.org/how-to-drop-one-or-multiple-columns-in-pandas-dataframe/
# 
for colname, col in df.iteritems(): 
    if all(col.isnull()) : 
        print(colname)
        df.drop(colname, axis = 1, inplace = True)
```

## How to loop through columns in a dataframe   
- https://www.geeksforgeeks.org/loop-or-iterate-over-all-or-certain-columns-of-a-dataframe-in-python-pandas/   
```
import pandas as pd
# List of Tuples
students = [('Ankit', 22, 'A'),
           ('Swapnil', 22, 'B'),
           ('Priya', 22, 'B'),
           ('Shivangi', 22, 'B'),
            ]
# Create a DataFrame object
stu_df = pd.DataFrame(students, columns =['Name', 'Age', 'Section'],
                      index =['1', '2', '3', '4'])
 
# gives a tuple of column name and series
# for each column in the dataframe
for (columnName, columnData) in stu_df.iteritems():
    print('Column Name : ', columnName)
    print('Column Contents : ', columnData.values)
```
## How to loop through rows in a dataframe 
## # https://stackoverflow.com/questions/16476924/how-to-iterate-over-rows-in-a-dataframe-in-pandas
```
import pandas as pd

df = pd.DataFrame({'c1': [10, 11, 12], 'c2': [100, 110, 120]})
df = df.reset_index()  # make sure indexes pair with number of rows

for index, row in df.iterrows():
    print(row['c1'], row['c2'])
```

## pandas Read CSV

```
# read with utf-8 and utf-16
try: 
    df = pd.read_csv(file, dtype=str, encoding="utf-8", sep="|")
except UnicodeDecodeError:
    df = pd.read_csv(file, dtype=str, encoding="utf-16", sep="|")
```

```
# https://stackoverflow.com/questions/61264795/pandas-unicodedecodeerror-utf-8-codec-cant-decode-bytes-in-position-0-1-in
data = pd.read_csv("COVID-19-geographic-disbtribution-worldwide.csv", encoding = 'unicode_escape', engine ='python')
```

```
# https://stackoverflow.com/questions/55076502/utf-8-codec-cant-decode-byte-0xb5-in-position-0-invalid-start-byte
import pandas as pd  
pd.read_csv(filename,encoding = 'unicode_escape')
```

```
pd.read_csv(parse_dates=['date_col_name']) # parse to datetime type
df['date_col_name'] = df['date_col_name'].dt.date
# other option: see - https://stackoverflow.com/questions/16852911/how-do-i-convert-strings-in-a-pandas-data-frame-to-a-date-data-type
pd.to_datetime(df) # parse str to datetime type
df['date_col_name'] = pd.to_datetime(df['date_col_name'] ).dt.date  # str --> date  
```

## Read string into pandas
```
# https://stackoverflow.com/questions/22604564/create-pandas-dataframe-from-a-string
import pandas as pd
from io import StringIO

df = pd.read_csv(StringIO("""measure,rate_period,Process,Scorecard
ED Visits,Calendar Year,132,188"""),sep=',', header=0)
```

## https://pandas.pydata.org/docs/getting_started/intro_tutorials/index.html
```
# https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html
above_35 = titanic[titanic["Age"] > 35]
```

# Reshape: https://pandas.pydata.org/docs/user_guide/reshaping.html
## Pivot long to wide
```
# https://stackoverflow.com/questions/47152691/how-can-i-pivot-a-dataframe
pd.pivot(
    data=df,        
    index='foo',    # Column to use to make new frame’s index. If None, uses existing index.
    columns='bar',  # Column to use to make new frame’s columns.
    values='baz'    # Column(s) to use for populating new frame’s values.
).reset_index(drop=False)
```

## Pivot wide to long
```
# https://stackoverflow.com/questions/36537945/reshape-wide-to-long-in-pandas
# Setup
df = pd.DataFrame({
    'date' : ['05/03', '06/03', '07/03', '08/03'],
    'AA' : [1, 4, 7, 5],
    'BB' : [2, 5, 8, 7],
    'CC' : [3, 6, 9, 1]
}).set_index('date')

# 
df = df.reset_index()
pd.melt(df, id_vars='date', value_vars=['AA', 'BB', 'CC'], value_name = 'value')

```

```
df = pd.melt(  
        frame=df,  
        id_vars=["id","name"],        # Columns to keep
        value_vars=["var1", "var2"],  # Columns to unpivot
        var_name='name',              # Name for the variable column
        value_name='value',           # Name for the value column
        col_level=None,               # If columns are a MultiIndex, this level will be melted
        ignore_index=True             # If True, original index is ignored
    )
```

```
# 
df.melt(ignore_index=False).reset_index()
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
import pandas as pd  
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

```
# sort by column name
df.sort_values(by = ['Name'])
```


```
# https://www.geeksforgeeks.org/how-to-filter-rows-using-pandas-chaining/
# Using pipe() method
df2 = data.pipe(lambda x: x['Country'] == "India")

# Chaining loc[] operator to filter rows
df2 = data.loc[lambda x: x['ID'] <= 103].loc[lambda x: x['Age'] == 23]


```


```
Select multiple rows and particular columns.
Dataframe.loc[["row1", "row2"...], ["column1", "column2", "column3"...]]
Dataframe.loc[[:, ["column1", "column2", "column3"]]


# Using the operator .iloc[]
# to select single row
result = df.iloc[2]

# Using the operator .iloc[]
# to select multiple rows with
# some particular columns
result = df.iloc[[2, 3, 5], [0, 1]]

# Using the operator .iloc[]
# to select multiple rows
result = df.iloc[[2, 3, 5]]

# Using the operator .iloc[]
# to select all the rows with
# some particular columns
result = df.iloc[:, [0, 1]]

# Using mask and lambda function to filter
df2 = data.mask(lambda x: x['Age'] <= 39)

# select the rows with specific string
# or character value in a particular column
print(data[data.Name.str.contains('am')])

# define the set of values
lst=['Uk','Australia']
 
# select the rows from specific set
# of values in a particular column
print(data[data.Country.isin(lst)])

# subset
# https://stackoverflow.com/questions/17071871/how-do-i-select-rows-from-a-dataframe-based-on-column-values
some_value_list = ['a', 'b', 'b']
df.loc[df['column_name'].isin(some_value_list)]

# Filter rows use query function
# https://www.geeksforgeeks.org/how-to-filter-rows-based-on-column-values-with-query-function-in-pandas/
df.query("Age>13 and Name=='C'")

# ==================================
# Filter based on date
# convert date column into date format
df['date_added'] = pd.to_datetime(df['date_added'])
# filter rows on the basis of date
newdf = (df['date_added'] > '01-03-2020') & (df['date_added'] <= '31-12-2020')
# locate rows and access them using .loc() function
newdf = df.loc[newdf]
# print dataframe
print(newdf)
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
https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html
```
df1.merge(df2, how='left', on='a')
df1.merge(df2, how='cross')
df1.merge(df2, left_on='lkey', right_on='rkey',
          suffixes=('_left', '_right'))
```
# pivot  
## wide to long
## long to wide 

# Encoding error 



# RAF
```
# 1. calculate age
def get_age(x):
    from datetime import datetime
    from datetime import date
    from_service_year = x['member_year']
    to  = str(from_service_year) + '0201'
    to  = datetime.strptime(to, '%Y%m%d')
    dob = datetime.strptime(x['date_birth'], '%Y-%m-%d')
    if dob is None: 
        age = None
    else:
        age = to.year - dob.year - ((to.month, to.day) < (dob.month, dob.day))
#     dob =datetime.strptime(dob, '%Y-%m-%d')
    return age
hcc_dat['age'] = hcc_dat.apply(get_age, axis = 1)
hcc_dat.head(2) 

# 2 
def get_icd10_list(icd10):
    if icd10 is None:
        return []
    return icd10.split("|")
hcc_dat['icd10list_current'] = hcc_dat['diag_current'].apply(get_icd10_list)
hcc_dat['icd10list_past'] = hcc_dat['diag_past'].apply(get_icd10_list)

# 3. 
from functools import partial
from hccpy.hcc import HCCEngine
he = HCCEngine(version = '24')
def get_raf(row, col='icd10list_current'):
    icd10 = row[col]
    age   = row['age']
    sex   = row['gender']
    if not all([age, sex]):
        return None
    rp = he.profile(dx_lst=icd10, age=age, sex=sex, elig="CNA", orec="0", medicaid=False)
    return rp['risk_score']    

# 
hcc_dat['raf_observed']         = hcc_dat.apply(partial(get_raf, col='icd10list_current') , axis = 1)
```

# Ref:  
- https://pandas.pydata.org/docs/getting_started/comparison/comparison_with_sql.html
- https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html

