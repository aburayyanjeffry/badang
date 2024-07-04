# Chapter 6
<div style="text-align: center;">
  <img src="/img/chap6-rbac.png" alt="Description of the image" width="500"/>
</div>
In this chapter, we will cover Kubernetes Role-Based Access Control (RBAC).

## 1. Download the Correct kubectl version
The kubectl version that comes with MicroK8s has cluster admin privileges. To operate with a non-admin role, we need a version of kubectl that supports customizable kubeconfig files.

First, check the cluster version:
```
k version
```

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
cd 
mkdir .kube
sudo microk8s config > .kube/config
```

After generating the kubeconfig, attempt to use the host's kubectl version:
```
kubectl get all
```

To simplify usage, set up a shortcut alias and enable bash completion. **Note: We will alias MicroK8s's kubectl to 'mk' and the host's kubectl to 'k':**
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

Create namespaces named `lab01` and `lab02`
```
k create namespace lab01
k create namespace lab02
```

Kubernetes does not manage user and user credentials. We need to create manually. One of the way is to create it by using openssl. In this example the user name is `user01`. The current user need to be added  to `microk8s` group because it need to access its CA cert.
```
sudo usermod -aG microk8s $USER
export K8SUSER=user01
openssl genrsa -out $K8SUSER.key 2048
openssl req -new -key $K8SUSER.key -out $K8SUSER.csr -subj "/CN=$K8SUSER"
openssl x509 -req -in $K8SUSER.csr -CA /var/snap/microk8s/current/certs/ca.crt -CAkey /var/snap/microk8s/current/certs/ca.key -CAcreateserial -out $K8SUSER.crt -days 9999
```

Create the user context in the kubeconfig. In this example the user01 context is named `user01 and having `lab01` as the default namespace.
```
k config set-cluster microk8s-cluster --server=https://127.0.0.1:16443 --certificate-authority=/var/snap/microk8s/current/certs/ca.crt
k config set-credentials $K8SUSER --client-certificate=$K8SUSER.crt --client-key=$K8SUSER.key
k config set-context $K8SUSER --cluster=microk8s-cluster --user=$K8SUSER --namespace=lab01
```

Now list the available contexts. We should see the new context
```
k config get-contexts
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
k apply -f rolebinding-lab01-admin.yaml
```

User the user01 context
```
k config use-context user01 
```

Let's deploy an Nginx server to lab01 and lab02. You should be able to deploy to lab01 because we have already assigned the cluster admin role to the user for lab01. However, the deployment to lab02 will fail because the user doesn't have any role in this namespace.
``
k create deployment mynginx --image=nginx -n lab01
k create deployment mynginx --image=nginx -n lab02
```

Execute this to reset back the kubeconfig
```
sudo microk8s config > .kube/config
```

