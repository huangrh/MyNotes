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



