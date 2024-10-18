---
layout: default
title: Available Container Images
parent: Batch Jobs
nav_order: 40
has_children: false
description: ""
permalink: /batch-jobs/container-images
---

# Available Container Images
Containers have become increasingly popular, so the software you need to run your code may already have an existing container.
If you are just starting out with batch jobs, then we recommend that you use the container images that we provide for the CSU TIDE JupyterHub in batch jobs.
Your code will have access to the software that is present in the notebook container images and does not necessarily need to run in a Jupyter notebook.

## Finding Container Images
In order to find new container images, our first recommendation would be [Docker Hub](https://hub.docker.com/).
Docker Hub is one of the most popular and user-friendly container registries.

In addition to Docker Hub, you may find containers published on GitHub and GitLab repositories.
If you are using open source software hosted on one of these platforms, then check the repository for information about an official Docker container.
- *Note* The TIDE cluster runs Docker container images, but not Singularity containers. Singularity containers are used in more traditional HPC clusters.

When looking for container images, it is important to make sure that they come from a reputable source and are regularly updated.
On Docker Hub, you may find "trusted content" in the form of [Docker Official Images](https://hub.docker.com/search?image_filter=official), [Verified Publishers](https://hub.docker.com/search?image_filter=store) and [Sponsored OSS](https://hub.docker.com/search?image_filter=open_source).
For other container image registries, it is important to research the container image providers to determine their trustworthiness.

## Creating Custom Container Images
If you find that the software you need is not already in an available container image, then you can create a custom container image.
For more on this topic, check out our [container creation]() section.
