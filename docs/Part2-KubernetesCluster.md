![kubernetes architecture](https://github.com/Parag-S-Salunkhe/twotierapp/assets/45193125/1c26b7dc-02f6-4b80-b782-c80803d97830)

8 main components of kubernetes architecture. Master Node which is also called as control plane (as it controls all the activities that will happen in worker node) and Worker node.

Here is an interesting analogy to understand the flow in this architecture.

1. API Server (Team Lead): The API Server is the team lead. It delegates tasks to nodes, handles all communications, and oversees the overall operations within the cluster.
2. Scheduler (HR): The Scheduler acts like HR. It gathers information about pods, ensures everything is functioning correctly within them, and manages pod creation and placement.
3. etcd (Database Administrator): Think of etcd as the database administrator. It stores all the critical information related to the cluster, maintaining its state and configuration.
4. Controller Manager (Project Manager): The Controller Manager functions like the project manager. It ensures the cluster state is as expected, managing node controllers, route controllers,
   service controllers, and volume controllers.
5. Kubelet (Manager): The Kubelet is akin to the manager on the ground. It reports to the API Server, executing tasks related to the nodes based on instructions from the API Server.
6. Kube Proxy (Gatekeeper): The Kube Proxy is like the gatekeeper, providing access to the application by maintaining network rules across nodes for connectivity.
7. CNI (Container Network Interface): The CNI represents the network infrastructure within the Kubernetes cluster, enabling seamless communication between containers.
8. kubectl (CEO): Finally, kubectl is like the CEO. It communicates with the team lead (API Server), issuing commands to manage and operate the cluster.
This analogy simplifies understanding how Kubernetes orchestrates.

As we aim for scalability and resilience, relying solely on the Docker engine can be limiting, as it isn't designed for auto-healing or managing large-scale deployments effectively. 
To address these challenges, we use Kubernetes, a powerful orchestration tool that scales and auto-heals applications.

## Setting Up the Cluster

We will spin up 2 instances of T2 medium

for installation follow [installation guide](https://github.com/LondheShubham153/kubestarter/blob/main/kubeadm_installation.md) 

in summary we will be downloading and installing services to both the instances. we will be initializing kubeadm, give ownership to root and current useer and get token of the master node which helps 
in joining worker nodes to the same network. we will also be creating CNI network

in worker we perform preflight checks which means we will remove kubeadm and kubectl configurations as we want to make it a worker node.

allow inbound traffic on port 6443 on master node (its default port on which k8's api service listens), 

We are using CRI-O instaead of cocker for container runtime interface. instead of 'docker ps' we use 'crictl ps' , same for other commands

after installation to see home many nodes are there use below command on master:
```
kubectl get nodes
```
cluster setup is done , follow next steps at [part 3](https://github.com/Parag-S-Salunkhe/twotierapp/blob/main/docs/Part3-KubernetesDeployment.md)

PS - turn off instances when not in use to not incur costs.
interesting note: you will still incur minimal storage costs, if you want to optimize the costs use terraform to spin up the architecture and destroy it after use.
---------
