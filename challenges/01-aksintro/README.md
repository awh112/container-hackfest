# Intro to Azure Kubernetes Service

## Expected outcome

In this lab, you will prepare your local environment for working with AKS.

## How to

### Register Azure CLI to work with AKS

Make sure AKS is enabled: `az provider register -n Microsoft.ContainerService`

### Create SSH Keys:

```
ssh-keygen -t rsa -b 4096 -C "username@email.com" -f .ssh/id_rsa
```

## Create the cluster

_Note:_ Must be one of these regions as of 1.18.2018 'eastus,westeurope,centralus,canadacentral,canadaeast'

```
az group create -n Kubernetes-Hackfest -l centralus

az aks create -g Kubernetes-Hackfest -n hackfestK8sCluster --node-count 1 --ssh-key-value .ssh/id_rsa.pub
```

### Verify provisioning is complete
```
$ az aks list -o table
Name                Location    ResourceGroup        KubernetesVersion    ProvisioningState    Fqdn
------------------  ----------  -------------------  -------------------  -------------------  -------------------------------------------------------------------
hackfestK8sCluster  centralus   Kubernetes-Hackfest  1.7.7                Succeeded            hackfestk8-kubernetes-hackf-****.hcp.centralus.azmk8s.io
```

### Connect to cluster
List your AKS clusters: `az aks list -o table`

Authenticate and browse:
```
az aks get-credentials -g Kubernetes-Hackfest -n hackfestK8sCluster
kubectl get nodes
az aks browse -g Kubernetes-Hackfest -n hackfestK8sCluster
```

If kubectl is not installed, it can be installed via the Azure CLI with `az acs kubernetes install-cli` (`az aks` does not work)

## Advanced areas to explore

1. If you have time, explore deploying a Kubernetes Cluster using the [ACS-Engine Open Source](https://github.com/Azure/acs-engine) project. 
2. Deploy a cluster using acs-engine with advanced options such as custom Network Range, Azure CNI or Calico Network Policy. 
