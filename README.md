# Code With Mosh: The Ultimate Docker Course 🐳

## Before we start ❗️

### Developer information 🙋🏼‍♂️

Developed by Magnus Rødseth, spring 2021.

### Description ❓

Docker is a platform for building, running, and shipping applications with ease. That's why most companies use it and are looking for software or DevOps engineers with Docker skills.

**But what is this course?** A clear, concise, comprehensive, and highly practical course that prepares you for the job. You'll learn everything about Docker from the absolute basics to more advanced concepts in under 5 hours. By the end of this course, you'll have a live full-stack application with a database and automated tests in the cloud.

### Goals 🏁

By the end of this course, you'll be able to…

✅ Confidently build and ship applications with Docker  
✅ Work in development teams using Docker  
✅ Troubleshoot issues like a pro

## So let's get started ⏩

### What is Docker?

Docker is a platform for building, running and shipping applications in a consistent manner. In practice, this means that if the application runs on your development machine, you're sure it can run on test and production machines.

Additionally, if a new developer needs to spin up your application, he simply needs to tell Docker to start the application, without the need for much configuration.

### Virtual Machines versus Containers

A **virtual machine**, or VMs for short, is an absatraction of a machine or physical hardware. Using VMs, we can _run apps in isolated environments_. However, each VM _needs a full OS to function_. This causes it to be _slow to start_ and quite _resource intensive_.

A **container** is an isolated environment for running an application. As with VMs, containers also allow us to _run multiple apps in isolation_. Additionally, they are _lightweight_ and _use the OS of the host_. Because the OS already runs on the host, a container can _start relatively quickly_. Container also need _less hardware resources_. In theory, using containers, we can run thousands of containers side by side on one host.

### Docker Architecture

Docker uses a client-server architecture using a REST API. All containers running on a host share the operating system kernel.

**What's a kernel?** A kernel is the core of an operating system, which manages applications and hardware resources. Different operating systems have different kernel with different APIs. Thus, we cannot run a Windows application on Linux, because the Windows application needs to talk to the uncompatible Linux kernel under the hood.

**On Linux OS**, we can only run Linux containers, because the containers need to run on Linux.

**On Windows OS**, we can run both Windows and Linux containers, because Windows 10 ships with both a Windows kernel and a Linux kernel.

**Docker on Mac OS** uses a lightweight Linux virtual machine to run Linux containers using the Linux kernel.

### Development Workflow

Let's say you have a full-stack web application that you want to dockerize. Now, you add a Dockerfile.

A **Dockerfile** is a plaintext file that includes instructions that Docker uses to package the application into an image.

A **Docker Image** has a cut-down OS, a runtime environment, application files, third-party libraries, environment variables, etc...

Once we have built the image, we can tell Docker to create a container from that image.

Once we have a complete Docker image, we can push it to **Docker Hub**. Docker Hub to Docker is like GitHub to Git; it's a storage for Docker Images that anyone can pull down a specific version of our application and use it.

### Docker in action

Let's say you have a JavaScript application called `app.js` with 1 line: `console.log("Hello, Docker!);`. If you want to ship this application to other machines, you must meet the following requirements:

- Start with an OS
- Install a runtime environment, for example Node
- Copy all app files
- Run `node app.js`

Docker solves this problem by defining a **Dockerfile**.

```
# Pull official Node image from Docker Hub, using the Alpine distribution
FROM node:alpine

# Copy all application files
COPY . /app

# Set the working directory to the /app directory in the Node image
WORKDIR /app

# Run the application
CMD node app.js
```

Now, we enter the following in the working directory in the terminal: `docker build -t hello-docker .`. `-t` tells Docker to add a tag with the build. We build from the current working directory using `.`.

To run the application, we enter `docker run hello-docker` in the terminal. We can see that the output is: "Hello, Docker!".