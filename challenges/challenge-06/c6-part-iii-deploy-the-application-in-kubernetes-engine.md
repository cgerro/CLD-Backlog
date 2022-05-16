# C6 - Part III - Deploy the application in Kubernetes Engine

In this part you will deploy the application in the public cloud service Google Kubernetes Engine (GKE).

### Create Project

Log in to the [Google Cloud console](http://console.cloud.google.com/). Navigate to the [Resource Manager](https://console.cloud.google.com/cloud-resource-manager) and create a new project.

### Create a cluster

Go to the [Google Kubernetes Engine (GKE) console](https://console.cloud.google.com/kubernetes/). If necessary, enable the Kubernetes Engine API. Then create a cluster.

* Choose a GKE Standard cluster.
* Give it a name of the form gke-cluster-1
* Select a region close to you.
* Set the number of nodes to 4.

{% hint style="info" %}
Proceed like this:

* Initialize your cluster with 3 nodes (1 node in each zone)
* Once the cluster is set, update the number to 4 nodes.
{% endhint %}

* Keep the other settings at their default values.

### Deploy the application on the cluster

Once the cluster is created, the GKE console will show a **Connect** button next to the cluster in the cluster list. Click on it. A dialog will appear with a command-line command. Copy/paste the command and execute it on your local machine. This will download the configuration info of the cluster to your local machine (this is known as a context). It also changes the current context of your _kubectl_ tool to the new cluster.

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

### Deploy the ToDo-Application

Use the command `kubectl` to deploy the todo app on the GKE.

The _frontend-svc.yaml_ will trigger the creation of a load balancer on GKE. This might take some minutes. You can monitor the creation of the load balancer using

> kubectl describe

### Verify the ToDo application

Now you can verify if the ToDo application is working correctly.

{% hint style="info" %}
* Find out the public URL of the Frontend Service load balancer using kubectl describe.&#x20;
* Access the public URL of the Service with a browser. You should be able to access the complete application and create a new ToDo.
{% endhint %}

### Deliverables

Document any difficulties you faced and how you overcame them. Copy the object descriptions into the lab report (if they are unchanged from the previous task just say so).

Take a screenshot of the cluster details from the GKE console. Copy the output of the _kubectl describe_ command to describe your load balancer once completely initialized.
