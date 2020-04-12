# Deployments

[Kubernetes Official Docs - Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

A Deployment provides declarative updates for pods and replicas. You only need to describe the desired state in a Deployment object, and it will change the actual state to the desired state. The Deployment object defines the following details:

The elements of a Replication Controller definition The strategy for transitioning between deployments To create a deployment for a nginx webserver, edit the nginx-deploy.yaml file as

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  generation: 1
  labels:
    run: nginx
  name: nginx
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      run: nginx
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx:latest
        imagePullPolicy: Always
        name: nginx
        ports:
        - containerPort: 80
          protocol: TCP
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      securityContext: {}
      terminationGracePeriodSeconds: 30
```

and create the deployment

```bash
kubectl create -f nginx-deploy.yaml
# deployment "nginx" created
```

The deployment creates the following objects

```bash
kubectl get all -l run=nginx
```

```bash
NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deploy/nginx   3         3         3            3           4m

NAME                 DESIRED   CURRENT   READY     AGE
rs/nginx-664452237   3         3         3         4m

NAME                       READY     STATUS    RESTARTS   AGE
po/nginx-664452237-h8dh0   1/1       Running   0          4m
po/nginx-664452237-ncsh1   1/1       Running   0          4m
po/nginx-664452237-vts63   1/1       Running   0          4m
```

## 

