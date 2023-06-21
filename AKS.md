# AKS
Managed kubernetes service. AKS makes it easy to install and manage k8s clusters.

Locally once you have installed az cli. You can run the `az account set` and `az aks get-credential` commands to connect to your k8s cluster.

`kubectl get node` lists all nodes.

AKS cli actually has extension that you can install via `az extension`

Example commands:
- `az extension list-available --output table`
- `az extension add --name aks-preview` # install aks-preview extension.
- `az extension update --name aks-preview`

## Set up aliases.
Open up and edit ~/.bashrc

```
# Add to ~/.bashrc file
alias k=kubectl
alias kn='k config set-context --current --namespace'
```

`k get deployment` - lists all deployments.
`k delete deploy testapp` - delete testappp deployment.


## Imperative and declarative methods.

### Imp
Make decisions in the moment.
- Step by step instructions.
- Example `k create deployment nginx-imperative --image nginx --replicas 2`

### Dec
Outline desired result (blueprint)
- Use yaml file.
- `k apply -f nginx-declarative.yaml`
- Declarative is preferred and is what AKS is built on.

## Dry run
k create deployment nginx-dryrun --image nginx --replicas 2 --dry-run=client
k delete deploy nginx-dryrun
k create deployment nginx-dryrun --image nginx --replicas 2 --dry-run=client -o yaml # spits out the yaml.
k create deployment nginx-dryrun --image nginx --replicas 2 --dry-run=client -o yaml > dryrun.yaml


k api-resources # lists resources and namespaces
k api-resources -o wide # lists resources and namespaces and get more details


k run nginx --image nginx # create nginx pod
k run flask --image tiangolo/uwsgi-nginx-flask:python3.7 # create flask pod
k get pod -w
k get pod -o wide
k exec -it nginx -- bash # open nginx pod
curl 10.244.0.14
exit

k apply -f k8-objects.yaml
k get cm # get config map
k get secret # get secrets
k get deployment
k get svc # List services!
k get rs # replica set!
k get pod

k exec -it nginx -- bash
curl flask-app-service

k get secret api-key-secret -o yaml # displays endoded yaml.
echo YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXo= | base64 -d # d for decode.

k get ds -A # -A look in all namespaces, ds = daemonset
k get node
k describe node aks-nodepool1-34319545-vmss000000 # describes what pods are running on a node!