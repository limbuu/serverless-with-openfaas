# Setting Up OpenFaaS in Minikube

## Prerequisites 
### 1. Docker 
Install docker ce following official docker documentation. 

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
   
### 2. OpenFaas CLI
Install the OpenFaaS CLI using the official bash script. We will use faas-cli to scaffold new functions, build, deploy and invoke functions. 
For MacOS or Linux run following in terminal:
```
$ curl -sLSf https://cli.openfaas.com | sudo sh
``` 

For Windows, run this in Git Bash:
```
$ curl -sLSf https://cli.openfaas.com | sh
```
Test the installation in the terminal:
```
$ faas-cli help
$ faas-cli version
```