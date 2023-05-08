# Notes

General notes and must know things!

## Imperative vs Declarative

Within `kubectl` you can issue imperative commands (like scaling a replica set or creating a deployment set).

**IMPORTANT**:  When you issue an imperative command you can use `--dry-run=client -o yaml` to see the declarative code!

This is cool, say you need to scaffold a deployment and don't have any existing yaml to work with.  Do the following:

```bash
kubectl create deployment httpd-frontend --image httpd:2.4-alpine --replicas 3 --dry-run=client -o yaml
```

Presto!  You now have the yaml to build it.
