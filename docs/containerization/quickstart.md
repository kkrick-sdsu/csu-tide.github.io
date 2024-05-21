---
layout: default
title: Quickstart
parent: Containerization
nav_order: 1
has_children: false
description: ""
permalink: /containerization/quickstart
---

{: .note }
This page is currently under construction. Information will be updated soon.

# Prerequisites
This documentation assumes that you have:
- Completed the [Getting Access](./gettingaccess) walk-through
- Familiarity with the Linux terminal
- Familiarity with text editors (vi, emacs, etc.)

## Containers and Kubernetes
At a high-level, containers are packaged software applications containing all of their dependencies like the operating system (OS), file structures, configuration and other code libraries.

Containers offer many benefits but here we list a few of the most impactful ones:
1. Isolated Runtime Environments
    - Two or more containers running on the same system do not affect one another.
1. Portability
    - The same container can be run on a laptop, on the cloud, or on TIDE without being modified.
1. Consistency
    - The same container given the same input will produce the same output.

[Kubernetes](https://kubernetes.io/), often shortened to 'k8s', is a container orchestration platform for "automating deployment, scaling and management of containerized applications." If you are familiar with more traditional HPC systems, you can think of Kubernetes like a workload manager (i.e. Slurm). Similar to workload managers, Kubernetes allows us to make requests for resources like CPUs, GPUs and memory to run our programs. Kubernetes wraps a container in a [pod](https://kubernetes.io/docs/concepts/workloads/pods/), the smallest Kubernetes compute unit, which is then scheduled and run on the cluster.  It is important to note that pods are ephemeral and once a pod is deleted everything inside the pod is deleted -- meaning any data downloaded, content generated or files modified. Make sure to transfer data that you want to save out of the pod before deleting it.

## Running Containerized Software on TIDE


### Running The Test Repository
Follow these steps to get a copy of the repo cloned to your local machine and then examine the files to get familiar with them.

1. From your local machine, pull up a terminal window and run this command to get a copy of the repository:
    - `git clone https://github.com/csu-tide/hello-csu.git`
1. Change directory into the hello-csu directory and then list the files:
    - `cd hello-csu`
    - `ls -la`
1. View the contents of the `hello.py` file using `cat hello.py`; you should see the following:
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
    - This program is a simple variation on the typical Hello, World program with a bit of ascii art
1. Open the `Dockerfile` using `vi Dockerfile`; you should see the following:
    ```Dockerfile
    FROM python:3

    WORKDIR /usr/src/app

    COPY hello.py /usr/src/app/hello.py

    CMD ["python3", "hello.py"]
    ```
    - This is a [Dockerfile](https://docs.docker.com/engine/reference/builder/) which defines a container image
    - This container image is based on the Python 3 image and it copies our `hello.py` program into the container and then runs it
    - Type `:q` to exit the vi editor
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
        resources:  # NOTE: CPU/RAM limit may not exceed 1.2x (20%) of request size
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
            - 'key': 'nautilus.io/csu-tide'
                'operator': 'In'
                'values': ["true"]
    tolerations:
        - effect: NoSchedule
        key: nautilus.io/csu-tide
        operator: Exists
        - effect: NoSchedule
        key: nautilus.io/sdsu-fix
        operator: Exists
    ```
    - Kubernetes manifests are defined in the [YAML file format](https://en.wikipedia.org/wiki/YAML)
    - At first glance there is a lot going on in this file, but for now we will focus on these three things:
        1. `kind: Pod`
            - This specifies the kind of Kubernetes object, in this case a pod but we could also specify other [Workload Resources](https://kubernetes.io/docs/concepts/workloads/controllers/) like jobs or deployments
        1. `image: ghcr.io/csu-tide/hello-csu:main`
            - This is the container image which the container will be based on
        1. `command: ["sh", "-c", "sleep infinity"]`
            - This is the linux command, passed in as a string array, to be executed once the container is running inside the pod
            - Typically a pod will be deleted after its command(s) have finished executing, but in this case we have a never-ending command so that we have time to log into and examine the pod as it is running

Now that we have explored the files, let's talk about how this all comes together. First, we have the simple Python program `hello.py`, which we could execute on any machine with Python 3 installed. Then we take that Python program and put it into a container image with the `Dockerfile`, which is based on the [Python 3 image](https://hub.docker.com/_/python/) and thus has Python 3 pre-installed. At this point we can build the container image, or in this example use the [pre-built image](https://github.com/orgs/csu-tide/packages/container/package/hello-csu), and run a container on a container runtime like [Docker](https://www.docker.com/). Lastly we wrap this container in a Kubernetes pod in the `hello-pod.yaml`. At this point, we have everything we need in order to schedule this pod to the Kubernetes cluster. 

### Scheduling the Pod
Now that we have the files cloned and an understanding of what they do, let's schedule the pod on the Kubernetes cluster. Run the following commands in the terminal of your Kube Notebook:

1. First, define an environment variable for your namespace: 
    - `ns=[namespace]`
    - Note: Replace the namespace with yours and remove the brackets
1. Tell Kubernetes to schedule and run your pod definition:
    - `kubectl apply -f hello-pod.yaml -n $ns`
    - You should see ouput similar to this
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
    - Your pod is running once you see the READY column showing 1/1
    - Hit `ctrl` + `c` to stop watching the pods

At this point the pod is running our container and is executing the command specified in the pod YAML file: `sleep infinity`.

### Accessing the Pod
Now that the pod is running, let's get a bash shell on the container running in the pod:

1. Tell Kubernetes to launch an interactive shell session using the bash shell
    - `kubectl exec -it hello-pod -n $ns -- /bin/bash`
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

At this point we have run our Python program and created an output file in the container running in our pod on the Kubernetes cluster.

### Deleting the Pod
Now that we have run our program and generated some output, let's get our data and then delete the pod.

Follow these steps using the terminal in your Kube Notebook:
1. Copy the data from the container's working directory to your Kube Notebook:
    - `kubectl -n $ns cp hello-pod:/usr/src/app/hello.txt ./hello.txt`
    - Note: The kubectl cp command is intended for small file transfers
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
    No resources found in sdsu-joaquinmendoza namespace.
    ```

Now, as you might recall, we said that pods are ephemeral and everything in them gets deleted once the pod is deleted. Just to illustrate that point, let's schedule the pod again and attach a bash shell to it.

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
    - Note that only the `hello.py` program is listed and that our `hello.txt` file is gone. The `hello.py` program is there because the container copies the program each time it is launched, as we specified in the Dockerfile.
1. Exit the container
    - `exit`
1. Tell Kubernetes to delete the pod:
    - `kubectl -n $ns delete -f hello-pod.yaml`

Congratulations! You've run your first pod on TIDE, run a program within the container and you've gotten some data back out. With that, you have the basics to be able to run containers on TIDE.

## Next Steps
Try running your own software in this containerized approach by editing the YAML file.

Until then, you can also check out the [National Research Platform's documentation](https://docs.nationalresearchplatform.org/) which has some good examples.
