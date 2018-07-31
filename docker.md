# Projects overview and needs
The "Healthy Digital House" (Terveellinen Digitalo) project and the TOE-hanke project involves significant amount IOT data (Big Data) and datasources. The needs for storage, data handling, visulisation and demonstration require a flexible and robust solution. In terms of flexibility, the system has to receive and deploy apps which are prone to continuos update releases with zero downtime. In terms of robustness, the system should be capable of self-healing and fault-tolerance in case of hardware failures, e.g. A malfunctioned server node in a cluster. In terms of both robustness and flexibility, applications within the system should be well isolated for better security, but they should still be able to communicate data if necessary.

One important aspect of these projects so far is scalability. For now, both projects are using only one server with one OS instance. In the past, scalling this server was only done vertically, which means adding more capacity and memory within one OS. Since all of our applications are also within that OS, we have no problem giving them more space. However, as our project grows, so does our ways of expanding our server. Cloud storage and horizontal scalling are next two options. Cloud storage demands less infrastructure, but yet it provides as much as the service provides, while horizontal scalling involves joining multiple server into one cluster. Applications should now be able to allocate on any machine within a cluster or in a remote machine and still have access to all the data they need if necessary. The answer to this challenge raise the need for an orchestration system.

These mentioned features of an orchestration system is important as not only do they close the gap between Dev and Ops (Development and Operations) to improve the overall workflow, but also managing and maintenance work are carried out with increased efficiency.



# Software solutions




# Docker architecture concepts

