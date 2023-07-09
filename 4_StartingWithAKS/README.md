# Starting with AKS

Download azure cli: https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-macos

az aks reference: https://learn.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest

An AKS cluster can be created either via the az cli or the azure portal.

## Create a resource group
`az group create -l eastus -n aks-rg`

## Create a cluster
`az aks create -g aks-rg -n aks --node-count 2 --generate-ssh-keys`

Generate ssh keys generates private and public key files if missing.

## We can then easily install kubectl with
`az aks install-cli`

`kubectl version`

You can connect to your cluster via kubectl or azure cloud shell.
the azure portal provides instructions on how to do this.

# Virtual Network
Each AKS cluster will have a virtual network and the AKS resources, like nodes will be part of a subnet.

## Pod CIDR
- Represents the address range from where the pods will assign an IP address.
- Large range recommended because cannot be changed after cluster created.

k get node -o wide
k get pod -o wide
sipcalc 10.244.0.0/16

## Service CIDR
Represents address range that internal services will assign IPs from.

k get svc
k get svc -A
sipcalc 10.0.0.10/16

## Docker bridge CIDR
not used by pods or services but reserved for docker operations like docker build.

## Node pool?
Nodes with same config are grouped in pools, pools contain the supporting VMs that run container apps.
- A node pool creation will result in the creation of VMSS (VM Scale Set) and VMAS (VM Availability Set).
- When you create an AKS cluster a node pool is already included.
- You may have a leaner node pool for stage and a beefier one for prod with more resources allocated to it.

### System Node Pool
Used to host system node pool. You need at least one.

### User node pool
Used to host app pods.

## Connect to AKS nodes.
By default AKS nodes do not have public IP assigned.
- To connect to a node we install a high privelage pod on the node that has full access to the node.

k get node -o wide
RUN DEBUG MODE
https://learn.microsoft.com/en-us/azure/aks/node-access

kubectl debug node/<node name> -it --image=mcr.microsoft.com/dotnet/runtime-deps:6.0
kubectl debug node/aks-nodepool1-34319545-vmss000000 -it --image=mcr.microsoft.com/dotnet/runtime-deps:6.0

You are in the node! Try now `hostname`
Will spit out aks-nodepool1-34319545-vmss000000

chroot /host will give you higher privelages

- Node shell makes this all a lot easier
- Install:
curl -LO https://github.com/kvaps/kubectl-node-shell/raw/master/kubectl-node_shell
chmod +x ./kubectl-node_shell
sudo mv ./kubectl-node_shell /usr/local/bin/kubectl-node_shell

Now simply you can invoke: `k node-shell aks-nodepool1-34319545-vmss000000`

## K8s side of AKS cluster
describe deploy coredns -n kube-system
^ will display a label like: addonmanager.kubernetes.io/mode=Reconsile, this means the resource is directly
managed by AKS, and if changed will immediately be reverted/reconciled to it's configured state.

k get deploy -n kube-system
kn kube-system
k describe coredns

## Kubelet
Agent that runs on each worker node and communicates with the control pane.

k get node -o wide
k proxy # Creates a proxy (it will tell you the port) that forwards all traffic to the API server in the AKS cluster from your local.

k proxy is a process that you must exit later on.

curl -sSL "http://localhost:8001/api/v1/nodes/<node>/proxy/configz"

k proxy
curl -sSL "http://localhost:8001/api/v1/nodes/aks-nodepool1-34319545-vmss000000/proxy/configz"


OR

k node-shell aks-nodepool1-34319545-vmss000000
journalctl -u kubelet -o cat # get all node logs.
journalctl -u kubelet -o cat --no-pager # get all node logs.

- Gives you the status of the kubelet, to see if it is running.
systemctl status kubelet


## Containerd
- Installed as a linux service in AKS
- AKS default container runtime, replacing docker in the past.
- Containerd can run any container.
- Manages container images and instances themselves.
- It is OCI (Open Container Initiative) compliant.
- crictl is recommended to troubleshoot inside nodes.

k get node -o wide
k node-shell aks-nodepool1-34319545-vmss000000
systemctl status containerd

- List all images in the node.
crictl images

- List all pods scheduled on node.
crictl pods

- List all containers
crictl ps

- See container logs
crictl logs <container id>
crictl logs eb8ec9af71dad

## Azure ip masquerading agent.
Masquerading in the context of networking referes to hiding the true identity of a device on a network.

## Cloud node manager
DaemonSet that works with the node lifecycle controller to:
- Update nodes with system unique id from cloud provider.
- Annotate and label nodes with cloud specific info, e.g. OS role.
- Get node's hostname and net address.
- Verify health of a node.

## Config Map (CM)
An object that lets you store configurations for other objects to use. A config map is essentially a configuration object.

## Core dns
- Authoritative DNS server. It levergaes plugins which are modular components that provide functions.
- When a client requests to connect to a service in the kluster. It is coredns that resolves that service name to the correct ip address in the cluster.

kubectl logs --namespace kube-system -l k8s-app=kube-dns
k run dnsutils --image registry.k8s.io/e2e-test-images/jesse-dnsutils:1.3 -- sleep infinity
k exec -it dnsutils -- bash
nslookup flask-app-service
nslookup microsoft.com

-- Get config map.
k get cm -n kube-system

-- Get description of core dns config maps.
k describe cm coredns -n kube-system
-- Second one does not have Reconcile label.
k describe cm coredns-custom -n kube-system

-- Restart dns.
k delete pod -n kube-system --selector k8s-app=kube-dns

## Coredns-autoscaler
Tool installed by AKS as a deployment that automatically adjusts the number of coredns replicas.
- Behaviour controlled bty coredns-autoscaler CM

-- View the CM of coredns autoscaler
k describe cm -n kube-system coredns-autoscaler
k get deploy coredns -n kube-system
k get pod -n kube-system | grep coredns

-- Edit coredns CM
k edit cm -n kube-system coredns-autoscaler
k get deploy coredns -n kube-system
