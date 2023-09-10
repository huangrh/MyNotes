# Dax function  

- https://www.sqlbi.com/articles/introducing-calculate-in-dax/
- https://www.sqlbi.com/articles/row-context-in-dax/
    - calculated column: row by row
    - iterator scan the table row by row
        - iterator: sumx
        - sum is same as sumx
- https://www.sqlbi.com/articles/filter-context-in-dax/
    - filter context: each cell of the matrix is evaluated ina a filter context
    - the filter context is set for the entire formula, restricting the rows visible in the model.
    - Developers have the option of creating their filter context too, by leveraging CALCULATE
    -  The row context iterates a table. Therefore, during the iteration you can access the value of each column of only the iterated table.
    -  The filter context, on the other hand, operates at the model level.

- SUMMARIZECOLUMNS
- https://www.geeksforgeeks.org/power-bi-values-function/
- https://www.sqlbi.com/articles/understanding-context-transition-in-dax/
- 
# Pareto:  column chart - line and clusted. 
```
// count
# CPT = CALCULATE(COUNTA(Bill[CPT]))
// %Pareto line 
% Pareto = 

VAR CPT_Count = [# CPT]
VAR Total_CPT_Count = CALCULATE([# CPT],ALL(Bill[CPT]))
VAR CUM_CPT_Count   = CALCULATE(
    [# CPT], 
    FILTER(ALL(Bill[CPT]), [# CPT] >= CPT_Count)
    )
VAR CUM_CPT_Pct = CUM_CPT_Count / Total_CPT_Count

RETURN CUM_CPT_Pct 
```

```
Total Appointments = DISTINCTCOUNT(fact_appointments[APPOINTMENT_ID])
# 
Completed Appointments = CALCULATE(
    DISTINCTCOUNT(fact_appointments[APPOINTMENT_ID]),
    fact_appointments[FINAL_APPOINTMENT_STATUS] = "Completed"
)
#
Final Budgeted Appointments = SUM(dim_date[Budgeted Visits])
# 
Appointment Month Year = FORMAT(fact_appointments[APPOINTMENT_DATE], "MMM YYYY")
```

```
# 
# CPT = CALCULATE(COUNTA(fact_rcm[BILL_CODE]))

# 
% CPT Code Running CPT Count = 

VAR CPT_Count = [# CPT] /* this sets the charges for the current category 
                                          (the one that is the current column in the column chart) */

VAR Total_CPT_Count =
    CALCULATE([# CPT], ALL(dim_cpt[CPTCODE])) /* the total charges  
                                              This is needed to turn the absolute margin into a percentage */

RETURN
    CALCULATE(
        [# CPT],
        FILTER(ALL(dim_cpt[CPTCODE]), [# CPT] >= CPT_Count)
    ) / Total_CPT_Count
```
# DAX Studio
- https://daxstudio.org/
- [How to install dax-studio on windows](https://blog.enterprisedna.co/how-to-install-dax-studio-tabular-editor-in-power-bi/)
- 

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

## https://dax.guide/calculatetable/

## [Dax Studio](https://dax.do/qH3yzC4dtmSXhd/)
