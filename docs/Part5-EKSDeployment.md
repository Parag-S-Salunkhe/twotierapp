Worker Node and Master Node in EKS:

Worker Nodes: You can view worker nodes using kubectl get nodes. These are EC2 instances managed by EKS to run your applications.
Master Node: EKS is a managed service, so you don't have direct access to the master nodes. AWS manages the control plane (master nodes) for you.
kubectl vs eksctl:

kubectl: Use kubectl for interacting with Kubernetes clusters in general, including EKS, to manage pods, deployments, services, etc.
eksctl: Use eksctl for managing the lifecycle of your EKS clusters, including creating, updating, deleting clusters, managing node groups, and configuring cluster settings.
