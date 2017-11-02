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
