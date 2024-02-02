# Container Tools
Table of contents
- [Container Tools](#container-tools)
  - [Orchestrators](#orchestrators)
    - [Kubernetes](#kubernetes)
    - [Red Hat OpenShift](#red-hat-openshift)
    - [Amazon Elastic Kubernetes Service (EKS)](#amazon-elastic-kubernetes-service-eks)
    - [Google Kubernetes Engine (GKE)](#google-kubernetes-engine-gke)
    - [Azure Kubernetes Service (AKS)](#azure-kubernetes-service-aks)
    - [Docker Swarm](#docker-swarm)
  - [CI/CD](#cicd)
    - [Jenkins](#jenkins)
    - [CircleCI](#circleci)
    - [Azure Pipelines](#azure-pipelines)
  - [Security](#security)
    - [Docker Bench](#docker-bench)
    - [Clair](#clair)
    - [JFrog](#jfrog)
    - [Trivy](#trivy)
    - [Docker Scout](#docker-scout)


**Productivity Tools**: There is a diverse array of third-party tools that seamlessly integrate with Docker containers, enhancing overall productivity.

Docker offers a plethora of attractive features, with its notable compatibility with third-party tools standing out. The following tools are curated to elevate both speed and efficiency:

- Orchestrators
- CI/CD
- Logging
- Storage
- Management
- Builds
- Security
- Monitoring

## Orchestrators
### Kubernetes

Kubernetes remains a popular choice among developers as an open-source platform with extensive tools, offering flexibility and ease of use to enhance workflows and maximize productivity. The platform boasts a vast library of features developed by communities worldwide, providing unparalleled microservices management capabilities.

Components:

- **Node:** Responsible for running container workloads, either physically or virtually. These machines serve as hosts for container runtimes and facilitate communication within the Kubernetes service.

- **Cluster:** A group of nodes that share resources and execute applications in containers.

- **Replication Controllers:** Intelligent agents responsible for scheduling and resource allocation among containers.

- **Labels:** Tags used by the Kubernetes service to identify containers that are members of a pod.

### Red Hat OpenShift

OpenShift offers Platform as a Service (PaaS) and Container as a Service (CaaS) cloud computing models. It allows defining application source code in a Dockerfile or converting source code into a container using a Source-to-Image builder.

Key Features:

- Built-in Jenkins pipelines streamline workflows, enabling faster production.

- Integrated container runtime (CoreOS) and compatibility with standard runtimes like CRI-O and Docker.

- Integration of various development and operations tools for self-service container orchestration.

- Embedded OperatorHub ensures easy access to Kubernetes operators, third-party solutions, and direct access to cloud service providers like AWS.

- OpenShift is an open-source, provider-agnostic platform, avoiding vendor lock-in.

### Amazon Elastic Kubernetes Service (EKS)

Amazon EKS assists developers in creating, deploying, and scaling Kubernetes applications on-premise or in the AWS cloud. EKS automates tasks such as patching, updates, and node provisioning, ensuring the delivery of reliable, secure, and highly scalable clusters.

- Flexible Kubernetes control plane available in all regions for high availability and scalability.

- Directly manage applications from Kubernetes using AWS Controllers for Kubernetes.

- Easily scale, create, update, and terminate EKS cluster nodes with a single command.

### Google Kubernetes Engine (GKE)

GKE is a managed orchestration service providing a user-friendly environment to deploy, manage, and scale Docker containers on the Google Cloud Platform. The service enables the creation of agile, serverless applications without compromising security.

- Platform automates cluster management for ease of use.

- Native Kubernetes tools integration for faster application development without compromising security.

- Google Site Reliability Engineers provide infrastructure management support.

- Continuous improvement with regular feature updates makes GKE robust and reliable.

### Azure Kubernetes Service (AKS)

Azure offers automated management and scalability of Kubernetes clusters for enterprise-level container orchestration. End-to-end developer productivity with debugging, CI/CD, logging, and automated node maintenance.

- Advanced identity and access management for container security governance at scale.

- Compatibility with Linux, Windows Server, and IoT resources, deploying AKS in preferred infrastructure through Azure Arc.

- Apply compliance controls with Azure Policy, built-in protection limits, and internet security benchmarking. Precise access and identity control with Azure Active Directory. Grant privileged access with Just-In-Time cluster access. Utilize Microsoft Defender for Containers to enhance, monitor, and maintain security.

### Docker Swarm

As Docker continues to be one of the most widely used container runtimes, Docker Swarm proves to be an effective container orchestration tool. Swarm simplifies scaling, application updates, and workload balancing, making it ideal for deploying and managing applications, even in extensive clusters.

Key Features:

- **Manager Nodes:** Assist in load balancing by assigning tasks to the most suitable hosts.

- **Redundancy:** Docker Swarm utilizes redundancy to ensure high service availability.

- **Lightweight and Portable Containers:** Swarm containers are lightweight and portable.

- **Tight Integration with Docker Ecosystem:** Seamlessly integrated into the Docker ecosystem, allowing easier container management.

- **No Additional Plugins Required:** Configuration doesn't demand additional plugins.

- **High Scalability:** Ensures high scalability by balancing loads and adding worker nodes as workload increases.

## CI/CD
### Jenkins

Jenkins is an open-source automation tool that empowers Docker developers worldwide to create, test, and reliably deploy their software. It stands out as one of the best available Continuous Integration (CI) tools.

Key Features:

- **Versatility:** Offers a variety of tools that can be further integrated into the stack.

- **Ease of Configuration:** Requires minimal effort for setup.

- **Cost:** Free

### CircleCI

CircleCI boasts impeccable speed and ranks among the top CI tools. With thousands of pre-built integrations and a flexible environment, it caters to all integration and deployment needs.

Key Features:

- **Selective Build Trigger:** Executes a build only when a pull request is made, eliminating unnecessary builds for every new branch creation.

- **Parallel Testing:** Reduces total build time by running tests in multiple containers simultaneously.

- **Cost:** Free for up to 6000 build minutes per month.

### Azure Pipelines

As part of Microsoft Azure, Azure Pipelines simplifies the creation and testing of code projects before making them freely accessible. It can be used with virtually any programming language or project type, allowing concurrent code creation and testing while distributing it to any destination.

Key Features:

- **Developer-Centric:** Users can effortlessly create and push images to container registries like Docker Hub and Azure Container Registry. Containers can be deployed on individual hosts or Kubernetes clusters.

- **Collaboration Support for DevOps:** Users benefit from robust DevOps processes with native container support.

- **Integration Flexibility:** Pipelines support a diverse set of community-developed build, test, and deployment apps, along with hundreds of extensions ranging from Slack to SonarCloud.


## Security
### Docker Bench

Docker Bench is a popular tool for assessing the security configuration of Docker installations. It automatically evaluates Docker hosts against common security best practices, providing valuable insights to enhance Docker environment security.

Key Features:

- **Secure Docker Daemon:** Checks if the Docker daemon is configured securely and if container runtime security features are enabled.

- **Vulnerability Identification:** Identifies potential vulnerabilities on the Docker host, offering a comprehensive security assessment.

- **Detailed Security Audit Report:** Generates a detailed security audit report.

- **Results Categorization:** Each test is assigned a result - INFO, NOTE, PASS, or WARN.

- **Adherence to Industry Best Practices:** Ensures Docker environment adheres to recognized industry best practices.

- **Integration Ease:** Easily integrates into existing security workflows.

- **Price:** Free

### Clair

Clair is an open-source tool that performs static analysis of vulnerabilities in Docker containers. Widely used by developers, it indexes container images and compares them against known vulnerabilities.

Key Features:

- **User-Defined Vulnerability Data Updates:** Allows users to update vulnerability data from various user-defined sources.

- **API Access:** Provides an API for clients to query the vulnerability database for a specific container image.

- **Layer-by-Layer Container Analysis:** Conducts layer-by-layer analysis of container images, inspecting each layer for known security flaws.

- **Container Image Indexing:** Indexes container images by creating a list of features present in each image.

- **Seamless Docker Ecosystem Integration:** Integrates seamlessly with the Docker ecosystem.

- **Command-Line Tool (Clair-scanner):** Offers a command-line tool that simplifies the scanning process.

- **Price:** Free

### JFrog

JFrog offers a powerful Docker vulnerability scanner covering the entire lifecycle of Docker images. Utilize JFrog for development management, vulnerability analysis, artifact flow control, and distribution.

Key Features:

- **JFrog Docker Desktop Extension:** Scans local Docker images to detect security vulnerabilities.

- **JFrog Xray:** Performs deep recursive scanning of Docker images.

- **Identifies Infected Artifacts:** Displays all Docker images containing the infected artifact.

- **Best Suited For:** Companies already using JFrog products looking to cover the complete lifecycle of Docker images.

- **Price:** JFrog offers various pricing options, including Pro, Enterprise X, and Enterprise+. Custom plans are available based on specific needs.

### Trivy

Trivy is another vulnerability scanner for Docker containers and Kubernetes clusters. It detects vulnerabilities in various operating systems and programming languages, including Oracle Linux and Red Hat Enterprise Linux.

Key Features:

- **Covers OS Packages and Programming Language Dependencies:** Scans both operating system packages and programming language dependencies.

- **Seamless Docker Desktop Integration:** Easily scans container images for vulnerabilities directly from the Docker Dashboard.

- **Fast and Stateless Scanning:** Allows quick integration into daily routines, scripts, and Continuous Integration (CI) pipelines.

- **Unlimited Image Scanning:** Permits developers to analyze and scan an unlimited number of container images.

- **Supports Various Programming Languages:** Supports multiple programming languages, OS packages, and application dependencies.

- **Shift-Left Security Approach:** Enables early scanning of artifacts and dependencies in the software development lifecycle.

- **Best Suited For:** Teams seeking an all-in-one open-source scanning solution.

- **Price:** Free

### Docker Scout

Docker Scout proactively helps find and address vulnerabilities, creating a more secure software supply chain. It analyzes images, creating a comprehensive inventory of packages and layers called a Software Bill of Materials (SBOM). It then correlates this inventory with a continuously updated vulnerability database to identify vulnerabilities in your images.

Key Features:

- **Image Analysis:** Analyzes images to create a complete inventory of packages and layers.

- **Advisory Database:** Access information sources used by Docker Scout for guidance.

- **Integrations:** Connect Docker Scout with your CI, logs, and other third-party services.

- **Dashboard:** Web interface for Docker Scout.

- **Policies:** Ensure artifacts align with supply chain best practices.

- **Price:** Free plan includes up to 3 repositories.
