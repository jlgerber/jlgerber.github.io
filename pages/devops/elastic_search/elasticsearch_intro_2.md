# Intro to elasticsearch
Current version 6.4

# concepts
- Elastic stores documents of json strings
- Documents are saved in an index
- An index is a collection of documents that have somewhat similar characteristics. An index is analogous to a database in relational databases
- Indices are stored in and potentially split across shards. Since an index may exceed the capacity of a single node, elasticsearch allows us to split the index into multiple shards which may live on different nodes
- Shards may have Replicas, which are Shard copies that provide fault tolerance. For this reason, a Replica Shard is never allocated on the same node as the Primary Shard.
- shards are stored on nodes (servers)
- multiple servers are called a cluster
- if you add more servers to the cluster, the nodes balance the shards such that they are distributed evenly across the nodes
- Each Elasticsearch shard is a Lucene index

# Schemas ( Mappings )
what would a schema of an index look like for a blog?

```
POST http://localhost:9200/my_blog

{
    "mappings": {
        "post": {
            "properties": {
                "user_id": {
                    "type": "integer"
                },
                "post_text": {
                    "type": "text"
                },
                "post_date": {
                     "type": "date"
                }
            }
        }
    }
}
```
## get indices
```
(GET) http://localhost:9200/_cat/indices
```

## create index
```
(PUT) http://localhost:9200/my_blog
{...}
```

## retrieving the mapping
```
(GET) http://localhost:9200/my_blog/_mapping
```

## insert a document
```
(POST)
http://localhost:9200/my_blog2/post
{
    "post_date": "2018-08-20",
    "post_text": "this is my first blogpost",
    "user_id": 1
}
```
## query
### get all posts
```
(GET)
http://localhost:9200/my_blog2/post/_search

{}
```
### get id with id
```
http://localhost:9200/my_blog2/post/<id>
eg
http://localhost:9200/my_blog2/post/DXok6GUBhoolfTvREulo
```
### Creating a post with our own id
```
(POST)
http://localhost:9200/my_blog2/post/1
{
    "post_date": "2018-08-20",
    "post_text": "this is a post with id 1. what do you think?",
    "user_id": 1
}
```

# Datatypes
- string (up to elastic 5)
- text (elastic 5+)
- keyword (elastic 5+)
- number
- boolean
- date
- binary

## String
Most common data type. Perfect for storing text or group of characters. Many options and settings.

## Text
When you want to be able to search for some part of a string, use text.

## keyword
when you want to search for whole terms, use a keyword.

## Boolean
- basic.
- values are true or false
-
## Number
- can be float, double, byte, short, integer, or long
- corresponds with the numeric types in Java

## Date
- dates and times
- UTC by default
- dateoptionaltime is the default formatter

## Binary
- store binary blob image
- store as a base64 string
- difficult to index. not indexed by default

## Options
most datatypes support options.
For example, we will set a format option on the date type.
```
{
    "mappings": {
        "post": {
            "properties": {
                "user_id": {
                    "type": "integer"
                },
                "post_text": {
                    "type": "text"
                },
                "post_date": {
                     "type": "date",
                     "format": "YYYY-MM-DD"
                }
            }
        }
    }
}
```
## Creating an Index
You add a settings section to the mappings section to define an Index.
```
{
    "settings": {
        "index": {
            "number_of_shards": 5
        }
    },
    "mappings": {
        "post": {
            "properties": {
                "user_id": {
                    "type": "integer"
                },
                "post_text": {
                    "type": "text"
                },
                "post_date": {
                     "type": "date",
                     "format": "YYYY-MM-DD"
                }
            }
        }
    }
}
```
## Routing

We can give elasticsearch a hint ahead of time to identify which shard or shards to search.

We can add a ```_routing``` property to a schema.

```bash
"mappings": {
        "post": {
            "_routing": {
                "required": true,
                "path": "user_id"
            },
            "properties": {
                "user_id": {
                    "type": "integer"
                },
                "post_text": {
                    "type": "text"
                },
                "post_date": {
                     "type": "date",
                     "format": "YYYY-MM-DD"
                }
            }
        }
    }
```
The ```"required": true``` bit indicates that searches must supply the field or fields in the routing block. (in this case, they must supply a user_id). More importantly, it indicates to Elasticsearch that it should divide up the posts by user_id when sharding. That means retrieval should be much faster because ES doesnt have to search all of the shards.

