# Installing Docker on Amazon Linux server

### Pre-requisites
1. Amazon Linux EC2 Instance

## Installation Steps

1. Install docker and start docker services
   ```sh 
   yum install docker -y
   docker --version 
   
   # start docker services
   service docker start
   service docker status
   ```
1. Create a user called dockeradmin
   ```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. add a user to docker group to manage docker 
   ```
   usermod -aG docker dockeradmin
   ```
### Validation test
1. Create a tomcat docker container by pulling a docker image from the public docker registry
   ```sh
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   ```

## Docker Installation on CentOS server
##### Referance URL : https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository
### Pre-requisites

Please follow below steps to install docker CE on CentoOS server instance. For RedHat only Docker EE available 

1. Install the required packages.

   ```sh 
   sudo yum install -y yum-utils \
   device-mapper-persistent-data \
   lvm2
   ```
  
1. Use the following command to set up the stable repository.
 
   ```sh 
   sudo yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo
   ```

### INSTALLING DOCKER CE

1. Install the latest version of Docker CE.
   ```sh 
   sudo yum install docker-ce
   ```

   Note: If prompted to accept the GPG key, verify that the fingerprint matches 
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

1. Start Docker.
   ```sh 
   sudo systemctl start docker
   ```

1. Verify that docker is installed correctly by running the hello-world image.
   ```sh
   sudo docker run hello-world
   ```

   you should see a similar output as below 

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```






