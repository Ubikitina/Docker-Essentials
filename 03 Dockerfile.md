# Dockerfile

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

![ARG vs ENV](./docker_environment_build_args.png)

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

