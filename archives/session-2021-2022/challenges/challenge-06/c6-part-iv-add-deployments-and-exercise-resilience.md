# C6 - Part IV - Add deployments and exercise resilience

By now you should have understood the general principle of configuring, running and accessing applications in Kubernetes. However, the above application has no support for resilience. If a container (resp. Pod) dies, it stops working. Next, we add some resilience to the application.

### Add Deployments

In this task you will create Deployments that will spawn Replica Sets as health-management components.

Converting a Pod to be managed by a Deployment is quite simple.



* Have a look at an example of a Deployment described [here](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).
* Create Deployment versions of your application configurations (e.g. redis-deploy.yaml instead of redis-pod.yaml) and modify/extend them to contain the required Deployment parameters. Again, be careful with the YAML indentation!
* Make sure to have always 2 instances of the API and Frontend running.
* Use only 1 instance for the Redis-Server. Why?
* Delete all application Pods (using kubectl delete pod ...) and replace them with deployment versions.
* Verify that the application is still working and the Replica Sets are in place. (kubectl get all, kubectl get pods, kubectl describe ...)

{% file src="../../.gitbook/assets/redis-pod.yaml" %}

### Verify the functionality of the Replica Sets

In this subtask you will intentionally kill (delete) Pods and verify that the application keeps working and the Replica Set is doing its task.

Hint: You can monitor the status of a resource by adding the --watch option to the get command. To watch a single resource:

```
kubectl get <resource-name> --watch
```

To watch all resources of a certain type, for example all Pods:

```
kubectl get pods --watch
```

You may also use _kubectl get all_ repeatedly to see a list of all resources. You should also verify if the application stays available by continuously reloading your browser window.

* What happens when you delete the Redis Pod?
* How can you change the number of instances temporarily to 3? Hint: look for scaling in the deployment documentation
* What autoscaling features are available? Which metrics are used?
* How can you update a component? (see "Updating a Deployment" in the deployment documentation)What happens if you delete a Front-end or API Pod? How long does it take for the system to react?

### (optional) - Put autoscalling in place and load-test it

On the GKE cluster deploy autoscalling on the Frontend with a target CPU utilization of 30% and number of replicas between 1 and 4. Load-test using JMeter.

### Deliverables

* Document any difficulties you faced and how you overcame them.
* Join the setting files (.yaml) as attachments
* Compressed them in a .zip archive and sent it through the private teams channel.
