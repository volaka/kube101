# Pod

[Kubernetes Official Docs - Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)

In Kubernetes, a group of one or more containers is called a pod. Containers in a pod are deployed together, and are started, stopped, and replicated as a group. The simplest pod definition describes the deployment of a single container. For example, an nginx web server pod might be defined as such:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mynginx
  namespace: default
  labels:
    run: nginx
spec:
  containers:
  - name: mynginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

## 

