# Command

Cheat sheet of commands

## Pods 

Viewing Pods and Pod Details.

```bash
# Get the pods
kubectl get pods

# Get a specific pod, in this case 'my-app'
kubectl get pods my-app

# Desribe all pods
kubectl describe pod

# Describe a single pods
kubectl describe pod my-app

# Describe a single pod from a specific namespace
kubectl describe pod lacework-agent-zzjhd -n lacework
```

Editing Pods.

*CKAD Note:*
In CKAD if you are given a pod definition edit that to update the pod.
If you are ***NOT*** given a pod definition file you will need to output one.


```bash
# When not given a pod definition file you can output one using the syntax below
# This will output the yaml for the 'my-app' pod to a file called pod-def.yaml
kubectl get pod my-app -o yaml > pod-def.yaml

# Edit the file and then run the edit command
kubectl edit pod my-app
```

---

## Replica Sets


