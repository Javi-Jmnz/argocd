# Project Title

This repository is to trigger ArgoCD when any changes are commited to this repository.

Argo is running on my local but pointing to this Git repo from the Application.yaml in line:
repoURL: https://github.com/Javi-Jmnz/argocd

## Installation

To Install ArgoCD in local machine run:

```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Get the password run:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

result pass: M7zWjUXhLbNTIn-k
```

Make sure to Port Forward to avoid the self bloking of the Pod and ArgoCD 8080 RULs make it point to 8081 let running in a seperate and dedicated console:

```bash
kubectl port-forward svc/argocd-server -n argocd 8081:443
```

You'll get:

```bash
Forwarding from 127.0.0.1:8081 -> 8080
Forwarding from [::1]:8081 -> 8080

Handling connection for 8081
Handling connection for 8081
Handling connection for 8081
...
```

kubectl port-forward svc/argocd-server -n argocd 8081:443

In local go to: https://localhost:8081/applications/argocd/javi-nginx-app?conditions=false&view=tree&resource=

## Run Locally

To Get all the pods:

```bash
kubectl get pods -n argocd
```

```bash
NAME                                                READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                     1/1     Running   0          100m
argocd-applicationset-controller-7b6ff755dc-x7sfp   1/1     Running   0          100m
argocd-dex-server-584f7d88dc-q5sr5                  1/1     Running   0          100m
argocd-notifications-controller-67cdd486c6-wp9np    1/1     Running   0          100m
argocd-redis-6dbb9f6cf4-6ss7g                       1/1     Running   0          100m
argocd-repo-server-57bdcb5898-tdq6w                 1/1     Running   0          100m
argocd-server-57d9cc9bcf-zfmtm                      1/1     Running   0          100m
```

To see that your application got registered run:

```bash
kubectl get applications -n argocd
```

```bash
NAME             SYNC STATUS   HEALTH STATUS
javi-nginx-app   Synced        Healthy
```

To double check your application is registered run:

```bash
kubectl apply -n argocd -f application.yaml
```

```bash
application.argoproj.io/javi-nginx-app unchanged
```

To apply the changes from your application.yaml run:

```bash
kubectl apply -f application.yaml
```
