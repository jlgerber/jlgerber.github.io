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
- string (changed to text)
- number
- boolean
- date
- binary

## string
Most common data type. Perfect for storing text or group of characters. Many options and settings.

## boolean
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