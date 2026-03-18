## Force Kubernetes to re-pull a container image

When a deployment uses a mutable tag (like `staging` or `latest`) and the
underlying image has been updated in the registry, the running pods won't
automatically pick up the new image. To force a re-pull:

```bash
kubectl -n <namespace> rollout restart deployment <deployment-name>
```

This triggers a rolling update that creates new pods. Combined with
`imagePullPolicy: Always` (the default for `latest` tags, but must be set
explicitly for other tags), Kubernetes will pull the image fresh from the
registry.

### Verify

```bash
# Watch the rollout
kubectl -n <namespace> rollout status deployment <deployment-name>

# Or watch pods directly
kubectl -n <namespace> get pods -w
```

### Gotcha

If `imagePullPolicy` is set to `IfNotPresent`, the restart won't help — the
node already has the image cached. Check with:

```bash
kubectl -n <namespace> get deployment <name> -o jsonpath='{.spec.template.spec.containers[0].imagePullPolicy}'
```
