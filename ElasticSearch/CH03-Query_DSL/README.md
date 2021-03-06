# Query DSL

Elasticsearch provides a full Query DSL (Domain Specific Language) based on JSON to define queries. Think of the Query DSL as an AST (Abstract Syntax Tree) of queries, consisting of two types of clauses:

## Leaf query clauses
Leaf query clauses look for a particular value in a particular field, such as the match, term or range queries. These queries can be used by themselves.

## Compound query clauses
Compound query clauses wrap other leaf or compound queries and are used to combine multiple queries in a logical fashion (such as the bool or dis_max query), or to alter their behaviour (such as the constant_score query).



# Query and filter context

## Relevance score

Relevance score measures how well each document matches query.

Score also depends on weather its placed in filter or query conetxt.

## Query context

How well does this document match this query clause?

## Filter context

“Does this document match this query clause?” The answer is a simple Yes or No

## Example of query and filter context

```
GET /_search
{
  "query": { 
    "bool": { 
      "must": [
        { "match": { "title":   "Search"}},
        { "match": { "content": "Elasticsearch" }}
      ],
      "filter": [ 
        { "term":  { "status": "published" }},
        { "range": { "publish_date": { "gte": "2015-01-01" }}}
      ]
    }
  }
}
```

- The query parameter indicates query context.
- The bool and two match clauses are used in query context, which means that they are used to score how well each document matches.
- The filter parameter indicates filter context. Its term and range clauses are used in filter context. They will filter out documents which do not match, but they will not affect the score for matching documents.

```Warning``` 
Scores calculated for queries in query context are represented as single precision floating point numbers; they have only 24 bits for significand’s precision. Score calculations that exceed the significand’s precision will be converted to floats with loss of precision.

```TIP``` Use query clauses in query context for conditions which should affect the score of matching documents (i.e. how well does the document match), and use all other query clauses in filter context.

## Compound queries
Compound queries wrap other compound or leaf queries, either to combine their results and scores, to change their behaviour, or to switch from query to filter context


## Boolean queries
A query that matches documents matching boolean combinations of other queries.

- must, The clause (query) must appear in matching documents and will contribute to the score.
- filter, The clause (query) must appear in matching documents. However unlike must the score of the query will be ignored. Filter clauses are executed in filter context, meaning that scoring is ignored and clauses are considered for caching.
- should, The clause (query) should appear in the matching document.
- must_not, The clause (query) must not appear in the matching documents. Clauses are executed in filter context meaning that scoring is ignored and clauses are considered for caching. Because scoring is ignored, a score of 0 for all documents is returned.


## Boosting query

Returns documents matching a positive query while reducing the relevance score of documents that also match a negative query.

You can use the boosting query to demote certain documents without excluding them from the search results.

```
POST /kibana_sample_data_ecommerce/_search
{
  "query": {
    "boosting": {
      "positive": {
        "term" : { "email" : "mary@bailey-family.zzz" }
      },
      "negative": {
        "term" : { "customer_gender" : "FEMALE" }
      },
      "negative_boost": 0.1
    }
  }
}
```

## Constant score query

Wraps a filter query and returns every matching document with a relevance score equal to the boost parameter value.

```
GET /_search
{
    "query": {
        "constant_score" : {
            "filter" : {
                "term" : { "user" : "kimchy"}
            },
            "boost" : 1.2
        }
    }
}
```


## Disjunction max query.
- use cases of query?


# Full Text queries

The full text queries enable you to search **analyzed text fields** such as the body of an email. The query string is processed using the same analyzer that was applied to the field during indexing.

## Intervals:
Returns documents based on the order and proximity of matching terms.

The intervals query uses matching rules, constructed from a small set of definitions. These rules are then applied to terms from a specified field.

```
POST /kibana_sample_data_ecommerce/_search
{
  "query": {
    "intervals":{
      "customer_full_name":{
        "all_of":{
          "intervals":[
            {
              "match":{
                "query" : "Mary",
                "max_gaps" : 0,
                "ordered" : true
              }
            },
            {
              "any_of":{
                "intervals":{
                  "match": { "query":"Bailey"}
                }
              }
            }
          ] 
        }
      }
    }
  }  
}
```

##Top-level parameters for intervals

## <field> 
  (Required, rule object) Field you wish to search. 

## "match" rule parameter 
  The match rule matches analyzed text.

  ```query``` --> Text you wish to find in the provided <field>.

  ```max_gaps``` --> (Optional, integer) Maximum number of positions between the matching terms.

  ```ordered``` --> (Optional, boolean) If true, matching terms must appear in their specified order. Defaults to false.

  ``` analyzer``` --> (Optional, string) analyzer used to analyze terms in the query. Defaults to the top-level <field>'s analyzer. 

  ``` filter ``` --> (Optional, interval filter rule object) An optional interval filter.

  ``` use_field ``` --> need to understand aim of this.

## "prefix" rule parameter

The prefix rule matches terms that start with a specified set of characters. This prefix can expand to match at most 128 terms. If the prefix matches more than 128 terms, Elasticsearch returns an error. You can use the index-prefixes option in the field mapping to avoid this limit.

## "wildcard" rule parameter

The wildcard rule matches terms using a wildcard pattern. This pattern can expand to match at most 128 terms. If the pattern matches more than 128 terms, Elasticsearch returns an error.


## "fuzzy" rule parameter

The fuzzy rule matches terms that are similar to the provided term, within an edit distance defined by Fuzziness. If the fuzzy expansion matches more than 128 terms, Elasticsearch returns an error.

## "all_of" rule parameter 

The all_of rule returns matches that span a combination of other rules.

## "any_of" rule parameter

The any_of rule returns intervals produced by any of its sub-rules.

## "any_of" rule parameter


# Match query

Returns documents that match a provided text, number, date or boolean value. The provided text is analyzed before matching.

The match query is the standard query for performing a full-text search, including options for fuzzy matching.

```
POST /kibana_sample_data_ecommerce/_search
{
  "query": {
    "match":{
      "customer_full_name":{
          "query" : "Bailey"
      }
    }
  }  
}
```




