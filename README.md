# Elastic Search

## Running

After downloading, to start elastic search use:

```
bin/elasticsearch
```

Then update Kibana config at kibana/config/kibana.yml to point to Elastic Search by uncommenting:

```
#elasticsearch.url: "http://localhost:9200"
```

Then start Kibana by using:

```
bin/kibana
```

## Elastic Search

Document-oriented search engine. You can insert, delete, read, search, and perform analytics on the documents.

Creates an inverted index to optimize searches by mapping the document locations of where they occur. Built on top of Apache Lucene. An inverted index contains a list of all the unique words that appear in a document. When searching, a document that contains more words that match the search query is considered more relevant than a document with less matching words.

A type is essentially a table in a RDMS.
A document is a row in a RDMS.
A field is a column in a RDMS.
An index is a database in a RDMS.

Indexing is the same as inserting.

To insert use:

```
PUT /{index}/{document}/{id}
{
  "field1": "value",
  "field2": "value"
}
```

When updating, the version field is incremented.

To retrieve the document, use:

```
GET /{index}/{document}/{id}
```

To see if the document exists, use:

```
HEAD /{index}/{document}/{id}
```

To delete the document, use:

```
DELETE /{index}/{document}/{id}
```

Deleted documents are marked as deleted and then later purged.

## Schemas

Every index in Elastic Search has settings and mappings properties. Mappings contains all of the fields in the index.
Settings contain properties such as the number of shards and replicas.

## Term Query

To search a specific term, use:

```
{
  "query": {
    "term": { "name": "honor" }
  }
}
```

To match all terms, use:

```
{
  "query": {
    "match_all": {}
  }
}
```

Sharding split up data into multiple sets where the number of sets is equal to the number of shards. Replicas can duplicate the same data among several different shards.
Based on the ID of the document, Elastic Search can determine which shard to route the document to.

When a document in requested, the requests are distributed among the shards in an ordered fashion (round robin).

Each segment in a shard is an inverted index.

## Analysis

The process of taking the input words and creating an inverted index from it. You can't overwrite segments once populated.

### Filters

Remove stop words- the, and, etc.
Lowercase words- case insensitive
Stemming- get to the root of the word (i.e. swimming -> swim)
Synonyms- words that mean the same thing

Also stores the position/offset of the term in the document.

## Data Types

To define custom types for fields, use:

```
PUT /customers/_mapping
{
  "properties": {
    "name": {
      "type": "string",
      "analyzer": "standard"
    }
  }
}
```

Data Types for document fields:

```
String fields: text, keyword
Numeric fields: long, integer, short, byte, double, float
Date fields: text, keyword
True/false fields: boolean
Binary fields: binary
```

You can set the dynamic field to be false means that unmarked data that has not been defined will be ignored. A strict setting means that the unmarked data will cause an error to throw an error.

The simple analyzer splits on whitespace and only matches characters.
The standard analyzer splits on whitespace and matches entire words.

You can define your own analyzers by specifying the tokens and filters.

### DSL

Each DSL component can contain a query and a filter context.

```
{
  "match": {"name": "term"}
}
```

When searching the score displays how relevant the search is.

To ensure a field exists on a document use:

```
{
  "exists": {"field": "name"}
}
```

You can use multiple matching criteria by using:

```
{
  "bool": {
    "must": [
      {"match": {"name": "name"}},
      {"match": {"name": "name"}}
    ],
    "must_not": [
      {"match": {"name": "name"}},
      {"match": {"name": "name"}}
    ],
    "should": [
      {"match": {"name": "name"}},
      {"match": {"name": "name"}}
    ]
  }
}
```

To match a phase use:

```
{
  "query": {
    "match_phrase": {
      "name": "words"
    }
  }
}
```

To do a range query use:

```
{
  "query": {
    "range": {
      "number": {
        "gte": 10,
        "lte": 20
      }
    }
  }
}
```

Everything in the query context will result in a relevancy score. The more filters that match a document, the higher relevancy it will have.

You can boost the relevancy score of a field by using by using a ^2 after the field.

### Pagination

```
{
  "from": 0,
  "size": 5
}
```

### Sorting

```
{
  "sort": [
    {"price": {"order": "desc"}}
  ]
}
```

### Aggregation

```
{
  "aggs": {
    "popular_cars": {
      "terms": {
        "field": "make.keyword"
      }
    },
    "avg_price": {
      "avg": {
        "field": "price"
      }
    },
    "max_price": {
      "max": {
        "field": "price"
      }
    }
  }
}
```

Returns aggregations for a values in an index. Avg returns the average price for the category (i.e. average car price). Min/max returns the minimum or maximum price. 