##  Index aliases
You can create aliases for an index or indexes, which allow you to search for documents by said alias. For example, lets say that you are creating an index per day for a log, and you want to search through a week's worth of logs. You can create an index alias and assign it to each of the indices in the week in question, and then write the query in terms of the alias.

```bash
(POST) http://localhost:9200/_aliases
{
    "actions": [
        { "add": { "index": "eventlog-2018-05-20", "alias": "eventlog" } }
    ]
}
```
### Example
#### Generate schema
```bash
(POST) http://localhost:9200/eventlog_2018-02-02
{
    "mappings": {
        "event": {
            "properties" : {
                "error": {
                    "type": "text"
                }
            }
        }
    }
}

(POST) http://localhost:9200/eventlog_2018-02-03
{
    "mappings": {
        "event": {
            "properties" : {
                "error": {
                    "type": "text"
                }
            }
        }
    }
}
```

#### Post logs

```bash
(POST) http://localhost:9200/eventlog_2018-02-02/event
{
    "error": "this is a fine mess you've gotten us into"
}
```
And another one

```bash
(POST) http://localhost:9200/eventlog_2018-02-03/event
{
    "error": "this is another fine mess you've gotten us into"
}
```
#### Verify, query logs
```
(get) http://localhost:9200/eventlog_2018-02-02/_search
```
#### Set up aliases

```bash
 (POST) http://localhost:9200/_aliases
{
    "actions": [
        { "add": { "index": "eventlog-2018-02-02", "alias": "eventlog" } }
    ]
}
```

Now we can search across multiple indexes using the ```eventlog``` alias:

```bash
(GET) http://localhost:9200/eventlog/_search
```

# Querying and Analysis

## basic queries
The most basic query is a query string search with an http GET:
```
(GET) http://localhost:9200/my_blog2/_search?q=post_text:awesome
```
This allows us to search on fields. Pretty basic.

### Delete old my_blog
First we need to delete my_blog and recreate it.

```
(DELETE) http://localhost:9200/my_blog2
(DELETE) http://localhost:9200/my_blog
```

### New Schema
```bash
(POST) http://localhost:9200/my_blog

"mappings": {
        "post": {
            "properties": {
                "user_id": {
                    "type": "integer"
                },
                "post_text": {
                    "type": "text"
                },
                "post_date": {
                     "type": "date",
                     "format": "YYYY-MM-DD"
                },
                "post_word_count": {
                    "type": "integer"
                }
            }
        }
    }
```

### Post some data
```bash
POST http://localhost:9200/my_blog/post
{
    "post_text": "yet another amazing post",
    "user_id": 1,
    "post_date": "2018-02-02",
    "post_word_count": 4
}
```
## Elasticsearch Query DSL
The aforementioned query, when composed in the dsl looks something like this:
```json
(POST) http://localhost:9200/my_blog/post/_search
{
    "query": {
        "match": {
            "post_text": "blog"
        }
    }
}
```

### match all
```
{
    "query": {
        "match_all": {} # select * FROM ...
    }
}
```
### term query
```
{
    "query": {
        "term": {
            "name": "fred"
        }
    }
}
```

### match

given
```
{... indexed document
    "name": "Filbert Sorting for fun and Profit"
...}
```
match will look for each term in the text. This is a match:

```
{
    "query": {
        "match": {
            "name": "sort filbert"
        }
    }
}
```
### match_phrase
match_phrase on the other hand, will look for a phrase in text so:
```
{
    "query": {
        "match_phrase": {
            "name": "sort filbert"
        }
    }
}
```
will not match, because ```sort filbert``` does not appear in name. However
```
{
    "query": {
        "match_phrase": {
            "name": "filberts sort"
        }
    }
}
```
is a match, despite the case difference, and the 's' on the end of filbert.

```
{
    "query": {
        "match_phrase": {
            "name": "filbert fun"
        }
    }
}
```
wont match becasue there is space between 'filbert' and 'fun' (remember, the phrase is 'Filbert Sorting for fun and Profit')

Howerver,
```
{
    "query": {
        "match_phrase": {
            "name": "filbert fun",
            "slop": 2
        }
    }
}
```
will match, because of the 'slop'.

