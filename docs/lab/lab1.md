---
description: >-
  Learn how to deploy an application to a Kubernetes cluster hosted within the
  IBM Container Service.
---

# Set up and deploy your first application

## 0. Install Prerequisite CLIs and Provision a Kubernetes Cluster

If you haven't already: 

1. Install the IBM Cloud CLIs and login, as described in [Lab 0](https://github.com/volaka/kube101/tree/bf470977788909d418ca40830f565648a89dc640/Lab0/README.md). 
2. Provision a cluster:

`$ ibmcloud cs cluster-create --name <name-of-cluster>`

Once the cluster is provisioned, the kubernetes client CLI `kubectl` needs to be configured to talk to the provisioned cluster.

1. Run `$ ibmcloud cs cluster-config <name-of-cluster>`, and set the `KUBECONFIG`

   environment variable based on the output of the command. This will

   make your `kubectl` client point to your new Kubernetes cluster.

Once your client is configured, you are ready to deploy your first application, `guestbook`.

## Claim Kubernetes Cluster in Workshops

{% page-ref page="../additional-info/get-precreated-kubernetes-cluster.md" %}

{% hint style="warning" %}
### You have to connect to your kubernetes cluster before next step.
{% endhint %}

```bash
docker run --it -v /var/run/docker.sock:/var/run/docker.sock volaka/ibmcloud-devtools
cd
ibmcloud login
```

{% hint style="info" %}
Don't forget to select `IBM (e2b54d0c3bbe4180b1ee63a0e2a7aba4) <-> 184086`
{% endhint %}

```bash
git clone https://github.com/IBM/guestbook.git
cd guestbook
```

## 1. Deploy your application

In this part of the lab we will deploy an application called `guestbook` that has already been built and uploaded to DockerHub under the name `ibmcom/guestbook:v1`.

1. Start by running `guestbook`:

   ```bash
   kubectl create deployment guestbook --image=ibmcom/guestbook:v1
   ```

   This action will take a bit of time. To check the status of the running application, you can use .

   ```bash
   kubectl get pods -w
   ```

   You should see output similar to the following:

   ```text
   NAME                          READY     STATUS              RESTARTS   AGE
   guestbook-59bd679fdc-bxdg7    0/1       ContainerCreating   0          1m
   ```

   Eventually, the status should show up as `Running`.

   ```text
   guestbook-59bd679fdc-bxdg7    1/1       Running             0          1m
   ```

   The end result of the run command is not just the pod containing our application containers, but a Deployment resource that manages the lifecycle of those pods.  
  
   Exit with `ctrl + c`  

2. Once the status reads `Running`, we need to expose that deployment as a service so we can access it through the IP of the worker nodes. The `guestbook` application listens on port 3000. Run:

   ```text
   kubectl expose deployment guestbook --type="NodePort" --port=3000
   # service "guestbook" exposed
   ```

3. To find the port used on that worker node, examine your new service:

   ```text
   kubectl get service guestbook
   ```

   ```
   NAME        TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
   guestbook   NodePort   10.10.10.253   <none>        3000:31208/TCP   1m
   ```

   We can see that our `<nodeport>` is `31208`. We can see in the output the port mapping from 3000 inside the pod exposed to the cluster on port 31208. This port in the 31000 range is automatically chosen, and could be different for you.  

4. `guestbook` is now running on your cluster, and exposed to the internet. We need to find out where it is accessible. The worker nodes running in the container service get external IP addresses. Run `$ ibmcloud cs workers <name-of-cluster>`, and note the public IP listed on the `<public-IP>` line. Also `kubect get nodes -o wide`command helps to get public ip of a node.

   ```text
   ibmcloud cs workers --cluster osscluster
   ```

   ```bash
   OK
   ID                                                       Public IP      Private IP      Flavor               State    Status   Zone    Version   
   kube-bq9k969l04l8ov8r6tv0-iksakbank22-default-000001ee   159.8.191.72   10.113.56.139   b3c.4x16.encrypted   normal   Ready    lon02   1.16.8_1527   
   ```

   We can see that our `<public-IP>` is `159.8.191.72.`

5. Now that you have both the address and the port, you can now access the application in the web browser at `<public-IP>:<nodeport>`. In the example case this is `159.8.191.72:31208`.

Congratulations, you've now deployed an application to Kubernetes!



