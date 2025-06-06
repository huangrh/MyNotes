# Time series forcast - bootstraping. Bootstrapping and bagging
- Rob J Hyndman   
https://otexts.com/fpp3/bootstrap.html  

I prefer to work with the patient level data if available. For example,  to estimate the distribution of HbA1c poor control measures with 5000 diabetes patients, we can randomly sample (with replacement) 5000 patients  and repeat the process 10, 000 times as Angelo mentioned (take some time, but can be parallel with a multiple cores computer). We can calculate the rate for each sampling and then derive the sampling distribution.  

Yes, Time series monte carlo is more complicated. First the time series is transformed and decomposed into trend, seasonal and remainder components using STL. Then we obtain shuffled versions of the remainder component to get bootstrapped remainder series. Fortunately, there is an R package fpp3 and it is well documented in a book, available free online (https://otexts.com/fpp3/bootstrap.html).   


# ODBC
```
library(odbc)

# Create a connection string
conn_string <- "Driver={SQL Server};Server=your_server;Database=your_database;Uid=your_user;Pwd=your_password;"

# Connect to the database
con <- DBI::dbConnect(odbc::odbc(), DSN = "your_dsn", UID = "your_user", PWD = "your_password") # Replace with your DSN and credentials

library(DBI)

# Execute a query
result <- DBI::dbGetQuery(con, "SELECT * FROM your_table")
df <- dbGetQuery(con, "SELECT film_id, title, description FROM film WHERE release_year = 2006 AND rating = 'G'")
# Using DBI with odbc
DBI::dbDisconnect(con)
```

```
# https://solutions.posit.co/connections/db/databases/microsoft-sql-server/
con <- DBI::dbConnect(odbc::odbc(), 
                      Driver = "SQL Server", 
                      Server = "localhost\\SQLEXPRESS", 
                      Database = "master", 
                      Trusted_Connection = "True")
```


```
library(DBI)
# Create an ephemeral in-memory RSQLite database
con <- DBI::dbConnect(RSQLite::SQLite(), dbname = ":memory:")

# 
dbWriteTable(con, "mtcars", mtcars)

dbListTables(con)
dbListFields(con, "mtcars")
dbReadTable(con, "mtcars")

# You can fetch all results:
res <- dbSendQuery(con, "SELECT * FROM mtcars WHERE cyl = 4")
dbFetch(res)
dbClearResult(res)

# Or a chunk at a time
res <- dbSendQuery(con, "SELECT * FROM mtcars WHERE cyl = 4")
while(!dbHasCompleted(res)){
  chunk <- dbFetch(res, n = 5)
  print(nrow(chunk))
}
dbClearResult(res)

dbDisconnect(con)
```


# send email through outlook
```
# https://stackoverflow.com/questions/26811679/sending-email-in-r-via-outlook
- https://www.seancarney.ca/2020/10/07/sending-email-from-outlook-in-r/

library(RDCOMClient)
## init com api
OutApp <- COMCreate("Outlook.Application")
## create an email 
outMail = OutApp$CreateItem(0)
## configure  email parameter 
outMail[["To"]] = "dest@dest.com"
outMail[["subject"]] = "some subject"
outMail[["body"]] = "some body"
## send it                     
outMail$Send()

# outMail[["Attachments"]]$Add(path_to_attch_file)
# SentOnBehalfOfName not working 
# outMail[["SentOnBehalfOfName"]] = "yoursecondary@mail.com"
```

```
# Send rmarkdown  
- https://www.seancarney.ca/2020/10/10/advanced-email-in-r-embedding-images-and-markdown/
# Knit the 'analysis' Markdown file into an HTML document, 
# you'll need to change this to the name of your Markdown report.
rmarkdown::render("analysis.Rmd", "html_document")

# Load the readtext library
require (readtext)

# Read the HTML document into an object, the file name 
# should match your .Rmd document above
body <- readtext("analysis.htm")

# Load the DCOM library
require (RDCOMClient)

# Open Outlook
Outlook <- COMCreate("Outlook.Application")

# Create a new message
Email = Outlook$CreateItem(0)

# Set the recipient, subject, and body
Email[["to"]] = "recipient1@test.com; recipient2@test.com"
Email[["subject"]] = "Quarterly Sales Report"
Email[["htmlbody"]] = body$text

# Send the message
Email$Send()
```

# Nonparametric Tests
- Mann–Whitney U Test   
- https://rcompanion.org/handbook/F_01.html
- https://sphweb.bumc.bu.edu/otlt/MPH-Modules/BS/BS704_Nonparametric/BS704_Nonparametric4.html
- 
# Read CSV
```
# https://readr.tidyverse.org
readr::read_csv()
# 
# https://stackoverflow.com/questions/14383710/read-fixed-width-text-file
# readr::read_fwf IS TWO TIMES FASTER
x <- readr::read_fwf(
  file="http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for",   
  skip=4,
  readr::fwf_widths(c(12, 7, 4, 9, 4, 9, 4, 9, 4)))


x <- read.fwf(
  file=url("http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for"),
  skip=4,
  widths=c(12, 7, 4, 9, 4, 9, 4, 9, 4))

head(x)
```

# https://www.emilyzabor.com/tutorials/survival_analysis_in_r_tutorial.html
- https://stats.oarc.ucla.edu/r/dae/mixed-effects-cox-regression/
- https://www.datacamp.com/tutorial/survival-analysis-R
- https://bioconnector.github.io/workshops/r-survival.html
- [mixed effecrts cox regression](https://stats.oarc.ucla.edu/r/dae/mixed-effects-cox-regression/#:~:text=Mixed%20effects%20cox%20regression%20models,both%20fixed%20and%20random%20effects.)
- http://www.sthda.com/english/wiki/survival-analysis-basics  
- http://www.sthda.com/english/wiki/cox-proportional-hazards-model  
# https://stackoverflow.com/questions/2885660/how-to-send-an-email-with-attachment-from-r-in-windows

```
library('httr')    
content(GET('https://www.linkedin.com/in/lillyzhu'))
```

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
- https://docs.snowflake.com/developer-guide/odbc/odbc-parameters
	- snowflake ODBC use https, because in the document it says it use the port 443, which the port https uses.    



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
# get azire storage containers  
get_azure_cons <- function(az = NULL) {

    if (is.null(az)) {
        az <- create_azure_login()
        az   = get_azure_login()
    }  

    token <- AzureRMR::get_azure_token("https://storage.azure.com",
                                       tenant="tenant id from Azure Active Directory Portal",
                                       app=az$token$client$client_id)
    #
    cons <- list(
        uat  = "stu Storage account",
        prod = "stp Storage account",
        qa   = "stq Storage account",
        dev  = "std Storage account"
    ) %>%
        lapply(function(storage_act){
            storage_endp = glue::glue("https://{storage_act}.dfs.core.windows.net/")
            ad_endp_tok  = AzureStor::storage_endpoint(storage_endp, token=token)
            con          = AzureStor::storage_container(ad_endp_tok, "my_container_name")
        })
}  

storage_upload <- function(con, src, dest) {
    AzureStor::storage_upload(con, src=src, dest=dest)
}  

storage_download <- function(con, src, dest ) {
    AzureStor::storage_download(con, src=src, dest=dest)
}  
```

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
storage_download(con, src, dest) # container, src local file, dest storage file name  
```

# ggplot2
- [how-to-put-labels-over-geom-bar-for-each-bar-in-r-with-ggplot2](https://stackoverflow.com/questions/12018499/how-to-put-labels-over-geom-bar-for-each-bar-in-r-with-ggplot2)

```
ggplot(data=dat, aes(x=Types, y=Number, fill=sample)) + 
     geom_bar(position = 'dodge', stat='identity') +
     geom_text(aes(label=Number), position=position_dodge(width=0.9), vjust=-0.25,
     angle = 90)
     
```

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
# Replace NA with Last non-NA in a data frame 
```
# [57%](https://stackoverflow.com/questions/7735647/replacing-nas-with-latest-non-na-value)
library(tidyr)
fill(df, y, .direction = 'down')
```
# Rmarkdown render
```
# How to run R script containing Rmd rendering from command line  
# 
# C:\App\R\R-4.2.3\bin\Rscript.exe
# Sys.getenv("RSTUDIO_PANDOC")
Sys.setenv(RSTUDIO_PANDOC="C:/App/RStudio/resources/app/bin/quarto/bin/tools")
require(rmarkdown)
input_file = file.path(
    "C:/workspace/R/alpine/data-raw/Pipeline",
    "Data_Upload_Daily_Run.Rmd"
)
output_dir = "C:/Alpine/OneDrive - Alpine Physician Partners/Alpine/Alpine_Analytics_Team/Prod/log"
output_file = file.path(
    glue::glue("upload_daily_run{format(Sys.time(), '%Y%m%d')}.html")
); output_file
rmarkdown::render(input = input_file, output_dir = output_dir, output_file = output_file)
```

# https://stackoverflow.com/questions/28432607/pandoc-version-1-12-3-or-higher-is-required-and-was-not-found-r-shiny
```
Sys.getenv("RSTUDIO_PANDOC")
Sys.setenv(RSTUDIO_PANDOC="--- insert directory here ---")
```
