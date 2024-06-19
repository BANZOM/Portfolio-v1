---
title: "How to work with Docker - Complete Hands-On"
description: "A comprehensive guide to understanding and working with Docker. This tutorial covers Docker installation, containerization of a project, image creation, Dockerfile, and more."
dateString: Jun 2024
draft: false
tags: ["Docker", "Containerization", "DevOps", "Containers"]
weight: 147
showToc: true
cover:
    image: "/blogs/docker/docker_handson_cover.png"
---

# Getting Started with Docker: Practical Examples

In my previous blog post, we discussed the basics of Docker and containerization. In this tutorial, we will dive deeper into Docker and explore hands-on examples to understand how Docker works and how you can leverage it in your projects.

If you are new to Docker, I recommend reading my previous blog post on [Docker Tutorial for Beginners](/blogs/docker-tutorial-for-beginners) to get a foundational understanding of Docker and containerization.

## Prerequisites

Before we get started, make sure you have Docker installed on your system. You can follow the official [Docker installation guide](https://docs.docker.com/get-docker/) to install Docker on your machine.

<center>OR</center>

You can use online platforms like [Play with Docker](https://labs.play-with-docker.com/) to practice Docker commands without installing it locally.

[![Play with Docker](/blogs/docker/play_with_docker_ss.png)](https://labs.play-with-docker.com/)


### Verify Docker Installation

To verify that Docker is installed correctly, run the following command in your terminal:

```bash
docker --version
```

If Docker is installed, you should see the version information displayed in the terminal.

## Basic Docker Commands

Let's start with some basic Docker commands to get familiar with the Docker CLI.

### 1. Running Your First Container

To run a container, you can use the `docker run` command followed by the image name. For example, to run a simple `hello-world` container, you can use the following command:

```bash
docker run hello-world
```

This command will download the `hello-world` image from the Docker Hub and run it in a container. You should see a message confirming that your Docker installation is working correctly.

### 2. Listing Containers

To list all the containers running on your system, you can use the `docker ps` command. This command will display a list of running containers along with their container IDs, names, and other details.

```bash
docker ps
```

### 3. Listing Images

To list all the Docker images available on your system, you can use the `docker images` command. This command will display a list of images along with their repository, tag, and size.

```bash
docker images
```

### 4. Stopping a Container

To stop a running container, you can use the `docker stop` command followed by the container ID or name. For example, to stop a container with the ID `banzo123`, you can use the following command:

```bash
docker stop banzo123
```

### 5. Removing a Container

Removing a container is as simple as stopping it. You can use the `docker rm` command followed by the container ID or name to remove a container. For example, to remove a container with the ID `banzo123`, you can use the following command:

```bash
docker rm banzo123
```

### 6. Removing an Image

To remove a Docker image from your system, you can use the `docker rmi` command followed by the image ID or name. For example, to remove an image with the ID `hello-world`, you can use the following command:

```bash
docker rmi hello-world
```
<br>

The above commands cover some of the basic Docker operations. Now, let's move on to more advanced topics like creating custom images and using Dockerfiles.

## What is a Dockerfile?

A Dockerfile is a text file that contains a set of instructions for building a Docker image. It specifies the base image, dependencies, environment variables, and other configurations needed to create the image.

It is a declarative file that defines the steps required to build a Docker image, making it easy to automate the image creation process and ensure consistency across different environments.

## Dockerizing a Sample Project

Now that we understand the basics of Docker and Dockerfiles, let's Dockerize a sample project to see how it works in practice.

The Project can be anything from a simple web application to a backend service. For this tutorial, we will clone a simple **ToDo App** from GitHub and containerize it using Docker.

### Step 1: Clone the ToDo App Repository

First, clone the ToDo App repository from GitHub using the following command:

```bash
git clone https://github.com/docker/getting-started-app.git
```

This will create a directory named `getting-started-app` containing the ToDo App source code.

### Step 2: Create a Dockerfile

Next, create a Dockerfile in the root directory of the project. The Dockerfile contains the instructions for building the Docker image for the ToDo App.

First, Move to the project directory:

```bash
cd getting-started-app/
```

Then, create a new file named `Dockerfile`

```
touch Dockerfile
```

Open the `Dockerfile` in a text editor and add the following content:

```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
EXPOSE 3000

CMD ["node", "src/index.js"]
```

Let's break down the contents of the Dockerfile:

- `FROM node:18-alpine`: Specifies the base image for the Docker image. In this case, we are using the `node:18-alpine` image as the base image.
- `WORKDIR /app`: Sets the working directory inside the container to `/app`.
- `COPY . .`: Copies the project files from the host machine to the container.
- `RUN yarn install --production`: Using the `yarn` package manager to install the project dependencies.
- `EXPOSE 3000`: Exposes port 3000 on the container. So that we can access the application running on this port.
- `CMD ["node", "src/index.js"]`: Specifies the command to run when the container starts. In this case, it runs the `index.js` file using Node.js.

After creating the Dockerfile, save the changes and close the text editor.

### Step 3: Build the Docker Image

We can now build the Docker image using the Dockerfile we created. Run the following command to build the Docker image:

```bash
docker build -t todo-app .
```

This command will build the Docker image with the tag `todo-app` using the Dockerfile in the current directory.

Which will take some time to build the image. Once the image is built successfully, you should see a message indicating that the image has been successfully built.

Also, you can verify the image by running the `docker images` command:


### Step 4: Run the Docker Container

Now that we have built the Docker image, we can run the container using the following command:

```bash
docker run -p 3000:3000 todo-app
```

This command will run the `todo-app` container and map port 3000 on the host machine to port 3000 on the container. You should see a message indicating that the application is running on port 3000.

You can also specify the `-d` flag to run the container in detached mode:

```bash
docker run -d -p 3000:3000 todo-app
```

This will run the container in the background, and you can access the ToDo App by opening a web browser and navigating to `http://localhost:3000`.

![ToDo App](/blogs/docker/app_ss.png)

### Step 5: Stopping and Removing the Container

If you want, you can stop and remove the container using the following commands:

```bash
docker stop <container_id>
docker rm <container_id>
```

Replace `<container_id>` with the actual container ID or name.

### Step 6: Optional - Push the Docker Image to Docker Hub

If you want to share the Docker image with others or deploy it to a cloud platform, It is recommended to push the Docker image to a Docker registry like Docker Hub.

Create a [Docker Hub](https://hub.docker.com/) account if not already created and login to your account and create a new repository with name of your choice.

Then, tag the Docker image with your Docker Hub username and repository name:

```bash
docker tag todo-app <username>/<repository_name>:latest
```

Login to Docker Hub using the following command:

```bash
docker login
```

Finally, push the Docker image to Docker Hub:

```bash
docker push <username>/<repository_name>:latest
```

Replace `<username>` and `<repository_name>` with your Docker Hub username and repository name.

From next time, you can pull the image from Docker Hub and run the container on any machine. You can use the following command to pull the image and run the container:

```bash
docker pull <username>/<repository_name>:latest
docker run -p 3000:3000 <username>/<repository_name>:latest
```


### Checking Logs

Logs are essential for debugging and monitoring applications running inside containers. You can check the logs of a running container using the `docker logs` command:

```bash
docker logs <container_id>
```

### Making Changes to the Application

If you make changes to the application code, you can rebuild the Docker image and run the container again to see the changes reflected in the application.
Or you can go inside the container and make changes directly.

```bash
docker exec -it <container_id> bash
```

This command will open a bash shell inside the running container, allowing you to make changes to the application files.

## Final Thoughts

In this tutorial, we explored how to work with Docker by containerizing a sample project using Dockerfiles. We covered the basics of Docker commands, creating custom Docker images, running containers, and pushing images to Docker Hub.

Docker provides a powerful and efficient way to package, deploy, and manage applications, making it an essential tool for modern software development and DevOps practices.

I hope this tutorial has helped you understand the fundamentals of Docker and how you can leverage it in your projects. If you have any questions or feedback, feel free to reach out to me.

In the next blog post, we will explore more advanced Docker concepts like Multi-Stage Deployments and practical examples to further enhance your Docker skills.

Happy Dockering! 🐳🚀

