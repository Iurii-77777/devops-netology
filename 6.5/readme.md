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
#### Получите список индексов и их статусов, используя API и приведите в ответе на задание.
#### Получите состояние кластера elasticsearch, используя API.
```
```
#### Как вы думаете, почему часть индексов и кластер находится в состоянии yellow?
#### Удалите все индексы.
```
```
## **Задача 3.**
#### Создайте директорию {путь до корневой директории с elasticsearch в образе}/snapshots.
#### Используя API зарегистрируйте данную директорию как snapshot repository c именем netology_backup.
#### Приведите в ответе запрос API и результат вызова API для создания репозитория.
```
```
#### Создайте индекс test с 0 реплик и 1 шардом и приведите в ответе список индексов.
#### Создайте snapshot состояния кластера elasticsearch.
#### Приведите в ответе список файлов в директории со snapshotами.
```
```
#### Удалите индекс test и создайте индекс test-2. Приведите в ответе список индексов.
#### Восстановите состояние кластера elasticsearch из snapshot, созданного ранее.
#### Приведите в ответе запрос к API восстановления и итоговый список индексов.
```
```
