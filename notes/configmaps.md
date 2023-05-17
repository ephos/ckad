# ConfigMaps

This makes managing Pod configurations much easier.
Key Value pairs that can be passed into the pod.

1. Create the ConfigMap
2. Inject it into the Pod

You can create a ConfigMap declaratively and imperatively.

- Imperative:

```bash
kubectl create configmap <name> --from-literal=<key>=<value>
```

- Declarative:

```bash
# Creating a config map, it is assumed you already have one but run this if you don't
kubectl create configmap MyConfigMap-App1 --from-literal=APP_COLOR=blue --dry-run=client -o yaml > app1-configmap-def.yaml

kubectl create -f app1-configmap-def.yaml
```

Or with the following schema if writing from scratch:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: MyRedisConfig
data:
  port: 6379
  rdb-compression: yes
```

## Working Example for the Color App

_pod-definition.yml_

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    envFrom:
      - configMapRef:
          name: app-config
```

_config-map.yml_

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_ENV: prod
```
---

## Additional Notes

If you need to create multiple mappings you can do the following:

- Specify `--from-literal` as many times as you need to create a mapping.
- Pass in the key-value pairs from another file with `--from-file`
  * `kubectl create configmap MyCfgMap --from-file app_config.properties`

Config maps can also come from a volume.

## Map One Value vs the Whole ConfigMap

This can be tricky.  Say for example you want to change the APP_COLOR env var.
This can technically be done 2 ways, you can map that one value to the ConfigMap or you can map the whole config map.

The whole ConfigMap:

```yaml
spec:
  containers:
  - envFrom:
    - configMapRef:
        name: webapp-config-map
```

Just pulling one value from the map:

```yaml
spec:
  containers:
  - env:
    - name: APP_COLOR
      valueFrom:
        configMapKeyRef:
          name: webapp-config-map
          key: APP_COLOR
```

## Commands

```bash
# See which ones exist
kubectl get configmaps

# See the details
kubectl describe configmaps
```
