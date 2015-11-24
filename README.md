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
The screencast of File IO can be found in [this link](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/blob/master/FileIO/ScreenCast_FileIO.mp4)

####2.Ambassador Pattern:
Created a virtual machine in Virtual Box and installed docker and docker compose.

Used local machine as one of the machine and installed docker and docker compose in it.

Run redis-server and server-ambassador on local machine.

Run client-ambassador and client-redis on virtual machine.

######Command to execute on local machine (acts as server):
```
sudo docker-compose up
```
######Command to execute on virtual machine (acts as client):
```
sudo docker-compose run client-redis
```
Then set/get keys in client

The screencast of Ambassador Pattern can be found in [this link](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/blob/master/Ambassador%20Pattern/ScreenCast_AmbassadorPattern.mp4)

####3.Docker Deploy:

##### Node App and Dockerfile
Node App and Dockerfile exists in [this](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/tree/master/FileIO) folder

[post-commit hook](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/blob/master/DockerDeploy/post-commit) in App is configured to build the image named ncsu-app and pushes it to registry

[post-receive hook of blue slice](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/blob/master/DockerDeploy/post-receive%28blue%20slice%29) has been configured to get the lastest image from registry and build the container named blueapp. The container blueapp maps host port 50100 to docker post 8081.

[post-receive hook of green slice](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/blob/master/DockerDeploy/post-receive%28green%20slice%29) has been configured to get the lastest image from registry and build the container named greenapp. The container greenapp maps host port 50101 to docker post 8081.

######Steps followed to demonstrate Docker Deploy:
1. Make a change to main.js in App

2. Commit the changes : post-commit hook is configured to build the image named ncsu-app and pushes it to registry

3. Deploy to blue slice : git push blue master
	-In post-receive file of blue.git, it has been configured to get the lastest image from registry and build the container named blue app.
	-It maps host port 50100 to docker post 8081.
	-So we can test using "curl -i localhost:50100".

4. Repeat step 3 with green slice
	-It maps host port 50101 to docker post 8081
	-So we can test using "curl -i localhost:50101"

5. Make a change to main.js in App and commit the changes

6. Deploy only to blue instance, with no changes to green
	- Only blue instance gets reflected with the changes but not green

The screencast of Docker Deploy can be found in [this link](https://github.com/rkvardhi/DevOps_HW4_AdvancedDocker/blob/master/DockerDeploy/Screencast_DockerDeploy.mp4)
