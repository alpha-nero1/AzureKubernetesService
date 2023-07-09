## DaemonSet (DS)
A DaemonSet is a k8s object that ensures a copy of a pod that is defined in the configuration is always available on every worker node on a cluster.

It is used when you need to run a specific task or service on **every node** in the cluster, such as log collection, monitoring, or a network daemon.

DaemonSets provide a way to manage cluster-level daemons or background tasks that need to run on each node without the need for manual intervention

## CSI (Container Storage Interface)
CSI is a standard used for providing storage services to containerised applications.

## Konnectivity Agent (KA)
`konnectivity-agent` is a deployment running on worker nodes and communicates with `konnectivity-server` which is a container on the API server. Their goal is to provide a TLS encrypted channel of communication between the API server and kubelet.

k get deploy -n kube-system
k get pod -n kube-system | grep konn
k logs konnectivity-agent-7bfffd9c6d-2rtnj
-- Makes node unschedulable. Meaning you can delete components and they will not be recreacted.
k cordon <node>

## Kube Proxy (KP)
`kube-proxy` is a DS in AKS responsible for maintaining network rules on nodes and handling network traffic to and from pods.

A kubernetes service routes traffic to particular pods, this is actually acheived via kube-proxy as it configures the iptables rules on each node to forward traffic to the pods exposed by that service.

iptables -n -t nat -L KUBE-SERVICES

-- See KP logs.
k logs --selector component=kube-proxy

k get endpoints

## Metrics Server (MS)
A deployment which acts as an aggregator of resource usage like CPU and memory.
Fetches metrics from the kubelet running on each node and exposes them via the API server.

Any of the scalers use this as when a certain memory usage is reached there may be a rule to scale.

k get deploy
-- See cpu and memory of nodes and pods.
-- top will not work without metrics-server.
k top node
k top pod

k get pod | grep metric

# Azure Resources

## Virtual Machine Scale Set (VMSS)
Group of similar and load balanced VMs that are easily scalable.
Each VMSS is controlled by an AKS node pool which represents a group of nodes with the same configuration.

VMSS ~= your node pool.