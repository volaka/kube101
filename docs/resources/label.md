# Label

[Kubernetes Official Docs - Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

In Kubernetes, labels are a system to organize objects into groups. Labels are key-value pairs that are attached to each object. Label selectors can be passed along with a request to the apiserver to retrieve a list of objects which match that label selector.

To add a label to a pod, add a labels section under metadata in the pod definition:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
...
```

To label a running pod

```bash
kubectl label pod mynginx type=webserver
# pod "mynginx" labeled
```

To list pods based on labels

```text
kubectl get pods -l type=webserver
```

```text
NAME      READY     STATUS    RESTARTS   AGE
mynginx   1/1       Running   0          21m
```

## 

