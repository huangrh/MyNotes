```
# Change file path format from windows to R
winpath2r <- function() {
    gsub("\\\\", "/", readClipboard())
}
```


# Snow Flake DSN Connection R odbc  
- https://hevodata.com/learn/snowflake-r/  
- https://dbi.r-dbi.org/reference/dbgetquery  
- https://stackoverflow.com/questions/55161683/does-snowflake-support-ssl  
- https://stackoverflow.com/questions/63453843/does-snowflake-support-ssl-using-odbc



```
# DSN-less ODBC Connections 
conn <- DBI::dbConnect(
  odbc::odbc(),
  Driver = "SnowflakeDSIIDriver",
  server = "myorganization-myaccount.snowflakecomputing.com",
  uid = "Username", 
  database = "My_Snowflake_Database_Name",
  schema = "My_Snowflake_Schema",
  warehouse = "My_Snowflake_Virtual_Warehouse",
  role = "My_Snowflake_Role",
  authenticator = "snowflake",
  pwd = "Password"
)
```

# https://github.com/Azure/AzureR

# https://bookdown.org/yihui/rmarkdown/html-document.html

---
title: Habits
author: John Doe
date: March 22, 2005
output: html_document
---


# Optimize 1-d function
https://stackoverflow.com/questions/17013612/how-to-minimize-a-function-over-one-input-parameter-in-r  
https://statisticsglobe.com/optimize-function-in-r/  
```
my_function <- function(x) {             # Create function
  x^3 + 2 * x^2 - 10 * x
}

optimize(my_function,                    # Apply optimize
         interval = c(- 1, 1))
# $minimum
# [1] 0.999959
# 
# $objective
# [1] -6.999877
```

# copy file to adls gen2
https://github.com/Azure/AzureRMR    
https://mran.microsoft.com/web/packages/AzureStor/vignettes/intro.html    
https://github.com/yueguoguo/r-on-azure   
```r
library(AzureRMR)
require(AzureStor)
# if (!exists('az')) {
#   az <- create_azure_login()
#   az$token$refresh()
# }
# az <- create_azure_login()
az   = get_azure_login()
az$token$refresh()
token <- AzureRMR::get_azure_token("https://storage.azure.com", tenant="my tenant id", app=az$token$client$client_id)
# 
ad_endp_tok2 <- AzureStor::storage_endpoint("https://my_storage_name.dfs.core.windows.net/", token=token)
# Set up container connection 
cont <- AzureStor::storage_container(ad_endp_tok2, "my_container_name")
# 
AzureStor::list_storage_files(cont)
# 
src =  file.path("C:/O365/OneDrive/Azure_Landing", "src_file_name.csv")
# 
storage_upload(cont, src, "share/path/target_file_name.csv")
```