## Complex queries with bool
In ES, a bool search uses the following terms
- "must" = and
- "should" = or
```
{
    "query": {
        "bool": {
            "must": [{
                "term": {
                    "city": "Nashville"
                }
            }],
            "should": [{
                "match_phrase": {
                    "name": {
                        "query": "filbert fun",
                        "slop": 2,
                        "boost": 2 # control how important each clause is
                    }
                },
                # other phrases....
            }]
        }
    }
}
```
In the former search, you have a search terms  which must match. Additionally, you have a list of terms which should match, and if they do, will boost the relevance of the match...

## Search Relevance

Each document is scored based on TF*IDF.

- TF = term frequency
- IDF = Inverse document frequency

 document frequency is how often a term appears in all documents. Inverse Document Frequence is basically 1/document frequency

Example query
    *the diddle*

Example Document
    *hey diddile diddle the cat and the fiddle*

Score:
- TF of the = 2
- TF of diddle = 2
- IDF of the = 1/infinity = 0
- IDF of diddle = 1/7
- TF X IDF of the + TF X IDF of diddle

score = 2 X 0 + 2 X 1/7 = 2/7

## Getting data in

### Analysis

  *The Conspirators conspire conspicuously!*

#### tokenize
[The][Conspirators][conspire][conspicuously]

#### Lower caseing

[the][conspirators][conspire][conspicuously]

####  Stop Wording
Throw away non essential words like prepositions...

__[conspirators][conspire][conspicuously]

#### Stemming
Take a word and using a statistical technique chop off the end to make it simpler to match

__[conspir][conspir][conspicu]

### Indexing
Doc #1 -> __[conspir][conspir][conspicu]
Store the id of each document in which each token appears in an inverted index.

'conspir': [1, 47, 72]
'conspicu': [1, 55, 92, 107]

In practive ES stores more information in the keys and values, like document frequency (stored in keys) and term frequency (stored in values).

## Getting Data Out
```
inverted_index = {
    'ardvaark': [34, 92, 119],
    'conspir': [1, 47, 72],
    'conspicu': [1, 55, 92, 107],
    'malific': [34, 72, 103],
    'zebra': [15, 34, 55, 107],
}
```
Searching for ```conspicuous and ardvaarks```
- Search becomes ```conspicu and ardvaark```
- Look up intersection of document indexes
- [92]

Searching for ```conspicuous OR ardvaarks```
- [1, 55, 92, 107, 34, 119]

Searching for ```conspicuous AND (ardvaarks or zebras)```
- [55, 92, 107]

### Relevance - ie Sorting
- initialize a Priority Queue of some fixed size depending upon the query.
- for each matching doc
    - calculate TF*IDF score
    - add to Priority Queue
    - pop off lowest scoring doc
- returns the Priority Queue

It should be noted that the queue never gets past a fixed size, and the search can further optimize because the queue is sorted by priority. So, one can compare the TF*IDF for a couple of docs and be done.

It should be further noted that ES allows you to turn off relevance.

## Aggregation

*conspicous or ardvaarks* -> [1, 34, 55, 92, 107, 119]

- Initialize Aggregator (sum, average, count, etc)
- for each matching doc
    - retrieve "interesting" information
    - add to Aggregator
- return results from Aggregator

So we can Filter, Group, and Aggregate.

### Aggregations in query
To get aggregations in a query, simply add an 'aggs' section and populate it.

```
{
    "query": {
        "match_all": {}
    },
    "aggs": {
        "by_city": {
            "terms": {
                "field": "city"
            }
        },
        "by_price": {
            "histogram": {
                "field": "price",
                "interval": 10,
            }
        }
    }
}
```

#### Results

In addition to the search results, you get an 'aggregations' section:

```
{
    ...,
    "aggregations": {
        "by_city": {
            "buckets": [
                {"key": "Nashville", "doc_count": 3204},
                {"key": "Los Angeles", "doc_count": 124},
                {"key": "Dallas", "doc_count": 32}
            ],
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
        },
        "by_price": {
            "buckets": [
                {"key": 10, "doc_count": 131},
                {"key": 20, "doc_count": 1293},
                {"key": 30, "doc_count": 43},
            ]
        }
    }
    ...
}
```

Aggregations may also be nested, so in our case, we could have asked for a histogram per city.

