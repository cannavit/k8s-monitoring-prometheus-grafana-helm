# Grafana/Prometheus Monitoring configuration with Helm

## Requirements

### Helm. 
 
Helm uses a packaging format called charts. A chart is a collection of files that describe a related set of Kubernetes resources. A single chart might be used to deploy something simple, like a memcached pod, or something complex, like a full web app stack with HTTP servers, databases, caches, and so on.

  [Helm Documentation](https://helm.sh/docs/topics/charts/)
  [Install Helm](https://helm.sh/docs/intro/install/#helm)
 

### Access to Cluster by kubectl

The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs. For more information including a complete list of kubectl operations, see the kubectl reference documentation.

  [Install Kubectl](https://kubernetes.io/docs/tasks/tools/)


## Update package for helm. (LOCALHOST)

You can consult the information about the selected CHART in this link

  [Chart prometheus-community/kube-prometheus-stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack)

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  
    helm repo update
    
## Create the environment:

    kubectl create ns monitoring-metrics
    
## Custom the configuration file. 
  
At this point, you can customize the configuration file. To enable persistent volumes and other required settings

    helm inspect values prometheus-community/kube-prometheus-stack > config_monitoring.yaml
  
  
## Install Grafana and Prometheus in cluster with kubectl (CLUSTER INSTALL)

    helm install -f config_monitoring.yaml monitoring prometheus-community/kube-prometheus-stack --namespace monitoring-metrics  --create-namespace
  
## Get the password of Grafana: 

    kubectl get secret -n monitoring-metrics monitoring-grafana  -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
  
## If you are use Minikube you can follow this steps for expose the serivce.

    kubectl expose service monitoring-grafana  --type=NodePort --target-port=3000 --name=grafana-np -n monitoring-metrics

    minikube service grafana-np
  


  
In this repository is posible to see the  helm configuration for run Grafana and Prometheus monitoring services with presitents volumes
