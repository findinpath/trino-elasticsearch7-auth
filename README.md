Trino with Elasticsearch 7
==========================


This showcase project is inspired from [trino-gettingstarted/trino-elasticsearch7](https://github.com/bitsondatadev/trino-getting-started/tree/main/elasticsearch/trino-elasticsearch7) guideline.

On top of the initial project, this project adds authentication to Elasticsearch 7
and verifies whether the trino elasticsearch connector works as expected with authentication.



## Demo


Use

```
docker-compose up -d
```

in order to spin up the docker testing environment.

Once both Trino and Elasticsearch 7 are up and running,
connect via trino to Elasticsearch.


```
docker container exec -it trino-elasticsearch7-auth_trino-coordinator_1 trino
```


```
trino> show catalogs;
    Catalog    
---------------
 elasticsearch 
 system        
 tpcds         
 tpch          
(4 rows)
```


```
trino> show schemas in elasticsearch;
       Schema       
--------------------
 default            
 information_schema 
 system             
(3 rows)
```


```
trino> show tables in elasticsearch.default;
 Table 
-------
(0 rows)
```

In case that the password configured  `etc/catalog/elasticsearch.properties` file would be incorrect (`changeme1` instead of `changeme`), the previous statement would fail with a `401` error:

```
trino> show tables in elasticsearch.default;

Query 20211103_213008_00006_szuip, FAILED, 1 node
Splits: 19 total, 0 done (0.00%)
0.13 [0 rows, 0B] [0 rows/s, 0B/s]

Query 20211103_213008_00006_szuip failed: method [GET], host [http://es01:9200], URI [/_cat/indices?h=index,docs.count,docs.deleted&format=json&s=index:asc], status line [HTTP/1.1 401 Unauthorized]
{"error":{"root_cause":[{"type":"security_exception","reason":"unable to authenticate user [elastic] for REST request [/_cat/indices?h=index,docs.count,docs.deleted&format=json&s=index:asc]","header":{"WWW-Authenticate":"Basic realm=\"security\" charset=\"UTF-8\""}}],"type":"security_exception","reason":"unable to authenticate user [elastic] for REST request [/_cat/indices?h=index,docs.count,docs.deleted&format=json&s=index:asc]","header":{"WWW-Authenticate":"Basic realm=\"security\" charset=\"UTF-8\""}},"status":401}
```



Once the demo session is over, tear down the docker environment:

```
docker-compose down
```
