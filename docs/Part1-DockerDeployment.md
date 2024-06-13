## setting up the environment 

spin up a Ec2 instace ( t2 micro - is ok for this deployment), connect through AWS CLI or CMD

Next steps 

```
sudo apt install update 
sudo apt install docker.io
sudo chown $USER /var/run/docker.sock
```

To install docker we need update the current package list on the system. or you'll face system default ubuntu version old
we are changing the ownership to current user because by default root has the previliges. or you'll face issues for the permissions for running docker commands

clone this project and remove all the files except for app.py for handson

## Write a Dockerfile 

Build a dockerfile- it contains set of instructions to build a docker image . Out of docker images we build docker containers.
To see the structure and explanations got to  [Dockerfile](../Dockerfile)  write/copy the docker file using vim

## Build Dockerfile

We will build and tag the docker image. tagging the docker image is very important . 
```bash
docker build . -t flaskapp
docker images
```
dockerfile is a default name which will be searched during docker build . -t flaskapp, if you have a custom namefor docker file use docker build -f filename . -t flaskapp
docker images prints all the images 

What will happen if we don't tag the docker image ? 

## Making container out of images 

Making container out of image. go to [app.py](../app.py) in the last line the developers have mentioned on which port the app will be running.
```
docker run -d -p 5000:5000 flaskapp:latest
docker ps
```
-d detached mode , runs the app in background , doesn't show you the app terminal
-p mapping port number 'host(ec2):continerPort' 
docker ps shows all the containers which are running

By default we don't have access to ec2 on port 5000, so we change the inbound rules in ec2 to allow on port 5000

now copy the ec2 public ip, past  and add ':5000' , you'll see sql error but atleast we know our we have some app running 


