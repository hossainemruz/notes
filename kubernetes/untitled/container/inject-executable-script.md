# Inject Executable Script

## How to Inject an Executable Script into a Container?

Let's we want to execute a script inside a container. The script is not not a part of the docker image. We have to inject script then execute it.

**Solution:**

1. Create a configMap with the script.
2. Mount the configMap to the container as a volume with `defaultMode: 0744`.
3. Use `command:["/path/to/the/script.sh"]` to run the script.

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: ghost
  labels:
    role: blog
spec:
  replicas: 1
  template:
    metadata:
      labels:
        role: blog
    spec:
      containers:
      - name: ghost
        image: ghost:0.11-alpine
        command: ["/scripts/wrapper.sh"]
        ports:
        - name: ghost
          containerPort: 2368
          protocol: TCP
        volumeMounts:
        - name: wrapper
          mountPath: /scripts
      volumes:
      - name: wrapper
        configMap:
          name: wrapper
          defaultMode: 0744
```



