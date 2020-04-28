# springboot-k8s-tanzu-observability
springboot-k8s-tanzu-observability 


# helm for wavefront https://github.com/wavefrontHQ/helm

# clone the loadgen repo
git clone https://github.com/howardyoo/loadgen.git

# create the cluster
kind create cluster --name kind-wf

#  point to the correct cluster context
kubectl cluster-info --context kind-kind-wf
 
 # *****change name***** of cluster to desired in
 ./wavefront-collector-dir/4-collector-config.yml 
####################################################

 ######## manual install bellow ###########
 #deploy wf proxy
 kubectl create -f wavefront.yml

 #deploy kube state metrics server
 kubectl create -f kube-state.yml

 #deploy wavefront collector
 kubectl create -f wavefront-collector-dir


#deploy loadgen app
kubectl create -f loadgen-svc.yml 

 #run loadgen
 kubectl proxy

 # one of several metrics that can be created - cpu/mem
 curl "http://127.0.0.1:8001/api/v1/namespaces/loadgen/services/loadgen-svc/proxy/"

# you can select cpu or mem
curl "http://127.0.0.1:8001/api/v1/namespaces/loadgen/services/loadgen-svc/proxy/cpu"

# to get instructions on how to run
curl "http://127.0.0.1:8001/api/v1/namespaces/loadgen/services/loadgen-svc/proxy/cpu/run"   


# cpu load reqs 2 params threads and duration
curl "http://127.0.0.1:8001/api/v1/namespaces/loadgen/services/loadgen-svc/proxy/cpu/run?threads=5&duration=60"

# for info on run 
curl "http://127.0.0.1:8001/api/v1/namespaces/loadgen/services/loadgen-svc/proxy/cpu/info"

