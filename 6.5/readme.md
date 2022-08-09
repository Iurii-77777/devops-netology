## **Задача 1.**
#### В ответе приведите:
#### Текст Dockerfile манифеста:
```
FROM centos:7
LABEL ElasticSearch Lab 6.5 \
    (c)Zaytsev Alexey
ENV PATH=/usr/lib:/usr/lib/jvm/jre-11/bin:$PATH

RUN yum install java-11-openjdk -y
RUN yum install wget -y

RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.11.1-linux-x86_64.tar.gz \
    && wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.11.1-linux-x86_64.tar.gz.sha512
RUN yum install perl-Digest-SHA -y
RUN shasum -a 512 -c elasticsearch-7.11.1-linux-x86_64.tar.gz.sha512 \
    && tar -xzf elasticsearch-7.11.1-linux-x86_64.tar.gz \
    && yum upgrade -y

ADD elasticsearch.yml /elasticsearch-7.11.1/config/
ENV JAVA_HOME=/elasticsearch-7.11.1/jdk/
ENV ES_HOME=/elasticsearch-7.11.1
RUN groupadd elasticsearch \
    && useradd -g elasticsearch elasticsearch

RUN mkdir /var/lib/logs \
    && chown elasticsearch:elasticsearch /var/lib/logs \
    && mkdir /var/lib/data \
    && chown elasticsearch:elasticsearch /var/lib/data \
    && chown -R elasticsearch:elasticsearch /elasticsearch-7.11.1/
RUN mkdir /elasticsearch-7.11.1/snapshots &&\
    chown elasticsearch:elasticsearch /elasticsearch-7.11.1/snapshots

USER elasticsearch
CMD ["/usr/sbin/init"]
CMD ["/elasticsearch-7.11.1/bin/elasticsearch"]

```
#### Текст elasticsearch.yml:
```
#for Image from TAR (est)
path.repo: /elasticsearch-7.11.1/snapshots
#
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: 0.0.0.0
#
# Set a custom port for HTTP:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
discovery.seed_hosts: ["127.0.0.1", "[::1]"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
#cluster.initial_master_nodes: ["node-1", "node-2"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Gateway -----------------------------------
#
# Block initial recovery after a full cluster restart until N nodes are started:
#
#gateway.recover_after_nodes: 3
#
# For more information, consult the gateway module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Require explicit names when deleting indices:
#
#action.destructive_requires_name: true

"elasticsearch.yml" 96L, 3024C                                                                                                                             
```
#### Ссылку на образ в репозитории dockerhub
```
docker pull iuriinetology/for_elastic:latest 
```
#### Ответ elasticsearch на запрос пути / в json виде
```
iurii-devops@Host-SPB:~/elasticsearch$ sudo docker run --name elasticsearch -p 9200:9200 -p 9300:9300 36089842433b

iurii-devops@Host-SPB:~$ curl localhost:9200
{
  "name" : "756d6b1fdc62",
  "cluster_name" : "netology_test",
  "cluster_uuid" : "-exO51eZStOeujeuPnBiOg",
  "version" : {
    "number" : "7.11.1",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "ff17057114c2199c9c1bbecc727003a907c0db7a",
    "build_date" : "2021-02-15T13:44:09.394032Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
## **Задача 2.**
#### Ознакомтесь с документацией и добавьте в elasticsearch 3 индекса, в соответствии со таблицей:
```
iurii-devops@Host-SPB:~$ curl -X PUT localhost:9200/ind-1 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-1"}

iurii-devops@Host-SPB:~$ curl -X PUT localhost:9200/ind-2 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 2,  "number_of_replicas": 1 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-2"}

iurii-devops@Host-SPB:~$ curl -X PUT localhost:9200/ind-3 -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 4,  "number_of_replicas": 2 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"ind-3"}
```
#### Получите список индексов и их статусов, используя API и приведите в ответе на задание.
```
iurii-devops@Host-SPB:~$ curl -X GET 'http://localhost:9200/_cat/indices?v'
health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   ind-1 OG3p_rxCS-Sw80GEW1rtug   1   0          0            0       208b           208b
yellow open   ind-3 -IqyURodQhev2J4xrCJRWg   4   2          0            0       832b           832b
yellow open   ind-2 SKBgN5YYSRac5FofNGKXUg   2   1          0            0       416b           416b
```
```
iurii-devops@Host-SPB:~$ curl -X GET 'http://localhost:9200/_cluster/health/ind-1?pretty' 
{
  "cluster_name" : "netology_test",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}

iurii-devops@Host-SPB:~$ curl -X GET 'http://localhost:9200/_cluster/health/ind-2?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 2,
  "active_shards" : 2,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 2,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.17647058823529
}

