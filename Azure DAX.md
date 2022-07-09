# Data Analysis Expressions (DAX)

```
EVALUATE 
VAR CurrentLoggedIn = USERPRINCIPALNAME()
Return
ADDCOLUMNS(
Employees_table,
"Include_col", CurrentLoggedIn
)
```


```
# -------------------------------------
# Add hierachical 
Pathfull = PATH(Employee_ID, Manager_ID)
```

## Reference
- [Data Analysis Expressions (DAX)](https://docs.microsoft.com/en-us/dax/switch-function-dax)
