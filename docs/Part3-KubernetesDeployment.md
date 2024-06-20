## Part 3 - Kubernetes Deployment

There are 5 main components of kubernetes:

1) Pod
A Pod is the smallest and simplest Kubernetes object. It represents a single instance of a running process in your cluster.
Pods contain one or more containers, such as Docker containers. All containers in a Pod share the same network namespace, including IP address and ports, and can communicate with each other using localhost.

2) Deployment
A Deployment is a higher-level abstraction that manages Pods. It provides declarative updates to applications and manages the lifecycle of Pods.
With a Deployment, you can define how many replicas of a Pod should run, perform rolling updates, and rollbacks.
Deployments ensure that the desired state of your application is maintained and provide self-healing capabilities by automatically replacing failed Pods.

3) Service
A Service is an abstraction that defines a logical set of Pods and a policy by which to access them.
Services enable communication between different sets of Pods or between Pods and other services by providing a stable IP address and DNS name. 
Kubernetes supports different types of services such as ClusterIP (default), NodePort, and LoadBalancer to manage internal and external traffic.

4) Persistent Volume (PV)
A Persistent Volume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
PVs are cluster resources that exist independently of Pods, and they provide a way to store data persistently. PVs are used to store data that needs to persist even if the Pod accessing it is deleted or rescheduled.

5) Persistent Volume Claim (PVC)
A Persistent Volume Claim (PVC) is a request for storage by a user. It is similar to a Pod requesting compute resources. 
PVCs specify the size and access modes (e.g., ReadWriteOnce, ReadOnlyMany, ReadWriteMany) required by the application. PVCs are bound to PVs, and once bound, the PVC can be used by Pods to access the storage.
