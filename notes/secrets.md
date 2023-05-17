# Secrets

- Applications running in containers can have hardcoded values, not good.
- ConfigMaps can also store these values and inject them into the app, but it's plain text, not good.

Enter secrets!

Secrets are like ConfigMaps except they are encoded.  They are also used like ConfigMaps:

1. Create the Secret
2. Inject it into the Pod

Things to know:

- ❗By the way the defaults here for encoding is Base64 encoded strings so, um, not great.
- ❗This should be beyond obvious but don't check in secrets in source control
- ❗Secrets are NOT encrypted in etcd, so um, you might want to turn that on
  * ✅You can use an EncryptionConfiguration object to do this potentially
- ❗Any secrets in a namespace can be viewed by all who have access to that namespace
- ✅ Consider using an external provider for secrets like AWS or HashiCorp Vault

Secrets are only sent to a node if a pod on that node requires it.
They are stored by the **kubelet** in tempfs so they are never written to disk
When a pod with a secret leaves a node, the secret is removed from that node.


## Create a Secret

### Imperative

```bash
# Imperative creation of a secret
kubectl create secret generic secretName --from-literal=DB_Host=mysql

# You can provided multiple key/values
kubectl create secret generic mySecret2 --from-literal=Name=Bobby --from-literal=Secret=peanutbutter

# Like a ConfigMap you can also pass in from a file
kubectl create secret generic supahSecret --from-file=~/secretz.json

# Imperative to make declarative!
kubectl create secret generic mysecret --from-literal=Name=Bobby --from-literal=Secret=peanutbutter --dry-run=client -o yaml > secrets.yml
kubectl create -f ./secrets.yml
```

### Declarative

The values **NEED** to be Base64 encoded.  You can do that the following way:

`echo -n "bobby" | base64`

```yml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: bXlzcWwK 
  DB_User: Ym9iYnkK 
  DB_PW: c2hoZG9udHRlbGxhbnlvbmUK
```

You can then run `kubectl create -f app-secret.yml`

## Using Secrets with Pods

Given this Pod definition (poddefinition.yml):

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
    - secretRef:
       name: app-secret
```

And this Secret definition (app-secret.yml):

```yml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: bXlzcWwK 
  DB_User: Ym9iYnkK 
  DB_PW: c2hoZG9udHRlbGxhbnlvbmUK
```

Then run:

```bash
kubectl create -f poddefinition.yml
```

You are also able to mount secrets from volumes.

## Commands

```bash
# Get the secrets
kubectl get secrets

# Show more detail
kubectl describe secrets

# If you want to see the values
kubectl get secret app-secret -o yaml
# The yaml will show the base64 encoded values
echo 'c2hoZG9udHRlbGxhbnlvbmUK' | base64 -d
```

