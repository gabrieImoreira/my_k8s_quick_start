
# 🚀 My K8s Quick Start Guide

This guide is a personal quick reference to get started with Kubernetes using `k3d` and `kubectl` on a local environment with Docker + WSL.

---

## 🌐 What is Kubernetes (K8s)?

**Kubernetes (K8s)** is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

Key features:
- Self-healing containers
- Load balancing
- Service discovery
- Rollouts and rollbacks
- Declarative configuration with YAML

---

## 🔧 Tools Installation

1. **Install k3d**  
   > Lightweight wrapper to run [K3s](https://k3s.io/) in Docker containers  
   📎 [Install k3d](https://k3d.io/stable/#install-current-latest-release)

2. **Install kubectl**  
   > Kubernetes CLI for managing clusters  
   📎 [Install kubectl (Linux)](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

---

## 🧪 Getting Started with k3d and kubectl

Run these in your terminal (WSL):

```bash
# Create a basic cluster
k3d cluster create

# Check if the cluster is up
kubectl get nodes

# Create a cluster with 3 control-plane and 3 worker nodes
k3d cluster create --servers 3 --agents 3

# Open project folder
code .
```

---

## 📦 Creating and Running a Pod

```bash
# 1. Create the manifest file (pod.yaml)
# 2. Apply it:
kubectl apply -f pod.yaml

# 3. Check if the pod is running:
kubectl get po

# 4. Forward port from pod to host:
kubectl port-forward pod/web 8080:80

# 5. Access your app:
http://localhost:8080
```

---

## 🌀 Creating a ReplicaSet

```bash
# 1. Create the manifest file (replicaset.yaml)
# 2. Apply it:
kubectl apply -f replicaset.yaml

# 3. Check all resources:
kubectl get all

# 4. Edit the replica count inside YAML, then re-apply:
kubectl apply -f replicaset.yaml

# 5. Watch pod changes:
watch 'kubectl get po'

# 6. Port forward to a specific pod:
kubectl port-forward pod/<pod-name> 8081:80
```

---

## 🚀 Deployments (Manage Changes Automatically)

```bash
# 1. Delete previous replicaset:
kubectl delete -f replicaset.yaml

# 2. Apply deployment:
kubectl apply -f deployment.yaml

# 3. List pods:
kubectl get pods
```

---

## 🌐 Connecting to Pods via Services

When you have multiple pods, which one will you connect to? That's where **Services** come in.

### Types of Services:
| Type         | Description                                                   |
|--------------|---------------------------------------------------------------|
| LoadBalancer | Exposes app to the internet (used in cloud environments)      |
| NodePort     | Exposes app on a static port on each node                     |
| ClusterIP    | Default, used for internal communication (pod-to-pod)         |

### Creating a Cluster with Port Exposure:
```bash
# Delete old cluster:
k3d cluster delete

# Create new cluster with port mapping (LoadBalancer):
k3d cluster create --servers 3 --agents 3 -p "8080:30000@loadbalancer"
```

### Apply Service with Deployment:
- Add the Service definition to `deployment.yaml` after the `---`
- Apply with:
```bash
kubectl apply -f deployment.yaml
```

### Rollback Deployment Without Downtime:
```bash
kubectl rollout undo deployment <deployment-name>
watch 'kubectl get pod'
```

---

## 📘 Glossary: Core Kubernetes Concepts

| Term         | Description |
|--------------|-------------|
| **Pod**      | Smallest unit in K8s. Runs a container (like a lightweight VM). |
| **ReplicaSet** | Ensures a specified number of pods are always running. Uses labels to target pods. |
| **Deployment** | Manages rollouts, updates, and scaling |
| **Service**     | Abstracts access to a set of pods. Routes traffic and balances load. |

---

## ✅ Next Steps

Want to go beyond? Try these:

### 📁 Project with Database:
- Deploy an app and connect it to a **PostgreSQL** or **MongoDB** running in another pod or as a service.

### ☁️ Move to the Cloud:
- Try deploying the same configuration to:
  - [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine)
  - [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/products/kubernetes-service/)
  - [Amazon EKS](https://aws.amazon.com/eks/)

### ⚙️ Use Helm:
- Helm is a package manager for Kubernetes.
- Allows you to use and customize pre-made charts (apps) easily.
- [Learn Helm](https://helm.sh/)

### 🔍 Monitor Your Cluster:
- Use tools like:
  - `kubectl top`
  - [Lens](https://k8slens.dev/) (GUI for managing clusters)
  - [Prometheus + Grafana](https://prometheus.io/docs/visualization/grafana/)

---

## 🧠 Author Note

This quick start is meant for personal use, experimentation, and sharing with peers.  
Feel free to fork, clone, and improve!
