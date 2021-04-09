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

### Introduction to Docker 🐳

#### What is Docker?

Docker is a platform for building, running and shipping applications in a consistent manner. In practice, this means that if the application runs on your development machine, you're sure it can run on test and production machines.

Additionally, if a new developer needs to spin up your application, he simply needs to tell Docker to start the application, without the need for much configuration.

#### Virtual Machines versus Containers

A **virtual machine**, or VMs for short, is an absatraction of a machine or physical hardware. Using VMs, we can _run apps in isolated environments_. However, each VM _needs a full OS to function_. This causes it to be _slow to start_ and quite _resource intensive_.

A **container** is an isolated environment for running an application. As with VMs, containers also allow us to _run multiple apps in isolation_. Additionally, they are _lightweight_ and _use the OS of the host_. Because the OS already runs on the host, a container can _start relatively quickly_. Containers also require _less hardware resources_. In theory, using containers, we can run thousands of containers side by side on one host.

#### Docker Architecture

Docker uses a client-server architecture using a REST API. All containers running on a host share the operating system kernel.

**What's a kernel?** A kernel is the core of an operating system, which manages applications and hardware resources. Different operating systems have different kernel with different APIs. Thus, we cannot run a Windows application on Linux, because the Windows application needs to talk to the uncompatible Linux kernel under the hood.

**On Linux OS**, we can only run Linux containers, because the containers need to run on Linux.

**On Windows OS**, we can run both Windows and Linux containers, because Windows 10 ships with both a Windows kernel and a Linux kernel.

**Docker on Mac OS** uses a lightweight Linux virtual machine to run Linux containers using the Linux kernel.

#### Development Workflow

Let's say you have a full-stack web application that you want to dockerize. Now, you add a Dockerfile.

A **Dockerfile** is a plaintext file that includes instructions that Docker uses to package the application into an image.

A **Docker Image** has a cut-down OS, a runtime environment, application files, third-party libraries, environment variables, etc...

Once we have built the image, we can tell Docker to create a container from that image.

Once we have a complete Docker image, we can push it to **Docker Hub**. Docker Hub to Docker is like GitHub to Git; it's a storage for Docker Images that anyone can pull down a specific version of our application and use it.

#### Docker in action

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

### The Linux Command Line ⚙️

#### Running Linux

In the terminal, run `docker run ubuntu`. In a way, this is a shortcut for pulling and running Docker images. After running the command, the Docker image process is terminated. Validate this by running `docker ps -a`, showing all running and stopped Docker containers.

Enter `docker run -it ubuntu` ,to run the Ubuntu container in interactive mode. In the terminal, you can see something like this: `root@db07914dac5c:/#`. `root` is the logged in user. `db07914dac5c` is the machine that Ubuntu is running on. `/` represents the root directory, and this is the current working directory. The `#` informs us that the current user (the `root` user) has the highest privileges. Non-root users should have a `$` instead of the `#`.

Typing `echo $0` displays the location of this shell program. This outputs `/bin/bash`.

**Important to note!** Linux is a case-sensitive operating system; `echo` is not the same as `Echo`. Additionally, we use `/` and not `\` in Linux, when navigating directories.

#### Managing Packages

In Linux, the package manager is called `apt`. Let's say we want to install the text editor Nano in Ubuntu.

```
# Update the package database
apt update

# Install Nano using apt
apt install nano

# Remove Nano using apt
apt remove nano
```

#### Searching for text

`grep`, short for "Global Regular Expression Print", is an insanely useful command line tool.

```
# Case sensitive search for "hello" in file1.txt
grep hello file1.txt

# Case insensitive search for "hello" in file1.txt
grep -i hello file1.txt

# Case insensitive search for "hello" in multiple files
grep -i hello file1.txt file2.txt

# Case insensitive search for "hello" in all files that start with "file"
grep -i hello file*

# Case insensitive search for "hello" in current working directory
grep -i -r hello .

# Same as above, but Linux allows for combined options
grep -ir hello .
```

#### Chaining Commands

Linux allows us to chain commands. This is very useful when defining workflows using Docker.

```
# Create new dir, cd into it and output "Done"
mkdir test ; cd test ; echo Done
```

#### Environment Variables

```
# Prints all environment variables on the machine
printenv

# Prints the value of PATH env variable
printenv PATH

# Set env variable in the current terminal session
export DB_USER=john

# Prints "john"
echo $DB_USER

# Append env variable to .bashrc
echo DB_USER=john >> .bashrc
```

#### Managing Users

```
# Add new user. Set home directory using -m.
useradd -m john

# Last line of this print shows the new added user
cat /etc/passwd

# Modify user record. Set shell of user john using -s.
usermod -s /bin/bash john

# Prints credentials in encrypted format. Only accessible to root user.
cat /etc/shadow

# Starts an interactive bash session with user john, using a given Docker container ID
docker exec -it -u john 2f759e6996e9 bash

# Delete user
userdel john

# Executes useradd under the hood for a more interactive way of creating new users
adduser adam
```

### Building Images 🖼

#### Images and Containers

**An image** includes everything an application needs to run:

- A cut-down OS
- Third-party libraries
- Application files
- Environment variables
- etc...

**A container** provides an isolated environment. Containers can be started, stopped and restarted. Technically, a container is just a process; that is, a running instance of an application.

#### Dockerfile Instructions

When defining the base image, always use a specific version. If you're using Node as base image, don't use `node:latest` in the `Dockerfile`. This may cause unpredictable releases in the future. **Always specify the version of images you use.**

```
# Using base image with specific version
FROM node:14.16.0-alpine3.13
```

#### Copying files and directories

**Dockerfile**

```
FROM node:14.16.0-alpine3.13

# Set working dir to absolute path from root
WORKDIR /app

# Copy entire dir into working directory
COPY . .
```

**Terminal**

```
# Build Docker image
docker build -t react-app .

# Run Docker container in interactive mode with shell script
docker run -it react-app sh

# Output dir structure. This will output this docker-course repository dir structure, because we copied its contents to the container.
ls
```
