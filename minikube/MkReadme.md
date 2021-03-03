# Setting Up OpenFaaS in Minikube

## Prerequisites 
## 1. Install Kubectl 
`kubectl` is CLI that allows you to run commands against Kubernetes clusters and can be used to deploy applications, inspect and manage cluster resources and view logs.
Install kubectl following kubernetes official documentation.`

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

## 2. OpenFaaS CLI

Install the OpenFaaS CLI using the official bash script. We will use the `faas-cli` to scaffold new functions, build, deploy and invoke functions. You can find out commands available for the cli with `faas-cli --help`.

With MacOS or Linux run the following in a Terminal:

```sh
$ curl -sLSf https://cli.openfaas.com | sudo sh
```

For Windows, run this in *Git Bash*:

```sh
$ curl -sLSf https://cli.openfaas.com | sh
```

Test the `faas-cli`. Open a Terminal or Git Bash window and type in:

```sh
$ faas-cli help
$ faas-cli version
```  
## 3. SetUp a local Kuberenetes Cluster 

### Install Minikube
`Minikube` is single node local Kubernetes. To install minikube to set up local kubernetes cluster, you will need:
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
Install `docker ce` to use as virtualization driver following official docker documentation. 

For MacOS:
* [Docker CE for Mac](https://docs.docker.com/docker-for-mac/install/)

For Windows:
* [Docker CE for Windows](https://docs.docker.com/docker-for-windows/install/)

For Linux/Ubuntu:
* [Docker CE for Linux/Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

Test the docker installation in the terminal:
```sh
$ docker help
$ docker version
```

### Create an isolated profile to deploy OpenFaaS

Create a profile with vitualization driver as docker (can use virtualbox instead, follow virtualbox.org official documenatation for installation)
```
$ minikube start -p openfaas-profile
```

## 4. Deploy OpenFaas 
OpenFaaS can be installed in three different ways(arkade, helm and yamls), we here will use `arkade`, a package-manager/marketplace to download/install OpenFaaS.

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
```sh
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
### Log into your OpenFaaS gateway
Check the gateway is ready or not,

```
$ kubectl rollout status -n openfaas deploy/gateway
```
1. If you're using your laptop, a VM, or any other kind of Kubernetes distribution run the following instead:
```
$ kubectl port-forward svc/gateway -n openfaas 8080:8080
```

2. But, If you're using a managed cloud Kubernetes service then get the LoadBalancer's IP address or DNS entry from the EXTERNAL-IP field from the command below.
```
$ kubectl get svc -o wide gateway-external -n openfaas
```

Now for local setup login, the URL will be the IP/DNS at port 8080 at 127.0.0.1

Use username as admin and to generate password do:
```
$ PASSWORD=$(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode; echo)
$ echo $PASSWORD
```
![alt text](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/login_page.png)


`Alternatively, do following to automate this process to use openFaaS through terminal`
* Log in:
```
export OPENFAAS_URL="" # Populate as above

# This command retrieves your password
PASSWORD=$(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode; echo)

# This command logs in and saves a file to ~/.openfaas/config.yml
echo -n $PASSWORD | faas-cli login --username admin --password-stdin

```

* Check that faas-cli list works:
```
$ faas-cli list
```

## 5. Test Function
Deploy some sample functions and then use them to test things out:
```
$ faas-cli deploy -f https://raw.githubusercontent.com/openfaas/faas/master/stack.yml
```
![alt text](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/test_function.png)

Beside UI, you can invoke functions through terminal using `faas-cli`.

First, show the invocation count and replicas of deployed functions:
```
$ faas-cli list
```
Second, pick on function that appear as output of `faas-cli list` and invoke it.
```
$ faas-cli invoke markdown
```

## 6. Monitroing Using Grafana

First, run grafana in `openfaas` kubernetes namespace:
```
$ kubectl -n openfaas run --image=stefanprodan/faas-grafana:4.6 --port=3000 grafana
```
Second, expose grafana with a nodeport:
```
$ kubectl -n openfaas expose pod grafana --type=NodePort --name=grafana
```
Run port-forward command to access grafana at `http://127.0.0.1:8000`
```
$ kubectl port-forward pod/grafana 8000:3000 -n openfaas
```
Now, login with username `admin` password `admin` and navigate to the pre-made OpenFaaS dashboard at:
`http://127.0.0.1:8000/dashboard/db/openfaas`

![alt text](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/grafana.jpeg)
