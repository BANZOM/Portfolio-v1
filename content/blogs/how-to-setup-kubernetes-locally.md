---
title: "Learning Kubernetes from Scratch: Setting Up Your Own Cluster with Kind"
description: "A step-by-step guide to setting up a local Kubernetes cluster using Kind, covering single-node and multi-node configurations."
dateString: Jun 2024
draft: false
tags: ["Kubernetes", "DevOps", "Containerization", "Cloud Computing"]
weight: 144
showToc: true
cover:
    image: "/blogs/kubernetes/kubernetes_with_kind.png"
    # caption:
---

# Create Your First Kubernetes Cluster Locally

You might have heard about Kubernetes and its capabilities in managing containerized applications. But have you ever thought about setting up your own Kubernetes cluster locally?

You may wonder why we don’t use a cloud provider like GKE, AKS, or EKS. Using a cloud service would mean that the cloud provider is in charge of the control plane, limiting learning. If we want to learn Kubernetes from scratch, we have to configure everything ourselves. Once we have done enough manual troubleshooting, we can move on to a managed Kubernetes deployment later.

## Pre-requisites

Before we start, make sure you have fundamental knowledge of Kubernetes and Docker. I highly recommend you to go through my previous blog on [Kubernetes Architecture](/blogs/kubernetes-architecture-best-explanation) to understand how Kubernetes works under the hood.

And of course, you should know how to work with Docker. If you are new to Docker, you can check out my blog on [Docker for Beginners](/blogs/docker-tutorial-for-beginners).

## Options for Hosting a Kubernetes Cluster Locally

There are several ways to set up a Kubernetes cluster locally. Some of the popular options include:

1. **Minikube**: Minikube is a tool that makes it easy to run Kubernetes locally. It runs a single-node Kubernetes cluster inside a virtual machine (VM) on your laptop for users looking to try out Kubernetes or develop with it day-to-day.

_Limitation_: It only supports a single node cluster, so it does not provide the same level of scalability and fault-tolerance as a multi-node cluster. Also, it may not be suitable for use on lower-end machines.

2. **MicroK8s**: MicroK8s is a lightweight, certified Kubernetes distribution designed for edge, IoT and CI/CD workloads. It is built and maintained by Canonical, the company behind Ubuntu.

_Limitation_: It is not as widely used as some other distributions, so there may be less community support and resources available.

3. **Kind (Kubernetes in Docker)**: Kind is a tool for running local Kubernetes clusters using Docker container “nodes”. It is primarily designed for testing Kubernetes itself, but may be used for local development or CI.

_Limitation_: It is not designed for production use, and it is not recommended to use it as a replacement for a full Kubernetes cluster.

In this blog, we will focus on setting up a Kubernetes cluster using Kind. Since Kind is purely Docker-based and easy to set up, it is a great choice for learning Kubernetes from scratch.

## Installation and Setup

### Step 1: Initial Setup

