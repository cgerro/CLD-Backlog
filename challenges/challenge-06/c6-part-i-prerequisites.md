# C6 - Part I - Prerequisites

[Kubernetes Cheatsheet](https://kubernetes.io/fr/docs/reference/kubectl/cheatsheet/)

### Prerequisites

#### Setup Kubernetes

* [Source](https://kubernetes.io/fr/docs/tasks/tools/install-kubectl/)
* Get source

```
[Request]
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.24.0/bin/windows/amd64/kubectl.exe

[Response]
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 44.1M  100 44.1M    0     0   774k      0  0:00:58  0:00:58 --:--:--  978k
```

#### Verify successful installation by running `k version`. You should see something similar to

```shell
Client Version: version.Info{Major:"1", Minor:"23", ...
```
