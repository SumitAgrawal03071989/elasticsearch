# Elasticsearch introduction.

## Data in: documents and indices 

Elasticsearch is a distributed document store. 
Instead of storing information as rows of columnar data, Elasticsearch stores complex data structures that have been serialized as JSON documents.

Documents are indexed and fully searchable in near real-time-- within 1 second. Elastic search uses data structure called inverted index that supports very fast full text search.

Elasticsearch indexes all data in every field and each indexed field has a dedicated, optimized data structure. For example, text fields are stored in inverted indices, and numeric and geo fields are stored in BKD trees.

You can define rules to control dynamic mapping and explicitly define mappings to take full control of how fields are stored and indexed.

Defining your own mappings enables you to:

- Distinguish between full-text string fields and exact value string fields
- Perform language-specific text analysis
- Optimize fields for partial matching
- Use custom date formats
- Use data types such as geo_point and geo_shape that cannot be automatically detected

## Information out: search and analyse


### Searching your data

The Elasticsearch REST APIs support structured queries, full text queries, and complex queries that combine the two

In addition to searching for individual terms, you can perform phrase searches, similarity searches, and prefix searches, and get autocomplete suggestions.

- JSON-style query language (Query DSL)
- You can also construct SQL-style queries 


### Analyzing your data

Elasticsearch aggregations enable you to build complex summaries of your data and gain insight into key metrics, patterns, and trends.


### Installation elastic search.

Download the Elasticsearch archive for your OS:

```
https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.6.0-darwin-x86_64.tar.gz
```

 Extract the archive
```
Move it to /usr/local/elasticsearch-7.6.0
```

```
cd elasticsearch-7.6.0/bin
./elasticsearch
```