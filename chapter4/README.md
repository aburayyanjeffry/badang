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


## 5. Enable normal user to execute Docker commands
Add the current user to `docker` group. Relogin and try to run "Hello World" container to verify.
```
sudo usermod -aG docker $USER
```

## 6. Make Docker to start on boot
```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```



