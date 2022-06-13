## [Hash each value in a pandas data frame](https://stackoverflow.com/questions/30143911/hash-each-value-in-a-pandas-data-frame)  

```
import pandas as pd
df = pd.DataFrame({'a':['asds','asdds','asdsadsdas', '123']})
df["hash"] = pd.util.hash_array(df["a"].to_numpy())
df["hash2"] = pd.util.hash_array(df["a"].to_numpy())
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



