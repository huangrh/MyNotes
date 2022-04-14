# Useful Resource
[eGFR](https://cran.r-project.org/web/packages/transplantr/vignettes/egfr.html)


# Get Age
https://stackoverflow.com/questions/3611314/calculate-ages-in-r
```
get_age = function(from, to) {
  from_lt = as.POSIXlt(from)
  to_lt   = as.POSIXlt(to)

  age     = to_lt$year - from_lt$year

  ifelse(to_lt$mon < from_lt$mon |
         (to_lt$mon == from_lt$mon & to_lt$mday < from_lt$mday),
         age - 1, age)
}
```
