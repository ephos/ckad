# Security Context

You can set a user for the container as we saw in the security notes.

Since containers are encapsulated in Pods, you can set the security context at either.

You can set the security at:

- Container level
  * If defined here AND Pod, then settings here take precedence 
- Pod Level
  * If done here the settings carry to all the containers

Example setting the security context for the pod:

---

Setting it at the Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-app-pod
spec:
  securityContext:
  runAsUser: 1000

  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
```

Setting it at the container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-app-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities:
          add: ["MAC_ADMIN"]
```

---

An interactive way to check the user/context a pod is running as can be done the following way:

```bash
kubectl exec my-pod-name -- whoami
```
