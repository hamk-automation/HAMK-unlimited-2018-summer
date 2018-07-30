# Projects overview and needs
The "Healthy Digital House" (Tervelinen Digitalo) project and the TOE-hanke project involves significant amount IOT data (Big Data) and datasources. The needs for storage, data handling, visulisation and demonstration require a flexible and robust solution. In terms of flexibility, the system has to receive and deploy apps which are prone to continuos update releases with zero downtime. In terms of robustness, the system should be capable of self-healing and fault-tolerance in case of hardware failures, e.g. A malfunctioned server node in a cluster. In terms of both robustness and flexibility, applications within the system should be well isolated for better security, but they should still be able to communicate data if necessary. These features are so important for an orchestration system as not only do they close the gap between Dev and Ops (Development and Operations) to improve the overall workflow, but also managing and maintenance work are carried out with increased efficiency.



# Software solutions




# Docker architecture concepts

## System architecture
![Image of Docker architecture](https://docs.docker.com/engine/images/architecture.svg)

## Docker host

### __Docker daemon__
On a host machine, a docker daemon process manages all docker-related content, namely as containers, images, networks, volumes, etc. In other words, it is a server which is contantly listening to requests, either from host terminal via CLI interface, or via its own rest API from remote terminal.
This whole concept (including docker daemon and its interfaces) is called Docker Engine.

![Image of Docker Engine](https://docs.docker.com/engine/images/engine-components-flow.png)

### __Docker Objects__
Docker Objects are contents managed by Docker daemon. This section will cover only the basics of the two important concepts: images and containers. To better understand these concepts, see the [example]() provided later. 


- #### Images

  - Images reprensent the whole application. Images can only run as containers, thus they are also read-only template for constructing containers.

  - Images can be pull and stored on a public registry like Docker Hub or Docker Cloud or on your own private registry. This is what smooths out project sharing process between teams. 

  - An application image can be built from scratch or based on an existing image to save time and effort and/or to further extends the base image.


- #### Containers

  - Containers are executable image instances, hence the ability to start, stop, delete or even exec into a running instance via Docker CLI or API.

  - Containers, compares to VM instances, are very light-weighted, which results in just seconds of deploying.

  - Containers can store data on their writeable image layer. This layer will disappear when container is removed. More detailed discussions in [this section](). Thus, new images can be built upon different state of a container.

  - Containers' subsystems such as network, storage define how isolated a container is to other containers and to the host machine. All of which can be configured by defining flags when a container is started or created.

    An example run command including explanation can be found [here](https://docs.docker.com/engine/docker-overview/#docker-objects)

## Docker client
User communicate with Docker daemon from Docker client via either Docker CLI (on same host as Docker daemon) or Docker API (on remote host). Connection from client to more than one daemon is possible.

## Docker Registries
Images can be shared and pulled from Docker Registries, which is Docker Hub by default. Using other public and/or private registry is also possible.

An image is pulled with a ```docker pull``` command or with ```docker run``` if the image is not yet existed on local machine. An image is pushed to the referred registry with a ```docker push``` command.
