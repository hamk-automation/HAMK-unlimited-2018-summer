# Projects overview and needs

## Motivations

The "Healthy Digital House" (Terveellinen Digitalo) project and the TOE-hanke project involves significant amount IOT data (Big Data) and datasources. The two projects involves an application base solution which includes storage, data handling, visulization and demonstrations. This application is a combined of several microservices which run on our local server. In the past and just until the last couple of months, our production server were just two small machines sitting in an office in Valkeakoski. As a new physical server was installed in HAMK's Datacenter located in Hämeenlinna, we have to come up with a system that should not only be able to migrate our existing programs with most of their data, but also provide us a better way of managing our micro services.

## Our needs

One important aspect we are heading for is scalability. For now, both projects are using only one server with one OS instance. In the past, scalling this server was only done vertically, which means adding more capacity and memory within one OS. Since all of our applications are also within that OS, we have no problem giving them more space. However, as our project grows cloud storage and horizontal scalling are the next two options. Cloud storage demands less infrastructure, but yet it provides as much as the service provides, while horizontal scalling involves joining multiple server into one cluster. Applications should now be able to allocate on any machine within a cluster or in a remote machine and still have access to all the data they need if necessary. This challenge raises the need for containerized applications and an orchestration system.

Container is an amazing technology, because they are flexible and robust. Containerized apps are deployed within seconds, and they function regardless of the environment and infrastructure we provide. In an ideal situation, all of our containerize apps would be stateless, meaning they do not contain any data, but rather all data is stored somewhere else. Only these data need to be migrated for our migration task to complete. Containerized apps is also the answer for an improved and optimized workflow. Since containers can be shared and deployed quickly, they close the gap between Dev and Ops (Development and Operations). At the time of writing this document, our current server is still young and expansion is yet to come, thus orchestration will not be our main target. Instead, we focus on bringing a closer look on containerizing apps.

# Software solutions

