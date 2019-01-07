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
  
## Minikube
- install using `sudo apt-get install`
- get version `minikube version
- starting kubernetes on local system `minikube start`
## kubectl
 - its a commandline tool to interact with the kubernetes bootcamp
 - check version `kubectl version`
 - getting cluster details `kubectl cluster-info`
 - This command shows all nodes that can be used to host our applications. `kubectl get nodes`
## Deploying app
- `kubectl` utility is used to deploy apps on kuberbates nodes
- to create a new deployment run `kubectl run kubenetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080`
  - will create a new depolyment with name `kubenetes-bootcamp`
  - will scheduled the application to run on that Node (one in our case)
  - will export port 8080 to external world to interact with app
  - configured the cluster to reschedule the instance on a new Node when needed
- check deployment details `kubectl get deployments`
- pods inside kubernetes are running inside private/isolated network. they are visibile to other pos with in the cluster but not outside.
- The `kubectl proxy` command can create a proxy that will forward communications into the cluster-wide, private network. 
- use `curl` utility to communicae to cluster e;g `curl http://localhost:8001/version`
## Explore your app
- Pods are created when we create deployment modules , pods host application instances. 
- A `Pod` is a Kubernetes abstraction that represents a group of one or more application containers and some shared resources for those containers(volumes,networling ,instruction to how to run images and ports ).
- The containers in a Pod share an `IP Address` and `port` space, are always co-located and co-scheduled, and run in a shared context on the same Node.
- A Pod always runs on a Node.
- A Node can have multiple pods, and the Kubernetes master automatically handles scheduling the pods across the Nodes in the cluster.
- Every Kubernetes Node runs at least:
  - `Kubelet`, a process responsible for communication between the Kubernetes Master and the Node; it manages the Pods and the containers running on a machine.
  - A `container runtime` (like Docker, rkt) responsible for pulling the container image from a registry, unpacking the container, and running the application.
  - Containers should only be scheduled together in a single Pod if they are tightly coupled and need to share resources such as disk.
- The most common operations can be done with the following kubectl commands:
  - `kubectl get` - list resources
  - `kubectl describe` - show detailed information about a resource
  - `kubectl logs` - print the logs from a container in a pod
  - `kubectl exec` - execute a command on a container in a pod
  - getting pod naem `export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
  - communicating to pod `curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/`
  - `kubectl log $PD_NAME`
  - `kubectl exec $POD_NAME env`
  -  getting bash prompt of pod `kubectl exec -it $POD_NAME bash`

## Expose Your App Publicly (kubernetes services)
- A `ReplicaSet` might then dynamically drive the cluster back to desired state via creation of new Pods to keep your application running.
- A `Service` in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them.defined using YAML or JSON
- The `set of Pods` targeted by a Service is usually determined by a `LabelSelector`
- Although each Pod has a unique IP address, those IPs are not exposed outside the cluster without a Service.
- `kubectl get services`
-  creating new service `kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080`
- applying lable to pods `kubectl label pod $POD_NAME app=v1`
- deleting service `kubectl delete service -l run=kubernetes-bootcamp`

## Scaling aplication
- `kubectl scale deployments/kubernetes-bootcamp --replicas=4`  where replicas define nember of replicas os deploymant to create  
## Updating App
-  `Rolling updates` allow Deployment's update to take place with zero downtime by incrementally updating Pods instances with new ones. 
- to update image `- kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2`
- check the status of rollout `kubectl rollout status deployments/kubernetes-bootcamp`
- rollback `kubectl rollout undo deployments/kubernetes-bootcamp`
