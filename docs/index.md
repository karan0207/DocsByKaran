# Docker

### Objective of this documentation
 Hello Guys, If you are reading this documentation then you might be wondering about the reason for creating this doc. Well, When I started building projects and web apps I was using github. But as a team for better scalability of project and easy version control over various functionalities you will eventually have to learn docker. This documentation will guide you to dockerize your mern web-app. 
<!-- For full documentation visit [mkdocs.org](https://www.mkdocs.org). -->

### What is a container?
 “A container is the standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. ”

This tool becomes very useful during the development phase. As many developers are involved in the process, it often becomes a hefty task of setting up the environment to run the project, as each project comes with their list of dependencies along with the specified versions.

A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. Container images become containers at runtime and in the case of Docker containers — images become containers when they run on Docker Engine.

### Docker Hub

Docker Hub is a cloud-based repository service provided by Docker in which users create, test, store and distribute container images. Through Docker Hub, a user can access public, open-source image repositories, as well as use space to create their own private repositories, automated build functions, webhooks and workgroups.

### Getting Started with the Integration

Our main focus in this tutorial is understanding how to integrate Docker with a MERN Stack Application. For explaining this, let’s try to dockerize an E-Commerce Web Application.

## Overview

We are going to Dockerize Node.JS, React, and MongoDB into separate containers. Then we are going to use DOCKER COMPOSE to run the multi-container application.

At last, from a single command, we can create and start all the services from our configuration.


### Initializing the Project
Clone the GitHub link to a local folder in your computer. Open the folder using VSCode or any text editor of your choice.

### Docker Files
Now, we need to create a Dockerfile for the server and the client. The Dockerfile essentially contains the build instructions to build the image.

Let’s start by creating the Dockerfile for the client (our React Frontend).

In the client folder, create a file called Dockerfile without any extension.
Write the following lines of code in the file:

```
# Dockerfile for React client

# Build react client
FROM node:10.16-alpine

# Working directory be app
WORKDIR /usr/src/app

COPY package*.json ./

###  Installing dependencies

RUN npm install --silent

# copy local files to app folder
COPY . .

EXPOSE 3000

CMD ["npm","start"]
```



<!-- ...................................... -->


We can simply build our Frontend with this command

```docker build -t react-app .```

To verify everything is fine, we run our newly built container using the command:```docker run -p 3000:3000 react-app``` . This will run just the Frontend.

In the same way, we create a file called Dockerfile for the Backend Server.

Now, we create a Dockerfile for the server directory.
Write the following lines of code in the file:
```
#  Dockerfile for Node Express Backend

FROM node:10.16-alpine

# Create App Directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install Dependencies
COPY package*.json ./

RUN npm install --silent

# Copy app source code
COPY . .

# Exports
EXPOSE 5000

CMD ["npm","start"]
```
We can simply build our Backend with this command:

```docker build -t node-app .```

Tips:- ```If you are new to docker then start by dockerizing single image at a time. In easy word you can start by building a docker image for frontend or backend (your choice). Once you get the idea then you can build multiple images and learn the concept of docker compose to run multiple images.
```

