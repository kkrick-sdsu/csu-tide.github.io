---
layout: default
title: JupyterHub 2025-03-13
parent: Release Notes
nav_order: 10
has_children: false
description: ""
permalink: /release-notes/jupyterhub/2025-03-13
---
# JupyterHub Release Notes: 2025-03-13
We will be releasing the next version of JupyterHub for the following instances:
- [CSU TIDE JupyterHub]()
- [SDSU Research JupyterHub]()

## Improvements

### Updated JupyterHub to version 5.2.1
We have upgraded JupyterHub to version 5.2.1.
This version applies security fixes, bug fixes and new features.
See the official [release notes for JupyterHub to version 5.2.1](https://jupyterhub.readthedocs.io/en/stable/reference/changelog.html#id3).

### Updated notebook images
We have upgraded all of the notebook container images for compatibility with the latest version of JupyterHub.
We switched over to the official Jupyter Docker Stacks images published on quay.io.
Please see the updated [Available Container Images](/jupyterhub/images) page for more information.

### Server start form redesigned
We have redesigned the JupyterHub server start form to be more clear and compact.
The start form now provides reasonable defaults and set increments for both Central Processing Units (CPUs) and Random Access Memory (RAM).
The number and type of Graphical Processing Units (GPUs) have been combined into a single dropdown.
The notebook container images have been collapsed into a dropdown instead of a (ever increasing) set of radio buttons.
We have added the "Other" option to support users who wish to build and bring their own containers.
![JupyterHub start form](/images/release-notes/2025-03-13/start-form.png)

## New Features

### Shared storage
Research groups may now ask for access to shared storage which will allow members of a lab or group to access the same storage simultaneously.
Shared storage will be mounted at the path /home/jovyan/shared and will be named as requested, though we recommend short names for convenience.
- For example: `/home/jovyan/shared/rci`
- *Note*: Shared storage is not permitted to be used for installing packages via pip, conda or mamba, as a high volume of small files will degrade performance for all users.

### Shared memory
We have enabled share memory for applications that require this for distributed processing.
The amount of shared memory will be equal to the amount of RAM requested on the server start form.

### Group-based spawning
We can provide additional "profiles" for research labs or groups that override the options on the default profile.
This includes: CPUs, RAM, number & type of GPUs, and notebook container image.

### Bring your own container image
Using the new "Other" option for the Notebook Container Image field will allow you to enter a Docker image URI.
Your custom container image must be based on one of the [Jupyter Docker Stack images](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html) after the minimal notebook (see [image relationships](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#image-relationships) for more information).
- *Note*: Your base image should be built on or after 2024-07-29 for compatibility with the new version of JupyterHub

## Deprecations

### Old notebook container images
The previous set of notebook container images are now imcompatible with this version of JupyterHub due to a change in process for granting sudo access.
As a result, we are unable to offer a workaround for running the old notebook container images on the new version of JupyterHub.
We have published a repository with these [deprecated notebook container images](https://github.com/csu-tide/deprectated-notebook-images) including the URI for the notebook container image and the conda environment files.

If your code or project does not work with the new notebook container images, then you have two options:
1. Run the old notebook container image as a batch job
1. Attempt to recreate the conda environments in the new notebook container images

Both of these options are documented in greater detail in the [deprecated notebook container images](https://github.com/csu-tide/deprectated-notebook-images) repository.

### Stack PRP Notebook 
The Stack PRP Notebook has been discontinued by the developers and we recommend using the new PyTorch notebook as a replacement.
