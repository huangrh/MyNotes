
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
