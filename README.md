_Installation_

Build the Jenkins BlueOcean Docker Image

docker build -t myjenkins-blueocean:2.332.3-1 .

You might face some error during  the build like (unable to locate package docker-ce-cli)
so before proceeding make sure you run the following commands :-

1. sudo apt-get update
2. sudo apt install apt-transport-https ca-certificates curl software-properties-common
3. curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  (This might work as it is deprecated)
4. sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu kinetic stable"

_Create the network 'jenkins'_

docker network create jenkins -> docker network ls (Run this command to check the Network list).

_Run the Container_

MacOS / Linux
=============

docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1


Windows
=======

docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.332.3-1

P.S. If you are using a Linux WSL Terminal to Run the commands on Windows make sure you Replace ( ' ) with ( \ ) at the end of each row.


_Get the Password_
==================

docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword


_Connect to the Jenkins_
========================

https://localhost:8080/









