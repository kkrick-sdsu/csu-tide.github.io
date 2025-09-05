---
layout: default
title: Getting Access
parent: Batch Jobs
nav_order: 10
description: ""
permalink: /batch-jobs/getting-access
---

# Getting Access
In order to run batch jobs on TIDE, you will need an account with the National Research Platform (NRP) and you will need to be added to a namespace.
A namespace allows its members to share access to batch jobs and storage, enabling collaboration.

## Creating a National Research Platform Portal Account
The NRP portal will automatically create you an account once you login with your CSU-specific login credentials. Follow these steps to sign into the NRP portal:

1. Navigate to [https://nrp.ai/](https://nrp.ai/){:target="_blank"}
1. Click "Log In" in the top right corner
  - ![Nautilus Portal webpage](/images/batch-jobs/gettingaccess1.png)
1. You will be greeted by a CILogon page
  - ![CILogon webpage](/images/batch-jobs/gettingaccess2.png)
1. Click the dropdown in the Identity Provider section and search for your institution
  - ![Search Your Institution](/images/batch-jobs/gettingaccess3.png)
1. Click the checkbox for "Remember this selection"
  - ![Check remember this selection](/images/batch-jobs/gettingaccess4.png)
1. Click Log On
  - ![Log on](/images/batch-jobs/gettingaccess5.png)
1. You will be greeted by a login page
  - ![Institution Login page](/images/batch-jobs/gettingaccess6.png)
1. Enter your CSU-specific credentials
  - ![Institution credentials](/images/batch-jobs/gettingaccess7.png)
1. Click Login
  - ![Institution Login](/images/batch-jobs/gettingaccess8.png)
1. Complete any multi-factor authentication (if prompted)
  - ![DUO Push Notification](/images/batch-jobs/gettingaccess9.png)
1. You should now be redirected to the NRP Portal and you should see your randomly selected profile icon in the top right corner
  - ![Signed into NRP Portal](/images/batch-jobs/gettingaccess10.png)
1. Read the [Acceptable Use Policy (AUP)](https://nrp.ai/NRP-AUP.pdf){:target="_blank"}
1. Read the [Cluster Policies](https://nrp.ai/documentation/userdocs/start/policies/){:target="_blank"}

## Requesting Access to a Namespace
Now that you have signed into the NRP Portal, the TIDE support team can add you to a namespace which will allow you to run batch jobs on TIDE.

Submit the [TIDE Support form](https://tide.sdsu.edu/tide-support-request/){:target="_blank"} with the request type as "Namespace Access or Issue".

After you submit the form, a member of the TIDE support team will follow up and add you to the desired namespace.

## Installing Kubectl and Authenticating
Once you have been added to a namespace, then you can proceed to install and configure Kubectl.
This command line tool will allow you to interact with your namespace to schedule and access batch jobs.

Please follow the NRP documentation for [cluster access via kubectl](https://nrp.ai/documentation/userdocs/start/getting-started/#cluster-access-via-kubectl){:target="_blank"}.

## Next Steps
Once you have an NRP account and have been added to a namespace, continue to the batch jobs [Getting Started](/batch-jobs/getting-started) guide to run your first batch job on TIDE.
