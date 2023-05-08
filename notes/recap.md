# Recap

High level recap of Kubernetes, these notes probably exist elsewhere.
This could be a duplicate but I 

High level objects:

- Cluster: A set of nodes
- Nodes _(formerly minions)_: Runs applications
- Master: Controlls and orchestrates containers on worker nodes 

Components of Kubernetes:

- etcd: Keystore for all data used to manage the cluster
- API Server: Frontend for Kubernetes, everything management plane
- Kubelet: Node agent, this is what runs on the workers
- Container runtime: Underlying software for running software (docker/cri-o)
- Controller: Brain behind the orchestrators, responds to events
- Scheduler: Distributes work/containers across multiple nodes

Master (kube-apiserver) <->  Node (kube-kubelet)

What is on the Master:

- API Server
- etcd
- controller
- scheduler

What is on the Worker:

- Kubelet
- Conteiner Runtime

Management: 

- `kubectl` 


## High Level Commands

```bash
# Run an application
kubectl run hello-minikube

# Get cluster info
kubectl cluster-info

# Get cluster nodes
kubectl get nodes
```

## Pods 

The smallest object you can create.  Containers are encapsulated in a POD.
When scalling up, you create a new POD.
That said, PODs can contain more than 1 container if designed that way.
An example of a POD with multiple containers would be if you have a helper container.
Generally think of it as Application Component == POD.  

Deploying a POD

```bash
# Create a POD called nginx with the nginx image
kubectl run nginx --image nginx

# View the created POD
kubectl get pods
```

## YAML

Creating a POD with a YAML based definition file.

Kubernetes uses YAML as inputs for creation of objects within the platform.
All of these have the same structure at the top:

```yaml
apiVersion:
kind:
metadata:

spec:
```

What this would look like for a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp

spec:
  containers:
    - name: nginx-container
      image: nginx
```

Within `metadata:` you always need `name:` and `labels:`.  
However within the `labels:` dictionary you can add **ANY** key-value pair.

You can apply the definition now:

```bash
# Deploy the pod definition
kubectl create -f pod-definition.yml
```

## Replica Sets and Replication Controllers

#### OLD: Replication Controller

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

#### NEW: Replica Set

    This is a newer and recommended way.


