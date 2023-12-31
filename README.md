Installation
============

## Build the Jenkins BlueOcean Docker Image

1. <code>docker pull jenkins/jenkins</code>
2. <code>docker build -t myjenkins-blueocean:2.414.2-1 .</code>

**__You might face some error during  the build like (unable to locate package docker-ce-cli) so before proceeding make sure you run the following commands :-__**

1. <code>sudo apt-get update</code>
2. <code>sudo apt install apt-transport-https ca-certificates curl software-properties-common</code>
3. <code>curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add </code>-  (This might work as it is deprecated)
4. <code>sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu kinetic stable"</code>

Create the network 'jenkins'
============================

<code>docker network create jenkins</code>


Run the Container
===================

### MacOS / Linux


<code>docker run
--name jenkins-docker
--rm
--detach
--privileged
--network jenkins
--network-alias docker
--env DOCKER_TLS_CERTDIR=/certs
--volume jenkins-docker-certs:/certs/client
--volume jenkins-data:/var/jenkins_home
--publish 2376:2376
docker:dind
--storage-driver overlay2</code>


### Windows

<code>docker run \
  --name jenkins-blueocean \
  --restart=on-failure \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2-1</code>


Get the Password
================

<code>docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword</code>

Connect to the Jenkins
======================

<code>https://localhost:8080/</code>

Connect to Jenkins shell
========================

<code>docker exec -it jenkins-blueocean bash</code> -> Non-Root Access

<code>docker exec -it -u 0 {CONTAINER ID} /bin/bash</code> -> Root Access

Python Installation in Jenkins Container
========================================

**__Python Package does not comes with base image of Jenkins so we required to access the Jenkin shell with Root Access to download the Python Package.__**

#### Following command is to be executed to gain the root access :- ####
   

<code>docker exec -it -u 0 {CONTAINER ID} /bin/bash</code> to open an interactive terminal within the Docker Container as root (user 0).

<code>apt-get update</code> and <code>apt-get install python3</code> and <code>apt-get install python3-pip</code> to install Python3 and pip within the Docker container.

Pull and run the image alpine/socat
===================================

<code>docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
</code>

<code> docker inspect <contrainer_id> </code>

**The above container will be used as a external Docker container for jenkins** 

Jenkins-agent-python
====================

<code>docker pull masprtech1/jenkins-agent-python</code>

Build Image :- [Docker File link](https://github.com/Manpreet-Singh-MS/Docker/blob/main/python_image_JDK11_Alpine/Dockerfile)
