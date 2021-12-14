1. UDF

```sql
-- Median
-- https://hoffa.medium.com/new-in-bigquery-persistent-udfs-c9ea4100fd83
-- 
-- Below is an example
-- SELECT year
--   , fhoffa.x.median(ARRAY_AGG(weight_pounds)) as median_weight
-- FROM `bigquery-public-data.samples.natality`
-- GROUP BY 1
-- ORDER BY 1
CREATE OR REPLACE FUNCTION `fhoffa`.x.median(arr ANY TYPE) AS ((
  SELECT IF (
    MOD(ARRAY_LENGTH(arr), 2) = 0,
    (arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2) - 1)] + arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))]) / 2,
    arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))]
  )
  FROM (SELECT ARRAY_AGG(x ORDER BY x) AS arr FROM UNNEST(arr) AS x)
));

```



```sql
# get Age
CREATE OR REPLACE FUNCTION `my-project.my_udf.get_age`(date_birth DATE, to_date DATE)
OPTIONS (description="Get Age from date birth by Ren-Huai Huang. 
Ref: https://towardsdatascience.com/how-to-accurately-calculate-age-in-bigquery-999a8417e973") AS (
DATE_DIFF(to_date, date_birth, YEAR) - 
  IF(EXTRACT(MONTH FROM date_birth)*100 + EXTRACT(DAY FROM date_birth) > EXTRACT(MONTH FROM to_date)*100 + EXTRACT(DAY FROM to_date), 1, 0)
);
```

2. get average and median at the same time


```sql
  SELECT
    DISTINCT mpi,
    year,
    COUNT(*) OVER (PARTITION BY mpi, year)           test_count,
    MIN(numeric_value) OVER (PARTITION BY mpi, year) egfr_min,
    MAX(numeric_value) OVER (PARTITION BY mpi, year) egfr_max,
    AVG(numeric_value) OVER (PARTITION BY mpi, year) egfr_avg,
    --     PERCENTILE_DISC(numeric_value, 0) OVER (partition by mpi, year)   egfr_min,
    PERCENTILE_DISC(numeric_value, 0.5) OVER (PARTITION BY mpi, year)        egfr_median,
    --     PERCENTILE_DISC(numeric_value, 1) OVER (partition by mpi, year)   egfr_max
  FROM 
    dat
```