## System architecture
![Image of Docker architecture](https://docs.docker.com/engine/images/architecture.svg)
_Figure 1. Docker system architecture [(source)](https://docs.docker.com/engine/images/architecture.svg)_

### Docker host

### Docker daemon
On a host machine, a docker daemon process manages all docker-related content, namely as containers, images, networks, volumes, etc. In other words, it is a server which is contantly listening to requests, either from host terminal via CLI interface, or via its own rest API from remote terminal.
This whole concept (including docker daemon and its interfaces) is called Docker Engine.

![Image of Docker Engine](https://docs.docker.com/engine/images/engine-components-flow.png)

_Figure 2. Docker engine [(source)](https://docs.docker.com/engine/images/engine-components-flow.png)_

### Docker Objects
Docker Objects are contents managed by Docker daemon. This section will cover only the basics of the two important concepts: images and containers. 


- #### Images

  - Images reprensent the whole application (file system, libraries). Images can only run as containers, thus they are also template for constructing containers. 

  - An image is usually built from a Dockerfile, which contains a set of commands to construct the app, e.g. create files, install libraries. Afterwards, an image is then compressed into a stack of read-only layers. [[a]](https://www.aquasec.com/wiki/display/containers/Docker+Images+101)

    ![image layers](https://docs.docker.com/storage/storagedriver/images/container-layers.jpg)
    _Figure 3. Image layers [(source)](https://docs.docker.com/storage/storagedriver/images/container-layers.jpg)_

  - An example Dockerfile would be:
    ```
    FROM ubuntu:16.04
    sudo apt-get install mysql-server
    ```
    This Dockerfile will took an existing image of ubuntu OS and install a mysql server onto that OS to create a new image. To execute this Dockerfile (using CLI), we open a terminal in the Dockerfile directory:
    ```
    $ docker build -t ubuntu-mysql .
    ```
    The result will be a new image named ubuntu-mysql with the above description.

- #### Containers

  ![Image and containers](https://www.docker.com/sites/default/files/Package%20software.png)

  _Figure 4. Docker containers [(source)](https://www.docker.com/sites/default/files/Package%20software.png)_

  - Containers are executable image instances, hence the ability to start, stop, delete or even exec into a running instance via Docker CLI or API. [[a]](https://docs.docker.com/engine/docker-overview/#docker-objects)

  - Containers characteristic
    - _Lightweight_: Containers are just very lightweight VM instances, with a file system and limited methods for running images. They can be deployed in just matter of seconds. [[a]](https://www.aquasec.com/wiki/display/containers/Docker+Architecture)

    - _Standard_: Unlike VM instances, containers are both hypervisor and OS independent. Containers can be deployed across multiple platforms and infrastructure, and yet they will function the same in every environment.

    - _Secure_: Containers' subsystems such as network, storage define how isolated a container is to other containers and to the host machine. All of which can be configured by defining flags when a container is started or created.

      An example run command including explanation can be found [here](https://docs.docker.com/engine/docker-overview/#docker-objects)

### __Docker Client__
User communicate with Docker daemon from Docker client via either Docker CLI (on same host as Docker daemon) or Docker API (on remote host). Connection from client to more than one daemon is possible.

### __Image Registries__
Images can be shared and download from Docker Registries, which is Docker Hub by default. Using other public and/or private registry is also possible.

An image is pulled with a ```docker pull``` command or with ```docker run``` if the image is not yet existed on local machine. An image is pushed to the referred registry with a ```docker push``` command.


## Data managing in Docker 


### Data storing inside a container
![image layers](https://docs.docker.com/storage/storagedriver/images/container-layers.jpg) 

_Figure 5. Docker image layers [(source)](https://docs.docker.com/storage/storagedriver/images/container-layers.jpg)_

Under the hood, each image is a stack of read-only layers. The top most writable layer is often called container layer, since it is create and use by a container when it starts. Thus instantiating and image with many containers is possible, with each container having their own state of data. [[a]](https://docs.docker.com/storage/storagedriver/#container-and-layers) 

![container layers](https://docs.docker.com/storage/storagedriver/images/sharing-layers.jpg)

_Figure 6. Docker container layer [(source)](https://docs.docker.com/storage/storagedriver/images/sharing-layers.jpg)_

Docker uses a [Copy-on-write]() strategy, which allows its container to access the underlying read-only files from the image, modify them and then store these files on the top most layers. The original files are now obscured.

The only disadvantage is that all data stored in this layer will be destroyed when the container is removed. Therefore, to persist data (save data), Docker provides volumes and bind mounts.

### Bind mounts and Volumes

![Docker storage](https://docs.docker.com/storage/images/types-of-mounts.png)

_Figure 7. Data storage [(source)](https://docs.docker.com/storage/images/types-of-mounts.png)_

Bind mounts and Volumes are just files that are stored on host machine rather than on the container's writable layer. When these files are mounted onto a container, they obscure a file path on that container file system. 
  
>**Note:** Once these files are mounted into containers file paths, especially in case of bind mounts, all changes made by containers will be written onto that file. This is important when mounting host configurations file. 

#### The difference
The biggest difference between these two is that bind mounts use files directly on the host system, while volumes are create and manage by Docker. On linux, volumes are usually created at ```/var/lib/docker/volumes```.

On one host, bind mounts and volumes are almost the same in functionality, although there are minor different behaviors at creation. Read more about [(volumes)](https://docs.docker.com/storage/volumes/) and [(bind mounts)](https://docs.docker.com/storage/bind-mounts/). 

In a cluster with different node workers, only volumes are shared across the nodes. The situation is the same with remote machines sharing a common cloud storage. All volumes, no matter where they are stored, are managed by the ochestration system and their scheduling. For Docker Swarm, a volume driver, e.g. GlusterFS or Cloudstor, enables node workers to access each other's volumes.

#### Use cases
The use cases below for these two mounting types are well specified in the [Docker documentations](https://docs.docker.com/storage/#more-details-about-mount-types) [a]:

>For tips of using volumes or bind mounts see [here](https://docs.docker.com/storage/#tips-for-using-bind-mounts-or-volumes).

- #### Volumes:
  - Sharing data among multiple containers.
  - Host file structure independent.
  - Remote host or cloud storage.
  - Backing up and restore data is easy, since volumes are usually at ```/var/lib/docker/volume/<volume-name>```

- #### Bind mounts:
  
  - >"Sharing configuration files from the host machine to containers. This is how Docker provides DNS resolution to containers by default, by mounting /etc/resolv.conf from the host machine into each container."
  
  - Building artifacts or source code on host machine can be provided to containers in development process. Production built Dockerfile would only need to access the already built artifacts on host machine. 
  
    A good example for this use case should be building and bundling a react app, and then deploy it inside a Docker container. This example is available at the later [project use case]() section.

>**Notes:** Docker also provides tmpfs mounts for large sensitive data that do not need to persist. [Learn more here](https://docs.docker.com/storage/tmpfs/).