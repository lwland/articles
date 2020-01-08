### concept
1. Cluster:集群 可以由多个或一个Node组成，必须有一个独一无二的name，Node根据name加入不同的Cluster
2. Node：一个server，存储数据，参与索引和查询。UUID
3. Index：一系列document的集合，SQL中的database
4. Type：一类document的集合，SQL中的Table
5. Document：基本的信息单元，JSON形式
6. Shards&Replications：shar的,分片，index中大量document分散存储在不同node的shard上；Replication，从shard copy的副本。当index创建时可以指定shard和replcation的数量，index创建后，repliction的数量可以动态扩展，shard的数量不可变。SQL中读写分离主库和从库的关系

### _cat API 
1. curl -X GET "10.18.35.111:9001/_cat/health?v" check cluster health
2. curl -X GET "10.18.35.111:9001/_cat/nodes?v" node info
3. curl -X GET "10.18.35.111:9001/_cat/indices?v" index info
4. curl -X GET "10.18.35.111:9001/_cat/plugins?v"
5. curl -X GET "10.23.3.106:9001/_cat/health?v" check cluster

### index API
1. curl -X PUT "10.18.35.111:9001/customer?pretty"

### search API
1. curl -X GET "10.18.35.111:9001/focus-kefu/_search" -H 'Content-Type: application/json' -d'
{
  "query": { "match_all": {} },
  "sort": [
    { "id": "asc" }
  ]
}
'
curl -X GET "10.18.35.111:9001/focus-kefu/_search" -H 
'Content-Type: application/json' -d'
{
  "query": { "id": "9" },
}
'
curl -X  POST "10.18.35.111:9001/focus-kefu/kefu_document/_delete_by_query?conflicts=proceed" -H 'Content-Type: application/json' -d'
{
  "query": { 
    "match": {
      "id": "13"
    }
  }
}
'
curl -X DELETE "10.18.35.111:9001/focus-kefu/kefu_document/9"




