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
