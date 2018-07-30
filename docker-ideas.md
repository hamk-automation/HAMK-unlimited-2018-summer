### ~~Project overview and needs~~
~~Digitalo and TOE hanke involves multiple IoT data sources, needs storage, visualization and demonstration~~
### Software solutions
VM vs Docker

### Docker architecture concepts 
 
- ~~System architechture~~
    - ~~Dockerhost:~~
        - ~~Docker daemon~~
        - ~~Docker objects:~~
            - ~~Docker images~~
            - ~~Docker containers~~
    - ~~Docker client~~
    - ~~Docker registry~~ 

- Data storage with Docker
    - ~~Container and image layers~~
    - TMPFS Mounts
    - Bind mounts
    - Volumes
- Port mapping, networking
- Services, clutering and security.
### How to use/build own docker images
- Image and container layers
- Dockerfile
- Building and running the app
- Persistence and non-persistance containers
### Use case - HAMK iot.research.hamk.fi
Docker used for service testing
1. LDAP login integrations
2. Web apps instances deployment
3. Distributing InfluxDB Telegraf plugin
4. Monitoring docker stack using deployed web app (Grafana)