## [change-the-source-of-power-bi-datasets-dynamically-using-power-query-parameters](https://radacad.com/change-the-source-of-power-bi-datasets-dynamically-using-power-query-parameters)


## https://docs.microsoft.com/en-us/powershell/power-bi/overview?view=powerbi-ps


## [transpose-table-in-report-visual-in-power-bi-desktop](https://stackoverflow.com/questions/44053408/transpose-table-in-report-visual-in-power-bi-desktop)

## [Cross Report Drill Through](https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-cross-report-drill-through?tabs=powerbi-desktop)

## https://docs.microsoft.com/en-us/power-bi/developer/embedded/

## https://docs.microsoft.com/en-us/power-bi/report-server/install-report-server


## Connect Azure Databricks Delta tables with Power BI  
- 
https://www.youtube.com/watch?v=f_4dvWLrjnk&t=2s


## Build a Slicer Panel in Power BI and take it to the next level
https://www.youtube.com/watch?v=xy9nmSQeUWg&list=PLv2BtOtLblH1IJqcqSuMTyvEi7W-laWti&index=6

## Microsoft Power BI bookmark, selection, button
https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-bookmarks?tabs=powerbi-desktop

## 
https://www.youtube.com/watch?v=yJEAEHj617w   
How to Create and Publish an App to Power BI Service  


## Embed a report web part in SharePoint Online
https://docs.microsoft.com/en-us/power-bi/collaborate-share/service-embed-report-spo

## Row Level Security   
https://docs.microsoft.com/en-us/power-bi/enterprise/service-admin-rls  

[youtube: Dynamic Row-level Security 🔐 - Based on Dimension Tables](https://www.youtube.com/watch?v=Vc_5Jo6DyH8)
```
var _restriction = 
calculatetable(
values(users[rls_production]),
users[email]=userprinciapalname()
)

RLS_restriction = 
switch(
true(),
_restriction = 'None', True(),
[group] = _restriction, True() ,
False()
)

return 

rls_restriction
```

```
The role is straigtforward - '[e-mail] = userprincipalname()'
```


## [Youtube: Row Level Security With Hierarchies](https://www.youtube.com/watch?v=oPwDkgPU9uc)
```
VAR Current_user = lookupvalue(
  Employees[Employee_ID],
  Employees[User_Name], userprincipalname()
)

Restrictions = Pathcontains(Employeess[PathFull], Current_user)
Return Restrictions
```

## [Youtube: Azure Power BI DAX](https://www.youtube.com/watch?v=QJw4HkagVWc)  

## https://docs.microsoft.com/en-us/dax/username-function-dax


## [Upgrade Your REPORT DESIGN in Power BI | Complete Walkthrough From A to Z](https://www.youtube.com/watch?v=Lfzu74XDyco&t=4s)

## https://colorhunt.co/

## https://coolors.co/palettes/trending  

