# Домашнее задание к занятию «ELK»  **Шаров Олег**

---

## Задание 1

Установите и запустите Elasticsearch, после чего поменяйте параметр `cluster_name` на случайный.  
Приведите скриншот команды `curl -X GET 'localhost:9200/_cluster/health?pretty'`, сделанной на сервере с установленным Elasticsearch, где будет виден нестандартный `cluster_name`.

**Решение:**

Elasticsearch запущен в Docker-контейнере с помощью следующей команды:

```bash
docker run -d \
  --name elasticsearch \
  -p 9200:9200 \
  -e "discovery.type=single-node" \
  -e "cluster.name=my-elk-cluster-$(date +%s)" \
  -e "xpack.security.enabled=false" \
  -e "ES_JAVA_OPTS=-Xms1g -Xmx1g" \
  -v esdata:/usr/share/elasticsearch/data \
  --ulimit memlock=-1:-1 \
  docker.elastic.co/elasticsearch/elasticsearch:8.12.0
```

После успешного запуска выполнена команда проверки состояния кластера:
```bash
curl -X GET 'localhost:9200/_cluster/health?pretty'
```

![Результат выполнения curl](img/screen_curl.png)


## Задание 2

Установите и запустите Kibana.  
Приведите скриншот интерфейса Kibana на странице `http://<ip вашего сервера>:5601/app/dev_tools#/console`, где будет выполнен запрос `GET /_cluster/health?pretty`.

**Решение:**

Kibana была запущена в Docker-контейнере с использованием host-сети для прямого доступа к Elasticsearch, работающему на том же хосте:

```bash
docker run -d \
  --name kibana \
  --network host \
  -e ELASTICSEARCH_HOSTS="http://localhost:9200" \
  docker.elastic.co/kibana/kibana:8.12.0
```
В браузере перешёл по адресу:
>http://158.160.221.234:5601/app/dev_tools#/console

В консоли Dev Tools выполнил запрос:

```bash
GET /_cluster/health?pretty
```

На скриншоте ниже виден результат выполнения запроса — в частности, нестандартное значение `cluster_name`, подтверждающее корректную работу связки Kibana + Elasticsearch.

![Результат выполнения GET](img/kibana-dev-tools.png
)



