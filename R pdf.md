update 2024
- https://docs.ropensci.org/tabulapdf/index.html
```
2
```
- 
```
library(tabulizer)

# Extract the table
tabel <- extract_tables('file.pdf', pages = 60)
# Extract the first element of the variable
View(tabel[[1]])

# 
extract_areas('file.pdf', pages=60)

# 
cat(extract_text('file.pdf', pages=6), sep="\n")

# 
get_n_pages("file.pdf")

# 
get_page_dims("file.pdf", pages=1)
```


# https://levelup.gitconnected.com/extract-tables-and-text-from-pdf-files-in-r-3b7a07d2f5b6
