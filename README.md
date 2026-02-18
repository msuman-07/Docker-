🐳 Docker Installation & Swarm Setup on AWS EC2

This repository contains step-by-step commands and configurations for:

✅ Docker installation on AWS EC2 Linux
✅ Container creation & image management
✅ Dockerfile creation & volume setup
✅ Port mapping & Apache deployment
✅ Docker Hub image push
✅ Docker Swarm cluster setup (Manager + Workers)
✅ Service deployment in Swarm

📌 Table of Contents

Docker Installation on EC2 Linux

Basic Docker Commands

Create and Manage Containers

Create Docker Image using Commit

Create Image using Dockerfile

Docker Volume Creation

Port Mapping & Apache Deployment

Push Image to Docker Hub

Docker Swarm Setup

Deploy Services in Swarm

🚀 Docker Installation on EC2 Linux

Login to EC2 instance:

sudo su
yum update -y
yum install docker -y


Verify installation:

which docker
docker --version


Start Docker service:

service docker status
service docker start
service docker status


Run a test container:

docker run -it ubuntu /bin/bash


Exit container:

exit

📦 Basic Docker Commands

Clear screen:

Ctrl + L


List images:

docker images


List containers:

docker ps -a


Running containers only:

docker ps

🐋 Create and Manage Containers

Create container:

docker run -it ubuntu /bin/bash


Create named container:

docker run -it --name suman ubuntu /bin/bash


Start container:

docker start suman


Attach to container:

docker attach suman


Remove container:

docker rm suman

🖼 Create Docker Image using Commit

Create files inside container:

cd tmp
touch file1 file2 file3 file4


Create image from container:

docker commit suman newimage


Verify image:

docker images


Run container from image:

docker run -it --name testcon newimage /bin/bash

🧱 Create Image using Dockerfile

Create Dockerfile:

vi Dockerfile


Add content:

FROM ubuntu
WORKDIR /tmp
RUN echo "hello learner" > /tmp/testfile
COPY testfile1 /tmp


Save file → Esc → :wq

Create file to copy:

touch testfile1


Build image:

docker build -t docfileimg .


Run container:

docker run -it --name docfilecon docfileimg /bin/bash

💾 Docker Volume Creation

Edit Dockerfile:

vi Dockerfile


Add:

FROM ubuntu
VOLUME ["/myvolume"]
WORKDIR /tmp
RUN echo "hello learner" > /tmp/testfile
COPY testfile1 /tmp


Build image:

docker build -t myimg .


Run container:

docker run -it --name container2 myimg /bin/bash


Create files in volume:

cd /myvolume
touch file4 file5


Share volume with another container:

docker run -it --name container3 --privileged=true --volumes-from container2 ubuntu /bin/bash

🌐 Port Mapping & Apache Deployment

Run container with port mapping:

docker run -td --name techcon -p 80:80 ubuntu


Enter container:

docker exec -it techcon /bin/bash


Install Apache:

apt-get update
apt-get install apache2 -y


Create webpage:

cd /var/www/html
echo "training" > index.html


Start Apache:

service apache2 start
service apache2 status


Check port mapping:

docker port techcon

☁️ Push Image to Docker Hub

Create container and files:

docker run -it --name test1 ubuntu /bin/bash
touch file1 file2 file3 file4 file5


Create image:

docker commit test1 image1


Login to Docker Hub:

docker login


Tag image:

docker tag image1 <dockerhub-username>/projectlearn


Push image:

docker push <dockerhub-username>/projectlearn

⚡ Docker Swarm Setup
Requirements

3 EC2 instances

1 Manager node

2 Worker nodes

Docker installed on all instances

Install Docker (Ubuntu instances):

sudo su
apt-get update
apt install docker.io

Initialize Swarm (Manager Node)
docker swarm init --advertise-addr <MANAGER-IP>


You will get a join token.

Join Worker Nodes

Run the generated command on worker nodes:

docker swarm join --token <TOKEN> <MANAGER-IP>:2377


Verify nodes:

docker node ls

🚀 Deploy Services in Swarm

Create service:

docker service create --name mysvc --replicas 4 -p 8006:8080 tomcat


Check services:

docker service ls


Check service tasks:

docker service ps mysvc


Access application:

http://<IP-ADDRESS>:8006/

Deploy Apache Service
docker service create --name mysvc2 --replicas 4 -p 8008:80 httpd


Access Apache:

http://<IP-ADDRESS>:8008/


Remove service:

docker service rm <serviceName>

👨‍💻 Author

Suman M
