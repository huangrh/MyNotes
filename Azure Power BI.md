## Python - show table  

```
# Example 1
# 
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Create DataFrame
df = pd.DataFrame({
'team': ['A', 'A', 'A', 'B', 'B', 'B', 'C', 'C', 'C'],
'points': [18, 22, 19, 14, 14, 11, 20, 28, 30],
'assists': [5, 7, 7, 9, 12, 9, 9, 4, 15]
})

# Add table
table = plt.table(
    cellText=df.values,
    rowLabels=df.index,
    colLabels=df.columns,
    animated=True, 
    cellLoc='center', 
    loc='center',
    bbox=(-0.1, -0.1, 1.1, 1.1)
)
table.auto_set_font_size(False)
table.set_fontsize(24)
# table.scale(1.5, 1.5)

# Display final plot
plt.show()
```

- https://stackoverflow.com/questions/60076770/power-bi-dataframe-table-visualization
- 
```
# dataset = pandas.DataFrame(Year, Quarter, Month, Day, LOB, # member)
# dataset = dataset.drop_duplicates()

# Paste or type your script code here:
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

dataset = pd.DataFrame(np.random.randn(10, 8), columns=list('abcdefgh'))
#Round to two digits to print nicely
vals = np.around(dataset.values, 2)
#Normalize data to [0, 1] range for color mapping below
normal = (dataset - dataset.min()) / (dataset.max() - dataset.min())

fig = plt.figure()
ax = fig.add_subplot(111)
ax.axis('off')
the_table=ax.table(cellText=vals, rowLabels=dataset.index, colLabels=dataset.columns, 
                   loc='center', cellColours=plt.cm.RdYlGn(normal),animated=True)

plt.show()
```

## Fabric
- https://learn.microsoft.com/en-us/fabric/enterprise/licenses  

## incremental-refresh-overview
https://learn.microsoft.com/en-us/power-bi/connect-data/incremental-refresh-overview  

## Power BI Embedding
- embed a report with react js:
	- https://learn.microsoft.com/en-us/javascript/api/overview/powerbi/powerbi-client-react
- Embed a report web part in SharePoint Online  
	- https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-embed-report-spo  
