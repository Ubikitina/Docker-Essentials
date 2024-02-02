# Docker Swarm
- [Docker Swarm](#docker-swarm)
  - [What is it?](#what-is-it)
  - [Docker Stack](#docker-stack)
  - [Docker Secrets](#docker-secrets)
  - [Docker Configs](#docker-configs)


## What is it?
Docker Swarm is a container orchestration tool provided by Docker, offering integrated cluster management and orchestration capabilities within the Docker engine.

**Docker Swarm vs Docker Compose:**
Docker Swarm is geared towards orchestrating containers in a distributed and scalable environment, while Docker Compose simplifies the definition and deployment of multi-container applications on a single node.

**Docker Swarm Commands:**
| Command           | Description                                           |
|-------------------|-------------------------------------------------------|
| `init`            | Initializes swarm mode in Docker.                     |
| `leave`           | Exits swarm mode in Docker.                           |
| `node ls`         | Lists all nodes.                                      |
| `join-token`      | Lists the images ready in the docker-compose.yaml file.|
| `service ls`      | Lists all services.                                   |
| `node inspect`    | Removes a service.                                    |
| `service inspect` | Describes a service.                                  |
| `node rm`         | Removes a node.                                      |
| `service rm`      | Removes a service.                                   |
| `node promote`    | Promotes a worker node to a manager.                  |
| `node demote`     | Demotes a manager node to a worker.                  |


## Docker Stack

Docker Stack is a set of commands integrated into the Docker engine since version 1.12, enabling the deployment of solutions using docker-compose YAML files (version 3.0) in Docker Swarm. Some differences with docker-compose are that new images cannot be generated with docker stack commands, and certain parts of docker-compose files are ignored.

**Commands:**
| Command                   | Description                                         |
|---------------------------|-----------------------------------------------------|
| `docker stack deploy`      | Deploys a new stack or updates an existing one.     |
| `docker stack ls`          | Lists the stacks.                                   |
| `docker stack ps`          | Lists tasks in the stack.                           |
| `docker stack rm`          | Removes one or more stacks.                         |
| `docker stack services`    | Lists services in the stack.                        |


## Docker Secrets

Secrets in Docker:
- Mounted in the in-memory file system inside the container.
- In Linux containers, they are mounted at /run/secrets/<secret_name>.
- In Windows containers, they are mounted at c:\ProgramData\Docker\secrets.
- Only the service granted access can access the secret.
- When a container stops, secrets are unmounted from its memory and deleted from the node's memory. In Swarm, only manager nodes have access to encrypted secrets.
- In Swarm, if a node loses connectivity while running a container, it still has access to its secrets but won't receive updates.
- Consider using a version number or date in the secret name for updates.

**Commands:**
- To create a secret: `$ docker secret create my_secret /path/to/secret/file`
- To list secrets: `$ docker secret ls`
- To view the content of a secret: `$ docker secret inspect my_secret`
- To delete a secret: `$ docker secret rm my_secret`

## Docker Configs

Configs in Docker:
- Operate similarly to secrets but are not encrypted when transmitted between nodes (though they are when stored on manager nodes) and are directly mounted in the container's file system.
- Configurations can be shared among multiple services and used alongside environment variables.
- To update or roll back configurations more easily, consider adding a version number or date to the configuration name.
- Note that configuration files are immutable once they have started being used, so if updates are needed, a different one must be created.

**Commands:**
- To create a config: `$ docker config create my_config /path/to/config/file`
- To list configs: `$ docker config ls`
- To view the content of a config: `$ docker config inspect my_config`
- To delete a config: `$ docker config rm my_config`
