# Orchestrators
Why do we need them:
- For scaling applications.
- For high application availablity.
- Load balancing and resource management.
- Automate updates and rollbacks.
- Manage networking and service discovering.

Kubernetes is a prime example of this. Azure Kubernetes Service is an orchestration manager.

- Uses master and worker nodes.
Master nodes: 
    - hosts control pane responsible for maintaining desired state of cluster.
Worker nodes: 
    - running the containers and report state to master nodes.

## Pods
A pod is the smallest unit in a cluster. Multiple containers can be run inside one pod. Each pod has it's own ip address in the cluster.

Multiple containers in the pod share storage.

## Node
A virtual or physical machine that runs pods.

## Service
Object used to expose pods via a single ip address. You may have a plethora of pods for a single service. You can use a service to load balance traffic to all pods.
