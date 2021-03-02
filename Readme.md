## OpenFaaS
![alt text](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/faas_side.png)
[OpenFaas](https://github.com/openfaas/faas) is an opensource serverless tool primarily used to deploy event-driven functions and microservices without repetitive, boiler-plate coding. OpenFaaS can be deployed to a variety of container orchestrators such as [Kubernetes](https://github.com/kubernetes/kubernetes), [OpenShift](https://github.com/openshift), [Docker Swarm](https://github.com/docker/classicswarm) or to a single host with [faasd](https://github.com/openfaas/faasd). Just package your code or an existing binary in a Docker image to deploy as functions or microservices and get a highly scalable endpoint with autoscaling and metrics. It is also possible to use other forms of event triggers such as Apache Kafka for OpenfaaS functions. 

Main Features:
* Ease of use through UI portal and one-click install.
* Write functions in any language for Linux or Windows and package in Docker/OCI image format.
* Portable - runs on existing hardware or public/private cloud with Kubernetes or containerd.
* CLI available with YAML format for templating and defining functions.
* Auto-scales as demand increases.

## OpenFaaS Stack
![alt text](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/of-layer-overview.png)

* [OpenFaaS Cloud](https://docs.openfaas.com/openfaas-cloud/intro/) builds upon OpenFaaS to deliver GitOps with GitHub.com or GitLab self-hosted.
* [NATS](https://github.com/nats-io) provides asynchronous execution and queuing.
* [Prometheus](https://prometheus.io/) provides metrics and enables auto-scaling through [AlertManager](https://prometheus.io/docs/alerting/latest/overview/).
* [OpenFaaS Gateway](https://github.com/openfaas/faas/blob/master/gateway/README.md) provides an external route into functions and microservices, and collect Cloud Native metrics through Prometheus which is further used to scale functions and microservices based on demand.
* A container registry holds each immutable artifact that can be deployed on OpenFaaS via the API.

## Architecture and Conceptual Workflow
![alt text](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/of-workflow.png)
* The Gateway can be accessed through its REST API, via the CLI or through the UI. All services or functions get a default route exposed, but custom domains can also be used for each endpoint.
* Prometheus collects metrics which are available via the Gateway's API and which are used for auto-scaling.
* By changing the URL for a function from /function/NAME to /async-function/NAME an invocation can be run in a queue using NATS Streaming. You can also pass an optional callback URL.

## Setting Up OpenFaaS in Kubernetes
   We need a kubernetes cluster to deploy OpenFaaS, either single node or multinode cluster. Here, we will use minikube; a single node kubernetes cluster.  
* [Installing OpenFaaS in Minikube](https://github.com/limbuu/serverless-with-openfaas/blob/main/images/MkReadme.md)

### Other alternative ways to deploy OpenFaaS
* [Installing OpenFaaS with arkade](https://docs.openfaas.com/deployment/kubernetes/#1-deploy-the-chart-with-arkade-fastest-option)
* [Installing OpenFaaS with helm](https://docs.openfaas.com/deployment/kubernetes/#2-deploy-the-chart-with-helm)

## Online Course
* [Introduction to Serverless on Kubernetes on edX](https://www.edx.org/course/introduction-to-serverless-on-kubernetes)

## Workshop
* [OpenFaaS Workshop](https://github.com/openfaas/workshop)

## Tutorials
* [Build Serverless Single App](https://www.openfaas.com/blog/serverless-single-page-app/)
* [Database with OpenFaaS and Mongo](https://blog.alexellis.io/serverless-databases-with-openfaas-and-mongo/)
* [Stateless Microservices with OpenFaaS](https://www.openfaas.com/blog/stateless-microservices/)
* [Trigger OpenFaaS functions with Apache Kafka](https://www.openfaas.com/blog/kafka-connector/)
* [Build OpenFaaS Cloud in AWS EKS](https://www.openfaas.com/blog/eks-openfaas-cloud-build-guide/)
* [SetUp OpenFaaS Cloud for Local Development](https://blog.alexellis.io/openfaas-cloud-for-development/)

## Reference Links
* [OpenFaaS Documentation](https://docs.openfaas.com/)
* [OpenFaaS Github Repository](https://github.com/openfaas/faas)
* [faas-netes Kubernetes Controller for OpenFaaS](https://github.com/openfaas/faas-netes/)