Assuming you have Docker installed on your machine, if not, you can download it from the [official Docker website](https://www.docker.com/products/docker-desktop).

### Step 2: Install Kind

You can install Kind using the following command:

On Linux:

```bash
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

kind version # Verify the installation
```

<br>
On macOS:

```bash
# For Intel Macs
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-darwin-amd64
# For M1 / ARM Macs
[ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-darwin-arm64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind
```

<br>
On Windows (PowerShell):

```bash
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.23.0/kind-windows-amd64
Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe
```

<br>

It is recommended to use the latest version of Kind. You can check the latest version on the [Kind releases page](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)

### Step 3: Install kubectl

`kind` does not include `kubectl`, so you will need to install it separately. Since you will not be able to perform some operations without `kubectl`.

For Linux: Check the Installation guide for Linux [here](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

For macOS: Check the Installation guide for macOS [here](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)

For Windows: Check the Installation guide for Windows [here](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)

<br>

![Installed prerequisites](/blogs/kubernetes/prerequisite_installed.png)
<p style="text-align: center; margin-top: -1vw;">Check the Installation</p>


## Setting Up a Single Node Kubernetes Cluster with Kind

Now that you have Kind and kubectl installed, let's create a single-node Kubernetes cluster using Kind.

### Step 1: Create a Cluster

To create a single-node cluster, run the following command:

```bash
kind create cluster --name single-node-cluster 
```

This command will create a single-node Kubernetes cluster named `single-node-cluster`, You can choose any name you like for your cluster.

If you want to use any specific version of Kubernetes, you can specify it using the `--image` flag. For example, to create a cluster with Kubernetes version v1.29.4, you can run

```bash
kind create cluster --name single-node-cluster --image kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8
```
<br>

Find the available versions on the [Kind releases page](https://github.com/kubernetes-sigs/kind/releases)

### Step 2: Verify the Cluster

To verify that the cluster has been created successfully, run:

```bash
kind get clusters
```

You should see the name of the cluster you just created in the output.

Other Ways to Verify:

- Using `kubectl`:

```bash
kubectl cluster-info --context kind-single-node-cluster
```

- Using `docker`:

```bash
docker ps
```

You should see the Kubernetes containers running.

### Step 3: Interact with the Cluster

You can interact with the cluster using `kubectl`. For example, to get the nodes in the cluster, you can run:

```bash
kubectl get nodes
```

You should see the single node in the output.

### Step 4: Delete the Cluster

To delete the cluster, run:

```bash
kind delete cluster --name single-node-cluster
```

This will delete the single-node cluster you created.

<br>

![Single Node Cluster](/blogs/kubernetes/single_node_cluster.png)
<p style="text-align: center; margin-top: -1vw;">Single Node Cluster</p>


## Setting Up a Multi-Node Kubernetes Cluster with Kind

Creating a multi-node cluster with Kind is similar to creating a single-node cluster. You just need to specify the number of control-plane and worker nodes.

### Step 1: Create a Multi-Node Cluster

To create a multi-node cluster with one control-plane node and two worker nodes, run:

```bash
kind create cluster --name multi-node-cluster --config multi-node-cluster.yaml
```

You need to create a configuration file `multi-node-cluster.yaml` with the following content:

```yaml
kind : Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8
- role: worker
  image: kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8
- role: worker
  image: kindest/node:v1.29.4@sha256:3abb816a5b1061fb15c6e9e60856ec40d56b7b52bcea5f5f1350bc6e2320b6f8
```

This configuration file specifies one control-plane node and two worker nodes, all running Kubernetes version v1.29.4.

### Step 2: Verify the Cluster

To verify that the cluster has been created successfully, run:

```bash
kind get clusters
```

You should see the name of the multi-node cluster you just created in the output.

### Step 3: Interact with the Cluster

You can interact with the multi-node cluster using `kubectl`. For example, to get the nodes in the cluster, you can run:

```bash
kubectl get nodes
```

You should see the control-plane node and two worker nodes in the output.

### Step 4: Delete the Cluster

To delete the cluster, run:

```bash
kind delete cluster --name multi-node-cluster
```

This will delete the multi-node cluster you created.

<br>

![Multi Node Cluster](/blogs/kubernetes/multi_node_cluster.png)
<p style="text-align: center; margin-top: -1vw;">Multi Node Cluster</p>

## Working with Multiple Clusters

You can create and manage multiple clusters using Kind. This is useful for testing applications in different environments or configurations.

To switch between clusters, you can use the `kubectl` context:

```bash
kubectl config get-contexts
```

This will show you the available contexts. The Active context is marked with a Asterisk (*).

You can switch to a different context using:

```bash
kubectl config use-context <context-name>
```

After switching the context, you can interact with the cluster using `kubectl`.

Like shown in the image below:

<br>

![Multiple Clusters](/blogs/kubernetes/multiple_clusters.png)

<p style="text-align: center; margin-top: -1vw;">Working with Multiple Clusters</p>

## Conclusion and Next Steps

Setting up a Kubernetes cluster locally using Kind is a great way to learn Kubernetes from scratch. It provides a simple and easy-to-use tool for creating and managing Kubernetes clusters on your local machine.

In this blog, we covered how to set up single-node and multi-node Kubernetes clusters using Kind. We also discussed how to interact with the clusters using `kubectl` and how to switch between multiple clusters.

In the next blogs, we will explore more topics related to Kubernetes, such as understanding YAML manifests, deploying applications, and managing resources in a Kubernetes cluster.

Happy learning! 🚀

## References and Further Reading

- [Kind Documentation](https://kind.sigs.k8s.io/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Kubernetes Architecture Explained](/blogs/kubernetes-architecture-best-explanation)
- [Docker for Beginners](/blogs/docker-tutorial-for-beginners)
- [Day 6 CKA Series by Piyush Sachdeva](https://youtu.be/RORhczcOrWs?si=WGQMUwNzoE91uU8v)
