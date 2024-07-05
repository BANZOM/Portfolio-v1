---
title: "Understanding Pods in Kubernetes and YAML Fundamentals"
description: "This blog post will help you understand what are Kubernetes Pods and how to write YAML files to create them."
dateString: Jul 2024
draft: false
tags: ["Kubernetes", "DevOps", "YAML"]
weight: 143
showToc: true
cover:
    image: "/blogs/kubernetes/kubernetes_pods_cover.png"
    caption: "Image from Buddy Works"
---

# Intro to Kubernetes Pods

**Kubernetes** is a container orchestration platform that helps you manage and scale your containerized applications. **Pods** are the smallest unit of deployment in Kubernetes. A Pod is a group of one or more containers that share the same network and storage resources and are scheduled together on the same node.

In this blog post, we will discuss what are Kubernetes Pods and how to write **YAML** files to create them. We will also cover some basic YAML fundamentals that you need to know to work with Kubernetes. Let's get started!

## Importance of YAML in Kubernetes

**YAML** is a human-readable data serialization language that is commonly used for configuration files. In Kubernetes, YAML files are used to define resources such as Pods, Deployments, Services, etc. These files are used to create, update, and delete resources in a Kubernetes cluster.

YAML files are easy to read and write, and they follow a simple key-value pair format. Here is an example of a simple YAML file:

```yaml
name: John Doe
age: 30
address:
    street: 123 Main St
    city: New York
    state: NY
skills:
    - DevOps
    - Java
    - MySQL
```

In the above example, we have defined a YAML file with some key-value pairs. The `name`, `age`, `address`, and `skills` are keys, and their corresponding values are `John Doe`, `30`, `123 Main St`, `New York`, `NY`, and a list of skills.

## Kubernetes Basic Recap

Before we dive into Pods and YAML files, let's do a quick recap of some basic Kubernetes concepts by reading my previous blog posts:

1. [Understanding Kubernetes Architecture](/blogs/kubernetes-architecture-best-explanation)
2. [Learn Kubernetes from Scratch](/blogs/how-to-setup-kubernetes-locally)

In these blog posts, we discussed the architecture of Kubernetes, how to set up a Kubernetes cluster locally, and how to deploy applications using Kubernetes. If you are new to Kubernetes, I recommend reading these blog posts to get a better understanding of the platform. 

## What are Kubernetes Pods?

A **Pod** is the smallest unit of deployment in Kubernetes. It is a group of one or more containers that share the same network and storage resources and are scheduled together on the same node. Pods are ephemeral, which means they can be created, destroyed, and replaced at any time. 

### Characterstics of Pods

1. **Atomic Unit**: A Pod is an atomic unit of deployment in Kubernetes. It represents a single instance of a running process in your cluster.

2. **Shared Resources**: Containers in a Pod share the same network and storage resources. They can communicate with each other using `localhost`.

3. **Co-located Containers**: Containers in a Pod are co-located on the same node. They can communicate with each other using inter-process communication (IPC).

4. **Single IP Address**: Pods have a single IP address that is shared among all containers in the Pod. Containers in the Pod can communicate with each other using this IP address.

5. **Shared Volumes**: Pods can have shared volumes that are mounted to all containers in the Pod. This allows containers to share data between them.

## Creating a Pod: Imperative vs. Declarative Approach

There are two ways to create a Pod in Kubernetes: **Imperative** and **Declarative**. Let's understand the difference between these two approaches:


| Aspect | Imperative Approach | Declarative Approach |
|--------|---------------------|----------------------|
| Command | `kubectl run` | `kubectl apply -f` |
| Configuration | Specified in command line arguments | Specified in a YAML file |
| Execution | Immediate | Based on YAML file |
| Scalability | Less scalable | More scalable |
| Reproducibility | Harder to reproduce | Easier to reproduce |
| Error Proneness | More prone to human error | Less prone to human error |
| Use Case | Quick testing and experimentation | Production deployments |

### Imperative Approach

In the Imperative approach, you use `kubectl run` command to create a Pod. Here is an example of creating a Pod using the Imperative approach:

```bash
kubectl run nginx --image=nginx
```

We are creating a Pod named `nginx` with the `nginx` image. This approach is quick and easy for testing and experimentation.

View the created Pod using the following command:

```bash
kubectl get pods
```

### Declarative Approach

In this approach, we have to create a YAML file with the Pod configuration and use the `kubectl apply -f` command to create the Pod. Here is an example of a simple Pod YAML file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    type: declarative
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

Explanation of the YAML file:

