# Deployments

Scenario:

1. **Multi Instance Deployment (FT):** You need multiple instances of a webserver
2. **Version Updates/Rolling Updates:** You need to upgrade them from v1.0.0 to v2.0.0 seamlessly, but not always all at the same time
3. **Rollback:** You need to be able to rollback the changes
4. **Pausing/Resuming:** To deploy the changes all at once when needed, like webserver version

Replica sets are contained within a Deployment!

Deployment definition file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels: 
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  slector:
    matchLabels:
      type: front-end
```
