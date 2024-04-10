---
layout: default
title: Getting Access
parent: Containerization
nav_order: 2
description: ""
permalink: /containerization/gettingaccess
---

{: .note }
This page is currently under construction. Information will be updated soon.

# Getting Access
In order to run containers on TIDE, you will need an account with the National Research Platform and you will need to be added to a namespace. A namespace allows its members to share access to containers, enabling collaboration.

In addition to the written directions below, we've recorded a video walkthrough for [getting an account and requesting access to a namespace](https://mediasite.sdsu.edu/Mediasite/Play/8e7f235bc56f44fdb4586cffe1e477a71d){:target="_blank"}.

## Creating a National Research Platform Portal Account
The National Research Platform (NRP) portal will automatically create you an account once you login with your CSU-specific login credentials. Follow these steps to sign into the NRP portal:

1. Navigate to [https://portal.nrp-nautilus.io/](https://portal.nrp-nautilus.io/)
1. Click Login in the top right corner
  - ![Nautilus Portal webpage](/images/containerization/gettingaccess1.png)
1. You will be greeted by a CILogon page
  - ![CILogon webpage](/images/containerization/gettingaccess2.png)
1. Click the dropdown in the Identity Provider section and search for your institution
  - ![Search Your Institution](/images/containerization/gettingaccess3.png)
1. Click the checkbox for "Remember this selection"
  - ![Check remember this selection](/images/containerization/gettingaccess4.png)
1. Click Log On
  - ![Log on](/images/containerization/gettingaccess5.png)
1. You will be greeted by a login page
  - ![Institution Login page](/images/containerization/gettingaccess6.png)
1. Enter your CSU-specific credentials
  - ![Institution credentials](/images/containerization/gettingaccess7.png)
1. Click Login
  - ![Institution Login](/images/containerization/gettingaccess8.png)
1. Complete any multi-factor authentication (if prompted)
  - ![DUO Push Notification](/images/containerization/gettingaccess9.png)
1. You should now be redirected to the NRP Portal and you should see your CSU email in the top right corner
  - Note: You may not see as many options in the top navigation, as the screenshot below is from an admin account
  - ![Signed in NRP Portal](/images/containerization/gettingaccess10.png)
1. Read and accept the Acceptable Use Policy (AUP) at the home page
  - Note: You'll need to re-accept this policy every 12 months to maintain your access
  - ![Nautilus Portal AUP](/images/containerization/gettingaccess14.png)
1. Verify that you've agreed to the AUP
  - ![Nautilus Portal AUP Accepted](/images/containerization/gettingaccess15.png)

Congratulations! You now have an NRP account and you're one step closer to running containers.

### NRP Portal tasks:
1. [Peruse the namespaces](https://portal.nrp-nautilus.io/namespaces-g)
    - It may be easiest to use your browser's find tool (ctrl + f / cmd + f)
    - Search for "tide-" to see TIDE related namespaces
        - ![TIDE Namepaces](/images/containerization/gettingaccess12.png)

1. [Download your kube config](https://portal.nrp-nautilus.io/authConfig)
    - You will need to sign in with CILogon again
    - The file should download and you should see the following screen
        - ![kube config file](/images/containerization/gettingaccess13.png)
    - Open your terminal and run the following command:
        - `mkdir ~/.kube`
    - Copy your config file into the ~/.kube folder (replace the path below for your config file):
        - `cp ~/Downloads/config ~/.kube/config`
    - Check to make sure your config file copied successfully:
        - `ls ~/.kube`

## Requesting Access to a Namespace
Now that you have signed into the NRP Portal, the TIDE team can add you to a namespace which will allow you to run containers on TIDE.

Submit the [TIDE Contact Us form](https://tide.sdsu.edu/contact/){:target="_blank"} ServiceNow form with the following:
1. Request Type should be "TIDE Containerization"
2. In the "Please provide a description of your request" indicate the name of the namespace you want created, or specify an existing one you wish to be added to
    - Note: All namespaces will be pre-fixed with "tide-"
3. Click Request

After you submit the form a member of the TIDE team will follow up and add you to the desired namespace.

## Next Steps
Once you have an NRP account and have been added to a namespace, continue to the [Quickstart](./quickstart) to run your first container on TIDE.