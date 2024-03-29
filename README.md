# kubernetes-argocd
This repository deploys an ArgoCD instance in a kubernetes cluster.

## Installation
### 1. Create the ArgoCD namespace
```
kubectl apply -f argocd-namespace.yaml
```

### 2. Create the ArgoCD artifacts
Download the last version of the installation yaml from:
```
wget https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```

Note that argocd-server service has been updated to be LoadBalancer type and uses custom metallb annotation. Please, update it as you need.

Deploy the ArgoCD artifacts with:
```
kubectl apply -f install.yaml -n argocd
```

### 3. Access the UI as superuser
Connect to your ArgoCD instance following your network configuration.

You can get the admin user password with the following command:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 4. Download ArgoCD Client
Beside the UI, you can also access your ArgoCD instance throught a client. Download it from https://github.com/argoproj/argo-cd/releases/latest.

I've downloaded the linux client and added it to the PATH:
```
wget https://github.com/argoproj/argo-cd/releases/download/v2.1.7/argocd-linux-amd64
chmod +x argocd-linux-amd64
ln -s $PWD/argocd-linux-amd64 /usr/bin/argocd
```

### 5. Create an ArgoCD Project
In order to structure your k8s deployments, create an specific project for your needs.

You can navigate the UI to create it or use a declarative file. I have an example in `projects` directory:
```
kubectl apply -f projects/calfolguera-project.yaml
```

### 5. Create an ArgoCD Application
Once your installation is Up&Running, now you can start deploying your apps. 

You can follow the official documentation to deploy a test application:
https://argo-cd.readthedocs.io/en/stable/getting_started/#6-create-an-application-from-a-git-repository


Applications can be defined in a declarative way as yaml files. I have some examples in `applications` directory:
```
kubectl apply -f applications/grafana-application.yaml
```

## References
Get the official documentation at https://argo-cd.readthedocs.io/en/stable/getting_started/

## Next steps
* Add Persistent storage

## Known Issues
TODO
