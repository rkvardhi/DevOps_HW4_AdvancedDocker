# DevOps_HW4_AdvancedDocker

####1.File IO:

##### Registery

Start a private registery on port 5000.

```
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

##### Node App and Dockerfile
Node App and Dockerfile exists in [this](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/tree/master/FileIO) folder

##### Build and test 

Build a container named server for a node js app.

```
docker build -t ncsu-app .
docker run -p 50100:8081 -d --name server ncsu-app
docker logs <containerid>
```
##### Running socat and exposing over port 9001

```
socat -d -d -d -d TCP-LISTEN:9001,fork SYSTEM:'cat /message.txt'
```
##### Linking a container named client to server

```
sudo docker run -d -P --name client --link server ncsu-app
```
##### Accessing the file from client container

```
curl http://server:9001
```

####2.Ambassador Pattern:
Created two virtual machines in Virtual Box and installed docker and docker compose.

Run redis-server and server-ambassador in virtual machine 1.

Run client-ambassador and client-redis in virtual machine 2.

######Command to execute in virtual machine 1:
```
sudo docker-compose up
```
######Command to execute in virtual machine 2:
```
sudo docker-compose run client-redis
```
Then set/get keys in client

The screencast can be found in [this link]()

####3.Docker Deploy:
