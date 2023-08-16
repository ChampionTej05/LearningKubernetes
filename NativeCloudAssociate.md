KCNA (Cloud native Associate)

It is theoretical course with MCQ Options. This is entry level Kubernetes certification


1. What are containers
  - Container in detail
    - A container is a lightweight, standalone, and executable software package that contains everything needed to run a piece of software, including the code, runtime, system libraries, and settings
    - Characteristics : Isolation, Portability , Resource Efficiency, Fast Deployment & Scaling
    - At the Linux level, a container is a process or a group of processes that are isolated from the rest of the system using Linux kernel features like namespaces and control groups (cgroups). These features provide the foundation for containerization, allowing processes to have their own isolated environment, file systems, networking, and resource limits.
    - Namespaces : Namespaces are a key Linux kernel feature that enable process isolation by providing separate instances of various system resources for different processes
    - Cgroups : Cgroups allow you to set resource limits and manage resource allocation for groups of processes. Containers use cgroups to enforce resource constraints such as CPU, memory, disk I/O, and more.
    - Avoiding matrix of hell (so that libs and deps can be isolated)
    - isolated environments with own process, services, networking sharing same OS kernel
  - Architecture of Docker
    ```sql
    +---------------------+
    |      Docker CLI     |
    +---------------------+
            |
            v
    +---------------------+
    |    Docker Daemon    |
    +---------------------+
            |
            v
    +---------------------+
    |    Docker Image     |
    +---------------------+
            |
            v
    +---------------------+
    |     Container       |
    +---------------------+
            |
            v
    +---------------------+
    |   Host Operating    |
    |      System         |
    +---------------------+
    ```
    - Docker CLI: Users interact with Docker by using the Docker Command Line Interface (CLI). They issue commands to manage images, containers, networks, and more.

    - Docker Daemon: The Docker Daemon is the background service responsible for managing containers, images, networks, and volumes. It listens for commands from the Docker CLI and interacts with the Linux kernel to create and manage containers.

    - Docker Image: Images are the building blocks of containers. They include the application code, runtime, libraries, environment variables, and more. Images are created from Dockerfiles and can be stored in image registries.

    - Container: Containers are instances of Docker images. They run applications in isolated environments with their own filesystem, network, and process space. Containers share the host's kernel but remain isolated from each other.

    - Host Operating System: The host OS provides the underlying infrastructure for running containers. Containers share the host's kernel, which is responsible for managing system resources and providing isolation between containers.

    - Docker Compose: Docker Compose simplifies the management of multi-container applications by using a single YAML file to define services, networks, and volumes.

    - Docker Registry: A Docker Registry stores Docker images, allowing them to be versioned, shared, and distributed. Docker Hub is a popular public registry, and private registries can also be used.

    - Docker Network: Docker Networks enable communication between containers. Containers within the same network can communicate using DNS names or IP addresses.

    - Docker Volume: Docker Volumes persist data beyond the lifecycle of a container. They can be used to store databases, logs, and other data.

  - Container vs Image
    - Container : A container is a **runtime instance** of a Docker image. It's a lightweight and isolated execution environment that contains all the necessary components to run an application, including the application code, runtime, system libraries, and settings. Containers run processes in isolation from the host system and other containers, using features like namespaces and cgroups provided by the Linux kernel.
    - Image: An image is a **read-only template** that contains all the necessary files, configurations, and dependencies to create a container. Images serve as the starting point for creating containers. They are built from a set of instructions defined in a Dockerfile, which specifies how to package the application code, runtime, and other components.
    - Images are platform-independent and can be shared and reused.
    - Container are not host-OS-independent. Linux based container can not run on Windows based OS. Though it can run on all linux bases OS like fedora,kali,etc.
2. Container Orchestration
  - Container orchestration is the *practice of automating the deployment, scaling, management, and operation of containerized applications*. As the number of containers in an application grows, managing them manually becomes complex. Container orchestration tools simplify this process by providing automation, scaling, load balancing, high availability, and other critical functionalities.
  - Key Benefits :
    - Automated Deployment: Orchestration tools help automate the deployment process, reducing human error and ensuring consistency.

    - Scaling: Orchestration platforms allow you to scale your application horizontally by adding or removing containers dynamically based on demand.

    - Load Balancing: Orchestration tools distribute incoming traffic across multiple containers, ensuring even distribution and optimal resource utilization.

    - Service Discovery: Containers can be dynamically added or removed, and orchestration platforms help manage the communication between services by providing service discovery mechanisms. (*This is needed as IPs changes due to change in state of containers*)

    - Rolling Updates and Rollbacks: Orchestration platforms facilitate rolling updates, allowing you to deploy new versions of your application gradually. If an issue arises, rollbacks can be performed seamlessly.

    - High Availability: Orchestration platforms monitor the health of containers and can automatically replace failed containers to maintain high availability.

    - Resource Management: Orchestration tools manage resource allocation and utilization, ensuring optimal performance and efficient resource usage.

    - Networking: Orchestration platforms create and manage networks that allow containers to communicate securely.

    - Secrets and Config Management: Orchestration tools provide ways to manage sensitive information and configuration data separately from the application code.
  - Available Orchestration Tools :
    - Kubernetes: An open-source platform for automating deployment, scaling, and management of containerized applications. (Learning this in detailed)

    - Docker Swarm: A native clustering and orchestration solution for Docker, designed for simplicity and ease of use.

    - Apache Mesos: A distributed systems kernel for managing computer clusters, including orchestrating containers.

    - Amazon ECS: A managed container orchestration service provided by Amazon Web Services.
