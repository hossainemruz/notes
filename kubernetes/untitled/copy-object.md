# Copy Object

## Copy Object between two namespaces

### Copy Secret between two namespaces

```bash
kubectl get secret stash-apiserver-cert --namespace=kube-system -oyaml | grep -v '^\s*namespace:\s' | kubectl apply --namespace=monitoring -f -
```

