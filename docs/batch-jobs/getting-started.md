---
layout: default
title: Getting Started
parent: Batch Jobs
nav_order: 20
has_children: false
description: ""
permalink: /batch-jobs/getting-started
---

# Getting Started
This guide will walk you through a simple example of a batch job running on TIDE.
Once you have completed this you should be able to:
- Schedule a batch job
- Transfer small datasets in and out of the cluster
- Check the status of a batch job
- Access a remote terminal in a batch job

## Prerequisites
This documentation assumes that you have:
- Completed the [Getting Access](/batch-jobs/getting-access) guide
- Installed [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
- Familiarity with the Linux terminal
- A preferred text editor
- Exposure to [git source control](https://git-scm.com/)

## Running Your First Batch Job on TIDE

### Examining the Test Repository
Follow these steps to get a copy of the repository cloned to your local machine and then examine the files to get familiar with them.

1. From your local machine, pull up a terminal window and run this command to get a copy of the repository:
    - `git clone https://github.com/csu-tide/hello-csu.git`
    - *Note*: If you do not have git installed, you can [download this repository](https://github.com/csu-tide/hello-csu/archive/refs/heads/main.zip) and then un-zip it instead
1. Open the respository in your preferred text editor and inspect the files
1. Open the `hello.py` file; you should see the following:
    ```python
    csu = """


    ,----..    .--.--.
    /   /   \  /  /    '.          ,--,
    |   :     :|  :  /`. /        ,'_ /|
    .   |  ;. /;  |  |--`    .--. |  | :
    .   ; /--` |  :  ;_    ,'_ /| :  . |
    ;   | ;     \  \    `. |  ' | |  . .
    |   : |      `----.   \|  | ' |  | |
    .   | '___   __ \  \  |:  | | :  ' ;
    '   ; : .'| /  /`--'  /|  ; ' |  | '
    '   | '/  :'--'.     / :  | : ;  ; |
    |   :    /   `--'---'  '  :  `--'   \
    \   \ .'              :  ,      .-./
    `---`                 `--`----'

    """

    print(f"Hello there,\n{csu}")
    ```
    - This program is a simple variation on the typical "Hello, World" program with a bit of ASCII art
    - *Note*: It is better practice to keep your code outside of the container and transfer it in later, but we have simplified things for this example
1. Open the `Dockerfile`; you should see the following:
    ```Dockerfile
    FROM python:3

    WORKDIR /usr/src/app

    COPY hello.py /usr/src/app/hello.py

    CMD ["python3", "hello.py"]
    ```
    - This is a [Dockerfile](https://docs.docker.com/engine/reference/builder/) which defines a container image
    - This container image is based on the Python 3 image and it copies our `hello.py` program into the container and then runs it
    - For more information on creating a custom container image, see our [Container Creation](/container-creation) section
1. Open the Kubernetes manifest file `hello-pod.yaml`; you should see the following:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: hello-pod
    spec:
      containers:
      - name: hellopod
        image: ghcr.io/csu-tide/hello-csu:main
        resources:
          limits:
            memory: 100Mi
            cpu: 100m
          requests:
            memory: 100Mi
            cpu: 100m
        command: ["sh", "-c", "sleep infinity"]
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: 'nautilus.io/csu-tide'
                operator: Exists
      tolerations:
        - effect: NoSchedule
          key: nautilus.io/csu-tide
          operator: Exists
    ```
    - Kubernetes manifests are defined in the [YAML file format](https://en.wikipedia.org/wiki/YAML)
    - At first glance there is a lot going on in this file, but for now we will focus on these three things:
        1. `kind: Pod`
            - This specifies the kind of Kubernetes workload management object, in this case a simple pod
        1. `name: hello-pod`
            - This is the name of the pod and must be unique for each namespace
            - *Note*: If you are running this example in a shared namespace, you might consider editing the name with a short suffix like your `-[your-initials]` for example: `name: hello-pod-kk`
        1. `image: ghcr.io/csu-tide/hello-csu:main`
            - This is the container image which the container will be based on
            - The cluster will automatically download and run this container image for you
        1. `command: ["sh", "-c", "sleep infinity"]`
            - This is the linux command, passed in as a string list, to be executed once the container is running inside the pod
            - Typically a pod will be deleted after its command(s) have finished executing, but in this case we have a never-ending command so that we have time to log into and examine the pod as it is running

Now that we have examined the files, let's talk about how this all comes together.
First, we have the simple Python program `hello.py`, which we could execute on any machine with Python 3 installed. 
Then we take that Python program and put it into a container image with the `Dockerfile`, which is based on the [Python 3 image](https://hub.docker.com/_/python/) and thus has Python 3 pre-installed. 
At this point we can build the container image or, as in this example, use the [pre-built image](https://github.com/orgs/csu-tide/packages/container/package/hello-csu). 
Lastly, we wrap this container in a Kubernetes pod in the `hello-pod.yaml`. 

At this point, we have everything we need in order to schedule this pod on the TIDE cluster. 

### Scheduling the Batch Job
Now that we have the files cloned and an understanding of what they do, let's schedule the batch job on the TIDE cluster. 
Run the following commands in your terminal:

1. First, define an environment variable for your namespace: 
    - `ns=[namespace]`
    - Note: Replace the namespace with yours and remove the brackets
1. Tell Kubernetes to schedule and run your pod definition:
    - `kubectl apply -f hello-pod.yaml -n $ns`
    - You should see ouput similar to this:
    ```bash
    pod/hello-pod created
    ```
1. Check to see if your pod has been scheduled:
    - `kubectl get pods -n $ns --watch`
    - You may see output like this:
    ```bash
    NAME        READY   STATUS    RESTARTS   AGE
    hello-pod   0/1     Pending   0          12s
    hello-pod   1/1     Running   0          32s
    ```
    - Your pod is running once you see the READY column showing 1/1 and STATUS column showing "Running"
    - Hit `ctrl` + `c` to stop watching the pods

At this point the pod is running our container and is executing the command specified in the pod YAML file: `sleep infinity`.

### Accessing the Batch Job
Now that the pod is running, let's get a remote bash shell on the container running in the pod:

1. Tell Kubernetes to launch an interactive shell session using the bash shell
    - `kubectl exec -it hello-pod -n $ns -- /bin/bash`
    - *Note*: If you modified the pod name in `hello-pod.yaml`, please update the command above so that `hello-pod` matches the new name
    - You should see output similar to the following:
    ```bash
    root@hello-pod:/usr/src/app#
    ```
1. Print the working directory
    - `pwd`
    - You should see the following:
    ```bash
    /usr/src/app
    ```
    - The reason we are at this directory is because the container image's Dockerfile specified the working directory with this line
    ```Dockerfile
    WORKDIR /usr/src/app
    ```
1. List the files in the working directory:
    - `ls -la`
    - You should see the following:
    ```bash
    total 4
    drwxr-xr-x 1 root root  22 Jul 11 19:45 .
    drwxr-xr-x 1 root root  17 Jul 11 19:45 ..
    -rw-r--r-- 1 root root 348 Jul 11 19:45 hello.py
    ```
1. Now let's execute our Python program:
    - `python hello.py`
    - You should see the following:

    ```
    Hello there,



    ,----..    .--.--.
    /   /   \  /  /    '.          ,--,
    |   :     :|  :  /`. /        ,'_ /|
    .   |  ;. /;  |  |--`    .--. |  | :
    .   ; /--` |  :  ;_    ,'_ /| :  . |
    ;   | ;     \  \    `. |  ' | |  . .
    |   : |      `----.   \|  | ' |  | |
    .   | '___   __ \  \  |:  | | :  ' ;
    '   ; : .'| /  /`--'  /|  ; ' |  | '
    '   | '/  :'--'.     / :  | : ;  ; |
    |   :    /   `--'---'  '  :  `--'   \
    \   \ .'              :  ,      .-./
    `---`                 `--`----'
    ```
1. Let's run the Python program again, but direct the output to a file:
    - `python hello.py > hello.txt`
1. Verify the file was created:
    - `ls -la`
    - You should see the file hello.txt:
    ```bash
    total 8
    drwxr-xr-x 1 root root  23 Jul 13 18:22 .
    drwxr-xr-x 1 root root  17 Jul 11 19:45 ..
    -rw-r--r-- 1 root root 348 Jul 11 19:45 hello.py
    -rw-r--r-- 1 root root 317 Jul 13 18:22 hello.txt
    ```
1. Exit the bash shell from the container:
    - `exit`

At this point we have run our Python program and created an output file in the container running in our pod on the TIDE cluster.

### Deleting the Batch Job
Now that we have run our program and generated some output, let's get our data and then delete the pod.

Follow these steps using the terminal in your Kube Notebook:
1. Copy the data from the container's working directory to your Kube Notebook:
    - `kubectl -n $ns cp hello-pod:/usr/src/app/hello.txt ./hello.txt`
    - *Note*: If you modified the pod name in `hello-pod.yaml`, please update the command above so that `hello-pod` matches the new name
    - *Note*: The kubectl cp command is intended for small file transfers, please reference our [storage services](/storage-services) for larger transfers (I.E. > 5MB)
1. Check your local directory for the hello.txt file:
    - `ls`
    - You should see the file listed:
    ```bash
    Dockerfile  README.md  hello-pod.yaml  hello.py  hello.txt
    ```
1. Now, tell Kubernetes to delete the pod:
    - `kubectl -n $ns delete -f hello-pod.yaml`
    - You should see the following:
    ```bash
    pod "hello-pod" deleted
    ```
1. Check to make sure the pod was deleted:
    - `kubectl -n $ns get pods`
    - You should see something similar to the following:
    ```
    No resources found in csu-tide namespace.
    ```

Now, as you might recall, [we said that pods are ephemeral](/batch-jobs/#pods) and everything in them gets deleted once the pod is deleted.
Just to illustrate that point, let's schedule the pod again and attach a bash shell to it.

1. `kubectl -n $ns apply -f hello-pod.yaml`
1. `kubectl -n $ns exec -it hello-pod -- /bin/bash`
1. Now, check for the hello.txt file:
    - `ls -la`
    ```bash
    total 4
    drwxr-xr-x 1 root root  22 Jul 11 19:45 .
    drwxr-xr-x 1 root root  17 Jul 11 19:45 ..
    -rw-r--r-- 1 root root 348 Jul 11 19:45 hello.py
    ```
    - Note that only the `hello.py` program is listed and that our `hello.txt` file is gone. The `hello.py` program is there because it is part of the container, as we specified in the Dockerfile.
1. Exit the container
    - `exit`
1. Tell Kubernetes to delete the pod:
    - `kubectl -n $ns delete -f hello-pod.yaml`
    - *Note*: Although there is a [maximum runtime](/batch-jobs/#nrp-specific-information) for pods, it is always best to delete a batch job when you are done

Congratulations! You've run your first batch job on TIDE as a pod, run a program within the container and you've gotten some data back out.
With that, you have the basics to be able to run batch jobs on TIDE.

## Next Steps


You can also check out the [National Research Platform's documentation](https://docs.nationalresearchplatform.org/) which has some good examples.
