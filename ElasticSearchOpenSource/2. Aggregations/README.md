# Aggregations

## AVG Aggregation

- Computes average of numeric values that are extracted from aggregated documents.
- These values can be extracted either from specific numeric fields in the documents, or be generated by a provided script.

```
GET /kibana_sample_data_ecommerce/_search

GET /kibana_sample_data_ecommerce/_doc/Vkh0uHABOGNVffxkD48G

POST /kibana_sample_data_ecommerce/_search?size=0
{
    "aggs" : {
        "avg_base_price" : { 
          "avg" : { 
            "field" : "products.base_price"
          }
        }
    }
}
```

## Script

Computing average based on script

### inline script

```
POST /kibana_sample_data_ecommerce/_search?size=0
{
    "aggs" : {
        "avg_base_price" : {
          "avg" : { 
            "field": "products.price", 
            "script": {
              "source": "_value"
            }
          }
        }
    }
}
```


### inline script with parameters
```
POST /kibana_sample_data_ecommerce/_search?size=0
{
    "aggs" : {
        "avg_base_price" : {
          "avg" : { 
            "field": "products.price", 
            "script": {
              "source": "_value * params.correction", 
              "params" : {
                  "correction" : 2.2
              }
            }
          }
        }
    }
}
```




