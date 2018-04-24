# Azure Container Hackfest Challenge Guide 

Below is a series of "challenges" or guided exercises to help attendees learn about Azure Containers and Kubernetes. These are not meant the be "hands-on labs" or step-by-step guides. The goal is to provide a series of exercises that have an expected outcome. Some steps and code will be provided. In the end, the hands-on experience should lead to a deeper level of learning. 

## Environment Setup

For our Hackfest events, attendees will be provided a VM student environment hosted in Azure. For those setting up their machines manually, the prerequisites are below. 

-> Note: This has only been tested on Mac OS.

* Azure Account with access to deploy at least 10 cores
* [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* [Azure Service Principal with at Least Contributor Rights](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2Fazure%2Fazure-resource-manager%2Ftoc.json&view=azure-cli-latest#create-a-service-principal-for-your-application)
* Visual Studio Code
* Install and configure Golang https://golang.org/doc/install
* Bash (Windows users will require "Windows Subsystem for Linux")
* Docker

    * [Docker for Mac](https://docs.docker.com/docker-for-mac/install)
    * [Docker for Windows](https://docs.docker.com/docker-for-windows/install)

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl)
* [helm](https://docs.helm.sh/using_helm)

## Challenges:
 
### Challenge #1: [Installing Azure Kubernetes Service / AKS](challenges/01-aksintro/README.md)
 
### Challenge #2: [Kubernetes Basics](challenges/02-k8sbasics/README.md)
 
### Challenge #3: [Deploy app end-to-end](challenges/03-appdeploy/README.md)

### Challenge #4: [Service Discovery and Networking](challenges/04-servicediscovery/README.md)
 
### Challenge #5: [Ingress Controllers](challenges/05-ingress/README.md)

### Challenge #6: [Secrets and ConfigMaps](challenges/06-configuration/README.md)
 
### Challenge #7: [Helm Deployment](challenges/07-helm/README.md)
 
### Challenge #8: [CI/CD](challenges/08-cicd/README.md)

### Challenge #9: [Persistent Storage](challenges/09-storage/README.md)
 
### Challenge #10: [Scaling](challenges/10-scaling/README.md)
 
### Challenge #11: [Monitoring & Logging](challenges/11-logging/README.md)
 
### Challenge #12: [Troubleshooting & Debugging](challenges/12-debugging/README.md)
 
### Challenge #13: [Kubernetes Advanced](challenges/13-advanced/README.md)
 