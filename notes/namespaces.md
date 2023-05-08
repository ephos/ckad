# Namespaces

## Overview

Containers for pods, replicasets, deployments.  Think of it as house for these.
For example, some applications (like Lacework) deploy to their own namespace.

You get some out of the box namespaces:

- kube-system (resources for the cluster operations)
- kube-public (resources available to all users in cluster)
- default

You can also assign quotas to namespaces.

Resources within a namespace refer to each other by simple name.
Resources can talk between namespaces but the namespace name must be appended to the resource.

- Within a namespace `mysql.connect("db-service")`
- Inter-namespace `mysql.connect("db-service.dev.svc.cluster.local")` 
  * Where 'dev' is the namespace name

The inter-namespace name is important since it maps to a DNS entry that gets created by k8s

The DNS structure is as follows:

- _service.namespace.svc.cluster.local_

## Declarative vs Imperative

You can do everything either way.  With that said it is nice to use imperative to create the delarative.


## Commands

```bash
# Imperative: You can pass a namespace with 
kubectl create namespace dev

# Delcarative:
kubectl create namespace dev --dry-run=client -o yaml > def-namespace-dev.yaml
kubectl create -f ./def-namespace-dev.yaml
```

Context matters, you can switch namespace context at any time.

```bash
# Set the context for the current cluster you are interacting with to namespace dev
kubectl config current-context
kubectl config set-context $(kubectl config current-context) --namespace dev
kubectl get pods

# Or use --all-namespaces
kubectl get pods --all-namespaces
```
