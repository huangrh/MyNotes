
```
# GREATEST
with data as (
select NUll a , 1 b
union all
select 1 a, 2 b
union all 
select 2 a , 3 b
)

select a, b, greatest(a, b) c from data
order by a is null, a desc
```


```
regexp_extract(session, '([^\-]+)$', 1)
```


```
# Key words
, concat_ws("|", sort_array(collect_set(FromDate))) From_Date
, size(collect_set(FromDate)) size
```

```
# string to date
TO_DATE(from_unixtime(unix_timestamp(CAST(OnsetDateKey AS STRING) ,  'yyyyMMdd')))

```


```
# Hive: Long to Wide
select YEAR_LAST_SEEN, sum(group_map['key1']) Key01, sum(group_map['key2']) Key02
from
( 
        select YEAR_LAST_SEEN, 
        map(practice ,  UNIQUE_PATIENT) as group_map 
        from dat
 ) a 
 group by a.YEAR_LAST_SEEN
 order by year_last_seen
```


# https://stackoverflow.com/questions/41758949/difference-between-translate-and-regexp-replace
```
spark.sql("SELECT TRANSLATE('ed-ba', 'abcde', '12345')").show()

```
