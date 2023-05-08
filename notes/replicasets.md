# Replica Sets and Replication Controllers

## Replication Controller

**DEPRICATED**

This is what allows multiple pods to run across multiple pods.

This is facilitated by the replication controller on the node.
The replication controller can run multiple instances of the pod.
It maintains how many pods should be running and where.

If you have a pod servicing users and it gets too busy, it can scale on the same node.
If the nodes resources start to get taxed it can replicate over multiple nodes.

What you'll notice is that the Pod section from the Pod manifest ends up in the template.

```yaml
apiVersion: v1
kind: ReplicationController
metadata: 
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        costcenter: 238
        location: us-east
        type: frontend
    spec:
      containers:
        - name: frontend-container
          image: nginx
  replicas: 3
```

## Replica Set

This is a newer and recommended way.  Replication Controller is out of date.

Must define it with `apiVersion:apps/v1`
**Requires** the use of a `selector:`

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: frontend
spec:
  template:
    metadata:
      name: myapp-prod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-controller
          image: nginx
  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```

Labels and selectors explained.  You can use a replica set to even monitor existing pods.
So if you already have an nginx pod deployed the replica set can filter to match it by label.

You can write a replica set to create and manage all pods with label `tier: front-end`.
You need to have the template for this reason, that way K8s knows how to scale in the future.

Like anything else you can update the replica set in 2 ways:

1. Update the `replicas: 6` in the definition 
  a. Then run `kubectl replace -f rc-def.yml`
2. `kubectl scale --replicas=6` -f rc-def.yml
  a. Alternatively specify by name : `kubeclt scale replicas=6 replicaset ReplicaSetName`

---

## Commands

```bash
# Create replica set from definition
kubectl create -f replicaset-def.yml

# Get the replicaset
kubectl get replicaset

# Delete the replicaset (Also deletes underlying pods)
kubectl delete replicaset MyReplicaSetName

# Update replica set (after updating definition file)
kubectl replace replicaset -f my-replicaset-def.yml

# Scale the replicaset alternate way (can also use replicaset ReplicaSetName)
kubectl scale -replicas=6 -f my-replicaset.yml
```
