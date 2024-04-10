---
layout: default
title: Server Unavailable
parent: Frequently Asked Questions
grand_parent: JupyterHub
nav_order: 6
description: ""
permalink: /jupyterhub/faqs/serverunavailable
---

{: .note }
This page is currently under construction. Information will be updated soon.

# *Server unavailable or unreachable* Error
Sometimes, users may encounter a *Server unavailable or unreachable* error. This is often caused by the following:
1. Server timeout due to user inactivity (reopening yesterday's tab of JupyterLab)
2. [Manually stopping your server](/jupyterhub/faqs/stopnotebook) from another window/tab

![Example Server Unavailable Modal](/images/jupyterhub/faq-serverunavailable.png)

## Restart the Server
When you encounter the message above, **click the *Restart* button** to regain access to your notebook.

### Common Mistake
If you dismiss the message, errors will continue to pop-up (because your server has disconnected), restricting access to your notebook environment.

{: .note }
If you continue to encounter persistent *server unavailable or unreachable* messages despite restarting the server, please [contact TIDE](https://tide.sdsu.edu/){:target="_blank"} for further assistance.