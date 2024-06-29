# Chapter 5
<div style="text-align: center;">
  <img src="/img/chap5-helm.png" alt="Description of the image" width="500"/>
</div>
In this chapter, we will explore how to use Helm to install the Prometheus-Grafana stack. Helm serves as the package manager for Kubernetes, much like apt does for Ubuntu. The Prometheus-Grafana stack is a powerful set of tools designed to monitor both applications and Kubernetes nodes.

## 1. Ensure that MicroK8s is running with Helm 3 enabled.
```
sudo microk8s status
```
<div style="text-align: center;">
  <img src="/img/chap5-status.png" alt="Description of the image" width="500"/>
</div>

If it is **not enable**  enable it 
```
sudo microk8s enable helm3
```

## 2. Add the Prometheus-Grafana chart 
This chart is taken from here https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack . 
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

## 3. Install it! 
```
helm install my-kube-prometheus-stack prometheus-community/kube-prometheus-stack --version 61.1.0
```

Ensure the deployment is running
```
k get deployemnt
```
<div style="text-align: center;">
  <img src="/img/chap5-deployment.png" alt="Description of the image" width="500"/>
</div>


## 4. Expose the UI 
Grafana is the UI. Expose the Grafana deployment. 
```
k expose deployment my-kube-prometheus-stack-Grafana --type=NodePort --port=3000 --name=Grafana-ext
```

By doing this we will get a NodePort service for Grafana. We need to identified the expose port.Grafana is the UI. Expose the Grafana deployment.
<div style="text-align: center;">
  <img src="/img/chap5-service.png" alt="Description of the image" width="500"/>
</div>

## 6. Access it!
We can access Grafana at the host ip and the expose port. The defaut user and password is define in the helm chart. (admin/prom-operator)
<div style="text-align: center;">
  <img src="/img/chap5-grafana-login.png" alt="Description of the image" width="500"/>
</div>


You can now access the Grafana dashboard
<div style="text-align: center;">
  <img src="/img/chap5-node.png" alt="Description of the image" width="500"/>
</div>


