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

