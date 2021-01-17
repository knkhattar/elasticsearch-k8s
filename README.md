10.1.162.132# elasticsearch-k8s


# Docker setup

sudo service docker start

sudo docker pull docker.elastic.co/elasticsearch/elasticsearch:6.8.13
sudo docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.8.13

# APIs

* curl -X GET "localhost:9200/_cat/nodes?v&pretty"
* curl -X GET http://10.1.162.132:9200/_search?pretty=true
* curl -X GET http://10.1.162.132:9200/_cat/indices?pretty=true
* curl -X PUT http://10.1.162.132:9200/dummy-index
* curl -X GET http://10.1.162.132:9200/dummy-index/_search?pretty=true

* curl -d '{"@timestamp":"2099-11-15T13:12:00", "user":{"id":"testId"},"event":{"name":"login_success","type":"login"}}' -H "Content-Type: application/json" -X POST http://10.1.162.132:9200/dummy-index/_doc

* curl -X GET http://localhost:9200/dummy-index/_search?pretty=true

# Docker - Multi node - Persistence Test

 * 

sudo docker-compose -f docker-compose-v1.yml up -d

sudo docker-compose -f docker-compose-escom-v2.yml up -d
sudo docker-compose -f docker-compose-escom-v2.yml down -d

docker logs containerId -f

sudo sysctl -w vm.max_map_count=262144//need to check why is it required

you can go hardcore and write the Kubernetes YAML files yourself. It is not too difficult to pull off, just has a few gotchas (e.g. setting vm.max_map_count, securityContext.fsGroup: 1000, a correct readinessProbe, anti-affinity, etc).

# Kubernetes Sigle Node 

kubectl apply -f k8s-es-signle-node.yml
Test APIs

# Kubernetes Sigle Node with Persistence

kubectl apply -f k8s-es-signle-node-persistence.yml
Test APIs

# Kubernetes Multi Node Stateful

kubectl apply -f k8s-es-multi-node-stateful.yml
Test APIs

# kubernetes equivalent of esdata

# microk8s install
sudo snap install microk8s --classic


