# Dockerfile

- [Dockerfile](#dockerfile)
  - [What is Dockerfile?](#what-is-dockerfile)
  - [Dockerfile Commands:](#dockerfile-commands)
  - [Environment variables](#environment-variables)
    - [ARG example:](#arg-example)
    - [ENV example:](#env-example)
  - [Dockerfile Examples](#dockerfile-examples)
    - [Example 1 - Minimum Dockerfile example](#example-1---minimum-dockerfile-example)
    - [Example 2 - Git](#example-2---git)
    - [Example 3 - Copy](#example-3---copy)
    - [Example 4 - Entrypoint](#example-4---entrypoint)
    - [Example 5 - Workdir](#example-5---workdir)
    - [Example 6 - Python](#example-6---python)
    - [Example 7 - Args](#example-7---args)
    - [Example 8 - Entrypoint and cmd](#example-8---entrypoint-and-cmd)
    - [Example 9 - Entrypoint, cmd, push and pull](#example-9---entrypoint-cmd-push-and-pull)
    - [Example 10 - Others](#example-10---others)
  - [Docker multistage](#docker-multistage)
    - [Example - Multistage](#example---multistage)


## What is Dockerfile?
It is a text file that describes the steps to create a Docker image from a base image, upon which changes are made.

Using the `docker build` command, the instructions in the Dockerfile are executed. During the execution of each step, a **layer** is added to the base image.

The `.dockerignore` file is used to exclude files and directories from the context of the docker build command.

## Dockerfile Commands:

| Command       | Description                                                            |
|---------------|------------------------------------------------------------------------|
| FROM          | Specifies the parent/base image.                                       |
| WORKDIR       | Sets the working directory for the following commands.               |
| RUN           | Installs an application or packages necessary for the container.      |
| COPY          | Copies files or directories from a specific location.                 |
| ADD           | Similar to COPY, but capable of processing remote URLs and extracting packages. |
| ENTRYPOINT    | Command that is executed when the container starts. Default is /bin/sh -c. |
| CMD           | Arguments passed to ENTRYPOINT. If no ENTRYPOINT, commands will be executed by the container. |
| EXPOSE        | Defines the ports through which the container is accessed.            |
| LABEL         | Adds metadata to the container.                                       |
| #             | Marks the line as a comment.                                           |


## Environment variables
| Term            | Description                                                                                                   |
|-----------------|---------------------------------------------------------------------------------------------------------------|
| ARG             | Defines arguments within the Dockerfile that are available during the image build. However, the running container cannot access these values. If multiple ARGs are defined (without a default value) but not specified in the build command, an error message will be triggered. |
| ENV             | Defines environment variables that are available during the image build from the moment they are defined. Unlike ARG, the running container can access these values.                                   |

![ARG vs ENV](./images/docker_environment_build_args.png)

### ARG example:
```bash
$ docker build --build-arg required_var=value
```

- `--build-arg`: This option is used to set build-time variables (build arguments) that are accessed from the Dockerfile. It allows you to pass values into the build process, which can be useful for customizing the image during the build.
- `required_var=value`: This is the argument being passed to the build process. 
    - `required_var` is the name of the build argument (to be replaced with the actual build argument used).
    - `value` is the value assigned to it (also to be replaced with the desired value).

### ENV example:
```bash
$ docker run –e "foo=rewrite_value"
```
`-e "foo=rewrite_value"`: This option sets an environment variable (foo) inside the running container with the value rewrite_value. The -e flag is used to pass environment variables to the container.

```bash
$ docker run –e foo
```
`-e foo`: This sets an environment variable (foo) inside the container, but without specifying a value. In this case, it means the value will be inherited from the host environment where the docker run command is executed.

```bash
$ docker run --env-file=file_name
```
`--env-file=file_name`: This option specifies a file (file_name) containing environment variable assignments. The content of the file is used to set environment variables inside the running container. Each line in the file should be in the format VAR_NAME=value.

## Dockerfile Examples
### Example 1 - Minimum Dockerfile example
This example demonstrates a minimal Dockerfile that uses the Alpine Linux base image to print "Hello world!" when the container is run.

Dockerfile:
```Dockerfile
FROM alpine
CMD ["echo", "Hello world!"]
```

Build and Run:

```bash
$ docker build -t hello .
$ docker run --rm hello
```

### Example 2 - Git
Creates an image with Git.
```Dockerfile
FROM alpine:3.19.0
RUN apk update
RUN apk add git
```

Create an image with a file in its structure.
```Dockerfile
FROM alpine:3.19.0
RUN apk update
ADD http://www.vlsitechnology.org/pharosc_8.4.tar.gz .
```

Build with the app_git:v1 tag, run, enter and verify the existence of the file.
```bash
$ docker run --rm app_git:v1 ls
```

### Example 3 - Copy
Create an index.html file with the content "Welcome to the test lab".

Create a dockerfile with the following content:
```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

Build with the app_copy:v1 tag, execute and navigate to the html page:
```bash
$ docker run --rm -d -p 80:80 app_copy:v1
```


### Example 4 - Entrypoint
Create a Dockerfile with the following content:

```Dockerfile
FROM alpine:3.19.0
LABEL maintainer="datahack"
ENTRYPOINT ["/bin/echo", "Hello, your ENTRYPOINT in EXEC format!"]
```

Build and run with the tag `app_entrypoint:v1` and check the result.

Then, execute the following command:

```bash
docker run --entrypoint "/bin/echo" app_entrypoint:v1 "Hello, from the console!"
```


### Example 5 - Workdir
Dockerfile:
```Dockerfile
FROM alpine:3.17.1
WORKDIR /opt
RUN echo "Welcome to Docker Labs" > opt.txt
WORKDIR folder1
RUN echo "Welcome to Docker Labs" > folder1.txt
WORKDIR folder2
RUN echo "Welcome to Docker Labs" > folder2.txt
```

Build the image with the app_workdir:v1 tag. Execute with the pwd command:
```bash
$ docker run --rm app_workdir:v1 pwd
```

### Example 6 - Python
Create a file named hello.py with the following content:
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "welcome to Dockerlabs!! successfully done!!"


if __name__ == "__main__":
    app.run()
```

Create dockerfile:
```Dockerfile
FROM python:3.5
RUN apt-getupdate
RUN pipinstallFlask
ADD . /opt/webapp/
WORKDIR /opt/webapp
ENV FLASK_APP=hello.py
EXPOSE 5000
CMD ["flask", "run", "--host=0.0.0.0"]
```

Build the image with the app_python:v1 tag. Then execute the image specifying the port.
```bash
$ docker build t app_python:v1 .
$ docker run rm p 5000:5000 d app_python:v1
```

Open URL http://localhost:5000/ to see the web up and running.

### Example 7 - Args
Create dockerfile:
```Dockerfile
FROM alpine:3.17.1
ARG A_USER=amigo
RUN echo "Bienvenido $A_USER al mundo de Docker!" > message.txt
CMD cat message.txt
```

Build the image with the app_args:v1 tag and execute.

Build the image with the app_args:v2 tag passing "Ed" as value in the A_USER argument.
```bash
$ docker build -t app_args:v2 --build-arg A_USER=Ed .
```

Execute the image.

### Example 8 - Entrypoint and cmd
Create a Dockerfile with the following content, build the image with the name app_entry:v1, run and view the output.
```Dockerfile
FROM alpine:3.1 9.0
ENTRYPOINT ["echo", "Hola"]
CMD ["Command Exec"]
```
Delete the container created in the previous line and execute the following line.
```bash
$ docker run --rm app_entry:v1 Mundo
```

Add the following line to the dockerfile, rebuild the image, run and see the output.
```Dockerfile
CMD "Command shell"
```


### Example 9 - Entrypoint, cmd, push and pull
Create a Dockerfile with the following content, build the image, run and view the output.
```Dockerfile
FROM alpine:3.1 9.0
ENTRYPOINT ["echo","Hola, soy Juan, te doy la bienvenida"]
CMD ["amigo"]
```

```bash
$ docker build -t my-greeting-app .
$ docker run my-greeting-app:latest
```

Upload the image to a repository in a Docker Hub account.
```bash
$ docker tag my-greeting-app my-docker-hub-user/greeting-app:latest
$ docker push my-docker-hub-user/greeting-app:latest
```

Download a peer image from Docker Hub and run the image by passing its name as a parameter.

In the above Dockerfile, modify the ENTRYPOINT statement by CMD passing its name as the value of the E_USER environment variable, generate the image again and check the output. New Dockerfile:
```Dockerfile
FROM alpine:3.19.0
ENV E_USER=amigo
ENTRYPOINT [ "sh", "-c", "echo Hola, soy Juan, te doy la bienvenida $E_USER" ]
CMD ["$E_USER"]
```


### Example 10 - Others
Create a Dockerfile with the following content:
```Dockerfile
FROM alpine:3.17.1
ENV DIRPATH /myfolder
WORKDIR $DIRPATH
```

Build, with the tag workdir:v2.

Execute with the pwd command.

Build, with the tag workdir:v3 adapting the Dockerfile so that the value of the DIRPATH variable can also get the value from an argument.

Run with the pwd command to display the current directory.


## Docker multistage 
Docker multistage builds are a feature in Docker that allows you to use **multiple FROM statements** in a single Dockerfile. Each FROM statement defines a new build stage, and each stage can have its own set of instructions. The purpose of multistage builds is to create more efficient and smaller Docker images.

Why Docker multistage builds are useful:
- Reduced image size
- Clean separation of build and runtime Environments
- Improved build speed
- Simplified build scripts
- Easier maintenance

Example:
![Multistage builds](./images/docker-multi-stage-builds.jpg)

### Example - Multistage
This example combines several steps for the compilation of the source code to the final binaries that will be deployed in production.
```dockerfile
# Stage 1: Base Image for Runtime
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 5080
EXPOSE 5443
ENV ASPNETCORE_URLS=http://+:5080
# Create a non-root user for security purposes
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

# Stage 2: Build Image
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
# Copy and restore dependencies
COPY ["TodoApi.csproj", "./"]
RUN dotnet restore "TodoApi.csproj"
# Copy the entire project and build
COPY . .
WORKDIR "/src/."
RUN dotnet build "TodoApi.csproj" -c Release -o /app/build

# Stage 3: Publish Image
FROM build AS publish
# Publish the application
RUN dotnet publish "TodoApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Stage 4: Final Image for Deployment
FROM base AS final
WORKDIR /app
# Copy the published files from the 'publish' stage
COPY --from=publish /app/publish .
# Set the entry point for the container
ENTRYPOINT ["dotnet", "TodoApi.dll"]
```