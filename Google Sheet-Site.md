https://www.w3schools.com/googlesheets/google_sheets_countif.php
```
> countif
> transpose
> query
> importrange
> =TRANSPOSE(QUERY(IMPORTRANGE("1XgIU6-wBzvwpGuOKVmd_iTt0aDqmruhy_5ax8iSmF7E","'sheet4'!A1:Z"), "select Col2, Col1 where Col1='a'",1))

```

```
# Google sheet query example
=query(
    transpose(
        query(
            IMPORTRANGE("《google sheet id》","'2022-Qtr 4'!A1:Z20"),
        "SELECT * 
        WHERE Col1 matches '.*Lunch.*'",
        1)
    ),
"SELECT * 
WHERE  Col1 >= '"&TEXT(TODAY(),"yyyy-mm-dd")&"' 
order by Col1 
limit 6",
0)
```

```
# Embedding google sheet without board. 
<iframe 
    src="https://drive.google.com/file/d/《id》/preview" 
    frameborder="0" 
    style="overflow:hidden;height:100%;width:100%" 
    height="100%" 
    width="100%" 
    allow="autoplay">
</iframe>
```

https://nathannagele.com/how-to-remove-choices-in-google-forms/


