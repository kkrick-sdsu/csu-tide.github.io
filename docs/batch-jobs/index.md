---
layout: default
title: Batch Jobs Overview
parent: Batch Jobs
nav_order: 1
description: ""
permalink: /batch-jobs/
---

# Batch Jobs on TIDE
Batch jobs are computer programs or scripts that are executed automatically in the background without user interaction.
They are typically used for tasks that can be performed repeatedly or over a long period of time, such as data processing, file transfer or model training.

TIDE, as part of the National Research Platform's (NRP) Nautilus Kubernetes cluster, supports scheduling of batch jobs through Kubectl, which is the command line interface (CLI) for interacting with Kubernetes.
Users wishing to run batch jobs on TIDE will need to [install Kubectl locally](https://kubernetes.io/docs/tasks/tools/#kubectl).

Kubernetes only runs software containers, so software must be "containerized" before it can be run in a batch job.
Often you may find that the software that your code relies on already has a container.
For more information on containerization, including creating custom containers, please check out the [Container Creation](/container-creation) section of our documentation.

## Context for Containers and Kubernetes
At a high-level, containers are packaged software applications containing all of their dependencies like the operating system (OS), file structures, configuration and other code libraries.

Containers offer many benefits but here we list a few of the most impactful ones:
1. Isolated Runtime Environments
    - Two or more containers running on the same system do not affect one another.
1. Portability
    - The same container can be run on a laptop, on the cloud, or on TIDE without being modified.
1. Consistency
    - The same container given the same input will produce the same output.

[Kubernetes](https://kubernetes.io/) is a container platform for "automating deployment, scaling and management of containerized applications." 
If you are familiar with more traditional HPC systems, you can think of Kubernetes like a workload manager (i.e. Slurm). 
Similar to workload managers, Kubernetes allows us to make requests for resources like CPUs, GPUs and memory to run our programs. 

## Batch Job Types
Kubernetes offers several workload management options to package up your code and run it as a batch job. 
Primarily we recommend Pods, Deployments and Jobs, each of which we describe in the following sub-sections.

### Pods
[Pods](https://kubernetes.io/docs/concepts/workloads/pods/) are the foundational Kubernetes compute unit.
Every container is encapsulated in a pod prior to being scheduled and run on the cluster.
It is important to note that pods are ephemeral and once a pod is deleted everything inside the pod is deleted -- meaning any data downloaded, content generated or files modified.
Generally used for short jobs or for writing and testing automation scripts prior to moving onto more robust Deployments or Jobs.

### Deployments
[Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) allow you to inform Kubernetes of your desired pod state, such as keeping one or more pods running at a given time.
If something happens that causes a pod to quit unexepectedly, then Kubernetes can take automatic actions to return your pod(s) to the desired state.
These can be useful for running short-lived services that your computation may require.

### Jobs
[Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/) will run your pod(s) until they successfully complete or meet a specified failure count.
Jobs can be used to run multiple pods in parallel.
This trait is specifically useful for "simply parallel" workflows such as running the same simulation with differing inputs.

### NRP-Specific Information
The NRP has also implemented additional logic and limitations on top of the foundational workload management options, which is covered in the following table:

| **NRP-Specific Information** | **Pods** | **Deployments** | **Jobs** |
|------------------------------|----------|-----------------|----------|
| **Maximum runtime**          | 6 hours  | 2 weeks         | Not limited |
| **Automatic shutdown**       | Yes, when the maximum runtime has been hit | Yes, when the maximum runtime has been hit (you may receive emails prior to the automatic shutdown) | Yes, when your script, program or linux command has finished |
| **Never-ending command**     | Yes, may contain a “never-ending” command such as “sleep infinity” | Yes, may contain a “never-ending” command such as “sleep infinity” | No, may not contain a “never-ending” command such as “sleep infinity” |
| **Interactive start**        | Yes, may be started interactively, such as via remote terminal session | Yes, may be started interactively, such as via remote terminal session | No, computation must start automatically via script or shell command |
| **Automatic restart**        | No, wll not be restarted automatically in cases such as the node being restarted or otherwise going offline | Yes, will be restarted automatically in cases such as the node being restarted or otherwise going offline | Yes, will be restarted automatically in cases such as the node being restarted or otherwise going offline |

## Next Steps
In order to schedule batch jobs on TIDE, you must first complete the steps in the [Getting Access](/batch-jobs/getting-access) guide.

Once you have completed those steps and been granted access, you can continute onto the [Getting Started](/batch-jobs/getting-started) guide.

After completing the getting started guide, you can experiment with customizing our [Batch Job Recipes](/batch-jobs/recipes).
