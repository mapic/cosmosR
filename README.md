# cosmosR
Basic functions in R for simple interactions with a CosmosDB REST API using Document-based storage.


## Installation
```R 
install_github('mapic/cosmosR')

```
## Usage

### Get all documents from CosmosDB
```R
# import library
library(cosmosR)

# authenticate with CosmosDB container
cosmosAuth("accessKey", "uri", "databaseName", "collectionName")

# get all documents (default: SELECT * FROM c)
data <- cosmosQuery(content.response = TRUE)

# json prettify results
result <- jsonlite::prettify(jsonlite::toJSON(data, auto_unbox = TRUE), 4) 
```

### Query documents from CosmosDB
```R
# import library
library(cosmosR)

# authenticate with CosmosDB container
cosmosAuth("accessKey", "uri", "databaseName", "collectionName")

# example query
sql.full = "SELECT * FROM c"

# example query: note that with more complicated queries, SELECT * is not allowed, so use SELECT c
sql.full = 'SELECT c FROM c JOIN samples IN c.samples JOIN tests IN samples.tests WHERE tests.parameter.code = "10112"'

# query with filter, limit results
data <- cosmosQuery(sql.full = sql.full, max.items = 100, content.response = TRUE)

# json prettify results
result <- jsonlite::prettify(jsonlite::toJSON(data, auto_unbox = TRUE), 4)   
``` 

### Create Document in CosmosDB
You can also create a document, by using the `cosmosCreate()` function. 
(Note that the partition key _value_ is required by CosmosDB both as `value` in argument and as `key=value` in the document.)

```R
# import library
library(cosmosR)

# authenticate with "lab-orders" container
cosmosAuth("KeyGoesHere", "uri", "dbName", "collName")

# list of keys t
sql_doc <- list(
    id = "unique-id",       # required
    code = "code-1",        # required, must also be added to cosmosCreate() below
    other = "my-value",     # add whatever you want
    more = "more-values"    # add whatever you want
)

# create item in CosmosDB
data <- cosmosCreate(sql.doc = sql_doc, sql.partitionkey_value = "code-1", content.response = TRUE)

# json prettify results
result <- jsonlite::prettify(jsonlite::toJSON(data, auto_unbox = TRUE), 4) 

```


## License
[MIT License](https://github.com/mapic/cosmosR/blob/master/LICENSE)

Supported by [NGI.no](https://ngi.no).

Based on work by [aaron2012r2](https://github.com/aaron2012r2/cosmosR).