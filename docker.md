# Projects overview and needs

## Motivations

The "Healthy Digital House" (Terveellinen Digitalo) project and the TOE-hanke project involves a significant amount of IOT data (Big Data) and data sources. The two projects involve an application base solution which includes storage, data handling, visualization and demonstrations. This application is a combined of several microservices which run on our local server. In the past and just until the last couple of months, our production server was just two small machines sitting in an office in Valkeakoski. As a new physical server was installed in HAMK's Datacenter located in Hämeenlinna, we have to come up with a system that should not only be able to migrate our existing programs with most of their data, but also provide us a better way of managing our microservices.

## Our needs

One important aspect we are heading for is scalability. For now, both projects are using only one server with one OS instance. In the past, scaling this server was only done vertically, which means adding more capacity and memory within one OS. Since all of our applications are also within that OS, we have no problem giving them more space. However, as our project grows cloud storage and horizontal scaling are the next two options. Cloud storage demands less infrastructure, but yet it provides as much as the service provides, while horizontal scaling involves joining multiple servers into one cluster. Applications should now be able to allocate on any machine within a cluster or in a remote machine and still have access to all the data they need if necessary. This challenge raises the need for containerized applications and an orchestration system.

The container is an amazing technology because they are flexible and robust. Containerized apps are deployed within seconds, and they function regardless of the environment and infrastructure we provide. In an ideal situation, all of our containerize apps would be stateless, meaning they do not contain any data, but rather all data is stored somewhere else. Only these data need to be migrated for our migration task to complete. Containerized apps are also the answer for an improved and optimized workflow. Since containers can be shared and deployed quickly, they close the gap between Dev and Ops (Development and Operations). At the time of writing this document, our current server is still young and expansion is yet to come, thus orchestration will not be our main target. Instead, we focus on bringing a closer look at containerizing apps.

# Software solutions

