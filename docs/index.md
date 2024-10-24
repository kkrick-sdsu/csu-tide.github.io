---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
nav_order: 10
has_children: false
description: ""
permalink: /
---

<img src="/images/jupyterhub/tide_logo_large.png" alt="TIDE Technology Infrastructure for Data Exploration" height="200" width="600">

# Overview
[Technology Infrastructure for Data Exploration (TIDE)](https://tide.sdsu.edu/){:target="_blank"} provides California State University (CSU) researchers with free access to research-class computational resources focused on artificial intelligence (AI) and machine learning (ML). 
While AI and ML workflows are the focus of TIDE, they are not a requirement, meaning that other workloads amenable to containerization are welcome.

For the latest updates, check out the [TIDE timeline](https://tide.sdsu.edu/timeline/){:target="_blank"}.

## JupyterHub
TIDE offers a dedicated JupyterHub instance that offers simplified access to TIDE resources in a browser-based development environment.
JupyterHub is the latest web-based interactive development environment for notebooks, code, and data. 
Its flexible interface allows users to configure and arrange workflows in data science, scientific computing, computational journalism, and machine learning.
Languages such as Python, R, and Julia are available along with Pandas, PyTorch, and TensorFlow libraries.

[JupyterHub Info](/jupyterhub){: .btn .btn-green }

## Batch Jobs
In addition to the JupyterHub instance, batch jobs can be created and run as software containers directly on the TIDE cluster.
Batch jobs allow for longer running jobs as well as parallelization of jobs.

[Batch Jobs Info](/batch-jobs){: .btn .btn-green }

## Compute Power
With 27 Dell PowerEdge servers, TIDE is equipped to run your AI and ML workloads.
TIDE offers 17 GPU nodes each equipped with 4 NVIDIA L40 48 GB RAM GPUs for a total of 68 L40 GPUs.
In addtion to the GPU nodes, TIDE has one GPU-Advanced node equipped with 4 NVIDIA A100 80 GB RAM GPUs.

For workloads that do not require access to GPUs, we have 6 CPU nodes with 64 CPUs each totalling 384 CPUs.
The CPU nodes are also equipped with higher amounts of RAM to support CPU-only workloads.

TIDE has 3 storage nodes that are configured as a Ceph cluster with triple replication and offers a combined total of 240 TB usable storage.

| Node Type    | Quantity | Specifications for each Node |
|:-------------|:---------|:------|
| GPU          | 17       | PowerEdge R760<br/> (2x) Intel Xeon SIlver 4410Y 2G, 12C/24T <br/> (4x) NVIDIA L40, 48 GB RAM<br/> 512 GB System RAM |
| GPU-Advanced    | 1        | PowerEdge R750XA<br />(2x) Intel Xeon Gold 6338 2G CPU, 32C/64T<br/>(4x) NVIDIA A100 GPU, 80 GB RAM<br/>512 GB System RAM |
| CPU          | 6        | PowerEdge R760<br/>(2x) Intel Xeon Gold 6430 2.1G, 32C/64T <br/> 768 GB System RAM |
| Storage      | 3        | PowerEdge R760<br/> (3x) Intel Xeon Gold 6442Y 2.6G, 24C/48T <br/> 240 TB Storage<br/>256 GB System RAM |

*TIDE is funded by [NSF Award #2346701](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2346701).
TIDE is integrated with the [National Research Platform](https://nationalresearchplatform.org/).*
