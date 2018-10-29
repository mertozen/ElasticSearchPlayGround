# ElasticSearchPlayGround

POST /product/default/_bulk
{ "index": { "_id":"100"} }
{ "price" : 100 }
{ "index": { "_id":"101"} }
{ "price" : 101 }

POST /product/default/_bulk
{ "update": { "_id":"100"} }
{"doc":{"price": 1000}}
{"delete": { "_id":101}}

GET /product/default/1

PUT /product?pretty

POST /product/default
{
  "name": "Processing Events with Logstash",
  "instructor": {
    
    "fistName": "Bo",
    "lastName":"Andersen"
  }
}

PUT /product/default/1
{
  "name": "Processing Events with Logstash",
  "instructor": {
    
    "fistName": "Bo",
    "lastName":"Andersen"
  },
  "price":195
}

POST /product/default/1
{
  "name": "Complete Guide to ElasticSearch",
  "instructor": {
    
    "fistName": "Bo",
    "lastName":"Andersen"
  }
}

DELETE /product?pretty

#Updating documents

POST /product/default/1/_update 
{
  "doc": { "price":95,"tags":["ElasticSearch"]}
}

GET /product/default/100

#Script
POST /product/default/1/_update
{
  "script": "ctx._source.price +=10"
}

DELETE /product/default/1

POST /product/default/1/_update
{
  "script": "ctx._source.price +=5",
  "upsert": {
    "price":100
  }
}

POST /product/default
{
  "name":"Processing Events with Logstash",
  "category":"course"
}

POST /product/default
{
  "name":"The Art of Scalability",
  "category":"book"
}
#Delete by query
POST /product/_delete_by_query
{
  "query": {
    "match": {
      "category": "book"
    }
  }
}

DELETE /product

POST /product/default/_bulk
{"index":{"_id":"100"}}
{"price" : 100 }
{"index" : {"_id":"101" } }
{"price" : 101}

POST /product/default/_bulk
{ "update": {"_id":"100"}}
{ "doc": {"price":1000}}
{ "delete": {"_id":101}}

GET /product/default/100

POST /product/_count

GET /_cat/health?v

GET /_cat/nodes?v

GET /_cat/indices?v

GET /_cat/allocation?v

GET /_cat/shards?v
