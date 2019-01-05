# Kubernetes
basics of kubernetes

## Kubernetes cluster
- Kubernetes coordinates a highly available cluster of computers that are connected to work as a single unit.
- Kubernetes automates the distribution and scheduling of application containers across a cluster in a more efficient way.

- A Kubernetes cluster consists of two types of resources:

  - The `Master` coordinates the cluster .The Master is responsible for managing the cluster. 
  -  `Nodes` are the workers that runs application. A node is a VM or a physical computer that serves as a worker machine in a Kubernetes cluster.
  - Each node has a `Kubelet`, which is an agent for managing the node and communicating with the Kubernetes master. 
  - The nodes communicate with the master using the Kubernetes API which master exposes.
  - `Minikube` is a lightweight Kubernetes implementation that creates a VM on your local machine and deploys a simple cluster containing only one node.
