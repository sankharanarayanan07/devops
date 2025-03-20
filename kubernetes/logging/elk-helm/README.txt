Adapt the values in the yml file as required. 

The usage is as follows: 

# install elastic stack
helm repo add elastic https://helm.elastic.co
helm repo update 

# Install elastic search
helm install elasticsearch elastic/elasticsearch -f elasticsearch-values.yaml

# Wait and observe
#Watch all cluster members come up.
  kubectl get pods --namespace=default -l app=elasticsearch-master -w
# Retrieve elastic user's password. The user name is elastic 
kubectl get secrets --namespace=default elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d  #58Y84EBOAfNSj65C 
#Test cluster health using Helm test. (optional) 
helm --namespace=default test elasticsearch

# install Filebeat
helm install filebeat elastic/filebeat -f filebeat-values.yaml
# Wait and observe
kubectl get pods --namespace=default -l app=filebeat-filebeat -w

# install Logstash 
helm install logstash elastic/logstash -f logstash-values.yaml
# Wait and observe
kubectl get pods --namespace=default -l app=logstash-logstash -w

#install Kibana
helm install kibana elastic/kibana -f kibana-values.yaml

# Get service and po and ensure all are running
kubectl get svc,po

# forward the Kibana server to port 5601 on windows host
# watch: kubectl get pods --namespace=default -l release=kibana -w 
# get passwd
kubectl get secrets --namespace=default elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d  # user name is elastic
# get token

#1. Watch all containers come up.
  #$ kubectl get pods --namespace=default -l release=kibana -w
#2. Retrieve the elastic user's password.
  #$ kubectl get secrets --namespace=default elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
#3. Retrieve the kibana service account token.
  #$ kubectl get secrets --namespace=default kibana-kibana-es-token -ojsonpath='{.data.token}' | base64 -d

kubectl port-forward --address 0.0.0.0 svc/kibana-kibana 5601:5601 2>&1 >/dev/null & 



