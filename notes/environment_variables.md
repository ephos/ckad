# Environment Variables

You can set environment variables in Kubernetes. These can be the following:

- Plain Key Values

```yaml
env:
  - name: APP_COLOR
    value: purple
```

- ConfigMaps

```yaml
env:
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef:
```

- Secrets

```yaml
env:
  - name: APP_COLOR
    valueFrom:
      secretKeyRef:
```
