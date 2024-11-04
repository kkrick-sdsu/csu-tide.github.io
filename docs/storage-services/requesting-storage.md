---
layout: default
title: Requesting Storage
parent: Storage Services
nav_order: 10
has_children: false
description: ""
permalink: /storage-services/requesting-storage
---

# Requesting Storage
TIDE object, block and file storage is managed by the TIDE Support team and must be requested.

If you are not sure which storage type to request, please use the [storage type table](/storage-services/#comparing-storage-types) as a reference.
You may also want to look at the [Using Storage](/storage-services/using-storage) guide before requesting storage.

Please follow these steps to request new storage or modification to existing storage:
1. Navigate to the [TIDE Support Request](https://tide.sdsu.edu/tide-support-request/){:target="_blank"}
1. You should be greeted with the following form:
![tide support request form](/images/storage-services/requesting-storage1.png)
1. Complete the fields "Name", "Email", and "Campus" as appropriate
1. For the "Request Type" field, please select "Other"
1. In the "Additional Details" field, please indicate that you are requesting storage be provisioned or expanded and include an estimate of how much total storage you need in terms of gigabytes (GB) or terabytes (TB) and for how long you expect to use it
1. Please also include information relating to the specific storage type:
    - Object storage: 
        1. Bucket name
            - *Note*: It is best to keep this short, yet meaningful
            - *Note*: We will prefix your bucket name to reflect your campus i.e "sdsu-" for San Diego State
        2. Names and emails of collaborators who should be given access
    - Block and File storage:
        1. Namespace name
        1. Name of the PersistentVolumeClaim
1. Check the box for the reCAPTCHA
1. Click the "Submit" button

You should receive an email that your request has been opened.
You should receive a response from the TIDE Support team within 2 business days.

## Next Steps
After you have been provisioned with your chosen storage option, we recommend that you check out the [Using Storage](/storage-services/using-storage) guide.

You can also check out our [Managing Storage](/storage-services/managing-storage) guide.
