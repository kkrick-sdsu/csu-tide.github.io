---
layout: default
title: Using Storage
parent: Storage Services
nav_order: 20
has_children: false
description: ""
permalink: /storage-services/using-storage
---

# Using Storage


## JupyterHub Storage Workflow
JupyterHub > Copy data from S3 > Process Data > Copy data to S3

Keeping in mind the total storage of your home directory (I.E. /home/jovyan)
JupyterHub storage is provided by block storage.
This storage is capped and once the cap has been reached, you will not be able to store more data.
- *Note*: You may also run into issues when starting your notebook if your home directory has been filled.

Using Rclone

Using Boto3

Using Globus

## Batch Job Storage Workflow