- https://docs.microsoft.com/en-us/power-bi/developer/embedded/  
- [Embed a report web part in SharePoint Online](https://docs.microsoft.com/en-us/power-bi/collaborate-share/service-embed-report-spo)  
- https://learn.microsoft.com/en-us/power-bi/developer/embedded/embedded-analytics-power-bi
- https://www.imensosoftware.com/blog/power-bi-embedded-examples/
- 
## How to add reference line  
- https://youtu.be/74mk6NL7o6o?si=775KCN2_YjyIizNU  

## Power BI Premiun Pricing   

- https://www.saasworthy.com/product/microsoft-power-bi/pricing   
- https://learn.microsoft.com/en-us/power-bi/connect-data/refresh-troubleshooting-refresh-scenarios
- 
```
Scheduled refresh time-out
Scheduled refreshes for imported semantic models time out after two hours. This time-out is increased to five hours for semantic models in Premium workspaces. If you encounter this limit, consider reducing the size or complexity of your semantic model, or consider refactoring the large semantic model into multiple smaller semantic models.
```
## Power BI Service usage
- https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-modern-usage-metrics

## How-to-add-week-to-date-hierarchy-in-power-bi
- https://www.thecoderscamp.com/how-to-add-week-to-date-hierarchy-in-power-bi/
- 
## How to Create a Report Bookmark
- https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-bookmarks?tabs=powerbi-desktop
- 
## Create Custom Navigation Tabs to your Power BI Reports EASILY using this trick // Power BI Guide
- https://www.youtube.com/watch?v=O8jhZRXL4x8
- 
## How to create a Export to Excel / CSV button in Power BI
- https://www.youtube.com/watch?v=BnTipbooeP0
- 
## Create page and bookmark navigators   
https://learn.microsoft.com/en-us/power-bi/create-reports/button-navigators?tabs=powerbi-desktop

## Add Power BI in PowerPoint
https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-power-bi-powerpoint-add-in-about

## DAX
https://learn.microsoft.com/en-us/dax/dax-function-reference  

## Export Power BI report to PDF using Power Automate with filters applied
- power bi report to pdf
- https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-automate-power-bi-report-export
- https://learn.microsoft.com/en-us/power-automate/trigger-flow-powerbi-report  
- https://wisedatadecisions.com/2021/01/18/filter-email-power-bi-report-pages-using-power-automate-excel/  
- https://community.powerbi.com/t5/Service/Export-Power-BI-report-to-PDF-using-Power-Automate-with-filters/m-p/2885326
	- The report MUST be backed by Premium capacity (not Premium per User capacity).
- [How to use Power BI Rest API to export the Power BI Report in PDF format](https://www.bing.com/ck/a?!&&p=ddc45031b94257d8JmltdHM9MTY3Mjc5MDQwMCZpZ3VpZD0wYWYyMzMxYy05OTA3LTYxNWUtMzdhOS0yMjYxOTg3OTYwMGUmaW5zaWQ9NTQyOA&ptn=3&hsh=3&fclid=0af2331c-9907-615e-37a9-22619879600e&psq=power+bi+report+to+pdf&u=a1aHR0cHM6Ly9jb21tdW5pdHkucG93ZXJiaS5jb20vdDUvQ29tbXVuaXR5LUJsb2cvSG93LXRvLXVzZS1Qb3dlci1CSS1SZXN0LUFQSS10by1leHBvcnQtdGhlLVBvd2VyLUJJLVJlcG9ydC1pbi9iYS1wLzIxNzk3NDEjOn46dGV4dD0xJTIwU3RlcCUyMDElM0ElMjBFeGVjdXRlJTIwdGhlJTIwZmlyc3QlMjBSZXN0JTIwQVBJLHRoZSUyMFBERiUyMGZpbGUlMjBvZiUyMHRoZSUyMFBvd2VyJTIwQkklMjBSZXBvcnQu&ntb=1)
	- This method cannot be run in Pro and PPU, it is only suitable for implementation in premium workspace
	- https://learn.microsoft.com/en-us/rest/api/power-bi/reports/get-file-of-export-to-file-in-group  

## Pareto Chart
- https://community.powerbi.com/t5/Community-Blog/How-To-Make-A-Pareto-Chart-In-Power-BI-Step-By-Step-Tutorial/ba-p/2302774  

- 

## service-endorse-content#request-content-certification

- https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-endorse-content#request-content-certification

## Power BI Visuals  
- [Example: How to create a new visuals](https://learn.microsoft.com/en-us/power-bi/developer/visuals/create-bar-chart?tabs=CreateNewVisual)  
- https://learn.microsoft.com/en-us/power-bi/developer/visuals/create-react-visual 
- https://learn.microsoft.com/en-us/power-bi/developer/visuals/environment-setup?tabs=windows
- https://learn.microsoft.com/en-us/power-bi/developer/visuals/context-menu?tabs=EmptySpace


## Conditional Format  
- [Conditional Formatting | ROW by ROW color scale in a MATRIX in PowerBI](https://www.youtube.com/watch?v=wTRrskQzAHk&t=937s)
- https://community.powerbi.com/t5/Desktop/Matrix-conditional-format-Background-color-each-cell/m-p/1258851#M554084
- 
## Resources: 
- https://github.com/Microsoft/PowerBI-visuals-wordcloud

- GitHub: https://github.com/Microsoft/PowerBI-visuals-bulletchart

- Linear Gauge by MAQ Software

- [Concatenatex with distinct column](https://community.powerbi.com/t5/DAX-Commands-and-Tips/Concatenatex-with-Distinct-column-values/m-p/340244#M309)

```
Measures = CALCULATE(CONCATENATEX(DISTINCT(table_measure[measure]), (tablemeasure[measure]), ", "))
```

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

```
// Source uat as 
= Databricks.Catalogs(uat_hoster, uat_http, [Catalog=null, Database="cldw", EnableExperimentalFlagsV1_1_0=null])
// Navigation to data_source table
= Source{[Item="data_source",Schema="cldw",Catalog="hive_metastore"]}[Data]

/////////////////////////////////////////////////////
// Example 2: connect databricks
// Source
= Databricks.Catalogs(uat_host, uat_path, [Catalog=null, Database=null, EnableExperimentalFlagsV1_1_0=null])
// Navigation
= cldw_Schema{[Name="data_source",Kind="Table"]}[Data]
```


- https://docs.microsoft.com/en-us/powershell/power-bi/overview?view=powerbi-ps


- [transpose-table-in-report-visual-in-power-bi-desktop](https://stackoverflow.com/questions/44053408/transpose-table-in-report-visual-in-power-bi-desktop)

- [Cross Report Drill Through](https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-cross-report-drill-through?tabs=powerbi-desktop)



- https://docs.microsoft.com/en-us/power-bi/report-server/install-report-server


- [Connect Azure Databricks Delta tables with Power BI  ](https://www.youtube.com/watch?v=f_4dvWLrjnk&t=2s)


- [Build a Slicer Panel in Power BI and take it to the next level](https://www.youtube.com/watch?v=xy9nmSQeUWg&list=PLv2BtOtLblH1IJqcqSuMTyvEi7W-laWti&index=6)

- [Microsoft Power BI bookmark, selection, button](https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-bookmarks?tabs=powerbi-desktop)

- [How to Create and Publish an App to Power BI Service](https://www.youtube.com/watch?v=yJEAEHj617w)




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
// A more advanced example
/////////////////////////////////////////////////////////////////
// 1. Market
var Market_Restriction = CALCULATETABLE(
		VALUES(user[Market_RLS]),
		user[user_email] = USERPRINCIPALNAME()
	)

var RLS_Market = SWITCH(
		TRUE(),
		CONTAINSROW(Market_Restriction, "None"), TRUE(),
		CONTAINSROW(Market_Restriction, data_source[Market]), TRUE(),
		FALSE()
)

//////////////////////////////////////////////////////////////////
// 2. Region
var Region_Restriction = CALCULATETABLE(
		VALUES(user[Region_RLS]),
		user[user_email] = USERPRINCIPALNAME()
	)

var RLS_Region = SWITCH(
		TRUE(),
		CONTAINSROW(Region_Restriction, "None"), TRUE(),
		CONTAINSROW(Region_Restriction, data_source[Region]), TRUE(),
		FALSE()
)

///////////////////////////////////////////////////////////////////
// 3. Zone
var Zone_Restriction = CALCULATETABLE(
		VALUES(user[Zone_RLS]),
		user[user_email] = USERPRINCIPALNAME()
	)

var RLS_Zone = SWITCH(
		TRUE(),
		CONTAINSROW(Zone_Restriction, "None"), TRUE(),
		CONTAINSROW(Zone_Restriction, data_source[Zone]), TRUE(),
		FALSE()
)

///////////////////////////////////////////////////////////////////
// 4. Practice
var Practice_Restriction = CALCULATETABLE(
		VALUES(user[Practice_RLS]),
		user[user_email] = USERPRINCIPALNAME()
	)

var RLS_Practice = SWITCH(
		TRUE(),
		CONTAINSROW(Practice_Restriction, "None"), TRUE(),
		CONTAINSROW(Practice_Restriction, data_source[Practice]), TRUE(),
		FALSE()
)

var RLS_Rrestriction = 
RLS_Market &&
RLS_Region && 
RLS_Zone   && 
RLS_Practice

Return RLS_Rrestriction
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

# Power BI App Update

https://powerbi.microsoft.com/en-us/blog/announcing-public-preview-of-multiple-audiences-for-power-bi-apps/

# Power BI Licenses
- Users with free licenses can use the Power BI service to connect to data and create reports and dashboards for their own use.  
- They can't use the Power BI sharing or collaborating features with others, or publish content to other people's workspaces.   
- However, Pro and PPU users can share content and collaborate with free users if the content is saved in workspaces hosted in **Premium capacity**.     
https://learn.microsoft.com/en-us/power-bi/fundamentals/service-features-license-type

