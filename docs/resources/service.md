# Service

[Kubernetes Official Docs - Service](https://kubernetes.io/docs/concepts/services-networking/service/)

Kubernetes pods, as containers, are ephemeral. Replication Controllers create and destroy pods dynamically, e.g. when scaling up or down or when doing rolling updates. While each pod gets its own IP address, even those IP addresses cannot be relied upon to be stable over time. This leads to a problem: if some set of pods provides functionality to other pods inside the Kubernetes cluster, how do those pods find out and keep track of which other?

A Kubernetes Service is an abstraction which defines a logical set of pods and a policy by which to access them. The set of pods targeted by a Service is usually determined by a label selector. Kubernetes offers a simple Endpoints API that is updated whenever the set of pods in a service changes.

To create a service for our nginx webserver, edit the nginx-service.yaml file

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    run: nginx
spec:
  selector:
    run: nginx
  ports:
  - protocol: TCP
    port: 8000
    targetPort: 80
  type: ClusterIP
```

Create the service

`kubectl create -f nginx-service.yaml` service "nginx" created

```text
kubectl get service -l run=nginx
```

```text
NAME      CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
nginx     10.254.60.24   <none>        8000/TCP    38s
```

Describe the service:

```bash
kubectl describe service nginx
```

```text
Name:                   nginx
Namespace:              default
Labels:                 run=nginx
Selector:               run=nginx
Type:                   ClusterIP
IP:                     10.254.60.24
Port:                   <unset> 8000/TCP
Endpoints:              172.30.21.3:80,172.30.4.4:80,172.30.53.4:80
Session Affinity:       None
No events.
```

The above service is associated to our previous nginx pods. Pay attention to the service selector run=nginx field. It tells Kubernetes that all pods with the label run=nginx are associated to this service, and should have traffic distributed amongst them. In other words, the service provides an abstraction layer, and it is the input point to reach all of the associated pods.

