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

## Dockerfile 
