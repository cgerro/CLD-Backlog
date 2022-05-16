# C6 - Part II - In-house Kubernetes (IICT)

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
