# Chapter 6
<div style="text-align: center;">
  <img src="/img/chap5-helm.png" alt="Description of the image" width="500"/>
</div>
In this chapter, we will cover Kubernetes Role-Based Access Control (RBAC).

## 1. Download the Correct kubectl version
The kubectl version that comes with MicroK8s has cluster admin privileges. To operate with a non-admin role, we need a version of kubectl that supports customizable kubeconfig files.

First, check the cluster version:
```
k version
```
<div style="text-align: center;">
  <img src="/img/chap5-status.png" alt="Description of the image" width="500"/>
</div>

Next, download and install the appropriate kubectl version that matches your server:
```
Get the kubectl for the local user same version with the server
export PATH=$PATH:/~/bin
curl -LO "https://dl.k8s.io/release/v1.29.4/bin/linux/amd64/kubectl"
mv kubectl ~/bin
chmod +x ~/bin/kubectl
```

Then, generate the kubeconfig file:
```
cd $HOME
mkdir .kube
cd .kube
sudo microk8s config > config
```

After generating the kubeconfig, attempt to use the host's kubectl version:
```
kubectl get all
```

To simplify usage, set up a shortcut alias and enable bash completion. *Note: We will alias MicroK8s's kubectl to 'mk' and the host's kubectl to 'k':*
```
echo "source <(kubectl completion bash)" >> ~/.bashrc
echo "alias k=kubectl" >> ~/.bashrc
echo "complete -o default -F __start_kubectl k" >> ~/.bashrc
sourch ~/.bashrc
```
