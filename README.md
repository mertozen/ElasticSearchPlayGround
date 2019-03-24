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

GET /product/default/_mapping

GET /product/default/1

GET /product/default/_source

PUT /product/default/_mapping
{
  "properties": {
    "discount":{
      "type": "integer"
    }
  }
}


PUT /product/default/_mapping
{
  "properties": {
    "description":{
      "type": "text"
    },
      "name": {
        "type":"text",
        "fields":{
          "keyword":{
            "type":"keyword"
          }
          
        }
        
      },
      "tags":{
        "type":"text",
        "fields":{
          "keyword":{
            "type":"keyword"
          }
        }
        
      }
      
    }
    
  }
  
GET /product/default/_mapping  

PUT /product/default/_mapping
{
  "properties": {
    "created":{
      "type": "date",
      "format": "epoch_millis"
    }
    
  }
  
}


PUT /product/default/_mapping
{
  "properties": {
    "created":{
      "type": "date",
      "format": "yyyy/MM/dd HH:mm:ss||yyyy/MM/dd"
    }
    
  }
  
}


PUT /existing_analyzer_config
{
  "settings": {
    "analysis": {
      "analyzer": {
        "english_stop":{
          "type": "standard",
          "stopwords":"_english_"
          }
        
      },"filter": {
        "my_stemmer":{
          "type":"stemmer",
          "name":"english"
        }
      }
    }
  }
}


POST /existing_analyzer_config/_analyze
{
  "analyzer": "english_stop",
  "text": "I'm in the mood for drinking semi-dry red wine!"
}


POST /existing_analyzer_config/_analyze
{
  "tokenizer": "standard",
  "filter": ["my_stemmer"],
  "text":"I'm in the mood for drinking semi-dry red wine!"
}

PUT /analyzers_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "english_stop":{
          "type": "standard",
          "stopwords":"_english_"
          },
          "my_analyzer": {
            
            "type":"custom",
            "tokenizer": "standard",
            "char_filter": [
              "html_strip"
              ],
              "filter":[
                "standard",
                "lowercase",
                "trim",
                "my_stemmer"
                ]
          }
        
      },"filter": {
        "my_stemmer":{
          "type":"stemmer",
          "name":"english"
        }
      }
    }
  }
}


POST /analyzers_test/_analyze
{
  
  "analyzer": "my_analyzer",
  "text": "I'm in the mood for drinking <strong>semi-dry</strong> red wine!"
}


PUT /analyzers_test/default/_mapping
{
 "properties": {
   "description": {
     "type": "text",
     "analyzer": "my_analyzer"
   },
   "teaser":{
     "type": "text",
     "analyzer": "standard"
   }
 } 
}

POST /analyzers_test/default/1
{"description":"drinking",
  "teaser":"drinking"
}

GET /analyzers_test/default/_search
{
  "query": {
    "term": {
      "description": {
        "value": "drinking"
      }
    }
    
  }
}


POST /analyzers_test/_close

PUT /analyzers_test/_settings
{
  "analysis": {
    "analyzer": {
      "french_stop": {
        "type": "standard",
        "stopwords": "_french_"
      }
    }
  }
}

POST /analyzers_test/_open
