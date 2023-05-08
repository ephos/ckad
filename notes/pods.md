# Pods 

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

## Pod + Service

You can create a pod with a service by using `--expose --port 80`.
In that example it will expose the pod on port 80, it will create a ClusterIP service to do this.

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
