# Code With Mosh: The Ultimate Docker Course π³

## Before we start βοΈ

### Developer information ππΌββοΈ

Developed by Magnus RΓΈdseth, spring 2021.

### Description β

Docker is a platform for building, running, and shipping applications with ease. That's why most companies use it and are looking for software or DevOps engineers with Docker skills.

**But what is this course?** A clear, concise, comprehensive, and highly practical course that prepares you for the job. You'll learn everything about Docker from the absolute basics to more advanced concepts in under 5 hours. By the end of this course, you'll have a live full-stack application with a database and automated tests in the cloud.

### Goals π

By the end of this course, you'll be able toβ¦

β Confidently build and ship applications with Docker  
β Work in development teams using Docker  
β Troubleshoot issues like a pro

## So let's get started β©

### Introduction to Docker π³

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

**`Dockerfile`**

```Dockerfile
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

### The Linux Command Line βοΈ

#### Running Linux

In the terminal, run `docker run ubuntu`. In a way, this is a shortcut for pulling and running Docker images. After running the command, the Docker image process is terminated. Validate this by running `docker ps -a`, showing all running and stopped Docker containers.

Enter `docker run -it ubuntu` ,to run the Ubuntu container in interactive mode. In the terminal, you can see something like this: `root@db07914dac5c:/#`. `root` is the logged in user. `db07914dac5c` is the machine that Ubuntu is running on. `/` represents the root directory, and this is the current working directory. The `#` informs us that the current user (the `root` user) has the highest privileges. Non-root users should have a `$` instead of the `#`.

Typing `echo $0` displays the location of this shell program. This outputs `/bin/bash`.

**Important to note!** Linux is a case-sensitive operating system; `echo` is not the same as `Echo`. Additionally, we use `/` and not `\` in Linux, when navigating directories.

#### Managing Packages

In Linux, the package manager is called `apt`. Let's say we want to install the text editor Nano in Ubuntu.

**`Terminal`**

```shell
# Update the package database
apt update

# Install Nano using apt
apt install nano

# Remove Nano using apt
apt remove nano
```

#### Searching for text

`grep`, short for "Global Regular Expression Print", is an insanely useful command line tool.

**`Terminal`**

```shell
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

**`Terminal`**

```shell
# Create new dir, cd into it and output "Done"
mkdir test ; cd test ; echo Done
```

#### Environment Variables

**`Terminal`**

```shell
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

**`Terminal`**

```shell
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

### Building Images πΌ

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

**`Dockerfile`**

```Dockerfile
# Using base image with specific version
FROM node:14.16.0-alpine3.13
```

#### Copying files and directories

**`Dockerfile`**

```Dockerfile
# Set working dir to absolute path from root
WORKDIR /app

# Copy entire dir into working directory
COPY . .
```

**`Terminal`**

```shell
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

**`Dockerfile`**

```Dockerfile
# Use Node Package Manager to install dependencies
RUN npm install
```

#### Setting environment variables

We can set environment variables in the `Dockerfile`.

**`Dockerfile`**

```Dockerfile
# Sets env variable on machine
ENV API_URL=http://api.test-app.com/
```

#### Exposing Ports

Running `npm start`, the frontend application spins up on `localhost:3000`.

**`Dockerfile`**

```Dockerfile
# Does not automatically publish the port on the host.
# Is only a form of documentation,
# to tell us that the container will listen on port 3000.
EXPOSE 3000
```

#### Setting the user

By default, Docker run the application using the root user. This may cause security issues. Hence, we should take precautions to prevent this.

**`Terminal`**

```Dockerfile
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

**`Dockerfile`**

```Dockerfile
# Run all of the steps above in one line
RUN addgroup app && adduser -S -G app app

# Set the user on the machine
USER app
```

#### Defining Entrypoints

We want to use `ENTRYPOINT` when we know for sure that a given command should be executed when spinning up a container - there should be no exception.

**`Dockerfile`**

