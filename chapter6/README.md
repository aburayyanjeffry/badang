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

Enable RBAC at mickrok8s
```
microk8s enable rbac
```

Create namespaces named lab01 and lab02
```
k create namespace lab01
k create namespace lab02
```

Kubernetes does not manage user and user credentials. We need to create manually. One of the way is to create it by using openssl.
```
export USER=user01
openssl genrsa -out $USER.key 2048
openssl req -new -key $USER.key -out $USER.csr -subj "/CN=$USER"
openssl x509 -req -in $USER.csr -CA /var/snap/microk8s/current/certs/ca.crt -CAkey /var/snap/microk8s/current/certs/ca.key -CAcreateserial -out $USER.crt -days 999
```

Create the kubeconfig for the user
```
kubectl config set-cluster microk8s-cluster --server=https://127.0.0.1:16443 --certificate-authority=/var/snap/microk8s/current/certs/ca.crt
kubectl config set-credentials $USER --client-certificate=$USER --client-key=$USER.key
```



Create the rolebinding-lab01-admin.yaml file. This is a manifest file to bind the cluster-admin role to the user user01 for the namespace lab01.
```
cat <<EOF > rolebinding-lab01-admin.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: admin-rolebinding
  namespace: lab01
subjects:
- kind: User
  name: user01
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: admin
  apiGroup: rbac.authorization.k8s.io
EOF
```

Apply the manifest file to put it into effect:
```
kubectl apply -f rolebinding-lab01-admin.yaml
```

Check the available context
```
kubectl config get-contexts 
```

User the user01 context
```
kubectl config use-context user01 
```
