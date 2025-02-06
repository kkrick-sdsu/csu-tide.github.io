---
layout: default
title: Resource Monitoring
parent: Batch Jobs
nav_order: 60
has_children: false
description: ""
permalink: /batch-jobs/resource-monitoring
---

# Resource Monitoring

This guide will walk you through determining available resources on the cluster and monitoring the resources you are consuming personally. 
Once you have completed this guide you should be able to:
- View available resources on the Nautilus website
- Determine the resources you should be requesting
- View the CPU and GPU resources you are consuming on Grafana

## Prerequisites

This documentation assumes that you have:
- Completed the [Getting Access](/batch-jobs/getting-access) guide
- todo
- todo

## Available resources

### Determine available resources on Nautilus

Nautilus provides a [resources](https://portal.nrp-nautilus.io/resources){:target="_blank"} page that outlines all of the nodes in our cluster and their available resources in the form of a table. 
A node is another word to describe the computer that the servers are running on.
With the provided table we can determine what resources are available to us.

1. Navigate to [https://portal.nrp-nautilus.io/](https://portal.nrp-nautilus.io/){:target="_blank"}.
1. Click on the Resources tab.
  - ![Nautilus homepage](/images/batch-jobs/resourcemonitoring1.png)
1. Notice the table is filled with _all_ nodes in the cluster, refine your results by putting `rci-tide` in the Name entry section.
  - ![Nautilus resources tab with name search](/images/batch-jobs/resourcemonitoring2.png)

For more information, including documentation on determining usage with Tensorboard, see [Nautilus' monitoring documentation](https://docs.nationalresearchplatform.org/userdocs/running/monitoring/).

## Resource consumption

### Resource requests

### Determine resource consumption with Grafana