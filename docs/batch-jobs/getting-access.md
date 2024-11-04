---
layout: default
title: Getting Access
parent: Batch Jobs
nav_order: 10
description: ""
permalink: /batch-jobs/getting-access
---

# Getting Access
In order to run containers on TIDE, you will need an account with the National Research Platform (NRP) and you will need to be added to a namespace.
A namespace allows its members to share access to containers and data, enabling collaboration.

## Creating a National Research Platform Portal Account
The NRP portal will automatically create you an account once you login with your CSU-specific login credentials. Follow these steps to sign into the NRP portal:

1. Navigate to [https://portal.nrp-nautilus.io/](https://portal.nrp-nautilus.io/){:target="_blank"}
1. Click Login in the top right corner
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
1. You should now be redirected to the NRP Portal and you should see your CSU email in the top right corner
  - Note: You may not see as many options in the top navigation, as the screenshot below is from an admin account
  - ![Signed in NRP Portal](/images/batch-jobs/gettingaccess10.png)
1. Read and accept the Acceptable Use Policy (AUP) at the home page
  - Note: You'll need to re-accept this policy every 12 months to maintain your access
  - ![Nautilus Portal AUP](/images/batch-jobs/gettingaccess14.png)
1. Verify that you've agreed to the AUP
  - ![Nautilus Portal AUP Accepted](/images/batch-jobs/gettingaccess15.png)

Congratulations! You now have an NRP account and you're one step closer to running batch jobs on TIDE.

### NRP Portal tasks:
1. [Peruse the namespaces](https://portal.nrp-nautilus.io/namespaces-g){:target="_blank"}
    - It may be easiest to use your browser's find tool (ctrl + f / cmd + f)
        - ![TIDE Namepaces](/images/batch-jobs/gettingaccess12.png)
1. [Download your kube config](https://portal.nrp-nautilus.io/authConfig){:target="_blank"}
    - You will need to sign in with CILogon again
    - The file should download and you should see the following screen
        - ![kube config file](/images/batch-jobs/gettingaccess13.png)
    - Open your terminal and run the following command:
        - `mkdir ~/.kube`
    - Copy your config file into the ~/.kube folder (replace the path below for your config file):
        - `cp ~/Downloads/config ~/.kube/config`
    - Check to make sure your config file copied successfully:
        - `ls ~/.kube`

## Requesting Access to a Namespace
Now that you have signed into the NRP Portal, the TIDE team can add you to a namespace which will allow you to run containers on TIDE.

Submit the [TIDE Support form](https://tide.sdsu.edu/tide-support-request/){:target="_blank"} with the request type as "Namespace Access or Issue".

After you submit the form a member of the TIDE team will follow up and add you to the desired namespace.

## Next Steps
Once you have an NRP account and have been added to a namespace, continue to the batch jobs [Getting Started](/batch-jobs/getting-started) guide to run your first batch job on TIDE.