```Dockerfile
# Define the entrypoint to be run as an executable
ENTRYPOINT [ "npm", "start" ]
```

#### Speeding Up Builds

In order to understand how we can speed up builds in Docker, we must first understand layers in Docker. An image is a collection of layers. A layer can be thought of as the files modified after a given Docker instruction. As an example, running `FROM node:14.16.0-alpine3.13` adds certain files to the image, and that is one layer. Next, `RUN addgroup app && adduser -S -G app app` adds and edits new files, which are on another layer. This repeats for all instructions in the `Dockerfile`.

Layers of a Docker image can be inspected in the terminal.

**`Terminal`**

```shell
# Inspect history of this docker image
docker history react-app

...

=> RUN /bin/sh -c npm install # buildkit           175MB

=> RUN /bin/sh -c addgroup app && adduser -S -Gβ¦   4.84kB

...
```

As seen above, we can inspect what command added how much data to our machine.

Docker caches wherever possible. When a layer isn't changed between builds, Docker reuses the layer when building the image.

In order to run `npm install`, we only need `package-lock.json` and `package.json`. Thus, we can edit the `COPY` line in the `Dockerfile` as such:

**`Dockerfile`**

```Dockerfile
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

**`Terminal`**

```shell
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

**`Terminal`**

```shell
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

**`Terminal`**

```shell
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

**`Terminal`**

```shell
# Compress and zipped Docker image with specific tag
docker image save -o react-app.tar react-app:TAG

# Load Docker image from zipped file
docker image load -i react-app.tar
```

### Working with Containers π¦

#### Starting Containers

**`Terminal`**

```shell
# Run container in detached state
docker run -d react-app

# Run container in detached state with
# a custom, easy-to-read name
docker run -d --name custom-name-here react-app
```

#### Vieweing the Logs

**`Terminal`**

```shell
# Continuously output the log from the container
docker logs -f CONTAINER_ID

# Output the log from the container with timestamp
docker logs -t CONTAINER_ID
```

#### Publishing Ports

**`Terminal`**

```shell
# Run in detached state
# Publish on port 80 on the host (value before colon [:])
# from port 3000 on the container (value after colon [:])
# Set name "c1"
docker run -d -p 80:3000 --name c1 react-app

# Get overview of processes / running containers
docker ps

...

0.0.0.0:80 -> 3000/tcp

...
```

As seen in the output, the `localhost:80` is mapped to the container's port `3000`.

#### Executing Commands in Running Containers

Using `docker run`, we start a new container and run a command.

Using `docker exec`, we execute a command in a running container.

**`Terminal`**

```shell
# Assumes that c1 is a running container
# Outputs the files in current directory
docker exec c1 ls
```

#### Stopping and starting Containers

**`Terminal`**

```shell
# Stops the running container c1
docker stop c1

# Starts an existing container c1
docker start c1
```

Using `docker run`, we start a new container and run a command.

Using `docker start`, start an existing and previously stopped container.

#### Removing Containers

**`Terminal`**

```shell
# Removes an existing and stopped container
docker rm c1

# Forces container to stop and then removes it
docker rm -f c1

# Linux only
# Filters all Docker containers on the name provided
docker ps -a | grep c1

# Removes all stopped containers at once
docker container prune
```

#### Persisting Data using Volumes

A volume is a storage outside of containers. As an example, it can be located on the host or the cloud.

**`Dockerfile`**

```Dockerfile
# Create folder data using custom user
# This way, we ensure we have permission
RUN mkdir data
```

**`Terminal`**

```shell
# Creates new volume
docker volume create app-data

# Inspect an existing volume
docker volume inspect app-data

# Run in detatched
# Map ports
# Map app-data volume to a directory in the file system of the container
docker run -d -p 4000:3000 -v app-data:/app/data react-app
```

Volumes are the correct way to persist data in dockerized applications. Additionally, we can share volumes across containers.

#### Copying files between the Host and Containers

**`Terminal`**

