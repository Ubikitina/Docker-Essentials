# Docker Compose
Table of contents
- [Docker Compose](#docker-compose)
  - [What is Docker Compose?](#what-is-docker-compose)
  - [Commands for docker-compose.yaml file](#commands-for-docker-composeyaml-file)
    - [Commands in detail](#commands-in-detail)
  - [Docker Compose File Structure](#docker-compose-file-structure)
    - [File Example](#file-example)
    - [Services Section: Common Directives](#services-section-common-directives)
  - [Docker Compose Examples](#docker-compose-examples)
    - [Example 1 - Basics with nginx, php and mysql](#example-1---basics-with-nginx-php-and-mysql)
    - [Example 2 - Replicate mysql and adminer example](#example-2---replicate-mysql-and-adminer-example)
    - [Example 3 - Use the docker extension in Visual Studio Code to create docker-compose.yaml file](#example-3---use-the-docker-extension-in-visual-studio-code-to-create-docker-composeyaml-file)
    - [Example 4 - Resource reserve and limits](#example-4---resource-reserve-and-limits)
    - [Example 5 - Docker Compose for Scaling](#example-5---docker-compose-for-scaling)


In this section, we will explore how to define and run multiple applications in containers easily and quickly using Docker Compose.

## What is Docker Compose?

Docker Compose is a tool designed for defining and running applications with multiple containers as if they were a single service. It allows you to create a file to define various containers and start them all with a single command.

- Docker Compose files are written using the YAML (Yet Another Markup Language) format.
- For example, if you have an application that requires an NGINX server and a Redis database, you can create a Docker Compose file and run both as a service without the need to start each one separately.

## Commands for docker-compose.yaml file

| Command    | Description                                                  |
|------------|--------------------------------------------------------------|
| **up**     | Initiates all services (containers).                         |
| **down**   | Stops all services.                                          |
| **build**  | Validates that the images are ready, building any missing images. |
| **images** | Lists the images specified in the docker-compose.yaml file.  |
| **stop**   | Stops the containers for the services contained in docker-compose.yaml. |
| **run**    | Creates containers for the specified services in docker-compose.yaml. |
| **ps**     | Lists all containers specified in docker-compose.yaml.       |
| **pause**  | Pauses all containers specified in docker-compose.yaml.      |
| **unpause**| Resumes previously paused containers.                         |
| **logs**   | Displays the logs for the specified service's output.        |


These commands provide flexibility and efficiency in managing multi-container applications using Docker Compose.

### Commands in detail

To create and start the resources described in the `docker-compose.yaml` file, use the following command:

```bash
$ docker-compose up
```

Some options include:

- **detach/d:** Runs the containers in the background.
- **build:** Always builds the images even if they already exist.
- **force-recreate:** Forces the recreation of containers even if specifications haven't changed.
- **v/renew-anon-volumes:** Recreates anonymous volumes instead of retaining data from previous ones.
- **no-deps:** Avoids the creation of linked (dependent) containers.
- **scale:** Scales a service to a specified number of instances (replicas in `docker-compose.yaml`).

Examples:

```bash
$ docker-compose up -d # Starts the containers in the background.
$ docker-compose up nginx # Starts the nginx and mysql containers.
$ docker-compose up mysql # Starts only the mysql container.
```
If we are only going to create and start one of the containers, we must take into account the dependencies. When creating and starting one containers, the ones that depend on it will also be created and started.


To start the created containers:

```bash
$ docker-compose start [SERVICE…]
```

To display containers, including stopped ones:

```bash
$ docker-compose ps -a
```

To enter a container, for example, the php container:

```bash
$ docker exec -ti test_php bash
```

To stop active services while preserving containers, volumes, networks, and any modifications:

```bash
$ docker-compose stop
```

To discard changes and destroy everything:

```bash
$ docker-compose down
```



## Docker Compose File Structure

Docker Compose files operate by applying multiple commands declared within a `docker-compose.yaml` file. The basic structure of this file is as follows:

```yaml
version: "X"  # Optional starting from version 1.27.0
services: ...
volumes: ...
networks: ...
configs: ...
secrets: ...
```

- **Services:** Defines the configuration for containers.

- **Volumes:** Represents physical disk areas shared between the host and the server.

- **Networks:** Specifies communication rules between containers and the server.

- **Configs:** Stores non-sensitive information, such as configuration files outside of the images.

- **Secrets:** Serves as a storage mechanism for sensitive information like certificates, keys, etc.

This structure provides a comprehensive overview of the key components within a Docker Compose file, facilitating the configuration of containers and their associated resources. Each section plays a specific role in orchestrating and defining the behavior of the containers and their interactions.

### File Example
Here is an example Docker Compose file illustrating the configuration for a web application and a database:

![Web application and database](./images/DockerCompose_01.png)


```yaml
services:
  frontend:
    image: example/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: example/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:
    driver: flocker
    driver_opts:
      size: "10GiB"

configs:
  httpd-config:
    external: true

secrets:
  server-certificate:
    external: true

networks:
  front-tier: {}
  back-tier: {}
```

**Key Components of the Example:**
- Two services, backed by Docker images: `webapp` and `database`.
- One secret (HTTPS certificate) injected into the `frontend`.
- One configuration (HTTP) injected into the `frontend`.
- One persistent volume connected to the `backend`.
- Two networks: `front-tier` and `back-tier`.

This example provides a comprehensive configuration, showcasing the integration of services, secrets, configurations, volumes, and networks within a Docker Compose file. Adjustments can be made based on specific application requirements.

### Services Section: Common Directives
In the services section of a Docker Compose file, various directives are commonly used to configure and define the behavior of containers. Here are some key directives:

| Directive         | Description                                                                                                       |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
| **image**        | Configures the image and version used to build the container. Assumes the image is available on the server or Docker Hub.  |
| **build**        | Can be used instead of the image. Specifies the location of the Dockerfile for building the container.              |
| **container_name**| Defines a name for the container, corresponding to the service name.                                                |
| **volumes**      | Mounts a linked path on the server for use by the container.                                                        |
| **environment**  | Defines environment variables to be passed to the container.                                                        |
| **depends_on**   | Configures a service as a dependency, ensuring some services are loaded before others.                              |
| **ports**        | Maps a port from a container to the server using the structure `host_ip:container_ip`.                               |
| **links**        | Links this service to any other service in the docker-compose file by specifying their names.                        |

Docker Compose operates within a directory structure. You can have multiple groups of containers on a server—create a directory for each container and a `docker-compose.yaml` file for each directory. This modular approach allows for flexibility and organization in managing containerized applications.





## Docker Compose Examples
### Example 1 - Basics with nginx, php and mysql

```yaml
version: "3.9"
services:
  nginx:
    image: nginx:1.23.3
    container_name: test_nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./repo:/var/www
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/ssl:/etc/nginx/ssl/
    working_dir: /var/www
    links:
      - php

  php:
    image: php:8.1.0-fpm
    container_name: test_php
    volumes:
      - ./repo:/var/www
    working_dir: /var/www
    links:
      - mysql

  mysql:
    image: mysql:5.7.36
    container_name: test_mysql
    ports:
      - "3306:3306"
    volumes:
      - ./mysql:/var/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=root_password
```

**Create and Start Containers:**
```bash
$ docker-compose up -d # Start the containers in the background.
$ docker-compose up nginx # Start the nginx and mysql containers.
$ docker-compose up mysql # Start only the mysql container.
```
**Start Created Containers**
```bash
$ docker-compose start [SERVICE…] # Start the specified service containers.
```

**Display Containers:**
```bash
$ docker-compose ps -a # Show containers (including stopped ones).
```
```
Output:
PS C:\...\Docker-Essentials\Compose\example_01> docker-compose ps -a
NAME         IMAGE           COMMAND                  SERVICE   CREATED          STATUS          PORTS
test_mysql   mysql:5.7.36    "docker-entrypoint.s…"   mysql     11 minutes ago   Up 11 minutes   0.0.0.0:3306->3306/tcp, 33060/tcp
test_nginx   nginx:1.23.3    "/docker-entrypoint.…"   nginx     11 minutes ago   Created         0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp
test_php     php:8.1.0-fpm   "docker-php-entrypoi…"   php       11 minutes ago   Up 11 minutes   9000/tcp
```
    

**Enter a Container:**
To access a container, for example, the php container, execute the following command:
```bash
$ docker exec -ti test_php bash
```

**Stop Services:**
To stop active services while preserving containers, volumes, networks, and any modifications, run the following command:
```bash
$ docker-compose stop
```


**Destroy Everything:**
If you want to discard changes and destroy all resources, execute:
```bash
$ docker-compose down
```

### Example 2 - Replicate mysql and adminer example
We will do it with permanent storage. Our docker-compose.yaml is:
```yaml
version: '3.9'

services:
  mysql:
    image: mysql:latest
    container_name: mysql-container
    environment:
      MYSQL_ROOT_PASSWORD: my-root-pwd
      MYSQL_DATABASE: dbtest
      MYSQL_USER: my-user
      MYSQL_PASSWORD: my-sqlpwd
    # ports: # This is not necessary, we do not need to expose the ports for mysql, because adminer will communicate with it.
    #  - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql

  adminer:
    image: adminer:latest
    container_name: adminer-container
    ports: 
      - "8080:8080"
    depends_on:
      - mysql

volumes:
  mysql-data:
```

Create and start containers and display them:
```bash
$ docker-compose up -d
$ docker-compose ps -a
```
The status of the containers is "Up".

Access the website http://localhost:8080/, and log in by indicating the following connection parameters in Adminer:
- **Server:** `mysql` (this is the service name in the `docker-compose.yaml`)
- **Username:** `my-user`
- **Password:** `my-sqlpwd`
- **Database:** `dbtest`

Create a test table in Adminer. Add test values.

Stop Services and destroy everything:
```bash
$ docker-compose stop
$ docker-compose down
```

Now recreate and restart containers:
```bash
$ docker-compose up -d
```
Access the website http://localhost:8080/, log in and check if the created table and values still remain. We see that this content is still there because in the yaml we have specified to save the information in a volume.

We can view the volume created by doing 
```bash
$ docker volume ls
$ docker volume inspect <volume-name>
```

### Example 3 - Use the docker extension in Visual Studio Code to create docker-compose.yaml file
The goal of the following practice is for students to independently construct a solution using `docker-compose.yaml` based on the `dockerfile-multistage` practice (see Docker multistage in "04 Dockerfile.md").

1. **Generate `docker-compose.yaml` File:**
   - Use the Docker extension.
   - Press (Ctrl+Shift+P) and search for "Docker: Add Docker Files to Workspace..."
   - From the list of Application Platforms, select ".NET: ASP.NET Core."
   - Choose "Linux" from the list of Operating System options.
   - Enter the desired deployment port value.
   - When asked to include Docker Compose files, respond with "yes."

2. **Review and Execute:**
   - Examine the generated `docker-compose.yaml` file.
   - Execute the docker-compose.yaml file.

By following these steps, students will practice creating a Docker Compose solution independently, enhancing their skills with Docker and Docker Compose.

### Example 4 - Resource reserve and limits
To restrict the resources of a container, utilize the following `docker-compose.yaml` configuration:

```yaml
version: "3.9"

services:
  nginx:
    image: nginx
    deploy:
      resources:
        limits:
          cpus: "2"
          memory: 512M
        reservations:
          cpus: "1"
          memory: 256M
```

This configuration ensures that the specified container, in this case, "nginx," has resource limitations applied as follows:

- **CPU Limits:**
  - Maximum CPUs allocated: 2

- **Memory Limits:**
  - Maximum memory allocated: 512MB

- **CPU Reservations:**
  - Minimum CPUs guaranteed: 1

- **Memory Reservations:**
  - Minimum memory guaranteed: 256MB

By incorporating these settings into the `docker-compose.yaml` file, we can effectively control and allocate resources for the specified container, providing resource limitations and reservations.

### Example 5 - Docker Compose for Scaling

Utilize the following `docker-compose.yaml` configuration for scaling:

**docker-compose.yaml:**
```yaml
version: "3"

services:
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: pspdfkit
      POSTGRES_PASSWORD: password
      POSTGRES_DB: pspdfkit

  pspdfkit:
    image: "pspdfkit/pspdfkit:latest"
    environment:
      PGUSER: pspdfkit
      PGPASSWORD: password
      PGDATABASE: pspdfkit
      PGHOST: db
      PGPORT: 5432
      DASHBOARD_USERNAME: dashboard
      DASHBOARD_PASSWORD: secret
      SECRET_KEY_BASE: secret-key-base
      JWT_PUBLIC_KEY: |
        -----BEGIN PUBLIC KEY-----
        MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBALd41vG5rMzG26hhVxE65kzWC+bYQ94t
        OxsSxIQZMOc1GY8ubuqu2iku5/5isaFfG44e+VAe+YIdVeQY7cUkaaUCAwEAAQ==
        -----END PUBLIC KEY-----
      JWT_ALGORITHM: RS256
      API_AUTH_TOKEN: secret
      ASSET_STORAGE_BACKEND: built-in
    depends_on:
      - db
    expose:
      - "5000"

  nginx:
    image: nginx:latest
    container_name: scale_pr_nginx
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - pspdfkit
    ports:
      - "4000:4000"
```
Explanation:

1. **Version and Services:**
   - The `version: "3"` indicates the Docker Compose file version being used.

2. **Database Service (`db`):**
   - Defines a PostgreSQL service (`db`) with the specified environment variables.

3. **Main Application Service (`pspdfkit`):**
   - Defines the main application service (`pspdfkit`) using the `pspdfkit/pspdfkit:latest` image.
   - Sets necessary environment variables for the application.
   - Specifies dependencies on the `db` service using `depends_on`.
   - Exposes port `5000` within the service.

4. **Nginx Service (`nginx`):**
   - Defines an Nginx service using the `nginx:latest` image.
   - Names the container as `scale_pr_nginx`.
   - Mounts the local `nginx.conf` file into the container's `/etc/nginx/nginx.conf`.
   - Specifies dependencies on the `pspdfkit` service using `depends_on`.
   - Maps port `4000` on the host to port `4000` on the container.

**nginx.conf:**
```nginx
user nginx;
events {
  worker_connections 1000;
}

http {
  server {
    listen 4000;
    location / {
      proxy_pass http://pspdfkit:5000;
    }
  }
}
```

Explanation:

1. **User and Events:**
   - Sets Nginx to run as the `nginx` user with a limit of `worker_connections` to 1000.

2. **HTTP Server Configuration:**
   - Configures an HTTP server that listens on port `4000`.

3. **Proxy Configuration:**
   - The `location /` block specifies how Nginx should handle requests to the root URL (`/`).
   - The `proxy_pass http://pspdfkit:5000;` line instructs Nginx to forward requests to the `pspdfkit` service running on the internal Docker network at port `5000`. Nginx is used as a reverse proxy to forward requests from the external port (`4000`) to the internal port (`5000`) where the `pspdfkit` service is running.

Summary:

- The Docker Compose configuration defines three services: a PostgreSQL database (`db`), the main application (`pspdfkit`), and an Nginx server (`nginx`).
- The Nginx server acts as a reverse proxy, forwarding requests to the main application service.
- Ports are mapped to allow external access to the Nginx server on port `4000`.
- Dependencies are defined to ensure services start in the correct order (`pspdfkit` depends on `db`, and `nginx` depends on `pspdfkit`).

This setup allows us to scale the application easily and provides a simple example of using Nginx as a reverse proxy in a Dockerized environment. We can adjust the configurations based on our specific application requirements.

Using these files, perform the following exercise:

1. Create the files in a folder named `pspdkit-nginx`:

```bash
# Execute the following command to create the project
$ docker-compose up
```

2. List the deployed containers:

```bash
$ docker-compose ps
```

The result looks like this:
```bash
NAME                       IMAGE                      COMMAND                  SERVICE    CREATED         STATUS         PORTS
pspdkit-nginx-db-1         postgres:latest            "docker-entrypoint.s…"   db         3 minutes ago   Up 2 minutes   5432/tcp
pspdkit-nginx-pspdfkit-1   pspdfkit/pspdfkit:latest   "/usr/bin/dumb-init …"   pspdfkit   3 minutes ago   Up 2 minutes   5000/tcp
scale_pr_nginx             nginx:latest               "/docker-entrypoint.…"   nginx      3 minutes ago   Up 2 minutes   80/tcp, 0.0.0.0:4000->4000/tcp
```

3. Navigate to the URL [http://localhost:4000/dashboard](http://localhost:4000/dashboard)

4. Scale the `pspdfkit` service to two containers:

```bash
$ docker-compose up --scale pspdfkit=2
```

5. List the deployed containers and navigate back to the dashboard:

```bash
$ docker-compose ps
```
The result looks like this:
```bash
NAME                       IMAGE                      COMMAND                  SERVICE    CREATED          STATUS          PORTS
pspdkit-nginx-db-1         postgres:latest            "docker-entrypoint.s…"   db         7 minutes ago    Up 7 minutes    5432/tcp
pspdkit-nginx-pspdfkit-1   pspdfkit/pspdfkit:latest   "/usr/bin/dumb-init …"   pspdfkit   7 minutes ago    Up 7 minutes    5000/tcp
pspdkit-nginx-pspdfkit-2   pspdfkit/pspdfkit:latest   "/usr/bin/dumb-init …"   pspdfkit   32 seconds ago   Up 29 seconds   5000/tcp
scale_pr_nginx             nginx:latest               "/docker-entrypoint.…"   nginx      7 minutes ago    Up 7 minutes    80/tcp, 0.0.0.0:4000->4000/tcp
```