---
layout: default
title: Check Disk Quota
parent: Frequently Asked Questions
grand_parent: JupyterHub
nav_order: 5
description: ""
permalink: /jupyterhub/faqs/diskquota
---

# Check Disk Quota

Each user is granted a finite amount of persistent storage space accessible in JupyterHub at the home directory `/home/jovyan` (and across each project that you may be using JupyterHub). For an exact amount, please reference the appropriate JupyterHub information on the [Getting Access](/jupyterhub/gettingaccess) page.

To check how much of your persistent home directory storage that you are consuming:

1. From the **Launcher** page select **Terminal**.
![Launch Terminal](/images/jupyterhub/faq-space1.png)
1. The Terminal lets you enter commands and interact with the underlying container executing your code. Enter the following command to view details about container disk usage:
```
df -h
```
![Terminal Details](/images/jupyterhub/faq-space2.png)
1. Look for the `/home/jovyan` file filesystem. This is where your `home` directory resides. In the example above, you'll see its size is 20 GB (gigabyte), 177 MB (megabyte) is used, and 20 GB (gigabyte) is free/available. This file system is the same one you see in the left-hand side of JupyterHub.

{: .note }
If you need to clean up or back up files, please take a look at [disk space management](/jupyterhub/faqs/spacemanagement).