## Virtual machines (VM) and containers
![VMs and containers](https://twistlock2.wpengine.com/wp-content/uploads/2017/01/ContainerVsVM-1024x567.jpg)

_Figure 1. VMs and containers infrastructures [(source)](https://twistlock2.wpengine.com/wp-content/uploads/2017/01/ContainerVsVM-1024x567.jpg)_

### The difference
The two concept is best visulize by an analogy, as this [ebook](https://goto.docker.com/rs/929-FJL-178/images/Docker-for-Virtualization-Admin-eBook.pdf?mkt_tok=eyJpIjoiWm1FM09HRTROREF5TW1abSIsInQiOiJGcnJNQWFQRWVTSEh1YjBnanJIc0hVOWV5R2pneW5GSVY0dGF1VzNZdjhESUdQZkcxQ2g3S2ZQWDc1Q2JQYjB4bFYrTkVPZ2pxbis1OXlQUUVtcTNHT1k1WFFTUUErVVJOTHVGTGV3eHE5M3RabUxhbkIzc1FlNHpGMXVGRTlITyJ9) [[a]]() from Docker suggest: 

>"The analogy we
use here at Docker is comparing houses (virtual machines) to
apartments (Docker containers)." (Docker, 2016)

Virtual machines as houses are well protected against unwanted guests and have their own infrastructures. Since VMs are built from fully powered operating systems, quite often they are underused in smaller projects, and excessive feature might not be possible to remove.

Containers, represented by apartments, also provide protection, but they also share the same underlying infrastructure (provided by the Docker host) such as electricity, water, gas, etc. Containers are built in the opposite direction of VMs, in which they only get what developers need.

It is important to set a clear mind, that "Containers are not VMs" [[a]](https://goto.docker.com/rs/929-FJL-178/images/Docker-for-Virtualization-Admin-eBook.pdf?mkt_tok=eyJpIjoiWm1FM09HRTROREF5TW1abSIsInQiOiJGcnJNQWFQRWVTSEh1YjBnanJIc0hVOWV5R2pneW5GSVY0dGF1VzNZdjhESUdQZkcxQ2g3S2ZQWDc1Q2JQYjB4bFYrTkVPZ2pxbis1OXlQUUVtcTNHT1k1WFFTUUErVVJOTHVGTGV3eHE5M3RabUxhbkIzc1FlNHpGMXVGRTlITyJ9). VMs are just moveable packs of what used to sit on a physical server. Their abstraction layer is the physical layer, while containers sit at the abstraction of the application layer. Many microservices in containers combine to make up an application [[a]](https://goto.docker.com/rs/929-FJL-178/images/Docker-for-Virtualization-Admin-eBook.pdf?mkt_tok=eyJpIjoiWm1FM09HRTROREF5TW1abSIsInQiOiJGcnJNQWFQRWVTSEh1YjBnanJIc0hVOWV5R2pneW5GSVY0dGF1VzNZdjhESUdQZkcxQ2g3S2ZQWDc1Q2JQYjB4bFYrTkVPZ2pxbis1OXlQUUVtcTNHT1k1WFFTUUErVVJOTHVGTGV3eHE5M3RabUxhbkIzc1FlNHpGMXVGRTlITyJ9). Despite contradictions, it is in the best interest of many organizations to integrate the two technology.

### Integrating VMs and Containers

#### Bare metal containers
The argument of whether VMs could be replaced by containers originates from the concepts of bare metal containers. These are containers running on a bare-metal server - a server with one OS. Without hypervisors and VMs instances, more physical resource can be dedicated to our application.

Many organizations and teams have carried out many performance tests of containers running on VMs and bare-metal systems, and Stratoscale being one of which. Their [results](https://www.stratoscale.com/blog/containers/running-containers-on-bare-metal/) point out a consistent increased in performance for bare-metal containers, whether it was CPU or IO operations. In addition, open source minimalistic OSs are now released such as Container Linux, Project Atom, Ubuntu Core, etc, with the sole purpose of running only containers.

#### VMs together with containers
Despite a noticeable performance boost, there are limitations in deploying bare-metal containers. These problems do not come from containers themselves, but rather the OS. The first problem is security. No matter how isolated containers are, they are more likely exposed to a breach since they all share the same host [[a]](https://searchservervirtualization.techtarget.com/feature/The-debate-isnt-containers-vs-VMs-its-how-to-best-integrate-them). The second problem is OS management. Operations like rollbacks, updates or even migration can be difficult without the helps of VMs tools [[a]](https://www.stratoscale.com/blog/containers/running-containers-on-bare-metal/).

Although they have many drawbacks, VMs are still an efficient yet reliable options for managing servers, except for those organizations restricted by data security policy [[a]](https://www.stratoscale.com/blog/containers/running-containers-on-bare-metal/). In fact, containers have no problem running under VMs shared resources, and for small businesses, response time might not be one of their main concerns. Additionally, both Docker and VMware are heading towards this integration: VMware releases their version of minimalistic OS called Photon, and Docker suggests running their container anywhere you want.

![Containers on VMs](https://www.docker.com/sites/default/files/containers-vms-together.png)

_Figure 2. Containers on VMs [(source)](https://www.docker.com/sites/default/files/containers-vms-together.png)_

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
  
  - >"Sharing configuration files from the host machine to containers. This is how Docker provides DNS resolution to containers by default, by mounting /etc/resolv.conf from the host machine into each container." - [Docker, 2017](https://docs.docker.com/storage/)
  
  - Building artifacts or source code on host machine can be provided to containers in development process. Production built Dockerfile would only need to access the already built artifacts on host machine. 
  
    A good example for this use case should be building and bundling a react app, and then deploy it inside a Docker container. This example is available at the later [project use case]() section.

>**Notes:** Docker also provides tmpfs mounts for large sensitive data that do not need to persist. [Learn more here](https://docs.docker.com/storage/tmpfs/).


## Networking and port mapping

Communicating between different containerized apps could be a daunting task, since isolation is one of containers main feature. Fortunately, Docker takes care of their containers networking by default. Docker containers are powerful, because with the right network configuration, they can communicate with any other services and apps, whether they are containerized or not [[a]](https://docs.docker.com/network/). 

### Docker networking

Networking are managed with network drivers. Writing your own driver is possible, although Docker's drivers are good for most use cases [[a]](https://docs.docker.com/network/#network-drivers):
- Bridge network: This is the default network for every containers without any specifications. Bridge is used when multiple containers need connecting on a single Docker host.
- Host network: Host network is used when binding containers to the host IP address.
- Overlay: When multiple containers on one or multiple swarm needs to communicate.

We can list networks within Docker with the command in the following figure.

```
$ docker network ls

NETWORK ID          NAME                DRIVER
18a2866682b8        none                null
c288470c46f6        host                host
7b369448dccb        bridge              bridge
```

Figure 8. Docker network listing [(source)](https://docs.docker.com/engine/tutorials/networkingcontainers/)

#### From a container perspective
- #### For standalone containers
  Single standalone containers can have their own ports publising to the outside network via the host ports. The following methods are very common in configuring a container network:

  ##### Exposing and port mapping
  Exposing ports from a container is equal to allowing a container to talk to the outside network via exposed ports. This can be done either with an ```EXPOSE``` command in a Dockerfile or by adding an ```--expose``` flag at a container's run command [[a]](https://www.ctl.io/developers/blog/post/docker-networking-rules/). 

  ```
  "NetworkSettings": {
    "PortMapping": null,
    "Ports": {
        "1234/tcp": null
    }
  },
  "Config": {
    "ExposedPorts": {
        "1234/tcp": {}
    }
  }
  ```
  
  Figure 8. Exposed ports on a container on inspecting [(source)](https://www.ctl.io/developers/blog/post/docker-networking-rules/)

  Expose method is used in conjuction with port mapping [[a]](https://www.ctl.io/developers/blog/post/docker-networking-rules/). For a container's ports to be mapped with host ports, a ```-p``` or ```-P``` flag is used. The ```-p``` flag helps the user define the mapping rule, while the ```-P``` flag does it automatically.

  ```
  $ docker run -dit --name nodered -p 1880:1880 mynodered
  $ docker run -dit --name nodeRed -P mynodered
  
  ```

  Figure 9. Container port mapping with ```-p``` and ```-P``` flags.

  The above figure shown an example of running two node red instaces with their 1880 port exposed. The first example binds the host's 1880 port directly to the container, while the second example assigned a random port from the host.

- #### For simple connections between containers

  Since we were dockerizing one microservice at a time, a simple bridge connection was enough for our case. If not configured, containers are assigned to Docker default bridge network ```docker0```.

  ![docker0](https://docs.docker.com/engine/tutorials/bridge1.png)
  
  Figure 11. Default bridge network [(source)](https://docs.docker.com/engine/tutorials/bridge1.png)

  We can see from the figure that the container has an ip address of ```172.17.0.2```. This is the results of Docker adjusting the host IP table. It is possible for a container to join many different networks. The following figure shown an example of a containerized web application connect to a database inside another container via a different network name ```my_bridge```. The web app are connected to two bridge networks at the same time.

  ![Multiple bridge networks](https://docs.docker.com/engine/tutorials/bridge3.png)

  Figure 12. Connecting to multiple networks [(source)](https://docs.docker.com/engine/tutorials/bridge3.png)

  We can create a new bridge net work with command from the following figure.

  ```
  $ docker network create -d bridge my_bridge
  ```

  Figure 13. Create network command [(source)](https://docs.docker.com/engine/tutorials/networkingcontainers/#create-your-own-bridge-network)

  The network type is defined by the -d flag, which is bridge in this case. At runtime, a container net work can be definded with a ```--net``` flag. Our database containers was started with ```--net   my_bridge```, so it uses this net work and ignore the default ```docker0```. Our web container network has a default network connection, and it was joined to ```my_bridge``` by the folling command:

  ```
  $ docker network connect my_bridge web
  ```

# Use cases - HAMK iot.research.hamk.fi

## Improving development and deployment process

### TOE-hanke user interface architechture

Our user interface mimics the Model View Controller (MVC) pattern. The following figure from Mozilla Developer Network (MDN) demonstrate a shopping list app under the MVC architechture.

![MVC](https://mdn.mozillademos.org/files/16042/model-view-controller-light-blue.png)

Figure 14. An MVC architechture example [(source)](https://mdn.mozillademos.org/files/16042/model-view-controller-light-blue.png)

For the TOE-hanke project, our application displays the heating process information such as temperature, pressure, energy consumption, etc. as the view. After logging in, authorized users can control somewhat the process, for example, turning pumps on and off. These user's inputs are sent to the controller, in our case a Node-RED server. Node-RED then tell PLCs to execute the desired tasks. Informations from the field is then sent back to our Node-RED server, which could now be concern as a model for sending data back to our view. All connections from the web app to Node-RED is done through the MQTT protocol, while PLCs send their data via the OPC-UA protocol. The TOE-hanke web app is currently available at ["https://iot.research.hamk.fi/toehanke"](https://iot.research.hamk.fi/toehanke). At the moment, our website is implementing authentication based on HAMK's Lightweight Directory Acess Protocol (LDAP).

### Deployed with Docker

As our application is still under development, minimalizing the deploying downtime is one of our main concerns. Thus we have decided to serve it from inside a container, which can be deployed in seconds. In addition, deploying and managing multiple web app instances is pretty straightforward, and loadbalancing these apps is effortless. As we are new to this sort of approach, continuous intergration (CI) is off of our list. The current work flow from developing to deploying is as simple as it can get: after a new feature is released, the app is built into an image with a Dockerfile; this image is then executed inside container(s) on the server machine. 

The Docker build process is no different than the normal build process. The application's code, written in Javascript, is bundled into a minified version of itself. This bundle is then serve with an NGINX server. From the Docker perspective, the code is first bundled from a NodeJS image, which is quite heavy. Then, the bundled code is copied into the lightweight base image of NGINX for serving, and the previous image is discarded. This multi-step build process is lean and is known as the multi-stage build. The final image is lightweight and capable of functioning itself with or without an external load balancer.