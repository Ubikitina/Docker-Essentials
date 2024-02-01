# Container Registries
Table of contents:
- [Container Registries](#container-registries)
  - [What is a Container Registry?](#what-is-a-container-registry)
  - [Types of Registries](#types-of-registries)
    - [Aspects of a Private Registry](#aspects-of-a-private-registry)
  - [Container Registry Providers](#container-registry-providers)
    - [1. Docker Hub Container Registry](#1-docker-hub-container-registry)
    - [2. Azure Container Registry (ACR)](#2-azure-container-registry-acr)
    - [3. Amazon Elastic Container Registry (ECR)](#3-amazon-elastic-container-registry-ecr)
    - [4. GitHub Container Registry](#4-github-container-registry)
    - [5. GitLab Container Registry](#5-gitlab-container-registry)
    - [6. Google Artifact Registry (GAR)](#6-google-artifact-registry-gar)
    - [7. Harbor Container Registry](#7-harbor-container-registry)
  - [Commands for using Docker Hub](#commands-for-using-docker-hub)



## What is a Container Registry?

A container registry is a repository (or collection of repositories) used for storing and accessing container images. These registries play a crucial role in the development of container-based applications, often as part of DevOps processes. Additionally, container registries can directly integrate with container platforms such as Docker and Kubernetes. In this section, we'll explore a set of container image libraries, with a specific focus on Docker Hub.

## Types of Registries

- **Public Registries**: Public registries are typically utilized by individuals or small teams looking to quickly start using their registry. However, as businesses grow, this approach may introduce more complex security issues, such as patching, privacy, and access control.

- **Private Registries**: Private registries allow for the incorporation of security and privacy into the storage of enterprise container images, whether hosted remotely or locally. Private registries often provide advanced technical support and security features.

### Aspects of a Private Registry

When considering a private container registry, several key aspects should be taken into account to ensure robust functionality and security.

**Key Features:**

1. **Compatibility with Multiple Authentication Systems:**
   Ensure that the registry supports various authentication systems to accommodate diverse security requirements.

2. **Role-Based Access Control (RBAC) for Local Images:**
   Implement RBAC for local container images to manage access control efficiently, granting permissions based on roles.

3. **Vulnerability Scanning Capability:**
   Utilize vulnerability scanning tools to enhance security by identifying and addressing potential weaknesses in the images and configurations.

4. **Auditing Capabilities:**
   Enable logging of verifiable usage, allowing for traceability of activities back to individual users. This ensures accountability and transparency.

5. **Automation Enhancements:**
   In a DevOps environment, leveraging containers, images, and container registries enables developers to deploy each application service independently. This eliminates the need to merge code changes, improves testing, and facilitates error isolation in both testing and production.

By focusing on these aspects, a private container registry can provide a secure and efficient foundation for containerized application development in a DevOps environment.

## Container Registry Providers

When exploring container registry options, various providers offer unique features catering to different needs. Here are some notable container registry providers:

### 1. Docker Hub Container Registry

Docker Hub is the most popular container registry, serving as the default Docker repository. It functions as a marketplace for public container images, making it the top choice for distributing images publicly. Pricing is tiered, unlocking specific features with a subscription plan.

### 2. Azure Container Registry (ACR)

Microsoft's Azure Container Registry is built on Docker Registry 2.0, managing authentication through Azure RBAC. ACR introduces features not yet offered by many competitors, including:
- Automatic purging of old images
- Unlabeled manifest retention policy
- Content trust, a Docker-concept for signing images in the Azure container registry.

### 3. Amazon Elastic Container Registry (ECR)

Amazon ECR supports both private and public Docker registries. It integrates with AWS IAM to control user, service, and application access levels to registries. Notably, AWS ECR includes vulnerability scanning for container images, a crucial feature for DevSecOps, leveraging the Clair CVE database to assess issue severity. Another powerful feature is the immutable image tags, preventing image overrides once sent to the container registry.

### 4. GitHub Container Registry

GitHub's container registry provides a seamless experience, especially for developers. Authentication is managed through a personal access token. Alternatively, for public repositories, users need to authenticate with a GitHub user account.

### 5. GitLab Container Registry

GitLab offers a free-to-use container registry compatible with Docker images and Helm Charts. It has both self-hosted and cloud-based versions. A standout feature of GitLab Container Registry is its cleanup policy, removing tags matching a specific regex pattern. Additionally, GitLab offers a Package Registry supporting various package types.

### 6. Google Artifact Registry (GAR)

Google's Artifact Registry manages both container images and non-container artifacts like Maven, npm, Python, APT, and Yum packages. GAR seamlessly integrates with CI/CD processes, streamlining container creation and deployment. It also provides a manual vulnerability scanner for images.

### 7. Harbor Container Registry

Harbor requires installation, configuration, and management by the user. Compatible with any Linux distribution supporting Docker, Harbor can also be deployed on a Kubernetes cluster. Harbor supports features such as vulnerability scanning, garbage collection, replication between regions, and content trust. It's a robust option for hosting your container registry.


## Commands for using Docker Hub
Log in:

```bash
docker login
```

Create/tag an image and upload it:
```bash
docker build –t <hub-user>/<repo-name>[:tag]
docker tag <existing-image> <hub-user>/<repo-name>[:<tag>]
docker commit <existing-container> <hub-user>/<repo-name>[:<tag>] # Save the changes
docker push <hub-user>/<repo-name>:<tag>
```

Search images in Docker Hub:
```bash
docker search <term>
```

Example with a filter when searching:
```bash
docker search --filter=is-official=true ubuntu
```