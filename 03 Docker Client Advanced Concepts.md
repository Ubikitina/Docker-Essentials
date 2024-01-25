# Docker Client Advanced Concepts
Table of contents:
- [Docker Client Advanced Concepts](#docker-client-advanced-concepts)
  - [Configure resource limits](#configure-resource-limits)
    - [Restricting Memory Usage in Docker](#restricting-memory-usage-in-docker)
    - [Memory Reservation in Docker](#memory-reservation-in-docker)
    - [Checking Resource Usage](#checking-resource-usage)
    - [Restricting CPU Usage in Docker](#restricting-cpu-usage-in-docker)
    - [Prioritizing CPU Usage in Docker](#prioritizing-cpu-usage-in-docker)
  - [Networks](#networks)
    - [Basic concepts](#basic-concepts)
    - [Container Network Model](#container-network-model)
      - [Network Sandbox](#network-sandbox)
      - [Endpoints](#endpoints)
      - [Docker Engine](#docker-engine)
    - [Controllers](#controllers)
      - [Bridge](#bridge)
      - [Host](#host)
      - [None](#none)
      - [Overlay](#overlay)
      - [Macvlan](#macvlan)
    - [Basic network commands](#basic-network-commands)
  - [Labels](#labels)

## Configure resource limits
### Restricting Memory Usage in Docker
To limit the amount of memory (hard limit), we use the `-m` or `--memory` parameter.
```bash
$ docker run -m 512m nginx
```

### Memory Reservation in Docker
For setting a memory reservation (soft limit), use the `--memory-reservation` parameter.
```bash
$ docker run -m 512m --memory-reservation=256m nginx
```
### Checking Resource Usage
Resource usage can be verified using the `docker stats` command.
```bash
$ docker stats
```

### Restricting CPU Usage in Docker
To limit the amount of CPU (hard limit), use the `--cpus` parameter.
```bash
$ docker run --cpus=".5" nginx
```

### Prioritizing CPU Usage in Docker
For setting CPU prioritization (soft limit), use the `--cpu-shares` parameter.
```bash
$ docker run --cpus=2 --cpu-shares=2000 nginx
```



## Networks

### Basic concepts
Using Docker eliminates the need for explicit network configuration since Docker automatically establishes a default network where all created containers are seamlessly registered. In situations requiring more intricate setups, where the isolation of containers from one another is desired, configuring custom networks becomes essential.

Docker networks empower users to connect containers to specified networks, ensuring effective isolation. Some key advantages of Docker networks include:
- Shared operating system resources while maintaining container isolation.
- Reduced instances of an operating system required for workload execution.
- Accelerated software deployment processes.
- Enhanced application portability.

### Container Network Model
#### Network Sandbox
- It is an isolated environment that maintains the network configuration of containers.
- It is created when the user needs to generate an endpoint in the network.

#### Endpoints
- Represents the network configuration of a container (IP address, MAC address, etc.).
- Establishes connectivity for container services within the network and with other services.
- Facilitates connectivity between endpoints belonging to the same network while isolating them from others.

#### Docker Engine
- It is the same engine installed on your computer to build and run containers using Docker components and services.
- Its role is to manage the network with multiple drivers.
- Provides an entry point to libnetwork for maintaining networks. libnetwork supports multiple virtual drivers in a robust container network model, providing network abstraction levels.

### Controllers
#### Bridge
- It is the default private network created on the server.
- Containers linked to this network have an internal IP through which they can easily communicate with each other.
- This driver is widely used to run standalone containers.

#### Host
- It is a public network.
- Uses the server's IP and its TCP port space to expose services within the container.
- Disables network isolation between the container and the server, meaning that with this network driver, the user cannot run multiple containers on the same server.

#### None
- With this driver, containers have no access to external networks and cannot communicate with other containers.

![Docker Networking](./images/Network%20controllers.png)

#### Overlay
- Used to create internal private networks within Docker nodes in a Docker Swarm cluster.
- Docker Swarm is a service that enables development teams to build and manage a Swarm node cluster within the Docker platform.

#### Macvlan
- Simplifies communication processes between containers.
- Assigns a MAC address to the Docker container. With this address, the Docker server routes network traffic to a router.
- It is the suggested option when a user wants to directly connect the container to a physical network instead of connecting it to the Docker server where it is hosted.

### Basic network commands
List all available networks on the Docker server.
```bash
$ docker network ls
```

Connect a running container to a network.
```bash
$ docker network connect multi-host-network container
```

Specify an IP address to assign to a container.
```bash
$ docker network connect --ip 10.10.36.122 multi-host-network container
```

Disconnect a container from a network.
```bash
$ docker network disconnect multi-host-network container
```

Delete a network.
```bash
$ docker network rm network_name
```

Delete multiple networks.
```bash
$ docker network rm 7478d6eeac29 network_name
```




## Labels

A tool allowing users to add metadata in key-value format.

The basic syntax for LABEL instructions is:
```
LABEL <key string>=<value string> <key string>=<value string>...
```

- Example of labels in a Dockerfile:
  ```
  LABEL multi.label1="value1" multi.label2="value2" other="value3"
  ```

To view the labels of an image:
```bash
$ docker image inspect --format='{{ json .Config.Labels }}' myimage
```

- Labels included in the base or parent images (images in the FROM line) are inherited by their image.

- If a label already exists but with a different value, the most recently applied value overrides any previously set value.

To label a container with two labels at runtime (-l, --label, --label-file):
```bash
$ docker run -l mylabel=label com.example.foo=bar ubuntu bash
```

Use the `--label-file` flag to load multiple labels from a file.
```bash
$ docker run --label-file ./labels ubuntu bash
```

The file format for labels is similar to the format for loading environment variables. Unlike environment variables, labels are not visible to processes running inside a container. The following example shows a label file format:
```
com.example.label1="a label"
# this is a comment
com.example.label2=another label
com.example.label3
```