```shell
# Copies an existing file from a container
# to the current working directory on the host
docker cp CONTAINER_ID:ABS_PATH_TO_FILE .

# Copies a local file on host machine to
# an absolute path in the container
docker cp secret.txt CONTAINER_ID:ABS_PATH
```

#### Sharing the Source Code with a Container

**`Terminal`**

```shell
# Start a container in detatched state
# Map ports
# Map the volume from current working directory to app
docker run -d -p 5001:3000 -v $(pwd):/app react-app
```

In practice, this lets you edit code base and see real-time changes in your Docker container.

### Running Multi-container Applications π€

#### What is Docker Compose?

Docker Compose is a tool built on top of the Docker engine. It makes it a lot easier to run multi-container applications.

Follow [this guide](https://docs.docker.com/compose/install/) to install Docker Compose.

To verify that you have Dcoker Compose installed, type `docker-compose --version` in your Terminal.

#### Cleaning up our Workspace

**`Terminal`**

```shell
# Outputs the image IDs of existing Docker images
docker image ls -q

# Force remove all the images outputted above
docker image rm -f $(docker image ls -q)

# Outputs the container IDs of existing Docker containers
docker container ls -a -q

# Force remove all the containers outputted above
docker container rm -f $(docker container ls -a -q)
```

Alternatively, you can use the following steps:

1. Click on the **Docker Desktop icon**
2. Go to **Preferences**
3. Click on the **Troubleshoot** button that looks like a bug
4. Click **Clean / Purge data**. Note that this will restart Docker engine.

#### Creating a Compose File

A Docker Compose file must be named `docker-compose.yml`. You can see a list of valid properties in Docker Compose [here](https://docs.docker.com/compose/compose-file/compose-file-v3/).

**`docker-compose.yml`**

```yaml
# Set the version of Docker Compose
version: "3.8"

services:
  frontend:
    # Tell Docker to look for Dockerfile in frontend
    # directory in current working directory
    build: ./frontend

    # Port mapping.
    # Because we can have multiple port mappings,
    # we use the list syntax in .yml
    ports:
      - 3000:3000
  backend:
    # Tell Docker to look for Dockerfile in backend
    # directory in current working directory
    build: ./backend

    ports:
      - 3001:3001

    # Set env variables
    environment:
      DB_URL: MONGO_DB_CONNECTION_STRING
  database:
    # Pull an existing Docker image
    image: mongo:4.0-xenial

    ports:
      - 27017:27017

    # Map custom volume VOLUME to a directory
    # /data/db is MongoDB's default storage folder
    volumes:
      - VOLUME:/data/db

# Define the custom volumes to be mapped later
# Note that this volume name corresponds with
# volumes in the database service
volumes:
  VOLUME:
```

#### Starting and stopping the Application

**`Terminal`**

```shell
# Start containers
docker-compose up

# Start containers in detached state
docker-compose up -d

# Outputs all running containers relevant to current app
docker-compose ps

# Stops and removes containers
# Images are still present
docker-compose down
```

#### Docker Networking

When we run our application with `docker-compose`, Docker Compose will automatically add a network, and then add our container to that network. This allows containers to talk to eachother.

**`Terminal`**

```shell
# Run containers in detached state
docker-compose up -d

# Outputs an overview of running Docker Networks
docker network ls
```

#### Viewing Logs

**`Terminal`**

```shell
# Outputs logs across all containers in aoo
docker-compose logs

# Outputs running containers
docker ps

# Follow the output of a given container
docker logs CONTAINER_ID -f
```

#### Migrating the Database

Oftentimes, we want our database to have a certain shape in a given release. This is called database migration.

This repository uses the `migrate-mongo` package for database migration.

**`backend/package.json`**

```json
 "scripts": {
    "db:up": "migrate-mongo up",
    "start": "nodemon --ignore './tests' index.js",
    "test": "jest --watchAll --colors"
  }
```

Thus, run `npm run db:up` to migrate database.

Because we need to wait for the database to be up and running in order to migrate it, we use a tool recommended by Docker called [`wait-for-it`](https://docs.docker.com/compose/startup-order/).

#### Running Tests

**`docker-compose.yml`**

```yaml
# Frontend tests in Dcoker
frontend-tests:
  # Use the newly built frontend image
  image: docker-course_frontend

  volumes:
    - ./frontend:/app

  # Override default command, and rather
  # run test
  command: npm test
```

### Deploying Applications βοΈ

#### Deployment Options

When deploying a dockerized application, we can either use single host deployment or cluster deployment.

**Single Host Deployment** is suitable for many cases, but may be lackluster in large-scale applications. Hence, Single Host Deployment will be used in this course.

**Cluster deployment** allow for high availability and scalability. There are different cluster solutions, where **Kubernetes** is the most popular solution.

#### Getting a Virtual Private Server

Some of the most popular VPS options include:

- DigitalOcean
- Google Cloud Platform (GCP)
- Microsoft Azure
- Amazon Web Services (AWS)

DigitalOcean is the simplest VPS. The further down the list we get, the more complex the VPS solution gets. **DigitalOcean** is the VPS solution used in this course.

#### Installing a Docker Machine

Follow [this](https://github.com/docker/machine/releases) **Installation Guide** on GitHub.

Verify that `docker-machine` is installed on your computer. by typing `docker-machine --version` in your terminal.

#### Provisioning a Host

Inspect available drivers using [this link](https://docs.docker.com/machine/drivers/). In this course, we will use [DigitalOcean](https://docs.docker.com/machine/drivers/digital-ocean/). Use the provided link to inspect commands options when creating the Docker Machine using the steps in the Terminal below.

Now, go to DigitalOcean Control Panel to acquire an access token, so that Docker Machine can talk to DigitalOcean. [Follow this documentation for DigitalCoean](https://docs.digitalocean.com/reference/api/create-personal-access-token/).

Execute the following in the Terminal.

**`Terminal`**

```shell
# Get DIGITAL_OCEAN_ACCESS_TOKEN from DigitalOcean cloud portal

# Install specific Docker version, because of
# unresolved problem with Docker Machine,
# as of December 2020: --engine-install-url

# DOCKER_MACHINE_NAME can be repository name
docker-machine create \
    --driver digitalocean \
    --digitalocean-access-token DIGITAL_OCEAN_ACCESS_TOKEN \
    --engine-install-url "https://releases.rancher.com/install-docker/19.03.9.sh" \
    DOCKER_MACHINE_NAME
```

By default the Docker Machine provisions with Ubuntu. Then, Docker is installed on the Docker Machine. You should now be able to see a new _Droplet_ on DigitalOcean cloud. A _Droplet_ is a fancy word for a server on DigitalOcean.

#### Connecting to the Host

To connect to the Docker Machine, we will use SSH in the Terminal. **The beauty of Docker Machine** is that it abstracts away the complexity of SSH from us, allowing for a swift a pain-free connection experience.

**`Terminal`**

```shell
# Outputs all existing Docker Machines
docker-machine ls

# Connects to given docker machine using its name
docker-machine ssh DOCKER_MACHINE_NAME
```

#### Defining the Production Configuration

We don't neccessarily want the exact same Compose file for development and production. Thus, we create `docker-compose.prod.yml` to define the Compose file used in production. `prod` can be interchanged with anything, be the following example follows convention.

**`docker-compose.prod.yml`**

```yaml
version: "3.8"

services:
  frontend:
    build: ./frontend

    # Map port 80 of the host to
    # port 3000 on the container
    ports:
      - 80:3000

    # Restarts container only if we manually # stop it
    restart: unless-stopped

    # Notice how there's no need for volumes in production

  backend:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
    command: ./docker-entrypoint.sh
    restart: unless-stopped

  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    # vidly is the application name
    volumes:
      - vidly:/data/db
    restart: unless-stopped

volumes:
  # vidly is the application name
  vidly:
```

Read more about restart policy in Compose files [here](https://docs.docker.com/compose/compose-file/compose-file-v3/#restart).

#### Reducing Docker Image Size

Docker images can get unbelievably large, which is not suitable for production.

Running `npm run build` in `/frontend` creates optimized assets for production. All these assets are stored in the `/frontend/build` folder.

Now, we can make a separate `Dockerfile` suitable for production, because all we need to do is copy files in `/frontend/build`.

So, we create a `Dockerfile.prod` in `/frontend`.

**`Dockerfile.prod`**

```Dockerfile
# Step 1: Build stage
FROM node:14.16.0-alpine3.13 AS build-stage

WORKDIR /app
COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# Step 2: Production stage
FROM nginx:1.12-alpine

RUN addgroup app && adduser -S -G app app

USER app

# Copy all files from build-stage to be use din production
COPY  --from=build-stage /app/build /usr/share/nginx/html

# Expose default port for web traffic
EXPOSE 80

ENTRYPOINT [ "nginx", "-g", "daemon off;" ]
```

Now, we can build an image for `/frontend` that is optimized for web using the terminal.

**`Terminal`**

```shell
# docker-course_web_opt describes how this images is
# optimized for web
docker build -t docker-course_web_opt -f Dockerfile.prod .

...

docker-course_web_opt   16.2MB
docker-course_frontend  300MB
docker-course_backend   184MB

...
```

Notice how the image built for web is only 16.2MB large.

In `docker-compose.prod.yml`, we need to change a few properties to fit our new `frontend/Dockerfile.prod`.

**`docker-compose.prod.yml`**

```yaml
---
frontend:
  # Set context to frontend directory
  # Specify Dockerfile
  build:
    context: ./frontend
    dockerfile: Dockerfile.prod

  # Map host port 80 to container port 80
  ports:
    - 80:80
  restart: unless-stopped
```

Now, we can run `docker-compose -f docker-compose.prod.yml build` in `/` directory.

#### Deploying the Application

**`Terminal`**

```shell
# Outputs overview of existing Docker Machines
docker-machine ls

# Outputs env variables for given Docker Machine
docker-machine env MACHINE_NAME

# Run this command to configure your shell:
eval $(docker-machine env MACHINE_NAME)

# The line above results in the following:
#   - Any commands typed in the terminal will be sent
#   to the MACHINE_NAME Docker Machine
#
#   - Thus, the Docker client in your terminal
#   will be talking to the Docker Machine on DigitalOcean

# Output overview of existing images on the remote Docker Machine
docker images

# Compose production build
docker-compose -f docker-compose.prod.yml up -d
```

If you encounter a **Permission denied** error when running `docker-compose -f docker-compose.prod.yml up -d` that looks a bit like the following:

**`Terminal Troubleshooting`**

```shell
npm WARN checkPermissions Missing write access to /app
npm WARN vidly-backend@1.0.0 No description
npm WARN vidly-backend@1.0.0 No repository field.

npm ERR! code EACCES
npm ERR! syscall access
npm ERR! path /app
npm ERR! errno -13
npm ERR! Error: EACCES: permission denied, access '/app'
npm ERR!  [Error: EACCES: permission denied, access '/app'] {
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/app'
npm ERR! }
```

This may be because the `Dockerfile` does not respect the `app` user when executing `WORKDIR /app`. This issue should be resolved in the latest version of Docker, as long as Docker uses build-kit. _If the output from Docker commands is colorized, this should not be a problem_. However, we are currently using an older version of Docker on this particular VPS (DigitalOcean).

To fix this issue, we edit the `backend/Dockerfile` as follows:

**`backend/Dockerfile`**

```Dockerfile
FROM node:14.16.0-alpine3.13

RUN addgroup app && adduser -S -G app app

# Create /app directory
# Set the current owner of this dir
# to be the app user
RUN mkdir /app && chown app:app /app

USER app

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```
