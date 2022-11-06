https://www.w3schools.com/googlesheets/google_sheets_countif.php
```
> countif
> transpose
> query
> importrange
> =TRANSPOSE(QUERY(IMPORTRANGE("1XgIU6-wBzvwpGuOKVmd_iTt0aDqmruhy_5ax8iSmF7E","'sheet4'!A1:Z"), "select Col2, Col1 where Col1='a'",1))

```

```
=query(
    transpose(
        query(
            IMPORTRANGE("1BtDXqUvFD8379CdYwCoRzEDNi6Xxjhctkjry5z0CCdc","'2022-Qtr 4'!A1:Z20"),
        "SELECT * WHERE Col1='Lunch'", 1)
    ),
"SELECT * WHERE Col2 <> '' AND Col1 >= '"&TEXT(TODAY(),"yyyy-mm-dd")&"'",0)
```