iurii-devops@Host-SPB:~$ curl -X GET 'http://localhost:9200/_cluster/health/ind-3?pretty'
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 4,
  "active_shards" : 4,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 8,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.17647058823529
} 
```
#### Получите состояние кластера elasticsearch, используя API.
```
iurii-devops@Host-SPB:~$ curl -XGET localhost:9200/_cluster/health/?pretty=true
{
  "cluster_name" : "netology_test",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 7,
  "active_shards" : 7,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 41.17647058823529
}
```
#### Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?
```
Нет серверов для репликации, но при этом реплики указаны.
```
#### Удалите все индексы.
```
iurii-devops@Host-SPB:~$ curl -X DELETE 'http://localhost:9200/ind-1?pretty' 
{
  "acknowledged" : true
}
iurii-devops@Host-SPB:~$ curl -X DELETE 'http://localhost:9200/ind-2?pretty' 
{
  "acknowledged" : true
}
iurii-devops@Host-SPB:~$ curl -X DELETE 'http://localhost:9200/ind-3?pretty' 
{
  "acknowledged" : true
}
```
## **Задача 3.**
#### Создайте директорию {путь до корневой директории с elasticsearch в образе}/snapshots.
#### Используя API зарегистрируйте данную директорию как snapshot repository c именем netology_backup.
#### Приведите в ответе запрос API и результат вызова API для создания репозитория.
```
iurii-devops@Host-SPB:~/elasticsearch$ curl -XPOST localhost:9200/_snapshot/netology_backup?pretty -H 'Content-Type: application/json' -d'{"type": "fs", "settings": { "location":"/elasticsearch-7.11.1/snapshots" }}'
{
  "acknowledged" : true
}
```
#### Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.
#### Создайте snapshot состояния кластера elasticsearch.
#### Приведите в ответе список файлов в директории со snapshotами.
```
iurii-devops@Host-SPB:~/elasticsearch$ curl -X PUT localhost:9200/test -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{"acknowledged":true,"shards_acknowledged":true,"index":"test"}

iurii-devops@Host-SPB:~/elasticsearch$ curl http://localhost:9200/test?pretty
{
  "test" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "test",
        "creation_date" : "1660068127670",
        "number_of_replicas" : "0",
        "uuid" : "3K23au20Thqte8rQw5fDaA",
        "version" : {
          "created" : "7110199"
        }
      }
    }
  }
}

iurii-devops@Host-SPB:~/elasticsearch$ curl -X PUT localhost:9200/_snapshot/netology_backup/elasticsearch?wait_for_completion=true
{"snapshot":{"snapshot":"elasticsearch","uuid":"BqwRM7GQRNGouGLffaJy2g","version_id":7110199,"version":"7.11.1","indices":["test"],"data_streams":[],"include_global_state":true,"state":"SUCCESS","start_time":"2022-08-09T18:04:52.228Z","start_time_in_millis":1660068292228,"end_time":"2022-08-09T18:04:52.228Z","end_time_in_millis":1660068292228,"duration_in_millis":0,"failures":[],"shards":{"total":1,"failed":0,"successful":1}}}

iurii-devops@Host-SPB:~/elasticsearch$ sudo docker exec -it 756d6b1fdc62 bash
[elasticsearch@756d6b1fdc62 /]$ cd elasticsearch-7.11.1
[elasticsearch@756d6b1fdc62 elasticsearch-7.11.1]$ cd snapshots/

[elasticsearch@756d6b1fdc62 snapshots]$ ll -la
total 60
drwxr-xr-x 1 elasticsearch elasticsearch  4096 Aug  9 18:04 .
drwxr-xr-x 1 elasticsearch elasticsearch  4096 Aug  7 15:37 ..
-rw-r--r-- 1 elasticsearch elasticsearch   437 Aug  9 18:04 index-0
-rw-r--r-- 1 elasticsearch elasticsearch     8 Aug  9 18:04 index.latest
drwxr-xr-x 3 elasticsearch elasticsearch  4096 Aug  9 18:04 indices
-rw-r--r-- 1 elasticsearch elasticsearch 30977 Aug  9 18:04 meta-BqwRM7GQRNGouGLffaJy2g.dat
-rw-r--r-- 1 elasticsearch elasticsearch   269 Aug  9 18:04 snap-BqwRM7GQRNGouGLffaJy2g.dat
```
#### Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.
```
[elasticsearch@756d6b1fdc62 snapshots]$ curl -X DELETE 'http://localhost:9200/test?pretty'
{
  "acknowledged" : true
}

[elasticsearch@756d6b1fdc62 snapshots]$ curl -X PUT localhost:9200/test-2?pretty -H 'Content-Type: application/json' -d'{ "settings": { "number_of_shards": 1,  "number_of_replicas": 0 }}'
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "test-2"
}

[elasticsearch@756d6b1fdc62 snapshots]$ curl -X GET 'http://localhost:9200/_cat/indices?v' 
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 PjC0NSR1SgK-rgHGfaP-Rw   1   0          0            0       208b           208b
```
#### Восстановите состояние кластера elasticsearch из snapshot, созданного ранее.
#### Приведите в ответе запрос к API восстановления и итоговый список индексов.
```
[elasticsearch@756d6b1fdc62 snapshots]$ curl -X POST localhost:9200/_snapshot/netology_backup/elasticsearch/_restore?pretty -H 'Content-Type: application/json' -d'{"include_global_state":true}'
{
  "accepted" : true
}

[elasticsearch@756d6b1fdc62 snapshots]$ curl -X GET http://localhost:9200/_cat/indices?v
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   test-2 PjC0NSR1SgK-rgHGfaP-Rw   1   0          0            0       208b           208b
green  open   test   VSUWgJHsQrqRDvCCiPxQ7w   1   0          0            0       208b           208b
```
