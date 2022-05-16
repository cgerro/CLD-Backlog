# C6 - Part II - In-house Kubernetes (IICT Kubernetes cluster)

### Prerequisites

* Get the credentials on your private teams channel

### Log on IICT Plateform

* Log yourself on [https://kubernetes.iict.ch](https://kubernetes.iict.ch) with the credentials given in the teams channel. /!\ Select "Use a local user" /!\\
* Once you are logged go on "jupyter-enseignement" and then on the right corner click on the button "Kubeconfig File". Copy the content and paste it on your machine in the file `~/.kube/config`.

![kubernetes iict](<../../.gitbook/assets/image (3).png>)

* To test that you can speak with the cluster try the command listing all nodes available. You should have something like that:

```shell
[Request]
kubectl get nodes

[Response]
NAME           STATUS   ROLES                      AGE    VERSION
10.193.72.41   Ready    controlplane,etcd,worker   438d   v1.19.3
10.193.72.42   Ready    controlplane,worker        438d   v1.19.3
10.193.72.43   Ready    controlplane,worker        438d   v1.19.3
10.193.72.44   Ready    worker                     438d   v1.19.3
```

### Deploy the app on the IICT Kubernetes cluster

#### Deploy the Redis Service and Pod

1. Run the following commands to deploy redis

{% file src="../../.gitbook/assets/redis-pod.yaml" %}

{% file src="../../.gitbook/assets/redis-svc.yaml" %}



```shell
kubectl create -f redis-svc.yaml -n cldgrpXX
kubectl create -f redis-pod.yaml -n cldgrpXX
```

1. Verify that it's up and running with the commande `kubectl get all -n cldgrpXX`. To get more details on an object you can run the commmand `kubectl describe po/redis -n cldgrpXX` for the pod and `kubectl describe svc/redis-svc -n cldgrpXX` for the service.

#### Deploy the ToDo-API Service and Pod

Using the `redis-svc.yaml` file as example and information from `api-pod.yaml`, create the `api-svc.yaml` configuration file for the API Service. The Service has to expose port 8081 and connect to the port of the API Pod.

{% file src="../../.gitbook/assets/api-pod.yaml" %}





Be careful with the indentation of the YAML files. If your code editor has a YAML mode, enable it.

Deploy and verify the API-Service and Pod (similar to the Redis ones) and verify that they are up and running on the correct ports.

#### Deploy the Frontend Pod and Service

Using the `api-pod.yaml` file as an example, create the `frontend-pod.yaml` configuration file that starts the UI Docker container in a Pod.

Docker image for frontend container on Docker Hub is `icclabcna/ccp2-k8s-todo-frontend`

Note that the container runs on port: 8080

It also needs to be initialized with the following environment variables (check how api-pod.yaml defines environment variables):

`API_ENDPOINT_URL`: URL where the API can be accessed e.g., http://localhost:9000

* What value must be set for this URL?

{% hint style="info" %}
_Remember that anything you define as a Service will be assigned a DOMAIN that is visible via DNS everywhere in the cluster and a PORT._
{% endhint %}

Using the `redis-svc.yaml` file as an example, create the `frontend-svc.yaml` configuration file for the Frontend Service.

{% file src="../../.gitbook/assets/redis-svc.yaml" %}

Unlike the Redis and API Services the Frontend needs to be accessible from outside the Kubernetes cluster as a regular web server on port 80.

We need to change a configuration parameter.

* Read the section "Publishing Services - Service types" of the K8s documentation https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services---service-types

Deploy and verify the Frontend-Service and Pod and verify that they are up and running on the correct ports. The `frontend-svc` should have received an `EXTERNAL-IP` from the subnet 10.193.72 .0/24

#### Verify the ToDo application

Now you should be able to reach your app with the _EXTERNAL-IP_ of the frontend service.

### Deliverables <a href="#deliverables" id="deliverables"></a>

* Document any difficulties you faced and how you overcame them.
* Join the setting files (.yaml) as attachments
* Compressed them in a .zip archive and sent it through the private teams channel.

