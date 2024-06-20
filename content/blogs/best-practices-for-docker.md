---
title: "Mastering Docker: Multistage Builds, Docker Init, and Best Practices for Deployment"
description: "A Guide to mastering Docker with Multistage Builds, Docker Init, and Best Practices for Deployment. Learn how to build efficient Docker images and deploy them in production."
dateString: Jun 2024
draft: false
tags: ["Docker", "Containerization", "DevOps", "Best Practices"]
weight: 146
showToc: true
cover:
    image: "/blogs/docker/mastering_docker_cover.png"
    caption: "Image by Dev Genius"
---

# A Guide to Mastering Production-Ready Docker Images

As You already know, Docker is a powerful tool for building, shipping, and running applications inside containers. It provides a consistent environment for your applications to run in, regardless of the underlying host system. 

However, building efficient Docker images that are optimized for production can be challenging. In this guide, we'll explore some best practices for building production-ready Docker images, including the use of multistage builds, Docker init, and other advanced techniques.

## Prerequisites

Before we dive into the details, I assume that you have visited my previous blog post on [Docker Tutorial for Beginners](/blogs/docker-tutorial-for-beginners) and 
[Complete Hands-On with Docker](/blogs/how-to-work-with-docker)
to get a good understanding of the core concepts of Docker.

## What are Multistage Builds?

Multistage builds are a feature of Docker that allows you to create multiple build stages in a single Dockerfile. Each stage can be used to build and package different parts of your application, and the final image only includes the artifacts from the final stage. This can help reduce the size of your Docker images and improve build times.



## How to Use Multistage Builds?

To use multistage builds, you need to describe each stage in your Dockerfile using the `FROM` instruction. and then use the `COPY --from=<stage>` instruction to copy artifacts from one stage to another.

Now, Let's see how to create a multistage Dockerfile for a [React Todo App](https://github.com/piyushsachdeva/todoapp-docker):

Single Stage Dockerfile:
```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN npm install 
RUN npm run build
RUN npm install -g serve
EXPOSE 3000
CMD ["serve", "-s", "build"]
```

Multistage Dockerfile:
```Dockerfile
# Stage 1: Build the application
FROM node:18-alpine AS build
WORKDIR /app

COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Serve the application
FROM nginx:stable-alpine AS production

# Only copying necessary files into final stage
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

<br>

Now, Guess what, the size of the final image created using the multistage build is significantly smaller than the single-stage build.

![Multistage Builds](/blogs/docker/multistage-build-output.png)

`todo:singlestage` is a image built through Single Stage Dockerfile which is of 479 MB, and `todo:multistage` image is build from Multistage Dockerfile which is only of 49 MB. 

This significant reduction in image size, **by 89%**, demonstrates the efficiency and effectiveness of using multistage Dockerfiles. By only including the necessary components in the final image, we can streamline our applications, improve loading times, and make better use of our resources.


## Exploring `docker init`

The docker init command is a utility that automates the process of enabling a project to be built and run with Docker. 

It allows developers to quickly add Docker support to their projects by creating standard Dockerfiles and Compose files with sensible defaults for the project

To use `docker init`, you need to have Docker Desktop installed on your system. You can install Docker Desktop from the official Docker website.

Once done, you can run the following command to initialize a new project with Docker support:

```bash
docker init 
```

This will create a new Dockerfile and docker-compose.yml file in the current directory, which you can then customize to suit your project's needs.

## Best Practices for Docker Deployment

Now that we've covered multistage builds, Docker init, and other advanced techniques, let's explore some best practices for deploying Docker images in production.

1. **Use Official Base Images**: Whenever possible, use official base images provided by Docker or the software vendor. These images are well-maintained, secure, and optimized for production use.

2. **Minimize Image Size**: Keep your Docker images as small as possible by using multistage builds, removing unnecessary files, and optimizing your dependencies.

3. **Use Environment Variables**: Use environment variables to configure your applications at runtime, rather than hardcoding configuration values in your Dockerfile.

4. **Use Health Checks**: Implement health checks in your Dockerfile to monitor the health of your application and automatically restart it if it becomes unresponsive.

5. **Use Docker Compose**: Use Docker Compose to define and run multi-container Docker applications. This makes it easy to manage complex applications and dependencies.

6. **Use Secrets Management**: Use Docker's built-in secrets management to securely store sensitive information, such as passwords and API keys, in your Docker images.

7. **Monitor and Log**: Monitor your Docker containers and log their output to a centralized logging system to track performance, troubleshoot issues, and ensure compliance.

8. **Automate Builds**: Use a CI/CD pipeline to automate the build, test, and deployment of your Docker images. This ensures that your images are always up-to-date and secure.

You can also explore this article, [Building best practices](https://docs.docker.com/build/building/best-practices/) to ensure that your Docker images are secure and compliant with industry standards.

## Conclusion

In this guide, we've explored some best practices for building production-ready Docker images, including the use of multistage builds, Docker init, and other advanced techniques. By following these best practices, you can build efficient Docker images that are optimized for production and deploy them with confidence.

I hope you found this guide helpful. If you have any questions or feedback, feel free to reach out to me. 

**Happy coding!**

---
