# Containers
## Definitions
- **Container**: It is a software packaging that bundles all the files of an application, along with its dependencies and necessary configuration files, where it runs in isolation. 
    - This packaging is saved as an **image**. An image can be deployed by downloading and running it independently on the host operating system of any environment. 
    - Each execution of an image is considered to generate a **container** or instance of the application and can be run multiple times simultaneously. They can be executed on physical hardware, in the cloud, or in a virtual machine.
- **Container Engine**: It is a piece of software that accepts user requests, including command-line options, extracts images, and, from the end-user perspective, runs the container.
- **Container Runtime Platform**: It is responsible for managing the container's lifecycle: configuring its environment, running it, stopping it, etc. Runtimes can be subdivided into two types: high-level and low-level.
    - **High-level runtimes** manage images and integrate with low-level runtimes to execute the container process.
    - **Low-level runtimes** are responsible for creating a container through Linux kernel system calls.
    - It is also important to note that some engines can act as runtimes and, therefore, can be used from within other engines or orchestrators.
- **Container Orchestrator**: It is a software responsible for managing a set of containers across various computing resources, handling network and storage configurations delegated to the runtimes of the containers.

- **Virtual Machines**: They enable:
    - Virtualizing the underlying hardware to run multiple operating systems within the host operating system.
    - Provisioning of servers.
    - A virtual machine image contains the operating system, libraries, and applications, so it typically occupies a significant amount of space.

![Containers vs Virtual Machines](./images/containers-vs-virtual-machines.jpg)


- **Open Container Initiative (OCI)**: Docker and other key players in the container industry established the Open Container Initiative (OCI) in 2015. The OCI's goal is to create standards for container formats and runtimes. Currently, the OCI has three main specifications:
    - **image-spec**: The image specification that describes how to create an image compatible with the OCI.
    - **runtime-spec**: The runtime specification for unpacking the filesystem bundle.
    - **distribution-spec**: Defines an API protocol to facilitate and standardize content distribution.


- **Container Runtime Interface (CRI)**: Initially, Docker Engine was the sole runtime available on the platform. However, the popularity of containerization led to competing solutions and the need for Kubernetes to be compatible with all of them. With the Container Runtime Interface plugin, Kubernetes can communicate with major runtimes.


## Most popular containerization platforms
- **Docker**: It provides a complete platform for creating, packaging, and distributing containers. Docker's user-friendly interface and its extensive ecosystem of images and plugins have propelled it to the top of the list among developers and companies worldwide. Additionally, the strong support from the community complements it, making it the best choice for those seeking a robust containerization technology solution.

- **Containerd**: Originally created by Docker and subsequently donated to the Cloud Native Computing Foundation (CNCF), Containerd is a widely used container runtime. It provides a compact and efficient execution environment for containers and serves as a fundamental building block for container platforms and orchestrators.
    - Containerd is available as a daemon for both Linux and Windows. It manages the complete lifecycle of containers on its host system, including image transfer and storage, container execution and monitoring, low-level storage, network attachments, and more. 
    - Some features include:
        - OCI image specification.
        - OCI runtime specification (runC).
        - Container lifecycle and runtime, among others.

- **CRI-O**: CRI-O serves as the implementation of the Kubernetes Container Runtime Interface (CRI) and facilitates the use of runtimes compatible with the Open Container Initiative (OCI).
    - CRI-O is fully compatible with OCI container images and has the ability to pull from various container registries, providing a substitute for Docker, Moby, or Rkt. 
    - It is built from several components:
        - OCI-compatible runtime.
        - Networking (CNI) that supports various plugins like Flannel, Weave, and OpenShift.
        - Containers/images for image download.
        - Conmon for container monitoring.
        - Security policies, including those specified in OCI.


- **Podman (Pod Manager)**: It is a popular containerization technology that is rapidly maturing to compete with Docker. Unlike Docker, which uses a daemon for container management, Podman approaches containers with a daemonless technology called Conmon. Conmon is responsible for tasks such as container creation, state storage, container image extraction, and more.
    - Podman's ability to manage multiple containers "out-of-the-box" using pod-level commands sets it apart. Compared to Docker technology, Conmon consumes less memory. To create a pod, it is necessary to create a manifest file using the declarative format and YAML data serialization language. Kubernetes consumes these manifests for its container orchestration framework.
    - Podman inherited the Docker CLI, simplifying the transition from Docker to Podman. Some Linux distributions even include a package called podman-docker, which creates a docker alias to Podman.

![Most popular containerization technologies](./images/Container%20Engines%20Orchestrators.png)

