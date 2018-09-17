# Intro to elasticsearch
Current version 6.4

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