3. Kubernetes Architecture
  - Kubectl : `kubectl` is a command-line tool used to interact with Kubernetes clusters. It is the primary interface through which users interact with the Kubernetes control plane to manage applications and resources within a cluster. kubectl allows you to perform a wide range of tasks, such as creating, updating, deleting, and inspecting resources, as well as managing deployments, services, pods, and more.
  - Master Node: The master node is the control plane of the Kubernetes cluster. It manages and coordinates the overall cluster operations. The main components on the master node are:

    - API Server: The central entry point for controlling the cluster and exposing the Kubernetes API. This is like the frontend for the K8s

    - Controller Manager: Ensures the desired state of the cluster by managing various controllers, such as node controller, replication controller, and endpoints controller. If the pod or node goes down, it is the responsibility of Controller to make the decision for bringing it up

    - Scheduler: Assigns pods to nodes based on resource requirements and constraints.

    - etcd: A distributed key-value store that stores all the configuration data and state of the cluster.
  - Nodes (Minion) :Nodes are worker machines that run containerized applications. Each node has several components:

    - Kubelet: Responsible for managing the containers on the node. It ensures that containers are running in a pod's desired state.

    - Kube Proxy: Maintains network rules to allow communication between pods and services.

    - Container Runtime: Software responsible for running containers, like Docker or containerd. K8s uses **containerd as runtime for docker images**
  - Pods: The **smallest deployable units** in Kubernetes. A pod is a logical group of one or more containers that share the same network and storage. Containers within the same pod can communicate using localhost, and they share the same IP address. It hosts the container onto NODE
  - Services: A service is an abstraction that defines a set of pods and a policy to access them. It provides a stable IP address and DNS name to access pods, enabling load balancing and service discovery. [ Service Discovery Feature ]

  - ReplicaSets: Manages the desired number of pod replicas, ensuring that a specified number of pods are running at all times. [ High Availability Feature]

  - Deployments: A higher-level abstraction over ReplicaSets, enabling declarative updates to applications. Deployments manage the creation, scaling, and rolling updates of pods. [ Automated Deployment, Rolling updates ]

  - StatefulSets: Manages stateful applications by providing guarantees about the ordering and uniqueness of pods. Useful for applications like databases that require stable network identities and persistent storage.

  - ConfigMaps and Secrets: Used to manage configuration data and sensitive information separately from the application code. [ Config Management ]

  - Namespace: Provides an isolated scope within a cluster. It allows you to create multiple virtual clusters within the same physical cluster. [ Resource Management & Isolation]

  - Ingress: Manages external access to the services within the cluster. It provides routing rules and load balancing based on hostnames or paths. [ Networking b/w clusters]

  - Persistent Volumes (PVs) and Persistent Volume Claims (PVCs): Manage storage resources that can be dynamically provisioned and bound to pods.

4. CRI (Container Runtime Interface)
  - enable any container runtime vendor to work with K8s using OCI (open Container Initiative)
  - A container runtime is the engine that **turns container images into running containers**.
  - In detail, it is software responsible for running and managing containers on an operating system. It is the component that executes and supervises containerized applications, ensuring that they operate in isolated environments with their own filesystems, networking, and processes. Container runtimes provide the runtime environment for containers, allowing them to run consistently across different systems without interference.
  - The Container Runtime Interface (CRI):
    - It is an API specification that defines the interface between container runtimes (such as Docker, containerd, and others) and the Kubernetes *kubelet*, which is the primary node agent that runs on each node in a Kubernetes cluster.
    - The CRI standardises the interaction between Kubernetes and container runtimes, allowing different runtimes to be used interchangeably within a Kubernetes cluster.
    - Before the introduction of the CRI, Kubernetes had a tight integration with Docker as its default container runtime. CRI provides for abstract way of managing the containers.
    - K8s maintained dockershim exclusively for docker till 1.24. Since then, all docker images are OCI  compatible, so *containerd* is used to run those. [ containerd was part of docker, so it knows how to run docker images. containerd implements CRI.]
5. Docker vs containerD
  - containerD was part of docker earlier (demon to manage the container runtime in docker)
  - all images docker work, since imagespec are built using OCI, using containerD
  - CLI tools :
    - ctr : cli tool for container d
    -  nerdctl : docker like cli tool for container d
    -  crictl  :cli tool to interact with container runtime interface
7. Imperative vs Declarative
  - Imperative : Telling what to do and how to do
    - create/edit/expose/scale are all Imperative. Here we are specifying what to do and how to do
    - they are hard to keep track of, since they don't leave  history behind(unless --record is used)
  - Declarative : Specifying final outcome only
    - `kubectl apply -f def.yaml` is declarative command
    - this maintains history and ensures tracking (using git)
  - Learn APPLY command in detail
    - Last applied configuration, live configuration, local file (learn all this)
8. Namespaces
  - K8s creates namespace with "Default" to create our objects post bootup
  - K8s also creates namespace for storing networking/services information for K*
    working which should not be edited/deleted in the different namespace with
    name "kube-system"
  - "kube-public" : Namespace where public resources sharable are stored
  -  To connect to the resouce in other namespace
    - `mysql.connect("<service-name-in-other-namespace>.<namespace-name>.svc.cluster.local")`
    - When service is created, DNS entry is added automatically
    - cluster.local is DOMAIN (default)
    - svc is service subdomain
  - `kubectl get pods --namespace <namespaceName>`
  - `kubectl create -f pod-def.yml --namspace=dev` or
  - In the metadata section add field for `namespace: dev`
  - Create Namespace
    * ```YAML
      apiVersion : v1
      kind: Namspace
      metadata:
        name: dev
    ```
  - Switch to namespace : `kubectl config set-context $(kubectl config current-context) --namespace=dev`
  - Resource Quote : Limit resources on the Namespace for all components
