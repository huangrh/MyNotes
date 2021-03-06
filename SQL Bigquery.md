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

# The difference between PERCENTILE_CONT and PERCENTILE_DISC
# https://stackoverflow.com/questions/23585667/percentile-disc-vs-percentile-cont
# PERCENTILE_DISC returns a value in your set/window, whereas PERCENTILE_CONT will interpolate;
#  the idea was the PERCENTILE_DISC() was supposed to be for discrete ranges, while PERCENTILE_CONT() was supposed to be for continuous ranges.
    PERCENTILE_DISC(numeric_value, 0.5) OVER (PARTITION BY mpi, year)        egfr_median,
    PERCENTILE_CONT(numeric_value, 0.5) OVER (PARTITION BY mpi, year)        egfr_median,
    --     PERCENTILE_DISC(numeric_value, 1) OVER (partition by mpi, year)   egfr_max
  FROM 
    dat
```


```

```



3. eGFR 
```sql
-- example: select `project_id`.dataset.get_eGFR2009(1.99,'Fe',43, 'AFRICAN AMERICAN') -- eGFR = 46.28988
--  
CREATE OR REPLACE FUNCTION `project_id.dataset.get_eGFR2009`(creatinine NUMERIC, sex STRING, age INT64, race STRING) RETURNS FLOAT64 AS (
(
    WITH
      v1 AS (
      SELECT
      IF(REGEXP_CONTAINS(UPPER(sex), '^F$|FEMALE'), 0.7,   0.9) k,
      IF(REGEXP_CONTAINS(UPPER(sex), '^F$|FEMALE'),-0.329, -0.411) a ,
      IF(REGEXP_CONTAINS(UPPER(sex), '^F$|FEMALE'),1.018,  1) female_adj,
      IF(REGEXP_CONTAINS(UPPER(race),'BLACK|AFRICAN AMERICAN'), 1.159, 1) race_adj,
      ----------------------------------------
      ),
      v2 AS (
      SELECT
        IF(creatinine/k >1, 1, creatinine/k) min_cr,
        IF(creatinine/k <1, 1, creatinine/k) max_cr,
        v1.*
      FROM
        v1 
        ---------------------------------------
        )
      --final result
    SELECT
      141  * POW(min_cr,a) * POW(MAX_CR,-1.209) * POW(0.993,age) * female_adj * race_adj
    FROM
      v2 )
);
```

4. Pivot from long to wide  

```
# https://stackoverflow.com/questions/26272514/how-to-pivot-table-in-bigquery
# 
with Produce AS (
  SELECT 'Kale' as product, 51 as sales, 'Q1' as quarter UNION ALL
  SELECT 'Kale', 23, 'Q2' UNION ALL
  SELECT 'Kale', 45, 'Q3' UNION ALL
  SELECT 'Kale', 3, 'Q4' UNION ALL
  SELECT 'Apple', 77, 'Q1' UNION ALL
  SELECT 'Apple', 0, 'Q2' UNION ALL
  SELECT 'Apple', 25, 'Q3' UNION ALL
  SELECT 'Apple', 2, 'Q4')
SELECT * FROM
  (SELECT product, sales, quarter FROM Produce)
  PIVOT(SUM(sales) FOR quarter IN ('Q1', 'Q2', 'Q3', 'Q4'))
# 
with Produce AS (
  SELECT 'Kale' as product, 51 as sales, 'Q1' as quarter UNION ALL
  SELECT 'Kale', 23, 'Q2' UNION ALL
  SELECT 'Kale', 45, 'Q3' UNION ALL
  SELECT 'Kale', 3, 'Q4' UNION ALL
  SELECT 'Apple', 77, 'Q1' UNION ALL
  SELECT 'Apple', 0, 'Q2' UNION ALL
  SELECT 'Apple', 25, 'Q3' UNION ALL
  SELECT 'Apple', 2, 'Q4')

SELECT product, sales, quarter FROM Produce
  PIVOT(SUM(sales) FOR quarter IN ('Q1', 'Q2', 'Q3', 'Q4'))
```

```
# https://towardsdatascience.com/pivot-in-bigquery-4eefde28b3be
SELECT * FROM
(
  -- #1 from_item
  SELECT 
    airline,
    departure_airport,
    departure_delay
  FROM `bigquery-samples.airline_ontime_data.flights`
)
PIVOT
(
  -- #2 aggregate
  AVG(departure_delay) AS avgdelay
  -- #3 pivot_column
  FOR airline in ('AA', 'KH', 'DL', '9E')
)
```

```
# https://towardsdatascience.com/pivot-in-bigquery-4eefde28b3be
# dynamic 
DECLARE airlines STRING;
SET airlines = (
  SELECT 
    CONCAT('("', STRING_AGG(DISTINCT airline, '", "'), '")'),
  FROM `bigquery-samples.airline_ontime_data.flights`
);

EXECUTE IMMEDIATE format("""
SELECT * FROM
(
  SELECT 
    airline,
    departure_airport,
    departure_delay
  FROM `bigquery-samples.airline_ontime_data.flights`
)
PIVOT
(
  AVG(departure_delay) AS avgdelay
  FOR airline in %s
)
ORDER BY departure_airport ASC
""", airlines);
```

# Dedup
```
ROW_NUMBER() OVER (
        PARTITION BY  key 
        ORDER BY date DESC
                , value DESC) ranknum
```

# How to write to bigquery from python
```
# https://stackoverflow.com/questions/63200201/create-a-bigquery-table-from-pandas-dataframe-without-specifying-schema-explici
import pandas as pd
from google.cloud import bigquery
df = pd.DataFrame({'a': [1,2,4], 'b': ['123', '456', '000']})
table = "dataset_name.table_name"
client = bigquery.Client(location = "US", project="project_name")
job = client.load_table_from_dataframe(df, table, location="US")
```

# Find census tract from x,y-coordinates by bigquery

```
# ST_COVERS
SELECT
  
  dat.address_latitude,
  dat.address_longitude,
  tract.geo_id,
  svi.socioeconomic,
  svi.household_composition_disability,
  svi.minority_status_lang,
  svi.housing_type_transportation,
  svi.svi,

FROM
  dat
JOIN
  `bigquery-public-data.geo_census_tracts.us_census_tracts_national` tract
ON
  ST_COVERS(tract.tract_geom,
            st_geogpoint(dat.address_longitude,dat.address_latitude))
LEFT JOIN
  -- https://www.atsdr.cdc.gov/placeandhealth/svi/data_documentation_download.html
  us_svi2018 svi
ON
  svi.fips = tract.geo_id

```
