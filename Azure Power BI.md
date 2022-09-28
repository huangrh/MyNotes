## Resources: 

- [how to sort by column in power bi slider](https://community.powerbi.com/t5/Desktop/Sorting-the-Slicer/m-p/24570#M7951)

- [comet chart in R](https://gist.github.com/zanarmstrong/6c2855a34f504029847485c690692e75)

- [Change how visuals interact in a Power BI report](https://learn.microsoft.com/en-us/power-bi/create-reports/service-reports-visual-interactions?tabs=powerbi-desktop)

- [HierarchySlicer](https://appsource.microsoft.com/en-us/product/power-bi-visuals/WA104380820?tab=Overview)

- [sparkline in a table ](https://powerbi.microsoft.com/en-us/blog/power-bi-december-2021-feature-summary/#post-18168-_Toc89785231)

- https://www.tutorialgateway.org/add-data-bars-to-table-in-power-bi/

- https://community.powerbi.com/t5/Desktop/Integrate-a-graph-in-a-table-column/m-p/650661

- https://docs.microsoft.com/en-us/power-bi/admin/organizational-visuals (add outside visuals to organization with global administration)

- https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-drillthrough
- https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-drillthrough

- [Add hyperlinks (URLs) to a table or matrix](https://docs.microsoft.com/en-us/power-bi/create-reports/power-bi-hyperlinks-in-tables?tabs=powerbi-desktop)

- [change-the-source-of-power-bi-datasets-dynamically-using-power-query-parameters](https://radacad.com/change-the-source-of-power-bi-datasets-dynamically-using-power-query-parameters)


- https://docs.microsoft.com/en-us/powershell/power-bi/overview?view=powerbi-ps


- [transpose-table-in-report-visual-in-power-bi-desktop](https://stackoverflow.com/questions/44053408/transpose-table-in-report-visual-in-power-bi-desktop)

- [Cross Report Drill Through](https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-cross-report-drill-through?tabs=powerbi-desktop)

- https://docs.microsoft.com/en-us/power-bi/developer/embedded/

- https://docs.microsoft.com/en-us/power-bi/report-server/install-report-server


- [Connect Azure Databricks Delta tables with Power BI  ](https://www.youtube.com/watch?v=f_4dvWLrjnk&t=2s)


- [Build a Slicer Panel in Power BI and take it to the next level](https://www.youtube.com/watch?v=xy9nmSQeUWg&list=PLv2BtOtLblH1IJqcqSuMTyvEi7W-laWti&index=6)

- [Microsoft Power BI bookmark, selection, button](https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-bookmarks?tabs=powerbi-desktop)

- [How to Create and Publish an App to Power BI Service](https://www.youtube.com/watch?v=yJEAEHj617w)


- [Embed a report web part in SharePoint Online](https://docs.microsoft.com/en-us/power-bi/collaborate-share/service-embed-report-spo)

###  [Row Level Security   ](https://docs.microsoft.com/en-us/power-bi/enterprise/service-admin-rls)

- [youtube: Dynamic Row-level Security 🔐 - Based on Dimension Tables](https://www.youtube.com/watch?v=Vc_5Jo6DyH8)
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


- [Youtube: Row Level Security With Hierarchies](https://www.youtube.com/watch?v=oPwDkgPU9uc)
```
VAR Current_user = lookupvalue(
  Employees[Employee_ID],
  Employees[User_Name], userprincipalname()
)

Restrictions = Pathcontains(Employeess[PathFull], Current_user)
Return Restrictions
```

- [Youtube: Azure Power BI DAX](https://www.youtube.com/watch?v=QJw4HkagVWc)  

- https://docs.microsoft.com/en-us/dax/username-function-dax


- [Upgrade Your REPORT DESIGN in Power BI | Complete Walkthrough From A to Z](https://www.youtube.com/watch?v=Lfzu74XDyco&t=4s)

- https://colorhunt.co/

- https://coolors.co/palettes/trending  

