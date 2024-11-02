---
layout: default
title: Managing Storage
parent: Storage Services
nav_order: 30
has_children: false
description: ""
permalink: /storage-services/managing-storage
---

# Managing Storage
TIDE storage is a shared resource and we want to empower users to manage their storage allocations.
We will cover options for managing your storage capacity for block, file and object storage.
If you find that you need more capacity, you can request an increase to existing storage through the process detailed in the [Requesting Storage](/storage-services/requesting-storage) guide.

This documentation assumes that you are familiar with the content from the [Storage Services](/storage-services/) overview.
This documentation assumes that you have some experience with the linux command line.
If you are not familiar with the linux command line, we recommend this [beginner's guide](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview); you can use a terminal in [JupyterHub](/jupyterhub/) to follow along.

## Managing Block and File Storage
Block and file storage are mounted as linux volumes and can be managed in a similar fashion.
You can use several common linux commands to interact with these storage types.
It is important to check your usage of block and file storage periodically to avoid reaching full capacity.
Reaching full capacity will cause various issues that may disrupt your workflow.

### Checking Storage Capacity
You can check your storage capacity using the `df` command.
This will show you information for storage volumes including total, used and available capacity.

For example, on JupyterHub your persistent storage is provided by block storage which is mounted at the path `/home/jovyan`.
You can see your capacity by running this command:

```bash
df -h /home/jovyan
```
- *Note*: the `-h` prints storage size in human-friendly output

Example output:
```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/drbd1024    75G  9.1G   66G  13% /home/jovyan
```
In this case, we have 75 GB total, 9.1 GB used, and 66 GB remaining.

### Checking Storage Usage
You can check how much storage is used by a specific directory and its sub-directories using the `du` command.

For example, if you wanted to see how much space your home directory in JupyterHub is using, then you could run:
```bash
du -h /home/jovyan
```
- *Note*: the `-h` prints storage size in human-friendly output

Example output:
- *Note* this output has been truncated for brevity
```bash
2.1M    /home/jovyan/tutorial/LOGS
568K    /home/jovyan/tutorial/photos
60M     /home/jovyan/tutorial
20K     /home/jovyan/.scm_gui/userscripts
64K     /home/jovyan/.scm_gui
0       /home/jovyan/.scm/packages/AMS2024.1.packages
0       /home/jovyan/.scm/packages
0       /home/jovyan/.scm
8.6G    /home/jovyan
```

You could then inspect the directories to see if you still need the data that is stored there.

## Managing Object Storage
Object storage is managed differently than block and file storage, since it is not mounted as a linux volume.
TIDE object storage is [S3-compatible](https://en.wikipedia.org/wiki/Amazon_S3#S3_API_and_competing_services), so there are many tools that may be used to manage TIDE object storage.

### Checking Usage
The National Research Platform (NRP) provides a useful [storage page](https://portal.nrp-nautilus.io/storage) that provides a high-level view of the object storage use for TIDE.
This information is updated about once every 24 hours, so there may be a delay.
You can check when the information was last updated using the "LastChecked" column.

![TIDE S3 storage usage in terms of gigabytes used by individual buckets](/images/storage-services/managing-storage-1.png)

To access this information follow these steps:
1. Navigate to the [storage page](https://portal.nrp-nautilus.io/storage)
1. Sign in using your single sign-on credentials
1. Click the "Pool" drop down and select "TIDE S3"
1. Click the tidesupport user
1. Peruse the object storage buckets and storage consumption

### Using Rclone
[Rclone](https://rclone.org/) is a popular command line tool that offers file transfer support for several cloud providers.
We recommend it here as it supports S3-compatible storage providers like TIDE's Ceph object store.
Rclone's syntax is inspired by linux file management commands, so it may feel somewhat familiar though it does have some differences.

You can use the following text guide to configure Rclone on your local computer. 
This text guide may be supplemented with this [recording for configuring Rclone](https://drive.google.com/file/d/1Xg63oOs7MN5z01HGm5CGCM9o9Vq_-W_w/view?usp=sharing).
- *Note*: Rclone is actively developed and the configuration options may change order, the important thing is to use the "S3 Compliant" and "Ceph object storage" options

1. Start rclone configuration
    - `rclone config`
1. New remote
    - `n`
1. Recommend a short name for typing and using in scripts
    - `s3`
1. Search for the number associated with "Amazon S3 Compliant Storage Providers ..."
    - At the time of writing this was option 4
    - `4`
1. Search for the number associated with "Ceph Object Storage"
    - At the time of writing this was option 4
    - `4`
1. Enter AWS Credentials
    - At the time of writing this was option 1
    - `1`
    - Note: These will be displayed in plain text
1. Enter your Access Key
1. Enter your Secret Key
1. Leave the Region empty (hit enter)
1. Since we are setting this up on a local computer, we are external to the cluster, so we will use the external endpoint
    - `https://s3-tide.nrp-nautilus.io`
    - *Note*: If you are configuring this for use inside the cluster I.E. in a batch job or JupyterHub then the endpoint is `http://rook-ceph-rgw-tide.rook-tide`
    - *Note*: If you received a different set of endpoints when you were granted your S3 credentials, then please use those instead
1. Leave the location constraint empty (hit enter)
1. For the ACL, use "Owner gets FULL_CONTROL"
    - At the time of writing this was option 1
    - `1`
1. Leave server-side encryption empty (hit enter)
1. Leave KMS ID empty (hit enter)
1. Do not edit advanced config
    - `n`
1. Keep this remote
    - `y`
1. You should now see a list of your remotes:
    ```bash
    Current remotes:

    Name                 Type
    ====                 ====
    s3                   s3
    ```
1. Quit rclone config
    - `q`
1. Test your configuration
    - `rclone ls s3:my-bucket`
    - *Note*: Replace 's3' with your chosen remote name, replace 'my-bucket' with your bucket name
1. If you did not get an error message, then your configuration was successful!
    - *Note*: You may not get any output, this just means that your bucket is empty

