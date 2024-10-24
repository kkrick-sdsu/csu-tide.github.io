---
layout: default
title: Quickstart
parent: Container Creation
nav_order: 10
has_children: false
description: ""
permalink: /container-creation/quickstart
---

{: .note }
This page is currently under construction. Information will be updated soon.

# Prerequisites
- [Docker Hub](https://app.docker.com/signup){:target="_blank"} Account
- [Docker](https://docs.docker.com/engine/install/){:target="_blank"} installed locally
- Familiarity with the Linux terminal
- Familiarity with text editors (vi, emacs, etc.)

## Overview
At a high-level, containers are packaged software applications containing all of their dependencies like the operating system (OS), file structures, configuration and other code libraries.

Containers offer many benefits but here we list a few of the most impactful ones:
1. Isolated Runtime Environments
    - Two or more containers running on the same system do not affect one another.
1. Portability
    - The same container can be run on a laptop, on the cloud, or on TIDE without being modified.
1. Consistency
    - The same container given the same input will produce the same output.

{: .note }
Docker must be installed on your local machine to proceed. Learn how to install Docker [here](https://docs.docker.com/engine/install/){:target="_blank"}

## Steps to Run a Container

### 1. Define the Code to Run
This tutorial will use the GitHub repository `hello-csu` for simple demonstration. Clone this repo to your local machine:

```bash
git clone https://github.com/csu-tide/hello-csu.git
cd hello-csu
```

### 2. Understand the Dockerfile
Use `vi Dockerfile` to open the Dockerfile. You'll see the following content:

```Dockerfile
FROM python:3

WORKDIR /usr/src/app

COPY hello.py /usr/src/app/hello.py

CMD ["python3", "hello.py"]
```

Here's a breakdown of each line in this file:

1. Defines a base image. In this case, the official Python3 image from Docker Hub
```Dockerfile
FROM python:3
```
1. Sets the working directory inside the container to `usr/src/app`
```Dockerfile
WORKDIR /usr/src/app
```
1. Copies the python file from hello-csu to the container working directory
```Dockerfile
COPY hello.py /usr/src/app/hello.py
```
1. Runs the hello.py file
```Dockerfile
CMD ["python3", "hello.py"]
```

### 3. Build the Docker Image
Using the above Dockerfile, build a Docker image with this command:

```bash
docker build -t testimage .
```

### 4. Run the Docker Container
Now that the image has been created from the Dockerfile, run a Docker container from the image:

```bash
docker run --name testcontainer testimage
``` 

*NOTE*: Since the only command in our image definition was to run the python file, the container stops automatically once that command completes.

This command will list the stopped container along with any others that have been created: 

```bash
docker container ls -a
```

### 6. Push the Image to [Docker Hub](https://app.docker.com/signup){:target="_blank"}

- Login to Docker Hub
```bash
docker login
```
- Tag the Image
```bash
docker tag testimage your-dockerhub-username/testimage:latest
```
- Push the Image
```bash
docker push your-dockerhub-username/testimage:latest
```

Then navigate to your [Docker Hub profile](https://hub.docker.com/){:target="_blank"} and look for the new image under the "Repositories" section

🎉 Congrats! 🎉 You've successfully built a Docker image from a Dockerfile, created a running Docker container from an image, and pushed that image to the Docker Hub Image Repository!