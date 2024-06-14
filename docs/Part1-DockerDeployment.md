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

## Making flask App container out of images 

Making container out of image. go to [app.py](../app.py) in the last line the developers have mentioned on which port the app will be running.
```
docker run -d -p 5000:5000 flaskapp:latest
docker ps
```
-d detached mode , runs the app in background , doesn't show you the app terminal
-p mapping port number 'host(ec2):continerPort' 
docker ps shows all the containers which are running

By default we don't have access to ec2 on port 5000, so we change the inbound rules in ec2 to allow on port 5000

now copy the ec2 public ip, past  and add ':5000' , you'll see sql error but atleast we know our we have some app running, we need to spin up sql container

## Making Sql container out of images

spinning up one more container for sql database, where sql image will get pulled from dockerhub public repository

```
docker run -d -p 3306:3306 --name=mysql -e MYSQL_ROOT_PASSWORD="password" mysql:5.7
```
name == db name , we set root password, mysql:5.7 image gets pulled from dockerhub

## connectinng both the containers in a network

create a  network

```
docker network create twotiernet
docker nnetwork ls 
```

nnow we attach both the containers to the same net id
```
docker ps
docker network create networkID containerID
```

so now we have to add environment variable so we kill both containers and spin the containers again. (task - we can add environment variable to containers)
```
docker kill containerID
```

new command to run for bboth containers adding their environment variables abd attching them to same network. we can inspect the network
```
docker run -d -p 5000:5000 --network=twotiernet -e MYSQL_HOST=mysql -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_DB=myDB --name=flaskapp flaskapp:latest

docker run -d -p 3306:3306 --network=twotiernet -e MYSQL_DATABASE=mysql -e MYSQL_USER=admin -e MYSQL_PASSWORD=admin -e MYSQL_DB=myDB --name=mysql flaskapp:latest

docker network inspect
```
now we will create table inside the database, the sql script is given in [message.sql](../message.sql) , create a file of message.sql as we will need it later 

how to we create the table inside the container? by going inside the container!
how do we go inside the container? see below
```
docker exec -it containerID bash
my sql -u root -p
```
docker executes iteractive terminal (-it) as we enter the container opening bash shell, starting mysql we insert username (-u) and password (-p)
we are inside mysql now, now we create a table inside the databe we created , our database name is 'mysql'

```bash
show databases
```
paste the script in [message.sql](../message.sql)  to create table

and we are done, see if the app is running and you can see a UI and insert a message 

to see the message in database on sql container. enter mysql and type select * from messages

it will display all the message that you had inserted



