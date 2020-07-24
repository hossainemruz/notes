# Log

### See log of the pods that match a pattern

```bash
kubectl logs -n <namespace> $(kubectl get pod -n <namespace> |  awk '/<pattern>/{print $1}') -f
```

**Example:**

```bash
kubectl logs -n kube-system $(kubectl get pod -n kube-system |  awk '/kube-proxy*/{print $1}') -f
```

### See logs of all container in a pod

Use `--all-containers` flag.

```bash
kubectl logs my-pod --all-containers
```

### See logs of a container that has crashed

Use `--previous` \(short `-p`\) flag.

```bash
kubectl logs my-pod --previous
```





