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

# look up the value in the Employee_id_filed if email_address_field = "name@email.com"
LOOKUPVALUE(Employee_id_field, email_address_field, "name@email.com")
```

## Reference
- [Data Analysis Expressions (DAX)](https://docs.microsoft.com/en-us/dax/switch-function-dax)
- [https://www.sqlbi.com/articles/the-in-operator-in-dax/](https://www.sqlbi.com/articles/the-in-operator-in-dax/)
