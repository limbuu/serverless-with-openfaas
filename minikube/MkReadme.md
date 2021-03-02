# Setting Up OpenFaaS in Minikube

## Prerequisites 
## 1. Install Kubectl 
'kubectl' is CLI that allows you to run commands against Kubernetes clusters and can be used to deploy applications, inspect and manage cluster resources and view logs.
Install kubectl following [kubernetes official documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 

For MacOS:
* [kubectl for MacOS](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos)

For Windows:
* [kubectl for Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows)

For Linux/Ubuntu:
* [kubectl for Linux/Ubuntu](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux)

Test to ensure the kubectl version you installed is up-to-date:
```
$ kubectl version --client
```
   
## 2. SetUp a local Kuberenetes Cluster 

### Install Minikube
Minikube is single node local Kubernetes. To install minikube to set up local kubernetes cluster, you will need:
* 2 CPUs or more
* 2 GB of free memory
* 20 GB of free disk space
* Virtual Machine Manager:Docker, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMWare

Install minikube following official minikube documentation.

For MacOS:
* [Install Minikube for MacOS](https://minikube.sigs.k8s.io/docs/start/#macOS)

For Windows:
* [Install Minikube for Windows](https://minikube.sigs.k8s.io/docs/start/#Windows)

For Linux/Ubuntu:
* [Install Minikube for Linux](https://minikube.sigs.k8s.io/docs/start/#Linux)

Check minikube installation:
```
$ minikube help
$ minikube version
```

### Install Docker 
Install docker ce to use as virtualization driver following official docker documentation. 

For MacOS:
* [Docker CE for Mac](https://docs.docker.com/docker-for-mac/install/)

For Windows:
* [Docker CE for Windows](https://docs.docker.com/docker-for-windows/install/)

For Linux/Ubuntu:
* [Docker CE for Linux/Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

Test the docker installation in the terminal:
```
$ docker help
$ docker version
```

### Create an isolated profile to deploy OpenFaaS

Create a profile with vitualization driver as docker (can use virtualbox instead, follow virtualbox.org official documenatation for installation)
```
$ minikube start -p openfaas-profile
```

## 3. Deploy OpenFaas 
OpenFaaS can be installed in three different ways(arkade, helm and yamls), we here will use arkade, a package-manager/marketplace to download/install OpenFaaS.

### Install arkade

For MacOS / Linux:
```
$ curl -SLsf https://dl.get-arkade.dev/ | sudo sh
```

For Windows:
```
$ curl -SLsf https://dl.get-arkade.dev/ | sh
```

Check to ensure proper arkade version is installed:
```
$ arkade help
$ arkade version
```

### Install OpenFaas
For a local Kubernetes Cluster,

```
$ arkade install openfaas
```

Check the installation:
```
$ kubectl get pods -n openfaas
NAME                                 READY   STATUS    RESTARTS   AGE
alertmanager-79cd7f9479-65zh2        1/1     Running   0          15s
basic-auth-plugin-858495b9c6-x4ftk   1/1     Running   0          15s
gateway-7797d985f9-8xh2r             2/2     Running   1          15s
nats-cdc589ff7-c4bwx                 1/1     Running   0          15s
prometheus-fd89cf7cd-54b7d           1/1     Running   0          15s
queue-worker-749c7964f4-p4w2m        1/1     Running   2          15s

```