- `apiVersion`: The version of the Kubernetes API.
- `kind`: The type of resource we are creating (Pod in this case).
- `metadata`: Metadata about the Pod, such as name and labels.
- `spec`: Specification of the Pod, including containers, volumes, etc.
- `containers`: List of containers in the Pod.
- `name`: Name of the container.
- `image`: Docker image to use for the container.
- `ports`: List of ports to expose in the container.

To create the Pod using this YAML file, run the following command:

```bash
kubectl create -f pod.yaml
```

When you update the YAML file for some changes, you can apply the changes to the Pod using the following command:

```bash
kubectl apply -f pod.yaml
```

This approach is more scalable and reproducible than the Imperative approach. It is recommended for production deployments.

You can use same command to view the created Pod that we used in the Imperative approach:

```bash
kubectl get pods
```

## Some other useful commands

There are some other useful commands that you can use to manage Pods in Kubernetes, that includes:

- `kubectl describe pod <pod-name>`: Get detailed information about a Pod.

- `kubectl delete pod <pod-name>`: Delete a Pod.

- `kubectl logs <pod-name>`: Get logs of a Pod.

- `kubectl exec -it <pod-name> -- /bin/bash`: Get a shell to a running container in a Pod. Looks similar to `docker exec` command.

- `kubectl get pods -o wide`: Get a detailed view of Pods with additional information like node name, IP address, etc.

- `kubectl get pods --all-namespaces`: Get Pods from all namespaces.

## Advanced YAML Techniques for Pods

`kubectl` provides a way to generate a YAML file for an existing Pod using the `kubectl get` command. You can use the following command to generate a YAML file for a Pod:

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```

This command will create a YAML file for the Pod named `nginx` with the `nginx` image. You can then edit this file to make any changes and apply it to create the Pod.

Also, you can use `kubectl explain` command to get information about the fields in a resource. For example, you can use the following command to get information about the `Pod` resource:

```bash
kubectl explain pod
```

This command will give you detailed information about the fields that you can use in a Pod YAML file.

## Using JSON in place of YAML

It is worth mentioning that you can also use JSON files in place of YAML files to create resources in Kubernetes. The `kubectl apply` command supports both YAML and JSON formats. Here is an example of creating a Pod using a JSON file:

```bash
kubectl apply -f pod.json
```

The JSON file will have the same structure as the YAML file, but with JSON syntax.

Example Json file:

```json
{
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
        "name": "nginx-pod",
        "labels": {
            "app": "nginx",
            "type": "declarative"
        }
    },
    "spec": {
        "containers": [
            {
                "name": "nginx-container",
                "image": "nginx:latest",
                "ports": [
                    {
                        "containerPort": 80
                    }
                ]
            }
        ]
    }
}
```

JSON files are less human-readable than YAML files, but they can be useful if you are more comfortable working with JSON.

## Common Errors in Pod Creation and their Resolution

When creating Pods in Kubernetes, you may encounter some common errors. Let's discuss some of these errors and how to resolve them:

1. **ImagePullBackOff**: This error occurs when Kubernetes is unable to pull the container image specified in the Pod configuration. Make sure the image name is correct and accessible.

_Resolution_: Check the image name and make sure it is correct. Also, check the image repository for any access issues. 

_Command_: `kubectl describe pod <pod-name>`

2. **CrashLoopBackOff**: This error occurs when the container in the Pod keeps crashing and restarting. This could be due to a misconfiguration or an issue with the application running in the container.

_Resolution_: Check the logs of the container to identify the issue. Fix the issue and restart the Pod.

_Command_: `kubectl logs <pod-name>`

3. **Pending**: This error occurs when the Pod is stuck in the `Pending` state and is not getting scheduled on any node. This could be due to resource constraints or node availability issues.

_Resolution_: Check the resource requests and limits in the Pod configuration. Also, check the node status and availability.

_Command_: `kubectl describe pod <pod-name>`

## Where to go from here?

In this blog post, we discussed what are Kubernetes Pods and how to write YAML files to create them. We covered some basic YAML fundamentals that you need to know to work with Kubernetes. We also discussed the Imperative and Declarative approaches to create Pods in Kubernetes.

I'm sure you have learned a lot about Pods and YAML files in Kubernetes. 

In the next blog post, we will discuss **Kubernetes Deployments** and how to manage application deployments in Kubernetes. Stay tuned!

If you have any questions or feedback, feel free to contact me. 

Happy learning!

## References

1. <a href="https://kubernetes.io/docs/" target="_blank">Kubernetes Documentation</a>

2. <a href="https://rollout.io/blog/yaml-tutorial-everything-you-need-get-started/" target="_blank">YAML Tutorial</a>

3. <a href="https://youtu.be/_f9ql2Y5Xcc?si=ZJ3dxCBCO2Nqyewp" target="_blank">Day 7 CKA Series by Piyush Sachdeva</a>