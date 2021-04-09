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

**Dockerfile**

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

**Terminal**

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

**Terminal**

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

**Terminal**

```
# Create new dir, cd into it and output "Done"
mkdir test ; cd test ; echo Done
```

#### Environment Variables

**Terminal**

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

**Terminal**

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

**Dockerfile**

```
# Using base image with specific version
FROM node:14.16.0-alpine3.13
```

#### Copying files and directories

**Dockerfile**

```
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

# Output dir structure.
# This will output this docker-course repository dir structure,
# because we copied its contents to the container.
ls
```

#### Excluding files and directories

We can exclude files and directories using a `.dockerignore` file.

You can inspect the `.dockerignore` file [here](/frontend/.dockerignore).

Note that when running `docker build -t react-app .`, our build context is significantly smaller after ignoring `node_modules`. Additionally, note that the `node_modules` folder is not copied over to the container machine. Thus, we need to run `npm install` to get the dependencies.

#### Running Commands

We use the `RUN` command in the `Dockerfile` to run available commands.

**Dockerfile**

```
# Use Node Package Manager to install dependencies
RUN npm install
```

#### Setting environment variables

We can set environment variables in the `Dockerfile`.

**Dockerfile**

```
# Sets env variable on machine
ENV API_URL=http://api.test-app.com/
```

#### Exposing Ports

Running `npm start`, the frontend application spins up on `localhost:3000`.

**Dockerfile**

```
# Does not automatically publish the port on the host.
# Is only a form of documentation,
# to tell us that the container will listen on port 3000.
EXPOSE 3000
```

#### Setting the user

By default, Docker run the application using the root user. This may cause security issues. Hence, we should take precautions to prevent this.

**Terminal**

```
# Run alpine Linux in interactive mode
docker run -it alpine

# Add new group
addgroup app

# Add new system user in the app group with the username app.
# This is common best practice.
adduser -S -G app app

# Outputs the groups the the user "app" belongs to
groups app
```

**Dockerfile**

```
# Run all of the steps above in one line
RUN addgroup app && adduser -S -G app app

# Set the user on the machine
USER app
```

#### Defining Entrypoints

We want to use `ENTRYPOINT` when we know for sure that a given command should be executed when spinning up a container - there should be no exception.

**Dockerfile**

```
# Define the entrypoint to be run as an executable
ENTRYPOINT [ "npm", "start" ]
```

#### Speeding Up Builds

In order to understand how we can speed up builds in Docker, we must first understand layers in Docker. An image is a collection of layers. A layer can be thought of as the files modified after a given Docker instruction. As an example, running `FROM node:14.16.0-alpine3.13` adds certain files to the image, and that is one layer. Next, `RUN addgroup app && adduser -S -G app app` adds and edits new files, which are on another layer. This repeats for all instructions in the `Dockerfile`.

Layers of a Docker image can be inspected in the terminal.

**Terminal**

```
# Inspect history of this docker image
docker history react-app

...

=> RUN /bin/sh -c npm install # buildkit           175MB

=> RUN /bin/sh -c addgroup app && adduser -S -G…   4.84kB

...
```

As seen above, we can inspect what command added how much data to our machine.

Docker caches wherever possible. When a layer isn't changed between builds, Docker reuses the layer when building the image.

In order to run `npm install`, we only need `package-lock.json` and `package.json`. Thus, we can edit the `COPY` line in the `Dockerfile` as such:

**Dockerfile**

```
# Only copy package files
COPY package*.json .

# Install dependencies
RUN npm install

# Copy rest of files
COPY . .

...

# Result in terminal
=> CACHED [2/6] RUN addgroup app && adduser -S -G app app   0.0s
=> CACHED [3/6] WORKDIR /app                                0.0s
=> CACHED [4/6] COPY package*.json .                        0.0s
=> CACHED [5/6] RUN npm install                             0.0s
=> CACHED [6/6] COPY . .                                    0.0s

...
```

#### Removing images

**Terminal**

```
# Remove dangling images
docker image prune

# Remove stopped containers
docker container prune

# Remove image with image id.
# Can also use the name of the image.
# Can also add more IDs after the initial IMAGE_ID
# to remove several images.
docker image IMAGE_ID
```

#### Tagging images

```
# Replace TAG with the desired tag
# May be version number or codenames
docker build -t react-app:TAG .

# Remove image with given tag
docker image remove react-app:TAG

# Give existing image a new tag
docker image tag IMAGE_ID react-app:TAG
```

#### Sharing Images

1. **Login** to [Docker Hub](https://hub.docker.com/)
2. Navigate to **Repositories**
3. Click **Create Repository**
4. Set **desired name, description and other settings**
5. Optionally, you can link Docker Hub to GitHub to **automatically build**
6. When you are happy with the settings, click **Create**
7. **Create** Docker image, **login** using terminal and **push** to Docker Hub using commands below

```
# Create Docker image to be uploaded with
# given tag to Docker Hub
docker image tag IMAGE_ID magnusrodseth/react-app:TAG

# You have to provide username and password
docker login

# Push image with tag to Docker Hub
docker push magnusrodseth/react-app:TAG
```

Your image should now be available to pull on Docker Hub.

#### Saving and Loading Images

Let's say you have a Docker image that you want on another machine without going via Docker Hub.

```
# Compress and zipped Docker image with specific tag
docker image save -o react-app.tar react-app:TAG

# Load Docker image from zipped file
docker image load -i react-app.tar
```
