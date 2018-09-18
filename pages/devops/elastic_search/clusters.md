# Clusters

## Monitoring health

```
GET /_cluster/health
```

returns a json document. we are interested in the "health" field:

- green
    - All primary and replica shards are active.
- yellow
    - All primary shards are active, but not all replica shards are active.
- red
    - Not all primary shards are active.


## Index & Shard
In order to add data to Elasticsearch, we need to add an index first. An index is a logical namespace which points at one or more physical shards.

A shard is a single instance of a Lucene db. A shard may either be a *primary* or *replica* shard. Each document in your index belongs to a primary shard.

The number of primary shards assigned to an index is fixed at index creation time. The number defaults to 5.
