START K8S
	sudo service docker start
	sudo snap install microk8s --classic
	sudo microk8s start/stop/status 
ENABLE REQUIRED SERVICES
    Check if helm, dns and storage services are running
    	sudo microk8s status
    If no - enable them	
		microk8s.enable helm3
	
INSTALL ES HELM CHART
	
	sudo microk8s.helm3 repo add elastic https://helm.elastic.co
	microk8s.helm3 pull elastic/elasticsearch --untar=true
	sudo microk8s.helm3 install elasticsearch elastic/elasticsearch -f values.yaml
	
	sudo microk8s.kubectl get all
	sudo microk8s.kubectl describe pod elasticsearch-master-0
	
TEST REST OPERATIONS
	
	sudo microk8s.kubectl port-forward svc/elasticsearch-master 9200
	curl localhost:9200/_cat/indices
	
	curl -X PUT http://localhost:9200/dummy-index
	curl -d '{"@timestamp":"2099-11-15T13:12:00", "user":{"id":"testId"},"event":{"name":"login_success","type":"login"}}' -H "Content-Type: application/json" -X POST http://localhost:9200/dummy-index/_doc
	curl -X GET http://localhost:9200/_search?pretty=true
	
	
TEST PERSISTENCE
	- Stop k8s when stateful set is still running. Start it again and check if indexes are still there.
	- sudo microk8s stop
	- sudo microk8s start
	- sudo microk8s.kubectl get all
	  sudo microk8s.kubectl port-forward svc/elasticsearch-master 9200
	  curl -X GET http://localhost:9200/_search?pretty=true
	curl localhost:9200/_cat/indices
	
	OR CHANGE IMAGE TAG FROM 7.10.0 TO 7.10.1 AND TRY
	
	helm upgrade elasticsearch elastic/elasticsearch -f values.yaml
	
UNINSTALL es HELM IF NOT REQUIRED
    sudo microk8s.helm3 list
	sudo microk8s.helm3 uninstall elasticsearch	
	sudo microk8s.kubectl get all
	
	
	
	
	