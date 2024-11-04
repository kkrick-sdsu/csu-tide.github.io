---
layout: default
title: Storage Services Overview
parent: Storage Services
nav_order: 1
description: ""
permalink: /storage-services/
---

# Storage Services Overview
TIDE offers a total of 240 TB of usable storage configured as a [Ceph](https://ceph.io/en/){:target="_blank"} cluster and made available to Kubernetes via [Rook](https://rook.io/docs/rook/v1.9/ceph-storage.html){:target="_blank"}.
Ceph "provides a unified storage service with object, block, and file interfaces" which allows researchers to connect to their data in multiple ways.

Storage types on TIDE include the following:
- **Ephemeral Storage**: This type of storage is like a temp directory or scratch space and is cleaned up immediately after jobs have finished executing. This is the default storage option in Kubernetes.
- **Object Storage**: This type of storage manages data as objects, which include the data itself, metadata, and a unique identifier. It’s ideal for storing large amounts of unstructured data like photos, videos, and other types of files. This is available in the form of S3-compatible storage.
- **Block Storage**: This storage method divides data into fixed-size blocks and stores them separately. It offers high performance and low latency. This is available in the form of a PersistentVolumeClaim.
- **File Storage**: This storage type organizes data in a hierarchical structure of files and directories, similar to how data is stored on a personal computer. It is suitable for shared storage and collaborative environments. This is available in the form of a PersistentVolumeClaim.

## Comparing Storage Types
With the exception of object storage, all of the TIDE storage options can be mounted as linux volumes and used in a familiar way.
Whether you come from Linux, MacOS or Windows, you should be able to interact with your data in the form of files and folders like you normally would on a laptop or desktop computer.

However, all of the below storage types, with the exception of object storage, are limited to the namespace in which they are defined.
This means that any data created in one namespace cannot be accessed in another namespace.
For this reason, we recommend that *project data be stored in object storage* and then transferred as needed into one of the other storage types for processing.

Kubernetes has several access modes for data storage, but TIDE supports ReadWriteOnce (RWO) and ReadWriteMany (RWX).
- ReadWriteOnce indicates that a single pod on one node may read and write to the storage at a time.
- ReadWriteMany indicates that many pods on many nodes may read and write to the storage simultaneously.

In the table below, we compare the various storage types in the context of the TIDE cluster:

| Storage Type           | Persistent | Access Mode        | Accessible Outside of Namespace | Example Usage                                        |
|------------------------|------------|--------------------|---------------------------------|------------------------------------------------------|
| **Ephemeral Storage**  | No         | ReadWriteOnce      | No                              | Temporary data & scratch space for batch jobs        |
| **Object Storage**     | Yes        | ReadWriteMany      | Yes                             | Storing large datasets and sharing across namespaces |
| **Block Storage**      | Yes        | ReadWriteOnce      | No                              | JupyterHub home directory, processing buffer         |
| **File Storage**       | Yes        | ReadWriteMany      | No                              | Shared storage in a namespace                        |

## Storage Notices
When using TIDE storage it is important to note:
- TIDE storage is **not** CSU Protected level 1 compliant, nor is it suitable for HIPAA, PID, FISMA, FERPA, or similar regulated and protected data of any kind.
- TIDE storage is triple-replicated for redundancy, however TIDE storage is **not** backed up.
- TIDE storage is **not** intended for long-term storage, but rather for short-term storage while data is being generated, transferred, processed or analyzed.
- The TIDE Support Team reserves the right to **reclaim storage with sufficient notice**, though the notice period is arbitrary based on the size of the data.

We highly recommend that:
- Critical data be backed up externally to the TIDE cluster.
- Code be stored in a version control system on a remote repository, such as [GitHub](https://github.com/){:target="_blank"} or [GitLab](https://about.gitlab.com/){:target="_blank"}.

*Note*: Please check with your university's IT or research staff for approved long-term storage and version control solutions.

## Next Steps
If you would like access to object, block or file storage, please follow the [Requesting Storage](/storage-services/requesting-storage) guide.
