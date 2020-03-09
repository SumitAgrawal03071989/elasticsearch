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




















