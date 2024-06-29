# Chapter 4
<div style="text-align: center;">
  <img src="/img/chap4-microk8s.png" alt="Description of the image" width="500"/>
</div>
Badang's first love is Kubernetes (pronounced as "Kates"), often abbreviated as k8s. It surprised me how easy it is to get k8s running on Ubuntu with just a one-liner! The type of k8s used for the local install is microk8s.

## 1. Install K8s 
```
sudo snap install microk8s --classic
```

## 2. Check the K8s status 
```
sudo microk8s status --wait-ready
```
<div style="text-align: center;">
  <img src="/img/chap4-k8s-status.png" alt="Description of the image" width="500"/>
</div>

## 3. Set the kubeclt alias 
`kubectl` is the command line to interect with k8s. To use kubectl at microk8s is even longer `sudo microk8s kubectl`. To shorten this, we will create an alias for it. Put the following at the .bashrc
```
alias k='sudo microk8s kubectl'
```

Relogin to make it take effect and issue the following command to list all resources in all namespaces.
```
k get all -A
```
<div style="text-align: center;">
  <img src="/img/chap4-k-get-all.png" alt="Description of the image" width="500"/>
</div>


## 5. Lets deploy nginx
Create an nginx deployment
```
k create deployment mynginx --image=nginx
```

Issue the following command to list all resources in the current namespace and ensure the nginx pod is running. Please note that this process may take 30 seconds or more, depending on your machine's capabilities. You may need to execute this command multiple times until you observe the nginx pod running.
```
k get all 
```
<div style="text-align: center;">
  <img src="/img/chap4-nginx-pod.png" alt="Description of the image" width="500"/>
</div>

## 6. Lets expose the nginx.
To expose the nginx deployment to the outside world, we need to create a Kubernetes service using NodePort. Execute the following command to create the NodePort service
```
k expose deployment mynginx --type=NodePort --port=80 --name=mynginx-service
```

Next, list the services to retrieve the exposed port
```
k get service
```

From the output, you can identify the port at which the service is exposed.
<div style="text-align: center;">
  <img src="/img/chap4-nodepod.png" alt="Description of the image" width="500"/>
</div>


You can now access the nginx service in your browser using the external IP and the identified port.
<div style="text-align: center;">
  <img src="/img/chap4-browser.png" alt="Description of the image" width="500"/>
</div>


