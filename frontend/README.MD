# This is HPA steps:

* fisrt need to install metric server in kubernetes. because of the metric server it measure the usage data (like cpu utilization, and memory) from pods and nodes. use below cmd for metric srever installation 

      kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
* Then run our expense-k8 project with resources difination (give request and limit details under the resources:)
* 1. fisrt run mysql 
     kubectl apply -f mysql/manifest.yaml
  2. After that run below cmd for cpu and memory utilization
     kubectl top pod
  3. After run backend 
     kubectl apply -f backend/manifest.yaml
  4. finally frontend
     kubectl apply -f frontend/manifest.yaml

