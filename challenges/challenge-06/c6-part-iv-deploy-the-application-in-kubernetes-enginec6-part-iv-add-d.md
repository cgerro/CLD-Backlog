# C6 - Part IV - Deploy the application in Kubernetes EngineC6 - Part IV - Add d

In this part you will deploy the application in the public cloud service Google Kubernetes Engine (GKE).

### Create Project

Log in to the [Google Cloud console](http://console.cloud.google.com/). Navigate to the [Resource Manager](https://console.cloud.google.com/cloud-resource-manager) and create a new project.

### Create a cluster

Go to the [Google Kubernetes Engine (GKE) console](https://console.cloud.google.com/kubernetes/). If necessary, enable the Kubernetes Engine API. Then create a cluster.

* Choose a GKE Standard cluster.
* Give it a name of the form gke-cluster-1
* Select a region close to you.
* Set the number of nodes to 4.
* Keep the other settings at their default values.

### Deploy the application on the cluster

Once the cluster is created, the GKE console will show a Connect button next to the cluster in the cluster list. Click on it. A dialog will appear with a command-line command. Copy/paste the command and execute it on your local machine. This will download the configuration info of the cluster to your local machine (this is known as a context). It also changes the current context of your _kubectl_ tool to the new cluster.

To see the available contexts, type

```
$ kubectl config get-contexts
```

You should see two contexts, one for the _Minikube_ cluster and one for the GKE cluster. The current context has a star \* in front of it. The _kubectl_ commands that you type from now on will go to the cluster of the current context.

With that you can use _kubectl_ to manage your GKE cluster just as you did in task 1. Repeat the application deployment steps of task 1 on your GKE cluster.

Should you want to switch contexts, use

```
$ kubectl config use-context <context>
```

### Deploy the ToDo-Frontend Service

On the Minikube cluster we did not have the possibility to expose a service on an external port, that is why we did not create a Service for the Frontend. Now, with the GKE cluster, we are able to do that.

Using the redis-svc.yaml file as an example, create the frontend-svc.yaml configuration file for the Frontend Service.

{% file src="../../.gitbook/assets/redis-svc.yaml" %}



Unlike the Redis and API Services the Frontend needs to be accessible from outside the Kubernetes cluster as a regular web server on port 80.

{% hint style="info" %}
We need to change a configuration parameter. Our cluster runs on the GKE cloud and we want to use a GKE load balancer to expose our service.&#x20;



Read the section ["Publishing Services - Service types" of the K8s documentation](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types)&#x20;
{% endhint %}

Deploy the Service using _kubectl_.

This will trigger the creation of a load balancer on GKE. This might take some minutes. You can monitor the creation of the load balancer using _kubectl describe_.

### Verify the ToDo application

Now you can verify if the ToDo application is working correctly.

{% hint style="info" %}
Find out the public URL of the Frontend Service load balancer using kubectl describe. Access the public URL of the Service with a browser. You should be able to access the complete application and create a new ToDo.
{% endhint %}

### Deliverables

Document any difficulties you faced and how you overcame them. Copy the object descriptions into the lab report (if they are unchanged from the previous task just say so).

Take a screenshot of the cluster details from the GKE console. Copy the output of the kubectl describe command to describe your load balancer once completely initialized.
