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

{: .note }
This page is currently under construction. Information will be updated soon.

# Overview

[Technology Infrastructure for Data Exploration (TIDE)](https://tide.sdsu.edu/){:target="_blank"} provides SDSU researchers with access to research-class CPU & GPU resources and was built with a focus on AI and machine learning.
While AI and machine learning workflows are the focus of TIDE, they are not a requirement, meaning that other workloads amenable to containerization are welcome.

TIDE will initially be available for testing and feedback from the [science drivers](https://tide.sdsu.edu/science-drivers/#sdsu){:target="_blank"} before becoming generally availble to all researchers in the CSU.
For the latest updates, check out the [TIDE timeline](https://tide.sdsu.edu/timeline/){:target="_blank"}.

![Tech Logo](/images/jupyterhub/tide_logo_large.png)

## JupyterHub
TIDE offers a dedicated JupyterHub instance that offers simplifies access to TIDE resources in a browser-based development environment.
JupyterHub is the latest web-based interactive development environment for notebooks, code, and data. 
Its flexible interface allows users to configure and arrange workflows in data science, scientific computing, computational journalism, and machine learning. 
Languages such as Python, R, and Julia are available along with Pandas, PyTorch, and TensorFlow libraries.

### Getting Access
Users can request access by submitting a [TIDE Contact Us form](https://tide.sdsu.edu/contact/){:target="_blank"} with the Request Type of "Access to Existing Cyberinfrastructure" and sharing a few sentences about how they plan to use TIDE.

Once you have been granted access, you can click the button below and authenticate with your institution-specific credentials.

[Launch JupyterHub](https://csu-tide-jupyterhub.nrp-nautilus.io){: .btn .btn-green }{:target="_blank"}

## Batch / Long Running Jobs
In addition to the JupyterHub, more complex jobs can be created and run as software containers directly on the TIDE cluster.
Navigate to the [Quickstart](./kubectl/) is available to assist faculty and researchers with software containerization. 
Requests for batch access to TIDE can be made by submitting the [TIDE Contact Us form](https://tide.sdsu.edu/contact/).

*TIDE is supported by the Research & Cyberinfrastructure group within the IT Division. TIDE is funded by [NSF Award #2346701](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2346701)*
