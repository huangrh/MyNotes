

# https://learn.microsoft.com/en-us/azure/azure-maps/how-to-search-for-address  

```
library(httr)
library(jsonlite)

# Replace 'your_subscription_key' with your actual Azure Maps subscription key
subscription_key <- "your_subscription_key"

geocode_address <- function(address) {
  url <- paste0("https://atlas.microsoft.com/search/address/json?api-version=1.0&subscription-key=", 
                subscription_key, "&query=", URLencode(address))
  response <- GET(url)
  content <- content(response, "text")
  json <- fromJSON(content)
  return(json$results[[1]]$position)
}

# Example usage
address <- "1 Microsoft Way, Redmond, WA"
coordinates <- geocode_address(address)
print(coordinates)


```

# Search - Post Search Address Batch   
- https://learn.microsoft.com/en-us/rest/api/maps/search/post-search-address-batch?view=rest-maps-1.0&tabs=HTTP  
- https://learn.microsoft.com/en-us/rest/api/maps/search/post-search-address-batch?view=rest-maps-1.0&tabs=HTTP ******
- https://www.geeksforgeeks.org/python-requests-post-request-with-headers-and-body/   
```
import requests  

# URL for the POST request  
# Submit Synchronous Batch Request   
POST https://atlas.microsoft.com/search/address/batch/sync/json?api-version=1.0&subscription-key={subscription-key}

# Submit Asynchronous Batch Request  
POST https://atlas.microsoft.com/search/address/batch/json?api-version=1.0&subscription-key={subscription-key}
POST https://atlas.microsoft.com/search/address/batch/json?api-version=1.0&subscription-key={subscription-key}

url = "https://example.com/api"  

# Body of the POST request (replace with your actual data)
data = {
    "key1": "value1",
    "key2": "value2"
}

# Make the POST request
response = requests.post(url, json=data)

# Print the response from the server
print(response.text)

```

```
# https://www.w3schools.com/PYTHON/ref_requests_get.asp

import requests
x = requests.get('https://w3schools.com')

```

```
# Example
import requests
json = {
    "batchItems": [
        {"query": "?query=14214 Cypress Hill dr. Chesterfield, MO 63017&limit=1"},
        {"query": "?query=400 Broad St, Seattle, WA 98109&limit=3"},
        {"query": "?query=One, Microsoft Way, Redmond, WA 98052&limit=3"},
        {"query": "?query=350 5th Ave, New York, NY 10118&limit=1"},
        {"query": "?query=Pike Pl, Seattle, WA 98101&lat=47.610970&lon=-122.342469&radius=1000"},
        {"query": "?query=Champ de Mars, 5 Avenue Anatole France, 75007 Paris, France&limit=1"}
    ]
}
key = "use your subscription key"  
sync_url = f"https://atlas.microsoft.com/search/address/batch/sync/json?api-version=1.0&subscription-key={key}"
res = requests.post(sync_url,json=json)
res.json()['batchItems'][0]['response']['results'][0]['position']


