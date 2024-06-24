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
![kubernetes_component](https://github.com/Parag-S-Salunkhe/twotierapp/assets/45193125/0257835e-26df-4637-bdbd-f27d0732be92)

yaml file = manifest file
All the componentes will be written in manifest files also said yaml file which follows a key value syntax.

what is container D? what is container runtime engine?

## Next steps
Things to remember while writing a yaml file : indentation of yaml shouldd be only 2 spaces. default tab is 4 spaces, whiich can be configured to two spaces)

clone your project in master

### pod file
Start with writing pod yaml file. explanation and file structure of [pod](../k8s/two-tier-app-pod.yml)

To apply a manifest file which will spin up the pod and to get which pods  are running :
```bash
kubectl apply -f podname.yml
kubectl get pods
```

as kubectl is telling worker node to turn up the container , go to worker node as the container runs on worker node check by doing 
```
sudo crictl ps
```
### deployment file
now if we want to scale the pods we will have to make a deployment manifest, follow [deployment file](../k8s/two-tier-app-deployment.yml)
important topic to know more about in a yaml file is labels and selectors.

```bash
kubectl apply -f deploymentfilename.yml
kubectl get pods
```

now your podsd have scalinng capabilities and auto healing as well. to test this follow:
to scale the replicas up or down
```bash
kubectl scale deployment deployment_name --replicas=7
```
now go to worker mode to check sutohealing by killing the pod:
```bash
sudo crictl kill pod_id
```
you can see pods are restarted automatically
the reason is deployment, replica set . stateful set are higher objests in k8's 

### service file

Why do we need service file? as there are many pods running giving access to one pod from the cluster is not reliable, so we create service. we map every pods port to internal port 80
and for users to get access of app we create a Node port( generally between 30000 -32000)
[service file yaml](../k8s/two-tier-app-service.yml) can be followed here. 

change inbound permision to allow Node port (in my case 30007) 

### mysql deployment , persistent volume and  persistent volume claim

creating [mysql deployment file](../k8s/mysql-deployment.yml)
Create [persistent volume manifest](../k8s/mysql-pvc.yml) and [persistent volume claim](../k8s/mysql-pvc.yml)

Persistent volume takes some storage from file system to give it to conatiners and app
- it will be mounted as a filesystem, it means app and users can read andd write files to this volume just like on typical disk
- other method is block storage. block storage is like attaching low level storage like AWS ebs where apps can read and write directyl without any filesystem abstraction.

in this project we have given host path for persistent volume, which is not ideal for production, ok for dev and test. why not ok for prod?
1. node dependency  2. security risks  3. Portability

Alternatives to host_path
1. NFS storage like AWS EBS - independent of single node, data available across the cluster
2. Container storage interface
3. Stateful sets with PVC

while creataing pv and pvc remember to match access modes , they should match the storage required by claim, why?
one pv can only be binded to 1 pvc at a time.

Methods like Dynamic provisioning using storage classes can be used for felxibility, scalibility and efficient use of storage resources in your kubernetes cluster (in prod)

### creating service fo mysql

the service that we created before was for application can be accessed by the outside world , now we have to create service for my sql backend so the the flask app can connect to sql 
creation of my [sql service](../k8s/mysql-service.yml)
apply the manifest file , to see how many services are there:
```bash
kubectl get service
kubectl apply -f mysql-svc.ynl
```
when your sql service is up , copy the cluster ip of mysql service and go to your app deployment file where you see "MYSQL_HOST", replace it with your ip.
to fix the error realte to table perform the same steps we followed in part 1, by entering the container in worker node