# ggplot2
- [bar plot  ](http://www.sthda.com/english/wiki/ggplot2-barplots-quick-start-guide-r-software-and-data-visualization)
```
g = ggplot(hcc_raf, aes(x = month_only, y = oe, fill=year) ) + 
  geom_bar(stat="identity", color="black", position="dodge2")+
  theme_minimal()
g + scale_fill_manual(values=c('#999999','#E69F00'))
```

```
# Add space between the panel so the overlap 
# ref: https://stackoverflow.com/questions/3681647/ggplot-how-to-increase-spacing-between-faceted-plots
facet_wrap(~measure, nrow = 2, scales="free") + theme_classic() + theme(panel.spacing.y = unit(2, "lines"))

# Change spacing between facets on both axis
p + theme(panel.spacing = unit(2, "lines"))

# Change horizontal spacing between facets
p + theme(panel.spacing.x = unit(2, "lines"))

# Change vertical spacing between facets
p + theme(panel.spacing.y = unit(2, "lines"))
```

# strsplit and unnest
- R: Split comma-separated strings in a column into separate rows
```r
a dplyr / tidyr combination:

library(dplyr)
library(tidyr)
v %>% 
  mutate(director = strsplit(as.character(director), ",")) %>%
  unnest(director)
```

```python
# equvilent explode In python
df["Name"] = df["Name"].apply(lambda x: x.split(";"))
df.explode("Name")
```

# 
```
read.table(gzfile("/tmp/foo.csv.gz"))  
# Or

ibrary(R.utils)
gunzip("file.gz", remove=FALSE)
```

# [Accessing REST API using R Programming](https://www.geeksforgeeks.org/accessing-rest-api-using-r-programming/#:~:text=REST%20API%20can%20be%20used%20with%20any%20language,format%20and%20then%20into%20a%20parsed%20data%20frame.?msclkid=fb77c5e9bc5f11ec839e8080511396e6)

### An API can only be considered RESTful when it meets the following conditions:

- Uniform Interface: A well-defined interface between server and client.  
- Stateless: A state is managed via the requests themselves and not through the support of an external service.  
- Cacheable: Responses should be cacheable in order to improve scalability.  
- Client-Server: Well-defined separation of client and server.  
- Layered System: The client should be unaware of intermediaries between the client and the server.  
- Code on Demand: Response can include logic executable by the client  

```
# Installing the packages
install.packages("httr")
install.packages("jsonlite")
 
# Loading packages
library(httr)
library(jsonlite)
 
# Initializing API Call
call <- "http://www.omdbapi.com/?i=tt3896198&apikey=948d3551&plot=full&r=json"

# Getting details in API
get_movie_details <- GET(url = call)

# Getting status of HTTP Call
status_code(get_movie_details)

# Content in the API
str(content(get_movie_details))

# Converting content to text
get_movie_text <- content(get_movie_details,
						"text", encoding = "UTF-8")


# Parsing data in JSON
get_movie_json <- fromJSON(get_movie_text,
						flatten = TRUE)

# Converting into dataframe
get_movie_dataframe <- as.data.frame(get_movie_json)

```

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

# Get Query
```
#' Get Query
#'
#' Get Data From Databricks Delta Lake
#'
#' @export
#'
get_query <- function(sql, path=NULL, dsn='uat') {
  if (!is.null(path) && file.exists(path)) {
    cat("Read data from: ",path)
    dat = readRDS(path)
  } else {
    con  <- DBI::dbConnect(odbc::odbc(), dsn=dsn)
    dat = DBI::dbGetQuery(con, statement=sql)
    DBI::dbDisconnect(con)

    if (!is.null(path)) {
      path_dir = dirname(path = path)
      if (!file.exists(path_dir)) {
        dir.create(path_dir, recursive= TRUE)
      }
      saveRDS(dat,file = path)
    }
  }

  #
  dat
}

```

# Get TArget

```
get_target <- function(x, sys_relative_target=0.06057088, direction = -1 ) {


    # ==========================================================================
    # direction = -1
    df = x %>%
      dplyr::mutate(
        rate     = num/den,
        # 95 percentile as the max rate
        rate_max = direction*quantile(direction*rate, 0.95),
        # the gap between the max and the rate adjusted by the measure direction
        rate_gap = pmax(direction*(rate_max - rate), 0),
      )

    # system baseline rate and system target rate
    sys_base_rate   = sum(df$num) / sum(df$den)
    sys_target_rate = sys_base_rate * (sys_relative_target + 1)

    # optimize function
    fun <- function(scale=0.1) {
      df2 <- df %>%
        dplyr::mutate(
          # Target rate is the original rate + rate_gap adjusted by a scale with the measure direction
          target_rate = rate + rate_gap * scale * direction,
          target_num  = den * target_rate
        )
      # ========================================================================
      # total target numerator: sum(df2$den) * sys_target_rate
      # fit the scale factor so the term of sum(df2$target_num) is close to <total target numerator>
      #
      out = (sum(df2$target_num) - sum(df2$den) * sys_target_rate)^2
    }

    # fit the scale
    fit = optimise(fun, interval = c(0, 2))
    scale = fit$minimum
    print(fit)

    #
    out <- dplyr::mutate(
      df,
      target_rate          = rate + rate_gap * scale * direction,
      target_num           = den  * target_rate,
      relative_improve     = target_rate/rate - 1,
      sys_base_rate        = sum(num)/sum(den),
      sys_target_rate      = sum(target_num)/sum(den),
      sys_relative_improve = sys_target_rate/sys_base_rate - 1
    )

}

```


# How to open a file to edit in Rstudio
- https://stackoverflow.com/questions/23750513/how-do-i-open-a-script-file-in-rstudio-using-an-r-command
```
file.edit('test.R')
```
