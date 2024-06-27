## Installations and Pre-requisites

1) Launch a instance, its goong be our entrypoint to the eks cluster.

2) Setup IAM user to give access (for now attach Administrator policy), gives all access. 
Create access key and secret access key we will need it for congifuring

3)ssh into the instance, install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
```
Setup User access
```
aws configure  ## here it'll prompt for secret access key and access key and the region you want to be in 
```

4) Install Docker
```
sudo apt-get update
sudo apt install docker.io
docker ps
sudo chown $USER /var/run/docker.sock
```

5) Install Kubectl 
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

6) Install eksctl
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
This instance is not part of the eks cluster, it is acting like the kubectl or management node.
----------------------------------------------------------
## Launch EKS cluster

It will take 10 to 15 minutes to launch the instance
```
eksctl create cluster --name NameOf-Cluster --region eu-west1-1 --node-type t2.micro --nodes-min 2 --nodes-max 3
```
node tyoe == Innstance tyoe, node min and max == desired state and scalable max state

there are 2 option for running workloads on eks - fargate (serverless) and currently we are using ec2

-------------------------------------
## Manifest files
manifest file for eks are almsot same as part 3. but we remove pod, pv and pvc. 
As ebs is attached to the instance we dont need to attach any new pv and pvc.

----------------------------------------------------------
Worker Node and Master Node in EKS:

Worker Nodes: You can view worker nodes using kubectl get nodes. These are EC2 instances managed by EKS to run your applications.
Master Node: EKS is a managed service, so you don't have direct access to the master nodes. AWS manages the control plane (master nodes) for you.
kubectl vs eksctl:

kubectl: Use kubectl for interacting with Kubernetes clusters in general, including EKS, to manage pods, deployments, services, etc.
eksctl: Use eksctl for managing the lifecycle of your EKS clusters, including creating, updating, deleting clusters, managing node groups, and configuring cluster settings.
