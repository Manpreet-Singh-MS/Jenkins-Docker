Installation
============

## Build the Jenkins BlueOcean Docker Image

1. <code>docker pull jenkins/jenkins</code>
2. <code>docker build -t myjenkins-blueocean:2.414.2-1 .</code>

You might face some error during  the build like (unable to locate package docker-ce-cli)
so before proceeding make sure you run the following commands :-

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


docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind \
  --storage-driver overlay2


### Windows

docker run \
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
  myjenkins-blueocean:2.414.2-1 


Get the Password
================

<code>docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword</code>

Connect to the Jenkins
======================

https://localhost:8080/

Connect to Jenkins shell
========================

<code>docker exec -it jenkins-blueocean bash</code> -> Non-Root Access

<code>docker exec -it -u 0 {CONTAINER ID} /bin/bash</code> -> Root Access

Python Installation in Jenkins Container
========================================

**__Python Package does not comes with base image of Jenkins so we required to access the Jenkin shell with Root Access to download the Python Package.__**

### Following command is to be executed to gain the root access :- ###
   

<code>docker exec -it -u 0 {CONTAINER ID} /bin/bash to open an interactive terminal within the Docker Container as root (user 0).</code>

<code>apt-get update and apt-get install python3 and apt-get install python3-pip to install Python3 and pip within the Docker container.</code>






