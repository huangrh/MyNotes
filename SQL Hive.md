```
# Key words
, concat_ws("|", sort_array(collect_set(FromDate))) From_Date
, size(collect_set(FromDate)) size
```

```
# string to date
TO_DATE(from_unixtime(unix_timestamp(CAST(OnsetDateKey AS STRING) ,  'yyyyMMdd')))

```
