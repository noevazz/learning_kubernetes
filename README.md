# kubernetes

## What is it?

K8s is an open-source container orchestration platform that automates the deployment, scaling, networking, and management of containerized applications across a cluster of machines.

## What about Docker Compose?

Docker Compose is a tool for managing Docker applications that run in more than one container. A YAML file lists all the networks, volumes, and services that a program needs.

Developers can run multiple containers that are set up to work together with just one command. This makes it easier to run complex applications in development or testing settings.

**Docker Compose** is mainly for local or small-scale setups, while Kubernetes is built for production-grade, distributed systems:

- Multi-host support → manages containers across many machines, not just one.
- Self-healing → restarts failed containers, reschedules Pods on healthy nodes.
- Scaling → automatic horizontal scaling of apps (up/down).
- Load balancing & service discovery → built-in networking to expose apps reliably.
- Declarative configuration → desired state stored in the cluster, continuously reconciled.
- Rolling updates & rollbacks → zero-downtime deployments with version control.
- Resource management → quotas, scheduling based on CPU/RAM availability.

## Kubernetes Components

### kubectl

A command line tool used to interact the kubernetes API

### Node

A Node is a worker machine in Kubernetes. (like in AWS EC2, GCP Compute Engine, OCI) or a physical server.

- Each Node runs the software necessary to host Pods:
- kubelet → talks to the API server, ensures containers are running.
- container runtime → e.g., Docker, containerd (runs the actual containers).
- kube-proxy → handles networking/routing for Pods on the node.

> An special node called **Master node** is used for the control plane, and the nodes containing the actual application are called **worker nodes**

### Pod

A Pod is the smallest deployable unit in Kubernetes.
It represents one or more tightly coupled containers that should always run together on the same Node.

Containers in a Pod:

- Share the same network namespace (same IP address, can talk to each other via localhost).
- Can share storage volumes.
- Are scheduled together.

### Cluster

A Kubernetes Cluster is the whole set of machines (nodes) managed by Kubernetes.

### etcd

etcd is a distributed key–value store inside the master node

Kubernetes uses it as the backing database to store the entire **cluster state**.

### Scheduler

It is responsible for assigning newly created Pods to appropriate Nodes within a cluster.

Its primary function is to determine which Node a Pod should run on, ensuring efficient resource utilization and adherence to specified constraints.

### kubelet

kubelet is the agent that runs on every node in a Kubernetes cluster.

Its main job: make sure the containers that are supposed to run on that node are actually running.

It constantly talks with the API server and the container runtime (like containerd or Docker).

1. The scheduler assigns a Pod to a worker node.
2. The API server tells kubelet on that node: “Run this Pod with these containers.”
3. The kubelet:
    - Sets up networking (via CNI plugins).
    - Talks to the container runtime (containerd/Docker) to start the containers.
    - Mounts volumes if needed.
4. The kubelet reports back to the control plane:
    - Pod is Running, Pending, or Failed.
    - Sends resource usage and health info.

## Namespaces

A Namespace is a logical partition inside a cluster.
It lets you organize and isolate resources (Pods, Services, Deployments, etc.) within the same cluster.

Common use cases:

- Separate environments (e.g., dev, test, prod).
- Separate teams or projects in a shared cluster.
- Apply different resource limits and access controls.

## Manifests, Objects, Deployments, ReplicaSets, Services

### Manifest

A YAML or JSON file that describes what you want Kubernetes to create or manage.

Think of it as the “blueprint” or “instructions” for Kubernetes.

Example: a manifest can define a Deployment, a Service, or a Pod.

### Object

An entity in Kubernetes that represents the state of your cluster.

Objects are what you create with manifests.

Examples of Kubernetes objects: Pod, Service, Deployment, ReplicaSet, ConfigMap, Secret, Namespace.

Each object has:

- spec: what you want (desired state)
- status: what’s actually running (current state)

### Deployment

A higher-level object used to manage Pods in a scalable and reliable way.

It ensures that the desired number of Pods are always running.

Provides rolling updates and rollbacks.

You almost always use Deployments instead of managing Pods directly.

### ReplicaSet

A controller that makes sure the correct number of identical Pods are running.

It automatically replaces Pods if they crash.

Deployments use ReplicaSets under the hood to maintain Pods.

Usually, you don’t create ReplicaSets directly; you create a Deployment, which creates a ReplicaSet, which creates Pods.

### Service

A way to expose Pods (which can change dynamically) using a stable name and IP address.

Services allow other components or external clients to reach your application reliably.

Types of Services:

ClusterIP (default): internal-only access.

NodePort: exposes the app on each node’s IP with a static port.

LoadBalancer: integrates with cloud load balancers for external access.