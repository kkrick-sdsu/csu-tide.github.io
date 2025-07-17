---
layout: default
title: Common Tasks and Troubleshooting
parent: Batch Jobs
nav_order: 50
has_children: false
description: ""
permalink: /batch-jobs/tasks-troubleshooting
---

# Common Tasks and Troubleshooting
Kubernetes is a robust and powerful system, but with that system comes a verbose vernacular.
Likewise, the Kubectl command has many capabilities for interacting with a Kubernetes system. 
In response to that, this article subscribes to the Pareto Principle, also known as the "80-20 rule," and provides the 20% of commands that should cover 80% of the work you will need to do with Kubectl.

We also have some conventions that we use in this article such as:
- Code blocks
    - These should allow you to hover over them and reveal a clipboard copy icon to the right
    - Example of a code block:
```bash
echo hello
```
- Use of square `[]` brackets in commands
    - These, and their contents, should be replaced unless otherwise noted
- Use of `resource-type`
    - In the context of this article, resource-type could take on the values of: `pod`, `deployment` or `job`

## Common Tasks
Here is a short list of the common tasks that you might need to do when working with batch jobs and the Kubectl commands to accomplish them.
For more detailed information and examples, please reference the official [Kubectl Quick Reference](https://kubernetes.io/docs/reference/kubectl/quick-reference/){:target="_blank"}.
- *Note*: Not all users will have access to all commands and resource types on the TIDE cluster due to security reasons

### Setting Namespace
If you have access to more than one namespace, then you may need to switch between them periodically.
Instead of supplying the namespace with the `-n` flag for each command, you can set the namespace one time for all subsequent commands and terminal sessions until you re-run this command:
```bash
kubectl config set-context nautilus --namespace=[your-namespace-here]
```
- Example command: `kubectl config set-context nautilus --namespace=csu-example`
- Example output: `Context "nautilus" modified.`

### Targeting TIDE
TIDE hardware is both labeled and tainted to provide a way to target the hardware and reserve portions for CSU exclusive use.
In Kubernetes a "taint" is a way to prevent a job from scheduling on a node unless the job specifically "tolerates" the taint on the node.
You will need to add the following YAML to the `spec` of pods, deployments and jobs to target TIDE-labeled hardware and tolerate the TIDE taint.

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: nautilus.io/csu-tide
            operator: Exists
  tolerations:
  - key: nautilus.io/reservation
    operator: Equal
    value: csu-tide
    effect: NoSchedule
```

### Scheduling Batch Jobs
At the start of a workflow, you will want to schedule batch jobs onto the cluster using a Kubernetes YAML file that defines your batch job:
```bash
kubectl apply -f [file-name].[yaml/yml]
```
- Example command: `kubectl apply -f hello-pod.yaml`
- Example output: `pod/hello-pod created`

### Checking Batch Jobs
After you have scheduled a batch job, you will want to check to make sure it is showing a ready value of `1/1` and status value of `Running`:
```bash
kubectl get [resource-type]
```
- Example command: `kubectl get pods`
- Example output with no pods running: `No resources found in csu-example namespace.`
- Example output with pods running:
```bash
NAME        READY   STATUS    RESTARTS   AGE
hello-pod   1/1     Running   0          74s
```

Sometimes you may want to watch in real-time to make sure that your batch jobs get scheduled or deleted.
Add `--watch` to see real-time updates on the status of batch jobs.
When you want to exit the watch command, hit `ctrl + c`.
```bash
kubectl get [resource-type] --watch
```
- Example command: `kubectl get pods --watch`
- Example output with no pods in the namespace: `[blank]` 
- Example output with pods scheduling:
```bash
NAME        READY   STATUS    RESTARTS   AGE
hello-pod   0/1     Pending   0          0s
hello-pod   0/1     Pending   0          0s
hello-pod   0/1     ContainerCreating   0          0s
hello-pod   0/1     ContainerCreating   0          0s
hello-pod   1/1     Running             0          1s
```
- Example output with pods deleting:
```bash
NAME        READY   STATUS    RESTARTS   AGE
hello-pod   1/1     Running   0          68s
hello-pod   1/1     Terminating   0          74s
hello-pod   1/1     Terminating   0          104s
hello-pod   0/1     Terminating   0          105s
hello-pod   0/1     Terminating   0          105s
hello-pod   0/1     Terminating   0          105s
hello-pod   0/1     Terminating   0          105s
```

### Remoting into Batch Jobs
You can pull up a remote terminal session inside of a pod to execute linux commands:
```bash
kubectl exec -it [pod-name] -- [linux-command]
```
- Example command: `kubectl exec -it hello-pod -- bash`
- Example output: `root@hello-pod:/usr/src/app#`
- *Note*: The shell may change depending on the OS of the container you are running, but generally bash is very common

### Port-forwarding into Batch Jobs
You can forward a local port on your machine into a pod running on the cluster to remotely access the port or service running on the port:
- *Note*: This is a more advanced use case, but can be useful for running interactive batch jobs like Jupyter Lab.
```bash
kubectl port-forward [pod-name] [local-port]:[remote-port]
```
- Example command: `kubectl port-forward jupyter-kkrick-40sdsu-2eedu 8888:8888`
- Example output:
```bash
Forwarding from 127.0.0.1:8888 -> 8888
Forwarding from [::1]:8888 -> 8888
```

### Cleaning up Batch Jobs
It is best to manually clean up batch jobs when you have finished with them as opposed to relying on an automatic shutdown.
In Kubectl we clean up resources using the `delete` keyword.
Remember, once a batch job is deleted, so is any leftover data -- make sure to transfer important data out of batch jobs before deleting them!

You can clean up batch jobs with the file that was used to schedule them:
```bash
kubectl delete -f [file-name].[yaml/yml]
```
- Example command: `kubectl delete -f hello-pod.yaml`
- Example output: `pod "hello-pod" deleted`

You can also clean up batch jobs with the resource type and the value of the name column:
```bash
kubectl delete [resource-type] [resource-name]
```
- Example command: `kubectl delete pod hello-pod`
- Example output: `pod "hello-pod" deleted`


## Troubleshooting
Sometimes you may encounter an issue with a batch job.
Issues may arise during scheduling, running or cleaning up batch jobs.
This section offers some commands that may help you identify the various issues you may encounter.
- *Note*: When contacting the TIDE Support team for batch job assistance, we may ask you for output from one or more of these commands.

### Typical Issues
Below is a non-exhuastive list of issues that you may encounter:

- Syntax errors
    - Generally, Kubectl will warn you if you have a syntax issue in your YAML files
- FailedAttachVolume
    - Your persistent storage from a PersistentVolumeClaim could not attach to your pod for one of several reasons (I.E. already attached to another pod with read-write once mode)
- FailedScheduling
    - The specified resources may not be available (I.E. trying to schedule 4 NVIDIA A100 GPUs)
    - The specified tolerations may be incorrect

### Checking Description for Events
Kubernetes will log events on pods which can be very useful for describing the health of the pod.
You may see a wide range of output, but we are most interested in the `Events` section at the bottom of the output from this command.
- *Note*: If you are using deployments or jobs, it is best to describe the individual pods in your batch job.
```bash
kubectl describe pod [pod-name]
```
- Example command: `kubectl describe pod hello-pod`
- Example output of healthy pod:
```bash
[truncated]
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  9m     default-scheduler  Successfully assigned sdsu-kylekrick/hello-pod to rci-tide-gpu-14.sdsu.edu
  Normal  Pulled     8m59s  kubelet            Container image "ghcr.io/csu-tide/hello-csu:main" already present on machine
  Normal  Created    8m59s  kubelet            Created container hellopod
  Normal  Started    8m59s  kubelet            Started container hellopod
```
- Example output of unhealthy pod:
```bash
Events:
  Type     Reason              Age   From                     Message
  ----     ------              ----  ----                     -------
  Normal   Scheduled           27s   default-scheduler        Successfully assigned sdsu-rci-jh/volume-checker to rci-nrp-dtn-01.sdsu.edu
  Warning  FailedAttachVolume  27s   attachdetach-controller  Multi-Attach error for volume "pvc-5c17755d-5b40-489b-85ff-c296e269f4da" Volume is already used by pod(s) jupyter-kkrick-40sdsu-2eedu
```

### Checking Logs
Pods may be configured to log their output to linux standard output which can be read with the `logs` command:
```bash
kubectl logs [pod-name]
```
- Example command: `kubectl logs hello-pod`
- Example output with no logs: `[blank]`

If your pod contains more than one container, then this will default to the first container in the YAML file, but you can specify a container name with the optional `-c` flag:
```bash
kubectl logs [pod-name] -c [container-name]
```
- Example command: `kubectl logs ollama-84b4ccb96c-lfbx2 -c pod-ollama`
- Example output with logs:
```
[truncated]
ollama.service: [GIN] 2024/10/21 - 16:30:53 | 200 |  173.485485ms |   10.244.150.39 | POST     "/v1/chat/completions"
ollama.service: [GIN] 2024/10/21 - 16:30:55 | 200 |   135.23678ms |   10.244.150.39 | POST     "/v1/chat/completions"
ollama.service: [GIN] 2024/10/21 - 16:30:55 | 200 |  241.210909ms |   10.244.150.39 | POST     "/v1/chat/completions"
ollama.service: [GIN] 2024/10/21 - 16:30:55 | 200 |  140.942317ms |   10.244.150.39 | POST     "/v1/chat/completions"
ollama.service: [GIN] 2024/10/21 - 16:30:55 | 200 |  174.779168ms |   10.244.150.39 | POST     "/v1/chat/completions"
```
