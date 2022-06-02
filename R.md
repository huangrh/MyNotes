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
    dat = readRDS(path)
  } else {
    con  <- DBI::dbConnect(odbc::odbc(), dsn=dsn)
    dat = DBI::dbGetQuery(con, statement=sql)
    DBI::dbDisconnect(con)
    
    if (!is.null(path)) {
      path_dir = dirname(path = path)
      if (file.exists(path_dir)) {
        file.create(path_dir, recursive= TRUE)
      }
      saveRDS(dat,file = path)
    }
  }

  #
  dat
}
```
