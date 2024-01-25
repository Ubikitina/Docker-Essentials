# Volumes

## Managing File Persistence in Docker
By default, any files created within a container are stored in a write layer, and this data is not retained when the container stops running.

Docker offers two options to ensure file persistence, even after the container has stopped:
- Volumes
- Binding (Mounting) Files

Furthermore, Docker provides the ability to store files in memory; however, it's important to note that these files are not persistent.

## Volumes in detail
Volumes are the preferred mechanism for persisting data generated and used by Docker containers.

- They are easy to back up or migrate.
- Management can be handled using Docker CLI commands.
- They are applicable on both Windows and Linux platforms.
- Securely shareable among multiple containers.
- Various drivers allow storage on remote servers, cloud providers, content encryption, or additional functionality.
- New volumes can be pre-loaded with files.
- Volumes do not increase the size of the container images.


![Docker Volume](./Docker%20Volume.png)

## Volume management using the Docker Client
### Create a volume
Basic:
```bash
$ docker volume createmy-volume
```

Advanced:
```bash
$ docker run -d --name test --mount source=my volume,target =/app nginx:latest
```

Some options for --mount:
| Option           | Required | Description                                     |
|------------------|----------|-------------------------------------------------|
| type=volume      | No       | Mounts a managed volume inside the container.   |
| src/ source      | No       | Specifies the name of a volume. If it doesn't exist, it creates one. If not specified, assigns a volume with a random name that has the same lifecycle as the container and is destroyed when the container is destroyed. |
| dst/ destination/ target | Yes | The path inside the container. For example: /mydirectories/work/. If the directory doesn't exist, Docker automatically creates it. |
| readonly/ ro     | No       | Indicates whether to mount as read-only or read-write (default). |



### List volumes
```bash
$ docker volume ls
```

### See details of a volume
```bash
$ docker volume inspectmy-volume
```

### Delete a volume
```bash
$ docker volume rm my-volume
```

### Example
```bash
# Create a Docker volume named 'my_volume'
$ docker volume create my_volume

# Inspect the details of the 'my_volume' volume
$ docker volume inspect my_volume

# Run a container named 'my_container' and mount 'my_volume' to '/data' inside the container
$ docker run -it -v my_volume:/data --name my_container selaworkshops/busybox:latest

# Inside the container, navigate to the '/data' directory and create a file 'prueba.txt'
$ cd /data
$ echo "prueba de persistencia" > prueba.txt
$ ls

# In another terminal, run a second container named 'my_container_2' using the same volume
$ docker run -it -v my_volume:/data --name my_container_2 selaworkshops/busybox:latest
$ cd /data
$ ls

# Exit both containers
$ exit

# Remove the containers 'my_container' and 'my_container_2'
$ docker rm -f my_container my_container_2

# Check the list of all containers
$ docker ps -a

# Run a new container named 'new_container' and mount 'my_volume' to '/data'
$ docker run -it -v my_volume:/data --name new_container selaworkshops/busybox:latest
$ cd /data
$ ls

# Exit the container and remove it
$ exit
$ docker rm -f new_container

```