## Virtual machines (VM) and containers
![VMs and containers](https://twistlock2.wpengine.com/wp-content/uploads/2017/01/ContainerVsVM-1024x567.jpg)

_Figure 1. VMs and containers infrastructures [(source)](https://twistlock2.wpengine.com/wp-content/uploads/2017/01/ContainerVsVM-1024x567.jpg)_

### The difference
The two concept is best visulize by an analogy, as this [ebook](https://goto.docker.com/rs/929-FJL-178/images/Docker-for-Virtualization-Admin-eBook.pdf?mkt_tok=eyJpIjoiWm1FM09HRTROREF5TW1abSIsInQiOiJGcnJNQWFQRWVTSEh1YjBnanJIc0hVOWV5R2pneW5GSVY0dGF1VzNZdjhESUdQZkcxQ2g3S2ZQWDc1Q2JQYjB4bFYrTkVPZ2pxbis1OXlQUUVtcTNHT1k1WFFTUUErVVJOTHVGTGV3eHE5M3RabUxhbkIzc1FlNHpGMXVGRTlITyJ9) [[a]]() from Docker suggest: 

>"The analogy we
use here at Docker is comparing houses (virtual machines) to
apartments (Docker containers)." (Docker, 2016)

Virtual machines as houses are well protected against unwanted guests and have their own infrastructures. Since VMs are built from fully powered operating system, quite often they are under used in smaller projects, and excessive feature might not be possible to remove.

Containers, represented by apartments, also provide protection, but they also share the same underlying infrastructure (provided by the Docker host) such as electricity, water, gas, etc. Containers are buitl with the opposite direction of VMs, in which they only get what developers need.

It is important to set a clear mind, that "Containers are not VMs" [[a]](https://goto.docker.com/rs/929-FJL-178/images/Docker-for-Virtualization-Admin-eBook.pdf?mkt_tok=eyJpIjoiWm1FM09HRTROREF5TW1abSIsInQiOiJGcnJNQWFQRWVTSEh1YjBnanJIc0hVOWV5R2pneW5GSVY0dGF1VzNZdjhESUdQZkcxQ2g3S2ZQWDc1Q2JQYjB4bFYrTkVPZ2pxbis1OXlQUUVtcTNHT1k1WFFTUUErVVJOTHVGTGV3eHE5M3RabUxhbkIzc1FlNHpGMXVGRTlITyJ9). VMs are just moveable packs of what used to sit on a physical server. Their abstraction layer is the physical layer, while containers sit at the abstraction of the application layer. Many microservices in containers combine make up an application [[a]](https://goto.docker.com/rs/929-FJL-178/images/Docker-for-Virtualization-Admin-eBook.pdf?mkt_tok=eyJpIjoiWm1FM09HRTROREF5TW1abSIsInQiOiJGcnJNQWFQRWVTSEh1YjBnanJIc0hVOWV5R2pneW5GSVY0dGF1VzNZdjhESUdQZkcxQ2g3S2ZQWDc1Q2JQYjB4bFYrTkVPZ2pxbis1OXlQUUVtcTNHT1k1WFFTUUErVVJOTHVGTGV3eHE5M3RabUxhbkIzc1FlNHpGMXVGRTlITyJ9). Despite contradictions, it is best interest of many organizations to integrate the two technology.

### Integrating VMs and Containers

#### Bare metal containers
The argument of whether VMs could be replace by containers originates from the concepts of bare metal containers. These are containers running on a bare-metal server - a server with one OS. Without hypervisors and VMs instances, more physical resource can be dedicated to our application.

Many organizations and teams has carried out many performance test of containers running on VMs and bare-metal systems, and Stratoscale being one of which. Their [results](https://www.stratoscale.com/blog/containers/running-containers-on-bare-metal/) point out a consistant increased in performance for bare-metal containers, whether it was CPU or IO operations. In addition, open source minimalistic OS such as Container Linux, Project Atom and Ubuntu Core are now released, with the sole purpose of running only containers.

#### VMs together with containers
Despite a noticealbe performance boost, there are limitations in deploying bare-metal containers. These problems do not come from containers themselves, but rather the OS. The first problems is security. No matter how isolated containers are, they are more likely exposed to a breach since they all share them same host [[a]](https://searchservervirtualization.techtarget.com/feature/The-debate-isnt-containers-vs-VMs-its-how-to-best-integrate-them). The second problem is OS management. Operations like rollbacks, updates or even migration can be difficult without the helps of VMs tools [[a]](https://www.stratoscale.com/blog/containers/running-containers-on-bare-metal/).

Although they have many drawbacks, VMs are still an efficient yet reliable options for managing servers, except for those organizations restricted by data security policy [[a]](https://www.stratoscale.com/blog/containers/running-containers-on-bare-metal/). Infact, containers have no problem running under VMs shared resources, and for small businesses, response time might not be one of their main concerns. Additionally, both Docker and VMware are heading towards this integration: VMware releases their version of minimalistic OS called Photon, and Docker suggests running their container anywhere you want.

![Containers on VMs](https://www.docker.com/sites/default/files/containers-vms-together.png)

_Figure 2. Containers on VMs [(source)](https://www.docker.com/sites/default/files/containers-vms-together.png)_

## Our preferred solution

At the start of this project, our production server at HAMK Valkeakoski was already hosting multiple services

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

  - Images reprensent the whole program (file system, libraries). Images can only run as containers, thus they are also template for constructing containers. 

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
    - _Lightweight_: Containers are very lightweight since they are just images' compressed layers with an additional writable layer. They can be deployed in just matter of seconds. [[a]](https://www.docker.com/what-container#/package_software)

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

All data stored in this writable layer will be destroyed when the container is removed. Therefore, to persist data (save data), Docker provides volumes and bind mounts.

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


## Networking and port mapping

Communicating between different containerized apps could be a daunting task, since isolation is one of containers main feature. Fortunately, Docker takes care of their containers networking by default. Docker containers are powerful, because with the right network configuration, they can communicate with any other services and apps, whether they are containerized or not. 

### Docker networking

Networking are managed with network drivers. Writing your own driver is possible, although Docker's drivers are good for most use cases [a](https://docs.docker.com/network/#network-drivers):
- Bridge network: This is the default network for every containers without any specifications. Bridge is used when multiple containers need connecting on a single Docker host.
- Host network: Host network is used when binding containers to the host IP address.
- Overlay: When multiple containers on one or multiple swarm needs to communicate 
